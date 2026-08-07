# 🏢 ESTÁNDAR DE INGENIERÍA — PLANTILLA ENTERPRISE
> **NIVEL:** Enterprise Grade (Cero Tolerancia)
> **DESTINATARIO:** el agente de IA que trabaja en este repositorio.

---

# 🛑 PROTOCOLO DE ARRANQUE — ANTES DE RESPONDER NADA

> **Vale para cualquier asistente de IA: Claude, Gemini, Copilot o el que venga.**
> Un proyecto puede ser trabajado por varios agentes distintos y **ninguno recuerda la sesión
> anterior**.

0. **REVISA LA FUENTE DE INSTRUCCIONES GENERALES.** Vive en
   `app-ia/instrucciones-generales/` y es la **fuente del conocimiento acumulado** de todos los
   proyectos. Cuando trabajes en un proyecto instanciado:

   - ⬇️ **Si la plantilla tiene reglas 🔒 NO TOCAR que el proyecto no tiene** → **DETENTE** y avisa:
     *"⚠️ La plantilla tiene N reglas que este proyecto no tiene: [lista]. ¿Las incorporo?"*
   - ⬆️ **Si en la sesión aprendes algo que serviría a CUALQUIER proyecto** → avisa:
     *"💡 Esto debería subir a la plantilla. ¿Lo agrego?"* Se registra en §2.13 con fecha y motivo.
   - ⚠️ Si no encuentras la plantilla, avísalo y continúa igual.

   > **Regla anti-divergencia:** lo universal (🔒) se escribe **primero en la plantilla** y el
   > proyecto lo hereda. Lo específico (✏️) se escribe **solo en el proyecto y nunca sube**.

1. **LEE `docs/AUDIT_STATUS.md` ahora**, antes de contestar al usuario. Contiene qué está hecho, qué
   falta y cuál es la próxima acción. Sin eso vas a repetir trabajo ya validado o a contradecir
   decisiones ya tomadas.
2. **LEE este archivo completo.** Es la **única** fuente de reglas del proyecto.
3. **ACTUALIZA `docs/AUDIT_STATUS.md` al cerrar cada tarea**, no al final de la sesión: una sesión
   puede cortarse de golpe por límite de tokens y entonces no habrá segunda oportunidad de escribir
   el hand-off.
4. **INFORMA al usuario** rama actual y estado antes de proponer nada (§2.8.1).

> ⚠️ **No delegues esto en un automatismo.** El hook de inicio (§4, control 12) solo existe en Claude
> Code; los demás agentes **no lo ejecutan**. Esta instrucción escrita es la única que todos leen.

---

# §0 · CÓMO INSTANCIAR ESTA PLANTILLA
> **Esta sección se BORRA cuando termines de instanciar. Las demás se quedan.**

Este documento tiene **dos tipos de secciones**:

| Marca | Significado |
|---|---|
| 🔒 **NO TOCAR** | Vale igual para cualquier aplicación. Se copia sin cambios. |
| ✏️ **RELLENAR** | Depende de qué construyes. Se completa en el paso 2. |

**Pasos de instanciación:**

1. Guarda este archivo como `AGENTS.md` en la raíz del repositorio.
2. Rellena **§1 (Perfil)** y **§3 (Capa de dominio)** respondiendo sus preguntas. No inventes: si el
   dueño del producto no ha decidido algo, anótalo en la lista de **decisiones bloqueantes**.
3. **Reserva el rango de puertos de este proyecto** en §2.6 y anota los rangos de los demás proyectos
   de la máquina. **Hacerlo después significa romper un proyecto que ya funcionaba.**
4. Crea los punteros para que cualquier agente encuentre las reglas:
   - `CLAUDE.md` → una sola directiva: `@AGENTS.md`
   - `GEMINI.md` y `.github/copilot-instructions.md` → redirigen aquí. **No duplican reglas**
5. Crea `docs/AUDIT_STATUS.md` con el formato de §2.3.
6. Ejecuta **§4 (Fase 0)** completa, **incluida la prueba de que los controles bloquean de verdad**.
   Ninguna funcionalidad se escribe antes de eso.
7. Borra esta sección §0 y haz el commit inicial.

