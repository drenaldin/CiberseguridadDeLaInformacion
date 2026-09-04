# Clase 5 · Amenazas II: ingeniería social, denegación de servicio y APT

> Tres amenazas que no se parecen en nada: una engaña a la persona, otra tira abajo el servicio, la tercera se queda adentro y no hace ruido

**Contenidos del programa:** 1.5 Principales amenazas en el entorno digital
**Competencia:** CET1
**Tiempo de lectura:** unos 25 minutos

---

📥 **Presentación de la clase:** [Clase 5 · Amenazas II](../presentaciones/Clase5-Amenazas-II.pptx) — `.pptx`, se descarga con el botón **Download** que aparece arriba a la derecha al abrir el enlace.

---

> [!TIP]
> **Cómo usar este material.** La clase 4 vio malware, ransomware y phishing. Esta cierra el contenido 1.5 con las tres amenazas que faltaban, y están elegidas a propósito porque atacan cosas distintas: la **ingeniería social** ataca a la persona, la **denegación de servicio** ataca la disponibilidad, y la **APT** ataca sin que te des cuenta. Si podés nombrar qué propiedad de la tríada rompe cada una, entendiste la clase.

## Contenido

1. [Tres amenazas, tres blancos distintos](#1-tres-amenazas-tres-blancos-distintos)
2. [Ingeniería social: el eslabón humano](#2-ingeniería-social-el-eslabón-humano)
3. [Las técnicas clásicas](#3-las-técnicas-clásicas)
4. [Denegación de servicio: tirar abajo el servicio](#4-denegación-de-servicio-tirar-abajo-el-servicio)
5. [Cómo funciona un DDoS](#5-cómo-funciona-un-ddos)
6. [Los números del DDoS hoy](#6-los-números-del-ddos-hoy)
7. [APT: la amenaza que se queda](#7-apt-la-amenaza-que-se-queda)
8. [Dos APT que conviene conocer](#8-dos-apt-que-conviene-conocer)
9. [Cómo se defiende cada una](#9-cómo-se-defiende-cada-una)
10. [Errores frecuentes](#10-errores-frecuentes)
11. [Para la próxima clase](#11-para-la-próxima-clase)

---

## 1. Tres amenazas, tres blancos distintos

Las amenazas de la clase 4 tenían algo en común: casi todas buscaban plata rápido. Las de hoy no se parecen entre sí, y por eso se estudian juntas: cada una ataca una parte distinta del sistema.

| Amenaza | A qué le apunta | Qué propiedad rompe | En una frase |
|---|---|---|---|
| **Ingeniería social** | A la persona | Depende de qué logre después | «No hackearon el sistema, hackearon a alguien» |
| **Denegación de servicio** | Al servicio | **Disponibilidad** | «El sitio se cayó por saturación» |
| **APT** | A la organización entera, en silencio | Sobre todo **confidencialidad** e **integridad** | «Estuvieron adentro meses sin que nadie lo supiera» |

> [!NOTE]
> Fijate que la ingeniería social es transversal: casi nunca es el ataque completo, es la **puerta de entrada** de otro. El caso de C&M de la clase 4 —comprarle la contraseña a un empleado— fue ingeniería social pura, y terminó en un fraude de 541 millones.

---

## 2. Ingeniería social: el eslabón humano

> **Definición.** Conjunto de técnicas para manipular a una persona y lograr que entregue información, dé un acceso o realice una acción que no debería.

No ataca la máquina: ataca la confianza, el apuro, el miedo o las ganas de ayudar. Por eso ningún antivirus la detiene, y por eso es tan eficaz.

El dato que lo resume: según el informe anual de investigación de brechas de Verizon (2025), el **factor humano estuvo involucrado en alrededor del 60 % de las brechas**. La tecnología mejoró mucho más rápido que nuestra capacidad de no hacer clic.

> [!IMPORTANT]
> La ingeniería social es la razón por la que este curso insiste tanto en la **cultura de reportar sin miedo**. Si la puerta de entrada es una persona, esa misma persona tiene que poder avisar apenas sospecha, sin temor a la sanción. Una organización que reta al que cae, se queda sin su mejor sensor.

---

## 3. Las técnicas clásicas

Todas son viejas —algunas anteriores a internet— y todas siguen funcionando. Conviene reconocerlas por su nombre:

| Técnica | Qué es | Ejemplo en el liceo |
|---|---|---|
| **Pretexting** | Inventar un pretexto creíble para pedir algo. | «Hola, soy del soporte de la ANEP, necesito tu usuario para arreglar el sistema.» |
| **Baiting** (cebo) | Dejar algo tentador para que la víctima lo active. | Un pendrive «olvidado» en el patio con la etiqueta «Notas 2026». |
| **Tailgating** (colarse) | Entrar detrás de alguien autorizado. | Pasar a la sala de servidores atrás de un profe, sosteniéndole la puerta. |
| **Quid pro quo** | Ofrecer algo a cambio del dato o el acceso. | «Te instalo juegos gratis si me pasás tu contraseña.» |
| **Shoulder surfing** | Espiar por encima del hombro. | Mirar el PIN o la contraseña de un compañero mientras la escribe. |

> [!TIP]
> Las cinco tienen una defensa común y aburrida: **verificar antes de actuar**. Ante un pedido inesperado —por más creíble que suene— se confirma por otro canal. Es la misma regla que vimos contra el phishing en la clase 4.

---

## 4. Denegación de servicio: tirar abajo el servicio

> **Definición.** Ataque que busca que un servicio deje de estar disponible para sus usuarios legítimos, generalmente saturándolo de pedidos.

Acá no roban ni modifican nada: **rompen la disponibilidad**, la tercera propiedad de la tríada. El servicio sigue existiendo, los datos están intactos, pero nadie puede usarlo.

Hay dos siglas que conviene separar:

- **DoS** (*Denial of Service*): el ataque sale de **un solo origen**. Es más fácil de bloquear: se corta esa fuente.
- **DDoS** (*Distributed Denial of Service*): el ataque sale de **miles de equipos a la vez**. Es lo que se ve hoy, y es mucho más difícil de frenar, porque no hay una sola fuente que cortar.

> [!NOTE]
> ¿De dónde salen esos miles de equipos? De una **botnet** —la vimos en la clase 4—: computadoras, cámaras y routers infectados que obedecen a un atacante sin que sus dueños lo sepan. Tu equipo puede estar atacando a otro país ahora mismo y vos sin enterarte.

---

## 5. Cómo funciona un DDoS

La idea es simple y por eso es difícil de defender. Un servidor puede atender, digamos, mil pedidos por segundo. Si le llegan un millón, colapsa: los pedidos legítimos quedan mezclados entre los falsos y nadie entra.

```
        Sin ataque                         Con DDoS

  usuario  →  [ servidor ]         usuario  →  ✕
  usuario  →  [  atiende  ]        (botnet) →→→→→ [ servidor ]
  usuario  →  [   bien    ]        miles    →→→→→ [ saturado  ]
                                   de bots  →→→→→ [  no da    ]
                                   usuario  →  ✕  abasto
```

El atacante no necesita romper nada: le alcanza con **pedir de más**. Por eso la defensa no es un antivirus, sino filtrar el tráfico antes de que llegue al servidor, y tener capacidad de sobra para absorber el golpe. Es un problema de escala, no de cerradura.

> **Analogía.** Un DDoS es como mandar diez mil personas a hacer cola en una panadería de barrio, todas pidiendo la hora. No roban el pan ni rompen la vidriera: simplemente, quien quiere comprar de verdad no llega al mostrador.

---

## 6. Los números del DDoS hoy

Las cifras son enormes y crecen todos los años. Estas vienen de Cloudflare, una de las empresas que mitiga estos ataques —o sea, **dato de proveedor**, útil para dimensionar, no como estadística neutral:

| Dato | Cifra | Cuándo |
|---|---|---|
| Ataque DDoS más grande registrado | **31,4 Tbps**, durante 35 segundos | noviembre de 2025 |
| Récords anteriores del mismo año | 11,5 y luego 22,2 Tbps | 2025 |

Para tener una idea: 31,4 Tbps es muchísimo más de lo que consume un país entero en tráfico normal, concentrado sobre un solo objetivo durante medio minuto.

**El caso para analizar: Internet Archive (octubre de 2024).** El sitio que guarda copias históricas de la web —la Wayback Machine— sufrió un DDoS reivindicado por un grupo llamado SN_BLACKMETA, que coincidió además con una filtración de 31 millones de cuentas. La organización tuvo que **ponerlo fuera de línea** para reforzar las defensas y lo fue reabriendo por etapas: primero en modo solo lectura, y recién a los días volvió a funcionar completo.

> **Lo que enseña este caso:** contra un DDoS, a veces la mejor respuesta es apagar el servicio a propósito para defenderlo, aunque duela. Y que un DDoS puede ser la cortina de humo de otro ataque que pasa al mismo tiempo.

---

## 7. APT: la amenaza que se queda

> **Definición.** *Advanced Persistent Threat* (amenaza persistente avanzada): un atacante con muchos recursos que entra en una organización y se queda adentro mucho tiempo, sin hacerse notar, con un objetivo concreto.

Es la más distinta de las tres, y la que menos se parece a lo que uno imagina de un «hacker». Se define por cuatro rasgos, y hacen falta los cuatro:

| Rasgo | Qué significa |
|---|---|
| **Avanzada** | Usa técnicas sofisticadas y a veces vulnerabilidades desconocidas (día cero). |
| **Persistente** | No entra y sale: se queda **meses o años**. |
| **Con recursos** | Detrás suele haber un Estado o un grupo grande, no una persona sola. |
| **Dirigida** | Va por un objetivo específico —espionaje, sabotaje—, no por plata rápida. |

> [!CAUTION]
> La APT es a la ciberseguridad lo que la integridad es a la tríada: **la amenaza silenciosa**. No busca que te des cuenta. Un ransomware te avisa con un cartel en pantalla; una APT hace exactamente lo contrario. Por eso los casos que conocemos se descubrieron, en general, mucho después de que empezaron.

---

## 8. Dos APT que conviene conocer

Los dos están documentados y son los que más se citan en cualquier curso:

### Stuxnet (descubierto en 2010)

Un gusano diseñado para sabotear un tipo muy específico de equipo industrial: los controladores que hacían girar las centrifugadoras de un programa nuclear en Irán. No robaba datos ni pedía rescate: hacía que las máquinas se rompieran solas mientras los tableros mostraban que todo andaba bien. Es el ejemplo clásico de un ataque **dirigido, sofisticado y sigiloso**, y de que un ciberataque puede tener efectos físicos en el mundo real.

### SolarWinds (descubierto en 2020)

Los atacantes no entraron a cada víctima una por una: comprometieron a **un proveedor** de software de gestión de redes e insertaron código malicioso en una actualización oficial. Cuando miles de organizaciones —incluidas agencias de gobierno de EE. UU.— instalaron esa actualización de confianza, les abrieron la puerta sin saberlo. Estuvieron adentro **meses** antes de que alguien lo detectara.

> **Lo que enseñan estos casos:** el de SolarWinds es un **ataque a la cadena de suministro** —te atacan a través de alguien en quien confiás— y conecta con lo que vimos en la clase 3: la superficie de ataque incluye a tus proveedores. El eslabón más débil puede no estar en tu propia red.

---

## 9. Cómo se defiende cada una

No hay una defensa única, porque son tres problemas distintos. Pero cada una tiene sus controles típicos, y todos ya aparecieron en clases anteriores:

| Amenaza | Defensas principales |
|---|---|
| **Ingeniería social** | Formación de las personas · verificar por otro canal · segundo factor · cultura de reportar sin miedo · mínimo privilegio (que una credencial engañada haga poco daño). |
| **DoS / DDoS** | Filtrado de tráfico y servicios de mitigación · capacidad de sobra · planes de continuidad · detección temprana del pico de tráfico. |
| **APT** | Segmentación de la red · registros de auditoría que alguien mire · detección de comportamiento anómalo · cuidar la cadena de suministro · asumir la brecha (confianza cero). |

> [!TIP]
> Mirá la columna de la derecha: mínimo privilegio, segmentación, registros, confianza cero, reportar sin miedo. Son los principios de las clases 2 y 3 aplicados. No hay controles nuevos y mágicos: hay los mismos de siempre, bien puestos.

---

## 10. Errores frecuentes

> [!WARNING]
> Estos cuatro son los que más se repiten al clasificar estas amenazas.

**1. Confundir DoS con robo de datos.** Un DDoS **no** roba ni modifica nada: rompe la disponibilidad y nada más. Salvo que, como en Internet Archive, sea la cortina de humo de otra cosa.

**2. Creer que la ingeniería social es «para tontos».** Cae gente muy preparada, porque no ataca la inteligencia: ataca el apuro, la confianza y el momento de distracción. Suponer que a uno no le va a pasar es, justamente, la actitud que la hace funcionar.

**3. Pensar que una APT es un virus más.** Lo que la define no es el malware que usa, sino la **persistencia y el sigilo**. Puede usar herramientas simples; lo sofisticado es la operación, no el bicho.

**4. Creer que un DDoS «se arregla con un antivirus».** No hay nada que desinfectar en tu equipo: el problema es el volumen de tráfico que llega de afuera. Se defiende con filtrado y escala, no con software en la máquina atacada.

---

## 11. Para la próxima clase

📝 **Actividad:** [Tres amenazas, tres defensas](../actividades/05-tres-amenazas.md) — en grupos, análisis de casos y diseño de la respuesta.

✅ **Autoevaluación:** [Poné a prueba lo que leíste](../autoevaluacion/05-amenazas-ingenieria-dos-apt.md) — 10 preguntas con respuestas explicadas.

📥 **Presentación:** [Clase 5 · Amenazas II](../presentaciones/Clase5-Amenazas-II.pptx) — la misma que se usó en clase, con diagramas y notas del docente.

📖 **Consulta:** [Glosario](../recursos/glosario.md) · [Bibliografía](../recursos/bibliografia.md) · [Normativa uruguaya](../recursos/normativa-uruguay.md)

🔭 **Lo que viene:** con la clase 5 se cierra el panorama de amenazas (contenido 1.5). En la clase 6 pasamos a las vulnerabilidades y la superficie de ataque, y ahí llega la **primera evaluación formativa** del curso.

> **Para pensar antes de la próxima clase.** De las tres amenazas de hoy, ¿cuál te parece más difícil de defender en un liceo, y por qué? No hay respuesta única; hay respuesta fundamentada.

---

### Fuentes

- Verizon. *Data Breach Investigations Report 2025* — factor humano en ~60 % de las brechas. <https://www.verizon.com/business/resources/reports/dbir/>
- Cloudflare. *DDoS Threat Report, Q4 2025* — ataque récord de 31,4 Tbps. **Dato de proveedor.** <https://blog.cloudflare.com/>
- Internet Archive cyberattack (octubre de 2024). <https://en.wikipedia.org/wiki/Internet_Archive_cyberattack>
- Stuxnet. <https://en.wikipedia.org/wiki/Stuxnet>
- SolarWinds / SUNBURST — CISA, actividad APT en la cadena de suministro. <https://www.cisa.gov/>
- CERTuy / AGESIC — informes de incidentes de seguridad de la información. <https://www.gub.uy/centro-nacional-respuesta-incidentes-seguridad-informatica/>

> [!NOTE]
> Las cifras de DDoS provienen de la empresa que mitiga los ataques y las publica: sirven para dimensionar la magnitud, pero conviene decir de dónde salen. AGESIC informa incidentes en Uruguay de forma agregada y no desglosa DoS por separado, así que no hay un número nacional de DDoS para citar. Distinguir el origen de cada dato es parte del oficio.

---

[⬅ Volver al índice del curso](../README.md)
