# AGENTS.md — Estudio Vault

## Tipo de repo
Obsidian knowledge vault, no un proyecto de software con toolchain. Solo archivos Markdown estáticos. Sin build, test, lint, ni typecheck.

## Idioma
Contenido en **español**. Mantener consistencia al agregar o editar notas.

## Punto de entrada principal
`04 - Mis_Guias/Plan_Maestro_Aprendizaje_V2.md` — orquesta todos los recursos en 6 fases (~12 meses).  
El currículum técnico detallado está en `04 - Mis_Guias/04_1-Plan_Estudios_Staff_Engineer/00-0-README.md` (60+ archivos en 7 módulos).

## Estructura de directorios
- `00 - Inbox/` — captura rápida, pendiente de clasificar
- `01 - Conceptos/` — enciclopedia técnica de conceptos
- `02 - Entrevistas/` — estrategias de coding, system design y behavioral
- `03 - System_Design/` — escalabilidad, disponibilidad, arquitectura distribuida
- `04 - Mis_Guias/` — núcleo del vault: plan de estudios, frameworks, guías
- `05 - Templates/` — plantillas para prompt engineering y contexto entre sesiones AI
- `06 - Proyectos/` — implementaciones reales (ej: `01-Router_Multi_IA/`)
- `.obsidian/` — configuración del editor Obsidian

## Git — qué NO commitear
`.gitignore` excluye explícitamente: `.obsidian/workspace.json`, `.obsidian/app.json`, `.obsidian/cache/`, `.obsidian/backups/`, y `data.json` de plugins. No incluir estos en commits.

## Restricciones de seguridad
- **Alcance de ediciones**: solo se permite leer, crear o modificar archivos dentro del directorio raíz de este repositorio y sus subdirectorios. Nunca acceder ni modificar nada fuera del workspace.
- **Comandos destructivos**: cualquier comando potencialmente destructivo o peligroso (`rm -rf`, `git reset --hard`, `git push --force`, eliminación masiva) debe pedir aprobación explícita antes de ejecutarse. El agente debe explicar qué hará y esperar confirmación.
