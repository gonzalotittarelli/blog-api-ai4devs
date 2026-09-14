# Prompts

Aquí van **todos los prompts que lanzaste** para hacer el ejercicio, en el orden en que los
lanzaste, con el modelo y la herramienta de cada uno.

Esto no es papeleo. Lo que se revisa es **cómo pediste las cosas**, no solo lo que salió: un
resultado flojo con un prompt bueno y un resultado flojo con un prompt vago necesitan feedback
distinto, y sin este archivo no se distinguen.

## Cómo rellenarlo

- Un apartado `## Prompt N` por cada prompt.
- **Pega el prompt tal cual lo lanzaste**, dentro del bloque de código, aunque ocupe diez líneas
  y aunque tenga faltas. No lo reescribas para que quede bien: el que arreglaste mentalmente
  después no es el que lanzaste.
- Incluye también los que **no funcionaron**. Suelen ser los más útiles de leer.
- `Modelo` y `Herramienta` en todos. Si cambiaste de una a otra a mitad, se nota aquí.

Borra el ejemplo de abajo cuando escribas el primero.

---

# Tu agente arranca y funciona desde cada uno de los dos repositorios de la práctica

## Prompt 1

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-api

```
Resumime que hay en el repositorio, y que se puede hacer
```

**Qué salió:**  Respondió a la primera: stack (Adonis 6 + SQLite), capas, 6 reglas de negocio con 21 tests y que depende de blog-ai para /buscar, /preguntar e /indexar. Ya conocía el ejercicio por el README y se ofreció a armar la regla. Este repositorio no tiene CLAUDE.md ni AGENTS.md. 20 segundos, conocimiento de contexto.


## Prompt 2

**Herramienta:** terminal, desde Claude Code con `!`
**Sesión lanzada desde:** blog-api

```
!make check
```

**Qué salió:** Salida: `OK blog-api: carpeta correcta y Node v25.1.0.` Lo ejecuté yo con `!` (sin pasar por el agente); que el agente ejecuta comandos lo prueba el Prompt 1 (corrió ls/cat/find). 4 segundos.

## Prompt 3

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-ai

```
Resumime que hay en el repositorio, y que se puede hacer
```

**Qué salió:** Respondió a la primera: FastAPI + pgvector + Ollama, estructura, rutas y comandos. Sin config global: transcript sin archivos de instrucciones y solo find/wc/Read. 14 segundos.

## Prompt 4

**Herramienta:** terminal, desde Claude Code con `!`
**Sesión lanzada desde:** blog-ai

```
!make check
```

**Qué salió:** Salida: `OK blog-ai: carpeta correcta, Python 3.13.15, Docker corriendo y Ollama respondiendo.` Lo ejecuté yo con `!` (sin pasar por el agente); que el agente ejecuta comandos lo prueba el Prompt 3 (corrió find/wc). 1 segundo.


# Localiza, en cada uno de esos dos, tres cosas

## Sesion nueva

## Prompt 5

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-api

```
Sin modificar nada, decime si este repo tiene cada una de estas tres cosas, con ruta exacta o "no hay":
1. Instrucciones para el agente (CLAUDE.md, AGENTS.md, .claude/, .github/copilot-instructions.md…).
2. Comprobaciones automáticas que se disparen solas al editar o integrar (hooks en .claude/settings*.json, git hooks o core.hooksPath, pre-commit/husky/lefthook, CI en .github/workflows).
3. Atajos propios del equipo (.claude/commands, .claude/skills, .claude/agents, targets del Makefile).
Para cada respuesta, mostrame el comando con el que lo comprobaste.
```

**Qué salió:**

**1. Instrucciones para el agente:** No hay. No existen CLAUDE.md, AGENTS.md, .claude/ (el directorio ni siquiera existe), ni .github/copilot-instructions.md.

**2. Comprobaciones automáticas al editar/integrar:** No hay nada específico de este repo.

**3. Atajos propios del equipo:** Solo el Makefile, sin .claude/commands, .claude/skills ni .claude/agents (no existen).

## Prompt 6

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-ai

