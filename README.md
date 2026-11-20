# Desarrollo con IA en OpenCode

Cuatro skills para definir un proyecto e implementar funcionalidades en pasos pequeños. La IA propone y escribe; tú apruebas las decisiones y decides cuándo continuar.

## Las skills

| Skill | Qué hace | Dónde deja el resultado |
| --- | --- | --- |
| [`@setup`](skills/setup/SKILL.md) | Define o revisa stack, diseño, directorios y herramientas del agente. | Decisiones aprobadas en `AGENTS.md`. |
| [`@backlog`](skills/backlog/SKILL.md) | Propone las historias de usuario (HU) necesarias, pequeñas y funcionales. | Conversación, salvo que pidas guardarlas. |
| [`@story`](skills/story/SKILL.md) | Refina una HU o tarea de preparación: hasta tres criterios y cinco puntos de plan. | Un archivo por especificación, tras aprobarlo; por defecto en `docs/stories/`. |
| [`@pair`](skills/pair/SKILL.md) | Ejecuta la especificación, verifica resultados y registra el avance. | Código, pruebas y checklist en el archivo de la historia. |

```text
Base por definir:   @setup → aprobar decisiones
Base por preparar: @story preparación → aprobar → @pair preparación
Funcionalidades:   @backlog → @story una HU → aprobar → @pair archivo
```

Puedes comenzar con `@backlog quiero agregar login`. Si faltan definiciones necesarias, sugerirá `@setup`; si falta preparar la base aprobada, sugerirá `@story` para esa preparación. Las skills no se invocan entre sí automáticamente.

`@setup` es independiente. Se usa al comienzo y para revisar decisiones, no en cada nueva funcionalidad. Distingue entre stack definido, preparación pendiente y entorno verificado.

## Instalación con npx

