# Design

Sistema visual de la plataforma SONA — instrumento vivo de identidad. Registro: brand (escenario/etiqueta) con panel en registro producto. Escena rectora: *museo entre semana a las 9 am, niebla afuera, salas en silencio*.

## Tema

Claro, atmosférico, de calma. Una sola familia cromática (bruma azul-gris) sobre papel neutro-frío; la tinta del logo es el único negro. El grano de película (feTurbulence overlay, opacidad .5, blend overlay) da el clima orgánico sobre los degradados. Nada de puntos decorativos ni multicolor: el instrumento tiene UNA escala.

## Colores

| Token | Valor | Uso |
|---|---|---|
| `--papel` | `#f1f0ea` | Fondo de panel, cuerpo de etiqueta, exports |
| `--tinta` | `#1e2126` | Textos y marcos del PANEL (superficie producto) |
| tinta del logo | `#3d4e60` | El logo y la tinta de la etiqueta — bruma profunda, nunca negro ("muy brusco" — decisión del usuario). Default de `est.tinta` |
| `--bruma` | `#7d93ab` | Acento único: extremo alto de la escala, barra de índice |
| `--niebla` | `#ccd4da` | Extremo bajo de la escala, superficies suaves |
| `--acento` | `#6b84a0` | Estados interactivos (hover/focus/REC) |
| `--hair` | `rgba(30,33,38,.14)` | Líneas divisorias |
| `--cieloGrad` | `180deg: #8fa0b4 → #aebbc6 42% → #cfd4d4 74% → #e3e0d8` | Cielo del escenario y de la etiqueta |

Rampa de medidores (las 7 dimensiones comparten): `RAMPA_MET = #d6dbdf → #7d93ab` (interpolación por celda con `mezclaHex`). Barra de índice: mismo gradiente horizontal. **Regla: no introducir nuevos matices** — si algo necesita color, usa la rampa o la tinta.

## Tipografía

- **Fragment Mono** (400): etiquetas técnicas, caps con tracking `.12–.18em`, tamaños 8.5–11px; descripciones 11–11.5px/1.7.
- **Hanken Grotesk** (200–300): numerales y palabras de dato grandes — Índice del lugar 58px w200, palabras de nivel 21px w300, cifra de barra 22px w300. Nunca para texto corrido.
- Palabras de nivel: `mínima · baja · media · alta · plena` (capitalizadas en la etiqueta).

## Componentes

- **Medidor de panel** (`.niv`): 5 celdas píldora (r8, alto 16) entintadas con la rampa; apagadas al ~17% alfa. Click fija nivel; hover brightness+scaleY.
- **Metro de etiqueta** (`.metro`): píldora vertical 8×44 con gradiente niebla→bruma (alto arriba) y marcador de tinta 2.5px en el nivel. Click cicla el nivel.
- **Etiqueta** (`#etiqueta`): tarjeta 430px, r26, sombra `0 24px 60px rgba(30,33,38,.18)`; cielo 190px con grano y el logo VIVO (mismo SVG re-parentado, scale .28); cab ANÁLISIS DEL LUGAR + fecha; héroe Índice (promedio de niveles, %); sol de líneas (icono de abanico); dims en 2 columnas; descripción; barra de índice r14; pie con firma `luz·3 pau·4 …`.
- **Botones** (`.b`): mono caps 10px, borde 1px tinta, r10; lleno = tinta/papel.
- **Segmentado** (`.seg`): r10, activo en tinta.
- **Escenario**: cielo degradado + grano a viewport completo; el logo (supermuestreo 3×: svg 300% en `#lupa` scale .3333) flota centrado.

## Motion

- Física determinista en **loops exactos de 5 s** (armónicos enteros sobre reloj real); tempo cinematográfico ×0.3 por defecto (gobernado por Pausa).
- Rampa narrativa: 6 s smoothstep de identidad → distorsión en cada arranque/reposo/grabación.
- `prefers-reduced-motion`: la masa queda congelada en la identidad exacta (rampa 0); transitions CSS a .01ms. La grabación de video reactiva el movimiento (petición explícita).
- Easing UI: transiciones cortas (.12–.25s); sin bounce.

