# Glosario

Términos que se usan a lo largo del curso. Se va ampliando clase a clase.

> [!TIP]
> En la computadora, usá `Ctrl + F` (o `Cmd + F` en Mac) para buscar un término. En el celular, el buscador está en el menú del navegador.

---

## A

**Aceptabilidad psicológica** — Principio de diseño según el cual un control que las personas no pueden cumplir se termina esquivando, y por lo tanto no protege.

**Activo** — Todo aquello que tiene valor para la organización y por lo tanto requiere protección: datos, equipos, software, personas, reputación.

**Adware** — Malware que inyecta publicidad y perfila al usuario. Suele ser la puerta de entrada de algo peor.

**Agesic** — Agencia de Gobierno Electrónico y Sociedad de la Información y del Conocimiento. Organismo rector de la ciberseguridad en Uruguay; publica el Marco de Ciberseguridad.

**Amenaza** — Causa potencial de un incidente no deseado. Puede ser deliberada (un atacante), accidental (un error humano) o ambiental (una inundación).

**APT** (*Advanced Persistent Threat*) — Amenaza persistente avanzada: atacante con muchos recursos que entra y se queda meses o años sin hacerse notar, con un objetivo concreto (espionaje o sabotaje).

**Autenticidad** — Propiedad que garantiza que quien dice ser el emisor de una información efectivamente lo es.

## B

**Baiting** — Técnica de ingeniería social: dejar un cebo tentador (un pendrive «olvidado», una descarga) para que la víctima lo active.

**BEC** (*Business Email Compromise*) — Fraude que suplanta a una autoridad de la organización por correo para pedir una transferencia o una compra urgente. Se lo llama también «fraude del jefe».

**Botnet** — Red de equipos infectados y controlados a distancia, usada para atacar a terceros.

## C

**Cadena de suministro (ataque a la)** — Atacar a una organización comprometiendo a un proveedor en quien confía, por ejemplo insertando código en una actualización oficial.

**CERT / CSIRT** — Equipo de respuesta a incidentes de seguridad informática. El nacional de Uruguay es el **CERTuy**, que funciona en Agesic.

**CID** — Confidencialidad, Integridad y Disponibilidad: las tres propiedades que la seguridad de la información preserva. En inglés, *CIA triad*.

**CNA** (*CVE Numbering Authority*) — Organización autorizada a asignar identificadores CVE. Muchos fabricantes son CNA de sus propios productos.

**Confianza cero** (*zero trust*) — Arquitectura que no considera confiable a nadie por estar dentro de la red: verifica cada acceso de forma explícita, aplica mínimo privilegio y asume que el atacante ya entró.

**Confidencialidad** — Propiedad que garantiza que la información solo sea accesible para quien está autorizado.

**Control** — Medida que modifica el riesgo. Puede ser técnica, organizativa, física o de personal.

**Credenciales por defecto** — Usuario y contraseña con los que un equipo o programa viene de fábrica (`admin`/`admin` y similares). Están publicadas en los manuales: dejarlas puestas es una de las vulnerabilidades más explotadas y de las más fáciles de eliminar.

**CVE** (*Common Vulnerabilities and Exposures*) — Identificador público y único de **una** vulnerabilidad concreta en un producto concreto. Formato `CVE-año-número`, por ejemplo `CVE-2021-44228`. El año es el de reserva del identificador y el número es correlativo, sin significado propio.

**CVSS** (*Common Vulnerability Scoring System*) — Puntaje de **0,0 a 10,0** que mide la gravedad técnica de una vulnerabilidad. Bandas: 0,1–3,9 baja · 4,0–6,9 media · 7,0–8,9 alta · 9,0–10,0 crítica. Mide la falla en abstracto: **no es el riesgo**, porque no conoce el contexto de la organización.

**CWE** (*Common Weakness Enumeration*) — Catálogo de **tipos** de falla. Si el CVE es el caso, el CWE es la categoría: `CWE-79` scripting entre sitios, `CWE-89` inyección SQL, `CWE-798` credenciales embebidas en el código, `CWE-22` salto de directorio.

## D

**DDoS** (*Distributed Denial of Service*) — Denegación de servicio distribuida: el mismo ataque desde miles de equipos a la vez, normalmente una botnet.

