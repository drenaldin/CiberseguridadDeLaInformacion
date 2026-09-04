# Actividad 5 · Tres amenazas, tres defensas

**Para la clase 6** · Parte A en grupos de 3 o 4 · Parte B individual

---

## De qué se trata

Cada grupo recibe un **caso** de una de las tres amenazas de la clase —ingeniería social, DoS/DDoS o APT—, lo analiza y propone la defensa. Después se ponen los tres tipos en común, para ver por qué cada uno se defiende distinto.

---

## Parte A · Análisis en grupo

### Consigna

1. El docente asigna una **ficha de caso** por grupo.
2. Completá la **planilla de análisis** de más abajo. Cada casilla lleva justificación.
3. Proponé **tres defensas priorizadas**, y para cada una decí qué principio de las clases 2 y 3 la sostiene.
4. Preparate para una **defensa oral de 3 minutos**: otro grupo les va a preguntar.

### Planilla de análisis (se entrega)

| Qué se analiza | Respuesta |
|---|---|
| ¿Qué amenaza es? (ingeniería social, DoS/DDoS, APT) | |
| ¿Cuál fue el vector de entrada, si lo hubo? | |
| ¿Qué propiedad de la tríada se rompió: C, I, D? Justificá. | |
| ¿Sobre quién recae el daño, y de qué tipo es? | |
| Tres defensas priorizadas, cada una con su principio | |

> [!TIP]
> Ojo con la técnica: si es un DDoS, la propiedad casi siempre es **disponibilidad**, y el «vector» no es un correo, es el volumen de tráfico. No fuerces el análisis del phishing sobre un caso que no lo es.

---

## Fichas de caso

---

### Ficha A · La llamada del falso soporte

Un funcionario de secretaría recibe una llamada: «Hola, soy del soporte informático de la ANEP. Estamos migrando el sistema de gestión y necesito tu usuario y contraseña para no perder tus datos.» La voz es amable, sabe el nombre del funcionario y menciona el nombre real del sistema. El funcionario dicta su contraseña. Dos días después aparecen calificaciones modificadas y nadie sabe quién las tocó, porque se usó una cuenta legítima.

---

### Ficha B · El día que no se pudo inscribir nadie

El día de apertura de inscripciones en línea, el sitio del centro empieza a responder cada vez más lento hasta que deja de cargar por completo. El equipo técnico ve que llegan millones de pedidos por minuto desde miles de direcciones distintas de todo el mundo, muchas de ellas de cámaras y routers hogareños. No hay señales de que se hayan robado ni modificado datos. El sitio estuvo caído seis horas, justo las de mayor demanda.

---

### Ficha C · Los que estaban adentro desde marzo

En noviembre, una auditoría de rutina encuentra en un servidor un programa que no debería estar ahí. Al investigar, se descubre que alguien tenía acceso a la red **desde marzo**: entró por una actualización troyanizada de un software de gestión que usa el centro, se movió despacio, copió información de a poco y borró sus huellas. Los sistemas funcionaron con normalidad todo el año y nadie notó nada raro. No pidieron rescate ni dejaron ningún mensaje.

---

### Ficha D · El pendrive en el patio

Aparecen tres pendrives en el patio y en la cantina, con una etiqueta escrita a mano: «Parcial de Matemática — respuestas». Un estudiante conecta uno a una máquina de la sala de informática por curiosidad. El pendrive instala en silencio un programa que registra todo lo que se teclea en esa máquina durante las semanas siguientes, incluidas las contraseñas de quienes la usan.

---

## Parte B · Individual

**Extensión: media carilla.** Respondé la pregunta que quedó planteada en la clase:

1. De las tres amenazas (ingeniería social, DoS/DDoS, APT), elegí **la que te parece más difícil de defender en tu liceo** y justificá en tres o cuatro líneas.
2. Nombrá **una defensa concreta y realista** para esa amenaza en el liceo —algo que de verdad se pueda hacer con los recursos que hay— y decí qué principio la sostiene.

---

## Qué se evalúa

| Criterio | Logrado | En proceso | Inicial |
|---|---|---|---|
| **Identificación de la amenaza** | Nombra bien la amenaza y explica qué la distingue de las otras dos. | La identifica pero la confunde en algún punto con otra. | Identificación errónea. |
| **Clasificación en la tríada** | Marca la propiedad correcta y la justifica con la evidencia del caso. | Clasifica bien pero sin apoyarse en el caso. | Clasificación errónea o sin justificar. |
| **Defensas y principios** | Tres defensas pertinentes, cada una vinculada a un principio de las clases 2 o 3. | Defensas correctas pero genéricas, o sin vincular al principio. | Lista de medidas sueltas o no aplicables. |
| **Defensa oral** | Sostiene o corrige su análisis con argumentos ante las preguntas. | Responde con dificultad o repite lo escrito. | No logra sostener lo presentado. |
| **Parte B individual** | Elige con criterio propio y propone una defensa realista y bien fundada. | Elige sin justificar del todo, o propone algo poco realista. | No responde o no usa los conceptos del material. |

---

## Formato y tiempos

| Momento | Duración sugerida |
|---|---|
| Lectura de la ficha y análisis en grupo | 25 min |
| Preparación de la defensa oral | 10 min |
| Puesta en común y preguntas entre grupos | 20 min |

**Entrega:** la planilla por grupo y la parte B individual. Formato libre: papel o digital.

---

[⬅ Volver a la clase 5](../clases/05-amenazas-ingenieria-dos-apt.md) · [Índice del curso](../README.md)