## Moods: los 7 estados del lugar (2026-07-21)

Las 7 lecturas se clasifican en un ESTADO por prototipos (distancia L1 a firmas características, determinista) y el estado tiñe la etiqueta completa (cielo, orbes, barra) con su familia silenciada:

| Estado | Familia | Firma característica |
|---|---|---|
| **Calmado** | Azul bruma | luz y pausa plenas + flujo bajo |
| **Activo** | Ámbar | sonido pleno + orientación alta |
| **Ajetreado** | Teal | actividad con orden, pausa alta |
| **Concurrido** | Terracota | flujo alto + pausa mínima + materia alta |
| **Equilibrado** | Verde salvia | balance general + orientación plena |
| **Dinámico** | Lavanda | orientación mínima + flujo alto |
| **Inestable** | Grafito | ruido total: sonido/visual/materia altos sin orientación |

Nombres en ESPAÑOL ("español todo", 2026-07-21; ids internos en inglés estables); el estado tiñe TODO: escenario, cielo, papel, orbes, barras, metros.

El nombre del estado es dato-héroe (reemplazó al sol decorativo). MOODS[]/moodLugar() en código; exports en paridad.

## El organismo (gramática del círculo — v2 definitiva)

Todo en mínimo = el puro círculo individual existiendo. Diccionario dictado por el usuario (2026-07-20, referencia: racimo de orbes suaves):

