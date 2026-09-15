# Autoevaluación · Clase 6

**Vulnerabilidades y superficie de ataque**

---

Diez preguntas para comprobar por tu cuenta si entendiste el material. **No lleva nota y no se entrega.**

Respondé mentalmente o en papel *antes* de desplegar la respuesta. El efecto de aprendizaje está en el intento, no en la lectura.

> [!TIP]
> En el celular, tocá el triángulo ▸ para desplegar cada respuesta.

📖 Si algo no te sale, volvé a [Clase 6 · Vulnerabilidades y superficie de ataque](../clases/06-vulnerabilidades-y-superficie-de-ataque.md).

---

### 1. ¿Cuál es la diferencia entre una vulnerabilidad y una amenaza?

<details>
<summary>Ver respuesta</summary>

La **vulnerabilidad** es una debilidad **del sistema**: está ahí aunque nadie la mire. La **amenaza** es quien o qué podría aprovecharla.

La cerradura floja es la vulnerabilidad; el que anda robando en el barrio es la amenaza. Y hay dos más: el **exploit** es la ganzúa que abre justo esa cerradura, y el **riesgo** depende de qué guardás adentro.

De las cuatro, la vulnerabilidad es la única sobre la que podés actuar directamente.
</details>

---

### 2. ¿Qué información trae un identificador como `CVE-2021-44228`?

<details>
<summary>Ver respuesta</summary>

Solo dos cosas: **el año** en que se reservó el identificador (2021) y un **número correlativo** sin significado propio (44228).

No dice qué producto afecta, ni qué tan grave es, ni cómo se explota. Para eso hay que ir a la base pública —la **NVD**— donde está la descripción, el puntaje y la lista de versiones afectadas.

Lo importante del CVE es lo que implica: si la vulnerabilidad tiene número público, **la conocen los dos bandos**.
</details>

---

### 3. ¿En qué se diferencian un CVE y un CWE?

<details>
<summary>Ver respuesta</summary>

El **CVE es el caso**: esta falla, en este producto, en estas versiones. El **CWE es la categoría**: el tipo de error.

Miles de CVE distintos pueden ser todos **CWE-89** (inyección SQL), porque miles de programas cometieron el mismo error.

Sirven para cosas distintas: el CVE te dice **si te afecta**; el CWE te dice **qué clase de error fue** y si lo vas a volver a cometer.
</details>

---

### 4. Una vulnerabilidad tiene CVSS 9,8 y otra 6,5. ¿Cuál se arregla primero?

<details>
<summary>Ver respuesta</summary>

**Depende. No se puede decidir solo con el puntaje.**

El CVSS mide la gravedad de la falla **en abstracto**: no sabe si esa máquina está conectada a internet, qué guarda ni si ya hay un exploit circulando.

Si la de 9,8 está en un servidor interno sin salida a internet y sin exploit conocido, y la de 6,5 está en el sitio web público con un exploit publicado hace tres semanas, **se arregla primero la de 6,5**.

El puntaje te lo dan hecho; la prioridad la ponés vos, con el contexto.
</details>

---

### 5. ¿Qué significa que una vulnerabilidad sea de «día cero»?

<details>
<summary>Ver respuesta</summary>

Que quien defiende tuvo **cero días** para prepararse: el atacante la conoce y el fabricante todavía no. **No hay parche** porque nadie lo escribió.

Pero es la excepción, no la regla. La enorme mayoría de los ataques reales usa **fallas viejas, publicadas y con parche disponible**, en sistemas que nadie actualizó.

Un día cero es caro y escaso. Un servidor sin actualizar hace dos años es gratis.
</details>

---

### 6. ¿Cuáles son los tres planos de la superficie de ataque, y cuál se olvida más?

<details>
<summary>Ver respuesta</summary>

**Red** (equipos, puertos abiertos, servicios, wifi), **software** (sistemas operativos, aplicaciones y sus versiones) y **personas** (cuentas, permisos, accesos de terceros).

El que más se olvida es el de **personas**. Una cuenta activa de alguien que ya no está en la organización es una puerta abierta, y ningún escáner de red la va a marcar en rojo.
</details>

---

### 7. ¿Qué diferencia hay entre un puerto cerrado, uno abierto, y uno abierto con una versión vieja?

<details>
<summary>Ver respuesta</summary>

- **Cerrado:** no hay nadie del otro lado. No es un punto de entrada.
- **Abierto:** hay un **servicio escuchando**. Es una puerta con alguien atendiendo.
- **Abierto con versión vieja:** quien atiende tiene una **falla conocida y publicada**, con su CVE y su parche esperando.

Por eso el orden del análisis es inventario → servicios → **versiones** → vulnerabilidades. Sin la versión exacta no hay diagnóstico.
</details>

---

### 8. ¿Por qué un sitio web serio no puede decirte cuál era tu contraseña?

<details>
<summary>Ver respuesta</summary>

Porque **no la guarda**. Guarda su **hash**: el resultado de pasarla por una función de un solo sentido, fácil de calcular para adelante e inviable de revertir.

Cuando iniciás sesión, el sitio calcula el hash de lo que escribiste y lo compara con el que tiene guardado. Nunca necesita la contraseña original.

Corolario práctico: **si un sitio te manda tu contraseña por correo, la está guardando mal.**
</details>

---

### 9. `S3gur!d@` o `caballoverde`: ¿cuál aguanta más un ataque, y por qué?

<details>
<summary>Ver respuesta</summary>

**`caballoverde`**, y por dos motivos distintos.

Por la cuenta: 12 minúsculas dan 26¹² ≈ 9,5 × 10¹⁶ combinaciones, contra 95⁸ ≈ 6,6 × 10¹⁵ de la otra. Catorce veces más. **Cada carácter que agregás multiplica; cada símbolo raro apenas suma.**

Y por algo peor: `S3gur!d@` ni siquiera llega a ese número. Es la palabra «seguridad» con las cuatro sustituciones más previsibles que existen (`e`→`3`, `i`→`!`, `a`→`@`), y los diccionarios de ataque aplican esas reglas solos. Cae en segundos.
</details>

---

### 10. El estándar vigente dice que **no** hay que obligar a cambiar la contraseña cada 90 días. ¿Por qué?

<details>
<summary>Ver respuesta</summary>

Porque en la práctica **empeoraba** las contraseñas. La gente no inventa una nueva cada vez: pasa de `Marzo2026!` a `Junio2026!`. El cambio obligatorio producía contraseñas predecibles, y usuarios que las anotaban en un papel.

La regla del NIST (SP 800-63B-4, julio de 2025) es que se fuerza el cambio **solo si hay indicio de que la contraseña se filtró**. Y en la misma línea: mínimo 15 caracteres si es el único factor, no exigir mezclas de mayúsculas y símbolos, y **sí** comparar contra listas de contraseñas ya filtradas.
</details>

---

## Cómo te fue

| Respondiste bien | Qué significa |
|---|---|
| 8 a 10 | Estás pronto para el triage de la actividad. Sabés leer una ficha de vulnerabilidad y ponerla en contexto. |
| 5 a 7 | Repasá la sección 1 (los cuatro términos) y la 5 (el puntaje no es el riesgo). Son las dos que sostienen todo lo demás. |
| Menos de 5 | Volvé a leer el material. Empezá por la tabla de la sección 1: si no se separan vulnerabilidad, amenaza, exploit y riesgo, el resto no se ordena. |

---

[⬅ Volver a Clase 6](../clases/06-vulnerabilidades-y-superficie-de-ataque.md) · [Índice del curso](../README.md)
