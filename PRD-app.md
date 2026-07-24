# PRD — SONA · La app del visitante

**Versión:** 1.0 · 23 jul 2026
**Prototipo de referencia:** `sona-app.html` (v2 "tres toques"; v1 descartada respaldada en `sona-app-v1-respaldo.html`)
**Documentos hermanos:** `PRODUCT.md` (plataforma/identidad), `DESIGN.md` (sistema visual), `zona-llenado.html` (llenado del lugar), `zona-sona-posicionamiento.html` (posicionamiento)

---

## 1 · Objetivo

Que cualquier visitante —especialmente una persona sensible a los estímulos— sepa en menos de 5 segundos **cómo se siente cada espacio del lugar** (ruido, luz, gente) y pueda armar su visita a su manera, **sin cuenta, sin registro y sin ser seguida**.

La app es la cara pública del sistema SONA: la medición sensorial del lugar (las 7 dimensiones del instrumento) traducida a palabras de persona. El posicionamiento en una frase: *SONA es quien te dice lo que ningún museo te dice — cuánto ruido, cuánta luz, cuánta gente.*

**No-objetivos v1:** navegación GPS en interiores; cuentas o perfiles persistentes; gamificación; mensajería; analítica individual de visitantes.

---

## 2 · Usuarios

| Rol | Quién | Qué necesita | Qué NO quiere |
|---|---|---|---|
| **Primario** | Visitante sensorialmente sensible: espectro autista, TDAH, ansiedad sensorial, hiperacusia, migraña, adultos mayores | Saber los estímulos de cada sala ANTES de entrar; una sugerencia de por dónde empezar; permiso explícito de ir a su ritmo | Registrarse "como persona con necesidad especial"; ser rastreado; decidir entre opciones abstractas; que lo apuren |
| **Secundario** | Cualquier visitante cansado (carriola, jet lag, lunes) | Lo mismo, con menos urgencia | Fricción |
| **Comprador** | El lugar (museo primero; aeropuerto, campus, hospital después) | El expediente agregado y anónimo; la prescripción (qué señalizar, qué ruta abrir) | Datos personales (riesgo legal y reputacional) |
| **Operador de contenido** | Equipo SONA + operador del lugar | Cargar mediciones, fotos, textos "cómo llegar", reglas horarias | — |

**Persona de prueba obligatoria:** visitante de 58 años, primera visita, español únicamente, lentes puestos. Todo cambio de UI se evalúa contra esta persona (así se detectaron los 15 fallos de la v1).

---

## 3 · Roles de decisión (regla de oro)

- **La app informa y sugiere** (nunca ejecuta, nunca interrumpe): datos escritos junto a cada sala; la sugerencia es una etiqueta ("Empieza aquí") o un camino dibujado, jamás una orden.
- **La persona decide todo**: el orden, el ritmo, si ajusta sus preferencias, si se lleva la postal, cuándo termina (cerrar la app ES el cierre).
- **El lugar solo lee agregados anónimos** (capa de negocio, fuera de esta app): nunca ve personas ni sesiones.

---

## 4 · Principios de producto

1. **Nada se registra jamás.** Tocar = ver de cerca. No hay "visitas registradas", contadores, sesiones ni IDs visibles. El anonimato no es una promesa: es la arquitectura.
2. **Lenguaje humano, cero jerga del sistema.** Los estímulos y climas se dicen en palabras (*bajo / suave / poca*; *Tranquila / Normal / Llena / Con movimiento*). El vocabulario interno (moods, dimensiones, índice) NUNCA aparece en pantalla.
3. **El default inteligente evita configurar.** El modo tranquilo viene de fábrica (evitar ruido + gente). Ajustar es opcional, una sola pregunta autoexplicativa.
4. **Toda cifra o predicción declara su fuente y frescura.** "Medido hace 5 minutos", "lo medimos hace 20 minutos", "es un cálculo, no una promesa". Ningún dato puede ser más viejo que la pantalla que lo muestra.
5. **El mood tiñe toda la app** (cielo, papel, tinta, tempo del logotipo) — el clima del lugar se *siente*, no solo se lee. Familias de color de `DESIGN.md`, intactas.
6. **Determinismo SONA:** toda animación es nombrable, en loop exacto, y `prefers-reduced-motion` la congela.
7. **Indicaciones con referencias visibles** ("de frente y a la izquierda", "cruzando las puertas de cristal") — nunca puntos cardinales ni nombres internos.

