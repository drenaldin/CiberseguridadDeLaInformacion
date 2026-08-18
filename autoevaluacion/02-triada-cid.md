# Autoevaluación · Clase 2

**La tríada CID en profundidad y análisis de casos**

---

Diez preguntas para comprobar por tu cuenta si entendiste el material. **No lleva nota y no se entrega.**

Respondé mentalmente o en papel *antes* de desplegar la respuesta. Si la mirás primero, la actividad no sirve de nada: el efecto de aprendizaje está en el intento, no en la lectura.

> [!TIP]
> En el celular, tocá el triángulo ▸ para desplegar cada respuesta.

📖 Si algo no te sale, volvé a [Clase 2 · La tríada CID](../clases/02-triada-cid.md).

---

### 1. ¿Cuál de las tres propiedades, una vez perdida, no se puede recuperar? ¿Por qué?

<details>
<summary>Ver respuesta</summary>

La **confidencialidad**.

Un servicio caído se levanta y un dato alterado se restaura desde una copia, pero un dato que salió de la organización no se puede «desfiltrar»: ya está en manos de otro, y puede copiarse infinitas veces.

Esa asimetría tiene una consecuencia práctica: para los datos cuya exposición sería catastrófica —salud, menores, situación judicial o financiera— se justifica una inversión en prevención que a primera vista parece desproporcionada, porque después no hay reparación posible.
</details>

---

### 2. Roban una laptop de la oficina. El disco estaba cifrado con una contraseña fuerte que el ladrón no tiene. ¿Se perdió confidencialidad?

<details>
<summary>Ver respuesta</summary>

**No**, o al menos no todavía: nadie puede leer el contenido sin la clave.

Lo que se perdió es la **posesión o control**, la propiedad que agrega el hexágono de Parker justamente para describir esta situación, que la tríada clásica clasifica mal. También se puede haber perdido **disponibilidad**, si esos archivos no estaban en ningún otro lado.

Y aunque no haya pérdida de confidencialidad, el hecho **es un incidente y se reporta**: hay que verificar que el cifrado estaba efectivamente activo y que la contraseña no estaba anotada en la funda.
</details>

---

### 3. ¿Por qué se dice que la integridad es «la propiedad silenciosa»?

<details>
<summary>Ver respuesta</summary>

Porque su pérdida **puede no notarse nunca**.

Cuando se cae un servicio, alguien llama en cinco minutos. Cuando se filtran datos, tarde o temprano aparecen publicados. Pero un dato alterado sigue disponible, sigue siendo confidencial y se ve exactamente igual que antes: simplemente es falso.

El problema es lo que viene después: todas las decisiones tomadas a partir de ese dato son decisiones equivocadas tomadas con total confianza. Por eso los ataques más sofisticados suelen ir contra la integridad.
</details>

---

### 4. Un servicio comprometido al 99,9 % de disponibilidad, ¿cuánto puede estar caído por año? ¿Y al 99,99 %?

<details>
<summary>Ver respuesta</summary>

- **99,9 %** («tres nueves»): unas **8 horas y 46 minutos** al año.
- **99,99 %** («cuatro nueves»): unos **52 minutos** al año.

Lo importante no es memorizar la tabla sino entender que **cada nueve adicional multiplica el costo**. Decidir cuántos nueves se pagan es una decisión de gestión: para la cartelera de novedades del liceo, dos nueves sobran.
</details>

---

### 5. Diferenciá RTO de RPO con un ejemplo propio.

<details>
<summary>Ver respuesta</summary>

- **RTO** (*Recovery Time Objective*): **cuánto tiempo** tolero que el servicio esté caído. Se mide hacia adelante desde el incidente.
- **RPO** (*Recovery Point Objective*): **cuántos datos** tolero perder. Se mide hacia atrás, y equivale a la antigüedad máxima aceptable de la copia de seguridad.

Un ejemplo posible: en el sistema de reservas de una policlínica, un RTO de 2 horas y un RPO de 15 minutos significa que hay que estar funcionando otra vez a las dos horas y que no se acepta perder más de un cuarto de hora de reservas cargadas. Ese RPO obliga a respaldar cada 15 minutos, y eso cuesta plata: por eso lo fija la dirección, no el técnico.
</details>

---

### 6. Un ataque de ransomware cifra los archivos de una empresa. Alguien dice: «afectó solo la disponibilidad». ¿Es correcto?

<details>
<summary>Ver respuesta</summary>

**Casi seguro que no.**

