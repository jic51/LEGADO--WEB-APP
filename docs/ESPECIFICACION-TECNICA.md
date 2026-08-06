# LEGADO — Especificación Técnica

**Versión:** 0.1 (borrador de requisitos)
**Fecha:** agosto 2026
**Estado:** pre-implementación. Contiene supuestos marcados que deben confirmarse.

> Este documento está escrito para que cualquier persona o cualquier IA pueda entender
> qué es LEGADO y qué hay que construir, sin contexto previo de la conversación.

---

## 1. Resumen en una frase

LEGADO es una web app que convierte las fotos y los recuerdos de una persona en un
video documental fotorrealista donde **esa misma persona aparece en pantalla, con su
cara y su voz reales, contando la historia de su vida**, filmado aparentemente en un
estudio profesional — sin que nadie haya tocado una cámara, salido de su casa ni
viajado a ningún lado. Todo se genera con IA.

---

## 2. El producto

### 2.1 Qué recibe el usuario

Un **video documental de su historia de vida**, con la estética de una entrevista
profesional: fondo de ciclorama, iluminación de estudio, micrófono visible, cortes
entre varios ángulos de cámara, y la persona hablando a cuadro con su propia voz.

Complementos que se venden aparte:
- Clips verticales cortos para redes sociales
- Set de retratos de estudio / imagen de portada
- Álbum digital de fotos
- Cápsula: custodia del material y entrega programada en el futuro

### 2.2 Qué NO es

- **No** es una productora de video. No hay cámaras, ni equipo, ni oficinas, ni
  vehículos, ni personal desplazándose. Cero operación física.
- **No** es un avatar genérico ni un personaje inventado. La persona que aparece en
  pantalla **es** la persona real, con su cara real.
- **No** es un libro ni un producto de texto.
- **No** requiere que el usuario sepa escribir bien, ni redactar, ni editar video.

### 2.3 La idea central que hace que funcione

La IA **no inventa a la persona**. La persona se conserva exactamente como es —
misma cara, mismas proporciones, misma piel, misma postura, mismo encuadre — y lo
único que se reemplaza es **el entorno y la producción fotográfica**: el fondo pasa
de ser una recámara a ser un set de estudio, y la luz de ventana pasa a ser luz
cinematográfica.

Por eso el resultado se ve real: porque la persona *es* real. Solo el estudio es
sintético.

Prompt de referencia validado (del ejemplo que originó este proyecto):

> *"Create a photorealistic upgraded version of this exact portrait. **Keep the person
> completely unchanged**: identical face, facial proportions, natural skin texture, body
> shape, body size, posture, shoulder position, expression, direct eye contact, [rasgos
> específicos]. **Preserve the same straight-on camera angle, vertical framing and
> upper-body composition. Do not beautify, slim, reshape.** Replace only the environment
> and photographic production. Place them inside a high-end contemporary creative studio
> [descripción del set]. Light the portrait professionally using a very large diffused
> softbox [descripción de la iluminación]."*

### 2.4 Cómo se resuelve el multicámara sin inventar ángulos

Requisito original: que el video no sea una sola toma fija, sino que corte entre
distintos ángulos y se vea parte del escenario.

**Solución: cada foto que sube el usuario se convierte en un ángulo de cámara del
video final.** La app guía al usuario para que tome, por ejemplo, 4 fotos:

| Foto | Instrucción al usuario | Se convierte en |
|---|---|---|
| 1 | De frente, pecho y cabeza, mirando a la cámara | Cámara A — primer plano |
| 2 | De frente pero más lejos, viendo torso completo | Cámara B — plano medio |
| 3 | Girado 45° a tu izquierda, mirando a la cámara | Cámara C — perfil 3/4 |
| 4 | Girado 45° a tu derecha, mirando a la cámara | Cámara D — perfil 3/4 opuesto |

Al cortar entre esas cuatro tomas durante el relato se obtiene la sensación de
producción multicámara **sin que la IA tenga que inventar ni un solo ángulo nuevo de
la persona**. A cada toma se le puede añadir movimiento sutil (acercamiento lento,
ligera deriva lateral), que es mucho más fácil y confiable que rotar la cámara
alrededor del sujeto.

**Nota:** el movimiento amplio de cámara *orbitando* al protagonista, tal como se
describió en la idea original, queda **fuera del alcance de la V1** y marcado como
riesgo técnico abierto (ver §11).

---

## 3. Usuarios

