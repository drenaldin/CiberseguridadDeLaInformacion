# Autoevaluación · Clase 1

**Fundamentos de la ciberseguridad**

---

Diez preguntas para comprobar por tu cuenta si entendiste el material. **No lleva nota y no se entrega.**

Respondé mentalmente o en papel *antes* de desplegar la respuesta. Si la mirás primero, la actividad no sirve de nada: el efecto de aprendizaje está en el intento, no en la lectura.

> [!TIP]
> En el celular, tocá el triángulo ▸ para desplegar cada respuesta.

📖 Si algo no te sale, volvé a [Clase 1 · Fundamentos](../clases/01-fundamentos.md).

---

### 1. La definición de ciberseguridad de la ITU menciona ocho elementos. ¿Cuántos de ellos son tecnología?

<details>
<summary>Ver respuesta</summary>

**Uno solo.** Los otros siete son herramientas de gestión, políticas, directrices, métodos de gestión de riesgos, acciones, formación y prácticas idóneas: es decir, personas, procesos y decisiones.

Es el dato que más suele sorprender de la primera clase, y explica por qué este curso no se trata solo de manejar programas.
</details>

---

### 2. Un compañero te presta su usuario y contraseña del sistema de la escuela para que veas tus notas. ¿Hay algún problema de seguridad?

<details>
<summary>Ver respuesta</summary>

Sí, varios a la vez:

- **Confidencialidad**: accedés a información de otras personas que no te corresponde ver.
- **No repudio**: cualquier acción que hagas quedará atribuida a tu compañero, que ya no podría negar haberla realizado.
- **Trazabilidad**: el registro del sistema deja de reflejar quién hizo realmente cada cosa.

Y además, legalmente, el permiso de un compañero no es el permiso del titular del sistema.
</details>

---

### 3. Diferenciá amenaza de vulnerabilidad usando un ejemplo que no esté en el material.

<details>
<summary>Ver respuesta</summary>

Un ejemplo posible: en una casa, **la amenaza** es que existan personas que roban. **La vulnerabilidad** es que la ventana del fondo no tenga traba. **El riesgo** es la combinación de ambas cosas, y **el impacto** es lo que perderías si entran.

La clave: la amenaza existe independientemente de vos y no la podés eliminar. La vulnerabilidad es tuya y sí la podés corregir.
</details>

---

### 4. ¿Sobre qué factores del riesgo puede actuar realmente una organización?

<details>
<summary>Ver respuesta</summary>

Sobre la **vulnerabilidad** y sobre el **impacto**.

- La vulnerabilidad se reduce actualizando el software, configurando bien los sistemas y formando a las personas.
- El impacto se reduce con copias de seguridad, segmentación de la red y planes de respuesta a incidentes.

La amenaza no se elimina: los atacantes existen y no dependen de la organización.
</details>

---

### 5. Nombrá las seis funciones del NIST Cybersecurity Framework 2.0 y decí cuál se agregó en esta versión.

<details>
<summary>Ver respuesta</summary>

**Govern, Identify, Protect, Detect, Respond, Recover** (Gobernar, Identificar, Proteger, Detectar, Responder, Recuperar).

La que se agregó en la versión 2.0 es **GOVERN**. Su incorporación reconoce que la ciberseguridad es una decisión de dirección y no solo un asunto técnico.
</details>

---

### 6. ¿Qué es el OWASP Top 10 y cuál es el riesgo A01 en la edición 2025?

<details>
<summary>Ver respuesta</summary>

Es un **documento de concienciación** —no una norma certificable— que ordena los diez riesgos más críticos de las aplicaciones web a partir del análisis de datos reales de vulnerabilidades.

El **A01:2025 es Broken Access Control** (control de acceso roto): un usuario puede hacer o ver cosas que no le corresponden. Encabeza la lista desde hace varias ediciones.
</details>

---

### 7. Un ataque deja fuera de línea el sitio de inscripciones de UTU durante tres días. ¿Qué propiedad de la tríada se afectó, y qué pasa con las otras dos?

<details>
<summary>Ver respuesta</summary>

Se afectó la **disponibilidad**.

La confidencialidad y la integridad, en principio, no: nadie vio información que no debía ni la modificó. Es un buen ejemplo de que un incidente grave puede tocar una sola de las tres propiedades.

Ojo con un matiz: si el ataque fue una distracción para encubrir otra intrusión —algo que ocurre—, sí podría haber también un problema de confidencialidad. Por eso los incidentes se investigan y no se dan por cerrados al restablecer el servicio.
</details>

---

### 8. ¿Qué es el CERTuy, en qué organismo funciona y para qué sirve?

<details>
<summary>Ver respuesta</summary>

Es el **Centro Nacional de Respuesta a Incidentes de Seguridad Informática** de Uruguay, y funciona dentro de **Agesic**.

Fue creado por el artículo 73 de la Ley N.º 18.362 y reglamentado por el Decreto N.º 451/009. Es el equipo nacional con el que se coordinan y al que se reportan los incidentes de seguridad.
</details>

---

### 9. Alguien entra a una carpeta compartida de otra institución, mira los archivos, no descarga ni modifica nada. En Uruguay, ¿cometió un delito?

<details>
<summary>Ver respuesta</summary>

**Puede haberlo cometido igualmente.**

El artículo 297 BIS del Código Penal, incorporado por la Ley N.º 20.327, castiga con seis a veinticuatro meses de prisión el acceso a información ajena en soporte digital **sin autorización y sin justa causa**. El tipo penal se configura con el acceso: no exige daño, ni robo, ni perjuicio económico.

«No rompí nada» y «era para aprender» no son defensas.
</details>

---

### 10. ¿Qué distingue a un profesional de la ciberseguridad de un atacante?

<details>
<summary>Ver respuesta</summary>

**La autorización**, no el conocimiento técnico. Ambos pueden saber exactamente lo mismo.

A eso se suman los otros dos principios que rigen la práctica profesional: la **divulgación responsable** (avisar al responsable del sistema y darle tiempo para corregir, en lugar de publicar o explotar) y la **minimización del daño** (acceder a lo mínimo necesario para demostrar el problema, sin copiar datos de terceros).
</details>

---

## Cómo te fue

| Respondiste bien | Qué significa |
|---|---|
| 8 a 10 | Estás pronto para la clase 2. |
| 5 a 7 | Repasá especialmente las secciones 2 y 3, que son la base de todo lo que viene. |
| Menos de 5 | Volvé a leer el material completo y traé tus dudas anotadas a la próxima clase. No es un problema: es la primera clase. |

---

[⬅ Volver a Clase 1](../clases/01-fundamentos.md) · [Índice del curso](../README.md)
