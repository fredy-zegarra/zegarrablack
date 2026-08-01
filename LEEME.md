# Sitio de campaña — Fredy Zegarra Black (versión reparada v2)

## ✅ Cómo abrir el sitio

**Ahora funciona con doble clic sobre `index.html`.** Ya no es obligatorio usar un
servidor local: si el navegador bloquea la aplicación por abrirse como archivo
(`file://`), la página carga automáticamente una versión alternativa
(`assets/app-local.js`) con exactamente el mismo contenido y el mismo chat.

Los lanzadores locales siguen disponibles por si prefieres ver el sitio igual que
en internet: `iniciar-mac.command` (Mac), `iniciar-windows.bat` (Windows) o
`python3 -m http.server 8000` en una terminal.

Para publicarlo (cPanel, Netlify, GitHub Pages, etc.) sube el contenido de esta
carpeta. Funciona **en la raíz del dominio o en cualquier subcarpeta**, e incluso
si el visitante entra con la URL terminada en `/index.html`.

## 🔧 Qué estaba fallando y qué se corrigió

### 1. El botón del chat (el «RAG») no respondía al presionarlo
Dos causas independientes:

- **Doble clic (`file://`).** Los navegadores bloquean los módulos de JavaScript
  cuando la página se abre como archivo local, así que la aplicación nunca
  arrancaba: el botón se veía, pero estaba muerto. Ahora `index.html` detecta ese
  caso y carga `assets/app-local.js`, una versión en formato clásico que los
  navegadores sí ejecutan desde `file://`.
- **Página fuera de la raíz del dominio.** El enrutador interno solo reconocía la
  ruta `/`; en una subcarpeta (o abriendo `/index.html`) fallaba y **dejaba la
  página en blanco**. Ahora calcula su ruta base automáticamente en cualquier
  ubicación.

### 2. El contenido nuevo se revertía al contenido antiguo
El `index.html` traía el contenido actualizado (gestión organizada por años
2023–2026, 130 semáforos inteligentes, 95.9% de ejecución 2025…), pero el
JavaScript renderizaba una **versión anterior** de la página (300 cámaras, sin
bloques por año). Al terminar de cargar, React detectaba la diferencia y
**reemplazaba el contenido nuevo por el antiguo** ante los ojos del visitante.
Se actualizó el JavaScript para que renderice exactamente el mismo contenido que
el HTML: hoy servidor y navegador muestran lo mismo, sin parpadeos ni reversiones
(verificado con comparación automática: 0 diferencias de texto).

### 3. Rutas absolutas residuales
Quedaban referencias absolutas dentro del JavaScript (`/assets/styles-….css` y
las fotos `/__l5e/…`) que se rompían fuera de la raíz del dominio y generaban
peticiones 404. Se convirtieron a relativas.

## 🤖 Base de conocimiento del chat: datos alineados con la página

El motor del chat no se tocó. Solo se actualizaron las respuestas cuyo dato ya no
coincidía con lo que hoy afirma la propia página (ninguna cifra fue inventada;
todas provienen del contenido del sitio):

- Ejecución presupuestal: se añadió el **95.9% de 2025** (antes solo mencionaba 2024).
- Megaproyecto de seguridad: central de monitoreo con capacidad para **210 cámaras**,
  lectura de placas, fibra óptica, drones autónomos y PAR (antes decía «meta de
  300 cámaras»).
- Obras viales en cinco urbanizaciones: **más de S/ 12 millones** (antes S/ 13).
- Semaforización inteligente: **ya en funcionamiento** —130 semáforos vehiculares,
  182 peatonales y 120 cámaras de flujo, S/ 7 millones vía Obras por Impuestos—
  (antes figuraba como «proyectada»).
- Nueva entrada: **mejoramiento de la Piscina Municipal** (S/ 1.5 millones:
  techado, drenaje pluvial, cuarto de máquinas y tribunas).

## 🧪 Verificación realizada

Probado con un navegador Chromium real en cinco escenarios: dominio raíz,
subcarpeta, subcarpeta con `/index.html` explícito, doble clic (`file://` con
módulos bloqueados) y `file://` con permisos ampliados. En todos: página completa,
botón del chat activo, respuestas correctas con los datos actualizados y consola
sin errores. (Nota: al abrir con doble clic, el navegador registra dos avisos CORS
por los módulos bloqueados; son inofensivos y no visibles para el visitante.)

## 🆕 Actualización de agosto de 2026: consolidado de obras 2023-2026

Se incorporó al sitio el consolidado municipal de obras (34 obras):

- **Nueva sección «Obras»** (`#obras`), con enlace propio en el menú superior.
  Muestra las obras agrupadas por año —8 en 2023, 10 en 2024, 8 en 2025 y 8 en
  2026— cada una con su código (O-1 … O-34), su denominación y su estado
  (Culminado / En ejecución), más un resumen: 34 obras, 29 culminadas,
  5 en ejecución y 10 obras culminadas en 2024 (año de mayor ejecución).
- **Sección «Gestión»**: la Piscina Municipal pasó al bloque de 2025 porque el
  consolidado la registra como culminada (obra O-25); el bloque de 2026 ahora
  resume las 3 obras culminadas y las 5 en ejecución de este año.
- **Base de conocimiento del chat (RAG)**: se añadió el capítulo «Consolidado de
  obras 2023-2026» con siete entradas nuevas (total de obras, obras de cada año,
  obras en ejecución y urbanizaciones beneficiadas) y se alineó la respuesta
  sobre la Piscina Municipal. La base pasó de 74 a 81 preguntas.