Necesitas **OpenCode V2**, un proveedor/modelo configurado, Git y una versión de Node.js compatible con el [CLI de skills](https://github.com/vercel-labs/skills) (actualmente, 22.20 o posterior).

Desde la raíz del proyecto, instala las cuatro skills con la URL de este repositorio en GitHub. Sustituye `USUARIO/REPOSITORIO` por su ubicación publicada:

```bash
npx skills add https://github.com/USUARIO/REPOSITORIO -a opencode --skill setup backlog story pair
```

Añade `-g` para instalarlas para todos tus proyectos. El instalador administra los archivos de skills; no instala OpenCode, no copia [opencode.json](opencode.json), no configura Context7 ni instala el stack de tu aplicación. Consulta las [opciones del CLI](https://github.com/vercel-labs/skills/blob/main/docs/commands.md).

Abre o reinicia OpenCode y menciona la skill por su ID, por ejemplo `@setup Revisa este proyecto`. Las cuatro skills deshabilitan la invocación automática mediante `disable-model-invocation: true`; siguen disponibles mediante mención explícita. No necesitas registrar comandos slash. Consulta la [documentación de skills de OpenCode](https://opencode.ai/docs/skills/).

Para frontend y backend separados, abre la sesión en el directorio padre. La skill registra las carpetas y el agente ejecuta cada comando donde corresponde.

[opencode.json](opencode.json) queda como ejemplo **opcional** para quien prefiera comandos `/setup`, `/backlog`, `/story` y `/pair`. No lo copies completo sobre una configuración existente: incluye un modelo, permisos y rutas que debes adaptar. Las menciones `@` funcionan sin ese archivo; siguen sujetas a los permisos de tu OpenCode.

### Context7: autenticación

Context7 es opcional y se configura por separado de la instalación de skills. Para usarlo con **OAuth**, fusiona este fragmento en tu configuración de OpenCode V2, conservando los demás ajustes:

```json
{
  "mcp": {
    "servers": {
      "context7": {
        "type": "remote",
        "url": "https://mcp.context7.com/mcp/oauth"
      }
    }
  }
}
```

Después, desde la raíz del proyecto, ejecuta en una terminal:

```bash
opencode mcp auth context7
opencode mcp list
```

Completa el inicio de sesión en el navegador y comprueba que esté conectado. También puedes hacerlo desde `/mcps` en OpenCode. No necesitas instalar un servidor local ni un plugin adicional. Referencias: [MCP en OpenCode](https://opencode.ai/docs/mcp-servers/) y [Context7 para OpenCode](https://context7.com/docs/clients/opencode).

Autenticarse no implica pagar: hay un [plan gratuito con cuota](https://context7.com/plans). Si falta autenticación, se agota la cuota o el servicio no está disponible, el agente informa la limitación y recurre a documentación oficial mediante sus herramientas disponibles. Una API key es otra opción, pero requiere cambiar la configuración; no guardes credenciales en el repositorio.

## Ejemplo 1: proyecto ya instalado

Abre OpenCode en el directorio padre de `proyecto-frontend` y `proyecto-backend`. Envía los mensajes por separado, revisando cada propuesta.

**1. Documenta lo existente**, si todavía no está acordado en `AGENTS.md`:

```text
@setup El frontend y backend ya están instalados en proyecto-frontend
 y proyecto-backend. La raíz es el directorio actual.
Inspecciona y conserva el stack y diseño existentes. Identifica los
comandos y sus directorios. Propón qué registrar en AGENTS.md,
sin reinstalar dependencias ni generar variantes visuales nuevas.
```

Tras revisar:

```text
Apruebo las decisiones y su guardado. No instales herramientas adicionales.
```

Encontrar archivos no demuestra que el entorno funcione. El agente señala lo que aún no verificó; si solo faltaba documentar, no crea una tarea de inicialización.

**2. Pide la funcionalidad y elige una HU:**

```text
@backlog Quiero login con correo y contraseña: acceso válido y mensaje
ante credenciales incorrectas. Sin registro ni recuperación.
```

```text
@story Refinemos la HU de entrar con credenciales válidas.
Usa AGENTS.md y el código actual.
```

```text
Apruebo los criterios, el plan y la ruta propuesta. Guarda la historia.
```

**3. Implementa usando la ruta que devolvió el agente:**

```text
@pair docs/stories/001-login.md
```

Después de revisar los nombres y niveles de prueba:

```text
Apruebo la lista. Ejecuta un ciclo TDD y detente.
```

Para avanzar, responde `Continúa con el siguiente test`. Los nombres e identificadores de archivos en estos ejemplos son ilustrativos; usa los de tu proyecto.

## Ejemplo 2: proyecto desde cero

Abre OpenCode en la carpeta destinada a la aplicación, con las cuatro skills instaladas.

**1. Acuerda la base y el diseño:**

```text
@setup Quiero una aplicación de reservas para una peluquería.
El primer flujo será consultar horarios disponibles. El proyecto
está vacío. Conozco React y Python, prefiero PostgreSQL y shadcn/ui.

Usa el directorio actual como raíz, con proyecto-frontend y
proyecto-backend. Propón el stack mínimo, tres variantes visuales,
las herramientas del agente y la preparación necesaria.
```

El preview compara colores, tipografía, espaciado, bordes y radios con los mismos controles de muestra. Se guarda aislado, por defecto en `docs/design-preview/`. Si hay un servidor disponible, el agente entrega una URL local verificada y cómo reiniciarlo o detenerlo; si no, consulta antes de añadir herramientas.

```text
Elijo la opción 2. Reduce los radios y muéstrame el ajuste.
```

Cuando estés conforme:

```text
Apruebo el stack, los directorios, la opción 2 ajustada y las
instalaciones de herramientas del agente descritas en la propuesta.
Guarda las decisiones en AGENTS.md.
```

`@setup` puede instalar las skills y MCP aprobados, pero deja la creación de la aplicación para `@pair`. El diseño aprobado se registra como tokens y convenciones. El preview es aproximado: `@pair` aplica esos valores con el tema y componentes reales, sin copiar su CSS.

**2. Planifica la preparación:**

```text
@story Preparar la base técnica y visual acordada en @setup:
inicialización, dependencias, tema y entorno de pruebas necesarios.
Respeta los directorios de frontend y backend.
```

```text
Apruebo los criterios, el plan, la ruta y las excepciones de
preparación descritas en la propuesta. Guarda la especificación.
```

Los generadores pueden superar el límite habitual de tres archivos o unas 50 líneas por paso. El plan debe dividir el trabajo o incluir una excepción explícita y acotada antes de ejecutarlo.

**3. Crea y verifica la base:**

```text
@pair docs/stories/001-preparacion-inicial.md
```

Tras revisar las acciones y comprobaciones:

```text
Apruebo la lista. Ejecuta la preparación completa aprobada,
verifica cada parte y detente al terminar o ante un bloqueo.
```

La IA crea directorios, inicializa proyectos e instala lo acordado. Usa comprobaciones reales, sin fabricar tests fallidos para instalaciones. Solo marca la preparación como verificada cuando pasan las comprobaciones acordadas.

**4. Continúa con las funcionalidades:**

```text
@backlog Quiero que los clientes consulten horarios disponibles.
Por ahora no implementemos la reserva.
```

Con la base preparada, continúa con `@story` y `@pair` como en el primer ejemplo, sin repetir `@setup`.

## Avance y nuevas sesiones

`AGENTS.md` conserva decisiones generales y estado de preparación. Cada archivo de historia contiene criterios, plan, alcance y una sección de avance mantenida por `@pair`, por ejemplo:

```markdown
## Avance
- [x] Acceso con credenciales válidas — integración.
- [ ] Rechazo de contraseña incorrecta — integración.

## Última verificación
Comando, directorio y resultado real de las pruebas ejecutadas.

## Siguiente paso
Implementar el rechazo de contraseña incorrecta.
```

Solo se marca `[x]` con evidencia real. Aprobar una historia no equivale a implementarla; las verificaciones manuales pendientes impiden darla por completada. Se respetan otras ubicaciones establecidas en el proyecto y no se sobrescriben historias para añadir nuevas.

Para retomar en otra sesión:

```text
@pair docs/stories/002-login.md
```

También admite `@pair 002-login.md` si no hay ambigüedad. El agente contrasta el avance con el código y las comprobaciones antes de continuar. Si no guardaste la especificación, tendrás que proporcionarla de nuevo.

## Herramientas y reglas de ejecución

- **Context7:** documentación ante dudas concretas; reutiliza respuestas útiles disponibles en contexto.
- **MCP específicos:** se añaden si aportan capacidades adicionales. **Playwright MCP** es opcional para inspección del navegador y no sustituye la suite de pruebas.
- **`frontend-design`:** se propone por defecto si hay interfaz; no viene incluida y se comprueban su fuente y compatibilidad antes de instalarla con aprobación.
- **TDD:** un test → rojo esperado → código mínimo → verde → pausa. Puedes pedir varios ciclos consecutivos, siempre uno a uno. Un fallo de entorno no cuenta como rojo funcional.
- **Bloqueos:** `@pair` se detiene ante decisiones fuera del plan, dos intentos sin verde, necesidad de alterar tests existentes para hacerlos pasar o pasos que excedan los límites sin una excepción aprobada.
- **Git:** tú haces los commits y push; `@pair` no ejecuta `git commit`, `git push` ni `git reset`.

Las consultas y herramientas consumen contexto del modelo. El objetivo es trabajar con alcance claro y resultados verificables, sin prometer ahorro de tokens o cuota.
