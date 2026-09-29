# Clase 6 · Vulnerabilidades y cómo se cierran en el código

> No alcanza con saber que existe una falla como la inyección SQL. Hay que reconocer la línea de código que la deja entrar y la línea que la cierra. Esta clase presenta las dos, una al lado de la otra.

**Contenidos del programa:** 1.3 Seguridad de redes, sistemas, aplicaciones y datos
**Competencia:** CET1 · CET2
**Tiempo de lectura:** unos 25 minutos

---

📥 **Presentación de la clase:** [Clase 6 · Vulnerabilidades y cómo se cierran](../presentaciones/Clase6-Vulnerabilidades-y-Superficie-de-Ataque.pptx) — `.pptx`, se descarga con el botón **Download** al abrir el enlace.

💻 **Código de la clase:** el portal de boletines (versión vulnerable y versión corregida) lo prepara el docente en las máquinas de la sala. No se publica en este repositorio: no corresponde alojar código vulnerable en un sitio público.

---

> [!TIP]
> **Cómo se organiza el material.** Cada falla se presenta en tres partes: **qué es**, **cómo se ve en el código** y **con qué patrón se cierra**. No se memoriza sintaxis de PHP. El objetivo es poder leer un fragmento de código, identificar por dónde entra un ataque y saber qué patrón lo corrige. Eso mismo se practica en el laboratorio, sobre una aplicación que corre de verdad.

## Contenido

