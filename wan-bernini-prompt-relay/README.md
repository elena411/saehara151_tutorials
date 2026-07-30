# Wan Bernini with Prompt Relay

Workflow para generar videos más largos manteniendo control sobre cada segmento, sin que el modelo ignore las referencias o cambie el personaje.

## 1. Descargar el workflow

Primero descarga este workflow:

**[Wan Bernini with Prompt Relay](https://civitai.red/models/2815818/wan-bernini-with-prompt-relay-for-longer-video-generation-with-reference)**

Después instala los modelos y los custom nodes necesarios.  
Si el ComfyUI Manager no los instala correctamente, usa `git clone` en la carpeta `custom_nodes`.

> Si no sabes cómo usar `git clone`, consulta este artículo de referencia: [Troubleshooting – Regional Prompter](enlace-si-lo-tienes)

## 2. Conectar las imágenes de referencia

Cuando abras el workflow por primera vez, las imágenes de referencia **parecen** conectadas, pero no lo están.

Debes conectarlas manualmente. Una vez lo hagas, guarda el workflow y quedará corregido de forma permanente.

![Conexión correcta de las imágenes de referencia](./screenshots/01-conexiones-referencia.png)

## 3. Mantener la relación de aspecto

Es fundamental conservar la misma relación de aspecto de la imagen de referencia.

- Si la imagen es 1024×1024 (cuadrada), el video también debe ser cuadrado.
- El tamaño por defecto del workflow es 512×512. Mientras se esté probando, es preferible no modificarlo demasiado.
- Resoluciones muy bajas (206×206) se ven mal. Resoluciones altas (1024×1024) pueden saturar el hardware.

## 4. Los prompts (la parte más importante)

Después de muchas pruebas, esta es la forma correcta de usarlos:

- **Global prompt**: solo los prompts de la imagen de referencia.
- **Segment prompts**: únicamente los movimientos, sin etiquetas de calidad ni de estilo.

Copia los prompts positivos del PNG Info y elimina:

- Los paréntesis de peso
- Los `BREAK`
- Los nombres de embeddings y LoRAs

Este es el **único nodo** que debes modificar.

### Formato de los segment prompts

En la parte superior van los prompts de movimiento, separados por `|`:
action1. | action2. | action3. | action4.
textEjemplo usado en este tutorial:
woman's hair begins to sway softly in the breeze, water ripples more noticeably. | woman slowly starts smiling, soft expression, hair continues moving with the wind. | woman blinks once naturally, smile remains, clouds drift slowly in the background, air flowing with the breeze, water swaying | soft smile, hair still moving lightly, calm water movement, clouds slowly shifting.
textEn la parte inferior va el **global prompt** (el de la imagen de referencia).

En el campo **type** elige:

- `r2v` → reference to video  
- `i2v` → image to video

![Nodo Prompt Relay configurado](./screenshots/02-prompt-relay.png)

## 5. Si el nodo se ve diferente

Después de actualizar ComfyUI, el workflow original puede cambiar y el Prompt Relay puede quedar dentro de un subgraph.

En ese caso:

- Cada segmento se separa por colores.
- Puedes cambiar la duración de cada segmento arrastrando con el ratón.
- Puedes añadir o eliminar segmentos con los botones inferiores.

![Prompt Relay dentro de subgraph](./screenshots/03-subgraph.png)

<video src="./videos/05-subgraph-duration.mp4" controls width="700"></video>

## 6. Problema con la cara y solución

En las primeras pruebas el video cambiaba el rostro del personaje (Eto-chan).

**Imagen de referencia original:**

![Imagen original](./screenshots/04-referencia-original.png)

**Resultado incorrecto:**

<video src="./videos/06-resultado-incorrecto.mp4" controls width="500"></video>

**Solución aplicada:**

Se añadió una segunda imagen de referencia del rostro y se mejoraron los ojos con ADetailer.

**Resultado final correcto:**

<video src="./videos/07-resultado-final.mp4" controls width="500"></video>

---

### Notas finales

- Guarda el workflow una vez hayas conectado correctamente las referencias.
- No modifiques el resto de nodos innecesariamente.
- Si el hardware es limitado, mantén resoluciones moderadas mientras pruebas.