| Rol | Quién es | Qué hace |
|---|---|---|
| **Comprador** | Cualquier persona | Paga, crea el proyecto, invita al protagonista si no es él mismo |
| **Protagonista** | La persona cuya historia se cuenta | Sube fotos, responde el cuestionario, graba su voz, aprueba el guion |
| **Visor** | Familiares | Recibe un enlace para ver el video terminado |
| **Administrador** | El dueño del negocio | Revisa trabajos, reintenta generaciones fallidas, vigila costos |

Comprador y protagonista **pueden ser la misma persona o no**. El caso de dos
personas distintas (un hijo que le regala esto a su padre) debe estar soportado
desde el inicio.

**Restricción de política de uso:** solo se permite crear la historia de uno mismo o
de una persona bajo el cuidado del comprador (hijos, padres mayores). Debe aceptarse
explícitamente al crear el proyecto y estar en los términos de servicio.

---

## 4. Flujo del usuario, paso a paso

```
1.  Llega a la landing               → ve ejemplos, entiende el producto
2.  Descarga gratuita (opcional)     → deja su correo, recibe por email un PDF con
                                       nuestro cuestionario para entrevistar a quien
                                       quiera. Esto es captación de leads, no venta.
3.  Compra                           → paga en línea, se crea el proyecto
4.  Elige protagonista               → "soy yo" o "es otra persona" (+ invitación)
5.  Acepta consentimientos           → uso de imagen, clonación de voz, IA, derechos
6.  Sube fotos guiadas               → 4 fotos, con instrucción y validación de calidad
                                       en cada una
7.  Cuenta su historia               → cuestionario por capítulos. Puede responder
                                       ESCRIBIENDO o GRABANDO AUDIO. El audio es
                                       preferible: menos fricción y sirve de muestra
                                       de voz.
8.  Se genera el guion               → la IA convierte las respuestas en un relato en
                                       primera persona, con el vocabulario de la
                                       persona, dividido en capítulos
9.  Aprueba el guion       ← PUERTA  → el usuario lee, edita y aprueba. NADA se genera
                                       antes de esta aprobación. Es la barrera que
                                       impide quemar dinero en un video equivocado.
10. Generación (asíncrona)           → puede tardar de minutos a horas. El usuario
                                       cierra la ventana y recibe un correo al terminar.
11. Vista previa                     → ve el resultado, puede pedir ajustes acotados
12. Entrega                          → descarga + enlace para compartir con la familia
13. Complementos                     → clips cortos, retratos, álbum, cápsula
```

---

## 5. Pipeline técnico

Este es el corazón del sistema. Se ejecuta como una cadena de trabajos asíncronos.

```
ENTRADA
├── 4 fotos del protagonista (JPEG/PNG, ideal ≥1080px lado corto)
├── Respuestas del cuestionario (texto y/o audio)
└── Muestra de voz (los audios del cuestionario, o una grabación dedicada)

PASO 1 — TRANSCRIPCIÓN
  Audio del cuestionario → texto (speech-to-text)
  Salida: respuestas en texto + audio limpio guardado como corpus de voz

PASO 2 — NARRATIVA
  Respuestas → LLM → guion en primera persona, por capítulos
  Debe respetar: vocabulario y modismos de la persona, idioma (ES o EN),
  duración objetivo, tono. Salida estructurada por bloques con marcas de tiempo
  estimadas y asignación de ángulo de cámara por bloque.
  → APROBACIÓN HUMANA OBLIGATORIA ANTES DE CONTINUAR

PASO 3 — VOZ
  Corpus de voz → clonación → modelo de voz de la persona
  Guion aprobado + modelo de voz → audio narrado, segmentado por bloque
  Salida: N archivos de audio, uno por bloque del guion

PASO 4 — RETRATOS DE ESTUDIO  (imagen → imagen)
  Cada una de las 4 fotos → transformación fotorrealista al set de estudio
  Restricción crítica: persona sin cambios, solo cambia entorno y luz
  Salida: 4 retratos de estudio, consistentes entre sí (mismo set, misma luz,
  misma ropa, misma hora del día)
  → CONTROL DE CALIDAD: ¿sigue siendo la misma persona? ¿son coherentes entre sí?

PASO 5 — ANIMACIÓN + LIP SYNC  (imagen + audio → video)
  Por cada bloque: retrato correspondiente + su audio → clip de video con la
  persona hablando, labios sincronizados, movimiento corporal natural y
  movimiento sutil de cámara
  Salida: N clips de video

PASO 6 — ENSAMBLE
  Clips + fotos de la vida de la persona + títulos + música + corrección de color
  → video final
  Salida: master horizontal + cortes verticales para redes

PASO 7 — ENTREGA
  Almacenamiento, enlace para compartir, correo de notificación, descarga
```

### 5.1 Requisito de consistencia entre tomas (crítico)

