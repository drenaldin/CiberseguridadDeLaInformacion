# Autoevaluación · Clase 3

**Principios de arquitectura de seguridad y políticas**

---

Diez preguntas para comprobar por tu cuenta si entendiste el material. **No lleva nota y no se entrega.**

Respondé mentalmente o en papel *antes* de desplegar la respuesta. Si la mirás primero, la actividad no sirve de nada: el efecto de aprendizaje está en el intento, no en la lectura.

> [!TIP]
> En el celular, tocá el triángulo ▸ para desplegar cada respuesta.

📖 Si algo no te sale, volvé a [Clase 3 · Principios de arquitectura y políticas](../clases/03-arquitectura-y-politicas.md).

---

### 1. Una carpeta compartida nueva se crea accesible para todo el liceo y después se le van quitando permisos a quien no corresponde. ¿Qué principio se está violando?

<details>
<summary>Ver respuesta</summary>

**Valores predeterminados a prueba de fallos** (*fail-safe defaults*): por omisión hay que **denegar**.

El acceso se concede explícitamente, uno por uno; no se otorga a todos y se recorta después. La diferencia no es de estilo: si te olvidás de quitar un permiso, con el criterio correcto nadie accede de más, y con el criterio equivocado accede medio liceo. **El error humano tiene que fallar hacia el lado seguro.**
</details>

---

### 2. «Nuestra red wifi es segura porque ocultamos el nombre de la red y nadie sabe cómo está armada.» ¿Qué principio desconoce esa afirmación?

<details>
<summary>Ver respuesta</summary>

El **diseño abierto**. La seguridad no puede depender de que el diseño sea secreto: lo único que debe ser secreto es **la clave**.

Un nombre de red oculto se descubre con una herramienta gratuita en menos de un minuto, porque los dispositivos que ya se conectaron lo siguen anunciando. Esto se conoce como *seguridad por oscuridad*, y su problema de fondo es que **no se puede medir**: no sabés cuánto te protege, hasta que descubrís que no te protegía nada.
</details>

---

### 3. ¿Por qué el principio de aceptabilidad psicológica (el n.º 8) es un principio de seguridad y no de comodidad?

<details>
<summary>Ver respuesta</summary>

Porque **un control que la gente no puede cumplir no protege: genera el riesgo que quería evitar**.

El ejemplo clásico está en el material: contraseñas de veinte caracteres cambiadas cada treinta días terminan anotadas en un papel abajo del teclado. La organización cree que tiene contraseñas fuertes y en realidad tiene contraseñas fuertes **escritas en la mesa**.

Corolario práctico: cuando una regla la incumple todo el mundo, el problema casi nunca es la gente. Es la regla.
</details>

---

### 4. Un liceo instala tres antivirus distintos en la misma máquina. ¿Es defensa en profundidad?

<details>
<summary>Ver respuesta</summary>

**No.** Es una sola capa repetida tres veces, en la capa «equipo».

La defensa en profundidad exige capas **independientes**, que fallen de maneras distintas: física, red, equipo, aplicación, dato y persona. Tres productos que hacen lo mismo comparten los mismos puntos ciegos, y frente a lo que ninguno detecta, los tres fallan juntos.

Y hay un costo: cada producto agregado suma superficie de ataque y una nueva forma de fallar. CrowdStrike, en 2024, fue exactamente eso —un producto de seguridad causando la mayor caída del año.
</details>

---

### 5. Nombrá tres formas de reducir la superficie de ataque de la sala de informática que no cuesten dinero.

<details>
<summary>Ver respuesta</summary>

Cualquier tres de estas:

- **Desinstalar** el software que no se usa en clase.
- **Dar de baja** las cuentas de gente que ya no está y los usuarios de prueba.
- **Quitar** los permisos de administrador a las cuentas de uso diario.
- **Cambiar** las credenciales de fábrica de todo lo que esté conectado, incluida la impresora y las cámaras.
- **Cerrar** los servicios y puertos que nadie usa.
- **Borrar** los archivos con datos personales que quedaron en los escritorios.

Lo que tienen en común: casi todas consisten en **sacar**, no en comprar. Reducir superficie es la medida más barata que existe, y por eso es la primera.
</details>

---

### 6. ¿Qué quiere decir «asumir la brecha» en un modelo de confianza cero?

<details>
<summary>Ver respuesta</summary>

Diseñar el sistema **como si el atacante ya estuviera adentro**.

