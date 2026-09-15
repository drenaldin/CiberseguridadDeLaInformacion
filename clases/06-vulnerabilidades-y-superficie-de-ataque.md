# Clase 6 · Vulnerabilidades y superficie de ataque

> Las clases 4 y 5 vieron quién ataca. Esta ve **por dónde**: la puerta que dejaste abierta, la que no sabías que existía y la que cierra con una contraseña de ocho letras

**Contenidos del programa:** 1.3 Seguridad de redes, sistemas, aplicaciones y datos
**Competencia:** CET1
**Tiempo de lectura:** unos 30 minutos

---

📥 **Presentación de la clase:** [Clase 6 · Vulnerabilidades y superficie de ataque](../presentaciones/Clase6-Vulnerabilidades-y-Superficie-de-Ataque.pptx) — `.pptx`, se descarga con el botón **Download** que aparece arriba a la derecha al abrir el enlace.

---

> [!TIP]
> **Cómo usar este material.** Esta clase es más técnica que las anteriores: tiene identificadores, puertos, puntajes y una cuenta con potencias. No hay que memorizar nada de eso. Hay que saber **leerlo**: si te muestran `CVE-2021-44228`, `tcp/3389 abierto` o `CVSS 9,8`, tenés que poder decir qué significa y qué harías con esa información.

## Contenido

