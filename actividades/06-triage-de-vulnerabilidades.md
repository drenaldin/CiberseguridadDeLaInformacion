# Actividad 6 · Triage: seis vulnerabilidades, un técnico y una tarde

**Primera evaluación formativa del curso** · Parte A en grupos de 3 o 4 · Parte B individual

---

## De qué se trata

El Liceo Técnico del Parque tiene **un solo técnico** y **una tarde de cuatro horas** antes de que abran las inscripciones. Le acaban de pasar un informe con **seis problemas de seguridad**. No los puede arreglar todos.

Cada grupo tiene que decidir **en qué orden se atienden y por qué**. No hay un orden único correcto: hay órdenes bien fundamentados y órdenes que no se sostienen.

> [!IMPORTANT]
> Los identificadores CVE de las fichas **son inventados para esta actividad**. Tienen el formato y los datos de los reales, pero no corresponden a vulnerabilidades existentes. Se hizo a propósito: el análisis tiene que salir del razonamiento, no de una búsqueda. Los números de **CWE sí son reales** y se pueden consultar.

---

## El liceo

```
                          INTERNET
                              │
                    ┌─────────┴──────────┐
                    │                    │
            ┌───────▼────────┐   ┌───────▼────────┐
            │ A1 · Servidor  │   │ A5 · Cámara IP │
            │  web de        │   │  del portón    │
            │  inscripciones │   │  puerto 8080   │
            └───────┬────────┘   └────────────────┘
                    │
        ════════════╪═══════════ RED INTERNA ═══════════
                    │                │              │
            ┌───────▼───────┐ ┌──────▼──────┐ ┌─────▼──────┐
            │ A2 · Servidor │ │ A3 · Sala   │ │ A4 · Router│
            │  de archivos  │ │  de informá-│ │  wifi sala │
            │  de secretaría│ │  tica (18)  │ │  de profes │
            └───────────────┘ └─────────────┘ └────────────┘
```

| Equipo | Qué es | Qué guarda o hace | Desde dónde se alcanza |
|---|---|---|---|
| **A1** | Servidor web de inscripciones | Nombre, cédula, dirección y teléfono de 800 estudiantes | Internet, las 24 horas |
| **A2** | Servidor de archivos de secretaría | Calificaciones y actas | Solo red interna cableada |
| **A3** | 18 equipos de la sala de informática | Se usan para dar clase | Red interna, salen a internet |
| **A4** | Router y punto de acceso wifi | Da wifi a profesores y a invitados | Desde el wifi, sin cable |
| **A5** | Cámara IP del portón | La mira el portero desde el celular | Internet, puerto `8080/tcp` |

---

## Las seis fichas

---

### Ficha 1 · Salto de directorio en el servidor web

| | |
|---|---|
| **Dónde** | A1 · Servidor web de inscripciones |
| **Identificador** | `CVE-2025-1041` · tipo **CWE-22** (salto de directorio) |
| **Qué permite** | Leer archivos del servidor que no deberían ser accesibles, incluida la configuración. |
| **CVSS** | **7,5 — alta** · `AV:N/AC:L/PR:N/UI:N/C:H/I:N/A:N` |
| **¿Hay exploit público?** | **Sí**, publicado hace tres semanas y ya se está usando. |
| **Arreglo** | Hay parche. Tarda 10 minutos y reinicia el servicio 2 minutos. |

---

### Ficha 2 · Ejecución remota de código en el servidor de archivos

| | |
|---|---|
| **Dónde** | A2 · Servidor de archivos de secretaría |
| **Identificador** | `CVE-2025-2210` · tipo **CWE-502** (deserialización insegura) |
| **Qué permite** | Ejecutar cualquier programa en el servidor, con los máximos permisos. |
| **CVSS** | **9,8 — crítica** · `AV:N/AC:L/PR:N/UI:N/C:H/I:H/A:H` |
| **¿Hay exploit público?** | No se conoce ninguno. |
| **Arreglo** | Hay parche. Tarda 30 minutos y reinicia el servidor 20 minutos, en horario de secretaría. |

