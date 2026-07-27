# SONA — el lugar, medido y habitable

**SONA es un proyecto de investigación en diseño** de **Juan Alberto Arias Castillo** (Maestría en Diseño Gráfico, Universidad de Monterrey · Centro Roberto Garza Sada). Estudia cómo los estímulos de un lugar — el ruido, la luz, la cantidad de gente — moldean la experiencia de quienes lo habitan, y cómo comunicarlos con claridad para que cada persona (especialmente la sensorialmente sensible) recorra a su manera.

El principio rector en todo el sistema: **medición, no decoración** — cada comportamiento visual, sonoro o de movimiento corresponde a una lectura nombrable y determinista. Nada aleatorio, nada que no signifique.

## El ecosistema (tres superficies, un ciclo)

```
  el consultor mide ──▶ PANEL · biblioteca de lugares ──▶ acta JSON firmada + QR del lugar
                                                                        │
  el visitante escanea el código en la entrada ──▶ APP (?lugar=CÓDIGO) ◀┘
  la identidad late ◀── INSTRUMENTO (index.html) — el logo como medición
```

| Superficie | Archivo | Quién la usa | Qué hace |
|---|---|---|---|
| **El instrumento** | `index.html` | Equipo SONA / demos / instalación | El logotipo vivo que *representa* la medición (física blanda, etiqueta del análisis, exports) |
| **La app del visitante** | `sona-app.html` | Visitantes del lugar (foco: neurodivergentes) | Lectura sensorial del lugar + elegir espacios según lo que prefieres evitar |
| **El panel de consultoría** | `sona-panel.html` | Solo personal SONA | Biblioteca de lugares: llenado de espacios, código QR por lugar, acta firmada |

**En línea (GitHub Pages):**
- App: https://cristobalguerra.github.io/sona/sona-app.html
- Panel: https://cristobalguerra.github.io/sona/sona-panel.html
- Instrumento: https://cristobalguerra.github.io/sona/

Todo es HTML autocontenido, sin build ni dependencias. `sona-app-share.html` es el build de distribución de la app (variantes del logotipo, foto y fuentes incrustadas, ~345 KB).

---

# La app del visitante (`sona-app.html`)

Diseñada tras un rediseño radical guiado por prueba con visitante primerizo — el flujo, los textos y los anti-patrones están documentados en **[`PRD-app.md`](PRD-app.md)** (objetivo, usuarios, flujos, criterios de aceptación y la lista de lo que no debe repetirse).

## El flujo — "tres toques"
1. **La puerta** — el lugar se presenta: logotipo estampado en el cielo (morph granulado perpetuo), la onda de lectura, la palabra del clima (*Tranquilo*), y el desbloqueo tipo iPhone (la capa sigue al dedo y sube sin fade).
2. **La lectura + Para ti** — la onda del lugar, los 3 estímulos en palabras (chips de vidrio), y la lista de espacios **rankeada según lo que prefieres evitar**. Personalización sin preguntas: tres iconos tachables (oído = ruido, sol = luz, siluetas = gente) con interpretación escrita ("Evitar ruido · mucha gente").
3. **La ficha del espacio** — su onda teñida de su clima, sus estímulos en palabras, foto en mirilla, cómo llegar caminando, y el **campo de flujo**: partículas-viento sobre el fondo desenfocado (gente = cantidad/prisa, sonido = turbulencia, luz = brillo, color = mood).

## El sistema sensorial (todo conectado al clima)
- **Aurora**: el mood como luz de fondo (loops exactos 18/12 s) — tiñe toda la app.
- **Onda** (shader WebGL): tres canales ortogonales — sonido → vibración, luz → luminosidad, gente → hebras y deriva.
- **Sonido** (Web Audio sintetizado, opt-in en "Escuchar lectura"): dron raíz+quinta afinado por clima, murmullo de ruido determinista (LCG), pulso del filtro al tempo del mood.
- **Logotipo estampado**: hundido en el fondo (filtro de relieve invertido), con morph granulado letra-a-letra (3 s, salida retrasada — nunca huecos; turbulencia solo durante el pulso).
- **Liquid glass**: chips, tarjetas y botones son vidrio real — blur del mundo detrás, filo especular, destello; sin rellenos de color.

## Climas y palabras (nunca jerga interna en pantalla)
`Calmado→Tranquilo/a · Equilibrado→Normal · Activo→Con movimiento · Concurrido→Lleno/Llena ahora` — niveles 1–3 por estímulo dichos en palabras (`bajo/medio/alto`, `suave/natural/brillante`, `poca/algo/mucha`).

## Reglas duras (del test con visitante primerizo)
Sin "tap", sin "motor", sin porcentajes sin unidad, sin contadores, **nada se registra jamás** (tocar = ver de cerca; cerrar la app es el cierre), predicciones siempre como cálculo con fuente y frescura, texto esencial ≥11 px, referencias visibles (nunca puntos cardinales). Lista completa en `PRD-app.md §8`.

---

# El panel de consultoría (`sona-panel.html`)

**Herramienta interna** del equipo SONA — la **biblioteca de lugares** de la consultoría (capas 1–2 del llenado). Mismo lenguaje visual que la app (aurora, onda, vidrio).