> ⚠️ **El commit inicial va directo a la rama principal.** Es inevitable: no se puede abrir un PR
> contra una rama que aún no existe. Declararlo, activar la protección enseguida, y **desde el
> segundo commit todo va por PR**.

---

# §1 · PERFIL DEL PROYECTO ✏️ RELLENAR

| Campo | Valor |
|---|---|
| **Nombre** | _(nombre del producto)_ |
| **Propósito (una frase)** | _(qué problema resuelve y para quién)_ |
| **Stack** | _(ej.: Flutter · Supabase/PostgreSQL · GitHub Actions)_ |
| **Presupuesto** | _(ej.: free tier exclusivamente hasta tener ingresos)_ |
| **Jurisdicción / ley aplicable** | _(país y norma de datos personales)_ |
| **Tipo de datos que maneja** | _(ver §3 pregunta 1)_ |
| **Usuarios objetivo** | _(quiénes y cuántos se esperan)_ |

### Decisiones bloqueantes pendientes

> **REGLA:** mientras una decisión de esta lista siga abierta, está **PROHIBIDO** escribir el esquema
> de base de datos o las políticas de permisos que dependan de ella. Cambiar el modelo de permisos con
> usuarios reales en producción es una migración de alto riesgo, no un refactor.

- [ ] _(decisión pendiente 1)_
- [ ] _(decisión pendiente 2)_

---

# §2 · NÚCLEO INVARIABLE 🔒 NO TOCAR

> **Las reglas están aquí en una línea. El detalle, los ejemplos y el porqué están en
> `docs/estandares.md` — se lee solo cuando hace falta, no en cada sesión.**

## 2.1 Calidad y secretos

1. **Sin tests no hay merge.**
2. **Secretos solo en variables de entorno.** Nunca en código. Verificar `.gitignore` y que no se
   empaqueten como recurso de la app.
3. **Credenciales de CI** en los secretos del proveedor, referenciadas por nombre. Nunca impresas.
4. **Documentación viva:** si cambia la lógica, cambia la documentación. La doc obsoleta es un bug.
5. 🚨 **STOP-THE-LINE:** ante cualquier error o advertencia, detenerse. Corregirlo es la tarea de
   máxima prioridad. Prohibido decir "lo arreglamos después".

### Manejo de secretos — las tres reglas del traslado

6. **Antes de pedir un secreto, el agente dice tres cosas:** dónde va exactamente (dirección y nombre
   del campo) · si se conserva o se desecha · qué pasa si se pierde. **Prohibido decir "crea un
   secreto" sin decir dónde termina** — un secreto sin destino claro acaba en un archivo de texto.
7. **Un secreto se copia y se pega directo en su destino.** Nunca pasa por un editor de texto, ni
   "un momento", ni "por si se me olvida": los editores modernos **guardan solos** y el *deshacer*
   resucita lo borrado.
8. **La pregunta que decide si se anota: ¿se puede volver a crear?**
   · **Sí** → se pega y se olvida. **Anotarlo es solo riesgo, sin beneficio.**
   · **No** → se anota **fuera del computador**, y se dice explícitamente por qué esa no se puede
   perder.

## 2.2 Fuente única de reglas

`AGENTS.md` es la única fuente. `CLAUDE.md` solo contiene `@AGENTS.md`. **Las reglas no se duplican.**

## 2.3 Tablero de estado vivo

**Leer `docs/AUDIT_STATUS.md` al iniciar. Actualizarlo al cerrar CADA tarea**, no al final de la
sesión. **No re-auditar** lo marcado como HECHO. Escribirlo para un desconocido: qué se validó y cómo.

## 2.4 Modelo operativo

- **Antes de empezar:** objetivo claro, alcance pequeño, criterio de éxito verificable. Si falta
  información crítica, **preguntar en vez de adivinar**.
- **Antes de cerrar:** código y doc actualizados · verificación **ejecutada y reportada** · PR creado
  y CI verde · estado final informado.
- **Riesgo:** **P0** secretos, datos sensibles, permisos, producción, CI roto · **P1** bug funcional,
  regresión · **P2** deuda, UX, doc. **P0 antes que todo.** Prohibido mezclar P0 con estética.
