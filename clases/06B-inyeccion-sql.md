# Clase 6B · Inyección SQL en tu e-commerce

> Clase de profundización sobre el proyecto de UTULab. Tu tienda tiene un login con roles, un catálogo con búsqueda y una página de detalle de producto. Cada una de esas tres es una puerta, y esta clase muestra cómo se abre y cómo se cierra

**Contenidos del programa:** 1.3 Seguridad de redes, sistemas, aplicaciones y datos
**Competencia:** CET1 · CET2
**Tiempo de lectura:** unos 30 minutos
**Requiere:** haber leído la [clase 6](06-vulnerabilidades-y-superficie-de-ataque.md)

---

📥 **Presentación de la clase:** [Clase 6B · Inyección SQL](../presentaciones/Clase6B-Inyeccion-SQL.pptx) — `.pptx`, se descarga con el botón **Download** al abrir el enlace.

---

> [!TIP]
> **Por qué esta clase es sobre lo tuyo.** El proyecto de egreso que están armando en UTULab es un **e-commerce en PHP y MySQL**: un módulo de usuarios con login y roles (administrador, empleado, cliente), un catálogo de productos con búsqueda, y una página de detalle de cada producto. Los tres módulos le piden datos a la base con consultas SQL. Si esas consultas están mal escritas, cualquiera entra como administrador o se lleva la tabla de clientes. Esta clase usa esos mismos tres módulos como ejemplo, porque son los que ustedes están escribiendo ahora.

## Contenido