En vez de apostar todo a que la muralla aguante, se limita el radio de daño: segmentar la red, cifrar los datos, registrar todos los accesos, dar permisos mínimos y temporales, y volver a verificar la identidad en cada pedido.

Es la respuesta directa a lo que pasó en Bangladesh y en Costa Rica: en los dos casos el atacante estuvo **semanas** adentro moviéndose de un sistema a otro sin que nada volviera a preguntarle quién era.
</details>

---

### 7. Ordená de mayor a menor jerarquía: procedimiento, guía, norma, política. ¿Cómo se distinguen leyendo el texto?

<details>
<summary>Ver respuesta</summary>

**Política → norma → procedimiento → guía.**

La forma rápida de distinguirlos es mirar el verbo:

| Nivel | Tono | Ejemplo |
|---|---|---|
| Política | *«se protege», «se garantiza»* | «La información de los estudiantes se protege según su clasificación.» |
| Norma | **«debe»** | «Toda cuenta con acceso a calificaciones debe usar segundo factor.» |
| Procedimiento | *«hacé clic en…»* | «Alta de usuario docente: 1) recibir el formulario firmado…» |
| Guía | *«se recomienda»* | «Recomendaciones para armar una frase de contraseña.» |

Quién los aprueba también cambia: la política la firma la máxima autoridad; el procedimiento lo escribe quien opera el sistema.
</details>

---

### 8. ¿Desde cuándo están obligados los organismos de la Administración Central uruguaya a tener una política de seguridad de la información, y qué agregó la normativa de 2025?

<details>
<summary>Ver respuesta</summary>

Desde el **Decreto N.º 452/009**, es decir **desde 2009**.

El **Decreto N.º 66/025** endureció el régimen después de la seguidilla de incidentes: obliga a **designar un Responsable de Seguridad de la Información**, a adoptar el **Marco de Ciberseguridad** de AGESIC y alcanzar los niveles mínimos de madurez del perfil asignado, a **notificar los incidentes al CERTuy dentro de las 24 horas** y a **conservar los registros de auditoría por doce meses**.

El dato que conviene tener presente: la obligación existe desde hace más de quince años, y aun así el cumplimiento relevado es muy bajo. Tener la norma escrita no es lo mismo que cumplirla.
</details>

---

### 9. Nombrá las seis funciones del Marco de Ciberseguridad del Uruguay y decí en cuál vive la política de seguridad.

<details>
<summary>Ver respuesta</summary>

**Gobernar · Identificar · Proteger · Detectar · Responder · Recuperar.**

La política de seguridad vive en **Gobernar**, que es la función que define quién decide, con qué reglas y con qué recursos. Es la que sostiene a las otras cinco: sin gobierno, «proteger» y «detectar» dependen de la buena voluntad de quien esté de turno.

El marco además mide la **madurez del 0 al 4** —de «sin medidas» a «mejora continua»— y define tres **perfiles** (Básico, Estándar y Avanzado), porque no se le exige lo mismo a una oficina chica que a un servicio crítico.
</details>

---

### 10. «Los docentes deben cuidar sus contraseñas.» ¿Por qué esta regla no sirve, y cómo la reescribirías?

<details>
<summary>Ver respuesta</summary>

No sirve porque **no se puede verificar**. «Cuidar» no describe ninguna conducta observable: nadie puede ir a comprobar si se cumple o no, y por lo tanto nadie puede incumplirla ni cumplirla.

Una versión utilizable necesita **sujeto, acción y plazo**. Por ejemplo:

> «Cada docente usa una cuenta personal e intransferible para el sistema de calificaciones. Las cuentas compartidas están prohibidas. El acceso se hace con segundo factor. Toda sospecha de que una contraseña quedó expuesta se avisa al referente informático el mismo día, y avisar de buena fe nunca es motivo de sanción.»

Fijate en la última oración: sin ella, la regla compra silencio en lugar de seguridad.
</details>

---

## Cómo te fue

| Respondiste bien | Qué significa |
|---|---|
| 8 a 10 | Estás pronto para redactar la política de la actividad 3. |
| 5 a 7 | Repasá las secciones 2 a 5 (los principios) y la 8 (anatomía de una política). |
| Menos de 5 | Volvé a leer el material completo. Empezá por la tabla de los ocho principios y por la pirámide normativa: casi todas las respuestas salen de esas dos. |

---

[⬅ Volver a Clase 3](../clases/03-arquitectura-y-politicas.md) · [Índice del curso](../README.md)
