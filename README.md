# haz-test

Skill para agentes de IA: escribe o completa los tests de un proyecto con la filosofía de
**expect tests** — el test declara el resultado visible esperado y ejecuta el código real,
simulando solo lo externo.

Antes de escribir nada, el agente detecta si el proyecto ya usa esta forma:
si la usa, la respeta; si no, la implementa empezando por plata, permisos y datos del usuario.

## Instalación

Copiá `SKILL.md` a la carpeta de skills de tu agente:

- Qoder: `~/.qoder/skills/haz-test/SKILL.md`
- Claude Code y similares: `.claude/skills/haz-test/SKILL.md`

Después se invoca con `/haz-test` o pidiendo "haz test".

## Licencia

MIT
