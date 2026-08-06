# Spike 01 — Prueba de la barra visual

**Objetivo:** averiguar, antes de escribir código, si el pipeline visual alcanza el
nivel "indistinguible de video real", cuánto cuesta cada video y cuánto tarda.

**Quién lo ejecuta:** manualmente, a mano, sin programar nada.
**Cuánto debería costar:** entre $20 y $60 USD.
**Cuánto debería tardar:** 3 a 5 horas.

**Por qué se hace antes de construir:** los tres riesgos más graves del proyecto
(parecido facial, coherencia entre ángulos, costo real por video) se responden con
este ejercicio. Si falla, cambiamos de enfoque sin haber gastado semanas de trabajo.
Si pasa, construimos con números reales en vez de suposiciones.

---

## Parte A — Tomar las 4 fotos

Usa **una persona real y disponible** (tú mismo sirve). Con teléfono, sin equipo.

Reglas para las 4 fotos:
- Misma ropa, mismo peinado, misma sesión, mismos 10 minutos.
- Fondo cualquiera (una pared, una recámara — da igual, se va a reemplazar).
- Luz pareja en la cara. Evita contraluz y sombras duras.
- El teléfono a la altura de los ojos, no desde abajo.
- Vertical, cámara trasera, sin filtros, sin modo retrato, sin belleza.

| Archivo | Encuadre | Postura |
|---|---|---|
| `foto-A.jpg` | Cabeza y hombros | De frente, mirando a la cámara |
| `foto-B.jpg` | Medio cuerpo, más lejos | De frente, mirando a la cámara |
| `foto-C.jpg` | Cabeza y hombros | Cuerpo girado 45° a tu izquierda, cara hacia la cámara |
| `foto-D.jpg` | Cabeza y hombros | Cuerpo girado 45° a tu derecha, cara hacia la cámara |

---

## Parte B — Conseguir las referencias de estudio

Necesitas **3 imágenes de referencia** del set que quieres imitar: `studio-ref-1.jpg`,
`studio-ref-2.jpg`, `studio-ref-3.jpg`.

Busca imágenes de: *"documentary interview setup white cyclorama"*, *"minimal
photography studio interview lighting"*, *"talking head interview set design"*.

Elige **un solo estilo coherente** entre las 3. Si una es un estudio blanco minimalista
y otra es un set oscuro y cálido, la IA no va a saber qué quieres y los 4 ángulos van a
salir distintos entre sí — que es justamente lo que estamos tratando de evitar.

---

## Parte C — Generar los 4 retratos de estudio

Corre esto **4 veces**, una por foto, cambiando solo la parte marcada. Usa el mismo
prompt base y las mismas 3 referencias de estudio en las 4 corridas. Esto es lo que
mantiene la coherencia entre ángulos.

### Prompt base (pégalo tal cual, en inglés)

```
Create a photorealistic upgraded version of this exact portrait.

KEEP THE PERSON COMPLETELY UNCHANGED: identical face, identical facial
proportions, natural skin texture including pores and imperfections, same body
shape, same body size, same posture, same shoulder position, same expression,
same eye direction, same hair, same clothing, same accessories.

DO NOT beautify. DO NOT slim. DO NOT reshape. DO NOT smooth the skin. DO NOT
change age. DO NOT change ethnicity. The person must remain recognizable as the
exact same individual.

PRESERVE the same camera angle, the same framing and the same composition as the
source photo.

REPLACE ONLY the environment and the photographic production.

ENVIRONMENT: place the subject in a professional documentary interview studio.
Seamless white cyclorama backdrop, softly falling off to light grey at the edges.
A boom microphone visible at the top edge of the frame. Clean, uncluttered,
high-end. Match the style of the attached studio reference images.

LIGHTING: professional interview lighting. One very large diffused softbox above
eye level, 45 degrees to camera left, creating soft dimensional shadows on the
face without harsh edges. A subtle rim light separating the subject from the
background. Maintain realistic contrast, accurate skin tones, natural highlight
roll-off and natural shadow detail.

CAMERA: shot on a cinema camera with a fast prime lens. Shallow but not extreme
depth of field. Natural film grain. No digital sharpening halos.

[VARIANTE POR FOTO — ver abajo]
```

### Variante por foto (añade UNA de estas al final del prompt)