```
Sin modificar nada, decime si este repo tiene cada una de estas tres cosas, con ruta exacta o "no hay":
1. Instrucciones para el agente (CLAUDE.md, AGENTS.md, .claude/, .github/copilot-instructions.md…).
2. Comprobaciones automáticas que se disparen solas al editar o integrar (hooks en .claude/settings*.json, git hooks o core.hooksPath, pre-commit/husky/lefthook, CI en .github/workflows).
3. Atajos propios del equipo (.claude/commands, .claude/skills, .claude/agents, targets del Makefile).
Para cada respuesta, mostrame el comando con el que lo comprobaste.
```

## Repositorio: blog-ai

**1. Instrucciones para el agente:** No hay. Ni CLAUDE.md, ni AGENTS.md, ni .claude/, ni .github/copilot-instructions.md.

**2. Comprobaciones automáticas al editar/integrar:** No hay, dentro del repo. Ni hooks de Claude, ni git hooks propios, ni pre-commit/husky/lefthook, ni CI.

**3. Atajos propios del equipo:** No hay .claude/commands, .claude/skills ni .claude/agents. Sí hay targets de Makefile.

# La práctica

## Sesion nueva

## Prompt 7

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-api

```
Quiero que esta regla se cumpla en este repo y en ../blog-ai:

"Todo cambio en archivos .ts o .py de un repo deja una línea en REGISTRO.md, en la raíz de ese mismo repo, con el formato AAAA-MM-DD | archivo | qué cambió."

No la quiero como instrucción en un CLAUDE.md: quiero que se haga cumplir sola. Montá un hook de Claude Code de tipo Stop en .claude/settings.json de este repo que ejecute un script en .claude/hooks/. El script debe:
1. Añadir siempre una línea con fecha, hora y el directorio actual (pwd) a tmp/guardia.log de este repo, para que yo pueda comprobar cuándo se ejecutó.
2. Revisar este repo y ../blog-ai con git: si hay archivos .ts o .py modificados o nuevos y el REGISTRO.md de ese repo no cambió, escribir en stderr qué falta y salir con código 2 para que no des la tarea por terminada.
3. No quedarse en bucle: si la entrada trae stop_hook_active en true, salir con 0.
No toques ningún otro archivo. Al terminar mostrame el settings.json, el script y cómo lo probaste.
```

**Qué salió:** Creó .claude/settings.json (hook Stop) y .claude/hooks/registro-guardia.sh; lo probó con 8 casos (sin cambios, .ts sin registrar → exit 2, stop_hook_active, ../blog-ai). Cargó la skill update-config por su cuenta.

**Comprobación previa (terminal, fuera del agente):** con la sesión del Prompt 7 ya cerrada, `wc -l < blog-api/tmp/guardia.log` dio 8 líneas, todas de las pruebas que hizo el agente en el Prompt 7.

## Sesion nueva

## Prompt 8

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-api

```
Añadí una función pequeña en cada repo, sin tests ni endpoints:
- en blog-api: dado un slug, devuelve el post o nada si no existe.
- en blog-ai: dado un id de fragmento, devuelve su texto o nada si no existe.
Ubicalas donde encajen con el código existente.
```

**Qué salió:** Pidió permiso para leer fuera del directorio de trabajo (../blog-ai) y acepté que lo permita siempre. Añadió `porSlug` en `app/services/post_service.ts` y `texto_de_fragmento` en `app/servicios/indice.py`, sin crear ningún REGISTRO.md. Al intentar terminar, el hook Stop lo frenó con "falta actualizar REGISTRO.md" para los dos archivos, y recién entonces creó REGISTRO.md en los dos repos. El prompt no menciona la regla: la cumplió porque el hook lo obligó. ~3 minutos.

## Prompt 9

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-api (misma sesión que el Prompt 8)

