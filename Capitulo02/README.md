# Práctica 2. Construcción de un proceso de solicitud con decisiones

## Metadatos

| Elemento | Valor |
|---|---|
| Duración | 30 minutos |
| Complejidad | Alta |
| Nivel de Bloom | Crear |

## Descripción General

> Caso práctico transversal: el agente de solicitudes de TI debe identificar el tipo de solicitud, recopilar los datos necesarios en variables y dirigir la conversación por rutas distintas según la información recibida.

Los colaboradores escriben mensajes como «necesito acceso al Portal BI» o «quiero permisos de administración». En esta práctica construyes el tema `Solicitud de servicio de TI`, que identifica el tipo de solicitud, recopila la aplicación, la justificación y el correo del jefe inmediato, determina si se requiere autorización adicional y dirige la conversación por tres rutas: **completa**, **incompleta** y **sujeta a autorización adicional**.

El tema construido es el insumo obligatorio de las prácticas 3, 4 y 6.

Nota de compatibilidad de interfaz: Copilot Studio puede mostrar los menús con nombres ligeramente diferentes según la versión del tenant. Si la interfaz no coincide exactamente con los pasos, usa la opción equivalente en tu entorno (por ejemplo: Agentes/Agents, Configuración/Settings, Temas/Topics, Herramientas/Tools y Panel de prueba/Test pane). La lógica de la práctica sigue siendo la misma.

### Antes de iniciar la práctica: verificar el entorno

