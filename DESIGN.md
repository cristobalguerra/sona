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

EQUILIBRIO ESTRICTO: todas las esferas comparten los mismos parámetros en todo instante — pulsan y respiran EN SINCRONÍA (jamás una más grande), formación perfectamente simétrica (anillo circular puro / fila de espaciado idéntico). TRAYECTORIA: cada esfera orbita INDIVIDUALMENTE el centro invisible sobre un plano HORIZONTAL en perspectiva (radio Ro=r/sin(π/n) — el ancho de todos los círculos juntos, tangentes componiendo el círculo mayor; eje vertical ×0.32); 1 vuelta por loop; pausa = separación angular sobre la órbita (baja = tren de perlas juntas; plena = equidistantes alrededor de toda la trayectoria); orden de pintor por Y (las del fondo detrás). SOMBRAS DE PISO: elipse suave bajo cada esfera en y=274 (rx=r·(0.55+0.40·profundidad), ry=rx·0.20, opacidad 0.10–0.23 según cercanía) — el flotar se lee. GRANO en paridad: overlay del panel en vivo, LCG determinista en canvas (overlay .16), feTurbulence en SVG export (.35 overlay). Círculos con VOLUMEN de esfera: gradiente radial cenital (cx 50% cy 34% r 80%) de `claro` (luz→#93a7ba–#ffffff) a `profundo` (luz→#1f2c39–#8ea2b5) — la base nunca se aclara del todo, así el orbe siempre se despega del cielo. r 44, respiración ±2.5%, siempre centrados, sin tocar bordes. Loop exacto de 4 s (el giro usa coeficiente entero: 1 vuelta/loop). `prefers-reduced-motion` congela. Pipeline compartido vivo/PNG/SVG/MP4, desenfoque incluido en los tres exports.

## Reglas de contraste

Texto sobre papel: tinta plena o tinta ≥.72 alfa (AA). Los metros nunca llevan texto encima; la cifra de la barra va en tinta sobre el extremo niebla. El logo siempre en tinta sobre cielo (contraste sobrado).

## Exports (paridad obligatoria)

Todo cambio visual debe replicarse en: filtro SVG vivo, `svgActual()`, `svgEtiqueta()`, `pintarCanvas()`, `pintarEtiqueta()` (PNG/MP4). La nitidez del contorno: umbral adaptativo `pendienteGoo` (transición ≈1px del raster final) + supermuestreo 3× en vivo.