```
No modifiques ni crees ningún archivo, solo verificá y reportá.

Antes de empezar esta tarea, ../blog-api/tmp/guardia.log tenía 8 líneas.

Ejecutá estos comandos y pegame la salida literal de cada uno, sin resumirla:
1. wc -l < ../blog-api/tmp/guardia.log
2. tail -5 ../blog-api/tmp/guardia.log
3. git -C ../blog-api status --short
4. git -C ../blog-ai status --short
5. cat ../blog-api/REGISTRO.md ../blog-ai/REGISTRO.md
6. cat .claude/settings.json

Después respondé solo estas tres preguntas, cada una con sí/no y la evidencia de arriba que lo prueba:
- ¿El hook se ejecutó durante la tarea (el log creció respecto de 8)?
- ¿Se cumplió la regla en blog-api y en blog-ai (cada .ts/.py cambiado tiene su REGISTRO.md cambiado)?
- ¿Este directorio desde el que lanzaste la sesión tiene un hook Stop configurado?
```

**Qué salió:** Pegó las salidas literales y respondió sí a las tres preguntas. guardia.log pasó de 8 a 10 líneas: 12:36:51 (el bloqueo) y 12:37:23 (el cierre con `stop_hook_active`). Cada archivo cambiado tiene su línea en el REGISTRO.md de su propio repo, y `blog-api/.claude/settings.json` define el hook Stop. La conclusión del agente coincide con los números.

**Comprobación previa (terminal, fuera del agente):** con la sesión del Prompt 9 ya cerrada, borré el código del encargo y los dos REGISTRO.md (`git restore` + `rm`). `wc -l < blog-api/tmp/guardia.log` dio 11 líneas; `git status` quedó limpio en blog-ai y con solo `prompts.md` y `.claude/` en blog-api.

## Sesion nueva

## Prompt 10

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-ai

```
Añadí una función pequeña en cada repo, sin tests ni endpoints:
- en blog-api: dado un slug, devuelve el post o nada si no existe.
- en blog-ai: dado un id de fragmento, devuelve su texto o nada si no existe.
Ubicalas donde encajen con el código existente.
```

**Qué salió:** Añadió `porSlug` en `../blog-api/app/services/post_service.ts` y `texto_de_fragmento` en `app/servicios/indice.py`, y dio la tarea por terminada sin crear ningún REGISTRO.md. No hubo bloqueo del hook ni ningún aviso. Mientras exploraba ejecutó `find` sobre blog-api y en la salida aparecía `.claude/hooks/registro-guardia.sh`: vio el archivo del hook, pero el hook no se ejecutó. ~1 minuto.

## Prompt 11

**Modelo:** Sonnet 5 High
**Herramienta:** Claude Code (config aislada, CLAUDE_CONFIG_DIR)
**Sesión lanzada desde:** blog-ai (misma sesión que el Prompt 10)

```
No modifiques ni crees ningún archivo, solo verificá y reportá.

Antes de empezar esta tarea, ../blog-api/tmp/guardia.log tenía 11 líneas.

Ejecutá estos comandos y pegame la salida literal de cada uno, sin resumirla:
1. wc -l < ../blog-api/tmp/guardia.log
2. tail -5 ../blog-api/tmp/guardia.log
3. git -C ../blog-api status --short
4. git -C ../blog-ai status --short
5. cat ../blog-api/REGISTRO.md ../blog-ai/REGISTRO.md
6. cat .claude/settings.json

Después respondé solo estas tres preguntas, cada una con sí/no y la evidencia de arriba que lo prueba:
- ¿El hook se ejecutó durante la tarea (el log creció respecto de 11)?
- ¿Se cumplió la regla en blog-api y en blog-ai (cada .ts/.py cambiado tiene su REGISTRO.md cambiado)?
- ¿Este directorio desde el que lanzaste la sesión tiene un hook Stop configurado?
```

**Qué salió:** Pegó las salidas literales y respondió no a las tres preguntas. guardia.log siguió en 11 líneas (la última, 12:40:25, es anterior a la tarea); `post_service.ts` e `indice.py` aparecen modificados, pero no existe REGISTRO.md en ningún repo; y `.claude/settings.json` no existe en blog-ai. El propio agente notó que `.claude/` está en blog-api, que no es el directorio desde el que se lanzó la sesión. Contrastado con el transcript de la sesión: no hay ningún "Stop hook feedback". La regla se rompió sin avisar: nada falló y el agente dio la tarea por terminada. 23 segundos.
