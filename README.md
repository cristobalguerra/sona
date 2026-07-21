# SONA · Sistema vivo — el logo como instrumento

SONA mide 7 dimensiones de un lugar (p. ej. el lobby de un museo). Esta plataforma hace que la identidad **viva representando esa medición**: cada lectura conduce un comportamiento bien definido y determinista del logotipo — nada orgánico, nada aleatorio — y todas actúan a la vez. Archivo único (`index.html`), sin dependencias ni build.

## Las 7 dimensiones → 7 verbos visuales
| Lectura (0–100) | Comportamiento determinista |
|---|---|
| **Luz** | Peso de la tinta: oscuro = masa densa (+22% área), luminoso = trazo ligero (−22%) — la conservación de área acompaña vía dA/dn precomputado |
| **Pausa** | Tempo global del reloj físico (mucha pausa = casi inmóvil) |
| **Sonido** | Pulso: latido radial sinusoidal (~9.5 s por ciclo real), amplitud = nivel |
| **Visual** | Tensión de fusión σ (más estímulo = silueta más fundida) |
| **Orientación** | Cizalla estática: mala orientación = la masa se ladea |
| **Flujo** | Onda de compresión sinusoidal viajera, solo horizontal (λ=340 u) |
| **Materia** | Rugosidad periódica del contorno (grano por la normal, λ≈45 u) |

Los canales del esqueleto (orientación, flujo, sonido) y del contorno (luz, materia) se superponen; la **mezcla** es la ganancia global del instrumento y las 7 lecturas son manuales hoy (sliders) y sensores mañana: `SONA.leer("luz",72)` / `SONA.lecturas()`. Con todas las lecturas en 0 el instrumento duerme en su identidad exacta. Los materiales V1–V5 fueron reemplazados por este sistema (base física única BASE_FIS, temperamento slime). El ruido simplex se eliminó por completo.

## Piezas (el universo de la marca)
Selector **00 Pieza**: el logotipo + tres formas básicas, cada una un cuerpo independiente con exactamente los mismos parámetros físicos (material, mezcla, tempo, fusión — todo compartido):
- **SONA** — wordmark 950×200 (70 nodos, 485 puntos).
- **● Círculo** — r100 en lienzo 400×400 (20 nodos).
- **■ Cuadrado** — 200×200, esquinas R25 (28 nodos).
- **▲ Triángulo** — equilátero h200, esquinas R25 (15 nodos).
La geometría de cada pieza se construye con la misma fábrica (remuestreo 9 u, siembra en retícula de módulo 50, tejido por dentro de la tinta) y se cachea. Los previews de material, exports y retícula siguen a la pieza activa; los archivos exportados llevan el id de la pieza en el nombre.

## Origen
- `sona.eps` — vector original (Adobe Illustrator 30.6, Juan Carrillo, 950×200 pt).
- `sona-master.svg` — geometría exacta extraída del EPS (un path compuesto, 3 subtrazados).

## Constantes de identidad (protegidas)
- **Radio de redondez: 25 u** · **módulo: 50 u** · **lienzo: 950×200 u**.
- El reposo del sistema ES el vector maestro (residual < 0.1 u, verificado).

## Motor (dos capas)
1. **Esqueleto blando** — ~608 nodos internos en el logo (retícula de siembra 11.2 u; las demás piezas escalan por su área de tinta), unidos por ~1900 muelles de cohesión (las aristas corren por dentro de la tinta, sin atajos por contadores; alcance 21 u, kInt 42, amortiguamiento relativo 0.11c). Cada nodo: masa, muelle de retorno al reposo, amortiguamiento. Solo los nodos reciben fuerzas (dimensiones, API).
2. **Contorno** — ~485 puntos remuestreados uniformemente (9 u) del maestro. Cada punto sigue un objetivo *skinned* por influencia gaussiana `exp(−d²/(2σ²))` normalizada (σ fija ≈ 46 u), con retardo viscoso. Después, 3 iteraciones de restricciones:
   - distancia entre vecinos (reparte tensión, evita cortes),
   - suavizado laplaciano **de la desviación** (mata picos sin limar las R25),
   - anti-deriva de centroide (el lazo no "camina"),
   - conservación de área por subtrazado (presión por la normal, contadores incluidos).
   Reconstrucción Catmull-Rom → Bézier cúbicas: salida siempre vectorial.

