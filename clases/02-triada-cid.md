# Clase 2 · La tríada CID en profundidad y análisis de casos

> Qué protege cada propiedad, cómo se rompe cada una, y cómo se clasifica un incidente real

**Contenidos del programa:** 1.1 Conceptos de Ciberseguridad
**Competencia:** CET1
**Tiempo de lectura:** unos 25 minutos

---

> [!TIP]
> **Cómo usar este material.** En la clase 1 la tríada ocupó una tabla de tres filas. Acá ocupa toda la clase, y esa diferencia es intencional: la tríada no es una definición para repetir en una prueba, es una **herramienta de trabajo**. Al terminar tenés que ser capaz de leer la noticia de un incidente y decir, con fundamento, qué propiedad se rompió y por qué.

## Contenido

1. [Por qué volvemos sobre las mismas tres palabras](#1-por-qué-volvemos-sobre-las-mismas-tres-palabras)
2. [Confidencialidad](#2-confidencialidad)
3. [Integridad](#3-integridad)
4. [Disponibilidad](#4-disponibilidad)
5. [Lo que la tríada no alcanza a cubrir](#5-lo-que-la-tríada-no-alcanza-a-cubrir)
6. [El triángulo se tensa: conflictos entre propiedades](#6-el-triángulo-se-tensa-conflictos-entre-propiedades)
7. [Método de análisis en cinco pasos](#7-método-de-análisis-en-cinco-pasos)
8. [Cinco casos para analizar](#8-cinco-casos-para-analizar)
9. [Errores frecuentes al clasificar](#9-errores-frecuentes-al-clasificar)
10. [Para la próxima clase](#10-para-la-próxima-clase)

---

## 1. Por qué volvemos sobre las mismas tres palabras

Un profesional de la seguridad recibe, en un día cualquiera, información desordenada: un ticket que dice «no anda el sistema», una captura de pantalla, un rumor, una nota de prensa. La primera tarea siempre es la misma: **traducir ese ruido a un lenguaje que permita decidir**.

La tríada CID es ese lenguaje. Tiene tres palabras y sirve para cuatro cosas distintas:

| Se usa para | Ejemplo de pregunta que responde |
|---|---|
| **Clasificar** un incidente que ya ocurrió | ¿Qué se rompió acá, exactamente? |
| **Priorizar** qué se protege primero | En un hospital, ¿qué duele más: que se filtre o que no funcione? |
| **Diseñar** controles proporcionados | ¿Cifrar, respaldar, firmar? Depende de qué propiedad quiero sostener. |
| **Comunicar** con quien no es técnico | Una directora entiende «no podemos garantizar que las notas sean las que se pusieron». |

> [!NOTE]
> La tríada aparece con dos nombres según el idioma: **CID** en español (Confidencialidad, Integridad, Disponibilidad) y **CIA** en inglés (*Confidentiality, Integrity, Availability*). Es la misma cosa. En bibliografía profesional vas a ver «CIA triad» todo el tiempo, y no tiene nada que ver con la agencia de inteligencia.

---

## 2. Confidencialidad

> **Definición.** Propiedad por la cual la información no se pone a disposición ni se revela a individuos, entidades o procesos no autorizados (ISO/IEC 27000).

Leé la definición con atención: no dice «que la información sea secreta». Dice **no autorizados**. La confidencialidad no consiste en que nadie vea el dato, sino en que lo vea exactamente quien debe.

### 2.1 Los dos principios que la sostienen

- **Mínimo privilegio.** Cada persona, cuenta o proceso tiene los permisos mínimos que necesita para hacer su tarea, y ni uno más. Un adscripto necesita ver las inasistencias; no necesita poder exportar la base entera.
- **Necesidad de conocer** (*need to know*). Aunque alguien tenga el nivel de autorización suficiente, solo accede a la información que su tarea concreta requiere.

### 2.2 Cómo se protege

| Control | Qué hace |
|---|---|
| **Control de acceso** | Define quién puede hacer qué sobre cada recurso. Es el control central; lo vemos en la clase 8. |
| **Cifrado** | Vuelve ilegible el dato para quien no tiene la clave, tanto **en tránsito** (HTTPS, VPN) como **en reposo** (disco, base de datos). Clase 9. |
| **Clasificación de la información** | Etiquetar los datos según su nivel (público, interno, confidencial, reservado) para saber qué proteger y cuánto. |
| **Enmascaramiento y anonimización** | Mostrar `****4821` en lugar del número completo; quitar identificadores en conjuntos de datos usados para estadística. |
| **Controles físicos y de personal** | Puertas, pantallas orientadas, acuerdos de confidencialidad. La confidencialidad también se pierde mirando por encima del hombro (*shoulder surfing*). |

### 2.3 La propiedad asimétrica: la confidencialidad no se restaura

> [!IMPORTANT]
> Un servicio caído se levanta. Un dato alterado se corrige desde una copia. **Un dato filtrado no se puede «desfiltrar».**
>
> Es la única de las tres propiedades cuya pérdida es **irreversible**. Por eso, cuando en una organización se discute dónde poner el presupuesto, los datos cuya filtración sería catastrófica —historias clínicas, identidades de personas protegidas, datos de menores— justifican una inversión que a primera vista parece desproporcionada.

### 2.4 Confidencialidad y datos personales en Uruguay

No todos los datos valen lo mismo ante la ley. La **Ley N.º 18.331** de Protección de Datos Personales distingue los **datos sensibles**: los que revelan origen racial o étnico, preferencias políticas, convicciones religiosas o morales, afiliación sindical, e información referente a la vida sexual o a la **salud**.

Esa distinción tiene una consecuencia directa para el análisis: una filtración de 1.600.000 registros de vacunación no es «una filtración grande», es una **filtración de datos sensibles**, con un régimen legal más estricto y un daño potencial mayor para las personas.

📄 Detalle en [`recursos/normativa-uruguay.md`](../recursos/normativa-uruguay.md)

---

## 3. Integridad

> **Definición.** Propiedad de exactitud y completitud de la información (ISO/IEC 27000).

Dos palabras, y las dos importan. **Exactitud**: el dato refleja la realidad. **Completitud**: no le falta nada. Un registro médico al que se le borró una alergia es tan peligroso como uno al que se le inventó un diagnóstico.

### 3.1 Integridad de los datos e integridad del sistema

- **Integridad de los datos:** la información no se modifica de forma no autorizada ni se corrompe.
- **Integridad del sistema:** el sistema hace lo que se supone que hace, sin manipulaciones. Un servidor con un programa oculto instalado por un atacante puede tener todos sus datos correctos y aun así haber perdido integridad.

### 3.2 Cómo se protege

| Control | Qué hace |
|---|---|
| **Funciones hash** | Producen una «huella digital» del archivo. Si cambia un solo bit, la huella cambia por completo. Clase 9. |
| **Firmas digitales** | Verifican a la vez la integridad y el origen del dato. |
| **Validación de entradas** | Rechazan datos con formato o rango imposible antes de guardarlos. Media docena de vulnerabilidades del OWASP Top 10 nacen de acá. |
| **Registros de auditoría** (*logs*) | Dejan constancia de quién modificó qué y cuándo. No previenen: permiten detectar y reconstruir. |
| **Separación de funciones y doble control** | Quien carga una operación no es quien la autoriza. Es un control organizativo, no técnico, y es de los más eficaces. |
| **Control de versiones y copias de seguridad** | Permiten volver al estado correcto anterior. |

### 3.3 La propiedad silenciosa

> [!CAUTION]
> La pérdida de disponibilidad se nota en segundos: alguien llama y dice que no anda.
> La pérdida de confidencialidad se nota tarde, pero se nota: los datos aparecen publicados.
> **La pérdida de integridad puede no notarse nunca.**
>
> Un dato alterado sigue estando disponible, sigue siendo confidencial y se ve exactamente igual que antes. Simplemente es falso. Y todas las decisiones que se tomen a partir de él van a ser decisiones equivocadas tomadas con total confianza.

Por eso los ataques más sofisticados y mejor financiados suelen ir contra la integridad: no buscan romper nada visible, buscan que un sistema siga funcionando pero mintiendo.

---

## 4. Disponibilidad

> **Definición.** Propiedad de ser accesible y utilizable a demanda por una entidad autorizada (ISO/IEC 27000).

Es la propiedad más fácil de entender y la más fácil de subestimar. Un sistema inaccesible tiene, para efectos prácticos, el mismo valor que un sistema que no existe.

### 4.1 Se mide, y se mide en serio

La disponibilidad es la única de las tres propiedades que se expresa habitualmente con un número: el porcentaje de tiempo en que el servicio está operativo, comprometido en un **acuerdo de nivel de servicio** (*SLA*).

| Disponibilidad comprometida | Tiempo fuera de servicio admisible por año |
|---|---|
| 99 % («dos nueves») | 3 días 15 horas |
| 99,9 % («tres nueves») | 8 horas 46 minutos |
| 99,99 % («cuatro nueves») | 52 minutos |
| 99,999 % («cinco nueves») | 5 minutos 15 segundos |

Cada nueve adicional multiplica el costo. Ahí está la decisión de gestión: nadie paga cinco nueves para la cartelera de novedades del liceo.

### 4.2 Dos siglas que hay que saber

Cuando se planifica la recuperación de un servicio se fijan dos objetivos, y conviene no confundirlos:

- **RTO** (*Recovery Time Objective*) — **cuánto tiempo** puede estar caído el servicio. Se mide hacia adelante desde el incidente.
- **RPO** (*Recovery Point Objective*) — **cuántos datos** se acepta perder. Se mide hacia atrás desde el incidente, y equivale a la antigüedad máxima tolerable de la copia de seguridad.

> **Ejemplo.** Un sistema de calificaciones con RTO de 4 horas y RPO de 24 horas significa: si se cae el martes a las 10, tiene que estar funcionando a las 14, y se acepta perder lo cargado desde la copia del lunes a la noche. Si el liceo no tolera perder un día de carga, el RPO no es 24 horas y hay que respaldar más seguido. Fijar el RPO es una decisión del negocio, no del técnico.

### 4.3 No siempre hay un atacante

Las causas de indisponibilidad, ordenadas por frecuencia real, incluyen:

- Fallas de hardware y de energía.
- **Errores humanos y de configuración**, incluidos los de actualizaciones mal probadas.
- Ataques de denegación de servicio (DoS/DDoS).
- Ransomware.
- Desastres físicos: incendio, inundación, robo.

El caso 4 de esta clase muestra la caída informática más grande de la historia reciente, y no hubo ningún atacante.

---

## 5. Lo que la tríada no alcanza a cubrir

La tríada es un punto de partida, no un techo. Ya vimos en la clase 1 tres propiedades complementarias: **autenticidad**, **no repudio** y **trazabilidad**. Hay dos más que conviene conocer, porque explican situaciones que la tríada clasifica mal.

En 1998, Donn Parker propuso ampliar la tríada a seis propiedades, en lo que se conoce como el **hexágono de Parker** (*Parkerian Hexad*). Agrega:

| Propiedad | Qué agrega | Caso que la tríada no explica bien |
|---|---|---|
| **Posesión o control** | Tener el soporte físico o lógico del dato, independientemente de que se haya leído. | Roban una laptop con el disco cifrado. **No** se perdió confidencialidad: nadie puede leer nada. Pero sí se perdió la posesión, y eso es un incidente que hay que reportar. |
| **Utilidad** | Que el dato sirva para algo. | Tenés el archivo, está íntegro, está disponible, nadie lo vio… pero se perdió la clave de cifrado. Está ahí y no sirve para nada. |

### Privacidad no es lo mismo que confidencialidad

Se usan como sinónimos y no lo son:

- La **confidencialidad** es una propiedad técnica de la información: quién puede acceder.
- La **privacidad** es un derecho de las personas sobre sus propios datos: qué se recolecta, para qué, por cuánto tiempo, y qué control conservan sobre eso.

Una aplicación puede tener una confidencialidad impecable —cifrado fuerte, control de acceso estricto— y ser un desastre de privacidad, porque recolecta veinte datos que no necesita para funcionar. La confidencialidad protege el dato que ya tenés; la privacidad pregunta si tenías que tenerlo.

---

## 6. El triángulo se tensa: conflictos entre propiedades

> [!IMPORTANT]
> **La seguridad perfecta en las tres propiedades a la vez no existe.** Cada refuerzo de una tiende a costar algo de otra, y siempre cuesta dinero y comodidad.

| Tensión | Ejemplo concreto |
|---|---|
| **Confidencialidad ↔ Disponibilidad** | Cifrar la base con una clave que custodia una sola persona. Confidencialidad máxima; disponibilidad cero el día que esa persona se va de licencia. |
| **Integridad ↔ Disponibilidad** | Validación tan estricta que rechaza operaciones legítimas mal tipeadas. Nada entra corrupto y tampoco entra media clase. |
| **Confidencialidad ↔ Trazabilidad** | Un registro de auditoría completo es una excelente herramienta forense y, a la vez, una base de datos personales más para proteger. |
| **Seguridad ↔ Usabilidad** | Contraseñas de 20 caracteres cambiadas cada 30 días: la gente las anota en un papel bajo el teclado y el control produce el riesgo que quería evitar. |

### El orden de prioridad depende del contexto

No hay una jerarquía universal entre C, I y D. Hay una jerarquía **por organización**, y saber justificarla es parte del oficio:

| Organización | Prioridad típica | Por qué |
|---|---|---|
| Hospital, emergencia médica | **D** → I → C | Un sistema caído durante una urgencia cuesta vidas. La historia clínica igual es dato sensible: C nunca es descartable. |
| Banco, sistema de pagos | **I** → C → D | Un saldo equivocado destruye la confianza en el sistema entero. Se prefiere frenar la operativa antes que procesar mal. |
| Periodismo de investigación, defensa de derechos humanos | **C** → I → D | Revelar una fuente puede poner en riesgo a una persona. Publicar un día más tarde, no. |
| Comercio electrónico | **D** → I → C | Cada minuto caído es venta perdida, aunque los datos de tarjeta imponen exigencias fuertes de C. |
| Centro educativo | **I** → C → D | Una calificación alterada compromete la validez de todo el sistema; los datos son además de menores de edad. |

> **Para pensar antes de la próxima clase:** ¿cuál sería el orden en el sistema de gestión de tu liceo, y qué argumento darías para defenderlo frente a alguien que lo ordena distinto? No hay una única respuesta correcta; hay respuestas fundamentadas y respuestas improvisadas.

---

## 7. Método de análisis en cinco pasos

Este es el procedimiento que vamos a usar en la actividad y en las evaluaciones. Cinco pasos, siempre en el mismo orden.

```
1. ACTIVO        ¿Qué se afectó? Nombrarlo con precisión.
2. PROPIEDADES   ¿Qué se rompió: C, I, D? Puede ser más de una. Justificar cada una.
3. VULNERABILIDAD ¿Por dónde entró? Si no se sabe, decirlo.
4. IMPACTO       ¿Sobre quién recae el daño, y de qué tipo es?
5. CONTROLES     ¿Qué lo habría evitado o reducido? Priorizados.
```

### Paso 2 en detalle: el diccionario de traducción

La prensa no escribe en lenguaje técnico. Esta tabla traduce lo que suele decir una noticia a la propiedad que corresponde:

| Lo que dice la noticia | Propiedad afectada |
|---|---|
| «se filtraron», «quedaron expuestos», «publicaron la base de datos» | **Confidencialidad** |
| «alteraron», «modificaron el sitio», «transferencias no autorizadas», «datos corruptos» | **Integridad** |
| «se cayó», «fuera de servicio», «no se pudo operar durante X horas» | **Disponibilidad** |
| «cifraron los archivos y piden un rescate» | **Disponibilidad** + casi siempre **Confidencialidad** (ver §9) |
| «suplantaron la identidad de», «se hicieron pasar por» | **Autenticidad** (y normalmente C) |
| «no se puede determinar quién hizo la operación» | **Trazabilidad** / **no repudio** |

### Paso 4 en detalle: el impacto no es solo técnico

Al describir el impacto conviene recorrer cuatro dimensiones, porque casi siempre hay más de una:

- **Operativo:** qué dejó de funcionar y por cuánto tiempo.
- **Económico:** costo de la respuesta, de la interrupción, de las multas.
- **Reputacional:** pérdida de confianza de usuarios, ciudadanos o clientes.
- **Sobre las personas:** el daño concreto a quien es titular de los datos. Suele ser el que menos aparece en la prensa y el más importante.

---

## 8. Cinco casos para analizar

Los cinco están elegidos para cubrir combinaciones distintas de propiedades. Leelos con la tabla del paso 2 al lado.

---

### Caso 1 · Ransomware al Banco Hipotecario del Uruguay (octubre de 2025)

**Qué pasó.** El grupo *crypto24* atacó al BHU y sustrajo unos 700 GB de información, incluidos expedientes clasificados como reservados por quince años, expedientes judiciales de grupos empresariales y registros de procesos de ejecución hipotecaria. La prensa especializada lo describió como el ciberataque más grave que conoce Uruguay hasta la fecha, atribuido no a una falla puntual sino a una acumulación de debilidades y a una cultura institucional que había relegado la ciberseguridad.

| Paso | Análisis |
|---|---|
| **Activo** | Expedientes crediticios y judiciales, y los sistemas que los procesan. |
| **Propiedades** | **Disponibilidad**: el cifrado deja los sistemas y archivos inutilizables. **Confidencialidad**: la exfiltración previa de 700 GB expone información reservada, y esa pérdida ya no se revierte. **Integridad**: en riesgo, porque tras una intrusión prolongada no se puede afirmar sin verificación que los datos no fueron alterados. |
| **Vulnerabilidad** | Acumulación de debilidades organizativas y técnicas; el detalle del vector de entrada no es público. |
| **Impacto** | Operativo y económico para la institución; **sobre las personas**, exposición de la situación financiera y judicial de deudores hipotecarios. Ese dato, en manos equivocadas, habilita extorsión y fraude dirigido. |
| **Controles** | Copias de seguridad desconectadas y probadas; segmentación de la red; cifrado en reposo de los expedientes; detección de exfiltración de volúmenes anómalos; gobierno de la seguridad al máximo nivel. |

> **Lo que enseña este caso:** el ransomware moderno es de **doble extorsión**. Primero roban los datos, después los cifran. Pagar el rescate podría devolver la disponibilidad, pero **no puede devolver la confidencialidad**: los datos ya salieron.

---

### Caso 2 · La cadena de filtraciones al Estado uruguayo (2025)

**Qué pasó.** Durante 2025 se sucedieron filtraciones en organismos públicos uruguayos: Migraciones (14.877 formularios de visa, marzo), Mides (37.756 archivos con cédulas, direcciones y teléfonos, abril), Sucive (618.000 registros con cédulas, matrículas y números de chasis, mayo), MTSS (350.800 registros, junio), registros de vacunación en poder de Agesic (1.645.000 registros con datos de dosis y vacunas, junio), Corte Electoral (622.649 registros con documento, contacto y foto, setiembre), ANEP (2,7 millones de registros de docentes, estudiantes y personal, setiembre) y Ceibal (1 millón de registros con nombres, documentos y números de serie de dispositivos, setiembre). En el primer semestre de 2025, CERTuy registró más de 17.000 ciberincidentes.

| Paso | Análisis |
|---|---|
| **Activo** | Bases de datos personales de la población uruguaya. En varios casos, de menores de edad. |
| **Propiedades** | **Confidencialidad**, casi en estado puro. Los sistemas siguieron funcionando (D intacta) y no hay evidencia de datos alterados (I intacta). Y sin embargo es el conjunto de incidentes más grave del período. |
| **Vulnerabilidad** | Variada: credenciales de administrador expuestas, accesos remotos mal protegidos, sistemas sin actualizar. |
| **Impacto** | Sobre las personas: los datos filtrados alimentan fraude dirigido y suplantación de identidad **durante años**, porque la cédula y la fecha de nacimiento no se pueden cambiar como una contraseña. En el caso de vacunación, además, son **datos sensibles** de salud según la Ley N.º 18.331. |
| **Controles** | Autenticación multifactor en accesos administrativos; cifrado en reposo; minimización de datos (no guardar lo que no se necesita); detección de consultas masivas anómalas; notificación a los titulares. |

> **Lo que enseña este caso:** un incidente puede afectar **una sola** propiedad y ser gravísimo. Y también que la escala importa: la misma filtración, multiplicada por millones de registros, cambia de naturaleza.

---

### Caso 3 · Costa Rica: el ataque que paró un país (abril–junio de 2022)

**Qué pasó.** El 17 de abril de 2022 el grupo Conti atacó cerca de treinta instituciones públicas de Costa Rica, entre ellas el Ministerio de Hacienda, del que sustrajo alrededor de 1 TB de datos, exigiendo 10 millones de dólares. El 8 de mayo el gobierno declaró **estado de emergencia nacional**: era la primera vez que un país lo hacía por un ciberataque. El cierre de los sistemas aduaneros provocó pérdidas estimadas en 30 millones de dólares diarios al sector productivo, y más de 16.000 trabajadores del Ministerio de Educación tuvieron atrasos o errores en sus pagos. El sistema tributario ATV se reabrió el 13 de junio y la plataforma aduanera TICA el 24 de junio. El 31 de mayo, un segundo grupo, Hive, atacó la Caja Costarricense de Seguro Social, con 800 servidores infectados. El gobierno no pagó.

| Paso | Análisis |
|---|---|
| **Activo** | Sistemas tributarios, aduaneros, de salud y de pago de salarios de un Estado. |
| **Propiedades** | **Las tres**, y a escala nacional. **D**: meses de servicios críticos caídos. **C**: 1 TB exfiltrado de Hacienda. **I**: pagos de salarios erróneos, y necesidad de verificar la exactitud de todos los registros tocados. |
| **Vulnerabilidad** | Acceso inicial mediante credenciales comprometidas y movimiento lateral por redes poco segmentadas; ausencia de capacidad nacional de respuesta a esa escala. |
| **Impacto** | Económico masivo y directo sobre la ciudadanía: comercio exterior detenido, salarios impagos, atención de salud afectada. |
| **Controles** | Segmentación de red; MFA en accesos remotos; plan de continuidad probado; equipos nacionales de respuesta con capacidad real; ejercicios de simulacro (los hacemos en la clase 12). |

> **Lo que enseña este caso:** la ciberseguridad es **infraestructura crítica**, no informática de apoyo. Y que la decisión de no pagar el rescate es defendible: pagar financia al atacante y no garantiza nada.

---

### Caso 4 · CrowdStrike: la caída global sin atacante (19 de julio de 2024)

**Qué pasó.** A las 04:09 UTC, la empresa de ciberseguridad CrowdStrike distribuyó una actualización defectuosa de su producto Falcon. El archivo provocó una lectura de memoria fuera de límites en el núcleo de Windows y dejó los equipos en un bucle de reinicio. Se estima que se vieron afectados unos **8,5 millones de dispositivos**. Se cancelaron más de mil vuelos, hubo cirugías suspendidas, bancos y medios fuera de línea y servicios de emergencia 911 afectados en varios estados de EE. UU. La actualización se revirtió a las 05:27 UTC —78 minutos después—, pero cada equipo caído requería intervención manual, y la recuperación llevó días. **No hubo ningún atacante.**

| Paso | Análisis |
|---|---|
| **Activo** | Los propios puestos de trabajo y servidores Windows de miles de organizaciones. |
| **Propiedades** | **Disponibilidad**, exclusivamente. Nadie accedió a datos ajenos ni los modificó. |
| **Vulnerabilidad** | Ausencia de despliegue gradual y de pruebas suficientes en el proceso de actualización del proveedor; y del lado de los clientes, dependencia de un único proveedor con permisos de núcleo, sin escalonamiento. |
| **Impacto** | Operativo y económico global; sobre las personas, vuelos y cirugías. |
| **Controles** | Despliegue por anillos (*canary*), primero a un grupo pequeño; ventanas de actualización escalonadas; planes de continuidad manuales; evaluación del riesgo de concentración de proveedores. |

> **Lo que enseña este caso:** un incidente de seguridad **no requiere un atacante**. Y algo más incómodo: el producto que causó la mayor caída de la historia reciente era un producto de seguridad. Todo control agrega, además de protección, una nueva superficie de falla. Esto conecta con la clase 1: el riesgo también incluye amenazas accidentales.

---

### Caso 5 · Banco Central de Bangladesh: el ataque a la integridad (febrero de 2016)

**Qué pasó.** Entre el 4 y el 5 de febrero de 2016, atacantes que llevaban semanas dentro de la red del Banco Central de Bangladesh emitieron 35 órdenes de transferencia fraudulentas por unos **951 millones de dólares** a través de la red interbancaria SWIFT, usando credenciales legítimas del banco. Cinco órdenes se ejecutaron: 81 millones de dólares hacia Filipinas y 20 millones hacia Sri Lanka. La Reserva Federal bloqueó las 30 restantes. La transferencia a Sri Lanka se frenó porque los atacantes escribieron mal la palabra *Foundation* —pusieron *Fandation*— y el banco intermediario sospechó. De los 81 millones enviados a Filipinas se recuperó una parte pequeña.

| Paso | Análisis |
|---|---|
| **Activo** | El sistema de órdenes de pago del banco y los registros de transacciones. |
| **Propiedades** | **Integridad** en primer lugar: se insertaron órdenes falsas en un sistema que las procesó como verdaderas, y se manipularon los mecanismos de verificación para que las transferencias no se notaran de inmediato. Con ella cae la **autenticidad** (las órdenes parecían del banco) y el **no repudio**. Hubo también pérdida de **confidencialidad** en la fase previa: los atacantes observaron durante semanas cómo operaba el banco. |
| **Vulnerabilidad** | Red interna sin segmentar, equipos sin actualizar, ausencia de doble control efectivo sobre las órdenes de alto monto y controles de verificación manipulables. |
| **Impacto** | Económico directo e irrecuperable en su mayor parte; reputacional para el banco y para el sistema SWIFT completo. |
| **Controles** | Separación de funciones y doble autorización para montos altos; límites y alertas por operación inusual; registros de auditoría **inalterables** y almacenados fuera del sistema auditado; verificación de la integridad del software crítico. |

> **Lo que enseña este caso:** el ataque más rentable no rompe nada visible. Deja el sistema funcionando y le hace decir mentiras. Y también: el control que salvó 850 millones de dólares no fue tecnológico —fue una persona que notó una palabra mal escrita.

---

### Los cinco casos en una tabla

| Caso | C | I | D | Lo que ilustra |
|---|:---:|:---:|:---:|---|
| BHU (UY, 2025) | ✅ | ⚠️ | ✅ | Doble extorsión: pagar no devuelve la confidencialidad |
| Filtraciones al Estado (UY, 2025) | ✅ | — | — | Una sola propiedad, daño masivo y permanente |
| Costa Rica (CR, 2022) | ✅ | ✅ | ✅ | Escala nacional, infraestructura crítica |
| CrowdStrike (global, 2024) | — | — | ✅ | Incidente grave sin atacante |
| Banco de Bangladesh (BD, 2016) | ✅ | ✅ | — | La integridad es la propiedad silenciosa |

✅ afectada · ⚠️ en riesgo o no verificable · — no afectada

---

## 9. Errores frecuentes al clasificar

> [!WARNING]
> Estos cinco errores explican la mayoría de las respuestas mal calificadas en el análisis de casos.

**1. Confundir el vector con la propiedad.** «Fue un phishing» describe **cómo entraron**, no qué rompieron. El phishing es un medio; la propiedad afectada depende de qué hicieron después.

**2. Dar por hecho que ransomware = disponibilidad y nada más.** Desde hace años, la mayoría de los grupos de ransomware roban los datos **antes** de cifrarlos. Salvo que haya evidencia de lo contrario, un caso de ransomware afecta también la confidencialidad.

**3. Olvidar la integridad después de una intrusión prolongada.** Si alguien estuvo semanas dentro del sistema con permisos de administrador, no se puede afirmar que los datos siguen siendo exactos: hay que **verificarlo**. «No hay evidencia de alteración» no es lo mismo que «no hubo alteración».

**4. Clasificar por la gravedad aparente en vez de por lo ocurrido.** Que un incidente sea escandaloso no lo hace afectar las tres propiedades. Rigor: se marca lo que la evidencia sostiene.

**5. Confundir «no puedo entrar a mi cuenta» con pérdida de disponibilidad del sistema.** Si te bloquearon la cuenta porque erraste la contraseña, el sistema está perfectamente disponible. La disponibilidad se define **para el usuario autorizado**.

---

## 10. Para la próxima clase

📝 **Actividad:** [Clasificación CID y diseño de controles](../actividades/02-clasificacion-cid.md) — trabajo en grupos con defensa oral.

✅ **Autoevaluación:** [Poné a prueba lo que leíste](../autoevaluacion/02-triada-cid.md) — 10 preguntas con respuestas explicadas.

📖 **Consulta:** [Glosario](../recursos/glosario.md) · [Bibliografía](../recursos/bibliografia.md) · [Normativa uruguaya](../recursos/normativa-uruguay.md)

🔭 **Lo que viene:** en la clase 3 pasamos de *qué* proteger a *cómo se diseña* la protección: defensa en profundidad, mínimo privilegio, fallo seguro y políticas de seguridad.

---

### Fuentes de los casos

- El Observador (2025). *BHU, Masonería y Ceibal: estos fueron los ciberataques más graves en Uruguay en 2025.* <https://www.elobservador.com.uy/ciencia-y-tecnologia/bhu-masoneria-y-ceibal-estos-fueron-los-ciberataques-mas-graves-uruguay-2025-n6028983>
- Infobae (8 de abril de 2025). *Más de 37 mil documentos filtrados en un nuevo ciberataque a un ministerio en Uruguay.* <https://www.infobae.com/america/america-latina/2025/04/08/mas-de-37-mil-documentos-filtrados-en-un-nuevo-ciberataque-a-un-ministerio-en-uruguay/>
- CERTuy / Agesic. Informe de ciberincidentes, primer semestre 2025. <https://www.gub.uy/centro-nacional-respuesta-incidentes-seguridad-informatica/>
- Wikipedia. *Ciberataque al Gobierno de Costa Rica.* <https://es.wikipedia.org/wiki/Ciberataque_al_Gobierno_de_Costa_Rica>
- Wikipedia. *Incidente de CrowdStrike de 2024.* <https://es.wikipedia.org/wiki/Incidente_de_CrowdStrike_de_2024>
- Wikipedia. *Bangladesh Bank robbery.* <https://en.wikipedia.org/wiki/Bangladesh_Bank_robbery>

> [!NOTE]
> Las cifras de los casos provienen de fuentes periodísticas y de informes públicos, y pueden ajustarse a medida que avanzan las investigaciones. Parte del oficio consiste en citar la fuente y la fecha de consulta, como se pide en las actividades.

---

[⬅ Volver al índice del curso](../README.md)