- **Biblioteca (vista raíz)**: los lugares como tarjetas (punto del clima dominante, nº de espacios, chip del código), **buscador** que filtra por nombre o código (sin acentos ni mayúsculas; si no hay resultados ofrece dar de alta lo buscado) y **alta con +** (crea el lugar, entra y deja el nombre enfocado para escribirlo).
- **Ficha del lugar**: lectura agregada (onda alimentada por los promedios reales; el clima dominante tiñe la aurora), espacios como tarjetas-acordeón (nombre, clima con 4 chips, estímulos en palabras, cómo llegar, señales llena/bancas), añadir y quitar espacios. Quitar el lugar entero pide **doble toque** (es destructivo).
- **El código del lugar**: cada lugar tiene un código estable — auto-derivado del nombre (palabras completas, sin conectores, tope 14) y editable/anclable a mano — y su **QR**: la liga `sona-app.html?lugar=CÓDIGO` que el visitante escanea en la entrada para llegar a la lectura de ese lugar. Generador de QR **propio y embebido** (modo byte, corrección M, versiones 1–10, máscara por penalización; verificado contra el decodificador de OpenCV). Botones: copiar liga y descargar el PNG imprimible.
- **El acta**: persiste en `localStorage` (`sona-panel-v2`, con migración automática desde el panel v1 de un solo lugar) y se exporta como **JSON firmado** — `codigo`, `liga` y `medicion: { consultor, fecha }` — con el formato que consume la app. Esa firma es la cadena de confianza que la app muestra como "medido por el equipo SONA · hace X".

*Pendiente de cableado: la app aún no lee `?lugar=` (trae MARCO horneado); el contrato de la liga ya queda definido desde el panel. Interno por diseño y declaración; si se despliega junto a la app pública necesitará una puerta real (auth o URL no listada).*

---

# El instrumento (`index.html`) — el logo como medición

La plataforma original del sistema vivo: el logotipo **vive representando la medición** de 7 dimensiones. Documentación completa del sistema visual en [`DESIGN.md`](DESIGN.md) y contexto estratégico en [`PRODUCT.md`](PRODUCT.md).

## Las 7 dimensiones → verbos visuales
| Lectura | Comportamiento determinista |
|---|---|
| **Luz** | Peso de la tinta (área ±22%, conservación acompañada) |
| **Pausa** | Tempo global del reloj físico |
| **Sonido** | Pulso radial sinusoidal |
| **Visual** | Tensión de fusión σ de la silueta |
| **Orientación** | Cizalla / congruencia del movimiento |
| **Flujo** | Onda de compresión viajera horizontal |
| **Materia** | Rugosidad periódica del contorno |

## Vistas
- **Etiqueta** (default): la ficha del análisis — cielo del mood con la palabra viva de la biblioteca tipográfica (vidrio líquido, partitura de 8 compases), índice del lugar, estado clasificado (7 moods por prototipos L1, español), medidores, descripción y firma de niveles. Exports PNG / SVG / MP4 vertical en paridad total.
- **Tipografía**: la biblioteca de flancos — s·o·n·a en 4 estados por letra (16 SVGs en `/variantes`, cajas 200×200), ranuras fijas, texto del usuario, exports por compás.

## Motor de física blanda (histórico técnico, vigente en el instrumento)
- **Esqueleto blando** (~608 nodos sembrados en la tinta, ~1900 muelles de cohesión) + **contorno** (~485 puntos, skinning gaussiano, restricciones: distancia, laplaciano de la desviación, anti-deriva, conservación de área). Salida siempre vectorial (Catmull-Rom → Bézier).
- **Constantes protegidas**: radio 25 u · módulo 50 u · lienzo 950×200 u. El reposo ES el vector maestro (residual < 0.1 u).
- Niveles 1–5 por dimensión; **loops exactos de 5 s** (armónicos enteros, costura invisible verificada); rampa narrativa de 6 s identidad→distorsión; `prefers-reduced-motion` congela en la identidad.
- Coalescencia (menisco por campo implícito + succión física con exclusión estructural), fusión pegajosa, tempo cinematográfico ×0.3.
- API: `SONA.leer(dim, nivel)` / `SONA.lecturas()` / `SONA.reposo()` — la puerta para sensores reales.
- Exports: SVG · PNG 1900/3800/7600 · MP4 12 s (H.264, WebM de respaldo) · Copiar SVG.
- Paleta calma vigente: papel `#f1f0ea`, tinta `#1e2126`, **bruma `#7d93ab` único acento**, cielo con grano; una sola rampa para las 7 dimensiones.

## Origen
- `sona.eps` — vector original (Juan Carrillo, AI 30.6, 950×200 pt) · `sona-master.svg` — geometría exacta extraída.

## Límites conocidos
Sin auto-colisión del lazo en extremos; WebM sin canal alfa; `SONA.conducir` usa muelle moderado (usar `impulso` para gestos violentos).

---

# Documentos y prototipos del proceso

| Archivo | Qué es |
|---|---|
| `PRD-app.md` | PRD de la app: objetivo, usuarios, flujos, criterios de aceptación, anti-patrones |
| `DESIGN.md` | Sistema visual del instrumento (tokens, componentes, motion, exports) |
| `PRODUCT.md` | Registro estratégico (brand híbrido, usuarios, principios) |
| `zona-llenado.html` | El llenado del lugar en 5 capas (modelo de datos/servicio) |
| `zona-sona-posicionamiento.html` | Posicionamiento: ZONA diagnostica · SONA trata |
| `zona-flujo.html` | Flujo de sesión original (Sensory Navigation System) |
| `proto-publica.html` | Vista pública / instalación (lámina viva del lugar) |
| `proto-zona-experiencia.html` · `proto-zona-app.html` · `proto-panel.html` · `proto-cartel.html` | Iteraciones del proceso |
| `sona-app-v1-respaldo.html` | Primera versión de la app (pre-rediseño por test de usuario) |

## Servir en local
```
python3 -m http.server 8031 -d /Volumes/Lexar/sona
```
(o `npx http-server -p 8029`; config `sona` registrada en el launch.json de UMBRAL/WEB). **Ojo**: la app y el panel necesitan servirse por http — abiertos como `file://` las variantes del logotipo no cargan (la versión `sona-app-share.html` sí funciona con doble clic: todo va incrustado).