## Conducción (sin cursor)
La manipulación directa con cursor (arrastrar/empujar) fue retirada — la masa se conduce solo por:
- **Deriva autónoma**: ruido lento sobre los objetivos de los nodos (estructurado por la física; nunca por punto). SOLO horizontal: el campo viaja en x como una corriente que atraviesa la palabra — compresión y deslizamiento lateral, sin ondulación vertical (verificado: contorno 38 u en x vs 4.9 u en y).
- **API externa**: `SONA.nodos() / conducir(k,dx,dy) / liberar(k) / impulso(k,vx,vy) / reposo() / params` — la puerta para datos externos (audio, scroll, sensores).

## Parámetros (panel Física)
elasticidad · viscosidad · amortiguamiento · masa · radio de influencia · resistencia al estiramiento · conservación de área · velocidad de retorno · suavizado de curvatura · elongación máx · **tensión de fusión**.

## Niveles 1–5 y loop de 5 s (2026-07-18 — estado actual)
Pieza única: el logotipo (abecedario y formas retirados para máximo control). Cada dimensión se fija en un **nivel 1–5** (celdas-medidor; mapeo interno 0.1/0.3/0.5/0.75/1). Todo movimiento es **periódico con ciclo exacto de 5 s reales**: las fases usan armónicos enteros (pulso de sonido = 2 latidos/ciclo, onda de flujo = 1 λ/ciclo, grano de materia = 1/ciclo) sobre un reloj real independiente del tempo — verificado: diferencia 0.17 u tras un ciclo (costura invisible). "Volver al reposo" resetea la fase → tomas reproducibles. El nombre de export codifica la lectura: `sona-n3422532`. API: `SONA.leer("sonido", 4)`.

## Histórico — Abecedario y escritura libre (retirado)
Campo de texto en la sección Pieza: escribe cualquier palabra (a–z + espacio, máx 14) y se construye como pieza viva con la misma física. Los 26 glifos están definidos como **esqueletos de trazos de 50 u sobre la retícula de 25** (la lógica original del wordmark); el contorno se genera con un campo de distancia suave —las uniones entre trazos quedan redondeadas ≈R25— y marching squares (res 2.5 u, RDP 0.45, orientación exterior/contador normalizada). El punto de la i/j es una gota independiente. Glifos cacheados; piezas de texto cacheadas (14). Exporta como `sona-txt-<palabra>`.

Nota de calibración (mismo día): al simplificar se había perdido la presencia del movimiento — ganancia horneada subió a 0.5 y amplitudes de canal recalibradas (cizalla 0.45, pulso 0.11@0.5 Hz, onda 60 u @1.3 rad/s, materia 9): amplitud visible ~28 u con lecturas por defecto.

## Paleta calma (2026-07-20 — vigente)
Rediseño cromático a petición: pocos colores, calma. Hora azul/niebla: papel `#f1f0ea`, tinta `#1e2126`, **bruma `#7d93ab` como único acento**, niebla `#ccd4da`; cielo `#8fa0b4→#e3e0d8` con grano. Las 7 dimensiones comparten una sola rampa (niebla→bruma) — instrumento de una escala. Puntos decorativos eliminados. `prefers-reduced-motion` implementado (identidad congelada; la grabación reactiva). Contexto estratégico en `PRODUCT.md` y sistema visual en `DESIGN.md`.

