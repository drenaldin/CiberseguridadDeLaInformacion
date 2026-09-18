# Actividad 6 · Romperla y arreglarla

**Primera evaluación formativa del curso** · En la sala de informática · Grupos de 2 (de a uno por máquina si alcanzan)

---

## De qué se trata

Tienen delante el **portal de boletines del liceo**, una aplicación web que corre de verdad y que tiene **cinco vulnerabilidades reales**. La actividad tiene dos mitades:

1. **Romperla.** Explotar cada falla desde el navegador, sin herramientas: solo escribiendo en los campos y en la barra de direcciones.
2. **Arreglarla.** Para cada falla, decir con qué patrón se cierra, y —cuando se pueda— escribir la línea corregida.

> [!IMPORTANT]
> Esto se hace **solo sobre el portal que está en la máquina, preparado para eso**. Hacer lo mismo contra un sitio ajeno es delito en Uruguay, aunque no rompas ni te lleves nada (Ley N.º 20.327, art. 297 BIS). Está en el [Acuerdo de uso responsable](../CODE_OF_CONDUCT.md) que firmamos.

---

## Antes de empezar

El portal ya está levantado en cada máquina (el docente lo dejó pronto). Se abre en:

```
http://localhost/clase6/vulnerable/login.php
```

Usuarios de prueba:

| Cédula | Contraseña | Rol |
|---|---|---|
| `51234567` | `1234` | estudiante (Camila) |
| `49876543` | `liceo2026` | adscripta (Marta) |

Entrá primero como **Camila** y date una vuelta por el portal para entender qué hace: lista boletines, tiene un buscador, muestra cada boletín y deja descargar archivos.

---

## Parte A · Romperla (se entrega la planilla)

Para cada falla: hacé lo que dice la columna del medio y anotá qué pasó.

### Falla 1 · Entrar sin contraseña (inyección SQL)

En el login, en el campo **Cédula**, escribí exactamente esto (con el espacio al final) y en Contraseña cualquier cosa:

```
' OR rol='adscripta' -- 
```

| Qué anotar | |
|---|---|
| ¿Entraste? ¿Como quién? | |
| ¿Sabías la contraseña de esa persona? | |
| ¿Por qué funcionó? (en tus palabras) | |

### Falla 2 · Meter código en la página (XSS)

Entrá como Camila. En el **buscador**, escribí:

```html
<script>alert('esto es un ataque')</script>
```

| Qué anotar | |
|---|---|
| ¿Qué hizo el navegador? | |
| Si en vez de un cartel fuera código que roba tu sesión, ¿qué pasaría? | |

### Falla 3 · Ver lo ajeno cambiando la URL (IDOR)

Como Camila, entrá a tu boletín (el enlace «ver» del primero). Mirá la dirección: termina en `boletin.php?id=1`. Cambiá el `1` por un `3` y dale Enter.

| Qué anotar | |
|---|---|
| ¿Qué viste? ¿Era tuyo? | |
| ¿El portal te pidió permiso o dio error? | |

### Falla 4 · Bajar un archivo prohibido (salto de directorio)

En la página principal hay enlaces para descargar `calendario.txt`. La dirección es `descargar.php?archivo=calendario.txt`. Cambiala por:

```
descargar.php?archivo=../conexion.php
```

| Qué anotar | |
|---|---|
| ¿Qué archivo bajó? | |
| ¿Qué dato sensible tiene adentro? | |

### Falla 5 · La contraseña guardada (hash débil)

Esta no se ve desde el navegador: la mira el docente en la base, o la ven en el archivo `esquema.sql`. La contraseña de Camila está guardada como `81dc9bdb52d04dc20036dbd8313ed055`.

| Qué anotar | |
|---|---|
| Buscá esa cadena en un buscador de internet. ¿Qué contraseña aparece? | |
| ¿Por qué se pudo «adivinar» sin probar millones de veces? | |

---

## Parte B · Arreglarla (se entrega)

Ahora abrí, al lado de la carpeta `vulnerable/`, la carpeta `seguro/`: es el mismo portal, ya corregido. Para cada falla, compará el archivo de las dos versiones y completá:

| Falla | Patrón que la cierra | ¿Qué cambió en el código? (una línea) |
|---|---|---|
| 1 · Inyección SQL | | |
| 2 · XSS | | |
| 3 · IDOR | | |
| 4 · Salto de directorio | | |
| 5 · Hash débil | | |

Después, **probá los cinco ataques de la Parte A contra la versión segura** (`http://localhost/clase6/seguro/login.php`) y confirmá que ya no funcionan. Anotá qué respondió el portal en cada caso (por ejemplo: «incorrecta», «403», «404», o el script como texto).

> [!TIP]
> La comparación más clara es abrir `vulnerable/login.php` y `seguro/login.php` uno al lado del otro. Cambian tres líneas y cierran dos fallas (la inyección y el hash). Buscá el `prepare` y el `password_verify`.

---

## Parte C · Individual (media carilla)

Elegí **una** de las cinco fallas y respondé:

1. Explicá con tus palabras **por qué el programa original confiaba de más** en lo que llegaba de afuera.
2. Escribí el patrón que la cierra y **por qué ese patrón, y no “tener más cuidado”**, es la solución.
3. ¿En cuál de las otras cuatro fallas se repite el mismo error de fondo? Justificá.

---

## Qué se evalúa

| Criterio | Logrado | En proceso | Inicial |
|---|---|---|---|
| **Explotación** | Reproduce las cinco fallas y explica qué logró cada una. | Reproduce la mayoría pero no explica el efecto de alguna. | No logra reproducir las fallas o no entiende qué obtuvo. |
| **Identificación del patrón** | Nombra el patrón correcto para cada falla. | Acierta la mayoría; confunde uno. | Nombra patrones sueltos o incorrectos. |
| **Lectura del código corregido** | Señala qué línea cambió y por qué cierra la falla. | Ve el cambio pero no explica por qué funciona. | No ubica el cambio en el código. |
| **El error de fondo** | Reconoce que las fallas comparten «confiar en lo de afuera» y lo justifica. | Lo intuye sin sostenerlo. | No conecta las fallas entre sí. |
| **Parte C individual** | Elige, explica el exceso de confianza y defiende el patrón sobre el “cuidado”. | Explica parcialmente o no justifica el patrón. | No responde o no usa los conceptos. |

---

## Formato y tiempos

| Momento | Duración |
|---|---|
| Recorrida por el portal y Parte A (romperla) | 25 min |
| Parte B (comparar y confirmar la versión segura) | 20 min |
| Parte C individual | 10 min |
| Puesta en común | 5 min |
| **Total** | **60 min** |

**Entrega:** las planillas de la Parte A y B por grupo; la Parte C, individual. Formato libre: papel o digital.

---

[⬅ Volver a la clase 6](../clases/06-vulnerabilidades-y-superficie-de-ataque.md) · [Índice del curso](../README.md)