- **Foto A:** `FRAMING: tight head-and-shoulders close-up, subject facing camera directly.`
- **Foto B:** `FRAMING: wider medium shot showing head and upper torso, more of the studio visible behind and beside the subject, subject facing camera directly.`
- **Foto C:** `FRAMING: head-and-shoulders, body turned 45 degrees to the left, face toward camera. This is a secondary camera angle from the same interview session — the lighting direction, background and wardrobe must match the other shots exactly.`
- **Foto D:** `FRAMING: head-and-shoulders, body turned 45 degrees to the right, face toward camera. This is a secondary camera angle from the same interview session — the lighting direction, background and wardrobe must match the other shots exactly.`

### Herramientas a comparar

Corre el ejercicio completo en las dos y compara:

1. **Google Flow** — ya lo tienes, no cuesta extra. Empieza por aquí.
2. **Higgsfield** — es la herramienta de la referencia original. Págala solo si Flow
   no da el nivel.

Prueba primero con Flow. Si el resultado ya aguanta, te ahorraste el gasto y además
descubriste que puedes usar la API de Google, que es más fácil de automatizar.

---

## Parte D — Evaluar los retratos

Antes de gastar en video, revisa los 4 retratos contra esta lista. **Sé duro.** Un
"casi" aquí se convierte en un producto que nadie paga.

| # | Criterio | ¿Pasa? |
|---|---|---|
| 1 | ¿Es reconociblemente la misma persona en las 4? | |
| 2 | ¿Le cambió la cara, la edad, el peso o los rasgos? *(debe ser NO)* | |
| 3 | ¿La piel se ve real, con textura, o se ve de plástico? | |
| 4 | ¿Las 4 parecen del mismo día, mismo set, misma luz? | |
| 5 | ¿La ropa es la misma en las 4? | |
| 6 | ¿La luz viene del mismo lado en las 4? | |
| 7 | ¿Manos, orejas, lentes, dientes se ven bien? | |
| 8 | Si se la enseñas a alguien sin contexto, ¿sospecha que es IA? *(debe ser NO)* | |

**Si fallan el 1, el 2 o el 8, detente.** No sigas a la parte E. Ajusta el prompt o
cambia de herramienta y vuelve a intentar. Ese es el hallazgo más importante del
spike y hay que resolverlo antes de seguir.

---

## Parte E — Animación y lip sync

Solo si la parte D pasó.

1. Graba **30 segundos de audio** de esa persona hablando, con su voz real. Cualquier
   cosa sirve: que cuente qué desayunó.
2. Toma el retrato de estudio de la **foto A** y anímalo con lip sync a ese audio.
3. Haz lo mismo con el retrato de la **foto C**.

Evalúa:

| # | Criterio | ¿Pasa? |
|---|---|---|
| 1 | ¿Los labios coinciden con el audio? | |
| 2 | ¿La cara se mantiene estable o "baila" y cambia? | |
| 3 | ¿El movimiento del cuerpo se ve natural o robótico? | |
| 4 | ¿Los ojos parpadean de forma creíble? | |
| 5 | ¿Aguanta ver 30 segundos seguidos sin que moleste? | |
| 6 | ¿Cuánto dura el clip máximo que la herramienta permite generar de una vez? | |

El punto 6 importa mucho: si la herramienta solo genera clips de 5 u 8 segundos, un
video de 10 minutos son entre 75 y 120 generaciones. Eso cambia por completo el costo,
el tiempo y la arquitectura.

---

## Parte F — Anotar los números

Esto es la mitad del valor del spike. **Anota todo, aunque te parezca tedioso.**

| Dato | Valor |
|---|---|
| Herramienta usada | |
| Intentos necesarios para lograr un retrato aceptable (foto A) | |
| Intentos para las otras 3 | |
| Costo por generación de imagen | |
| Costo total de las 4 imágenes, incluyendo intentos fallidos | |
| Tiempo por generación de imagen | |
| Duración máxima de clip de video | |
| Costo por segundo de video generado | |
| Tiempo por generación de video | |
| Intentos necesarios para un clip de video aceptable | |
| **Costo estimado de un video de 10 minutos** | |
| **Tiempo estimado de un video de 10 minutos** | |

---

## Qué hacer con el resultado

- **Si pasa todo:** tenemos números reales. Se elige proveedor, se calcula el margen
  con el precio de $1,000–$2,500 y se empieza a construir con base firme.
- **Si el parecido falla:** el problema es el prompt o la herramienta. Se itera aquí,
  que es barato, no en la app, que es caro.
- **Si la coherencia entre ángulos falla:** puede que la V1 tenga que ser de un solo
  ángulo con movimiento sutil. Sigue siendo un producto vendible, solo menos ambicioso.
- **Si el costo por video sale muy alto:** se recorta la duración del video, o se sube
  el precio, o se cambia de proveedor. Mejor saberlo ahora que con clientes esperando.
