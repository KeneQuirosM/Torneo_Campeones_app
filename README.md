# Skill Claude Code: Torneo de Campeones

Skill específica para desarrollar la plataforma de torneos descrita en `.claude/skills/torneo-campeones/references/Informe_Torneo_de_Campeones.pdf`, **por fases y siempre con aprobación**.

## Instalar en Windows

Descomprime el ZIP y copia la carpeta `torneo-campeones` completa en:

`C:\Users\TU_USUARIO\.claude\skills\torneo-campeones\`

El archivo principal debe quedar en `...\torneo-campeones\SKILL.md` y el PDF en `...\torneo-campeones\references\Informe_Torneo_de_Campeones.pdf`.

## Usar en Claude Code

`/torneo-campeones iniciar`

Luego: `Autorizo iniciar la Fase 0`. No uses «haz toda la app» para eludir los controles. Puedes pedir `/torneo-campeones estado` o `/torneo-campeones continuar`.

## Cómo opera

La primera invocación solo presenta el plan. Cada fase requiere aprobación. El MVP se desarrolla módulo a módulo. Commit, push, migraciones, costos y deploy siempre requieren aprobación adicional.

La estructura de aprobación es una **instrucción de comportamiento**, no un mecanismo de seguridad técnica: configura los permisos de Claude Code/Git/proveedores por separado.

## Versión 4.0: dirección visual profesional

Añade pautas contra interfaces genéricas, doble aprobación de wireframes/sistema visual y compatibilidad opcional con UI UX Pro Max. No la instala ni la incorpora; para usarla, instálala aparte desde su fuente oficial tras revisar código y permisos.
