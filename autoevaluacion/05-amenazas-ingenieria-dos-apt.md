# Autoevaluación · Clase 5

**Amenazas II: ingeniería social, denegación de servicio y APT**

---

Diez preguntas para comprobar por tu cuenta si entendiste el material. **No lleva nota y no se entrega.**

Respondé mentalmente o en papel *antes* de desplegar la respuesta. El efecto de aprendizaje está en el intento, no en la lectura.

> [!TIP]
> En el celular, tocá el triángulo ▸ para desplegar cada respuesta.

📖 Si algo no te sale, volvé a [Clase 5 · Amenazas II](../clases/05-amenazas-ingenieria-dos-apt.md).

---

### 1. ¿Qué propiedad de la tríada rompe un ataque de denegación de servicio?

<details>
<summary>Ver respuesta</summary>

La **disponibilidad**. El servicio deja de estar accesible para sus usuarios legítimos.

No se roban ni se modifican datos: el sistema sigue existiendo y la información está intacta, pero nadie puede usarlo. Es el ejemplo más puro de pérdida de disponibilidad.
</details>

---

### 2. ¿Cuál es la diferencia entre DoS y DDoS?

<details>
<summary>Ver respuesta</summary>

El **DoS** sale de **un solo origen**; el **DDoS** sale de **miles de equipos a la vez** (la D de más es de *distribuido*).

El DDoS es mucho más difícil de frenar, porque no hay una única fuente que cortar. Esos miles de equipos suelen ser una **botnet**: computadoras, cámaras y routers infectados que obedecen al atacante sin que sus dueños lo sepan.
</details>

---

### 3. ¿Por qué ningún antivirus detiene un ataque de ingeniería social?

<details>
<summary>Ver respuesta</summary>

Porque **no ataca la máquina, ataca a la persona**: su confianza, su apuro, su miedo o sus ganas de ayudar.

El antivirus revisa archivos y programas; no puede impedir que alguien dicte su contraseña por teléfono a quien dice ser del soporte. La defensa es humana: formación, verificar por otro canal y una cultura donde reportar un error no se castiga.
</details>

---

### 4. Relacioná cada técnica con su definición: pretexting, baiting, tailgating, shoulder surfing.

<details>
<summary>Ver respuesta</summary>

- **Pretexting:** inventar un pretexto creíble para pedir algo («soy del soporte técnico»).
- **Baiting:** dejar un cebo tentador para que la víctima lo active (un pendrive «olvidado»).
- **Tailgating:** colarse a un lugar físico detrás de una persona autorizada.
- **Shoulder surfing:** espiar por encima del hombro una contraseña o un PIN.

La quinta que vimos es **quid pro quo**: ofrecer algo a cambio del dato o el acceso.
</details>

---

### 5. Explicá con tus palabras cómo un DDoS deja caído un servicio sin robar ni romper nada.

<details>
<summary>Ver respuesta</summary>

Satura al servidor de **pedidos**. Un servidor puede atender una cantidad limitada por segundo; si le llegan muchísimos más, colapsa, y los pedidos legítimos quedan mezclados entre los falsos sin poder entrar.

El atacante no necesita romper ninguna cerradura: le alcanza con **pedir de más**. Por eso es un problema de escala, y la defensa no es un antivirus sino filtrar el tráfico y tener capacidad de sobra.
</details>

---

### 6. ¿Qué cuatro rasgos definen a una APT, y por qué hacen falta los cuatro?

<details>
<summary>Ver respuesta</summary>

**Avanzada, persistente, con recursos y dirigida.**

- *Avanzada:* usa técnicas sofisticadas, a veces vulnerabilidades desconocidas.
- *Persistente:* se queda meses o años, no entra y sale.
- *Con recursos:* detrás suele haber un Estado o un grupo grande.
- *Dirigida:* va por un objetivo concreto (espionaje, sabotaje), no por plata rápida.

Hacen falta los cuatro juntos: un malware sofisticado que entra y sale en un día no es una APT, y un aficionado que se queda mucho tiempo tampoco. Lo que la define es la combinación.
</details>

---

### 7. ¿Por qué se dice que la APT es «la amenaza silenciosa»?

<details>
<summary>Ver respuesta</summary>

Porque **su objetivo es no ser detectada**. Un ransomware te avisa con un cartel; una APT hace lo contrario: se mueve despacio, copia de a poco y borra sus huellas para quedarse el mayor tiempo posible.

Por eso casi todos los casos conocidos se descubrieron mucho después de que empezaron. Es, en el mundo de las amenazas, el equivalente a la integridad en la tríada: la pérdida que puede no notarse nunca.
</details>

---

### 8. En el caso de SolarWinds, ¿por qué se lo llama «ataque a la cadena de suministro»?

<details>
<summary>Ver respuesta</summary>

Porque los atacantes no entraron a cada víctima directamente: comprometieron a **un proveedor** de software e insertaron código malicioso en una actualización oficial. Al instalar esa actualización de confianza, miles de organizaciones abrieron la puerta sin saberlo.

Conecta con la clase 3: **la superficie de ataque incluye a tus proveedores**. El eslabón más débil puede no estar en tu propia red, sino en alguien en quien confiás.
</details>

---

### 9. El sitio de inscripciones se cae por millones de pedidos desde miles de equipos. Alguien dice «hay que pasarle el antivirus al servidor». ¿Es correcto?

<details>
<summary>Ver respuesta</summary>

**No.** En el servidor atacado no hay nada que desinfectar: el problema es el **volumen de tráfico que llega de afuera**, no un programa malicioso instalado.

Un DDoS se defiende filtrando el tráfico antes de que llegue, con servicios de mitigación y con capacidad de sobra para absorber el pico. El antivirus resuelve otro tipo de problema.
</details>

---

### 10. De las tres amenazas de la clase, ¿cuál NO suele buscar plata rápida, y qué busca en cambio?

<details>
<summary>Ver respuesta</summary>

La **APT**. No busca ganancia rápida sino un objetivo específico y de largo plazo: **espionaje o sabotaje**, sostenido en el tiempo.

Es la gran diferencia con las amenazas de la clase 4 (ransomware, phishing), que casi siempre iban por dinero inmediato. Stuxnet saboteaba centrifugadoras; SolarWinds espiaba agencias. Ninguno pedía un rescate.
</details>

---

## Cómo te fue

| Respondiste bien | Qué significa |
|---|---|
| 8 a 10 | Cerraste el panorama de amenazas. Estás pronto para la clase 6 y la primera evaluación formativa. |
| 5 a 7 | Repasá la sección 5 (cómo funciona un DDoS) y la 7 (los cuatro rasgos de la APT). |
| Menos de 5 | Volvé a leer el material. Empezá por la tabla de la sección 1: las tres amenazas atacan cosas distintas, y de ahí sale casi todo. |

---

[⬅ Volver a Clase 5](../clases/05-amenazas-ingenieria-dos-apt.md) · [Índice del curso](../README.md)
