# Actividad 2 · Clasificación CID y diseño de controles

**Para la clase 3** · Parte A en grupos de 3 o 4 · Parte B individual

---

## De qué se trata

En la clase 1 analizaste un incidente que elegiste vos. Acá el ejercicio se invierte: el caso viene dado, hay que **defender la clasificación frente a los demás grupos**, y hay que proponer controles priorizados —no una lista de buenas intenciones.

---

## Parte A · Análisis en grupo

### Consigna

Cada grupo recibe una **ficha de caso** (están más abajo; el docente asigna una por grupo). Con ella:

1. Aplicá el **método de cinco pasos** de la [sección 7 de la clase 2](../clases/02-triada-cid.md#7-método-de-análisis-en-cinco-pasos).
2. Completá la **matriz CID** que está debajo. Cada casilla lleva justificación: una propiedad marcada sin argumento no cuenta.
3. Proponé **tres controles priorizados**. Para cada uno: qué es, qué propiedad protege y por qué lo ponés en ese lugar de la lista.
4. Preparate para una **defensa oral de 3 minutos**. Van a recibir preguntas de otro grupo.

### Matriz CID (esto es lo que se entrega)

| | ¿Se afectó? | Justificación (una o dos oraciones, con evidencia del caso) |
|---|:---:|---|
| **Confidencialidad** | Sí / No / No se puede determinar | |
| **Integridad** | Sí / No / No se puede determinar | |
| **Disponibilidad** | Sí / No / No se puede determinar | |
| **Otras propiedades** (autenticidad, no repudio, trazabilidad, posesión) | | |

> [!TIP]
> **«No se puede determinar» es una respuesta válida y a veces la más correcta.** Lo que no es válido es marcarla para no comprometerse. Si la usás, explicá **qué información te falta** para decidir.

### Tabla de controles priorizados

| Prioridad | Control | Propiedad que protege | Por qué va en este lugar |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |

> [!IMPORTANT]
> El criterio de priorización no es «cuál es el más avanzado» sino **cuánto riesgo elimina por cada peso y cada hora invertidos**. Es exactamente la lógica de los CIS Controls que vimos en la clase 1. Un control aburrido y barato que evita el 80 % del problema va antes que uno sofisticado.

---

## Fichas de caso

> Todas describen situaciones verosímiles, algunas inspiradas en incidentes reales. **La ficha dice lo que se sabe: si algo no está, es porque no se sabe.**

---

### Ficha A · El liceo sin notas

El sistema de gestión de calificaciones de un centro educativo aparece un lunes con un mensaje en pantalla: todos los archivos del servidor están cifrados y se exige un pago en criptomonedas para recuperarlos. La copia de seguridad más reciente es de hace once días y estaba en un disco conectado permanentemente al mismo servidor: también quedó cifrada. Una semana después, un grupo publica en un foro un archivo con nombres, cédulas y domicilios de 1.200 estudiantes, afirmando que lo obtuvo de ese centro. La dirección debe cerrar el período de calificaciones en cinco días.

---

### Ficha B · La transferencia que nadie autorizó

Una cooperativa detecta, en la conciliación bancaria mensual, cuatro transferencias a cuentas desconocidas por un total equivalente a dos meses de sueldos, hechas a lo largo de las últimas seis semanas. El sistema contable las muestra registradas con el usuario de la tesorera, que asegura no haberlas hecho. Ese usuario es compartido por tres personas del área desde hace años, «porque es más práctico». El sistema no guarda registro de desde qué equipo se inició cada sesión. Los servicios funcionaron con normalidad todo el período.

---

### Ficha C · La actualización del viernes

Una empresa de logística instala el viernes a la tarde una actualización automática de su antivirus corporativo. El lunes, 140 de sus 160 computadoras no arrancan: quedan en un bucle de reinicio. El fabricante confirma en pocas horas que la actualización tenía un error y publica una corrección, pero cada equipo debe repararse a mano, uno por uno. La empresa opera tres días en papel. No hubo acceso no autorizado, ni datos robados, ni datos modificados.

---

### Ficha D · La cámara de la sala de servidores

Un centro de estudios descubre que una cámara de seguridad conectada a la red interna quedó con el usuario y la contraseña de fábrica, y que su interfaz web era accesible desde internet. Se comprueba que hubo accesos desde direcciones IP del exterior durante al menos cuatro meses. La cámara apunta a la puerta de la sala de servidores y a un pizarrón donde el equipo técnico anota, entre otras cosas, las tareas pendientes. La cámara está en la misma red que los servidores administrativos.

---

### Ficha E · El padrón que se vendía

Aparece a la venta en un foro un archivo con datos de 600.000 personas: nombre completo, documento, teléfono, correo electrónico y una fotografía de identificación. El organismo titular de esos datos verifica que el archivo es auténtico y que corresponde a un volcado de su base. Sus sistemas nunca dejaron de funcionar y no hay indicios de que se hayan modificado registros. El organismo no puede determinar cuándo se produjo la extracción ni por qué vía.

---

### Ficha F · La planilla que engordó sola

En un club, el tesorero detecta que la planilla de cuotas sociales muestra 40 socios menos que el listado de la secretaría, y que varios importes no coinciden con lo que figura en los recibos en papel. La planilla está en un servicio en la nube, con enlace de edición compartido por WhatsApp «al grupo de la comisión», que tiene 22 integrantes y del que nadie recuerda quién sigue estando. El historial de versiones existe, pero muestra ediciones de una cuenta de correo que ningún integrante actual reconoce. El archivo siempre estuvo accesible y nadie lo copió fuera del club, hasta donde se sabe.

---

## Parte B · Individual

**Extensión: media carilla.** Elegí **un sistema informático de tu liceo** (gestión de calificaciones, red wifi, sala de informática, cartelera digital, lo que sea) y respondé:

1. **Ordená C, I y D según su prioridad para ese sistema**, y justificá el orden en tres o cuatro líneas. No hay orden correcto único: hay orden fundamentado.
2. Describí **una tensión concreta** entre dos de esas propiedades en ese sistema: un control que mejoraría una y empeoraría otra.
3. Proponé **un RTO y un RPO** para ese sistema, con una línea de justificación cada uno. (Definiciones en la [sección 4.2 de la clase 2](../clases/02-triada-cid.md#42-dos-siglas-que-hay-que-saber).)

---

## Qué se evalúa

> [!IMPORTANT]
> Se evalúa el **razonamiento**, no la coincidencia con una respuesta modelo. Dos grupos pueden clasificar distinto el mismo caso y estar los dos bien calificados si ambos justifican con la evidencia de la ficha.

| Criterio | Logrado | En proceso | Inicial |
|---|---|---|---|
| **Clasificación CID** | Marca las propiedades correctas y justifica cada una con evidencia concreta de la ficha, incluyendo las no afectadas. | Clasifica bien pero justifica de forma general, sin apoyarse en los datos del caso. | Clasificación errónea o sin justificar. |
| **Manejo de la incertidumbre** | Distingue lo que la ficha permite afirmar de lo que no, y dice qué información falta. | Afirma cosas que la ficha no sostiene, o marca «no se puede determinar» sin explicar. | Inventa hechos que no están en el caso. |
| **Controles y priorización** | Tres controles pertinentes, vinculados a una propiedad, con criterio de priorización explícito y razonable. | Controles pertinentes pero genéricos, o sin justificar el orden. | Lista de medidas sueltas o no aplicables al caso. |
| **Defensa oral** | Responde las preguntas sosteniendo o corrigiendo su análisis con argumentos. Reconocer un error bien fundamentado suma. | Responde con dificultad o repite lo escrito sin ampliar. | No logra sostener lo que presentó. |
| **Parte B individual** | Prioriza con criterio propio, identifica una tensión real y propone RTO/RPO coherentes con esa prioridad. | Prioriza sin justificar, o confunde RTO con RPO. | No responde o responde sin usar los conceptos del material. |

---

## Formato y tiempos

| Momento | Duración sugerida |
|---|---|
| Lectura de la ficha y análisis en grupo | 25 min |
| Redacción de la matriz y los controles | 15 min |
| Defensas orales (3 min por grupo + 2 de preguntas) | según cantidad de grupos |
| Puesta en común y cierre | 10 min |

- **Parte A:** una carilla por grupo, con los nombres de todos los integrantes. Puede ser manuscrita.
- **Parte B:** media carilla individual, se entrega la clase siguiente.

> [!WARNING]
> Igual que en la actividad 1: la actividad es de **lectura y análisis**. No corresponde —ni hace falta— probar nada sobre ningún sistema real, ni del liceo ni de ningún otro. Ver el [Acuerdo de uso responsable](../CODE_OF_CONDUCT.md).

---

[⬅ Volver a Clase 2](../clases/02-triada-cid.md) · [Índice del curso](../README.md)
