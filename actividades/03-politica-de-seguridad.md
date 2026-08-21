# Actividad 3 · Redactar una política de seguridad

**Para la clase 4** · Parte A en grupos de 3 o 4 · Parte B individual

---

## De qué se trata

En la actividad 2 clasificaste un incidente que ya había ocurrido. Acá el trabajo es al revés: **escribir el documento que lo habría evitado**.

Cada grupo redacta una política de seguridad de **una carilla** para un sistema real del liceo, y después **audita la de otro grupo**. La auditoría cruzada no es un trámite: casi todo lo que se aprende en esta actividad aparece cuando leés con ojo crítico lo que escribió otro.

---

## Parte A · Redacción en grupo

### Consigna

1. El docente asigna a cada grupo uno de los **escenarios** de más abajo.
2. Redactá una política de **una carilla** que contenga las **siete partes obligatorias** (sección 8 de la clase 3): objetivo, alcance, roles y responsabilidades, reglas, consecuencias del incumplimiento, vigencia y revisión, aprobación.
3. Las reglas son **cinco como mínimo**, y cada una tiene que poder verificarse. Sujeto, acción, plazo. Nada de «se debe actuar con responsabilidad».
4. Debajo de la política, agregá una tabla corta —la llamamos **tabla de fundamentos**— donde para **tres** de tus reglas indiques qué principio de arquitectura la sostiene.

### Tabla de fundamentos (se entrega junto con la política)

| Regla n.º | Principio que la sostiene | En una línea: por qué |
|---|---|---|
| | | |
| | | |
| | | |

