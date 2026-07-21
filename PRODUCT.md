# Product

## Register

brand

*(Nota de registro: proyecto híbrido. El escenario y la etiqueta —lo que ven clientes, visitantes e instalaciones— son superficie de MARCA: el diseño ES el producto. El panel lateral de control es superficie de PRODUCTO y se evalúa con ese registro cuando se trabaja sobre él.)*

## Users

- **Cristóbal y el equipo del proyecto SONA** (diseñadores): miden lugares (lobby de museo, aeropuerto…), fijan los 7 niveles, escriben la descripción y exportan la etiqueta / los assets. Contexto: escritorio, sesiones de trabajo de diseño.
- **Clientes y stakeholders**: ven la plataforma en vivo como demo del sistema de identidad (p. ej. la dirección del museo). Contexto: presentación proyectada o en laptop; primeras impresiones cuentan.
- **Instalación pública**: la vista del logo/etiqueta corre sin operador en pantallas del lugar medido, en loop. Debe verse impecable indefinidamente sin intervención (loops de 5 s, rampa de arranque, sin degradación).

## Product Purpose

SONA mide 7 dimensiones de un lugar (luz, pausa, sonido, visual, orientación, flujo, materia) y su identidad **vive representando esa medición**: el logotipo es un instrumento. La plataforma es donde esa identidad se opera: fijar lecturas, observar la respuesta física del logo (masa viscosa determinista, loops exactos de 5 s) y producir los entregables — etiqueta del análisis (PNG/SVG/MP4), assets del logo. Éxito = cada lugar medido produce una ficha única, hermosa y reproducible; el sistema se siente vivo pero nunca aleatorio.

## Brand Personality

Instrumento sensible, calma crepuscular, precisión viva. Voz: técnica-poética en español (etiquetas mono en caps, valores como palabras: *mínima… plena*). Emociones meta: contemplación, confianza en la medición, asombro contenido. Referencia confirmada por el usuario: la app de atardeceres (cielo degradado con grano, tarjeta crema, numerales ligeros, medidores de gradiente) — de ella se toma la esencia atmosférica, no el contenido.

## Anti-references

- **Movimiento orgánico-aleatorio**: nada de ruido sin estructura; todo movimiento es determinista, nombrable y en loop ≤5 s (pedido explícito del usuario).
- **Desenfoque o pixelación en los contornos**: el delineado del logo es sagrado — nítido a cualquier resolución (3 iteraciones de historia sobre esto).
- **Dashboard genérico de SaaS**: la plataforma no debe leerse como admin panel; es un instrumento de laboratorio con temperatura.
- **Sobredecoración**: el logo vivo es el espectáculo; la UI lo enmarca, no compite.

## Design Principles

1. **El reposo es la identidad**: toda animación nace del logotipo base exacto (R25/módulo 50) y puede regresar a él con residual cero. Nada rompe las constantes.
2. **Medición, no decoración**: cada comportamiento visual corresponde a una dimensión medida; si no representa una lectura, no se agrega.
3. **Determinista y reproducible**: mismos niveles = misma pieza; los exports son actas de la medición (llevan firma de niveles y fecha).
4. **La atmósfera es cálida, el dato es preciso**: gradiente y grano para el clima; mono en caps y numerales ligeros para la lectura.
5. **Sin operador también funciona**: loops perfectos, arranque narrativo automático, dormancia — la instalación pública es un usuario de primera clase.

## Accessibility & Inclusion

- Estándar razonable: contraste AA (4.5:1) en textos del panel y de la etiqueta; verificar especialmente texto sobre gradientes.
- `prefers-reduced-motion`: **pendiente de implementar** — la simulación debería ofrecer estado quieto (rampa congelada en la identidad o amplitudes a mínimo) cuando el usuario lo prefiera.
- El significado nunca depende solo del color: los niveles se leen también como palabra (*mínima–plena*) y posición del marcador.