- **Un tema por PR.** Si crece, se divide. Cada PR declara: Resumen · Plan de prueba · Riesgo · Reversión.
- **Todo cambio debe poder revertirse.** Producción se despliega **solo por CI/CD**.
- ⚠️ **Sin protección de rama, NO usar auto-merge.** Fusionar a mano con los checks en verde.

→ Detalle completo en `docs/estandares.md` §2.4

## 2.5 Flujo de trabajo Git

1. **Sincronizar primero, siempre.** `git pull` + revisar CI. Si está rojo → STOP-THE-LINE.
2. **Informar:** *"Rama actual: X, Estado: limpio/sucio"*.
3. **Validación local obligatoria antes de commit:** formatear → analizar (sin issues) → tests (todos
   pasan). Si falla alguno, **prohibido hacer push**.
4. **Nunca commits directos a la rama principal.** Usar `feat/`, `fix/`, `docs/`, `refactor/`,
   `chore/`, `ci/`, `test/`.
5. **Commits convencionales** en inglés: `tipo(alcance): descripción`.
6. **`push` a la rama propia es respaldo, no publicación — hacerlo seguido.** Lo que se hace **una
   sola vez, al cerrar el tema**, es el PR. Un commit sin push vive en un solo disco duro.
7. **El agente crea el PR y lo gestiona**, y fusiona a mano con los checks verdes.
8. **Profesionalismo sobre velocidad.** Prohibido saltarse pasos por "eficiencia". Sin excepciones.

**Palabra clave `enterprise`:** el usuario autoriza continuar con la recomendación profesional sin
aprobación paso a paso. **No elimina la validación local ni STOP-THE-LINE.**

→ Documentos vivos y detalle en `docs/estandares.md` §2.5

## 2.6 Desarrollo local primero

Todo cambio de base de datos o backend se prueba **primero en local**. Orden: reset local → tests de
base de datos → linter. **Auditar el estado vivo, no las migraciones.** Cero advertencias visibles.
**Prohibido ejecutar escrituras contra producción desde local.**

⚠️ **Aislamiento de puertos:** cada proyecto tiene su rango propio, anotado en `docs/estandares.md`.
Un segundo proyecto con puertos por defecto **tumba al primero**.

## 2.7 Verificación por cambio

Cada cambio de lógica: escribir el test → formatear → analizar → ejecutar tests.
**Los tests validan la lógica; los ojos validan el diseño.** Prohibido validar lógica mirando la
pantalla. **Un cambio sin test es un cambio no verificado.**

## 2.8 Comportamiento del agente

1. **Informar primero** rama y estado.
2. **Esperar aprobación explícita** ante cualquier comando que modifique archivos o git.
3. **Un "interesante" no es un "procede".**
4. **Reportar con honestidad.** Si un test falla, se dice con la salida. Nunca declarar terminado lo
   no verificado.

## 2.9 Idioma en las autorizaciones

El cuadro de permiso está fijo en inglés y no se puede traducir. **Antes** de cualquier acción que lo
abra, el agente escribe en español: qué ejecuta · por qué · qué pasa si se aprueba · qué si se rechaza.

## 2.10 Rol de mentor

Explicación breve antes de cada acción · comandos desglosados en tabla · lenguaje sencillo ·
avisos proactivos sobre secretos y rama · invitar a preguntar.

## 2.11 Toma de decisiones

**Recomendar UNA sola acción**, no listar opciones sin guía. Prioridades: trazabilidad · cero errores ·
estándares sobre velocidad. Marcar la mejor con **(✅ RECOMENDACIÓN ENTERPRISE)** y explicar por qué.
**También en decisiones binarias.**

## 2.12 Dato, juicio y supuesto

| Etiqueta | Qué es | Requisito |
|---|---|---|
| **[DATO]** | Verificable por un tercero | **Fuente citada** |
| **[JUICIO]** | Inferencia del agente | Declararlo y exponer el razonamiento |
| **[SUPUESTO]** | No verificado | Va a decisiones bloqueantes |

**Prohibido presentar un JUICIO con apariencia de DATO.** Toda cifra concreta exige fuente.
**Los identificadores nunca se escriben de memoria.**

→ Reglas completas en `docs/estandares.md` §2.12

# §3 · CAPA DE DOMINIO ✏️ RELLENAR

> No se copian reglas de otro proyecto. Se **derivan** respondiendo estas siete preguntas. Cada
> respuesta se convierte en una regla concreta y verificable.