1. [Cómo le habla tu PHP a la base](#1-cómo-le-habla-tu-php-a-la-base)
2. [El error: concatenar el dato del usuario](#2-el-error-concatenar-el-dato-del-usuario)
3. [Ataque 1 · Entrar como administrador sin contraseña](#3-ataque-1--entrar-como-administrador-sin-contraseña)
4. [Ataque 2 · Romper la búsqueda del catálogo](#4-ataque-2--romper-la-búsqueda-del-catálogo)
5. [Ataque 3 · Robar la tabla de clientes con UNION](#5-ataque-3--robar-la-tabla-de-clientes-con-union)
6. [Qué está en juego en un e-commerce](#6-qué-está-en-juego-en-un-e-commerce)
7. [La solución: consultas parametrizadas](#7-la-solución-consultas-parametrizadas)
8. [Defensa en profundidad](#8-defensa-en-profundidad)
9. [Lo que NO alcanza](#9-lo-que-no-alcanza)
10. [Checklist para tu proyecto](#10-checklist-para-tu-proyecto)
11. [Errores frecuentes](#11-errores-frecuentes)
12. [Cierre](#12-cierre)

---

## 1. Cómo le habla tu PHP a la base

Cada vez que tu tienda muestra productos o valida un login, tu PHP le manda una **consulta** a MySQL: una orden escrita en lenguaje SQL. Por ejemplo, para el login:

```sql
SELECT * FROM usuarios WHERE correo = 'ana@mail.com' AND clave = '...'
```

El problema aparece cuando **parte de esa orden la escribe el usuario**. El correo lo escribió quien está en el formulario. Si tu PHP arma la consulta pegando ese correo directo en el texto, el usuario no está llenando un campo: está **escribiendo parte de la orden SQL**.

> [!NOTE]
> Es exactamente la superficie de ataque de la clase 6: cada campo del formulario, cada `?id=` en la URL, cada término de búsqueda es un punto por el que entra un dato que vos no controlás. La inyección SQL es lo que pasa cuando ese dato termina siendo tratado como orden.

---

## 2. El error: concatenar el dato del usuario

Así es como **no** hay que escribir el login (y como suele estar en una primera versión):

```php
// login.php — VULNERABLE
$correo = $_POST['correo'];
$clave  = $_POST['clave'];

$sql = "SELECT * FROM usuarios
        WHERE correo = '$correo' AND clave = '" . md5($clave) . "'";

$res = $conn->query($sql);
```

La variable `$correo` se **pega** al texto del SQL con las comillas. Mientras el usuario escriba un correo normal, funciona. El problema es qué pasa cuando escribe algo que **no** es un correo.

> [!IMPORTANT]
> La regla de oro, que vamos a repetir toda la clase: **el dato del usuario nunca se concatena al SQL**. En cuanto ves un `"... '$variable' ..."` armando una consulta, ahí hay una inyección esperando. No importa el lenguaje: el error es mezclar la orden con el dato.

---

## 3. Ataque 1 · Entrar como administrador sin contraseña

En el campo **correo** del login, el atacante escribe esto (y en la contraseña, cualquier cosa):

```
' OR rol='administrador' -- 
```

Con eso, la consulta que tu PHP arma y le manda a la base queda así:

```sql
SELECT * FROM usuarios
WHERE correo = '' OR rol='administrador' -- ' AND clave = '...'
```

Leela con cuidado, porque es todo el ataque:

- La comilla `'` **cierra** el correo antes de tiempo.
- `OR rol='administrador'` le agrega una condición: «...o cualquier usuario cuyo rol sea administrador».
- `-- ` (dos guiones y un espacio) **comenta** todo lo que sigue: la parte `AND clave = '...'` desaparece, así que el control de la contraseña ya no existe.

Resultado: la base devuelve al administrador, tu PHP lo da por logueado, y el atacante entra al panel de administración **sin saber ninguna contraseña**. Con la variante `' OR '1'='1' -- ` entra directamente como el primer usuario de la tabla.

> [!CAUTION]
> Esto no es teórico ni difícil. Es el ataque más viejo y más común contra un login mal hecho, y funciona en la primera versión de casi todos los proyectos. Por eso es lo primero que hay que cerrar.

---

## 4. Ataque 2 · Romper la búsqueda del catálogo

El buscador del catálogo suele armar una consulta parecida:

```php
// buscar.php — VULNERABLE
$q = $_GET['q'];
$sql = "SELECT nombre, precio FROM productos WHERE nombre LIKE '%$q%'";
```

De nuevo, el término de búsqueda `$q` se pega al texto. Si el atacante busca:

```
' OR '1'='1
```

la condición se vuelve siempre verdadera y el catálogo devuelve **todos** los productos, incluidos los que estuvieran despublicados o en borrador. Es una entrada menos grave que el login, pero es la puerta que lleva al ataque que sigue.

---

## 5. Ataque 3 · Robar la tabla de clientes con UNION

La página de detalle de un producto recibe el id por la URL:

```php
// producto.php — VULNERABLE
$id = $_GET['id'];
$sql = "SELECT nombre, precio, descripcion FROM productos WHERE id = $id";
```

Acá el id **ni siquiera lleva comillas** (es un número), así que es todavía más fácil de inyectar. El atacante trabaja en dos pasos.

**Paso 1 — contar las columnas.** Necesita saber cuántas columnas muestra la consulta. Va probando con `ORDER BY`:

```
producto.php?id=1 ORDER BY 3     → funciona
producto.php?id=1 ORDER BY 4     → error: "Unknown column '4'"
```

El error en el 4 le dice que hay **3 columnas** (nombre, precio, descripción).

**Paso 2 — pegar sus propios datos con UNION.** `UNION SELECT` une, debajo de los resultados del producto, los resultados de **otra** consulta que elige el atacante. Pide un id que no existe (para que arriba no haya nada) y abajo pone lo que quiere robar:

```
producto.php?id=0 UNION SELECT correo, id, clave FROM usuarios
```

La consulta que se ejecuta es:

```sql
SELECT nombre, precio, descripcion FROM productos WHERE id = 0
UNION
SELECT correo, id, clave FROM usuarios
```

Y la página de detalle del producto, en los lugares donde iban el nombre, el precio y la descripción, ahora muestra **el correo, el id y la contraseña de cada usuario de tu tienda**:

```
admin@tienda.uy   1   0192023a7bbd73250516f069df18b500
ana@mail.com      2   17b92615448714587fd56973009d91ad
```

> [!WARNING]
> Fijate lo que pasó: una página que solo mostraba productos terminó **volcando la tabla de usuarios completa**. Si las contraseñas están en MD5 (como en el ejemplo), esos hashes se rompen en segundos con una tabla, y el atacante tiene las contraseñas reales. Con UNION se puede sacar cualquier tabla: clientes, pedidos, direcciones, medios de pago. La inyección en una sola página deja expuesta **toda la base**.

---

## 6. Qué está en juego en un e-commerce

En el portal del liceo de la clase 6 lo que se filtraba eran boletines. En tu e-commerce lo que se filtra son **datos de personas que confiaron en la tienda**: nombres, correos, direcciones, historial de compras, y a veces datos de pago.

> [!IMPORTANT]
> En Uruguay, esos datos están protegidos por la **Ley N.º 18.331 de Protección de Datos Personales**, que controla la **URCDP** (Unidad Reguladora y de Control de Datos Personales). Quien maneja datos de clientes tiene la **obligación legal** de protegerlos con medidas de seguridad adecuadas. Una inyección SQL que expone la base de clientes no es solo un error técnico: es un incumplimiento que puede traer sanciones. La seguridad del software es, para un e-commerce, un requisito legal, no un lujo.

Y del lado del atacante: acceder a esa base sin autorización es delito (Ley N.º 20.327, art. 297 BIS), como vimos. Todo lo de esta clase se prueba **solo sobre el proyecto propio**, en la máquina propia, nunca contra un sitio ajeno.

---

## 7. La solución: consultas parametrizadas

El patrón es siempre el mismo: **separar la orden del dato**. La orden SQL se escribe con un hueco (un `?`) donde va el dato, y el dato se manda **aparte**. El motor de la base nunca lo mezcla con la orden: lo trata siempre como un valor, aunque contenga comillas, `OR`, `UNION` o lo que sea.

En PHP con **mysqli** (lo que usan en el proyecto), el login seguro se escribe así:

```php
// login.php — SEGURO
$correo = $_POST['correo'];
$clave  = $_POST['clave'];

// 1. la orden, con un ? donde va el dato
$stmt = $conn->prepare("SELECT * FROM usuarios WHERE correo = ?");

// 2. el dato se manda aparte ("s" = string)
$stmt->bind_param("s", $correo);
$stmt->execute();

$res = $stmt->get_result();
$usuario = $res->fetch_assoc();

// 3. la contraseña se verifica en PHP, con hashing (nunca en el SQL)
if ($usuario && password_verify($clave, $usuario['clave'])) {
    // login correcto
}
```

Ahora, si alguien escribe `' OR rol='administrador' -- ` en el correo, la base **busca un usuario cuyo correo sea, literalmente, ese texto raro**. No existe, no entra nadie. El ataque del punto 3 deja de funcionar.

La página de detalle del producto, con el id como parámetro:

```php
// producto.php — SEGURO
$id = $_GET['id'];
$stmt = $conn->prepare("SELECT nombre, precio, descripcion FROM productos WHERE id = ?");
$stmt->bind_param("i", $id);   // "i" = entero: además obliga a que el id sea un número
$stmt->execute();
```

El `"i"` fuerza a que el id sea un entero, así que `0 UNION SELECT ...` ni siquiera llega a la base como texto. El UNION del punto 5 muere ahí.

> [!TIP]
> Si en el proyecto usan **PDO** en vez de mysqli, el patrón es el mismo:
> ```php
> $stmt = $pdo->prepare("SELECT * FROM usuarios WHERE correo = ?");
> $stmt->execute([$correo]);
> ```
> Lo que no cambia nunca: **el dato va por el `?`, no pegado al texto de la consulta.**

---

## 8. Defensa en profundidad

Las consultas parametrizadas cierran la inyección. Pero un sistema bien hecho pone varias capas, para que un descuido no lo exponga todo. Estas son baratas y van todas en el proyecto:

| Medida | Qué evita |
|---|---|
| **Usuario de BD con mínimo privilegio** | Que tu app se conecte a MySQL como `root`. Si se conecta con un usuario que solo puede leer y escribir en las tablas que necesita —no borrar, no cambiar la estructura—, una inyección hace mucho menos daño. |
| **No mostrar los errores de SQL al cliente** | Que un mensaje de error le regale al atacante el nombre de las tablas y columnas. En producción, los errores van a un log, no a la pantalla. |
| **Validar el tipo de cada dato** | Que un `id` que debería ser un número llegue como texto con un `UNION` adentro. Si esperás un entero, convertilo a entero. |
| **Guardar las contraseñas con `password_hash`** | Que, si igual se filtra la tabla, las contraseñas se puedan recuperar. Con bcrypt no sirven las tablas de MD5. |

> [!NOTE]
> El mínimo privilegio y guardar bien las contraseñas ya los vieron en las clases 3 y 6. Acá se aplican al proyecto: son la diferencia entre «se filtró una consulta» y «se filtró todo».

---

## 9. Lo que NO alcanza

Hay tres «soluciones» que aparecen solas y que **no** cierran la inyección. Conviene descartarlas de entrada:

**1. Escapar las comillas a mano.** Reemplazar `'` por `\'` con `str_replace` o `addslashes` parece que arregla, pero se escapa por mil lados (comillas de otro tipo, codificaciones, campos numéricos sin comillas como el del punto 5). Es una pelea que se pierde. El motor ya sabe escapar bien: por eso se usan parámetros.

**2. Validar solo del lado del cliente.** La validación en JavaScript del formulario es para la comodidad del usuario. El atacante no usa tu formulario: manda el pedido directo. **Toda** validación que importa para la seguridad va del lado del servidor (PHP).

**3. Esconder los nombres de las tablas.** Que la tabla se llame `u_9x` en vez de `usuarios` no frena a nadie: con UNION y unos intentos se descubre igual. La seguridad no puede depender de que el diseño sea secreto (lo vimos como *diseño abierto* en la clase 3).

---

## 10. Checklist para tu proyecto

Antes de la entrega, revisá cada lugar donde tu PHP arma una consulta. Para cada uno:

- [ ] ¿El dato del usuario se pega al texto del SQL con comillas o `$variables`? → **reescribir con `prepare` + `?`**.
- [ ] El **login**: ¿usa consulta parametrizada y `password_verify`?
- [ ] La **búsqueda del catálogo**: ¿el término va por parámetro?
- [ ] El **detalle de producto** (`?id=`): ¿el id se fuerza a entero y va por parámetro?
- [ ] El **ABM de productos** (altas, bajas, modificaciones): ¿todas las consultas son parametrizadas?
- [ ] ¿La app se conecta a MySQL con un usuario que **no** es root?
- [ ] ¿Las contraseñas se guardan con `password_hash`, no con MD5?
- [ ] ¿Los errores de SQL van a un log y no a la pantalla del cliente?

> [!TIP]
> Un truco para encontrarlas rápido: buscá en tu código el signo `$` dentro de un texto de consulta (`"... $ ..."`) y el uso de `query(`. Casi cada resultado es un lugar para pasar a `prepare`.

---

## 11. Errores frecuentes

> [!WARNING]
> Los cuatro que más aparecen al corregir esto en los proyectos.

**1. Parametrizar el login pero no el resto.** El login es el más famoso, así que se arregla primero y a veces único. Pero el catálogo, el detalle y el ABM tienen el mismo problema. Se revisan **todas** las consultas.

**2. Poner el `?` pero seguir concatenando al lado.** `WHERE id = ? AND categoria = '$cat'` sigue siendo vulnerable por `$cat`. Si hay un `?`, que **todos** los datos vayan por `?`.

**3. Confiar en que «es solo un número».** El id del punto 5 no tenía comillas y fue el más fácil de atacar. Un número que viene de afuera también se valida y se parametriza.

**4. Creer que con el login por parámetro ya está todo seguro.** La inyección SQL es una de varias fallas. La clase 6 tiene las otras cuatro (XSS, IDOR, salto de directorio, hash débil), y todas pueden estar en el mismo e-commerce.

---

## 12. Cierre

La inyección SQL se resume en una frase: **pasa cuando el dato del usuario se mezcla con la orden SQL, y se cierra separándolos con parámetros.** En tu proyecto, eso significa revisar el login, la búsqueda, el detalle y el ABM, y reescribir cada consulta con `prepare` y `?`.

📖 **Consulta:** [Glosario](../recursos/glosario.md) · [Clase 6 · Vulnerabilidades](06-vulnerabilidades-y-superficie-de-ataque.md) · [Normativa uruguaya](../recursos/normativa-uruguay.md)

> **Para pensar antes de tu entrega.** Abrí el archivo del login de tu proyecto ahora mismo. ¿La consulta arma el correo con `'$correo'` o con `?`? Esa sola línea decide si cualquiera entra como administrador.

---

### Fuentes

- OWASP. *SQL Injection* y *SQL Injection Prevention Cheat Sheet*. <https://owasp.org/www-community/attacks/SQL_Injection>
- MITRE. *CWE-89: Improper Neutralization of Special Elements used in an SQL Command*. <https://cwe.mitre.org/data/definitions/89.html>
- PHP. *Manual: mysqli::prepare, mysqli_stmt::bind_param, PDO::prepare, password_hash*. <https://www.php.net/manual/es/>
- Uruguay. *Ley N.º 18.331 de Protección de Datos Personales* — URCDP. <https://www.gub.uy/unidad-reguladora-control-datos-personales/>
- Uruguay. *Ley N.º 20.327* — delitos informáticos (acceso ilícito, art. 297 BIS).

---

[⬅ Volver al índice del curso](../README.md)
