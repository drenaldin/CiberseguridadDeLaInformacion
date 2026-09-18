# Clase 6 · Vulnerabilidades y cómo se cierran en el código

> No alcanza con saber que existe la inyección SQL. Hay que ver la línea que la deja entrar, y la línea que la cierra. Esta clase es las dos líneas, una al lado de la otra

**Contenidos del programa:** 1.3 Seguridad de redes, sistemas, aplicaciones y datos
**Competencia:** CET1 · CET2
**Tiempo de lectura:** unos 30 minutos

---

📥 **Presentación de la clase:** [Clase 6 · Vulnerabilidades y cómo se cierran](../presentaciones/Clase6-Vulnerabilidades-y-Superficie-de-Ataque.pptx) — `.pptx`, se descarga con el botón **Download** que aparece arriba a la derecha al abrir el enlace.

💻 **Código de la clase:** el portal de boletines (versión vulnerable y versión corregida) lo prepara el docente en las máquinas de la sala. No se publica en este repositorio: código vulnerable a propósito no debe quedar en un sitio público.

---

> [!TIP]
> **Cómo usar este material.** Cada vulnerabilidad de esta clase viene en tres partes: **qué es**, **cómo se ve en el código** y **con qué patrón se cierra**. No hay que memorizar sintaxis de PHP. Hay que poder mirar un pedazo de código, decir «acá entra un ataque» y saber qué patrón lo arregla. Eso es lo que vas a hacer en la práctica, sobre una aplicación que corre de verdad.

## Contenido