### Las 7 preguntas

1. **¿Qué dato, si se filtrara, causaría el mayor daño a una persona real?**
   → Ese dato define tu P0. Escribe su nombre aquí y protégelo primero.

2. **¿Manejas datos de personas que nunca dieron su consentimiento?**
   (fotos con terceros, contactos, menciones, menores)
   → Si sí: reglas de retención, minimización y borrado.

3. **¿Registras la ubicación física de personas?**
   → El historial de ubicaciones revela domicilio, trabajo y rutinas. Nunca en logs; guardar con la
   **precisión mínima necesaria**, no la máxima disponible.

4. **¿Alguien puede sufrir daño si se revela su identidad dentro de la app?**
   (denunciantes, víctimas, pacientes, testigos)
   → Si sí: la identidad **nunca** viaja en respuestas públicas, logs ni mensajes de error.

5. **¿La app puede hacer que alguien NO use un canal oficial que sí debería usar?**
   (emergencias, atención médica, denuncia formal)
   → Si sí: aviso claro **dentro del flujo**, no enterrado en los términos de uso.

6. **¿Puede usarse para discriminar, perfilar o acosar?**
   → Si sí: se resuelve **en el diseño del esquema** (qué campos existen), no en moderación posterior.
   Un campo de texto libre mal planteado se convierte en el problema.

7. **¿Qué pasa si el sistema está caído justo cuando más se necesita?**
   → Define el modo degradado. En aplicaciones críticas, "no funciona" puede ser peor que no existir.

### Reglas derivadas ✏️
_(Escribe aquí las reglas que salen de tus respuestas, en imperativo y verificables.)_

### Tabla de riesgo del proyecto ✏️
| Nivel | En **este** proyecto significa |
|---|---|
| **P0** | |
| **P1** | |
| **P2** | |

### Checklist de privacidad ✏️
- [ ] Ningún log contiene datos personales, identificadores, contenido de usuario ni tokens.
- [ ] Los mensajes de error visibles no exponen datos internos ni de otras personas.
- [ ] Todo acceso a datos está filtrado por usuario/organización a nivel de **base de datos**, no solo
      en la interfaz.
- [ ] _(añadir las específicas del dominio)_

---

## 2.13 Aprendizajes acumulados 🔒 NO TOCAR

> Reglas universales extraídas de proyectos reales. **Detalle y precedentes en
> `docs/estandares.md` §2.13.** Al aprender algo universal nuevo, se propone subirlo aquí.

| # | Regla |
|---|---|
| 1 | **Protocolo de 4 capas** para toda cifra: fuente oficial · test automático · contraste con otra IA · resultado visible al usuario |
| 2 | **Contrastar con otra IA y después verificar uno mismo.** Nunca aceptar un dato solo porque otra IA lo diga — ni descartarlo por lo mismo |
| 3 | **La máquina vigila, el humano confirma.** Todo dato externo que cambia: la automatización detecta, un humano aprueba |
| 4 | **El dinero viaja como texto, nunca como número.** El decimal exacto en la base no protege al cliente |
| 5 | **Priorizar herramientas populares.** Ante dos opciones parecidas, gana la más usada: más soporte y **mejor ayuda de IA** |
| 6 | **Flujo de producto:** catálogo abierto → priorizar → cerrar MVP → crecer con usuarios reales |
| 7 | **Criterio de MVP:** *¿sin esto no funciona, o funciona pero MIENTE?* Una app con números equivocados es peor que no tenerla |
| 8 | **La información nunca desaparece en silencio.** Historial, papelera, deshacer, aviso de fallo |
| 9 | **Toda recomendación declara:** datos usados · faltantes · nivel de confianza · si es cálculo, sugerencia o dato oficial |
| 10 | **Nunca cobrar por algo que la competencia dé gratis.** No pierdes esa función: pierdes al cliente |

---

# §4 · FASE 0 — GATES ANTES DE LA PRIMERA FUNCIONALIDAD 🔒 NO TOCAR

> **REGLA DURA: ninguna funcionalidad se escribe antes de que estos gates estén verdes en CI.**
> Contra un repo vacío cuestan un día; sobre 20.000 líneas, semanas.
>
> **Detalle de cada gate, con el porqué, en `docs/fase-0.md`.**

