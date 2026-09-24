# Mazo virtual de tarot — CLAUDE.md

Proyecto de aprendizaje de Roberto (`@r4rhc4fwyc-alt`) para aprender Claude Code
construyendo algo que resuelve un problema real: no tener siempre el mazo de
tarot físico a mano. Roberto trabaja solo desde iPad (app/navegador de GitHub),
nunca terminal propia — toda la ejecución de comandos pasa por Claude Code.

## Qué es la app

Mazo virtual con los 22 arcanos mayores. Un botón tira 6 cartas al azar,
acomodadas en forma de pentagrama (5 puntas + 1 en el centro). Cada carta
puede salir derecha o invertida, tiene su propio ícono SVG, y al tocarla
(una vez tirada) abre un modal con su significado derecho, invertido, y
palabras clave para cada orientación.

- App en vivo: https://r4rhc4fwyc-alt.github.io/aprendiendo-claude-code-1er-proyecto/
- Todo vive en un único archivo: `index.html` (HTML + CSS + JS inline, sin
  dependencias ni build tools).

## Decisiones tomadas y por qué

- **Un solo archivo HTML, sin frameworks**: Roberto no tiene terminal propia
  ni puede correr un build; el objetivo era que cualquier cambio se vea
  reflejado con solo pushear, sin pasos intermedios.
- **Mezcla con Fisher-Yates** sobre el array completo de 22 arcanos, y luego
  `slice(0, 6)`: garantiza que nunca se repita una carta en la misma tirada,
  igual que barajar un mazo físico (en vez de "tirar un dado" 6 veces
  independientes, que sí podría repetir).
- **Posiciones del pentagrama con CSS puro**: `position: absolute` +
  `top/left` en porcentaje + `transform: translate(-50%, -50%)`, sin
  JavaScript para el layout.
- **Cartas invertidas = rotar el contenido, no solo avisar con texto**: se
  rota 180° un `<div class="contenido-carta">` interno (ícono + nombre),
  separado del `<div class="carta">` que maneja la posición — así no chocan
  los dos `transform`. Esto se decidió porque algunos íconos son simétricos
  (ej. El Sol) y rotar solo el ícono no se notaría; rotando también el
  nombre queda inequívoco.
- **Íconos propios en SVG (`<symbol>` + `<use>`)** en vez de imágenes reales
  del mazo Rider-Waite: el entorno donde corre Claude Code tiene bloqueado
  el acceso a internet general (solo GitHub, npm, etc.), así que no se
  pueden descargar imágenes externas. Se optó por una estética propia
  "cyber-mystic" (bordes dorados + resplandor neón cian + tipografía
  monoespaciada) en vez de depender de assets externos.
- **Al armar una tirada se copian los objetos de carta** (`{...carta, invertida: ...}`)
  en vez de mutar los objetos del array `arcanosMayores` directamente, para
  no arrastrar el estado de `invertida` de una tirada a la siguiente (los
  mismos objetos se reutilizan en cada shuffle).
- **Repo público**: GitHub Pages gratis solo sirve repos públicos. Se decidió
  con Roberto explícitamente (no hay datos sensibles, es un proyecto de
  aprendizaje/portfolio).
- **GitHub Pages apunta a `main`** (no a la rama de trabajo): así el link
  público siempre refleja lo último mergeado, y las ramas nuevas no afectan
  lo publicado hasta que se mergean.
- **Flujo de git**: cada tanda de cambios va en una rama, se abre PR, y
  Roberto mismo lo mergea desde la app/web de GitHub (para practicar el
  flujo real). Cuando una rama ya fue mergeada, se reinicia desde el `main`
  actualizado (`git checkout -B <rama> origin/main`) antes de seguir
  sumando cambios nuevos, en vez de seguir commiteando sobre historial viejo.

## Estado actual

- PR #1 (mergeado): esqueleto HTML, layout CSS del pentagrama, mezcla
  Fisher-Yates, íconos SVG cyber-mystic, cartas invertidas.
- PR #2 (mergeado): significados (derecho/invertido) + palabras clave para
  los 22 arcanos, y modal al tocar una carta tirada.
- App funcionando en vivo y probada por Roberto desde Safari (iPad y
  teléfono).

## Qué falta / ideas para seguir

- Historial de tiradas pasadas (aún no definido cómo guardarlo — no hay
  backend, se resolvería con `localStorage` del navegador).
- Animación al "voltear" cada carta al tirarla (hoy aparece directo, sin
  transición).
- Si en algún momento se consigue acceso a imágenes reales del mazo
  Rider-Waite (dominio público), evaluar reemplazar los íconos SVG propios
  — Roberto las subiría manualmente vía GitHub, ya que Claude Code no puede
  descargarlas por la restricción de red del entorno.