**Defensa en profundidad** — Estrategia de superponer varias capas de control, de modo que el fallo de una no comprometa todo el sistema.

**Día cero** (*zero-day*) — Vulnerabilidad que el atacante conoce y el fabricante todavía no: **no existe parche**. Quien defiende tuvo cero días para prepararse.

**Diccionario (ataque de)** — Prueba de contraseñas a partir de listas de palabras y de contraseñas ya filtradas, aplicando además reglas de sustitución previsibles (`a`→`@`, `e`→`3`, agregar `123` al final). Es mucho más rápido que la fuerza bruta y por eso se prueba antes.

**Diseño abierto** — Principio según el cual la seguridad no puede depender de que el diseño sea secreto: lo único secreto debe ser la clave. Su opuesto es la *seguridad por oscuridad*.

**Disponibilidad** — Propiedad que garantiza que la información y los servicios estén accesibles cuando se los necesita.

**Doble extorsión** — Táctica del ransomware actual: primero se exfiltran los datos y después se cifran, para amenazar con publicarlos además de bloquearlos.

**DoS** (*Denial of Service*) — Ataque de denegación de servicio desde un solo origen: satura un servicio hasta dejarlo inaccesible.

## E

**Exfiltración** — Copia no autorizada de datos hacia afuera de la organización. En un ataque de ransomware ocurre **antes** del cifrado.

**Exploit** — Código o técnica que aprovecha una vulnerabilidad concreta.

## F

**Fuerza bruta** — Prueba sistemática de todas las combinaciones posibles hasta dar con la correcta. Es el último recurso de un atacante de contraseñas, porque es el más caro: primero prueba listas filtradas y diccionarios.

## G

**Gusano** (*worm*) — Malware que se propaga solo por la red, sin que nadie ejecute nada. Por eso produce contagios masivos en horas.

## H

**Hash** — Función de un solo sentido que transforma un dato de cualquier tamaño en una cadena de longitud fija. Se usa para almacenar contraseñas y para verificar integridad. No es cifrado: no se puede revertir.

## I

**Impacto** — Consecuencia adversa concreta si el incidente ocurre.

**Ingeniería social** — Conjunto de técnicas para manipular a una persona y lograr que entregue información, dé un acceso o haga algo que no debería. Incluye phishing, vishing, smishing, pretexting, baiting, tailgating, quid pro quo y shoulder surfing.

**Integridad** — Propiedad que garantiza que la información sea exacta y completa, y que no se modifique sin autorización.

## K

**Keylogger** — Spyware que registra todo lo que se teclea, incluidas las contraseñas en el momento en que se escriben.

## M

**Madurez** — Escala del 0 al 4 con la que el Marco de Ciberseguridad mide cuán consolidada está la seguridad de una organización: de «sin medidas» a «mejora continua».

**Malware** — Software malicioso. Categoría general que incluye virus, gusanos, troyanos, spyware y ransomware.

**Marco de Ciberseguridad** — Conjunto de requisitos de Agesic organizado en seis funciones —Gobernar, Identificar, Proteger, Detectar, Responder y Recuperar— con niveles de madurez y perfiles según la criticidad del organismo.

**Mediación completa** — Principio según el cual cada acceso debe verificarse todas las veces, y no solo al iniciar sesión.

**Mínimo mecanismo común** — Principio según el cual cuanto menos se comparta entre usuarios distintos, menos caminos hay para pasar de uno a otro.

**Mínimo privilegio** — Principio según el cual cada usuario o proceso recibe solo los permisos estrictamente necesarios para su función.

## N

**No repudio** — Propiedad que impide que quien realizó una acción pueda negar después haberla realizado.

**Norma** (o estándar) — Documento obligatorio y medible que baja una política a requisitos concretos. Su verbo característico es «debe».

**NVD** (*National Vulnerability Database*) — Base pública del NIST donde se consulta cada CVE: descripción, puntaje CVSS, tipo de CWE y versiones afectadas.

## P

**Parche** — Corrección que el fabricante publica para eliminar una vulnerabilidad. Que exista no alcanza: el riesgo baja recién cuando alguien lo instala.