Los 4 retratos de estudio deben verse como si fueran **el mismo día, el mismo set y
la misma sesión**. Si la camisa cambia de color, si la luz viene de otro lado, o si
el fondo es distinto entre tomas, la ilusión se rompe por completo y el producto no
sirve.

Esto exige que el paso 4 use el mismo set de imágenes de referencia y el mismo prompt
base para las 4 fotos, y que el control de calidad lo verifique explícitamente.

### 5.2 Puntos de control de calidad automáticos

| Dónde | Qué se verifica | Si falla |
|---|---|---|
| Subida de fotos | Resolución, nitidez, rostro detectable, una sola persona, buena luz | Se rechaza y se pide otra foto, con explicación |
| Muestra de voz | Duración mínima, ruido de fondo, un solo hablante | Se pide regrabar |
| Retratos generados | Parecido facial contra la foto original; coherencia entre las 4 | Se regenera automáticamente hasta N intentos, luego pasa a revisión manual |
| Video generado | Duración, sincronía de labios, no hay artefactos evidentes | Se regenera o pasa a revisión manual |

---

## 6. Arquitectura y stack

Criterio rector: **una sola persona lo mantiene**. Se prefiere lo simple, lo
manejado por terceros y lo que no requiere administrar servidores, aunque cueste
algo de dinero o de elegancia.

| Capa | Elección | Por qué |
|---|---|---|
| Frontend + backend | Next.js (App Router) en Vercel | Un solo repo, un solo despliegue, sin servidores que administrar |
| Base de datos, auth, archivos | Supabase | Postgres + autenticación + almacenamiento en un solo servicio |
| Pagos | Stripe | Estándar, soporta pago único y suscripción para la Cápsula |
| Trabajos largos | Servicio de colas gestionado (Inngest o Trigger.dev) | La generación tarda minutos/horas; **no puede vivir en una petición HTTP**. Es la decisión de arquitectura más importante del proyecto. |
| Correo | Resend | Notificaciones y entrega del PDF gratuito |
| Narrativa (LLM) | Claude vía API de Anthropic | Escritura larga en español e inglés |
| Transcripción | Por definir en el spike | |
| Clonación de voz + TTS | Por definir en el spike | Debe soportar español e inglés con la misma voz |
| Imagen → imagen (retratos) | Por definir en el spike | Candidato principal: Higgsfield |
| Imagen + audio → video | Por definir en el spike | Debe hacer lip sync sobre una imagen de una persona real |
| Ensamble de video | FFmpeg en un worker | Cortes, títulos, música, exportaciones |

### 6.1 Capa de abstracción de proveedores (obligatoria)

Cada paso generativo (imagen, video, voz) debe estar detrás de una **interfaz común**
con implementaciones intercambiables:

```ts
interface PortraitProvider  { generate(photo, styleRefs, prompt): Promise<Image> }
interface TalkingProvider   { generate(image, audio, motion): Promise<Video> }
interface VoiceProvider     { clone(samples): Promise<VoiceId>
                              speak(voiceId, text, lang): Promise<Audio> }
```

Razón: este mercado cambia cada pocos meses. Los proveedores suben precios, cambian
la API, mejoran o empeoran. Amarrar el producto a uno solo es un riesgo grave. Con
la interfaz, cambiar de proveedor es reescribir un archivo, no la app.

### 6.2 Sobre Google Flow

El usuario tiene acceso a Google Flow. **Flow es una herramienta de interfaz gráfica
para uso manual; no expone una API pública para automatizar.** Sirve perfectamente
para hacer pruebas manuales y comparar calidad, pero la app no puede llamarlo por
programa. Para usar los modelos de Google dentro del producto habría que ir por su
API para desarrolladores. Esto debe verificarse en el spike.

---

## 7. Modelo de datos (mínimo)

```
users            id, email, rol, idioma, creado
projects         id, comprador_id, protagonista_id, estado, idioma, plan,
                 pagado_en, creado
consents         id, project_id, tipo (imagen|voz|ia|tercero_a_cargo),
                 texto_version, aceptado_en, ip
photos           id, project_id, slot (A|B|C|D), url_original, url_estudio,
                 qc_estado, qc_notas
answers          id, project_id, pregunta_id, texto, audio_url, transcripcion
script           id, project_id, version, contenido_json, aprobado_por, aprobado_en
voice_models     id, project_id, proveedor, voice_id_externo, creado
jobs             id, project_id, tipo, estado, proveedor, intento,
                 costo_usd, error, inicio, fin
renders          id, project_id, tipo (master|vertical|retrato), url, duracion
deliveries       id, project_id, share_token, expira, vistas
capsule          id, project_id, entregar_en, destinatarios, estado
```

