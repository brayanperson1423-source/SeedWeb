# Micro Apps · Plataforma base

Plataforma para crear micro apps de una sola tarea para pequeños negocios,
con un núcleo común y paquetes de nicho configurables.

> Fuente de verdad del proyecto: [`CLAUDE.md`](./CLAUDE.md)

## Estado

- Fase actual: **0 · Descubrimiento** (nicho aún sin definir)
- Stack: **por confirmar** (ver `docs/decisiones.md`)

## Cómo empezar

1. Instala Git y Node.js (versión LTS). Revisa la documentación oficial de cada uno.
2. Instala Claude Code siguiendo su guía oficial (los pasos pueden cambiar).
3. Abre una terminal en esta carpeta e inicia Claude Code.
4. Abre `docs/prompts-claude-code.md` y pega el **Prompt 0**.
5. Avanza prompt por prompt, en orden, y no pases al siguiente hasta probar el actual.

## Estructura

```
CLAUDE.md                 Brief maestro (Claude Code lo lee solo)
docs/
  decisiones.md           Registro de decisiones
  prompts-claude-code.md  Secuencia de prompts para construir
  niche-packs.md          Cómo crear un paquete de nicho
  onboarding-cliente.md   Instalar un cliente en 1 día
  checklist-lanzamiento.md
  legal/                  Borradores para revisión de un abogado
src/
  core/  modules/  niche-packs/  shared/
tests/
```

## Reglas rápidas

- Nunca subir `.env` ni datos reales de clientes.
- Una decisión importante = una entrada en `docs/decisiones.md`.
- Cambió algo grande → actualizar `CLAUDE.md`.