Desde hace años la mayoría de los grupos de ransomware operan por **doble extorsión**: primero exfiltran los datos, después los cifran. Si no pagás, publican. Así que, salvo evidencia en contrario, hay que asumir también pérdida de **confidencialidad**.

Y la **integridad** queda al menos en duda: si el atacante estuvo dentro con permisos de administrador, no se puede afirmar que los datos siguen siendo exactos sin verificarlo.

De ahí la consecuencia más importante del caso del BHU: pagar el rescate podría devolver la disponibilidad, pero **no puede devolver la confidencialidad**.
</details>

---

### 7. La caída de CrowdStrike de julio de 2024 dejó fuera de servicio a millones de equipos. ¿Qué propiedades se afectaron y qué tiene de particular el caso?

<details>
<summary>Ver respuesta</summary>

Se afectó **únicamente la disponibilidad**. Nadie accedió a datos ajenos ni los modificó.

Lo particular es doble. Primero: **no hubo atacante**. Fue una actualización defectuosa del propio proveedor, y aun así es un incidente de seguridad de manual, porque la seguridad incluye las amenazas accidentales.

Segundo, y más incómodo: el producto que causó la caída era **un producto de seguridad**. Todo control que se agrega protege de algo y, al mismo tiempo, crea una nueva forma de fallar.
</details>

---

### 8. En el ataque al Banco Central de Bangladesh (2016), ¿cuál fue la propiedad principal afectada, y qué control terminó frenando la mayor parte del robo?

<details>
<summary>Ver respuesta</summary>

La propiedad principal fue la **integridad**: se insertaron órdenes de transferencia falsas en un sistema que las procesó como legítimas, y se manipularon los mecanismos de verificación para que no se notaran de inmediato. Con ella caen la **autenticidad** y el **no repudio**. Antes hubo también pérdida de **confidencialidad**, porque los atacantes observaron durante semanas cómo operaba el banco.

Y el control que frenó las transferencias más grandes no fue tecnológico: fue **una persona que notó una palabra mal escrita** (*Fandation* en lugar de *Foundation*) y levantó la sospecha. La Reserva Federal terminó bloqueando 30 órdenes por unos 850 millones de dólares.
</details>

---

### 9. Una app de mensajería cifra todo de punta a punta y controla los accesos con rigor, pero pide acceso a tus contactos, tu ubicación permanente y tu galería de fotos para funcionar. ¿Tiene un problema de confidencialidad o de privacidad?

<details>
<summary>Ver respuesta</summary>

De **privacidad**.

La confidencialidad es una propiedad técnica: quién puede acceder al dato que la app ya tiene. En este ejemplo está bien resuelta.

La privacidad es un derecho de la persona sobre sus propios datos: qué se recolecta, para qué, por cuánto tiempo y qué control conserva sobre eso. La app recolecta mucho más de lo que necesita para funcionar, y eso es un problema aunque lo custodie perfectamente.

Dicho corto: **la confidencialidad protege el dato que ya tenés; la privacidad pregunta si tenías que tenerlo.**
</details>

---

### 10. Un hospital y un banco tienen que ordenar C, I y D por prioridad. ¿Coinciden? Justificá.

<details>
<summary>Ver respuesta</summary>

**No coinciden, y las dos decisiones son correctas en su contexto.**

- **Hospital: D → I → C.** Un sistema caído durante una urgencia cuesta vidas; el acceso a la historia clínica tiene que estar garantizado. Eso no vuelve descartable la confidencialidad: los datos de salud son **datos sensibles** según la Ley N.º 18.331.
- **Banco: I → C → D.** Un saldo equivocado destruye la confianza en el sistema entero. Se prefiere frenar la operativa —sacrificar disponibilidad— antes que procesar una operación mal.

La conclusión es la que ordena la clase: **no existe una jerarquía universal entre las tres propiedades**. Existe una jerarquía por organización, y saber defenderla con argumentos es parte del oficio.
</details>

---

## Cómo te fue

| Respondiste bien | Qué significa |
|---|---|
| 8 a 10 | Estás pronto para la clase 3 y para la actividad en grupo. |
| 5 a 7 | Repasá las secciones 2 a 4 (cada propiedad por separado) y la 9 (errores frecuentes). |
| Menos de 5 | Volvé a leer el material completo, y prestá especial atención a los cinco casos: ahí están casi todas las respuestas aplicadas. |

---

[⬅ Volver a Clase 2](../clases/02-triada-cid.md) · [Índice del curso](../README.md)