---

## 5 · Flujos

### 5.1 Flujo principal — "tres toques"
```
La puerta ──VER LAS SALAS──▶ El plano ──tocar sala──▶ La ficha
    │                           ▲   │                    │
    │                           │   └──"Ajustar"──▶ pregunta sensorial
    │                           │                        │
    └──"¿Qué es SONA?"──▶ tarjeta explicativa            │
                                │◀──IR A ESTA SALA (dibuja camino)──┘
                                │
                                └──"Llévate la postal de hoy"──▶ postal del lugar
```
Cierre: **no existe pantalla de cierre.** La persona guarda el teléfono y ya.

### 5.2 Flujo de sala llena (variante de la ficha)
Solo si la persona TOCA la sala llena (nunca interrumpe): la ficha muestra el estado con frescura, la estimación honesta y una alternativa concreta. Botones que nombran destinos: **"Mejor voy al Patio"** / *"Ir a la Sala principal de todos modos"*. Ninguna opción regaña.

### 5.3 Flujo de ajuste sensorial
"Ajustar" → *"¿Qué estímulos prefieres evitar hoy?"* → chips multiselección (El ruido / La luz intensa / La mucha gente) → **Listo**. Efecto: recoloca "Empieza aquí" (sala con menor suma de los estímulos elegidos) y marca con ● los estímulos críticos en cada ficha. Si no elige ninguno: se sugiere la sala más viva.

---

## 6 · Pantallas y criterios de aceptación

### P1 · La puerta
Contenido: identificación del lugar (`SONA · LOBBY DEL MUSEO`), fecha y hora reales escritas ("jueves, 23 de julio · 9:04"); logotipo vivo (banda de variantes, tempo según clima); línea de posicionamiento (*"SONA mide los estímulos de este lugar — ruido, luz, gente — y el logotipo se mueve como el museo respira"*); enlace "¿Qué es SONA?"; clima general en **palabra de visitante** (*"Ahora mismo el museo está **Tranquilo** — hay poca gente en las salas · medido hace 5 minutos"*; nunca la clave interna "Calmado"); botón único **VER LAS SALAS** con *"sin cuenta · la app no guarda nada"*.

- [ ] CA-1.1 Un primerizo responde "¿qué es esto y qué hago?" en <5 s (test con persona de §2).
- [ ] CA-1.2 Cero decisiones antes del valor: un solo botón accionable.
- [ ] CA-1.3 El movimiento del logotipo está explicado en pantalla (nadie cree que "el teléfono falla").
- [ ] CA-1.4 No aparece ningún número sin unidad ni referencia (el "82%" está prohibido).
- [ ] CA-1.5 Si las variantes del logotipo no cargan, hay fallback (SVG maestro estático) y ningún hueco mudo.
- [ ] CA-1.6 Fecha y hora salen de `toLocaleDateString/Time("es-MX")`, nunca de formatos codificados.

### P2 · El plano
Contenido: cabecera "Las salas · [hora]" + enlace "Ajustar"; plano con anclas del edificio real (**Entrada — estás aquí** con pulso; **Salida**); cada sala con nombre (≥11 px) y clima en palabra (*Tranquila / Normal / Con movimiento · tiene bancas*; la sala llena dice *"Llena · suele calmarse ~11"* — estimación, nunca "se calma a las 11" en indicativo); píldora **EMPIEZA AQUÍ** sobre la sala sugerida; pie: *"Toca una sala para verla de cerca. Ve en el orden que quieras — la app no sabe dónde estás ni te sigue"*; enlace "Llévate la postal de hoy".

- [ ] CA-2.1 Tocar una sala SOLO abre su ficha. Nunca "registra", cuenta ni anuncia nada.
- [ ] CA-2.2 No existe contador de progreso de ningún tipo.
- [ ] CA-2.3 Los nombres de sala son legibles a distancia de brazo por la persona de prueba (≥11 px).
- [ ] CA-2.4 ENTRADA y SALIDA siempre visibles; "estás aquí" solo en la entrada (la app no conoce la posición real y no la finge).
- [ ] CA-2.5 La declaración de no-seguimiento está escrita en la pantalla.
- [ ] CA-2.6 "Empieza aquí" se recoloca según los estímulos evitados, con transición suave.