- Se regeneró `assets/app-local.js` para que el chat siga funcionando con doble
  clic, y se verificó que el HTML pre-renderizado y el render de React coinciden
  (0 diferencias de texto: no hay reversión de contenido ni parpadeo).

## 📁 Estructura

```
index.html                  ← página principal (doble clic ya funciona)
favicon.ico
assets/
  styles-….css              ← estilos
  index-….js  routes-….js   ← aplicación (módulos, para servidor/hosting)
  app-local.js              ← misma aplicación en formato clásico (respaldo file://)
__l5e/assets-v1/            ← fotos (candidato y logo del partido)
iniciar-mac.command / iniciar.sh / iniciar-windows.bat  ← servidor local opcional
LEEME.md                    ← este archivo
```

## 📝 Nota para la publicación
Las etiquetas `og:image` / `twitter:image` (imagen al compartir el enlace en
redes) siguen apuntando a `/__l5e/…`. Cuando el sitio tenga su dominio
definitivo, conviene reemplazarlas por la URL completa, por ejemplo:
`https://tudominio.pe/__l5e/assets-v1/2763be67-…/zegarra-black.jpg`.

## 🆕 Actualización de agosto de 2026 (2): letra más grande y sugerencias continuas

### 1. Tipografía legible en celular
Se agregó al final de `assets/styles-CyarxWSb.css` un bloque de reglas que **solo
se activa en pantallas de hasta 767 px** (celulares). No modifica nada en
computadora ni en tablet:

| Texto | Antes | Ahora (celular) |
|---|---|---|
| Etiquetas pequeñas (`2023 · Orden y destrabe`, `O-25`, `Culminado`…) | 10–11 px | 12–13 px |
| Texto secundario (`text-xs`) | 12 px | 13 px |
| Texto de tarjetas y listas (`text-sm`) | 14 px | 15 px |
| Párrafos (`text-base`) | 16 px | 17 px |
| Subtítulos (`text-lg` / `text-xl` / `text-2xl`) | 18 / 20 / 24 px | 19 / 21 / 25 px |

También se abrió un poco el interlineado y se redujo el espaciado entre letras de
las etiquetas en mayúsculas, para que no se corten al crecer.

### 2. El chat «Black Smart AI» ahora sugiere después de cada respuesta
- Antes las sugerencias aparecían **solo al abrir el chat** y desaparecían con el
  primer mensaje. Ahora, **después de cada respuesta se muestran 4 sugerencias
  nuevas** bajo el título «También puedes preguntar».
- Las sugerencias son **relacionadas con la pregunta que se acaba de responder**:
  se priorizan preguntas del mismo capítulo de la base de conocimiento
  (seguridad, obras, propuestas, trámites…) y con mayor coincidencia de términos;
  se completa con una pregunta de otro tema para invitar a seguir explorando.
- **No se repiten** preguntas ya respondidas en la misma conversación.
- Si el asistente no entiende bien la consulta, las opciones del mensaje
  «¿Quizás te refieres a…?» ahora aparecen como **botones tocables**.
- Si el tema queda fuera de la base de conocimiento, vuelven las preguntas
  generales (quién es, logros en seguridad, obras, propuestas, elecciones).
- El motor de búsqueda del chat (`s`, `l`, `u`, `zzRank`, `zzOk`) **no se tocó**:
  las sugerencias son una capa nueva (`zzIndex`, `zzRelated`, `zzGuess`, `zzSug`).

### 3. Chat más cómodo de leer y de usar en el celular
- Texto de las burbujas: 15 px en computadora y **16 px en celular**.
- Botones de sugerencia más grandes (14.5 px en celular) y con más aire.
- El campo de escritura usa 16 px: **evita el zoom automático del iPhone** al
  tocar el teclado.
- En celular la ventana del chat ocupa el ancho disponible y ajusta su alto a la
  pantalla (`100dvh`), así no queda cortada por la barra del navegador.

### 4. Verificación
- `assets/app-local.js` regenerado con esbuild (respaldo para abrir con doble clic).
- Prueba de hidratación con jsdom: **0 diferencias de texto** entre el HTML
  pre-renderizado y el render de React (no hay reversión de contenido).
- Prueba automática del chat: se abre, muestra 4 sugerencias iniciales, responde
  al tocarlas y entrega 4 sugerencias nuevas y relacionadas en cada ronda, sin
  errores en consola.

## 🆕 Actualización de agosto de 2026 (3): lectura guiada en el chat

Al tocar una sugerencia (o al enviar una pregunta escrita), el chat ya no salta
al final de la conversación. Ahora:

1. **La pregunta se coloca arriba**, a la vista, apenas se toca.
2. La respuesta queda debajo: el visitante la **lee deslizando el dedo hacia
   arriba**, a su ritmo.
3. **Al final del recorrido aparecen las 4 sugerencias** relacionadas.

Detalles de implementación:
- Cada burbuja de pregunta del visitante lleva el marcador `data-zbq`; tras cada
  envío, la función `zzScroll` calcula la posición de la última pregunta y
  desplaza el área de conversación para dejarla en la parte superior (con 10 px
  de aire).
- Se agregó un **espaciador invisible** al final del área desplazable
  (`aria-hidden`, alto 0 por defecto). Cuando la respuesta es corta y no habría
  suficiente contenido para subir la pregunta hasta arriba, el espaciador crece
  lo justo para que sí se pueda; en respuestas largas queda en 0.
- Con el chat recién abierto (solo el saludo) la vista se mantiene arriba.
- Verificado: hidratación con 0 diferencias, orden en el DOM
  pregunta → respuesta → sugerencias → espaciador, y pruebas de la geometría del
  desplazamiento con respuestas largas y cortas.
