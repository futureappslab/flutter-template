# §4 · FASE 0 — GATES ANTES DE LA PRIMERA FUNCIONALIDAD 🔒 NO TOCAR

> **REGLA DURA: ninguna funcionalidad se escribe antes de que estos gates estén verdes en CI.**
>
> **Por qué esto es lo primero y no lo último:** una regla escrita que nadie verifica se incumple sola.
> Cada gate convierte una intención en una **máquina** que bloquea el error. Añadirlos después obliga a
> auditar todo el código ya escrito, que cuesta mucho más que escribirlos ahora contra un repo vacío.

| # | Gate | Por qué existe |
|---|---|---|
| 1 | **Formateo verificado en CI** (modo que falla si hay cambios pendientes) | Si el formateo es solo una regla escrita, el código diverge y cada PR se llena de ruido. Debe fallar el build. |
| 2 | **Tests en la ruta que CI ejecuta de verdad** | Si CI corre `test/unit` y alguien deja tests en otra carpeta, **nunca se ejecutan**: pasan en local, dan sensación de cobertura y no protegen nada. Verifica que el comando de CI cubre **todos** los directorios de tests. |
| 3 | **Análisis estático estricto, sin advertencias** | Las advertencias toleradas se acumulan hasta volverse invisibles. Cero, o no sirve. |
| 4 | **Escaneo de secretos sobre el historial completo** | Con profundidad de clonado `1` el escáner solo ve el último commit. Un secreto subido y borrado hace meses **sigue en el historial** y no se detecta. Usa profundidad completa y escanea el historial de git, no el directorio de trabajo. |
| 5 | **Acciones de CI ancladas por hash (SHA)** | Una etiqueta tipo `@v4` es **mutable**: quien controle ese repositorio puede cambiar qué código se ejecuta dentro de tu CI, con acceso a tus secretos. Ánclalas al hash completo y añade un verificador que lo mantenga. |
| 6 | **Escaneo de vulnerabilidades de dependencias** | Sobre el archivo de bloqueo de versiones, en cada PR. |
| 7 | **Hook `pre-commit` versionado en el repo** | Bloquea commits directos a la rama principal y el añadido accidental de archivos de secretos. **Límite honesto:** se puede saltar con `--no-verify`; es un cortafuegos contra accidentes, **no** un control de seguridad. El control real es el CI. |
| 8 | **Tests de base de datos + linter** | Deben verificar las **políticas de permisos por fila** y que ninguna función privilegiada sea ejecutable por usuarios anónimos. El linter de consola **no** sustituye estos tests. |
| 9 | **Comparación contra el esquema real en PRs de migración** | Si la comparación solo corre por calendario, un PR con migración llega a producción **sin haberse comparado nunca** contra el esquema real. Debe dispararse al tocar la carpeta de migraciones. |
| 10 | **Cobertura obligatoria para migraciones** | Rechazar un PR que modifique una migración sin tocar los tests de base de datos. |
| 11 | **Plantilla de PR que pregunta "¿qué NO verificaste?"** | Invierte la carga de la prueba: es fácil marcar casillas de lo hecho, incómodo declarar por escrito lo que se omitió. |
| 12 | **Aviso de estado al iniciar sesión** | Muestra automáticamente: rama actual, estado del CI, antigüedad de la última comparación de esquema **y el bloque de hand-off del tablero de continuidad (§2.3)**. El agente arranca en frío en cada sesión: si el estado no se le pone delante, no lo consulta. Esto convierte "acuérdate de leer el tablero" en una máquina, y es lo que evita que el usuario tenga que repetir la instrucción en cada chat nuevo. |
| 13 | **CODEOWNERS** | Revisión obligatoria en rutas sensibles: CI, base de datos, autenticación, migraciones. |
| 14 | **Respaldo automático y cifrado de la base de datos** | Los planes gratuitos de las plataformas gestionadas **no incluyen backups**. Un borrado accidental es irreversible. El volcado se cifra **antes** de salir del proceso, para que el artefacto nunca contenga datos personales legibles. |
| 15 | **Vigilante contra la pausa por inactividad** | Los proyectos gratuitos se **pausan** tras días sin actividad. Para una aplicación de uso esporádico eso significa que **se apaga sola** justo cuando nadie la está mirando. Una tarea programada mantiene el proyecto vivo. |
| 16 | **Vigilante de la rama principal** | Cuando el plan contratado **no aplica protección de rama** (habitual en repositorios privados gratuitos), «nunca hagas commit directo» deja de ser una máquina. Este control no impide el push, pero **falla ruidosamente y deja registro** cuando un commit llega sin PR asociado. |

**Puerta de salida de la Fase 0:** los gates existen, corren en cada PR y están en verde.

> ⚠️ **Y el paso que casi todos se saltan: PROBAR QUE BLOQUEAN.**
> Provocar a propósito tres fallos —formato sucio, test roto, secreto falso— en una rama desechable y
> **comprobar que el CI los rechaza**. Un control que nunca ha bloqueado nada está *configurado*, no
> *demostrado*. Recién con esa prueba superada empieza la primera funcionalidad.

---

