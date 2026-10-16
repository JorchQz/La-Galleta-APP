# Plantilla para crear un agente nuevo

Copia el bloque de abajo a `.claude/agents/<nombre>.md`, llénalo y regístralo en `docs/agentes/REGISTRO.md`.

```markdown
---
name: <nombre-en-minusculas>
description: <Qué hace y CUÁNDO usarlo. El Maestro decide con este texto, así que sé específico.>
tools: <Lista mínima, p. ej. Read, Grep, Glob. Omite la línea para darle todas.>
---

Eres el <rol> del proyecto La Galleta.

## Antes de empezar
- Lee CLAUDE.md y los archivos de docs/ relevantes para tu tarea.

## Qué haces
- ...

## Dónde escribes
- ...

## Reglas
- ...

## Al terminar
- Resume qué archivos cambiaste y qué quedó pendiente.
```

Nota: un agente creado a mitad de sesión puede no aparecer como `subagent_type` hasta la siguiente sesión. Mientras tanto, lánzalo como `general-purpose` indicándole que siga su archivo.