La tabla `jobs` **debe registrar el costo en dólares de cada llamada a un proveedor.**
Sin eso es imposible saber cuánto cuesta realmente un video, y ese número decide si
el negocio es viable.

---

## 8. Requisitos de calidad

**Barra de realismo: A — indistinguible de video real.** Un espectador que no sepa
que es IA no debe sospecharlo.

Criterios de aceptación de un video entregable:
1. La persona en pantalla es reconocible como la persona de las fotos, sin
   deformaciones ni cambios de identidad.
2. Los 4 ángulos parecen la misma sesión de grabación.
3. Los labios coinciden con el audio.
4. La voz suena como la persona, no como un robot ni como otra persona.
5. No hay artefactos visibles: manos deformes, ojos raros, fondo que se derrite,
   parpadeos imposibles.
6. El relato se entiende, tiene principio y final, y suena a esa persona hablando —
   no a una IA escribiendo.

**Si un video no cumple los 6, no se entrega.** Se regenera o se escala a revisión
manual. Entregar un video que casi funciona destruye la marca en una categoría
emocional como esta.

---

## 9. Economía por unidad (a medir, no a suponer)

Precio de venta objetivo: **$1,000 – $2,500 USD** por video.

Costo por video: **desconocido hasta ejecutar el spike.** Se compone de:
transcripción + narrativa (LLM) + clonación de voz + TTS + 4 generaciones de imagen
+ N generaciones de video con lip sync + almacenamiento + comisión de pago +
**los reintentos**, que en generación de imagen y video no son la excepción sino la
norma.

**El número que decide todo el proyecto es el costo real por video terminado,
incluyendo reintentos.** Todo lo demás se acomoda alrededor de ese dato.

---

## 10. Legal y consentimiento

Obligatorio desde el día uno, no después:

1. **Consentimiento de imagen** del protagonista, por escrito y registrado.
2. **Consentimiento específico de clonación de voz**, separado del anterior.
3. **Declaración de que se tiene derecho** a usar la imagen de esa persona (uno mismo
   o alguien a su cargo).
4. **Divulgación de uso de IA** al comprador y en la entrega.
5. **Derechos post-mortem**: qué pasa con el material y con el modelo de voz cuando
   el protagonista fallece; quién puede autorizar su uso.
6. **Derecho de eliminación**: borrar el proyecto, las fotos y el modelo de voz a
   solicitud, de forma verificable.
7. Todo en **español e inglés**.

Contexto regulatorio relevante: leyes estatales de EE. UU. sobre réplica digital de
voz e imagen (Tennessee, California) y obligaciones de transparencia sobre contenido
generado por IA en la Unión Europea. Debe revisarse con un abogado antes de cobrar
al primer cliente.

---

## 11. Riesgos técnicos, en orden de gravedad

| # | Riesgo | Por qué importa | Cómo se prueba |
|---|---|---|---|
| 1 | El parecido facial no aguanta la barra "indistinguible" | Es todo el producto | Spike: 5 personas reales, 4 fotos cada una |
| 2 | Los 4 ángulos no se ven de la misma sesión | Rompe la ilusión multicámara | Spike, mismo ejercicio |
| 3 | El costo real por video es demasiado alto | Puede matar el margen | Medir cada llamada durante el spike |
| 4 | Los reintentos se disparan | Multiplica costo y tiempo | Contar intentos hasta resultado aceptable |
| 5 | La generación tarda horas | Afecta la promesa al cliente | Cronometrar el spike |
| 6 | El movimiento amplio de cámara alrededor del sujeto | Estaba en la idea original; es lo más difícil | Prueba separada, después de la V1 |
| 7 | Dependencia de un solo proveedor | Cambio de precio o de API rompe el negocio | Mitigado por la capa de abstracción |
| 8 | Fotos de mala calidad del usuario | Basura entra, basura sale | Validación estricta en la subida |

---

## 12. Supuestos y decisiones pendientes

**Supuestos que estoy tomando** (corregir si están mal):
- El video final dura entre 5 y 15 minutos.
- El plazo de "3 días a 1 semana" se refiere a construir una primera versión
  funcional, no el producto completo cobrable.
- El mercado inicial es Estados Unidos, en español e inglés.
- No hay equipo: una sola persona construye y mantiene.

**Pendientes de decidir:**
- Duración objetivo del video.
- Precio exacto y qué incluye cada paquete.
- Si la V1 acepta el caso "comprador ≠ protagonista" o solo "soy yo".
- Cuántos ciclos de ajuste incluye el precio antes de cobrar extra.
- Proveedores definitivos de imagen, video y voz.
- Detalle del funcionamiento de la Cápsula.