1. Abre [Copilot Studio](https://copilotstudio.microsoft.com) e inicia sesión con la cuenta del curso.
2. Abre el selector de entorno en la barra superior (normalmente en la esquina superior derecha) y selecciona exactamente `Laboratorio Copilot Studio - <tus iniciales>`, reemplazando el marcador por tus iniciales.
3. Antes de editar el agente o crear el tema, confirma que ese nombre permanece visible en el selector.
4. Si trabajas en Power Automate durante la práctica, verifica también que su selector de entorno muestre `Laboratorio Copilot Studio - <tus iniciales>`.

**Si el entorno no aparece o no puedes seleccionarlo, detente y contacta al instructor o a TI. No continúes en el entorno predeterminado, personal o de producción.**

## Objetivos de Aprendizaje

- [ ] Crear un tema con frases desencadenadoras representativas.
- [ ] Recopilar información mediante preguntas y almacenarla en variables con nombres descriptivos.
- [ ] Usar variables de tipo opción, cadena y booleano con alcance de tema.
- [ ] Ramificar la conversación con condiciones y expresiones Power Fx.
- [ ] Manejar información incompleta mediante reintentos y una ruta alternativa.

## Prerrequisitos

**Conocimientos**

- Práctica 1 completada.
- Haber revisado los temas del Capítulo 2: temas, preguntas y variables, tipos y alcance de variables, condiciones y manejo de información incompleta.
- Conocimientos básicos de variables, tipos de datos, condiciones lógicas y ramificaciones.

**Acceso**

- Agente `Agente Solicitudes TI - <tus iniciales>` en `Laboratorio Copilot Studio - <tus iniciales>`.
- Permisos para crear y modificar temas y variables.
- Archivo [assets/formulas-power-fx.md](../assets/formulas-power-fx.md) del repositorio.
- Usa solo datos ficticios; los correos de ejemplo usan el dominio `contoso.com`.

## Entorno de Laboratorio

| Recurso | Versión, edición y arquitectura | Uso |
|---|---|---|
| Microsoft Copilot Studio | Servicio SaaS vigente, experiencia clásica (orquestación clásica); arquitectura no aplicable. Fuente oficial: https://learn.microsoft.com/es-es/microsoft-copilot-studio/ | Creación del tema, variables y condiciones. |
| Power Fx | Lenguaje de fórmulas integrado en Copilot Studio; no requiere instalación. Fuente oficial: https://learn.microsoft.com/es-es/microsoft-copilot-studio/advanced-power-fx | Expresiones en variables y condiciones. |
| Microsoft Edge | Versión estable vigente, 64 bits, administrada. Fuente oficial: https://learn.microsoft.com/es-es/deployedge/ | Acceso a Copilot Studio. |

### Consideraciones de licencia y configuración

- No se usan herramientas, flujos ni conectores en esta práctica.
- Las variables se crean con alcance de **tema** (`Topic.`).
- El catálogo de aplicaciones es ficticio y coincide con el documento de procedimientos que se incorporará en la Práctica 4.

### Preparación del espacio de trabajo

No se requieren comandos de consola. Abre tu agente en Copilot Studio y ten a la mano [assets/formulas-power-fx.md](../assets/formulas-power-fx.md). El tema que construirás tiene esta estructura:

```text
[Desencadenador] → Pregunta: TipoSolicitud
   ├─ Acceso a herramienta ─────────┐
   ├─ Habilitación de permisos ─────┤→ Pregunta: Aplicacion (+ NivelPermiso en permisos)
   └─ Requerimiento tecnológico ────┘   (Requerimiento: AplicacionTexto = "No aplica")
→ Pregunta: Justificacion → Pregunta: CorreoJefe (reintentos)
→ Establecer RequiereAutorizacion
→ Condición: ¿CorreoJefe en blanco?
     ├─ Sí → Ruta INCOMPLETA
     └─ No → Condición: ¿RequiereAutorizacion?
               ├─ Sí → Ruta SUJETA A AUTORIZACIÓN (EstadoInicial: Pendiente de autorización)
               └─ No → Ruta COMPLETA (EstadoInicial: Registrada)
```

## Instrucciones Paso a Paso

### Paso 1: Crear el tema y sus frases desencadenadoras

**Objetivo:** Definir cuándo debe activarse el proceso de solicitud.

**Instrucciones:**

1. En tu agente, selecciona **Temas › + Agregar un tema › Desde cero**.
2. Cambia el nombre del tema a `Solicitud de servicio de TI`.
3. En el nodo **Desencadenador**, agrega estas frases:

   ```text
   Necesito acceso a una aplicación
   Quiero solicitar permisos
   Solicitar acceso a una herramienta corporativa
   Tengo un requerimiento tecnológico
   Necesito que me habiliten permisos de edición
   Registrar una solicitud de TI
   ```

4. Selecciona **Guardar**.

**Resultado esperado:** El tema existe y tiene al menos seis frases desencadenadoras.

**Verificación:** Las frases son variadas y describen solicitudes, no preguntas informativas sobre requisitos.

### Paso 2: Recopilar el tipo de solicitud

**Objetivo:** Identificar el tipo de requerimiento y generar las ramas iniciales.

**Instrucciones:**

1. Debajo del desencadenador selecciona **+ › Formular una pregunta** y escribe: `¿Qué tipo de solicitud deseas realizar?`
2. En **Identificar** elige **Opción múltiple** y agrega: `Acceso a herramienta`, `Habilitación de permisos` y `Requerimiento tecnológico`.
3. Selecciona la variable de salida y, en **Propiedades de variable**, cámbiale el nombre a `TipoSolicitud`.
4. Verifica que Copilot Studio creó una rama de **Condición** para cada opción y la rama **Todas las demás condiciones**.

**Resultado esperado:** Existe la variable `Topic.TipoSolicitud` de tipo opción y tres ramas.

**Verificación:** El panel **Variables** del tema muestra `TipoSolicitud`.

### Paso 3: Recopilar la aplicación y el nivel de permiso

**Objetivo:** Solicitar solo los datos que corresponden a cada tipo de solicitud.

**Instrucciones:**

1. En la rama **Acceso a herramienta**, agrega la pregunta `¿Qué aplicación o recurso necesitas?` con **Opción múltiple** y estas opciones:

   ```text
   Portal BI Corporativo
   Gestor Documental
   Herramienta de Diagramación
   TesoNet (Tesorería)
   Consola de Servidores
   ```

   Nombra la variable `Aplicacion`.
2. En la rama **Habilitación de permisos**, agrega la misma pregunta y, en **Guardar respuesta del usuario como**, selecciona la variable existente `Topic.Aplicacion`.
3. Solo en la rama **Habilitación de permisos**, agrega la pregunta `¿Qué nivel de permiso requieres?` con las opciones `Lectura`, `Edición` y `Administración`. Nombra la variable `NivelPermiso`.
4. En las ramas **Acceso a herramienta** y **Habilitación de permisos**, agrega **+ › Administración de variables › Establecer un valor de variable**, crea la variable `AplicacionTexto` y asigna la fórmula:

   ```powerfx
   Text(Topic.Aplicacion) & If(IsBlank(Topic.NivelPermiso), "", " - " & Text(Topic.NivelPermiso))
   ```

5. En la rama **Requerimiento tecnológico**, establece `AplicacionTexto` con el valor `"No aplica"`.

**Resultado esperado:** Las tres ramas asignan `Topic.AplicacionTexto`.

**Verificación:** `AplicacionTexto` es de tipo cadena en las tres ramas; si el verificador marca un conflicto de tipos, revisa que el valor de la tercera rama esté entre comillas.

### Paso 4: Recopilar justificación y correo del jefe con reintentos

**Objetivo:** Obtener los datos obligatorios y controlar respuestas inválidas.

**Instrucciones:**

1. Después del punto donde convergen las ramas, agrega la pregunta `Describe brevemente la justificación de tu solicitud.` con **Identificar: Respuesta completa del usuario**. Nombra la variable `Justificacion`.
2. Agrega la pregunta `¿Cuál es el correo de tu jefe inmediato (aprobador)?` con **Identificar: Correo electrónico**. Nombra la variable `CorreoJefe`.
3. Abre las **Propiedades** de la pregunta del correo (menú **…** del nodo) y configura:
   - **Volver a preguntar:** repetir hasta 2 veces.
   - **Mensaje para volver a preguntar:** `Necesito un correo válido, por ejemplo jefe@contoso.com.`
   - **Acción si no se encuentra ninguna entidad:** establecer la variable sin valor (vacía).

**Resultado esperado:** El agente vuelve a preguntar ante un correo inválido y, si no lo obtiene, deja `CorreoJefe` sin valor.

**Verificación:** El nodo muestra las propiedades configuradas y `Topic.CorreoJefe` es de tipo cadena.

### Paso 5: Determinar la autorización adicional

**Objetivo:** Calcular de forma determinista si la solicitud requiere autorización adicional.

**Instrucciones:**

1. Agrega **Establecer un valor de variable**, crea la variable `RequiereAutorizacion` y asigna la fórmula:

   ```powerfx
   Or(
     Text(Topic.Aplicacion) = "TesoNet (Tesorería)",
     Text(Topic.Aplicacion) = "Consola de Servidores",
     Text(Topic.NivelPermiso) = "Administración"
   )
   ```

2. Si el editor marca un error, usa la alternativa: un nodo **Condición** con tres condiciones unidas por **O**; en la rama verdadera establece `RequiereAutorizacion = true` y en **Todas las demás condiciones** `false`.

**Resultado esperado:** `Topic.RequiereAutorizacion` es de tipo booleano.

**Verificación:** El panel Variables muestra `RequiereAutorizacion` con tipo booleano.

### Paso 6: Construir las tres rutas

**Objetivo:** Dirigir la conversación según la completitud de la información y la autorización requerida.

**Instrucciones:**

1. Agrega **+ › Agregar una condición**: `Topic.CorreoJefe` **está en blanco**.
2. **Ruta INCOMPLETA** (rama verdadera): agrega **Enviar un mensaje**:

   ```text
   Tu solicitud está incompleta: necesitamos el correo de tu jefe inmediato para continuar.
   Cuando lo tengas, vuelve a escribir "Registrar una solicitud de TI".
   ```

   Después agrega **Administración de temas › Finalizar el tema actual**.
3. En **Todas las demás condiciones**, agrega otra condición: `Topic.RequiereAutorizacion` **es igual a** `true`.
4. **Ruta SUJETA A AUTORIZACIÓN** (verdadera): establece `EstadoInicial = "Pendiente de autorización"` y envía:

   ```text
   La solicitud de {Topic.AplicacionTexto} requiere autorización adicional del dueño del proceso
   y de Seguridad de la Información. Quedará en estado "Pendiente de autorización".
   ```

5. **Ruta COMPLETA** (todas las demás): establece `EstadoInicial = "Registrada"` y envía: `Tu solicitud cumple los datos requeridos y puede continuar con el registro.`
6. Después de ambas rutas, agrega un mensaje de resumen con **{x} Insertar variable**:

   ```text
   Resumen: {Topic.TipoSolicitud} · {Topic.AplicacionTexto} · Estado: {Topic.EstadoInicial}
   ```

7. Selecciona **Guardar** y corrige cualquier error del verificador de temas.

**Resultado esperado:** El tema tiene tres rutas y se guarda sin errores.

**Verificación:** La ruta incompleta finaliza el tema antes del resumen; las otras dos rutas llegan al resumen.

### Paso 7: Realizar una prueba rápida

**Objetivo:** Confirmar que el tema se desencadena y recorre una ruta completa.

**Instrucciones:**

1. Abre el panel de prueba y selecciona **Actualizar**.
2. Escribe `Necesito acceso a una aplicación` y responde: `Acceso a herramienta`, `Portal BI Corporativo`, una justificación y `jefe@contoso.com`.

**Resultado esperado:** El agente muestra el estado «Registrada» y el resumen.

**Verificación:** El panel Variables muestra los valores capturados.

## Validación y Pruebas

La práctica se considera lograda cuando se cumplen todos los criterios medibles siguientes:

| Criterio | Evidencia mínima | Resultado requerido |
|---|---|---|
| Desencadenamiento | Prueba del Paso 7 | El tema se activa con «Necesito acceso a una aplicación». |
| Variables | Captura del panel Variables: `images/evidencia-cap02-p2-variables.png` | Existen `TipoSolicitud`, `Aplicacion`, `NivelPermiso`, `AplicacionTexto`, `Justificacion`, `CorreoJefe`, `RequiereAutorizacion` y `EstadoInicial`. |
| Rutas | Captura del lienzo del tema: `images/evidencia-cap02-p2-tema.png` | Existen las rutas completa, incompleta y sujeta a autorización. |
| Calidad del tema | Verificador de temas | El tema se guarda sin errores. |

## Solución de Problemas

### Problema 1: Error «variable no definida» al guardar

**Síntoma:** El verificador marca que una variable no existe en alguna rama.

**Causa probable:** La variable se usa en una rama en la que no fue asignada.

**Solución:** Asigna la variable en todas las ramas antes de usarla (como `AplicacionTexto`) o selecciónala con **Guardar respuesta del usuario como** en lugar de crear una nueva.

### Problema 2: La ruta incompleta nunca se activa

**Síntoma:** Al escribir un correo inválido, el agente escala o repite indefinidamente.

**Causa probable:** La acción para «no se encontró una entidad» no está configurada para dejar la variable sin valor.

**Solución:** Revisa el Paso 4, numeral 3, y guarda el tema.

## Limpieza

1. Guarda el tema `Solicitud de servicio de TI`; se usará en las prácticas 3, 4 y 6.
2. Cierra la conversación del panel de prueba.
3. Elimina únicamente los temas de prueba adicionales que hayas creado.

## Resumen (+ optional resources)

En esta práctica se construyó un proceso de solicitud con preguntas, variables tipadas, condiciones y tres rutas de conversación. La información incompleta se controla mediante reintentos y una ruta alternativa, y la autorización adicional se determina con una fórmula verificable.

**Recursos opcionales:**

- Microsoft Learn: [Crear y editar temas](https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-create-edit-topics)
- Microsoft Learn: [Formular una pregunta](https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-ask-a-question)
- Microsoft Learn: [Trabajar con variables](https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-variables)
- Microsoft Learn: [Usar condiciones](https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-using-conditions)

---

# Práctica 3. Validación de escenarios y manejo de excepciones

## Metadatos

| Elemento | Valor |
|---|---|
| Duración | 5 minutos |
| Complejidad | Media |
| Nivel de Bloom | Evaluar |

## Descripción General

> Caso práctico transversal: antes de incorporar conocimiento y acciones, la lógica del tema debe validarse con escenarios controlados.

En esta práctica ejecutas pruebas con solicitudes completas, incompletas y sujetas a autorización adicional. Identificas comportamientos incorrectos y ajustas las condiciones o las instrucciones del agente hasta que los tres escenarios produzcan el resultado esperado.

### Antes de iniciar la práctica: verificar el entorno

1. Abre [Copilot Studio](https://copilotstudio.microsoft.com) e inicia sesión con la cuenta del curso.
2. Abre el selector de entorno en la barra superior (normalmente en la esquina superior derecha) y selecciona exactamente `Laboratorio Copilot Studio - <tus iniciales>`, reemplazando el marcador por tus iniciales.
3. Antes de abrir o probar el agente, confirma que ese nombre permanece visible en el selector.
4. Si trabajas en Power Automate durante la práctica, verifica también que su selector de entorno muestre `Laboratorio Copilot Studio - <tus iniciales>`.

**Si el entorno no aparece o no puedes seleccionarlo, detente y contacta al instructor o a TI. No continúes en el entorno predeterminado, personal o de producción.**

## Objetivos de Aprendizaje

- [ ] Ejecutar tres escenarios de prueba con resultados esperados definidos.
- [ ] Usar el seguimiento entre temas y el panel Variables para diagnosticar.
- [ ] Ajustar condiciones, fórmulas o frases desencadenadoras.
- [ ] Registrar el resultado de cada escenario antes y después del ajuste.

## Prerrequisitos

**Conocimientos**

- Práctica 2 completada.
- Comprensión de las tres rutas del tema `Solicitud de servicio de TI`.

**Acceso**

- Agente con el tema `Solicitud de servicio de TI` guardado.
- Archivo [assets/matriz-escenarios-prueba.csv](../assets/matriz-escenarios-prueba.csv) abierto en Excel para la Web o en un editor autorizado.

## Entorno de Laboratorio

| Recurso | Versión, edición y arquitectura | Uso |
|---|---|---|
| Microsoft Copilot Studio | Servicio SaaS vigente, experiencia clásica; arquitectura no aplicable. Fuente oficial: https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-test-bot | Panel de prueba y seguimiento entre temas. |
| Microsoft Excel para la Web | Servicio SaaS vigente de Microsoft 365; arquitectura no aplicable. Fuente oficial: https://support.microsoft.com/es-es/excel | Registro de la matriz de escenarios. |

### Consideraciones de licencia y configuración

- No se requieren conectores ni licencias adicionales.
- Selecciona **Actualizar** en el panel de prueba antes de cada escenario para evitar valores de una conversación anterior.

### Preparación del espacio de trabajo

Abre el agente y la matriz [assets/matriz-escenarios-prueba.csv](../assets/matriz-escenarios-prueba.csv). En esta práctica completas las filas 2, 3 y 4.

## Instrucciones Paso a Paso

### Paso 1: Preparar el panel de prueba

**Objetivo:** Observar el recorrido del tema durante la prueba.

**Instrucciones:**

1. Abre el tema `Solicitud de servicio de TI` en el lienzo.
2. Abre el panel de prueba y activa **Seguir entre temas** (Track between topics).
3. Selecciona **Actualizar**.

**Resultado esperado:** El lienzo resalta el nodo activo durante la conversación.

**Verificación:** Al escribir la primera frase, el nodo Desencadenador queda resaltado.

### Paso 2: Ejecutar los tres escenarios

**Objetivo:** Comparar el comportamiento real con el esperado.

**Instrucciones:**

1. Ejecuta cada escenario, seleccionando **Actualizar** antes de cada uno:

| # | Escenario | Entradas | Resultado esperado |
|---|---|---|---|
| 1 | Completa | Acceso a herramienta · Portal BI Corporativo · justificación · `jefe@contoso.com` | Estado «Registrada» y resumen |
| 2 | Incompleta | Acceso a herramienta · Gestor Documental · justificación · responde `no lo sé` dos veces | Mensaje de solicitud incompleta y fin del tema |
| 3 | Sujeta a autorización | Habilitación de permisos · Portal BI Corporativo · Administración · correo válido | Estado «Pendiente de autorización» |

2. Para cada escenario, abre el panel **Variables** y revisa los valores asignados.
3. Registra el resultado obtenido en la matriz.

**Resultado esperado:** Cada escenario tiene un resultado registrado.

**Verificación:** Los valores de `RequiereAutorizacion` y `EstadoInicial` coinciden con la ruta recorrida.

### Paso 3: Ajustar y volver a probar

**Objetivo:** Corregir los comportamientos incorrectos detectados.

**Instrucciones:**

1. Si un escenario falla, identifica el nodo donde se desvía el recorrido.
2. Aplica el ajuste correspondiente:

| Comportamiento incorrecto | Ajuste |
|---|---|
| El tema no se desencadena | Agrega frases cercanas a la forma de escribir de los usuarios |
| La ruta incompleta nunca se activa | Revisa la acción de «no se encontró una entidad» |
| TesoNet no requiere autorización | Revisa la fórmula u opción comparada en `RequiereAutorizacion` |

3. Guarda el tema, selecciona **Actualizar** y repite el escenario.
4. Registra en la matriz el ajuste realizado.

**Resultado esperado:** Los tres escenarios producen el resultado esperado.

**Verificación:** La matriz muestra el resultado antes y después de cada ajuste.

## Validación y Pruebas

| Criterio | Evidencia mínima | Resultado requerido |
|---|---|---|
| Escenarios ejecutados | Matriz con las filas 2, 3 y 4 | Tres de tres escenarios registrados. |
| Resultados | Captura de la matriz: `images/evidencia-cap02-p3-escenarios.png` | Tres de tres coinciden con el resultado esperado. |
| Ajustes | Columna «AjusteRealizado» | Cada fallo tiene un ajuste documentado. |

## Solución de Problemas

### Problema 1: El panel muestra valores de una prueba anterior

**Síntoma:** Las variables conservan datos de un escenario previo.

**Causa probable:** No se reinició la conversación.

**Solución:** Selecciona **Actualizar** en el panel de prueba antes de cada escenario.

### Problema 2: Otro tema responde en lugar del tema de solicitud

**Síntoma:** Se activa un tema del sistema o un tema distinto.

**Causa probable:** Frases desencadenadoras superpuestas o poco representativas.

**Solución:** Agrega frases más específicas al tema de solicitud y revisa que otros temas no compartan las mismas frases.

## Limpieza

1. Guarda la matriz actualizada; se completará en la Práctica 7.
2. Cierra la conversación del panel de prueba.

## Resumen (+ optional resources)

En esta práctica se validó la lógica del tema mediante tres escenarios controlados y se documentaron los ajustes necesarios. Al finalizar, el agente comprende solicitudes, recopila datos y ejecuta diferentes rutas conversacionales de manera verificable.

**Recursos opcionales:**

- Microsoft Learn: [Probar el agente](https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-test-bot)
- Microsoft Learn: [Entidades y llenado de ranuras](https://learn.microsoft.com/es-es/microsoft-copilot-studio/advanced-entities-slot-filling)

---

[Volver al inicio](../README.md)