| Dimensión | Verbo visual |
|---|---|
| **Flujo** | Cuántos círculos aparecen (1–5, racimo como la referencia) |
| **Pausa** | Separación sobre la órbita: racimo viajando junto → equidistantes, cada uno con su espacio |
| **Orientación** | Sentido de la órbita: plena = constante a la derecha; baja = vaivén que invierte la dirección |
| **Visual** | Nitidez: plena = nítidos; baja = desenfocados (blur hasta 13 u) |
| **Sonido** | Pulsaciones (expansión rítmica, 2 por ciclo) |
| **Luz** | Entre más, más blancos (#2e3d4d → #f2f4f6) |
| **Materia** | Densidad de la tinta (opacidad 0.28→1) |

CAPA RADIAL (aditiva, 2026-07-21 — "esto no sustituye nada, se suma"): la DISTANCIA AL CENTRO respira — cada círculo se expande hacia afuera o se recoge hacia adentro sobre su radio orbital (`radial=1+ampR·sin(2t+(1−cong)·i·2.1)`, `ampR=0.05+0.13·son`). Los parámetros sensoriales siguen mandando: sonido = amplitud del aliento (base 5% siempre viva → ±18% pleno); orientación = congruencia (plena = todos en sincronía, la formación entera se hincha y se recoge; baja = desfases propios, unos salen mientras otros entran). Armónico entero → el loop sigue cerrando exacto; la membrana metaball se estira y se recoge SIN romperse (1 lazo en ambos extremos, verificado). EQUILIBRIO ESTRICTO: todas las esferas comparten los mismos parámetros en todo instante — pulsan y respiran EN SINCRONÍA (jamás una más grande), formación perfectamente simétrica (anillo circular puro / fila de espaciado idéntico). TRAYECTORIA: cada esfera orbita INDIVIDUALMENTE el centro invisible sobre un plano HORIZONTAL en perspectiva (radio Ro=r/sin(π/n) — el ancho de todos los círculos juntos, tangentes componiendo el círculo mayor; eje vertical ×0.32); 1 vuelta por loop; pausa = separación angular sobre la órbita (baja = tren de perlas juntas; plena = equidistantes alrededor de toda la trayectoria); orden de pintor por Y (las del fondo detrás). SOMBRAS DE PISO: elipse suave bajo cada esfera en y=274 (rx=r·(0.55+0.40·profundidad), ry=rx·0.20, opacidad 0.10–0.23 según cercanía) — el flotar se lee. GRANO en paridad: overlay del panel en vivo, LCG determinista en canvas (overlay .16), feTurbulence en SVG export (.35 overlay). Círculos con VOLUMEN de esfera: gradiente radial cenital (cx 50% cy 34% r 80%) de `claro` (luz→#93a7ba–#ffffff) a `profundo` (luz→#1f2c39–#8ea2b5) — la base nunca se aclara del todo, así el orbe siempre se despega del cielo. r 44, respiración ±2.5%, siempre centrados, sin tocar bordes. Loop exacto de 4 s (el giro usa coeficiente entero: 1 vuelta/loop). `prefers-reduced-motion` congela. Pipeline compartido vivo/PNG/SVG/MP4, desenfoque incluido en los tres exports.

**FUSIÓN — una forma nueva (2026-07-21).** Los círculos no se conectan: se MEZCLAN. Unión implícita metaball `f(p)=Σ (r²/d²)²` con iso=1 — la caída AFILADA (exponente 2, ajuste "margen menor, más pegado a los círculos") hace que la membrana abrace los círculos: cinturas ajustadas en vez de colchón; exponente 3 se descartó porque la membrana desaparece y las esferas vuelven a leerse separadas — el contorno único se extrae por marching squares cada frame (res 4 u, ~0.6 ms) y se suaviza con Catmull-Rom. Donde dos cuerpos se acercan el contorno se funde con redistribución de volumen (cinturas y bulbos que ningún círculo tenía solo), continuidad suave, tensión superficial; lejos, cada uno recupera su círculo exacto (error < 0.2 u). Render en capas: la masa (`#pecMasa`) se llena con el gradiente vertical de luz cenital (`gradPuente`: mezcla(claro,profundo,0.30) → profundo) y los orbes volumétricos viven como BRILLOS perlados recortados dentro de la masa (`clip-path #clipMasa`) — cada lóbulo conserva su perla, las cinturas muestran la membrana. Sombras, grano, blur y densidad intactos. Paridad: canvas usa `Path2D` + `ctx.clip()`, SVG export `clipPath #clipMx`. Siempre UNA forma (tren, roseta, pares y solo: 1 lazo).

## Tipografía modular (apartado 03, 2026-07-21)

Sistema tipográfico paramétrico: **cada letra es datos, no trazos**. Lienzo 200×200 por letra, retícula fija 4×4 (celdas de 50). Módulo base = cápsula `[x,y,w,h]` con radio SIEMPRE la mitad del lado corto (módulo 200×50 → r 25); grosor de trazo constante 50. Alfabeto en `TIPO_LETRAS` (unicase, sin diagonales — abstracción ortogonal tipo fuente modular).

- **Tracking** base −20 (permite conexión), rango −50…80.
- **Conexión fluida**: se activa cuando la distancia horizontal entre módulos vecinos ≤ `maxDistance` (80) Y hay solapamiento vertical. Grosor: `8 + (50−8)·(1 − dist/maxD)^1.5`. Puente reloj-de-arena (entra 22 u dentro de cada módulo para cubrir la curvatura de las tapas); `curveIntensity` da la profundidad de la curva. Un módulo interpuesto bloquea la conexión. En costuras (solape ≤ 24) el filete solo procede entre barras horizontales de la misma banda.
- **Parámetros expuestos**: cellSize (escala física del export), tracking, maxDistance, curveIntensity, tinta y fondo (colores propios del apartado — la etiqueta sigue sin edición de color).
- **Salidas**: SVG prioridad (rects rx + path de puentes, viewBox con margen 50) · Canvas/PNG (roundRect + Path2D) · Copiar SVG. Video no aplica (estático).
- **Criterio de éxito cumplido**: palabras legibles Y gráficos abstractos con los mismos parámetros (tracking −50 + maxD 160 teje la palabra en una masa líquida).

## Reglas de contraste

Texto sobre papel: tinta plena o tinta ≥.72 alfa (AA). Los metros nunca llevan texto encima; la cifra de la barra va en tinta sobre el extremo niebla. El logo siempre en tinta sobre cielo (contraste sobrado).

## Exports (paridad obligatoria)

Todo cambio visual debe replicarse en: filtro SVG vivo, `svgActual()`, `svgEtiqueta()`, `pintarCanvas()`, `pintarEtiqueta()` (PNG/MP4). La nitidez del contorno: umbral adaptativo `pendienteGoo` (transición ≈1px del raster final) + supermuestreo 3× en vivo.
