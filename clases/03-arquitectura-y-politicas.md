# Clase 3 · Principios de arquitectura de seguridad y políticas

> De *qué* proteger a *cómo* se diseña la protección: ocho principios de 1975 que siguen vigentes, y el documento que los vuelve obligatorios

**Contenidos del programa:** 1.2 Principios de la arquitectura de seguridad · 1.6 Políticas de seguridad
**Competencia:** CET1
**Tiempo de lectura:** unos 25 minutos

---

📥 **Presentación de la clase:** [Clase 3 · Arquitectura y políticas](../presentaciones/Clase3-Arquitectura-y-Politicas.pptx) — 29 diapositivas en `.pptx`. Se descarga con el botón **Download** que aparece arriba a la derecha al abrir el enlace. Sirve para repasar aunque no hayas tomado apuntes.

---

> [!TIP]
> **Cómo usar este material.** Las clases 1 y 2 respondieron *qué* se protege. Esta responde *cómo se decide* la protección, y tiene dos mitades que parecen distintas y no lo son: los **principios de diseño** (lo que hace el técnico) y las **políticas** (lo que decide la organización). Un principio sin política es una buena idea que no se aplica; una política sin principios es un archivo PDF que nadie cumple.

## Contenido

1. [Del qué al cómo](#1-del-qué-al-cómo)
2. [Los ocho principios de Saltzer y Schroeder](#2-los-ocho-principios-de-saltzer-y-schroeder)
3. [Defensa en profundidad](#3-defensa-en-profundidad)
4. [Superficie de ataque: menos es más seguro](#4-superficie-de-ataque-menos-es-más-seguro)
5. [Confianza cero: se murió el perímetro](#5-confianza-cero-se-murió-el-perímetro)
6. [De los principios a los documentos: la pirámide normativa](#6-de-los-principios-a-los-documentos-la-pirámide-normativa)
7. [El marco uruguayo: qué obliga la norma](#7-el-marco-uruguayo-qué-obliga-la-norma)
8. [Anatomía de una política que sirve](#8-anatomía-de-una-política-que-sirve)
9. [Seis maneras de tener una política inútil](#9-seis-maneras-de-tener-una-política-inútil)
10. [Para la próxima clase](#10-para-la-próxima-clase)

---

## 1. Del qué al cómo

En la clase 2 aprendiste a mirar un incidente y decir qué propiedad se rompió. Es una habilidad de **diagnóstico**, y llega siempre tarde: el incidente ya pasó.

Esta clase es la otra mitad del oficio. La pregunta cambia:

| Clase 2 preguntaba | Clase 3 pregunta |
|---|---|
| ¿Qué se rompió acá? | ¿Cómo se diseña un sistema para que eso sea difícil de romper? |
| ¿Qué propiedad se afectó? | ¿Qué regla, escrita y firmada, obliga a sostener esa propiedad? |

La respuesta tiene dos niveles. Los **principios de arquitectura de seguridad** son criterios de diseño: se aplican al construir un sistema, y son casi los mismos desde hace cincuenta años. Las **políticas de seguridad** son las decisiones de la organización puestas por escrito y aprobadas por quien manda: convierten un criterio técnico en una obligación.

> [!NOTE]
> **Arquitectura** acá no significa dibujar servidores. Significa la estructura de decisiones que determina cómo se protege un sistema: qué se separa de qué, quién puede hacer qué, qué pasa cuando algo falla. Un sistema puede tener todos los productos de seguridad del mercado y una arquitectura pésima.

---

## 2. Los ocho principios de Saltzer y Schroeder

En 1975, Jerome Saltzer y Michael Schroeder publicaron en el MIT un artículo llamado *The Protection of Information in Computer Systems*. Enumeraron ocho principios de diseño. Se escribieron cuando una computadora ocupaba una sala y no existía internet comercial, y **siguen siendo la base de todo lo que se enseña hoy**. Vale la pena preguntarse por qué: porque no hablan de tecnología, hablan de cómo se comporta la gente y de cómo fallan los sistemas complejos.

| # | Principio | Qué dice, en criollo | Ejemplo en el liceo |
|---|---|---|---|
| 1 | **Economía de mecanismo** | El diseño de seguridad tiene que ser lo más simple posible. Lo complejo no se puede revisar, y lo que no se puede revisar tiene errores escondidos. | Una regla clara de quién entra a la sala de informática vale más que un reglamento de doce páginas con excepciones. |
| 2 | **Valores predeterminados a prueba de fallos** (*fail-safe defaults*) | Por omisión, **denegar**. El acceso se concede explícitamente, no se quita explícitamente. | Una carpeta compartida nueva arranca sin permisos para nadie y se van agregando, no arranca abierta a todo el liceo. |
| 3 | **Mediación completa** | **Cada** acceso se verifica, todas las veces, no solo la primera. | Que el sistema de calificaciones revise los permisos en cada pantalla, no solo al iniciar sesión. |
| 4 | **Diseño abierto** | La seguridad no puede depender de que el diseño sea secreto. Solo la clave es secreta. | Si tu wifi está «seguro» porque escondiste el nombre de la red, no está seguro. |
| 5 | **Separación de privilegios** | Que hagan falta **dos condiciones** independientes para una acción crítica, no una sola. | Para borrar el acta de calificaciones del grupo hacen falta dos personas, no una contraseña. |
| 6 | **Mínimo privilegio** | Cada cuenta tiene los permisos justos para su tarea, y ni uno más. Ya lo vimos en la clase 2. | El adscripto ve las inasistencias de su grupo; no puede exportar la base entera. |
| 7 | **Mínimo mecanismo común** | Cuanto menos se comparta entre usuarios distintos, menos caminos hay para pasar de uno a otro. | Que la red de las máquinas de alumnos no sea la misma red que la de administración. |
| 8 | **Aceptabilidad psicológica** | Si el control es insoportable, la gente lo esquiva y el sistema queda peor que antes. | Contraseñas de veinte caracteres cambiadas cada treinta días: terminan anotadas en un papel abajo del teclado. |

> [!IMPORTANT]
> **El principio 8 es el que más se olvida y el que más incidentes causa.** Un control que la gente no puede cumplir no es un control estricto: es un control que no existe, más la ilusión de que existe. Cuando veas una regla que todo el mundo incumple, el problema casi nunca es la gente.

### Los principios en los casos de la clase 2

Ningún caso de la clase pasada fue mala suerte. En cada uno se puede nombrar el principio que faltaba:

| Caso | Principio ausente | Cómo se habría visto |
|---|---|---|
| Banco de Bangladesh (2016) | **Separación de privilegios** | Doble autorización real para órdenes de alto monto: un juego de credenciales robadas no habría alcanzado. |
| Costa Rica (2022) | **Mínimo mecanismo común** | Redes segmentadas: el atacante entra a una institución y no se mueve hacia treinta. |
| CrowdStrike (2024) | **Economía de mecanismo** y despliegue gradual | Una actualización probada por anillos, primero en pocos equipos, en vez de a 8,5 millones a la vez. |
| Filtraciones al Estado (2025) | **Mínimo privilegio** | Ninguna cuenta con permiso de exportar millones de registros de una sola vez, y alerta si alguien lo intenta. |
| BHU (2025) | **Valores predeterminados a prueba de fallos** | Accesos concedidos uno a uno y revisados; copias de seguridad desconectadas por diseño, no por suerte. |

---

## 3. Defensa en profundidad

Es el principio moderno más citado y el más malentendido.

> **Defensa en profundidad:** superponer varias capas de control independientes, de modo que el fallo de una no comprometa todo el sistema.

La idea viene de la fortificación militar: foso, muralla, patio, torre. Ninguna capa pretende ser perfecta. Lo que se busca es que un atacante tenga que vencerlas **todas**, y que cada una le cueste tiempo y ruido.

En un sistema informático las capas típicas son seis:

| Capa | Ejemplos de control |
|---|---|
| **Física** | Puerta con llave de la sala de servidores, cámara, control de visitas. |
| **Red** | Segmentación, cortafuegos, wifi separado por rol, VPN. |
| **Equipo** | Sistema operativo actualizado, disco cifrado, antivirus, bloqueo de pantalla. |
| **Aplicación** | Validación de entradas, control de acceso por rol, registro de auditoría. |
| **Dato** | Cifrado en reposo, clasificación, respaldos, minimización. |
| **Persona** | Formación, procedimientos, cultura de reportar sin miedo a la sanción. |

> [!WARNING]
> **Defensa en profundidad no es comprar cinco productos de seguridad.** Cinco antivirus del mismo tipo son una sola capa repetida. Y —lección de CrowdStrike— cada capa nueva aporta protección **y** una nueva forma de fallar. La pregunta correcta no es «¿cuántos controles tengo?» sino «¿de qué manera distinta falla cada uno?».

**El ejemplo del liceo.** Alguien consigue la contraseña de un docente por phishing. ¿Cuántas capas quedan?

- Si el sistema pide un segundo factor: la contraseña sola no alcanza *(capa equipo/aplicación)*.
- Si esa cuenta solo puede cargar notas de sus grupos: el daño queda acotado *(mínimo privilegio)*.
- Si hay registro de auditoría: se detecta qué se tocó *(trazabilidad)*.
- Si hay respaldo diario: se restaura *(dato)*.
- Si el docente sabe que tiene que avisar y no lo van a retar por eso: se detecta en horas y no en meses *(persona)*.

Un liceo con una sola capa pierde todo con una contraseña. El mismo liceo con cinco pierde una tarde de trabajo.

---

## 4. Superficie de ataque: menos es más seguro

> **Superficie de ataque:** el conjunto de puntos por los que alguien podría intentar entrar a un sistema.

Cada cuenta, cada puerto abierto, cada programa instalado, cada formulario, cada dispositivo conectado y cada persona con acceso **suma** superficie. Y hay un dato incómodo: la superficie crece sola. Nadie decide un lunes «voy a exponer más el sistema»; simplemente se instalan cosas, se crean usuarios de prueba, se comparte un enlace «por ahora», se conecta una cámara.

**Reducirla es la medida más barata que existe**, porque casi siempre consiste en sacar, no en comprar:

| Acción | Qué elimina |
|---|---|
| Desinstalar lo que no se usa | Vulnerabilidades de software que ni sabías que tenías. |
| Cerrar puertos y servicios que no hacen falta | Puertas abiertas sin portero. |
| Dar de baja cuentas de gente que ya no está | El acceso legítimo de alguien que ya no debería tenerlo. |
| Quitar permisos que sobran | El alcance del daño cuando una cuenta se compromete. |
| Cambiar credenciales de fábrica | La entrada más usada del mundo, y la más tonta. |
| Guardar menos datos | Lo que no tenés no se puede filtrar. |

> [!CAUTION]
> **El caso de la cámara.** Una cámara de vigilancia con la contraseña de fábrica, conectada a la misma red que los servidores administrativos y accesible desde internet, no es «un problema de la cámara». Es un equipo cualquiera de la red interna, en manos de quien quiera, veinticuatro horas por día. La superficie de ataque no distingue entre dispositivos importantes y dispositivos que parecen tontos.

Este es el contenido que retomamos en la clase 6, donde se hace el análisis completo de superficie de ataque de un sistema del liceo. Guardá lo de acá: es el marco conceptual de esa evaluación.

---

## 5. Confianza cero: se murió el perímetro

Durante décadas la seguridad se diseñó como un castillo: una muralla fuerte y, adentro, todo el mundo confiando en todo el mundo. Ese modelo se llama **seguridad perimetral**, y hoy no alcanza por tres razones concretas:

1. **Ya no hay adentro.** Servicios en la nube, celulares personales, gente trabajando desde casa. El «adentro» está repartido por todos lados.
2. **El atacante entra igual.** Con credenciales robadas por phishing, el atacante *es* alguien de adentro. La muralla no lo detiene: le abrió la puerta.
3. **Adentro se movía libre.** Bangladesh y Costa Rica no fueron una entrada espectacular: fueron **semanas de movimiento lateral** sin que nada volviera a preguntar quién era.

El modelo que responde a eso se llama **confianza cero** (*zero trust*), y se resume en una frase: **nunca confiar, siempre verificar**. Tres reglas prácticas:

| Regla | Qué significa |
|---|---|
| **Verificación explícita** | Cada pedido de acceso se evalúa con lo que se sabe en ese momento: identidad, dispositivo, ubicación, comportamiento. Estar dentro de la red no es una credencial. |
| **Mínimo privilegio, siempre** | Permisos justos, temporales cuando se puede, revisados periódicamente. |
| **Asumir la brecha** | Diseñar como si el atacante **ya estuviera adentro**: segmentar, cifrar, registrar todo, limitar el radio de daño. |

> [!NOTE]
> Fijate que confianza cero no inventó nada: es Saltzer y Schroeder aplicados a un mundo sin perímetro. Mediación completa (regla 1), mínimo privilegio (regla 2) y mínimo mecanismo común (regla 3). Cuando un proveedor te venda «zero trust» como producto, acordate de esto: es una arquitectura, no una caja que se compra.

---

## 6. De los principios a los documentos: la pirámide normativa

Un principio que nadie escribió depende de que el técnico de turno se acuerde. Cuando ese técnico se va, se va el criterio con él. Por eso las organizaciones escriben lo que decidieron, en cuatro niveles:

```
        POLÍTICA          qué se quiere y por qué       la aprueba la dirección
       ───────────
        NORMA             qué hay que cumplir           obligatoria y medible
      ─────────────
       PROCEDIMIENTO      cómo se hace, paso a paso     lo escribe quien opera
    ───────────────────
        GUÍA              recomendaciones               no obligatoria
```

| Nivel | Responde | Quién lo aprueba | Cada cuánto cambia | Ejemplo |
|---|---|---|---|---|
| **Política** | Qué queremos proteger y por qué | La máxima autoridad (dirección, consejo) | Poco: años | «La información de los estudiantes se protege según su clasificación y solo accede quien lo necesita para su tarea.» |
| **Norma** o estándar | Qué hay que cumplir, con números | El área responsable de seguridad | Cada tanto: meses o años | «Toda cuenta con acceso a calificaciones usa segundo factor de autenticación.» |
| **Procedimiento** | Cómo se hace, paso a paso | Quien opera el sistema | Seguido | «Alta de usuario docente: 1) recibir el formulario firmado, 2) crear la cuenta en el grupo Docentes…» |
| **Guía** | Qué conviene | Cualquiera con criterio | Cuando se quiera | «Recomendaciones para armar una frase de contraseña.» |

> [!TIP]
> **Cómo distinguirlos en un examen y en la vida.** Mirá el verbo. La política dice *«se protege», «se garantiza»*. La norma dice **«debe»**. El procedimiento dice *«hacé clic en…»*. La guía dice *«se recomienda»*. Si un documento mezcla los cuatro tonos, no es ninguno de los cuatro y no lo va a cumplir nadie.

---

## 7. El marco uruguayo: qué obliga la norma

Acá se termina la teoría. En Uruguay, tener una política de seguridad de la información **no es una buena práctica opcional para los organismos del Estado: es una obligación legal, y lo es desde hace más de quince años.**

### Lo que obliga la normativa

| Norma | Año | Qué establece |
|---|---|---|
| **Decreto 452/009** | 2009 | Obliga a las unidades ejecutoras de la Administración Central a **adoptar una Política de Seguridad de la Información**, con el objetivo de implantar un sistema de gestión de seguridad. |
| **Decreto 66/025** | 2025 | Endurece el régimen tras los incidentes de los últimos años: obliga a **designar un Responsable de Seguridad de la Información**, a adoptar el Marco de Ciberseguridad y alcanzar los niveles mínimos de madurez, a **notificar los incidentes al CERTuy dentro de las 24 horas** y a **conservar los registros de auditoría (*logs*) doce meses**. |
| **Ley N.º 18.331** | 2008 | Protección de datos personales. Es la que define los **datos sensibles** que vimos en la clase 2. |
| **Ley N.º 20.327** | 2024 | Tipifica los delitos informáticos, incluido el acceso no autorizado. Está en el acuerdo de uso responsable de este curso. |

### El Marco de Ciberseguridad del Uruguay

Es el documento que publica **AGESIC** con los requisitos concretos. La versión vigente es la **5.0** y está alineada con el marco internacional del NIST. Tres cosas hay que saber de él:

**Seis funciones.** Organiza todo el trabajo de ciberseguridad en seis verbos, y conviene aprenderlos en orden porque describen un ciclo completo:

```
GOBERNAR  →  IDENTIFICAR  →  PROTEGER  →  DETECTAR  →  RESPONDER  →  RECUPERAR
   ↑                                                                      │
   └──────────────────────  se aprende y se ajusta  ──────────────────────┘
```

- **Gobernar:** quién decide, con qué política, con qué presupuesto. Acá vive la política de seguridad.
- **Identificar:** qué activos tengo y qué riesgos corren. *(Es la clase 1.)*
- **Proteger:** los controles. *(Clases 8, 9 y 10.)*
- **Detectar:** darse cuenta de que está pasando algo. *(Clase 10.)*
- **Responder:** contener y erradicar. *(Clase 11.)*
- **Recuperar:** volver a operar y aprender. *(Clases 11 y 12.)*

**Cinco niveles de madurez, del 0 al 4.** No se aprueba o se reprueba: se mide en qué punto está la organización.

| Nivel | Nombre | Cómo se reconoce |
|:---:|---|---|
| 0 | Sin medidas | No hay nada, ni siquiera responsable. |
| 1 | Iniciativas aisladas | Alguien hace algo por iniciativa propia. Si se va esa persona, se va todo. |
| 2 | Responsabilidades definidas | Hay roles asignados y documentos escritos. |
| 3 | Formalización y métricas | Se cumple, se mide y se puede demostrar con evidencia. |
| 4 | Mejora continua | Se revisa y se ajusta solo. |

**Tres perfiles.** No se le pide lo mismo a una oficina chica que a una que opera un servicio crítico: el marco define perfiles **Básico**, **Estándar** y **Avanzado**, y cada uno prioriza distinto los requisitos. Es la misma lógica de la clase 2: no hay jerarquía universal, hay jerarquía por organización.

> [!IMPORTANT]
> **El dato incómodo.** Tener la obligación escrita desde 2009 no significa que esté cumplida. Según información de AGESIC difundida en 2026, apenas **10 de 244 organismos públicos** relevados declaraban cumplimiento total del decreto de ciberseguridad. Volvé a leer la lista de filtraciones de la clase 2 con ese número en la cabeza: no fueron ataques sofisticados contra defensas modernas. Fueron organismos sin política, sin responsable y sin madurez.

📄 Detalle y enlaces en [`recursos/normativa-uruguay.md`](../recursos/normativa-uruguay.md)

---

## 8. Anatomía de una política que sirve

Una política de seguridad no es un texto de opinión. Tiene partes fijas, y si le falta alguna, no se puede aplicar:

| Parte | Qué contesta | Si falta… |
|---|---|---|
| **Objetivo** | Para qué existe este documento | Nadie entiende qué problema resuelve. |
| **Alcance** | A quién y a qué aplica | Todos suponen que es para otro. |
| **Roles y responsabilidades** | Quién responde por cada cosa | No hay a quién reclamarle. |
| **Reglas** | Qué se debe y qué no se debe hacer | Es una declaración de intenciones. |
| **Consecuencias del incumplimiento** | Qué pasa si no se cumple | Es una sugerencia. |
| **Vigencia y revisión** | Desde cuándo rige y cada cuánto se revisa | Queda vieja y nadie se entera. |
| **Aprobación** | Quién la firma | No obliga a nadie. |

### Cómo se escribe una regla que se puede cumplir

Una regla sirve cuando se puede **verificar**. Compará:

| ❌ No sirve | ✅ Sirve |
|---|---|
| «Los docentes deben cuidar sus contraseñas.» | «Cada docente usa una cuenta personal e intransferible. Las cuentas compartidas están prohibidas.» |
| «Se harán respaldos periódicos.» | «El servidor de calificaciones se respalda todos los días a las 20:00, en un medio desconectado, y una vez por mes se prueba la restauración.» |
| «Se debe usar el equipamiento de forma responsable.» | «Los equipos de la sala se bloquean al ausentarse. No se instala software sin autorización del referente informático.» |
| «Se reportarán los incidentes.» | «Todo incidente se avisa al referente informático el mismo día, sin importar la hora. Avisar de buena fe nunca es motivo de sanción.» |

Fijate qué tienen en común las de la derecha: **sujeto, acción, plazo y verificabilidad**. Alguien puede ir a mirar si se cumple o no.

> [!TIP]
> **La última fila es la más importante de todas.** Si reportar un error te puede costar una sanción, nadie reporta nada, y la organización se entera de sus incidentes por la prensa. Una política que castiga al que avisa está comprando silencio.

---

## 9. Seis maneras de tener una política inútil

> [!WARNING]
> Estas seis explican por qué muchas organizaciones tienen política y de todos modos les pasa lo mismo que a las que no tienen.

**1. La política PDF.** Existe, está firmada, está en una carpeta compartida, y nadie que trabaje ahí la leyó jamás. Una política que no se comunica no rige, por más firma que tenga.

**2. La política copiada.** Descargada de otra organización, con el nombre cambiado. Habla de sucursales que no existen y de sistemas que no se usan. Se nota enseguida: menciona cosas que en el liceo no hay.

**3. La política imposible.** Exige lo que nadie puede cumplir con los recursos que hay. Genera incumplimiento generalizado y, peor, enseña que las reglas son decorativas. Es el principio 8 pisoteado.

**4. La política sin dueño ni fecha.** Nadie es responsable de mantenerla, no dice cuándo se revisa, y adentro todavía figura un sistema que se dejó de usar hace cuatro años.

**5. La política que sanciona sin haber avisado.** Se aplica una consecuencia que la persona no tenía manera de conocer. Además de injusto, es inútil: no cambia conductas, solo genera miedo a hablar.

**6. La política confundida con una herramienta.** «Nuestra política de seguridad es el antivirus.» Un antivirus es un control. La política es la decisión de que ese control exista, quién lo administra y qué se hace cuando salta una alerta.

---

## 10. Para la próxima clase

📝 **Actividad:** [Redactar una política de seguridad](../actividades/03-politica-de-seguridad.md) — en grupos, con auditoría cruzada entre equipos.

✅ **Autoevaluación:** [Poné a prueba lo que leíste](../autoevaluacion/03-arquitectura-y-politicas.md) — 10 preguntas con respuestas explicadas.

📥 **Presentación:** [Clase 3 · Arquitectura y políticas](../presentaciones/Clase3-Arquitectura-y-Politicas.pptx) — la misma que se usó en el pizarrón, con las notas del docente en cada diapositiva.

📖 **Consulta:** [Glosario](../recursos/glosario.md) · [Bibliografía](../recursos/bibliografia.md) · [Normativa uruguaya](../recursos/normativa-uruguay.md)

🔭 **Lo que viene:** en la clase 4 empezamos con las amenazas concretas —malware, ransomware y phishing—, es decir, contra qué tienen que sostenerse todos estos principios.

> **Para pensar antes de la próxima clase.** Elegí una regla informática que exista de hecho en tu liceo, aunque nadie la haya escrito nunca: cómo se pide la clave del wifi, quién puede usar la impresora, qué se hace cuando una máquina anda mal. ¿A qué nivel de la pirámide correspondería? ¿Y a qué nivel de madurez está esa práctica, del 0 al 4?

---

### Fuentes

- Saltzer, J. H. y Schroeder, M. D. (1975). *The Protection of Information in Computer Systems*. Proceedings of the IEEE, 63(9). <https://web.mit.edu/Saltzer/www/publications/protection/>
- AGESIC. *Marco de Ciberseguridad*, versión 5.0. <https://www.gub.uy/agencia-gobierno-electronico-sociedad-informacion-conocimiento/comunicacion/publicaciones/marco-ciberseguridad-50>
- Decreto N.º 452/009. *Política de Seguridad de la Información para Organismos de la Administración Central*. <https://www.gub.uy/centro-nacional-respuesta-incidentes-seguridad-informatica/institucional/normativa>
- Decreto N.º 66/025. <https://www.impo.com.uy/bases/decretos/66-2025>
- Ley N.º 18.331 de Protección de Datos Personales · Ley N.º 20.327 de delitos informáticos.
- NIST. *Cybersecurity Framework 2.0* (2024), del que toma su estructura el marco uruguayo. <https://www.nist.gov/cyberframework>

> [!NOTE]
> La normativa uruguaya de ciberseguridad está cambiando rápido: entre 2025 y 2026 se dictaron decretos nuevos y el Marco se actualizó de versión. Antes de citar un número en un trabajo, verificá la versión vigente en gub.uy y anotá la fecha de consulta. Trabajar con normativa desactualizada es un error caro, y es exactamente el tipo de rigor que se evalúa en este curso.

---

[⬅ Volver al índice del curso](../README.md)