---

### Ficha 3 · El router con la contraseña de fábrica

| | |
|---|---|
| **Dónde** | A4 · Router y punto de acceso wifi |
| **Identificador** | Sin CVE · tipo **CWE-1392** (credenciales por defecto) |
| **Qué permite** | Entrar al panel de administración con `admin` / `admin` y cambiar toda la configuración de la red. |
| **CVSS** | No tiene. **No es una falla del producto, es de cómo quedó instalado.** |
| **¿Hay exploit público?** | No hace falta: la contraseña está en el manual, que está en internet. |
| **Arreglo** | Cambiar la contraseña. 2 minutos, sin reiniciar nada. |

---

### Ficha 4 · La cámara con la contraseña adentro del firmware

| | |
|---|---|
| **Dónde** | A5 · Cámara IP del portón |
| **Identificador** | `CVE-2024-7788` · tipo **CWE-798** (credenciales embebidas en el código) |
| **Qué permite** | Entrar a la cámara con una contraseña que viene grabada en el firmware y **no se puede cambiar**. Desde ahí se ve el video y se llega a la red interna. |
| **CVSS** | **9,8 — crítica** · `AV:N/AC:L/PR:N/UI:N/C:H/I:H/A:H` |
| **¿Hay exploit público?** | **Sí.** Hay botnets que recorren internet buscando este modelo solas. |
| **Arreglo** | **No hay parche.** El fabricante discontinuó el modelo y no va a sacar ninguno. |

---

### Ficha 5 · Los 18 equipos sin contraseña

| | |
|---|---|
| **Dónde** | A3 · Sala de informática |
| **Identificador** | Sin CVE · tipo **CWE-521** (requisitos de contraseña insuficientes) |
| **Qué permite** | El usuario `alumno` no tiene contraseña. Cualquiera que se siente entra, y todo lo que se haga queda a nombre de «alumno». |
| **CVSS** | No tiene. Es configuración, no una falla del producto. |
| **¿Hay exploit público?** | No hace falta ninguno. Alcanza con apretar Enter. |
| **Arreglo** | Poner contraseña en los 18 equipos: 2 horas. Hay que decidir cuál y cómo se les avisa a los grupos. |

---

### Ficha 6 · Scripting entre sitios en el formulario de inscripción

| | |
|---|---|
| **Dónde** | A1 · Servidor web de inscripciones |
| **Identificador** | `CVE-2025-3390` · tipo **CWE-79** (cross-site scripting) |
| **Qué permite** | Meter código en el formulario que después se ejecuta en el navegador de quien lo lee desde secretaría. |
| **CVSS** | **6,1 — media** · `AV:N/AC:L/PR:N/UI:R/C:L/I:L/A:N` |
| **¿Hay exploit público?** | Sí, y es trivial. |
| **Arreglo** | Hay parche. 10 minutos, reinicia el servicio 2 minutos. |

---

## Parte A · El triage (en grupo)

### Consigna

1. Ordená las seis fichas **de 1 a 6** según en qué orden las atendería el técnico.
2. Justificá cada posición con **tres criterios**: gravedad, exposición y costo de arreglarla.
3. Marcá qué queda **sin resolver** al terminar las cuatro horas, y qué se hace mientras tanto con eso.
4. Preparate para defender el orden: otro grupo les va a discutir por lo menos dos posiciones.

### Planilla (se entrega)

| Orden | Ficha | ¿Qué tan grave es la falla? | ¿Qué tan expuesta está? | ¿Cuánto cuesta arreglarla? | Por qué va en ese lugar |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |
| 6 | | | | | |

**Tiempo total disponible: 4 horas.** Sumá los tiempos a medida que ordenás. Cuando se acaben, trazá una línea: lo que queda abajo no se hizo hoy.

### Tres preguntas que hay que contestar por escrito

