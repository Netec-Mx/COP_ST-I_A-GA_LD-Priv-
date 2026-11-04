# Práctica 5. Creación de un flujo para registrar solicitudes

## Metadatos

| Elemento | Valor |
|---|---|
| Duración | 30 minutos |
| Complejidad | Media |
| Nivel de Bloom | Crear |

## Descripción General

> Caso práctico transversal: el agente necesita una herramienta que registre cada solicitud validada en un repositorio de pruebas y devuelva un identificador de seguimiento.

En esta práctica creas un flujo de agente que recibe los datos de una solicitud y los registra en la lista de SharePoint de pruebas `SolicitudesTI`, previamente preparada. Configuras los parámetros de entrada, la acción que crea el registro y los parámetros de salida que devuelven el identificador de la solicitud y el resultado de la operación, tanto en caso de éxito como de error.

Después pruebas el flujo de forma independiente, con datos de ejemplo, para verificar que los registros se crean correctamente y que se devuelve la información esperada. En la Práctica 6 conectarás este flujo al tema de solicitud.

Nota de compatibilidad de interfaz: Copilot Studio puede mostrar los menús con nombres ligeramente diferentes según la versión del tenant. Si la interfaz no coincide exactamente con los pasos, usa la opción equivalente en tu entorno (por ejemplo: Agentes/Agents, Configuración/Settings, Temas/Topics, Herramientas/Tools y Panel de prueba/Test pane). La lógica de la práctica sigue siendo la misma.

### Antes de iniciar la práctica: verificar el entorno