| # | Gate |
|---|---|
| 1 | Formateo verificado en CI (falla si hay cambios pendientes) |
| 2 | Tests en la ruta que CI ejecuta **de verdad** |
| 3 | Análisis estático estricto, **cero advertencias** |
| 4 | Escaneo de secretos sobre el **historial completo** (no `depth: 1`) |
| 5 | Acciones de CI **ancladas por hash**, no por etiqueta |
| 6 | Escaneo de vulnerabilidades de dependencias |
| 7 | Hook `pre-commit` versionado (bloquea commits directos a la rama principal) |
| 8 | Tests de base de datos + linter (permisos por fila, funciones privilegiadas) |
| 9 | Comparación contra el esquema real en PRs de migración |
| 10 | Cobertura obligatoria para migraciones |
| 11 | Plantilla de PR que pregunta **"¿qué NO verificaste?"** |
| 12 | Aviso de estado al iniciar sesión (rama, CI, hand-off) |
| 13 | CODEOWNERS en rutas sensibles |
| 14 | Respaldo automático y cifrado de la base de datos |
| 15 | Vigilante contra la pausa por inactividad |
| 16 | Vigilante de la rama principal (falla si llega un commit sin PR) |
| 17 | Auditoría de los **propios workflows de CI** (permisos excesivos, inyección, acciones sin anclar) |

> ⚠️ **Los gates 14 y 15 aplican a cualquier base de datos en plan gratuito**, no solo a proyectos
> críticos: los planes gratuitos suelen **pausarse por inactividad** y **no incluir respaldos**. Sin
> estos dos gates, un descuido de una semana deja el servicio caído y un borrado accidental es
> irreversible.

> 🚪 **Puerta de salida:** provocar 3 fallos a propósito —formato sucio, test roto, secreto falso— y
> **comprobar que el CI los rechaza**. Un control que nunca bloqueó nada está *configurado*, no
> *demostrado*.

---

# §5 · TRAMPAS DE SEGURIDAD CONOCIDAS 🔒 NO TOCAR

> Fallos reales y no evidentes. **Ninguno se detecta "leyendo el código con atención".**
> **Detalle completo en `docs/seguridad.md` — lectura obligatoria antes de tocar el esquema.**

**Base de datos:** `TRUNCATE` **no** respeta la seguridad por fila · auditar el **ACL completo**, no
solo las políticas · funciones con privilegios elevados invocables por anónimos son escalada de
privilegios · **auditar el estado vivo, no las migraciones** · índices "sin usar" en una base vacía no
significan que sobren.

**Aplicación:** metadatos **EXIF** con GPS viajan dentro de la foto · limitadores de tasa que **fallan
abiertos** · **CORS con origen comodín** · logs y errores **sin datos personales, tokens ni trazas**.

**Entorno del agente:** nunca versionar la configuración local · **nunca comodines de permisos para
git** (auto-aprobarían `push --force`, `reset --hard`, `commit --no-verify`).

---

# §6 · APÉNDICE — PERFILES DE DOMINIO

Categorías de referencia para rellenar §3 más rápido: **(a)** datos sensibles regulados ·
**(b)** ubicación de personas · **(c)** sin datos sensibles · **(d)** datos financieros y tributarios
de un negocio.

→ **Detalle de cada perfil en `docs/perfiles-dominio.md`.** Adaptarlos, no copiarlos tal cual.

---

# 📚 Índice de documentación

| Archivo | Cuándo leerlo |
|---|---|
| `docs/estandares.md` | Al necesitar el detalle o el porqué de una regla de §2 |
| `docs/fase-0.md` | Al montar los controles de CI |
| `docs/seguridad.md` | **Antes de tocar el esquema de base de datos o los permisos** |
| `docs/perfiles-dominio.md` | Al rellenar §3 |
| `docs/AUDIT_STATUS.md` | **Siempre, al iniciar sesión** |

> ⚠️ **No cargar estos archivos por defecto.** Se leen solo cuando la tarea lo exige. Cargarlos
> siempre multiplica el costo de cada sesión sin agregar nada.

---

_Plantilla Enterprise reutilizable. Al instanciarla: rellena §1 y §3, ejecuta §4, borra §0._