### P3 · La ficha de sala
Contenido: nombre + clima ("Tranquila ahora"); frescura del dato; **tres estímulos en palabras** (Sonido / Luz / Gente) con ● en los que la persona evita y están altos (nivel 3); mirilla circular (foto real con velo del mood y grano); "Cómo llegar" con referencias visibles y tiempo a pie; *"No hace falta avisar cuando llegues — la app no te sigue"*; botón **IR A ESTA SALA** (dibuja el camino Entrada→sala en el plano y cierra) ; cierre con botón ×, nunca por timeout.

- [ ] CA-3.1 Los estímulos aparecen SIEMPRE, en palabras, sin números ni medidores abstractos.
- [ ] CA-3.2 La ficha permanece hasta que la persona la cierra (× o velo). Prohibido el auto-descarte.
- [ ] CA-3.3 "Cómo llegar" nunca usa puntos cardinales ni claves internas (A/B/C).
- [ ] CA-3.4 La app entera se tiñe con el mood de la sala abierta y regresa al clima general al cerrar.
- [ ] CA-3.5 Targets táctiles ≥44 px en todos los controles.

### P3b · La ficha de sala llena
- [ ] CA-3b.1 Solo aparece si la persona toca esa sala. Prohibido interrumpir.
- [ ] CA-3b.2 La predicción se declara estimación con fuente y hora ("suele calmarse hacia las 11 — es un cálculo, no una promesa" / "lo medimos hace 20 minutos").
- [ ] CA-3b.3 Los botones nombran destinos concretos; existe siempre la opción de ir de todos modos, sin fricción extra.

### P4 · Ajustar (flotante)
- [ ] CA-4.1 Una sola pregunta, en frase completa; cada chip se entiende sin explicación.
- [ ] CA-4.2 El efecto del ajuste es visible de inmediato al volver (píldora se mueve; ● en fichas).
- [ ] CA-4.3 Se puede cerrar sin elegir nada y no pasa nada malo.

### P5 · ¿Qué es SONA? (flotante)
- [ ] CA-5.1 Explica qué mide, para qué sirve y el no-seguimiento, en ≤80 palabras de español llano.

### P6 · La postal del día (flotante)
- [ ] CA-6.1 La postal es del LUGAR (clima + fecha + firma *sona*): no contiene ningún dato de la visita de la persona.
- [ ] CA-6.2 El texto lo dice explícitamente ("esta postal es del lugar, no tuya").

### Transversales
- [ ] CA-T.1 Ninguna palabra de la lista prohibida (§8) aparece en la UI.
- [ ] CA-T.2 `prefers-reduced-motion` congela banda, pulsos y transiciones.
- [ ] CA-T.3 Contraste AA en todo texto (ojo con mono pequeño sobre cielos).
- [ ] CA-T.4 Todo elemento interactivo tiene `aria-label` o texto visible; flotantes con `role="dialog"`.
- [ ] CA-T.5 Estados obligatorios: cargando, sin conexión y error tienen presentación declarada (no silencios).
- [ ] CA-T.6 La app funciona en teléfono real a pantalla completa (100dvh, safe-areas) y en desktop como columna centrada.

---

## 7 · Datos y llenado del lugar

El llenado sigue las 5 capas de `zona-llenado.html`; la app **declara la fuente vigente** en cada dato:

| Capa | Fuente | La app dice |
|---|---|---|
| 1 | Medición de origen (equipo SONA, semana 1) | "Medido por el equipo SONA" |
| 2 | Contexto estructural (operador: horarios, reglas) | "Suele calmarse hacia las 11" |
| 3 | Personal de sala (botón de 2 toques) | "Lo medimos hace 20 minutos" |
| 4 | Comportamiento agregado | "Ahora mismo" |
| 5 | Re-medición (ciclo 30 días) | actualiza la capa 1 |

Regla: **el sistema nunca miente ni calla — cambia de fuente y lo declara.**