1. **La ficha 2 tiene 9,8 y la ficha 1 tiene 7,5. ¿Por qué el orden podría ser el inverso?** Usá las columnas de exposición y de exploit público.
2. **La ficha 4 no tiene parche.** Si no se puede arreglar, ¿qué se hace? Proponé una medida concreta que baje el riesgo sin parchear, y decí qué se pierde a cambio.
3. **La ficha 3 no tiene puntaje CVSS y la ficha 5 tampoco.** ¿Significa que son menos importantes? Justificá.

> [!TIP]
> Antes de ordenar, releé la sección 5 de la clase: **el puntaje no es el riesgo**. Un 9,8 encerrado en la red interna, sin exploit conocido y con un reinicio de 20 minutos en horario de secretaría, no es lo mismo que un 7,5 publicado a internet con exploit circulando desde hace tres semanas.

---

## Parte B · Individual

**Extensión: media carilla.** Hacé la ficha de superficie de ataque **de un equipo que uses de verdad** —tu celular, tu computadora, la consola de tu casa—, con los tres planos de la clase:

| Plano | Qué anotar |
|---|---|
| **Red** | ¿A qué redes se conecta? ¿Tiene algún servicio escuchando que vos sepas (compartir pantalla, escritorio remoto, un servidor de juegos)? |
| **Software** | ¿Hace cuánto no lo actualizás? ¿Qué aplicaciones tenés instaladas que **no usás hace más de seis meses**? |
| **Personas** | ¿Quién más tiene acceso? ¿Hay alguna cuenta activa de alguien que ya no lo usa? ¿Compartís la contraseña con alguien? |

Y cerrá con **una sola medida**: la que sacarías primero, y por qué esa antes que las otras.

> [!NOTE]
> No hay que escanear nada ni instalar ninguna herramienta. Es un inventario a ojo, de tu propio equipo. Sobre equipos ajenos no se hace nada: eso está en la sección 9 de la clase y en el acuerdo que firmamos.

---

## Qué se evalúa

| Criterio | Logrado | En proceso | Inicial |
|---|---|---|---|
| **Uso del vocabulario técnico** | Distingue vulnerabilidad, amenaza, exploit y riesgo, y usa cada término donde corresponde. | Usa los términos pero intercambia alguno. | Los usa como sinónimos. |
| **Lectura de las fichas** | Interpreta el CVSS, el tipo de CWE y la exposición, y los usa como evidencia. | Lee el puntaje pero ignora la exposición o el exploit público. | Ordena por el número de CVSS y nada más. |
| **Criterio de priorización** | El orden combina gravedad, exposición y costo, y cada posición está fundada. | El orden es razonable pero la justificación es genérica. | Orden sin justificar o contradictorio con los datos de las fichas. |
| **La ficha sin parche** | Propone una medida compensatoria concreta y reconoce qué se pierde a cambio. | Propone algo genérico («reforzar la seguridad») o no dice el costo. | No la resuelve o afirma que sin parche no hay nada que hacer. |
| **Defensa oral** | Sostiene o corrige el orden con argumentos ante las objeciones del otro grupo. | Responde con dificultad o repite lo escrito. | No logra sostener lo presentado. |
| **Parte B individual** | Completa los tres planos con datos reales y elige una medida bien fundada. | Completa parcialmente o elige sin justificar. | No responde o no usa los conceptos de la clase. |

---

## Formato y tiempos

| Momento | Duración |
|---|---|
| Lectura del mapa y de las seis fichas | 8 min |
| Triage y llenado de la planilla | 22 min |
| Las tres preguntas escritas | 12 min |
| Defensa cruzada entre grupos | 13 min |
| Cierre en común | 5 min |
| **Total** | **60 min** |

**Entrega:** la planilla y las tres preguntas por grupo; la parte B, individual. Formato libre: papel o digital.

---

[⬅ Volver a la clase 6](../clases/06-vulnerabilidades-y-superficie-de-ataque.md) · [Índice del curso](../README.md)
