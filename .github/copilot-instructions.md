# Reglas del proyecto — fuente única: AGENTS.md

> **IMPORTANTE (GitHub Copilot):** las reglas canónicas de este proyecto viven en `AGENTS.md`.
> Este archivo existe solo porque Copilot carga `.github/copilot-instructions.md` automáticamente;
> su única función es redirigir a la fuente única y evitar reglas duplicadas.
> **No añadas reglas aquí.**

**ACCIÓN OBLIGATORIA al iniciar sesión, en este orden:**

1. Leer `AGENTS.md` completo — es la única fuente de reglas.
2. Leer `docs/AUDIT_STATUS.md` — es el estado vivo: qué está hecho, qué falta y cuál es la próxima
   acción. **Actualizarlo al cerrar cada tarea**, no al final de la sesión.

Este proyecto lo trabajan varios asistentes de IA distintos y ninguno recuerda la sesión anterior.
El tablero es el único punto de continuidad.

⚠️ **Este archivo NO es una frontera de seguridad.** Las protecciones reales están en el hook
`pre-commit`, en el CI y en los permisos del agente — no en lo que aquí está escrito (§5.1).