1. [De la amenaza al código](#1-de-la-amenaza-al-código)
2. [Amenaza, vulnerabilidad y patrón](#2-amenaza-vulnerabilidad-y-patrón)
3. [Cómo se nombran: CWE](#3-cómo-se-nombran-cwe)
4. [Superficie de ataque de una aplicación](#4-superficie-de-ataque-de-una-aplicación)
5. [El error de fondo: confiar en lo que llega de afuera](#5-el-error-de-fondo-confiar-en-lo-que-llega-de-afuera)
6. [Falla 1 · Inyección SQL → consulta parametrizada](#6-falla-1--inyección-sql--consulta-parametrizada)
7. [Falla 2 · XSS → codificación de salida](#7-falla-2--xss--codificación-de-salida)
8. [Falla 3 · IDOR → control de acceso por objeto](#8-falla-3--idor--control-de-acceso-por-objeto)
9. [Falla 4 · Salto de directorio → lista blanca](#9-falla-4--salto-de-directorio--lista-blanca)
10. [Falla 5 · Contraseñas → hash lento con sal](#10-falla-5--contraseñas--hash-lento-con-sal)
11. [Los cinco patrones en una tabla](#11-los-cinco-patrones-en-una-tabla)
12. [Errores frecuentes](#12-errores-frecuentes)
13. [Para la próxima clase](#13-para-la-próxima-clase)

---

## 1. De la amenaza al código

Las clases 4 y 5 trataron **quién ataca**: malware, phishing, ingeniería social, DoS y APT. Esta clase estudia el otro lado: **por dónde entra el ataque**, esta vez en el código que lo permite.

Casi todos esos ataques necesitan una **puerta**. El phishing roba una contraseña, pero esa contraseña tiene que servir para entrar. El ransomware llega por un adjunto, pero luego tiene que ejecutarse. Esas puertas son las **vulnerabilidades**, y la mayoría de las que importan hoy están en el **software**: en cómo está escrito el programa.

Las más comunes son pocas, se repiten en todos los sistemas y cada una se cierra con un **patrón** conocido. No hay que inventar una solución: hay que reconocer la falla y aplicar el patrón que le corresponde.

---

## 2. Amenaza, vulnerabilidad y patrón

Conviene separar tres términos antes de empezar:

| Palabra | Qué es | Ejemplo |
|---|---|---|
| **Amenaza** | Quién o qué podría atacar. | Alguien que quiere entrar al portal sin contraseña. |
| **Vulnerabilidad** | La debilidad del sistema que lo permite. | El login une la cédula al texto del SQL. |
| **Patrón** (de solución) | La forma conocida de cerrar esa debilidad. | Usar una consulta parametrizada. |

> [!IMPORTANT]
> De las tres, la **vulnerabilidad es la única sobre la que se actúa directamente**. No se elige quién ataca; sí se elige cómo se escribe el código. Por eso el trabajo de defensa está casi todo en la columna del medio.

Un **patrón** es una solución probada a un problema que se repite. No es un recurso ocasional: es la manera en que hoy se resuelve *siempre* ese problema. Cuando el material indica «aquí va una consulta parametrizada», no es una opinión: es el patrón, y apartarse de él vuelve a abrir la falla.

---

## 3. Cómo se nombran: CWE

Cuando una falla es de un **tipo** conocido, tiene un identificador **CWE** (*Common Weakness Enumeration*): un catálogo internacional de tipos de debilidad de software. No describe un ataque concreto en un producto concreto —eso sería un CVE—; describe la **familia**.

| CWE | Nombre | En una frase |
|---|---|---|
| **CWE-89** | Inyección SQL | Datos del usuario terminan siendo órdenes para la base. |
| **CWE-79** | Cross-site scripting (XSS) | Texto del usuario termina siendo código en el navegador. |
| **CWE-639** | Referencia insegura a objeto (IDOR) | Cambiando un número en la URL se accede a lo ajeno. |
| **CWE-22** | Salto de directorio | Con `../` se llega a archivos que no correspondían. |
| **CWE-916** | Hash de contraseña débil | Las contraseñas se guardan de una forma que se rompe fácil. |

> [!NOTE]
> Estas cinco están entre las que más aparecen en el **SANS/CWE Top 25**, la lista de las debilidades de software más peligrosas y frecuentes. Conocerlas cubre buena parte de lo que se explota en la práctica.

---

## 4. Superficie de ataque de una aplicación

> **Definición.** La superficie de ataque es el conjunto de **todos los puntos por los que entra información a la aplicación desde afuera**.

En una aplicación web, cada uno de esos puntos es una entrada que controla otra persona:

| Entrada | Ejemplo en el portal del liceo |
|---|---|
| Campos de un formulario | La cédula y la contraseña del login. |
| Parámetros en la URL | `boletin.php?id=3` — el `3` lo define quien pide la página. |
| Término de búsqueda | Lo que se escribe en el buscador. |
| Nombre de un archivo a descargar | `descargar.php?archivo=...` |

Cada entrada es un lugar donde probar un ataque. **Reducir la superficie** significa tener menos entradas y, sobre todo, **no confiar en ninguna**.

---

## 5. El error de fondo: confiar en lo que llega de afuera

Las cinco fallas de esta clase parecen distintas, pero en el fondo son **el mismo error**:

> El programa confía en que lo que llega de afuera es lo que espera.

- Espera una cédula, y llega un fragmento de SQL.
- Espera un nombre para buscar, y llega un `<script>`.
- Espera que se pida el boletín propio, y se pide el de otra persona.
- Espera el nombre de un archivo del liceo, y llega `../conexion.php`.

Por eso hay una sola regla, y las cinco soluciones son formas de aplicarla:

> [!IMPORTANT]
> **Nunca se confía en lo que viene de afuera.** Todo dato que entra se valida, se trata como dato (nunca como orden) y se controla que quien lo envía tenga permiso. Los cinco patrones que siguen son esa regla, aplicada en cinco lugares distintos.

A continuación, las cinco fallas, cada una con el código que la abre y el que la cierra.

---

## 6. Falla 1 · Inyección SQL → consulta parametrizada

**La amenaza:** entrar al portal sin conocer ninguna contraseña.

**Cómo se ve en el portal:** en el login, en el campo de la cédula, se introduce el texto:

```
' OR rol='adscripta' -- 
```

y se ingresa como adscripta, que ve todos los boletines. No se adivinó nada: se aprovechó cómo está escrito el código.

**El código que lo permite** (`login.php` de la versión vulnerable):

```php
$sql = "SELECT * FROM usuarios WHERE cedula = '$cedula' AND clave = '...'";
```

La cédula escrita por el usuario se **une directamente** al texto del SQL. Si en lugar de una cédula se envía `' OR rol='adscripta' -- `, la consulta que se ejecuta es:

```sql
SELECT * FROM usuarios WHERE cedula = '' OR rol='adscripta' -- ' AND clave = '...'
```

El `--` convierte el resto en un comentario (desaparece el control de la clave), y el `OR rol='adscripta'` hace que la base devuelva a la adscripta. **El dato del usuario se transformó en una orden.**

**El patrón que lo cierra: consulta parametrizada.** La cédula viaja como un **parámetro aparte**, no unida al texto. El motor de la base la trata siempre como un dato, nunca como parte de la orden.

```php
$stmt = $db->prepare("SELECT * FROM usuarios WHERE cedula = ?");
$stmt->execute([$cedula]);
```

Ahora, sea cual sea el texto enviado, entra completo en el lugar del `?` y se busca *esa cédula literal*. `' OR rol='adscripta' -- ` se busca como si fuera una cédula inexistente, y no ingresa nadie.

> [!TIP]
> Es el patrón más importante de la clase, y la regla no tiene excepción: **el dato del usuario nunca se une al SQL**. Siempre va por parámetro. Donde aparece un `"... '$algo' ..."` armando una consulta, hay una inyección posible.

---

## 7. Falla 2 · XSS → codificación de salida

**La amenaza:** que el navegador de otra persona ejecute código introducido por un atacante.

**Cómo se ve en el portal:** en el buscador se introduce:

```html
<script>alert('robé tu sesión')</script>
```

y el navegador, en lugar de mostrar ese texto, **lo ejecuta**. Si en vez de un `alert` fuera código que roba la cookie de sesión, se la enviaría al atacante.

**El código que lo permite** (`buscar.php`):

```php
<h2>Resultados para: <?= $q ?></h2>
```

Lo buscado (`$q`) se inserta **sin procesar** en el HTML. Si contiene etiquetas, el navegador las interpreta como parte de la página. **El texto del usuario se transformó en código.** Es el mismo error que la inyección SQL, en otro lenguaje: allí era SQL, aquí es HTML.

**El patrón que lo cierra: codificación de salida.** Antes de mostrar cualquier dato, se lo **escapa**: los caracteres especiales del HTML (`<`, `>`, `"`) se convierten en su versión inofensiva (`&lt;`, `&gt;`, `&quot;`), que se ve igual en pantalla pero no se ejecuta.

```php
<h2>Resultados para: <?= h($q) ?></h2>
```

donde `h()` aplica `htmlspecialchars`. Ahora `<script>` aparece como texto visible y sin efecto.

> [!NOTE]
> El patrón es **escapar en la salida**, no «prohibir caracteres raros en la entrada». Una búsqueda con `<` puede ser legítima (`nota < 6`). El problema no es que el dato tenga un `<`, sino mostrarlo sin escapar. Se corrige donde se muestra, no donde se recibe.

---

## 8. Falla 3 · IDOR → control de acceso por objeto

**La amenaza:** ver datos de otra persona cambiando un número en la dirección.

**Cómo se ve en el portal:** una estudiante ve su boletín en `boletin.php?id=1`. Cambia el número a `boletin.php?id=3` y accede a un registro de la adscripción que no debería ver. No se vulneró nada: se **editó la URL**.

**El código que lo permite** (`boletin.php`):

```php
exigir_sesion();                 // pide estar logueado... y nada más
$id = $_GET['id'];
// trae y muestra el boletín $id, sea de quien sea
```

El programa verifica que **haya** una sesión, pero no que **este** usuario pueda ver **este** boletín. Tener entrada al edificio no es tener llave de todas las oficinas.

**El patrón que lo cierra: control de acceso por objeto.** Antes de mostrar el boletín, se comprueba que pertenezca a quien lo pide (o que su rol lo autorice).

```php
$es_propio    = ($boletin['usuario_id'] == $yo['id']);
$es_adscripta = ($yo['rol'] === 'adscripta');
if (!$es_propio && !$es_adscripta) {
    http_response_code(403);
    exit('No tenés permiso para ver este boletín.');
}
```

Ahora cada estudiante ve el suyo; una petición ajena recibe un `403`. Es el principio de **mínimo privilegio** de la clase 3, escrito en PHP.

> [!CAUTION]
> Es la más silenciosa de las cinco, porque el código «funciona»: muestra un boletín, no da error. La falla no se ve usando la aplicación con normalidad; se ve al preguntar «¿y si pido el de otro?». **Autenticar** (saber quién es) no es **autorizar** (saber qué puede ver). Son dos cosas distintas, y hacen falta las dos.

---

## 9. Falla 4 · Salto de directorio → lista blanca

**La amenaza:** leer archivos del servidor que no estaban previstos para descarga.

**Cómo se ve en el portal:** la página ofrece descargar `calendario.txt` con un enlace a `descargar.php?archivo=calendario.txt`. Se cambia el final por:

```
descargar.php?archivo=../conexion.php
```

y se descarga el archivo de conexión, **con la contraseña de la base de datos adentro**. Con más `../` se puede subir hasta archivos del sistema operativo.

**El código que lo permite** (`descargar.php`):

```php
$archivo = $_GET['archivo'];
readfile('archivos/' . $archivo);   // sirve lo que le pidan
```

El programa une el nombre recibido a la ruta y entrega ese archivo. Con `../` se sale de la carpeta `archivos/`.

**El patrón que lo cierra: lista blanca.** En lugar de confiar en el nombre recibido, solo se permiten los archivos de una **lista fija**. Lo que no está en la lista, no se sirve.

```php
$permitidos = ['calendario.txt', 'reglamento.txt'];
if (!in_array($archivo, $permitidos, true)) {
    http_response_code(404);
    exit('No se encontró el archivo.');
}
readfile('archivos/' . $archivo);
```

Pedir `../conexion.php` ya no funciona: no está en la lista y se responde `404`.

> [!TIP]
> La **lista blanca** (declarar qué SÍ se permite) le gana siempre a la **lista negra** (intentar prohibir lo malo). La lista negra olvida algún caso; la lista blanca deja afuera, por omisión, todo lo no permitido. Es el patrón de *predeterminados a prueba de fallos* de la clase 3.

---

## 10. Falla 5 · Contraseñas → hash lento con sal

**La amenaza:** que, si alguien obtiene la tabla de usuarios, pueda recuperar las contraseñas.

**Cómo se ve en el portal:** la base guarda la contraseña así:

```
81dc9bdb52d04dc20036dbd8313ed055
```

Eso es `md5('1234')`. El problema: MD5 es una función **rápida y sin sal**, y esa cadena figura en cualquier tabla de las que circulan. Se busca, aparece `1234` al instante y la contraseña queda al descubierto. (Los detalles de hash, sal y funciones lentas están en el [glosario](../recursos/glosario.md).)

**El código que lo permite** (`login.php` y `esquema.sql`):

```php
// se guarda y se compara con MD5
... WHERE clave = '" . md5($clave) . "'
```

**El patrón que lo cierra: hash lento con sal.** Se usa una función **diseñada para contraseñas** —`password_hash()`, que en PHP usa bcrypt—: es **lenta a propósito** (cada intento del atacante cuesta) y agrega una **sal** distinta a cada una (dos contraseñas iguales dan hashes distintos, y las tablas precalculadas no sirven).

```php
// al crear la cuenta:
$hash = password_hash($clave, PASSWORD_DEFAULT);   // bcrypt + sal automática
// al ingresar:
if (password_verify($clave, $fila['clave'])) { ... }
```

El sistema nunca guarda ni compara la contraseña en claro, y el hash guardado (`$2y$12$...`) no se rompe con una tabla.

> [!NOTE]
> En la versión segura **nunca aparece `md5`**. No se inventa el guardado de contraseñas: existe una función hecha para eso (`password_hash` / `password_verify`), y usarla es suficiente. Según el estándar vigente (NIST, 2025): mínimo 15 caracteres si es el único factor, no obligar a cambiarla salvo filtración, y compararla contra listas de contraseñas ya filtradas.

---

## 11. Los cinco patrones en una tabla

Esta es la clase entera resumida. Cada falla, el error de fondo y el patrón:

| Falla (CWE) | El dato del usuario… | Patrón | En una línea |
|---|---|---|---|
| **Inyección SQL** (89) | …se volvió parte de la consulta | **Consulta parametrizada** | El dato va por `?`, nunca pegado al SQL. |
| **XSS** (79) | …se volvió código en el navegador | **Codificación de salida** | Se escapa con `htmlspecialchars` al mostrarlo. |
| **IDOR** (639) | …pidió algo ajeno y se lo entregaron | **Control de acceso por objeto** | Se verifica que sea suyo, no solo que haya sesión. |
| **Salto de directorio** (22) | …nombró un archivo prohibido | **Lista blanca** | Solo se sirve lo que está en una lista fija. |
| **Hash débil** (916) | *(no aplica: es cómo se guarda)* | **Hash lento con sal** | `password_hash` / `password_verify`, nunca MD5. |

> [!IMPORTANT]
> En la columna del medio, cuatro de las cinco son la misma frase —«el dato del usuario se volvió otra cosa»— y todas se cierran igual: **tratar el dato como dato**, no como orden, no como código, no como permiso. Ese es el concepto. Los patrones son cómo se escribe en cada caso.

---

## 12. Errores frecuentes

> [!WARNING]
> Estos cuatro son los que más se repiten al aplicar los patrones.

**1. Creer que validar la entrada alcanza para el XSS.** Filtrar caracteres en la entrada es frágil y se evade. El patrón del XSS es **escapar en la salida**, donde el dato se muestra.

**2. Confundir estar logueado con tener permiso.** El IDOR ocurre en aplicaciones que sí piden login. Autenticar no es autorizar: hay que verificar el permiso **sobre cada objeto**, cada vez.

**3. Usar lista negra en vez de lista blanca.** Intentar prohibir `../` y todas sus variantes es una tarea que se pierde. Declarar qué archivos **sí** se permiten la resuelve.

**4. Inventar el guardado de contraseñas.** MD5, SHA-1, «MD5 dos veces» o MD5 con una sal fija en el código están rotos. Existe `password_hash`: se usa y punto.

---

## 13. Para la próxima clase

📝 **Actividad:** [Romperla y arreglarla](../actividades/06-romper-y-arreglar.md) — laboratorio en la sala: explotar las cinco fallas y aplicar los cinco patrones sobre código que corre. Es la primera evaluación formativa del curso.

✅ **Autoevaluación:** [Poné a prueba lo leído](../autoevaluacion/06-vulnerabilidades-y-superficie-de-ataque.md) — 10 preguntas con respuestas explicadas.

💻 **Código:** el portal (vulnerable y seguro) lo prepara el docente en la sala; no está en este repositorio público.

📖 **Consulta:** [Glosario](../recursos/glosario.md) · [Bibliografía](../recursos/bibliografia.md) · [Normativa uruguaya](../recursos/normativa-uruguay.md)

🔭 **Lo que viene:** la clase 6B profundiza en la inyección SQL sobre el proyecto de e-commerce. Más adelante, la clase 7 pasa al contenido 1.4, tecnologías emergentes e inteligencia artificial.

---

### Fuentes

- MITRE. *Common Weakness Enumeration* (CWE) — tipos de debilidad de software. <https://cwe.mitre.org/>
- SANS / CWE. *Top 25 Most Dangerous Software Weaknesses*. <https://www.sans.org/top25-software-errors/>
- OWASP. *Top 10* y hojas de referencia (inyección SQL, XSS, control de acceso). <https://owasp.org/>
- PHP. *Manual: `password_hash`, `PDO::prepare`, `htmlspecialchars`*. <https://www.php.net/manual/es/>
- NIST. *SP 800-63B-4, Digital Identity Guidelines* (2025) — requisitos de contraseñas. <https://pages.nist.gov/800-63-4/>

---

[⬅ Volver al índice del curso](../README.md)