> Los principios disponibles son los ocho de Saltzer y Schroeder, más defensa en profundidad, reducción de la superficie de ataque y confianza cero. Están en las [secciones 2 a 5 de la clase 3](../clases/03-arquitectura-y-politicas.md#2-los-ocho-principios-de-saltzer-y-schroeder).

> [!TIP]
> **Trampa frecuente:** escribir cinco reglas que son todas del mismo tipo (cinco sobre contraseñas). Una política que cubre una sola capa deja las otras cinco al descubierto. Revisá tu texto con las seis capas de la defensa en profundidad al lado.

---

## Escenarios

> Todos existen, con distintos nombres, en cualquier centro educativo del país.

---

### Escenario A · La sala de informática

Veinticinco máquinas de uso compartido por todos los grupos del turno. Hoy: todas tienen el mismo usuario `alumno` sin contraseña, cualquiera puede instalar programas, los archivos quedan en el escritorio de una clase a la otra y hay una impresora conectada a la misma red. Tres veces en el año apareció software que nadie sabe quién instaló.

---

### Escenario B · El sistema de calificaciones

Cada docente tiene cuenta propia, pero la contraseña de la cuenta de administración la saben cuatro personas «por si alguien no está». Se accede desde cualquier equipo, incluido el celular personal. No hay segundo factor. El sistema guarda registro de quién modificó cada nota, pero nadie lo mira nunca.

---

### Escenario C · El wifi del liceo

Hay una sola red, con una contraseña que se sabe de memoria medio barrio porque no se cambia desde hace tres años. Se conectan a ella los celulares de estudiantes, las máquinas de la sala, las computadoras de secretaría y una impresora. La red no está dividida.

---

### Escenario D · Los datos de los estudiantes

Las listas con nombres, cédulas, domicilios y teléfonos circulan por WhatsApp entre docentes y adscriptos, y viven en las carpetas de descargas de una docena de celulares personales. Cuando alguien deja el liceo, nadie borra nada porque nadie sabe qué tiene cada uno.

---

### Escenario E · Los respaldos

El servidor del liceo se respalda a un disco externo que está permanentemente conectado al mismo servidor. Nunca se probó una restauración. El respaldo lo hace un funcionario que se jubila en noviembre y es el único que sabe cómo se hace.

---

### Escenario F · Las cuentas de quien ya no está

Cuando un docente se va, su cuenta queda activa «por las dudas». Hay cuentas de gente que no trabaja en el liceo desde 2021, algunas con permisos de administración. Tampoco hay un procedimiento de alta: las cuentas nuevas se crean por WhatsApp copiando los permisos de otra persona.

---

## Auditoría cruzada

Cuando todos los grupos terminan, **se intercambian las políticas**. Cada grupo audita la de otro con esta planilla y devuelve el resultado por escrito, en no más de media carilla.

| Qué se revisa | Sí / No | Observación concreta |
|---|:---:|---|
| ¿Están las siete partes? | | ¿Cuál falta? |
| ¿Las reglas se pueden verificar? | | Señalá **una** regla que no se pueda verificar y reescribila. |
| ¿Cubre más de una capa de defensa? | | ¿Qué capa quedó afuera? |
| ¿Alguna regla es imposible de cumplir en este liceo? | | ¿Cuál y por qué? |
| ¿Cae en alguno de los seis antipatrones? | | ¿En cuál? |
| ¿Qué le falta para pasar del nivel 2 al 3 de madurez? | | Una sola cosa, la más importante. |

> [!IMPORTANT]
> **Auditar no es buscar errores para ganar.** Se evalúa la calidad de la observación, no la cantidad. Una crítica precisa y bien fundada vale más que ocho comentarios genéricos, y una auditoría que dice «está todo bien» sobre un texto que tiene problemas se califica peor que la que encuentra uno solo y lo explica.

---

## Parte B · Individual

**Extensión: media carilla.**

1. Elegí **una regla informática que ya funcione de hecho en tu liceo**, aunque nunca se haya escrito (cómo se pide la clave del wifi, quién puede entrar a la sala, qué se hace cuando una máquina falla).
2. Ubicala en la **pirámide normativa**: ¿es política, norma, procedimiento o guía? Justificá en dos líneas.
3. Escribila como corresponde a ese nivel, en una o dos oraciones, con el verbo que le toca.
4. Asignale un **nivel de madurez del 0 al 4** a esa práctica y justificá el número. Después decí qué haría falta para subir un solo nivel.

---

## Qué se evalúa

> [!IMPORTANT]
> Se evalúa el **criterio**, no la extensión. Una política de una carilla bien pensada vale más que tres carillas de frases hechas. Copiar una política de internet y cambiarle el nombre se califica como Inicial, porque es exactamente el antipatrón n.º 2 del material.

| Criterio | Logrado | En proceso | Inicial |
|---|---|---|---|
| **Estructura de la política** | Están las siete partes, cada una cumple su función y el documento se entiende sin explicación oral. | Faltan una o dos partes, o alguna está puesta de relleno. | Faltan tres o más partes; el texto es una lista de recomendaciones. |
| **Calidad de las reglas** | Cinco o más reglas verificables, con sujeto, acción y plazo, y cubren más de una capa. | Reglas pertinentes pero generales, difíciles de verificar, o todas de la misma capa. | Reglas vagas del tipo «usar con responsabilidad», o copiadas sin adaptar. |
| **Fundamento técnico** | La tabla vincula correctamente tres reglas con su principio y explica el porqué. | Vincula principios de manera superficial o con un error de asignación. | No vincula, o nombra principios que no corresponden. |
| **Adecuación al escenario** | La política habla del liceo real: sus equipos, su gente, sus recursos. | Es genérica pero aplicable. | Menciona sistemas o áreas que no existen en el centro. |
| **Auditoría cruzada** | Observaciones precisas, fundadas en el material, con una propuesta de mejora concreta. | Observaciones correctas pero generales, sin propuesta. | Auditoría de trámite: «está bien» o comentarios sin fundamento. |
| **Parte B individual** | Ubica bien el nivel, usa el verbo correcto y justifica el nivel de madurez con evidencia de lo que pasa en el liceo. | Ubica el nivel sin justificar, o confunde norma con procedimiento. | No responde o no usa los conceptos del material. |

---

## Formato y tiempos

| Momento | Duración sugerida |
|---|---|
| Lectura del escenario y borrador de reglas | 20 min |
| Redacción de la política y la tabla de fundamentos | 25 min |
| Intercambio y auditoría cruzada | 20 min |
| Puesta en común: dos hallazgos por grupo | 15 min |

**Entrega:** un archivo por grupo (política + tabla de fundamentos + auditoría recibida) y la parte B individual. Formato libre: papel o digital.

---

[⬅ Volver a la clase 3](../clases/03-arquitectura-y-politicas.md) · [Índice del curso](../README.md)