## Look atardecer (2026-07-20 — histórico, reemplazado por Paleta calma)
Lenguaje visual tomado de una referencia de app de atardeceres: cielo degradado (#8398c2→#e2764a) con grano (feTurbulence overlay) como atmósfera del escenario y de la ficha; acentos escarlata/naranja/lavanda (los 3 puntos); Hanken Grotesk 200–300 para numerales grandes; UI redondeada y suave. Cada dimensión tiene su gradiente propio (DIM_COL) — las celdas del panel y los metros verticales de la etiqueta lo usan. La etiqueta ganó: índice del lugar (promedio de niveles, %), palabras de nivel (mínima→plena), sol de líneas, barra inferior de gradiente y fecha. Exports en paridad total.

## Etiqueta del espacio (2026-07-19, del sketch del usuario)
Segunda vista de la plataforma (toggle **Vista: Logotipo | Etiqueta**): una ficha de análisis tipo etiqueta — marco, el logo VIVO arriba (el mismo SVG animado se muda dentro de la tarjeta), los 7 medidores de nivel al centro, "Descripción del espacio" (textarea en el panel, sección 02) y pie con firma de lecturas en notación de niveles (`luz·3 pau·4 …`). Exports propios: **PNG** (tarjeta 700×1000 escalada, tope 3800 px), **SVG** completo (textos + logo con filtro nítido anidado) y **MP4 vertical 1080×1543** con el arco de 12 s. Los niveles también se pueden fijar clicando las celdas de la propia etiqueta.

## Plataforma simplificada (2026-07-18)
El panel quedó en 4 secciones: **Pieza · Dimensiones · Color · Exportar** (+ botón "Volver al reposo"). La física está horneada: ganancia fija 32% (`SONA.ganancia(v)` la ajusta por código), fusión pegajosa siempre activa, sin sliders de ajuste fino ni toggles de depuración en la UI (esqueleto/retícula/fantasma siguen en el DOM, accesibles por consola). El movimiento viscoso ES la base del proyecto.

## Mezcla — ahora interna (histórico)
Un solo macro **MEZCLA 0–100%** gobierna el temperamento completo: a 0 la identidad respira contenida; a 100 no hay fuerza de retorno, los nodos vagan ±160 u, los contadores colapsan y la palabra se amasa hasta charco irreconocible. Los 11 parámetros finos siguen bajo "Ajuste fino" (colapsable) y son editables encima del macro.

**Fusión pegajosa** (toggle, ON): cada contacto entre gotas crea una soldadura persistente entre nodos del esqueleto (muelle kBond=120, máx 90 soldaduras). Solo "Volver al reposo" (o apagar el toggle) las deshace — verificado: reposo restaura la identidad con residual 0.

**Tempo cinematográfico**: el reloj completo de la simulación corre a ×0.3 (ajustable en "Ajuste fino" → Tempo, 0.1–1). Toda la física —deriva, arrastre, succión, retorno— ocurre en cámara lenta coherente: ~1.4 u/s de movimiento medio (11× más lento que tiempo real, verificado). El video de 8 s captura ese metraje lento tal cual.

## Coalescencia (gotas que se funden)
Dos capas complementarias controladas por "tensión de fusión" (σ en u, 0 = off):
1. **Menisco visual**: filtro campo-implícito (feGaussianBlur σ + umbral de alfa 24/−12). Matemática clave: blur+iso-0.5 deja bordes rectos y radios R25 idénticos en reposo; superficies a <~1.2σ forman puente líquido. Replicado en PNG/WebM vía canvas (blur + umbral de píxeles) y horneado en el SVG exportado.
2. **Succión física**: hash espacial del contorno detecta pares de superficie a <2.2σ; la fuerza se enruta al esqueleto vía los pesos de skinning. Exclusión estructural: pares ya cercanos en el máster (contadores, puentes, letras adyacentes) no se atraen — SALVO que ambos puntos estén en excursión >35 u de su reposo (dos gotas desplazadas que se tocan sí se funden). En reposo la fuerza es exactamente cero.
Al soltar, la elasticidad separa el puente y la identidad se recupera.

## Materiales
5 presets del mismo sistema: **V1 Denso · V2 Melaza · V3 Slime (default) · V4 Magma · V5 Goma**.

## Exportar
SVG (frame vectorial con metadatos) · PNG 1900/3800/7600 px (fondo opcional/transparente) · **Video MP4 12 s** (H.264 vía MediaRecorder; WebM como respaldo si el navegador no muxea mp4) · Copiar SVG.

## Arco narrativo (rampa)
Toda animación nace del logotipo/forma base: al cargar, al cambiar de pieza, al volver al reposo y SIEMPRE al grabar, una rampa smoothstep de 6 s funde de la identidad intacta hacia la distorsión elegida (multiplica la amplitud de conducción, no los parámetros), y ahí se queda fluctuando. El video de 12 s captura el arco completo: 6 s identidad→distorsión + 6 s de fluctuación. Verificado: fRampa 0→1 en 6 s, MP4 H.264 de 12.5 s @1400px descargado.

## Límites conocidos
- Sin auto-colisión: con elongación máxima alta y arrastres cruzados extremos, un lazo puede auto-intersecarse (la conservación de área usa área firmada neta). El límite de elongación lo hace raro en uso normal.
- El video WebM no admite canal alfa: al grabar con fondo transparente se aplica el color de fondo.
- La API `SONA.conducir` usa un muelle moderado: para gestos más violentos usar `SONA.impulso`.

## Servir
```
npx http-server /Volumes/Lexar/sona -p 8029
```
(config `sona` registrada en el launch.json de UMBRAL/WEB).