1. [Amenaza, vulnerabilidad, exploit y riesgo](#1-amenaza-vulnerabilidad-exploit-y-riesgo)
2. [De dónde salen las vulnerabilidades](#2-de-dónde-salen-las-vulnerabilidades)
3. [Cómo se nombran: CVE y CWE](#3-cómo-se-nombran-cve-y-cwe)
4. [Cuánto de grave: el puntaje CVSS](#4-cuánto-de-grave-el-puntaje-cvss)
5. [El puntaje no es el riesgo](#5-el-puntaje-no-es-el-riesgo)
6. [Día cero y ventana de exposición](#6-día-cero-y-ventana-de-exposición)
7. [Superficie de ataque: los tres planos](#7-superficie-de-ataque-los-tres-planos)
8. [Puertos y servicios](#8-puertos-y-servicios)
9. [Cómo se enumera y cómo se reduce](#9-cómo-se-enumera-y-cómo-se-reduce)
10. [La contraseña como vulnerabilidad](#10-la-contraseña-como-vulnerabilidad)
11. [Por qué el largo le gana a la complejidad](#11-por-qué-el-largo-le-gana-a-la-complejidad)
12. [Qué dice el estándar](#12-qué-dice-el-estándar)
13. [Errores frecuentes](#13-errores-frecuentes)
14. [Para la próxima clase](#14-para-la-próxima-clase)

---

## 1. Amenaza, vulnerabilidad, exploit y riesgo

Se usan como sinónimos todo el tiempo y no lo son. Separarlos es lo primero.

| Palabra | Qué es | En la puerta de tu casa |
|---|---|---|
| **Vulnerabilidad** | Una debilidad **del sistema**. Está ahí aunque nadie la mire. | La cerradura floja. |
| **Amenaza** | Quien o qué podría aprovecharla. | El que anda robando en el barrio. |
| **Exploit** | El procedimiento o el programa concreto que la aprovecha. | La ganzúa que abre justo esa cerradura. |
| **Riesgo** | Qué tan probable es que pase y cuánto duele si pasa. | Depende de si adentro hay una bicicleta o nada. |

> [!IMPORTANT]
> De las cuatro, la **vulnerabilidad es la única sobre la que podés actuar directamente**. No elegís quién te ataca ni cuándo. Sí elegís qué dejás abierto. Por eso el trabajo de defensa empieza acá.

Una forma corta de decirlo: **riesgo = vulnerabilidad × amenaza × impacto**. Si cualquiera de los tres es cero, el riesgo es cero. Una falla gravísima en un programa que nadie instaló no le hace daño a nadie.

---

## 2. De dónde salen las vulnerabilidades

No todas son errores de programación. Son cinco orígenes, y los tres primeros explican la enorme mayoría de los incidentes reales:

| Origen | Qué pasó | Ejemplo concreto |
|---|---|---|
| **Error de programación** | El código no controla lo que recibe. | Un formulario que acepta cualquier texto y se lo pasa tal cual a la base de datos. |
| **Mala configuración** | El software está bien, la instalación no. | Un router con el usuario `admin` y la contraseña `admin` que vinieron de fábrica. |
| **Software sin actualizar** | La falla ya se conoce y ya tiene arreglo, pero nadie lo instaló. | Un servidor con una versión de hace tres años. |
| **Decisión de diseño** | El protocolo es inseguro **por cómo fue pensado**. | Telnet y FTP mandan la contraseña en texto claro: no es una falla, es su diseño. |
| **Personas y procesos** | La debilidad está en cómo se administra. | La cuenta de un profesor que se fue el año pasado y sigue activa. |

> [!NOTE]
> Fijate que solo el primero es «culpa del programador». Los otros cuatro son de quien administra el sistema. Es el mismo punto de la clase 3: **la superficie de ataque se reduce sacando**, y casi todo lo que hay para sacar no lo puso un atacante, lo dejamos nosotros.

---

## 3. Cómo se nombran: CVE y CWE

Cuando se descubre una vulnerabilidad concreta en un producto concreto, se le da un identificador público y único: un **CVE**.

```
CVE - 2021 - 44228
 |      |      |
 |      |      └─ número correlativo, sin significado
 |      └──────── año en que se reservó el identificador
 └─────────────── Common Vulnerabilities and Exposures
```

- Lo asigna el **Programa CVE** a través de organizaciones autorizadas (las llaman CNA). Muchas empresas son CNA de sus propios productos: Microsoft le pone el número a las fallas de Windows.
- Se consulta en la **NVD** (*National Vulnerability Database*, `nvd.nist.gov`): ahí está la descripción, el puntaje y la lista de versiones afectadas.
- Que exista el CVE significa que la falla es **pública**. La conoce el que defiende y la conoce el que ataca.

**El CWE es otra cosa: es el _tipo_ de falla.** Si el CVE es «esta falla, en este producto, en esta versión», el CWE es la familia a la que pertenece:

| Identificador | Qué nombra | Ejemplo |
|---|---|---|
| **CWE-79** | Cross-site scripting (XSS) | La página muestra sin filtrar lo que escribió un usuario. |
| **CWE-89** | Inyección SQL | El formulario deja escribir órdenes directo en la base de datos. |
| **CWE-798** | Credenciales embebidas en el código | El programa trae una contraseña escrita adentro que no se puede cambiar. |
| **CWE-22** | Salto de directorio | Pidiendo `../../` se llega a archivos que no correspondían. |

> [!TIP]
> Regla para no confundirlos: **el CVE es el caso, el CWE es la categoría**. Miles de CVE distintos pueden ser todos CWE-89. Cuando leas un aviso de seguridad, el CVE te dice *si te afecta*; el CWE te dice *qué clase de error fue* y si lo vas a volver a cometer.

**Para dimensionar:** en 2025 se publicaron más de **48.000 CVE**, un promedio de unos **130 por día**. Nadie los parchea todos. De ahí sale el problema central de esta clase: **priorizar**.

---

## 4. Cuánto de grave: el puntaje CVSS

El **CVSS** (*Common Vulnerability Scoring System*) le pone a cada vulnerabilidad un número de **0,0 a 10,0**, y ese número cae en una de cuatro bandas:

| Puntaje | Severidad |
|---|---|
| 0,1 – 3,9 | Baja |
| 4,0 – 6,9 | Media |
| 7,0 – 8,9 | Alta |
| 9,0 – 10,0 | **Crítica** |

El número no sale de una opinión: sale de contestar unas pocas preguntas sobre cómo se explota la falla y qué rompe.

| Pregunta | El puntaje **sube** si… |
|---|---|
| ¿Desde dónde se explota? | Desde internet, sin estar en la red local. |
| ¿Es difícil de lograr? | Sale siempre, al primer intento, sin condiciones raras. |
| ¿Hace falta una cuenta? | No hace falta ninguna. |
| ¿La víctima tiene que hacer algo? | No: se explota sola, sin que nadie haga clic. |
| ¿Qué rompe? | Cuanto más rompe de la **tríada CID**, más alto. |

Fijate la última fila: el CVSS termina midiendo **daño a confidencialidad, integridad y disponibilidad**. Es la clase 2 convertida en número.

Ese conjunto de respuestas se escribe en una línea, el **vector**, que se lee de a pares:

```
CVSS:3.1 / AV:N / AC:L / PR:N / UI:N / C:H / I:H / A:H   →  9,8  CRÍTICA
           ↓      ↓      ↓      ↓      ↓     ↓     ↓
      por red  fácil   sin    sin   rompe rompe rompe
                     cuenta  clic    C     I     A
```

Un ejemplo real de los peores posibles: **CVE-2021-44228**, conocido como *Log4Shell*, una falla en una biblioteca de registro de Java que usaba medio mundo. Se explotaba por red, sin credenciales, sin interacción, y permitía ejecutar código en el servidor. Puntaje: **10,0**.

---

## 5. El puntaje no es el riesgo

Este es el punto más importante de la clase, y el que más se confunde.

> [!CAUTION]
> El CVSS mide **qué tan grave es la falla en abstracto**. No sabe nada de tu organización. No sabe si esa máquina está prendida, si tiene red, ni qué guarda adentro.

Dos casos del mismo liceo:

| | Servidor de la sala de informática | Sitio web de inscripciones |
|---|---|---|
| Vulnerabilidad | Crítica, **9,8** | Media, **6,5** |
| ¿Se llega desde internet? | No, solo desde la red interna | **Sí, desde cualquier lado** |
| ¿Hay exploit dando vueltas? | No se conoce | **Sí, publicado y funcionando** |
| ¿Qué guarda? | Copias de instaladores | **Datos de estudiantes** |

La de 9,8 es más grave *como falla*. La de 6,5 es más urgente *como riesgo*. **Se parchea primero la de 6,5.**

Poner el contexto es trabajo humano: el puntaje te lo dan hecho, la prioridad la ponés vos. Eso es exactamente lo que vas a practicar en la actividad de esta clase.

---

## 6. Día cero y ventana de exposición

La vida de una vulnerabilidad tiene etapas, y en cada una cambia quién tiene ventaja:

```
1. La falla existe            → nadie la conoce. Estás expuesto sin saberlo.
2. Alguien la descubre        → si es un atacante: DÍA CERO. No hay parche. No hay defensa específica.
3. Se publica el CVE          → ahora la conocen todos, los dos bandos.
4. Sale el parche             → existe el arreglo.
5. Alguien lo instala         → ← ACÁ DEPENDE DE VOS
       ╰─ entre el paso 4 y el 5 está la VENTANA DE EXPOSICIÓN
```

**Día cero** (*zero-day*) significa que quien defiende tuvo **cero días** para prepararse: el atacante la conoce y el fabricante todavía no.

> [!IMPORTANT]
> Suena impresionante, pero es la excepción. La enorme mayoría de los ataques reales **no usa días cero**: usa fallas viejas, publicadas hace meses o años, con parche disponible, en sistemas que nadie actualizó. Un día cero es caro y escaso. Un servidor sin actualizar desde 2023 es gratis.

De ahí sale la medida de seguridad más aburrida y más efectiva que existe: **actualizar**.

---

## 7. Superficie de ataque: los tres planos

> **Definición.** La superficie de ataque es el conjunto de **todos los puntos por los que alguien podría intentar entrar** a un sistema.

En la clase 3 la vimos como concepto. Acá la vamos a enumerar de verdad, y para eso conviene partirla en tres planos:

| Plano | Qué se cuenta | Pregunta que lo abre |
|---|---|---|
| **Red** | Equipos, puertos abiertos, servicios escuchando, wifi. | ¿Qué está escuchando, y desde dónde se lo alcanza? |
| **Software** | Sistemas operativos, aplicaciones, versiones, plugins. | ¿Qué está instalado y **en qué versión exacta**? |
| **Personas** | Cuentas, permisos, accesos de terceros y proveedores. | ¿Quién puede entrar, y quién ya no debería? |

> [!NOTE]
> El plano que más se olvida es el tercero. Una cuenta activa de alguien que no trabaja más ahí es una puerta abierta que ningún escáner de red va a marcar en rojo.

Y ojo con la idea de que sin internet no hay superficie: el pendrive del patio de la clase 5 entró por el plano físico, no por la red.

---

## 8. Puertos y servicios

Un equipo conectado a una red no tiene una puerta: tiene miles, numeradas. Se llaman **puertos**. Casi todas están cerradas; las pocas abiertas son las que interesan.

- **Puerto cerrado** → no hay nadie del otro lado. No es un punto de entrada.
- **Puerto abierto** → hay un **servicio** escuchando. Es una puerta con alguien atendiendo.
- **Puerto abierto + versión vieja** → quien atiende tiene una falla conocida y publicada.

Los que conviene reconocer de memoria:

| Puerto | Servicio | Qué hace | A qué prestarle atención |
|---|---|---|---|
| `21/tcp` | FTP | Transferencia de archivos | Manda usuario y contraseña **en texto claro**. |
| `22/tcp` | SSH | Consola remota, cifrada | El cifrado está bien; el problema suele ser la contraseña. |
| `23/tcp` | Telnet | Consola remota, **sin cifrar** | No debería estar abierto nunca. |
| `25/tcp` | SMTP | Envío de correo | Mal configurado, sirve para mandar spam a nombre tuyo. |
| `80/tcp` | HTTP | Web sin cifrar | Todo viaja legible. |
| `443/tcp` | HTTPS | Web cifrada | Lo correcto hoy. |
| `3306/tcp` | MySQL | Base de datos | **Nunca** debería verse desde internet. |
| `3389/tcp` | RDP | Escritorio remoto de Windows | Blanco clásico de fuerza bruta contra contraseñas. |

> [!TIP]
> Mirá la columna de la derecha: `21`, `23` y `80` tienen todos el mismo problema —no cifran— y los tres tienen reemplazo directo: `22`, `22` y `443`. Media reducción de superficie de ataque es simplemente dejar de usar lo viejo.

---

## 9. Cómo se enumera y cómo se reduce

Enumerar la superficie de ataque es un procedimiento en cuatro pasos, **en este orden**:

1. **Inventario.** ¿Qué equipos hay? No se puede proteger lo que no se sabe que existe.
2. **Servicios.** ¿Qué puertos tiene abierto cada equipo y qué escucha en cada uno?
3. **Versiones.** ¿Qué versión exacta corre cada servicio? Sin la versión no hay diagnóstico.
4. **Vulnerabilidades conocidas.** Buscar esas versiones en la NVD y ver qué CVE les corresponden.

Recién después se prioriza, con el criterio de la sección 5: gravedad **por** contexto.

Para reducirla no hay que agregar nada. Hay que **sacar**:

| Medida | Qué elimina |
|---|---|
| Cerrar los puertos que no se usan | Puertas que no hacían falta. |
| Desinstalar el software que no se usa | Todas sus fallas futuras, de una. |
| Actualizar lo que sí se usa | La ventana de exposición. |
| Cambiar credenciales de fábrica | La entrada más fácil que existe. |
| Separar la red en partes | Que entrar en un equipo no sea entrar en todos. |
| Dar de baja cuentas que sobran | Accesos sin dueño. |

> [!WARNING]
> **Escanear un sistema ajeno sin autorización es delito en Uruguay**, aunque no rompas ni te lleves nada: Ley N.º 20.327, art. 297 BIS del Código Penal. No existe la excusa de «estaba probando». Todo lo que se escanea en este curso es un entorno preparado para eso y con autorización escrita. Está en el [Acuerdo de uso responsable](../CODE_OF_CONDUCT.md) que firmamos.

---

## 10. La contraseña como vulnerabilidad

Una contraseña débil no es un descuido del usuario: es **una vulnerabilidad del sistema**, y se enumera igual que un puerto abierto. Entra en esta clase por eso.

**Cómo se guarda.** Un sistema bien hecho **no guarda tu contraseña**. Guarda su **hash**: el resultado de pasarla por una función de un solo sentido, fácil de calcular para adelante e inviable de revertir.

```
  "montevideo"  ──[ función de hash ]──►  8f2a…c71b     fácil
  "montevideo"  ◄──────── ✗ ────────────  8f2a…c71b     inviable
```

Por eso un sitio serio nunca puede decirte cuál era tu contraseña: solo puede calcular el hash de lo que escribís y compararlo con el que tiene guardado. **Si un sitio te manda tu contraseña por mail, la está guardando mal.**

**Cómo se rompe.** El atacante roba la tabla de hashes y prueba candidatos **por su cuenta**, en su propia máquina, sin límite de intentos y sin que nadie lo vea. Y no empieza por el principio: prueba en este orden.

1. **Listas de contraseñas ya filtradas** de otras brechas. Es lo primero y lo más productivo.
2. **Diccionarios con reglas**: palabras comunes más las transformaciones obvias —`a`→`@`, `o`→`0`, `e`→`3`, agregar `123` o `!` al final, poner la primera en mayúscula.
3. **Fuerza bruta**, todas las combinaciones. Es el último recurso porque es el más caro.

> [!CAUTION]
> El paso 2 es el que sorprende. `S3gur!d@` **no** es una contraseña difícil: es la palabra «seguridad» con las cuatro sustituciones más previsibles del mundo. Los diccionarios de ataque las aplican solas. Complicarla así no la hace fuerte, la hace incómoda.

**El salt.** Para que dos usuarios con la misma contraseña no tengan el mismo hash, el sistema le agrega a cada una un valor al azar —el *salt*— antes de calcularlo. Así un atacante no puede reconocer contraseñas repetidas de un vistazo ni usar tablas de hashes precalculadas: tiene que atacar cada cuenta por separado.

---

## 11. Por qué el largo le gana a la complejidad

Acá no hace falta creer en nadie: es una cuenta.

La cantidad de combinaciones posibles de una contraseña **al azar** es **(símbolos disponibles) elevado a (largo)**. Supongamos un atacante con una placa de video que prueba **10.000 millones de candidatos por segundo** contra un hash rápido, trabajando offline:

| Contraseña | Cómo está armada | Combinaciones | Tiempo |
|---|---|---|---|
| `montevid` | 8 minúsculas | 26⁸ ≈ 2,1 × 10¹¹ | **21 segundos** |
| `Segurid1` | 8 alfanuméricas | 62⁸ ≈ 2,2 × 10¹⁴ | **6 horas** |
| `S3gur!d@` | 8 con símbolos | 95⁸ ≈ 6,6 × 10¹⁵ | **8 días** |
| `caballoverde` | 12 minúsculas | 26¹² ≈ 9,5 × 10¹⁶ | **110 días** |
| `tren lampara queso arena bicho` | 5 palabras al azar | 7776⁵ ≈ 2,8 × 10¹⁹ | **90 años** |

Leé la tabla de arriba abajo. **Doce letras minúsculas aguantan catorce veces más que ocho caracteres con símbolos**, y son infinitamente más fáciles de recordar. Cada carácter que agregás multiplica; cada símbolo raro que agregás apenas suma.

> [!IMPORTANT]
> Dos advertencias sobre esa tabla, y son importantes.
>
> **Primera:** los tiempos valen solo si la contraseña es **realmente al azar**. `S3gur!d@` no lo es —cae en el paso 2 de la sección anterior, en segundos—. La cuenta es el techo, no la garantía.
>
> **Segunda:** valen para un hash rápido. Los sistemas serios usan funciones **lentas a propósito** (bcrypt, Argon2), diseñadas para que cada intento cueste, y ahí los mismos números se vuelven años. La velocidad del ataque depende tanto de cómo guardó el sistema como de qué eligió el usuario.

Y la regla que ninguna cuenta arregla: **una contraseña reusada vale lo que vale el sitio más débil donde la usaste**. Si se filtra de ahí, entra en la lista del paso 1 y abre todo lo demás.

---

## 12. Qué dice el estándar

El NIST publicó en **julio de 2025** la versión vigente de su guía de identidad digital (**SP 800-63B-4**). Varias de sus reglas contradicen lo que se enseñó durante veinte años:

| Regla | Detalle |
|---|---|
| **Largo mínimo 15** | Si la contraseña es el único factor. Con segundo factor, alcanza con 8. |
| **Permitir hasta 64** | Y aceptar espacios y cualquier carácter imprimible. |
| **No exigir mezclas** | Prohibido obligar a «una mayúscula, un número y un símbolo». |
| **No obligar a cambiarla** | Nada de cada 90 días. Solo se fuerza el cambio **si hay indicio de que se filtró**. |
| **Sí comparar contra listas** | Rechazar las contraseñas que ya aparecieron en filtraciones o son demasiado comunes. |
| **Nada de preguntas de seguridad** | «El nombre de tu primera mascota» no es un secreto: está en tus redes. |

> [!NOTE]
> ¿Por qué se cayó lo del cambio cada 90 días? Porque **empeoraba** las contraseñas. La gente no inventa una nueva: pasa de `Marzo2026!` a `Junio2026!`. El cambio obligatorio producía contraseñas predecibles y usuarios que las anotaban en un papel.

Las cinco más usadas del mundo en el último relevamiento anual de NordPass siguen siendo `123456`, `123456789`, `12345`, `12345678` y `password`. La lista casi no cambia hace diez años. Es un dato de una empresa que vende gestores de contraseñas, así que tomalo por lo que es, pero el orden de magnitud no está en discusión.

> [!TIP]
> Los mecanismos que reemplazan o refuerzan la contraseña —segundo factor, biometría, claves de acceso— los vemos completos en la **clase 8**. Acá nos quedamos en la contraseña como superficie de ataque.

---

## 13. Errores frecuentes

> [!WARNING]
> Estos cuatro son los que más se repiten, y los cuatro se corrigen entendiendo bien la sección 1 y la 5.

**1. Usar «vulnerabilidad» y «amenaza» como sinónimos.** La vulnerabilidad es la puerta floja; la amenaza es el que la quiere empujar. El ransomware no es una vulnerabilidad: es lo que entra por una.

**2. Creer que el CVSS es el riesgo.** Es la gravedad de la falla en abstracto. El riesgo depende de tu contexto: qué máquina es, si tiene red, qué guarda y si ya hay exploit circulando. Una media urgente le gana a una crítica dormida.

**3. Pensar que sin internet no hay superficie de ataque.** Quedan el pendrive, el wifi, la visita que se conecta, el proveedor que entra a dar soporte y la cuenta vieja que nadie dio de baja.

**4. Confundir contraseña «complicada» con contraseña fuerte.** `P@ssw0rd!` es corta y previsible. `caballoverde` es larga y aburrida, y aguanta muchísimo más. Lo que cuenta es el largo y que sea impredecible, no que tenga símbolos.

---

## 14. Para la próxima clase

📝 **Actividad:** [Triage: seis vulnerabilidades, un técnico y una tarde](../actividades/06-triage-de-vulnerabilidades.md) — primera evaluación formativa del curso.

✅ **Autoevaluación:** [Poné a prueba lo que leíste](../autoevaluacion/06-vulnerabilidades-y-superficie-de-ataque.md) — 10 preguntas con respuestas explicadas.

📥 **Presentación:** [Clase 6 · Vulnerabilidades y superficie de ataque](../presentaciones/Clase6-Vulnerabilidades-y-Superficie-de-Ataque.pptx) — la misma que se usó en clase, con notas del docente.

📖 **Consulta:** [Glosario](../recursos/glosario.md) · [Bibliografía](../recursos/bibliografia.md) · [Normativa uruguaya](../recursos/normativa-uruguay.md)

🔭 **Lo que viene:** en la clase 7 pasamos al contenido 1.4, tecnologías emergentes e inteligencia artificial: qué superficie de ataque nueva aparece cuando el sistema que hay que proteger es un modelo.

> **Para pensar antes de la próxima clase.** Mirá el equipo desde el que estás leyendo esto. ¿Cuántos días hace que no lo actualizás, y cuántas cuentas tiene activas que ya no usa nadie? Esa es tu superficie de ataque, y no hizo falta ningún escáner para empezar a verla.

---

### Fuentes

- Programa CVE y *National Vulnerability Database* (NIST) — identificadores CVE y datos de vulnerabilidades. <https://www.cve.org/> · <https://nvd.nist.gov/>
- MITRE. *Common Weakness Enumeration* (CWE) — tipos de falla. <https://cwe.mitre.org/>
- FIRST. *CVSS v4.0 Specification Document* — escala de 0 a 10 y bandas de severidad. <https://www.first.org/cvss/>
- NIST. *SP 800-63B-4, Digital Identity Guidelines: Authentication and Authenticator Management* (julio de 2025) — requisitos de contraseñas. <https://pages.nist.gov/800-63-4/>
- NordPass. *Top 200 Most Common Passwords* — **dato de proveedor.** <https://nordpass.com/most-common-passwords-list/>

---

[⬅ Volver al índice del curso](../README.md)