1. [De la amenaza al código](#1-de-la-amenaza-al-código)
2. [Amenaza, vulnerabilidad y patrón](#2-amenaza-vulnerabilidad-y-patrón)
3. [Cómo se nombran: CWE](#3-cómo-se-nombran-cwe)
4. [Superficie de ataque de una aplicación](#4-superficie-de-ataque-de-una-aplicación)
5. [Confiar en lo que llega de afuera: el error de fondo](#5-confiar-en-lo-que-llega-de-afuera-el-error-de-fondo)
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

Las clases 4 y 5 vieron **quién ataca**: malware, phishing, ingeniería social, DoS, APT. Esta ve el otro lado: **por dónde entra el ataque**, y esta vez no en abstracto, sino en el código que lo deja pasar.

Casi todos los ataques que vimos necesitan una **puerta**. El phishing roba una contraseña, pero después esa contraseña tiene que servir para entrar. El ransomware llega por un adjunto, pero después tiene que ejecutarse. Esas puertas son las **vulnerabilidades**, y la mayoría de las que importan hoy están en el **software**: en cómo está escrito el programa.

La buena noticia es que las más comunes son pocas, se repiten en todos lados, y cada una se cierra con un **patrón** conocido. No hay que inventar nada: hay que reconocer la falla y aplicar el patrón que le corresponde.

---

## 2. Amenaza, vulnerabilidad y patrón

Tres palabras que conviene separar antes de empezar:

| Palabra | Qué es | Ejemplo |
|---|---|---|
| **Amenaza** | Quien o qué podría atacar. | Alguien que quiere entrar al portal sin contraseña. |
| **Vulnerabilidad** | La debilidad del sistema que lo permite. | El login pega la cédula directo al texto del SQL. |
| **Patrón** (de solución) | La forma conocida de cerrar esa debilidad. | Usar una consulta parametrizada. |

> [!IMPORTANT]
> De las tres, la **vulnerabilidad es la única sobre la que actuás directamente**. No elegís quién te ataca. Sí elegís cómo escribís el código. Por eso el trabajo de defensa está casi todo en la columna del medio.

Un **patrón** es una solución probada a un problema que se repite. No es un truco de una vez: es la manera en que hoy se resuelve *siempre* ese problema. Cuando veas «acá va una consulta parametrizada», no es una opinión: es el patrón, y desviarse de él es volver a abrir la falla.

---

## 3. Cómo se nombran: CWE

Cuando una falla es de un **tipo** conocido, tiene un identificador **CWE** (*Common Weakness Enumeration*): un catálogo de tipos de debilidad de software. No es un ataque concreto en un producto concreto —eso sería un CVE—; es la **familia**.

| CWE | Nombre | En una frase |
|---|---|---|
| **CWE-89** | Inyección SQL | Datos del usuario terminan siendo órdenes para la base. |
| **CWE-79** | Cross-site scripting (XSS) | Texto del usuario termina siendo código en el navegador. |
| **CWE-639** | Referencia insegura a objeto (IDOR) | Cambiando un número en la URL se accede a lo ajeno. |
| **CWE-22** | Salto de directorio | Con `../` se llega a archivos que no correspondían. |
| **CWE-916** | Hash de contraseña débil | Las contraseñas se guardan de una forma que se rompe fácil. |

> [!NOTE]
> Estas cinco no están elegidas al azar: son de las que más aparecen en el **SANS/CWE Top 25**, la lista de las debilidades de software más peligrosas y frecuentes. Aprender estas cinco cubre una parte enorme de lo que se explota en la vida real.

---

## 4. Superficie de ataque de una aplicación

> **Definición.** La superficie de ataque es el conjunto de **todos los puntos por los que le entra información a tu programa desde afuera**.

En una aplicación web, cada uno de esos puntos es una entrada que alguien controla y vos no:

| Entrada | Ejemplo en el portal del liceo |
|---|---|
| Campos de un formulario | La cédula y la contraseña del login. |
| Parámetros en la URL | `boletin.php?id=3` — el `3` lo pone quien pide la página. |
| Lo que se busca | El término del buscador. |
| El nombre de un archivo a descargar | `descargar.php?archivo=...` |

Cada flecha que entra es un lugar donde probar un ataque. **Reducir la superficie** es tener menos entradas, y sobre todo, **no confiar en ninguna**.

---

## 5. Confiar en lo que llega de afuera: el error de fondo

Las cinco fallas de esta clase parecen distintas, pero abajo son **el mismo error**:

> El programa confía en que lo que llega de afuera es lo que espera.

- Espera una cédula, y llega un pedazo de SQL.
- Espera un nombre para buscar, y llega un `<script>`.
- Espera que pidas *tu* boletín, y pedís el de otro.
- Espera el nombre de un archivo del liceo, y llega `../conexion.php`.

Por eso hay una sola regla, y las cinco soluciones son formas de aplicarla:

> [!IMPORTANT]
> **Nunca confíes en lo que viene de afuera.** Todo dato que entra se valida, se trata como dato (nunca como orden) y se controla que quien lo manda tenga permiso. Los cinco patrones que siguen son esa regla, aplicada en cinco lugares distintos.

Ahora, las cinco, cada una con el código que la abre y el que la cierra.

---

## 6. Falla 1 · Inyección SQL → consulta parametrizada

**La amenaza:** entrar al portal sin saber ninguna contraseña.

**Cómo se ve en el portal:** en el login, en el campo de la cédula, alguien escribe esto:

```
' OR rol='adscripta' -- 
```

y entra como adscripta, que ve todos los boletines. No adivinó nada: aprovechó cómo está escrito el código.

**El código que lo permite** (`login.php` de la versión vulnerable):

```php
$sql = "SELECT * FROM usuarios WHERE cedula = '$cedula' AND clave = '...'";
```

La cédula que escribió el usuario se **pega directo** al texto del SQL. Si en vez de una cédula manda `' OR rol='adscripta' -- `, la consulta que termina ejecutándose es:

```sql
SELECT * FROM usuarios WHERE cedula = '' OR rol='adscripta' -- ' AND clave = '...'
```

El `--` convierte el resto en un comentario (desaparece el control de la clave), y el `OR rol='adscripta'` hace que la base devuelva a la adscripta. **El dato del usuario se transformó en una orden.**

**El patrón que lo cierra: consulta parametrizada.** La cédula viaja como un **parámetro aparte**, no pegada al texto. El motor de la base la trata siempre como un dato, nunca como parte de la orden.

```php
$stmt = $db->prepare("SELECT * FROM usuarios WHERE cedula = ?");
$stmt->execute([$cedula]);
```

Ahora, mande lo que mande el usuario, ese texto entra entero en el lugar del `?` y se busca *esa cédula literal*. `' OR rol='adscripta' -- ` se busca como si fuera una cédula rarísima, no existe, y no entra nadie.

> [!TIP]
> Es el patrón más importante de la clase. La regla es simple y no tiene excepción: **el dato del usuario nunca se concatena al SQL**. Siempre va por parámetro. Si ves un `"... '$algo' ..."` armando una consulta, ahí hay una inyección esperando.

---

## 7. Falla 2 · XSS → codificación de salida

**La amenaza:** que el navegador de otra persona ejecute código que puso un atacante.

**Cómo se ve en el portal:** en el buscador, alguien escribe:

```html
<script>alert('robé tu sesión')</script>
```

y el navegador, en vez de mostrar ese texto, **lo ejecuta**. Si en lugar de un `alert` fuera código que roba la cookie de sesión, se la manda al atacante.

**El código que lo permite** (`buscar.php`):

```php
<h2>Resultados para: <?= $q ?></h2>
```

Lo que buscó el usuario (`$q`) se mete **tal cual** en el HTML. Si `$q` trae etiquetas, el navegador las entiende como HTML de la página. **El texto del usuario se transformó en código.** Es el mismo error que la inyección SQL, en otro idioma: allá era SQL, acá es HTML.

**El patrón que lo cierra: codificación de salida.** Antes de mostrar cualquier dato, se lo **escapa**: los caracteres especiales del HTML (`<`, `>`, `"`) se convierten en su versión inofensiva (`&lt;`, `&gt;`, `&quot;`), que se ve igual en pantalla pero no se ejecuta.

```php
<h2>Resultados para: <?= h($q) ?></h2>
```

donde `h()` es una función que aplica `htmlspecialchars`. Ahora `<script>` aparece como texto `<script>` en la página, visible y muerto.

> [!NOTE]
> El patrón es **escapar en la salida**, no «prohibir caracteres raros en la entrada». La misma búsqueda con `<` puede ser legítima (`nota < 6`). El problema no es que el dato tenga un `<`; el problema es mostrarlo sin escapar. Se arregla donde se muestra, no donde se recibe.

---

## 8. Falla 3 · IDOR → control de acceso por objeto

**La amenaza:** ver datos de otra persona cambiando un número en la dirección.

**Cómo se ve en el portal:** Camila, que es estudiante, está viendo su boletín en `boletin.php?id=1`. Cambia el número a mano: `boletin.php?id=3`. Y ve el registro interno de la adscripción, que no debería poder ver. No hackeó nada: **editó la URL**.

**El código que lo permite** (`boletin.php`):

```php
exigir_sesion();                 // pide estar logueado... y nada más
$id = $_GET['id'];
// trae y muestra el boletín $id, sea de quien sea
```

El programa verifica que **haya** una sesión, pero no que **este** usuario pueda ver **este** boletín. Tener entrada al edificio no es tener llave de todas las oficinas.

**El patrón que lo cierra: control de acceso por objeto.** Antes de mostrar el boletín, se comprueba que le pertenezca a quien lo pide (o que tenga un rol que lo autorice).

```php
$es_propio    = ($boletin['usuario_id'] == $yo['id']);
$es_adscripta = ($yo['rol'] === 'adscripta');
if (!$es_propio && !$es_adscripta) {
    http_response_code(403);
    exit('No tenés permiso para ver este boletín.');
}
```

Ahora Camila ve el suyo; si pide el 3, recibe un `403`. Es el principio de **mínimo privilegio** de la clase 3, escrito en PHP: cada uno accede solo a lo suyo.

> [!CAUTION]
> Este es el más silencioso de los cinco, porque el código «funciona»: muestra un boletín, no da error. La falla no se ve probando la aplicación normalmente; se ve preguntando «¿y si pido el de otro?». Autenticar (saber quién sos) no es autorizar (saber qué podés ver). Son dos cosas distintas, y hacen falta las dos.

---

## 9. Falla 4 · Salto de directorio → lista blanca

**La amenaza:** leer archivos del servidor que no eran para descargar.

**Cómo se ve en el portal:** la página ofrece descargar `calendario.txt` y `reglamento.txt`, con enlaces a `descargar.php?archivo=calendario.txt`. Alguien cambia el final por:

```
descargar.php?archivo=../conexion.php
```

y descarga el archivo de conexión, **con la contraseña de la base de datos adentro**. Con más `../` puede subir hasta archivos del sistema operativo.

**El código que lo permite** (`descargar.php`):

```php
$archivo = $_GET['archivo'];
readfile('archivos/' . $archivo);   // sirve lo que le pidan
```

El programa pega el nombre que llega a la ruta y entrega ese archivo. Con `../` se sale de la carpeta `archivos/` y se llega a cualquier lado.

**El patrón que lo cierra: lista blanca.** En vez de confiar en el nombre que llega, solo se permiten los archivos de una **lista fija**. Lo que no está en la lista, no se sirve.

```php
$permitidos = ['calendario.txt', 'reglamento.txt'];
if (!in_array($archivo, $permitidos, true)) {
    http_response_code(404);
    exit('No se encontró el archivo.');
}
readfile('archivos/' . $archivo);
```

Pedir `../conexion.php` ya no sirve: no está en la lista, y se responde `404`.

> [!TIP]
> **Lista blanca** (decir qué SÍ se permite) le gana siempre a **lista negra** (tratar de prohibir lo malo). La lista negra se olvida de un caso; la lista blanca, no: todo lo que no está permitido, queda afuera por omisión. Es el patrón de *predeterminados a prueba de fallos* de la clase 3.

---

## 10. Falla 5 · Contraseñas → hash lento con sal

**La amenaza:** que, si alguien roba la tabla de usuarios, pueda recuperar las contraseñas.

**Cómo se ve en el portal:** la base guarda la contraseña de Camila así:

```
81dc9bdb52d04dc20036dbd8313ed055
```

Eso es `md5('1234')`. El problema: MD5 es una función **rápida y sin sal**, y esa cadena está en cualquier tabla de las que circulan. Se busca, aparece `1234` al instante, y la contraseña quedó al descubierto. (Los detalles de por qué —hash, sal, funciones lentas— están en el [glosario](../recursos/glosario.md).)

**El código que lo permite** (`login.php` y `esquema.sql`):

```php
// se guarda y se compara con MD5
... WHERE clave = '" . md5($clave) . "'
```

**El patrón que lo cierra: hash lento con sal.** Se usa una función **diseñada para contraseñas** —`password_hash()`, que en PHP usa bcrypt—: es **lenta a propósito** (cada intento del atacante cuesta) y le agrega una **sal** distinta a cada una (dos contraseñas iguales dan hashes distintos, y las tablas precalculadas no sirven).

```php
// al crear la cuenta:
$hash = password_hash($clave, PASSWORD_DEFAULT);   // bcrypt + sal automática
// al entrar:
if (password_verify($clave, $fila['clave'])) { ... }
```

El sistema nunca guarda ni compara la contraseña en claro, y el hash guardado (`$2y$12$...`) no se rompe con una tabla.

> [!NOTE]
> Fijate que en la versión segura **nunca aparece `md5`**. La regla es no inventar el guardado de contraseñas: existe una función hecha para eso (`password_hash`/`password_verify`), y usarla es todo. Reglas del estándar vigente (NIST, 2025): mínimo 15 caracteres si es el único factor, no obligar a cambiarla salvo que se filtre, y compararla contra listas de contraseñas ya filtradas.

---

## 11. Los cinco patrones en una tabla

Esta es la clase entera en una imagen. Cada falla, el error de fondo, y el patrón:

| Falla (CWE) | El dato del usuario… | Patrón | En una línea |
|---|---|---|---|
| **Inyección SQL** (89) | …se volvió parte de la consulta | **Consulta parametrizada** | El dato va por `?`, nunca pegado al SQL. |
| **XSS** (79) | …se volvió código en el navegador | **Codificación de salida** | Se escapa con `htmlspecialchars` al mostrarlo. |
| **IDOR** (639) | …pidió algo ajeno y se lo dieron | **Control de acceso por objeto** | Se verifica que sea suyo, no solo que haya sesión. |
| **Salto de directorio** (22) | …nombró un archivo prohibido | **Lista blanca** | Solo se sirve lo que está en una lista fija. |
| **Hash débil** (916) | *(no aplica: es cómo se guarda)* | **Hash lento con sal** | `password_hash` / `password_verify`, nunca MD5. |

> [!IMPORTANT]
> Mirá la columna del medio: cuatro de las cinco son la misma frase —«el dato del usuario se volvió otra cosa»— y todas se cierran igual: **tratar el dato como dato**, no como orden, no como código, no como permiso. Ese es el concepto. Los patrones son cómo se escribe en cada caso.

---

## 12. Errores frecuentes

> [!WARNING]
> Estos cuatro son los que más se repiten al aplicar los patrones.

**1. Creer que validar la entrada alcanza para el XSS.** Filtrar caracteres a la entrada es frágil y se escapa. El patrón del XSS es **escapar en la salida**, donde el dato se muestra, no donde se recibe.

**2. Confundir estar logueado con tener permiso.** El IDOR pasa en aplicaciones que sí piden login. Autenticar no es autorizar: hay que verificar el permiso **sobre cada objeto**, cada vez.

**3. Usar lista negra en vez de lista blanca.** Tratar de prohibir `../`, `..\`, y todas sus variantes es una pelea que se pierde. Decir qué archivos **sí** se permiten la gana de una.

**4. Inventar el guardado de contraseñas.** MD5, SHA-1, «MD5 dos veces», MD5 con una sal fija escrita en el código: todo eso está roto. Existe `password_hash`. Se usa y punto.

---

## 13. Para la próxima clase

📝 **Actividad:** [Romperla y arreglarla](../actividades/06-romper-y-arreglar.md) — laboratorio en la sala: explotar las cinco fallas y aplicar los cinco patrones sobre código que corre. Es la primera evaluación formativa del curso.

✅ **Autoevaluación:** [Poné a prueba lo que leíste](../autoevaluacion/06-vulnerabilidades-y-superficie-de-ataque.md) — 10 preguntas con respuestas explicadas.

💻 **Código:** el portal (vulnerable y seguro) lo prepara el docente en la sala; no está en este repositorio público.

📖 **Consulta:** [Glosario](../recursos/glosario.md) · [Bibliografía](../recursos/bibliografia.md) · [Normativa uruguaya](../recursos/normativa-uruguay.md)

🔭 **Lo que viene:** en la clase 7 pasamos al contenido 1.4, tecnologías emergentes e inteligencia artificial, y ahí aparece una superficie de ataque nueva: la de los sistemas que usan modelos de IA.

> **Para pensar antes de la próxima clase.** De las cinco fallas, ¿cuál te parece más fácil de cometer sin darte cuenta cuando programás con apuro? No hay respuesta única; hay respuesta fundamentada.

---

### Fuentes

- MITRE. *Common Weakness Enumeration* (CWE) — tipos de debilidad de software. <https://cwe.mitre.org/>
- SANS / CWE. *Top 25 Most Dangerous Software Weaknesses*. <https://www.sans.org/top25-software-errors/>
- OWASP. *Top 10* y hojas de referencia (inyección SQL, XSS, control de acceso). <https://owasp.org/>
- PHP. *Manual: `password_hash`, `PDO::prepare`, `htmlspecialchars`*. <https://www.php.net/manual/es/>
- NIST. *SP 800-63B-4, Digital Identity Guidelines* (2025) — requisitos de contraseñas. <https://pages.nist.gov/800-63-4/>

---

[⬅ Volver al índice del curso](../README.md)