1. Abre [Copilot Studio](https://copilotstudio.microsoft.com) e inicia sesión con la cuenta del curso.
2. Abre el selector de entorno en la barra superior (normalmente en la esquina superior derecha) y selecciona exactamente `Laboratorio Copilot Studio - <tus iniciales>`, reemplazando el marcador por tus iniciales.
3. Antes de crear el flujo de agente, confirma que ese nombre permanece visible en el selector.
4. Si el diseñador abre Power Automate, verifica también que su selector de entorno muestre `Laboratorio Copilot Studio - <tus iniciales>` antes de crear o editar el flujo.

**Si el entorno no aparece o no puedes seleccionarlo, detente y contacta al instructor o a TI. No continúes en el entorno predeterminado, personal o de producción.**

## Objetivos de Aprendizaje

- [ ] Crear un flujo de agente con el desencadenador «Cuando un agente llama al flujo».
- [ ] Definir parámetros de entrada tipados.
- [ ] Registrar un elemento en una lista de SharePoint de pruebas.
- [ ] Devolver parámetros de salida con «Responder al agente» en caso de éxito y de error.
- [ ] Publicar el flujo y probarlo de forma independiente.

## Prerrequisitos

**Conocimientos**

- Haber revisado los temas del Capítulo 4: tipos de herramientas, diferencias entre herramientas, acciones y flujos, flujos como herramientas, parámetros y criterios de ejecución.
- Conocimientos fundamentales de Power Automate: desencadenadores, acciones, conectores y parámetros.
- Conocimientos básicos de listas de SharePoint.

**Acceso**

- Lista `SolicitudesTI` creada según [SETUP_GUIDE.md](../SETUP_GUIDE.md), con permiso de edición.
- URL del sitio de SharePoint de laboratorio.
- Conector SharePoint permitido por la directiva de datos del entorno.
- Datos de ejemplo en [assets/datos-prueba-flujo.csv](../assets/datos-prueba-flujo.csv).
- Usa solo datos ficticios.

## Entorno de Laboratorio

| Recurso | Versión, edición y arquitectura | Uso |
|---|---|---|
| Microsoft Copilot Studio | Servicio SaaS vigente, experiencia clásica; arquitectura no aplicable. Fuente oficial: https://learn.microsoft.com/es-es/microsoft-copilot-studio/flow-agent | Creación del flujo de agente. |
| Power Automate | Servicio SaaS vigente (flujos de nube); arquitectura no aplicable. Fuente oficial: https://learn.microsoft.com/es-es/power-automate/ | Diseñador e historial de ejecuciones del flujo. |
| Conector SharePoint | Conector estándar de Power Platform; arquitectura no aplicable. Fuente oficial: https://learn.microsoft.com/es-es/connectors/sharepointonline/ | Acción «Crear elemento» en la lista `SolicitudesTI`. |
| SharePoint Online | Servicio SaaS vigente; arquitectura no aplicable. Fuente oficial: https://learn.microsoft.com/es-es/sharepoint/ | Lista de pruebas `SolicitudesTI`. |

### Consideraciones de licencia y configuración

- El conector SharePoint es estándar; no se requiere un conector Premium.
- El flujo debe responder al agente en tiempo real: la **Respuesta asincrónica** de «Responder al agente» debe estar desactivada.
- El flujo debe responder dentro del límite de **100 segundos** y estar **publicado** para usarse como herramienta.

### Preparación del espacio de trabajo

Confirma que la lista existe y que puedes crear elementos en ella:

`SolicitudesTI`

Columnas de texto: `Title`, `TipoSolicitud`, `Aplicacion`, `Justificacion`, `CorreoJefe`, `Solicitante` y `Estado`. El `ID` lo genera SharePoint. Usa este nombre para el flujo:

`Registrar solicitud de TI - <tus iniciales>`

## Instrucciones Paso a Paso

### Paso 1: Crear el flujo de agente

**Objetivo:** Iniciar un flujo que pueda ser invocado por el agente.

**Instrucciones:**

1. En Copilot Studio, abre la página **Flujos** (o **Herramientas › + Agregar una herramienta › + Nueva herramienta › Flujo de agente**, según la interfaz).
2. Crea un flujo de agente nuevo en el diseñador.
3. Verifica que el flujo inicia con el desencadenador **Cuando un agente llama al flujo** (When an agent calls the flow).

**Resultado esperado:** El diseñador muestra el desencadenador del flujo de agente.

**Verificación:** El desencadenador es «Cuando un agente llama al flujo», no un desencadenador manual ni automatizado.

### Paso 2: Definir los parámetros de entrada

**Objetivo:** Establecer el contrato de datos que enviará el agente.

**Instrucciones:**

1. En el desencadenador, agrega estas entradas de tipo **Texto**, escritas exactamente así:

| Entrada | Tipo | Descripción |
|---|---|---|
| `TipoSolicitud` | Texto | Tipo de solicitud |
| `Aplicacion` | Texto | Aplicación o recurso, con nivel de permiso si aplica |
| `Justificacion` | Texto | Motivo de la solicitud |
| `CorreoJefe` | Texto | Correo del jefe inmediato |
| `Solicitante` | Texto | Nombre del colaborador |
| `Estado` | Texto | Registrada o Pendiente de autorización |

2. Cambia el nombre del flujo a `Registrar solicitud de TI - <tus iniciales>` y guarda el borrador.

**Resultado esperado:** El desencadenador tiene seis entradas de texto.

**Verificación:** Cada entrada tiene una descripción clara que el agente pueda interpretar.

### Paso 3: Registrar la solicitud en SharePoint

**Objetivo:** Crear el elemento de la solicitud dentro de un ámbito controlado.

**Instrucciones:**

1. Debajo del desencadenador agrega la acción de control **Ámbito** (Scope) y cámbiale el nombre a `Intentar registro`.
2. Dentro del ámbito agrega **SharePoint › Crear elemento**. Si se solicita, crea la conexión con tu cuenta organizacional.
3. Configura la acción con el selector de **contenido dinámico**:

| Campo | Valor |
|---|---|
| Dirección del sitio | URL del sitio de laboratorio |
| Nombre de la lista | `SolicitudesTI` |
| Title | `TipoSolicitud`, el texto ` - ` y `Aplicacion` |
| TipoSolicitud | `TipoSolicitud` |
| Aplicacion | `Aplicacion` |
| Justificacion | `Justificacion` |
| CorreoJefe | `CorreoJefe` |
| Solicitante | `Solicitante` |
| Estado | `Estado` |

**Resultado esperado:** La acción Crear elemento está dentro del ámbito y todos sus campos están mapeados.

**Verificación:** La conexión de SharePoint muestra tu cuenta organizacional y no tiene autenticación pendiente.

### Paso 4: Responder al agente en caso de éxito

**Objetivo:** Devolver el identificador y el resultado de la operación.

**Instrucciones:**

1. Debajo del ámbito agrega **Responder al agente** y cámbiale el nombre a `Responder éxito`.
2. Configura las salidas:

| Salida | Tipo | Valor |
|---|---|---|
| `IdSolicitud` | Número | `ID` (contenido dinámico de Crear elemento) |
| `Resultado` | Texto | `OK` |
| `Mensaje` | Texto | `Solicitud registrada correctamente` |

3. En la configuración de la acción, verifica que **Respuesta asincrónica** esté desactivada.

**Resultado esperado:** El flujo devuelve tres salidas cuando el registro tiene éxito.

**Verificación:** `IdSolicitud` usa el contenido dinámico `ID` de la acción Crear elemento.

### Paso 5: Responder al agente en caso de error

**Objetivo:** Garantizar que el agente siempre reciba una respuesta, aun si el registro falla.

**Instrucciones:**

1. Agrega en paralelo una segunda acción **Responder al agente** llamada `Responder error`, con las mismas salidas:

| Salida | Tipo | Valor |
|---|---|---|
| `IdSolicitud` | Número | `0` |
| `Resultado` | Texto | `Error` |
| `Mensaje` | Texto | `No fue posible registrar la solicitud` |

2. Abre **Configurar ejecución posterior** (Configure run after) de `Responder error` y selecciona **ha fallado** y **ha agotado el tiempo de espera** para `Intentar registro`. Desmarca **se ha realizado correctamente**.
3. Guarda el flujo. La estructura resultante es:

   ```text
   Cuando un agente llama al flujo (6 entradas)
   └─ Ámbito: Intentar registro
      └─ Crear elemento (SharePoint · SolicitudesTI)
   ├─ Responder éxito  ← si el ámbito tiene éxito
   └─ Responder error  ← si el ámbito falla o agota el tiempo
   ```

**Resultado esperado:** El flujo tiene dos respuestas excluyentes.

**Verificación:** Ambas respuestas declaran las mismas tres salidas con los mismos tipos.

### Paso 6: Publicar y probar el flujo

**Objetivo:** Verificar el flujo de forma independiente antes de conectarlo al agente.

**Instrucciones:**

1. Selecciona **Publicar**.
2. Selecciona **Probar › Manualmente** y captura la primera fila de [assets/datos-prueba-flujo.csv](../assets/datos-prueba-flujo.csv):

   ```text
   TipoSolicitud: Acceso a herramienta
   Aplicacion:    Portal BI Corporativo
   Justificacion: Elaborar reportes mensuales de cartera para el comité
   CorreoJefe:    jefe.area@contoso.com
   Solicitante:   Participante de prueba
   Estado:        Registrada
   ```

3. Ejecuta la prueba y revisa el historial: **Crear elemento** y **Responder éxito** deben terminar correctamente.
4. Abre la lista `SolicitudesTI` y confirma el elemento creado con el mismo `ID`.
5. Repite la prueba con la segunda fila (estado «Pendiente de autorización»).

**Resultado esperado:** Se crean dos elementos y el flujo devuelve su identificador.

**Verificación:** El valor de `IdSolicitud` en el historial coincide con el `ID` de cada elemento de la lista.

## Validación y Pruebas

La práctica se considera lograda cuando se cumplen todos los criterios medibles siguientes:

| Criterio | Evidencia mínima | Resultado requerido |
|---|---|---|
| Contrato del flujo | Captura del diseño: `images/evidencia-cap04-p5-flujo.png` | Desencadenador de agente con seis entradas y dos respuestas al agente. |
| Ejecución | Captura del historial: `images/evidencia-cap04-p5-ejecucion.png` | Dos ejecuciones correctas. |
| Registro | Captura de la lista: `images/evidencia-cap04-p5-lista.png` | Dos elementos con los valores enviados. |
| Publicación | Estado del flujo | Flujo publicado y respuesta asincrónica desactivada. |

## Solución de Problemas

### Problema 1: No se puede publicar el flujo

**Síntoma:** El diseñador muestra un error al publicar.

**Causa probable:** Las dos acciones «Responder al agente» no declaran las mismas salidas o falta un valor.

**Solución:** Verifica que ambas respuestas tengan `IdSolicitud` (Número), `Resultado` (Texto) y `Mensaje` (Texto) con un valor asignado.

### Problema 2: Error 403 en «Crear elemento»

**Síntoma:** La ejecución falla en la acción de SharePoint con un error de acceso denegado.

**Causa probable:** La cuenta de la conexión no tiene permiso de edición sobre la lista.

**Solución:** Solicita permiso de edición sobre `SolicitudesTI` al propietario del sitio. No uses la conexión de otro participante.

## Limpieza

1. Conserva el flujo publicado; se usará en las prácticas 6 y 7.
2. Conserva los elementos de prueba en `SolicitudesTI` como evidencia, salvo que el instructor indique eliminarlos.
3. No crees copias adicionales del flujo.

## Resumen (+ optional resources)

En esta práctica se construyó un flujo de agente con un contrato de datos tipado, una acción de registro en SharePoint protegida por un ámbito y respuestas para el éxito y el error. El flujo se publicó y se probó de forma independiente, por lo que está listo para usarse como herramienta del agente.

**Recursos opcionales:**

- Microsoft Learn: [Agregar herramientas a agentes personalizados](https://learn.microsoft.com/es-es/microsoft-copilot-studio/add-tools-custom-agent)
- Microsoft Learn: [Información general de flujos de agente](https://learn.microsoft.com/es-es/microsoft-copilot-studio/flows-overview)
- Microsoft Learn: [Agregar un flujo de agente como herramienta](https://learn.microsoft.com/es-es/microsoft-copilot-studio/flow-agent)
- Microsoft Learn: [Ámbitos en Power Automate](https://learn.microsoft.com/es-es/power-automate/scopes)

---

[Volver al inicio](../README.md)
