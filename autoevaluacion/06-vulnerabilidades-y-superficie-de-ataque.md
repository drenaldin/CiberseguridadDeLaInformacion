# Autoevaluación · Clase 6

**Vulnerabilidades y cómo se cierran en el código**

---

Diez preguntas para comprobar por tu cuenta si entendiste el material. **No lleva nota y no se entrega.**

Respondé mentalmente o en papel *antes* de desplegar la respuesta. El efecto de aprendizaje está en el intento, no en la lectura.

> [!TIP]
> En el celular, tocá el triángulo ▸ para desplegar cada respuesta.

📖 Si algo no te sale, volvé a [Clase 6 · Vulnerabilidades y cómo se cierran](../clases/06-vulnerabilidades-y-superficie-de-ataque.md).

---

### 1. ¿Qué diferencia hay entre una vulnerabilidad y un patrón?

<details>
<summary>Ver respuesta</summary>

La **vulnerabilidad** es la debilidad del sistema —cómo está escrito el código que deja entrar el ataque—. El **patrón** es la forma conocida y probada de cerrar esa debilidad.

Ejemplo: la vulnerabilidad es que el login pega la cédula al texto del SQL; el patrón que la cierra es la consulta parametrizada. La vulnerabilidad es lo que hay que arreglar; el patrón es cómo se arregla.
</details>

---

### 2. En la inyección SQL, ¿cuál es exactamente el error del programa?

<details>
<summary>Ver respuesta</summary>

Que **pega el dato del usuario directo al texto de la consulta**. Así, un texto que parece una cédula puede contener órdenes para la base, y el motor las ejecuta.

El dato del usuario se transformó en parte de la orden. Por eso el patrón es la **consulta parametrizada**: el dato viaja por un parámetro (`?`) y el motor lo trata siempre como dato, nunca como parte del SQL.
</details>

---

### 3. Un compañero dice: «para arreglar el XSS, prohíbo que se escriba `<` en el buscador». ¿Alcanza?

<details>
<summary>Ver respuesta</summary>

**No, y además está mirando el lugar equivocado.** Filtrar caracteres en la entrada es frágil (hay muchas formas de escribir lo mismo) y bloquea búsquedas legítimas (`nota < 6`).

El patrón del XSS es **codificar la salida**: escapar el dato con `htmlspecialchars` **cuando se lo muestra**, para que `<script>` aparezca como texto y no se ejecute. Se arregla donde se muestra, no donde se recibe.
</details>

---

### 4. ¿Por qué el IDOR es peligroso incluso en una aplicación que pide login?

<details>
<summary>Ver respuesta</summary>

Porque **estar logueado no es lo mismo que tener permiso**. El IDOR aparece cuando el programa verifica que haya una sesión, pero no que ese usuario pueda ver *ese* objeto.

Camila está logueada; cambia `id=1` por `id=3` en la URL y ve algo ajeno. El patrón es el **control de acceso por objeto**: comprobar, en cada pedido, que el objeto le pertenezca o que su rol lo autorice. Autenticar (quién sos) y autorizar (qué podés ver) son dos cosas distintas.
</details>

---

### 5. ¿Qué permite el salto de directorio, y con qué patrón se cierra?

<details>
<summary>Ver respuesta</summary>

Permite **leer archivos que no eran para descargar**, usando `../` para salir de la carpeta prevista. En el portal, `descargar.php?archivo=../conexion.php` baja el archivo con la contraseña de la base.

Se cierra con una **lista blanca**: en vez de confiar en el nombre que llega, solo se sirven los archivos de una lista fija. Lo que no está en la lista, no se entrega.
</details>

---

### 6. ¿Por qué lista blanca le gana a lista negra?

<details>
<summary>Ver respuesta</summary>

Porque la **lista negra** (prohibir lo malo) siempre se olvida de un caso: hay muchas formas de escribir `../`, y con que se escape una, el ataque pasa.

La **lista blanca** (permitir solo lo bueno) no tiene ese problema: todo lo que no está explícitamente permitido queda afuera por omisión. Es el principio de *predeterminados a prueba de fallos*: por defecto, denegar.
</details>

---

### 7. La contraseña de Camila está guardada como `81dc9bdb...` (un MD5). ¿Cuál es el problema?

<details>
<summary>Ver respuesta</summary>

Que **MD5 es rápido y no tiene sal**. Al ser rápido, un atacante prueba millones de candidatos por segundo; al no tener sal, esa misma cadena ya está en tablas precalculadas que circulan, y la contraseña aparece al instante.

El patrón es un **hash lento con sal**: `password_hash()` (bcrypt), lento a propósito y con una sal distinta por contraseña. Se verifica con `password_verify()`. Nunca MD5.
</details>

---

### 8. ¿Qué tienen en común las cinco fallas de la clase?

<details>
<summary>Ver respuesta</summary>

Que el programa **confía en lo que llega de afuera**. Espera una cédula y llega SQL; espera un nombre y llega un `<script>`; espera que pidas lo tuyo y pedís lo ajeno; espera un archivo del liceo y llega `../conexion.php`.

Los cinco patrones son la misma regla aplicada en cinco lugares: **tratar el dato como dato** —nunca como orden, ni como código, ni como permiso automático— y verificar siempre quién manda qué.
</details>

---

### 9. En el login seguro, ¿por qué la cédula del usuario ya no puede cambiar la consulta?

<details>
<summary>Ver respuesta</summary>

Porque viaja como **parámetro**, no pegada al texto del SQL:

```php
$stmt = $db->prepare("SELECT * FROM usuarios WHERE cedula = ?");
$stmt->execute([$cedula]);
```

Mande lo que mande el usuario, ese texto entra entero en el lugar del `?` y se busca *esa cédula literal*. `' OR rol='adscripta' -- ` se busca como si fuera una cédula rarísima: no existe, y no entra nadie.
</details>

---

### 10. ¿Por qué escribir un buen patrón es mejor que «programar con más cuidado»?

<details>
<summary>Ver respuesta</summary>

Porque el «cuidado» depende de acordarse cada vez, y con apuro se falla. Un **patrón** es una solución probada que se aplica siempre igual: la consulta parametrizada cierra la inyección **haya o no** un atacante, se acuerde o no el que programa.

Los patrones sacan la seguridad de la buena memoria y la ponen en la forma de escribir el código. Por eso se dice «acá va una consulta parametrizada» como una regla, no como una recomendación.
</details>

---

## Cómo te fue

| Respondiste bien | Qué significa |
|---|---|
| 8 a 10 | Estás pronto para el laboratorio. Reconocés la falla y sabés qué patrón le corresponde. |
| 5 a 7 | Repasá la sección 5 (el error de fondo) y la tabla de la sección 11. Son las dos que ordenan todo lo demás. |
| Menos de 5 | Volvé a leer el material. Empezá por la sección 2: si no se separan amenaza, vulnerabilidad y patrón, el resto no se acomoda. |

---

[⬅ Volver a Clase 6](../clases/06-vulnerabilidades-y-superficie-de-ataque.md) · [Índice del curso](../README.md)
