# Clase 1 · Fundamentos de la ciberseguridad

> Conceptos, marcos de referencia internacionales y marco normativo uruguayo

**Contenidos del programa:** 1.1 Conceptos de Ciberseguridad · 1.5 Principales amenazas (introducción) · 1.6 Políticas de seguridad (introducción)
**Competencia:** CET1
**Tiempo de lectura:** unos 20 minutos

---

> [!TIP]
> **Cómo usar este material.** No hace falta memorizarlo. Lo que sí hay que lograr es entender las definiciones de la sección 2 y saber ubicar cada concepto en un ejemplo concreto. Las referencias entre paréntesis remiten a la [bibliografía](../recursos/bibliografia.md).

## Contenido

1. [Qué es la ciberseguridad (y qué no es)](#1-qué-es-la-ciberseguridad-y-qué-no-es)
2. [La tríada CID](#2-la-tríada-cid-confidencialidad-integridad-disponibilidad)
3. [Vocabulario técnico básico](#3-vocabulario-técnico-básico)
4. [Marcos de referencia internacionales](#4-marcos-de-referencia-internacionales)
5. [Panorama de amenazas](#5-panorama-de-amenazas-qué-está-pasando-realmente)
6. [Marco normativo uruguayo](#6-marco-normativo-uruguayo)
7. [Ética profesional](#7-ética-profesional)
8. [Para la próxima clase](#8-para-la-próxima-clase)

---

## 1. Qué es la ciberseguridad (y qué no es)

Existe una imagen popular de la ciberseguridad construida por el cine y las series: una persona sola frente a una pantalla negra, tecleando muy rápido, que «entra» en cualquier sistema en treinta segundos.

Esa imagen es falsa y, sobre todo, es poco útil. La ciberseguridad real es una disciplina de ingeniería y de gestión: se ocupa de proteger información y sistemas frente a amenazas, con métodos, normas y evidencia.

La **Unión Internacional de Telecomunicaciones**, organismo especializado de las Naciones Unidas, la define en su Recomendación X.1205 como el conjunto de herramientas, políticas, directrices, métodos de gestión de riesgos, acciones, formación, prácticas idóneas y tecnologías que pueden utilizarse para proteger los activos de una organización y de sus usuarios en el ciberentorno (ITU-T, 2008).

Conviene detenerse en esa enumeración: **de los ocho elementos que menciona la definición, solo uno es tecnología**. Los otros siete son personas, procesos y decisiones.

Por su parte, la norma **ISO/IEC 27000** define la seguridad de la información como la preservación de la confidencialidad, la integridad y la disponibilidad de la información (ISO/IEC, 2018). Esa tríada es el punto de partida de todo lo demás.

> [!NOTE]
> **Tres términos que se parecen pero no son iguales**
>
> - **Seguridad de la información:** protege la información en cualquier soporte, incluido el papel y la comunicación oral.
> - **Seguridad informática:** protege los sistemas informáticos que procesan esa información.
> - **Ciberseguridad:** protege activos y personas en el ciberentorno, incluyendo redes, dispositivos conectados y servicios en línea.
>
> Se solapan, y en la práctica profesional a veces se usan de forma intercambiable. En este curso usamos «ciberseguridad» en el sentido amplio del programa.

---

## 2. La tríada CID: confidencialidad, integridad, disponibilidad

Todo incidente de seguridad, sin excepción, puede describirse como la afectación de una o más de estas tres propiedades. Es la herramienta de análisis más simple y más potente del curso.

| Propiedad | Qué garantiza | Ejemplo de violación |
|---|---|---|
| **Confidencialidad** | Que la información solo sea accesible para quien está autorizado. | Se filtra la base de datos de estudiantes de un centro educativo y circula por internet. |
| **Integridad** | Que la información sea exacta y completa, y que no se modifique sin autorización. | Alguien altera una calificación en el sistema de gestión sin dejar rastro. |
| **Disponibilidad** | Que la información y los servicios estén accesibles cuando se los necesita. | Un ataque de denegación de servicio deja fuera de línea el portal de inscripciones el último día de plazo. |

La misma norma agrega otras tres propiedades que en muchos contextos son igual de importantes:

- **Autenticidad** — que quien dice ser el emisor efectivamente lo sea.
- **No repudio** — que quien realizó una acción no pueda negar después haberla realizado.
- **Trazabilidad** (o rendición de cuentas) — que toda acción quede registrada y pueda atribuirse a un responsable.

> [!IMPORTANT]
> Las tres propiedades de la tríada **pueden entrar en conflicto entre sí**. Cifrar una base de datos con una clave que solo conoce una persona maximiza la confidencialidad y destruye la disponibilidad el día que esa persona no está.
>
> La seguridad no consiste en llevar las tres al máximo, sino en **equilibrarlas** según lo que la organización necesita proteger (cf. Stallings y Brown, 2018, cap. 1).

---

## 3. Vocabulario técnico básico

Estos seis términos aparecen en toda la bibliografía profesional y en las normas. Se usan con precisión: **no son intercambiables**.

| Término | Definición | Ejemplo en la escuela |
|---|---|---|
| **Activo** | Todo aquello que tiene valor para la organización y por lo tanto requiere protección. | La base de datos de calificaciones. |
| **Amenaza** | Causa potencial de un incidente no deseado. Puede ser deliberada, accidental o ambiental. | Un atacante con motivación económica; también un corte de energía. |
| **Vulnerabilidad** | Debilidad de un activo o de un control que puede ser explotada por una amenaza. | El servidor no recibe actualizaciones desde hace dos años. |
| **Riesgo** | Combinación de la probabilidad de que una amenaza explote una vulnerabilidad y del impacto que eso produciría. | Alta probabilidad de infección por ransomware, con impacto crítico. |
| **Impacto** | Consecuencia adversa concreta si el incidente ocurre. | Se pierden las calificaciones de todo el semestre. |
| **Control** | Medida que modifica el riesgo. Puede ser técnica, organizativa, física o de personal. | Copias de seguridad diarias, verificadas y desconectadas. |

### La ecuación que ordena todo el curso

```
RIESGO  =  AMENAZA  ×  VULNERABILIDAD  ×  IMPACTO
```

No es una fórmula matemática exacta, sino un **modelo conceptual** (NIST, 2012). Su valor está en lo que muestra:

- **La amenaza no se puede eliminar.** Los atacantes existen y no dependen de nosotros.
- **La vulnerabilidad sí se puede reducir**: actualizando, configurando bien, formando a las personas.
- **El impacto también se puede reducir**: con copias de seguridad, segmentación de la red y planes de respuesta.

Ahí, en esas dos últimas líneas, es donde trabaja la ciberseguridad.

---

## 4. Marcos de referencia internacionales

La ciberseguridad profesional no se improvisa: se apoya en marcos y normas construidos por consenso internacional. Conocerlos por nombre y saber para qué sirve cada uno es parte del perfil técnico de esta orientación.

### 4.1 NIST Cybersecurity Framework 2.0

Publicado por el Instituto Nacional de Estándares y Tecnología de los Estados Unidos, es hoy el marco de gestión de ciberseguridad más difundido del mundo. Su versión 2.0, de 2024, organiza toda la disciplina en **seis funciones** (NIST, 2024):

| Función | De qué se ocupa |
|---|---|
| **GOVERN** (Gobernar) | Establecer la estrategia, los roles y la supervisión. Se agregó en la versión 2.0, y su incorporación reconoce que la ciberseguridad es una decisión de dirección, no solo un asunto técnico. |
| **IDENTIFY** (Identificar) | Conocer los activos, los riesgos y el contexto. No se puede proteger lo que no se sabe que se tiene. |
| **PROTECT** (Proteger) | Aplicar los controles: control de acceso, cifrado, formación, mantenimiento. |
| **DETECT** (Detectar) | Descubrir que algo está ocurriendo. El tiempo promedio de detección de una intrusión sigue midiéndose en semanas. |
| **RESPOND** (Responder) | Contener, erradicar, comunicar. |
| **RECOVER** (Recuperar) | Restaurar los servicios y aprender del incidente. |

> [!NOTE]
> Esto nos importa por una razón práctica: el **Marco de Ciberseguridad de Agesic**, obligatorio en Uruguay, está construido sobre esta misma lógica.

### 4.2 ISO/IEC 27001 e ISO/IEC 27002

La familia ISO/IEC 27000 es el estándar internacional para sistemas de gestión de la seguridad de la información.

- **ISO/IEC 27001:2022** establece los requisitos que debe cumplir una organización para certificarse.
- **ISO/IEC 27002:2022** desarrolla los controles concretos, agrupados en cuatro temas: organizativos, de personas, físicos y tecnológicos.

Que una empresa esté «certificada en 27001» es hoy un requisito habitual para contratar con el Estado o con clientes internacionales.

### 4.3 OWASP Top 10

El **Open Worldwide Application Security Project** es una fundación sin fines de lucro que produce, de forma abierta y gratuita, la mayor parte del material de referencia sobre seguridad de aplicaciones que usa la industria.

Su documento más conocido es el **OWASP Top 10**, que ordena los diez riesgos más críticos de las aplicaciones web a partir del análisis de datos reales de vulnerabilidades. La edición vigente es la de **2025**, la octava desde 2003, elaborada sobre el análisis de más de 175.000 CVE (OWASP Foundation, 2025).

| OWASP Top 10:2025 | De qué se trata, en una línea |
|---|---|
| **A01** · Broken Access Control | Un usuario puede hacer o ver cosas que no le corresponden. |
| **A02** · Security Misconfiguration | El sistema está mal configurado: valores por defecto, permisos abiertos. |
| **A03** · Software Supply Chain Failures | El problema entra por una dependencia o un proveedor, no por tu código. |
| **A04** · Cryptographic Failures | Cifrado ausente, débil o mal implementado. |
| **A05** · Injection | La entrada del usuario se interpreta como código o consulta. |
| **A06** · Insecure Design | El fallo está en el diseño: no hay control que lo arregle después. |
| **A07** · Authentication Failures | Se puede suplantar a un usuario legítimo. |
| **A08** · Software or Data Integrity Failures | Se confía en código o datos sin verificar su origen. |
| **A09** · Security Logging and Alerting Failures | Pasó algo y nadie se enteró, porque no se registra ni se alerta. |
| **A10** · Mishandling of Exceptional Conditions | El sistema falla mal: revela información o queda en estado inseguro. |

🔗 Documento completo: <https://owasp.org/Top10/2025/>

### 4.4 CIS Critical Security Controls

El **Center for Internet Security** publica un conjunto priorizado de controles, en su versión 8. Su utilidad es que están ordenados por relación entre esfuerzo y beneficio: indica **qué conviene hacer primero** cuando los recursos son limitados. Los primeros son inventario de activos de hardware y de software, protección de datos y configuración segura.

### 4.5 MITRE ATT&CK

Base de conocimiento pública que cataloga las tácticas y técnicas que **efectivamente usan los atacantes reales**, documentadas a partir de incidentes observados. Se usa para modelar amenazas, entrenar equipos de defensa y evaluar la cobertura de las herramientas de detección.

🔗 <https://attack.mitre.org>

---

## 5. Panorama de amenazas: qué está pasando realmente

Dos publicaciones anuales concentran la evidencia empírica sobre incidentes reales, y ambas son de acceso libre:

- **ENISA Threat Landscape** — de la agencia de ciberseguridad de la Unión Europea.
- **Data Breach Investigations Report (DBIR)** — de Verizon, que analiza decenas de miles de incidentes por año.

Un hallazgo se repite edición tras edición en ambos informes y ordena buena parte de este curso: **una proporción muy alta de las brechas involucra un factor humano**, ya sea por error, por uso indebido de credenciales o por ingeniería social.

La consecuencia es directa: el eslabón que más se ataca no es el más técnico. Por eso una campaña de concientización bien hecha puede reducir más riesgo que un equipamiento caro.

### Categorías de amenaza que vamos a trabajar

- **Malware** en sus distintas formas: virus, gusanos, troyanos, spyware, y especialmente **ransomware**, que cifra los archivos de la víctima y exige un pago.
- **Ingeniería social**: manipulación de personas para obtener información o accesos. Incluye el **phishing** (por correo), el **vishing** (por voz) y el **smishing** (por SMS).
- **Ataques de denegación de servicio (DoS/DDoS)**: saturan un servicio hasta dejarlo inaccesible.
- **Explotación de vulnerabilidades**: aprovechamiento de fallos conocidos en software sin actualizar.
- **Amenazas internas**: provienen de personas de la propia organización, por descuido o de forma deliberada.
- **APT** (*Advanced Persistent Threats*): intrusiones prolongadas y sigilosas, normalmente con respaldo de recursos importantes, orientadas a permanecer sin ser detectadas.

---

## 6. Marco normativo uruguayo

Uruguay tiene una institucionalidad de ciberseguridad consolidada y, desde 2024, legislación penal específica. Esta sección es la que más consecuencias prácticas tiene para ustedes.

📄 Detalle completo en [`recursos/normativa-uruguay.md`](../recursos/normativa-uruguay.md)

### 6.1 Institucionalidad

- **Agesic** (Agencia de Gobierno Electrónico y Sociedad de la Información y del Conocimiento) es el organismo rector en la materia y elabora el Marco de Ciberseguridad nacional.
- **CERTuy** (Centro Nacional de Respuesta a Incidentes de Seguridad Informática) fue creado por el artículo 73 de la **Ley N.º 18.362** y reglamentado por el **Decreto N.º 451/009**. Es el equipo nacional al que se reportan y con el que se coordinan los incidentes.
- El **Decreto N.º 66/025** hizo obligatoria la adopción del Marco de Ciberseguridad de Agesic para los organismos públicos y también para empresas privadas que prestan servicios críticos al Estado. La ciberseguridad dejó de ser una recomendación técnica para pasar a ser una exigencia legal.
- La **Ley N.º 18.331** de Protección de Datos Personales regula el tratamiento de datos personales y otorga derechos a sus titulares. Aplica a cualquier organización que maneje datos de personas, **incluida una escuela**.

### 6.2 La Ley N.º 20.327 de ciberdelincuencia

Promulgada el 25 de setiembre de 2024, es la primera ley uruguaya que tipifica específicamente los ciberdelitos, incorporándolos al Código Penal. Entre otras figuras, sanciona el acoso telemático, el fraude informático, el daño informático, el acceso ilícito a datos informáticos, la interceptación ilícita, la vulneración de datos, la suplantación de identidad y el abuso de los dispositivos.

> [!CAUTION]
> **El artículo que más les concierne: 297 BIS del Código Penal.**
>
> Castiga con **seis a veinticuatro meses de prisión** a quien, por medios informáticos o telemáticos, **sin autorización y sin justa causa**, acceda, interfiera, difunda, venda o ceda información ajena contenida en soporte digital.
>
> Léase con atención: el delito se configura **con el acceso sin autorización**. No exige que se haya roto nada, ni que se haya robado nada, ni que haya habido perjuicio económico.
>
> «Entré para ver si podía» **no es una defensa**.

Un detalle que conecta directamente con esta materia: la propia Ley N.º 20.327 dispone medidas educativas de prevención y prevé una campaña nacional en los centros educativos que debe incluir, entre otros contenidos, los fraudes orientados al acceso a datos —phishing, vishing, smishing, malware, troyanos e ingeniería social— y las buenas prácticas para el uso de canales digitales.

Es decir: **el propio legislador entendió que la respuesta al ciberdelito pasa, en parte, por lo que se hace en aulas como esta.**

---

## 7. Ética profesional

El conocimiento técnico de esta materia es de **doble uso**: las mismas herramientas y los mismos razonamientos que permiten proteger un sistema permiten atacarlo. Por eso, en seguridad, la ética no es un apéndice del contenido técnico sino una condición de su enseñanza.

Tres principios ordenan la práctica profesional y rigen este curso:

1. **Autorización previa, explícita y escrita.** Ninguna prueba se realiza sobre un sistema sin permiso documentado del titular, con alcance definido.
2. **Divulgación responsable.** Si se descubre una vulnerabilidad, se comunica al responsable del sistema y se le da tiempo razonable para corregirla antes de hacerla pública. No se publica ni se explota.
3. **Minimización del daño.** Se accede a lo mínimo necesario para demostrar el problema, y nada más. No se copian datos de terceros.

> [!IMPORTANT]
> **La diferencia entre un profesional de la seguridad y un delincuente no es el conocimiento: es la autorización.**

Al final de esta clase vamos a redactar entre todos, y firmar, el [**Acuerdo de uso responsable**](../CODE_OF_CONDUCT.md). Ese documento forma parte del portafolio de la unidad curricular.

---

## 8. Para la próxima clase

📝 **Actividad:** [Análisis de un incidente real](../actividades/01-analisis-de-incidente.md)

✅ **Autoevaluación:** [Poné a prueba lo que leíste](../autoevaluacion/01-fundamentos.md) — 10 preguntas con respuestas explicadas.

📖 **Consulta:** [Glosario](../recursos/glosario.md) · [Bibliografía](../recursos/bibliografia.md) · [Normativa uruguaya](../recursos/normativa-uruguay.md)

---

[⬅ Volver al índice del curso](../README.md)