Contenido por sala: nombre, mood (uno de los 7 estados internos → palabra de visitante), niveles 1–3 de sonido/luz/gente con su palabra, foto real, texto "cómo llegar", flags (llena, bancas).

---

## 8 · Lo que NO debe repetirse (anti-patrones, con evidencia)

Cada punto viene del test con el visitante primerizo sobre la v1. **Lista de bloqueo para toda iteración futura:**

| Prohibido | Evidencia del fallo |
|---|---|
| La palabra **"tap"** en cualquier forma ("Tap 1", "Entre taps", "Tap final") | "¿mi visita se mide en toques de pantalla?" |
| **Registrar visitas** al tocar el mapa | Tocó sin moverse de su banca y la app dijo "registrada" → "¿me sigue con GPS o le aviso yo?" |
| **Contadores de progreso** ("0 de 3 puntos") | Contaba 4 círculos y el letrero decía 3 |
| **Números sin unidad ni referencia** ("82% · índice del lugar") | Lo leyó como "82% de lleno — lo contrario de Calmado" |
| **Parámetros abstractos** (Ritmo/Gente/Sonido/Guía en niveles mudos) | "No sé qué estoy decidiendo… si esta app es para gente que se agobia, esto agobia" |
| **Voces intermediarias** ("El motor propone") | "¿QUÉ motor? Para mí un motor es el del coche" |
| **Anglicismos y claves internas** (Sensory Journey, Session ID, #4F2A, rutas "A → D") | "Como si el recibo del súper dijera 'compraste SKU-4471'" |
| **Interrumpir con decisiones** (pantalla de decisión que aparece sola) | "¿Cambiar QUÉ? Yo nunca armé un plan" |
| **Resúmenes de la visita personal** (journey con ruta y tiempos) | Contradice "no te seguimos"; imposible sin rastrear o sin obligar a reportarse |
| **Botones de miedo** ("Borrar todo") y retórica ("borrar también es un derecho") | "¿Le borro algo al museo?" — el anonimato debe ser comportamiento, no decisión |
| **Texto ilegible** (<11 px en contenido esencial) | "Con mis lentes puestos, ilegible" |
| **Puntos cardinales / nombres internos** ("rampa norte") | "No sé dónde está el norte ni en mi propia casa" |
| **Datos viejos sin declarar** ("medido 21 jul" mostrado el 23) | "¿El Calmado es de ahorita o de anteayer?" |
| **Promesas de futuro sin marco** ("Vuelve a las 11: estará Calmada") | "¿Cómo saben el futuro?" → siempre "es un cálculo, no una promesa" |
| **Animación sin explicación** (logotipo mutando sin contexto) | "Pensé que el teléfono estaba fallando" |
| **Pop-ups con timeout** (mirilla de 4.2 s) | Desaparece sin que la persona lo decida y no se puede reabrir |
| **Jerga poética en lugar de dato** ("el museo amanece Calmado", "todo es válido") | "Suena a taller de yoga: yo solo quería saber por dónde se sale" |

**Regla de guardia:** antes de mergear cualquier texto nuevo de UI, pasarlo contra esta tabla y contra la persona de prueba de §2 (basta un repaso con el marco `super-ux`: diagnóstico completo, nunca ley aislada).

---

## 9 · Validación y métricas

- **Prueba de 5 segundos** por pantalla con ≥5 personas ajenas al proyecto (incluida una >55 años): deben decir qué es y qué harían.
- **Prueba específica con usuarios neurodivergentes** antes del piloto (criterio: encuentran los estímulos de una sala y saben por dónde empezar, sin ayuda).
- Piloto ancla: museo entre semana — métrica del comprador: uso de la ruta tranquila, descensos de aglomeración en la zona crítica, satisfacción del personal de sala.
- Métrica de producto sin rastrear personas: sesiones abiertas (conteo anónimo), toques a fichas por sala (agregado), uso de "Ajustar" y de la postal.

## 10 · Roadmap corto (fuera de v1)

Plano real del edificio · fotos reales por sala · más estímulos (olores, temperatura) cuando la medición los soporte · modo instalación (pantalla pública = `proto-publica.html`) · botón del personal de sala (capa 3) · integración Supabase para el llenado.