**Phishing** — Engaño, generalmente por correo electrónico, para que la víctima entregue datos o credenciales o ejecute un archivo malicioso. Por voz se llama **vishing**; por SMS, **smishing**.

**Política de seguridad** — Documento aprobado por la máxima autoridad que establece qué se protege y por qué. En Uruguay es obligatoria para los organismos de la Administración Central desde el Decreto N.º 452/009.

**Predeterminados a prueba de fallos** (*fail-safe defaults*) — Principio según el cual, por omisión, hay que denegar: el acceso se concede explícitamente, no se recorta después.

**Pretexting** — Técnica de ingeniería social: inventar un pretexto creíble para pedir información o un acceso.

**Procedimiento** — Documento que describe paso a paso cómo se ejecuta una tarea. Lo escribe quien opera el sistema.

**Puerto** — Número que identifica un canal de comunicación en un equipo conectado a una red. **Cerrado**: no hay nadie del otro lado. **Abierto**: hay un servicio escuchando, es decir, un punto de entrada. Los habituales: `21` FTP, `22` SSH, `23` Telnet, `25` SMTP, `80` HTTP, `443` HTTPS, `3306` MySQL, `3389` RDP.

## Q

**Quid pro quo** — Técnica de ingeniería social: ofrecer algo a cambio de un dato o un acceso.

## R

**Ransomware** — Malware que cifra los archivos de la víctima y exige un pago para devolver el acceso. La defensa efectiva no es el antivirus sino la copia de seguridad desconectada y probada.

**Riesgo** — Combinación de la probabilidad de que una amenaza explote una vulnerabilidad y del impacto que eso produciría.

**Rootkit** — Malware que se esconde en lo profundo del sistema para que el equipo mienta sobre sí mismo. Puede pasar meses sin ser detectado.

## S

**Sal** (*salt*) — Valor al azar que se agrega a cada contraseña antes de calcular su hash. Hace que dos usuarios con la misma contraseña tengan hashes distintos, e inutiliza las tablas de hashes precalculadas.

**Saltzer y Schroeder** — Autores del artículo de 1975 que enunció los ocho principios de diseño de la protección de la información, todavía vigentes.

**Separación de privilegios** — Principio según el cual una acción crítica debe requerir dos condiciones independientes, no una sola credencial.

**Servicio** — Programa que escucha en un puerto y atiende pedidos de la red (un servidor web, un servidor de base de datos, un servicio de escritorio remoto). Cada servicio expuesto, y sobre todo cada versión desactualizada de un servicio, es superficie de ataque.

**Shoulder surfing** — Espiar por encima del hombro una contraseña, un PIN o una pantalla.

**Smishing** — Phishing por SMS o mensajería.

**Spear phishing** — Phishing dirigido a una persona concreta, usando su nombre, su cargo y su contexto.

**Spyware** — Malware que espía en silencio: pantallas, hábitos, archivos.

**Superficie de ataque** — Conjunto de puntos por los que un atacante podría intentar entrar en un sistema. Se analiza en tres planos: **red** (equipos, puertos, servicios), **software** (aplicaciones y versiones) y **personas** (cuentas, permisos, accesos de terceros). Se reduce sacando, no agregando.

## T

**Tailgating** — Colarse a un lugar físico restringido detrás de una persona autorizada.

**Trazabilidad** (o rendición de cuentas) — Propiedad que garantiza que toda acción quede registrada y pueda atribuirse a un responsable.

**Troyano** — Malware que se hace pasar por un programa útil para que la víctima lo instale. No se propaga solo.

## V

**Ventana de exposición** — Tiempo que pasa entre que existe el parche de una vulnerabilidad y el momento en que se lo instala. Es la parte del problema que depende enteramente de la organización.

**Virus** — Malware que se pega a un archivo y necesita que alguien lo ejecute para propagarse.

**Vishing** — Phishing por llamada de voz.

**Vulnerabilidad** — Debilidad de un activo o de un control que puede ser explotada por una amenaza. Es la única de las cuatro piezas del riesgo —vulnerabilidad, amenaza, exploit e impacto— sobre la que se puede actuar directamente.

---

[⬅ Índice del curso](../README.md)
