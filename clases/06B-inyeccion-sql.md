# Clase 6B · Inyección SQL

> Clase de profundización sobre una sola vulnerabilidad: la inyección SQL. Se estudia sobre el mismo comercio electrónico que se desarrolla en el proyecto de UTULab, con dos ataques sencillos y una única defensa correcta.

**Contenidos del programa:** 1.3 Seguridad de redes, sistemas, aplicaciones y datos
**Competencia:** CET1 · CET2
**Tiempo de lectura:** unos 20 minutos
**Requiere:** haber leído la [clase 6](06-vulnerabilidades-y-superficie-de-ataque.md)

---

📥 **Presentación de la clase:** [Clase 6B · Inyección SQL](../presentaciones/Clase6B-Inyeccion-SQL.pptx) — `.pptx`, se descarga con el botón **Download** al abrir el enlace.

💻 **Código de la clase:** los ejemplos vulnerables y corregidos se trabajan en la sala. No se publican en este repositorio: se trata de un repositorio público y no corresponde alojar código vulnerable.

---

## Contenido

1. [Cómo la aplicación consulta la base de datos](#1-cómo-la-aplicación-consulta-la-base-de-datos)
2. [El error: unir el dato a la consulta](#2-el-error-unir-el-dato-a-la-consulta)
3. [Qué es la inyección SQL](#3-qué-es-la-inyección-sql)
4. [Ataque 1 · Ingresar sin conocer la contraseña](#4-ataque-1--ingresar-sin-conocer-la-contraseña)
5. [Ataque 2 · Mostrar todo el catálogo](#5-ataque-2--mostrar-todo-el-catálogo)
6. [Qué está en juego en un comercio electrónico](#6-qué-está-en-juego-en-un-comercio-electrónico)
7. [La defensa: consultas parametrizadas](#7-la-defensa-consultas-parametrizadas)
8. [Capas adicionales de defensa](#8-capas-adicionales-de-defensa)
9. [Lo que no alcanza](#9-lo-que-no-alcanza)
10. [Verificación del proyecto](#10-verificación-del-proyecto)
11. [Cierre](#11-cierre)

---

## 1. Cómo la aplicación consulta la base de datos

Cada vez que la aplicación valida un ingreso o muestra productos, envía a la base de datos una **consulta** escrita en lenguaje SQL. La consulta es una orden: la base la ejecuta y devuelve un resultado. Por ejemplo, para el ingreso:

```sql
SELECT * FROM usuarios WHERE correo = 'ana@mail.com' AND clave = '...'
```

El valor `'ana@mail.com'` proviene del formulario: lo escribe la persona que usa el sistema. Ese dato de entrada no está bajo el control del programador.

> [!NOTE]
> Es la superficie de ataque de la clase 6. Cada campo de un formulario, cada término del buscador y cada parámetro de la dirección (por ejemplo, `?id=3`) es un punto por el que entra un dato que el sistema no controla. La regla general se mantiene: ningún dato que llega de afuera debe considerarse confiable.

---

## 2. El error: unir el dato a la consulta

El problema aparece cuando la consulta se arma **pegando directamente el dato del usuario** dentro del texto SQL. Así es como **no** debe escribirse el ingreso:

```php
// login.php — VULNERABLE
$correo = $_POST['correo'];

$sql = "SELECT * FROM usuarios WHERE correo = '$correo'";
```

Mientras el usuario escribe un correo normal, la consulta funciona. El problema aparece cuando escribe algo que **no** es un correo: en ese caso, el dato deja de ser un valor y pasa a formar parte de la orden.

> [!IMPORTANT]
> Esta es la regla central de la clase, y se repite en cada sección: **el dato del usuario nunca debe unirse al texto de la consulta.** Donde aparece un `"... '$variable' ..."` armando una consulta, hay una inyección posible.

---

## 3. Qué es la inyección SQL

> **Definición.** La **inyección SQL** (identificada como **CWE-89**) ocurre cuando un dato proporcionado por el usuario se interpreta como parte de la orden SQL, y no como un simple valor.

**CWE** (*Common Weakness Enumeration*) es un catálogo internacional de tipos de debilidad de software. La inyección SQL es una de las más frecuentes y peligrosas del catálogo **SANS/CWE Top 25**.

---

## 4. Ataque 1 · Ingresar sin conocer la contraseña

En el campo **correo** del formulario de ingreso, un atacante introduce el siguiente texto (y en la contraseña, cualquier valor):

```
' OR '1'='1' -- 
```

Con ese texto, la consulta que recibe la base de datos queda así:

```sql
SELECT * FROM usuarios WHERE correo = '' OR '1'='1' -- ' AND clave = '...'
```

La consulta se puede leer por partes:

- La comilla `'` cierra el correo antes de tiempo.
- `OR '1'='1'` agrega una condición que siempre es verdadera.
- Los dos guiones `-- ` convierten el resto en un comentario: el control de la contraseña desaparece.

**Efecto:** la condición es siempre verdadera, la base devuelve un usuario y el sistema concede el acceso **sin conocer ninguna contraseña**.

> [!CAUTION]
> No es un ataque difícil ni poco común: es el más frecuente contra un formulario de ingreso mal escrito, y funciona en la primera versión de casi cualquier proyecto. Por eso es lo primero que hay que corregir.

---

## 5. Ataque 2 · Mostrar todo el catálogo

El buscador del catálogo arma su consulta de la misma forma, uniendo el término de búsqueda:

```php
// buscar.php — VULNERABLE
$q = $_GET['q'];
$sql = "SELECT nombre, precio FROM productos WHERE nombre LIKE '%$q%'";
```

Si el atacante busca el texto:

```
' OR '1'='1
```

la condición se vuelve siempre verdadera y el catálogo devuelve **todos** los productos, incluidos los que estuvieran despublicados o en borrador. Es una consecuencia más leve que la del ingreso, pero muestra que el mismo error aparece en **cada** consulta de la aplicación, no solo en el login.

---

## 6. Qué está en juego en un comercio electrónico

En una tienda, la información que se expone es de **personas que confiaron sus datos**: nombres, correos, direcciones e historial de compras.

> [!IMPORTANT]
> En Uruguay, esos datos están protegidos por la **Ley N.º 18.331 de Protección de Datos Personales**, controlada por la **URCDP** (Unidad Reguladora y de Control de Datos Personales). Quien maneja datos de clientes tiene la obligación legal de protegerlos con medidas de seguridad adecuadas. Proteger el software es, para un comercio electrónico, un requisito legal, no una opción.

Del lado de quien ataca, acceder a un sistema ajeno sin autorización es delito en Uruguay (Ley N.º 20.327, art. 297 BIS). Todas las pruebas de esta clase se realizan **solo sobre el proyecto propio**, en la máquina propia, nunca contra un sitio ajeno.

---

## 7. La defensa: consultas parametrizadas

La solución es siempre la misma: **separar la orden del dato.** La orden SQL se escribe con un marcador (`?`) en el lugar del dato, y el dato se envía por separado. El motor de la base lo trata siempre como un valor, aunque contenga comillas, `OR` o cualquier otro texto.

En PHP con **mysqli** (lo que se utiliza en el proyecto), el ingreso seguro se escribe así:

```php
// login.php — SEGURO
$correo = $_POST['correo'];
$clave  = $_POST['clave'];

// 1. la orden, con un ? donde va el dato
$stmt = $conn->prepare("SELECT * FROM usuarios WHERE correo = ?");

// 2. el dato se envía por separado ("s" = texto)
$stmt->bind_param("s", $correo);
$stmt->execute();

$usuario = $stmt->get_result()->fetch_assoc();

// 3. la contraseña se verifica en PHP, con hashing (nunca dentro del SQL)
if ($usuario && password_verify($clave, $usuario['clave'])) {
    // ingreso correcto
}
```

Si el atacante vuelve a escribir el texto del Ataque 1 en el campo correo, la base busca un usuario cuyo correo sea, **literalmente**, ese texto. Ese correo no existe y no ingresa nadie.

> [!TIP]
> El mismo cambio se aplica al buscador, al detalle de producto y a cada consulta de la aplicación. Si el proyecto usa **PDO** en lugar de mysqli, el patrón es idéntico: la orden lleva `?` y el dato se envía por separado. Lo que no cambia nunca: **el dato siempre va por el marcador, no unido al texto de la consulta.**

---

## 8. Capas adicionales de defensa

Las consultas parametrizadas cierran la inyección. Un sistema bien construido agrega, además, otras capas, para que un descuido no lo exponga todo:

| Medida | Qué evita |
|---|---|
| Conectarse a la base con un usuario de **mínimo privilegio** | Que la aplicación se conecte como administrador. Con un usuario limitado, una inyección hace mucho menos daño. |
| **No mostrar** los errores de SQL al usuario | Que un mensaje de error revele los nombres de las tablas y columnas. En producción, los errores van a un registro, no a la pantalla. |
| Validar el **tipo** de cada dato | Que un dato que debería ser un número llegue como texto. Si se espera un número, se convierte a número. |
| Guardar las contraseñas con **`password_hash`** | Que, si igual se filtra la tabla, las contraseñas se puedan recuperar. Nunca se guardan en texto ni con MD5. |

> [!NOTE]
> El mínimo privilegio y el guardado correcto de contraseñas ya se trataron en las clases 3 y 6. Aquí se aplican al proyecto: son la diferencia entre un descuido acotado y la exposición de toda la base.

---

## 9. Lo que no alcanza

Hay tres soluciones aparentes que **no** cierran la inyección y conviene descartar de entrada:

1. **Escapar las comillas a mano** (por ejemplo, con `addslashes`). Se escapa por muchos lados y es una tarea que se pierde. El motor de la base ya sabe hacerlo correctamente cuando se usan parámetros.
2. **Validar solo en el navegador** (JavaScript). El atacante no usa el formulario: envía el pedido directo al servidor. La validación que importa para la seguridad va siempre en el servidor.
3. **Ocultar los nombres de las tablas.** La seguridad no puede depender de que el diseño sea secreto; es un principio ya visto en la clase 3.

---

## 10. Verificación del proyecto

Antes de la entrega, conviene revisar cada lugar donde la aplicación arma una consulta:

- [ ] El **ingreso**: ¿usa consulta parametrizada y `password_verify`?
- [ ] La **búsqueda del catálogo**: ¿el término va por marcador?
- [ ] El **detalle de producto** (`?id=`): ¿el valor va por marcador?
- [ ] El **ABM de productos** (altas, bajas y modificaciones): ¿todas las consultas son parametrizadas?
- [ ] ¿La aplicación se conecta a la base con un usuario que **no** es administrador?
- [ ] ¿Las contraseñas se guardan con `password_hash`, no con MD5?

---

## 11. Cierre

La inyección SQL ocurre cuando el dato del usuario se mezcla con la orden SQL, y se cierra separándolos con **consultas parametrizadas**. En el proyecto, esto significa revisar el ingreso, la búsqueda, el detalle y el ABM, y escribir cada consulta con un marcador `?`.

> **Para la próxima entrega.** Abrir el archivo del ingreso del proyecto y observar la consulta: ¿arma el correo con `'$correo'` o con `?`? Esa sola línea decide si cualquiera puede ingresar como administrador.

---

### Fuentes

- OWASP. *SQL Injection* y *SQL Injection Prevention Cheat Sheet*. <https://owasp.org/www-community/attacks/SQL_Injection>
- MITRE. *CWE-89: Improper Neutralization of Special Elements used in an SQL Command*. <https://cwe.mitre.org/data/definitions/89.html>
- PHP. *Manual: `mysqli::prepare`, `mysqli_stmt::bind_param`, `PDO::prepare`, `password_hash`*. <https://www.php.net/manual/es/>
- Uruguay. *Ley N.º 18.331 de Protección de Datos Personales* — URCDP. <https://www.gub.uy/unidad-reguladora-control-datos-personales/>
- Uruguay. *Ley N.º 20.327* — delitos informáticos (acceso ilícito, art. 297 BIS).

---

[⬅ Volver al índice del curso](../README.md)
