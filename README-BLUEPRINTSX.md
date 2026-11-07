# BluePrintsX — Fork de "The Agency"

> **Fork local de [agency-agents](https://github.com/msitarzewski/agency-agents) de msitarzewski (licencia MIT), con 4 agentes nuevos de acero estructural para BluePrintsX.** La base es el trabajo de msitarzewski — nació de un hilo de Reddit y hoy tiene más de 160,000 estrellas en GitHub. Este fork no cambia la base: solo agrega una división nueva, `blueprintsx/`, con agentes que trabajan con los números verificados de GA Steel / BluePrintsX.

## Qué es esto

"The Agency" es una biblioteca open source de más de 230 agentes de IA especializados, organizados como una agencia de verdad — departamentos de ingeniería, diseño, marketing, ventas, finanzas — cada uno con su rol, sus workflows y sus métricas de éxito. Se instalan en Claude Code, Cursor, Codex, Gemini y más.

Este fork agrega la división **BluePrintsX**: los mismos agentes especializados, pero para acero estructural — joists, joist girders, steel deck, submittals y shop drawings — alimentados con los precios de lista NEPOX verificados, las reglas de takeoff y la doctrina de BluePrintsX. Nada inventado: cada cifra en cada agente viene de los datos verificados del proyecto (`~/workspace/blueprintsx-ai/data/verified/`, 72 pares).

## Los 4 agentes nuevos

| Agente | Archivo | Qué hace |
|---|---|---|
| **Steel Estimator** 💰 | `blueprintsx/blueprintsx-steel-estimator.md` | Cotiza paquetes de joists, girders y deck desde la lista NEPOX vigente: joists $3,679.46/ST, estructural y girders $3,730.02/ST, deck galvanizado $199.58/SQ (G60 y G90), pintado $225.72/SQ. Deck siempre redondeado hacia arriba (ceil SF/100). Adds: +25% estándar en joists, +27% Georgetown, +16% girders. Flete TX $4,000/LOT, deck $1,600/LOT. Todo lo que emite es BUDGET hasta que existan submittals firmados por el EOR. |
| **Takeoff Crosschecker** 🔍 | `blueprintsx/blueprintsx-takeoff-crosschecker.md` | Revisa un takeoff contra los planos, los addenda y sus propios totales antes de cotizar. Recalcula cada total desde sus líneas, marca designaciones, porcentajes, áreas y conversiones que no cuadran, y devuelve veredicto **do-not-quote** ante cualquier discrepancia sin resolver. |
| **Submittal Writer** 📋 | `blueprintsx/blueprintsx-submittal-writer.md` | Escribe paquetes de submittals y shop drawings para el EOR, para revisión ANTES de fabricar. Usa SIEMPRE la firma oficial completa de GA Steel (nunca la corta) y el correo jorge@gasteel.com. Sigue el estado de cada submittal (No Exceptions Taken, Revise and Resubmit, vencido). |
| **Steel Detailer** 🔩 | `blueprintsx/blueprintsx-steel-detailer.md` | Detalla shop drawings y notas BIM desde las hojas de fabricación (que mandan sobre cualquier takeoff). Lleva la advertencia del HSS 2-1/2×2-1/2×3/8 — NO lo produce ningún fabricante — y exige aprobación escrita del EOR para cualquier sustituto. Anchor bolts y embeds siempre en UN solo paquete. Nunca presenta un visual esquemático como modelo de taller. |

## Reglas que llevan los 4 agentes

- **Nunca inventar un precio, peso o regla.** Todo número viene de la lista verificada o del archivo del proyecto.
- **Nunca presentar un budget como quote.** Sin submittals firmados por el EOR, todo es budgetary. La cadena es Budget → Quote → Sales Order → Invoice.
- **Como está dibujado es lo que manda.** La geometría dibujada gobierna sobre los textos.
- **Un takeoff marcado nunca se cotiza.** Se avanza como budgetary con huecos declarados, o se devuelve para corrección.
- **La hoja de fabricación manda sobre el takeoff.** Cuando discrepan, gana la hoja y la discrepancia se marca.

## Cómo instalarlos

Igual que la base. La forma rápida es la app oficial (macOS, Linux, Windows), que instala el roster en Claude Code, Cursor, Codex, Gemini CLI, OpenCode, Qwen y más:

👉 [Descargar la app](https://github.com/msitarzewski/agency-agents-app/releases/latest) · [agencyagents.app](https://agencyagents.app)

O por línea de comandos, desde la raíz de este repo:

```bash
# Solo la división BluePrintsX, en Claude Code
./scripts/install.sh --tool claude-code --division blueprintsx

# Ver qué trae la división
./scripts/install.sh --tool claude-code --division blueprintsx --dry-run

# Un solo agente
./scripts/install.sh --tool claude-code --agent steel-estimator

# Otros tools (Cursor, Codex, Gemini CLI...)
./scripts/convert.sh
./scripts/install.sh --tool cursor --division blueprintsx
./scripts/install.sh --tool codex --division blueprintsx
```

## Costo: $0

El fork, los agentes y la base son open source (MIT). Lo que sí cuesta es el modelo detrás de cada agente cuando se usa (Claude, etc.) — eso lo cobra el proveedor del modelo, no este repo. Para uso sin costo, los agentes también funcionan como prompts de referencia y como la base de datos verificada del proyecto BluePrintsX Sovereign AI (`~/workspace/blueprintsx-ai/`).

## Crédito

Base: **The Agency / agency-agents** por [msitarzewski](https://github.com/msitarzewski) — [github.com/msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents), licencia MIT. Los 4 agentes BluePrintsX son trabajo nuevo sobre esa base y viven en `blueprintsx/`. La división está registrada en `divisions.json`, `scripts/convert.sh`, `scripts/lint-agents.sh`, `.github/workflows/lint-agents.yml` y `scripts/install.sh`, como pide el repo.

## El único paso que falta (lo hace Jorge)

Crear el fork en GitHub y subir esta carpeta. La creación de un repo necesita un clic humano — ningún agente puede hacerlo:

1. Abrir https://github.com/msitarzewski/agency-agents
2. Clic en **Fork** (arriba a la derecha) → crear el fork en su cuenta
3. Desde esta carpeta, en terminal:
   ```bash
   cd ~/workspace/blueprintsx-agency
   git remote set-url origin https://github.com/jorgegtzus/agency-agents.git
   git push -u origin main
   ```
   (Si su fork se llama distinto, cambiar `jorgegtzus/agency-agents` por el nombre real.)
4. Listo: el fork queda público con la división BluePrintsX incluida.

Hasta ese paso, todo vive solo en local en `~/workspace/blueprintsx-agency/` — nada publicado, nada enviado.
