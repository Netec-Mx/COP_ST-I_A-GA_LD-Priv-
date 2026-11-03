# Setup Guide General

## Copilot Studio intermedio-avanzado: del conocimiento a la lógica y la acción

| Control documental | Valor |
|---|---|
| Curso | COP_ST I_A GA_LD (Priv) |
| Repositorio | Organización Netec-Mx; URL por registrar al publicar |
| Alcance | Seis capítulos, siete prácticas, README principal, SETUP_GUIDE.md e inventario de recursos de `assets/` |
| Fecha de revisión | 3 de noviembre de 2026 |
| Versión documental | 1.0 |
| Modalidad | Servicios web de Microsoft Copilot Studio, Power Automate y SharePoint Online |
| Versión de la tecnología | Microsoft Copilot Studio, experiencia clásica (orquestación clásica) |

# Objetivo

Preparar un único entorno para todo el repositorio: cuentas organizacionales, licencia de Copilot Studio, entorno de Power Platform, conectores estándar, sitio de SharePoint de laboratorio, documento de procedimientos y lista de pruebas `SolicitudesTI`.

La guía consolida únicamente preparación, configuración y validación. El agente, los temas, la fuente de conocimiento y el flujo de agente constituyen actividades del curso y se construyen durante la secuencia de prácticas. No es necesario desarrollarlos antes de iniciar.

La configuración administrativa del tenant, la asignación de licencias, la habilitación de permisos y la preparación de recursos **no forman parte de las tres horas de capacitación** y deben completarse previamente.

El repositorio contiene documentación, un logotipo y recursos de apoyo en `assets/`: el documento ficticio de procedimientos, el esquema de la lista, datos de prueba, instrucciones de referencia, fórmulas Power Fx y plantillas. No distribuye soluciones importables, paquetes de flujos, instaladores ni programas ejecutables.

# Requisitos de Hardware

| Recurso | Requisito consolidado |
|---|---|
| Equipo | Computadora portátil o de escritorio con procesador de 64 bits |
| Memoria | 8 GB de RAM como mínimo |
| Espacio libre | 2 GB para descargas y evidencias |
| Pantalla | Mínimo 1366 × 768; recomendado 1920 × 1080 para trabajar con el lienzo de temas y el panel de prueba |
| Periféricos | Teclado y dispositivo apuntador |
| Conexión | Internet estable y HTTPS saliente por TCP 443 durante toda la sesión |

No se requiere GPU ni virtualización de hardware. Todas las actividades se ejecutan en servicios web.

# Sistemas Operativos Compatibles

| Sistema | Condición de uso |
|---|---|
| Windows | Sistema con soporte vigente y Microsoft Edge de 64 bits administrado, como configuración común recomendada. |
| macOS | Sistema con soporte vigente y navegador compatible actualizado; validar las aplicaciones web del tenant. |

La ejecución es web y no utiliza componentes de escritorio. Si se utiliza un navegador distinto de Edge, confirmar su autorización institucional y su funcionamiento con Copilot Studio, Power Automate y SharePoint antes del curso.

# Software Requerido

## Plataforma común

| Componente | Edición o versión requerida | Condición de preparación |
|---|---|---|
| Microsoft Edge | Versión estable vigente, 64 bits, administrada | Registrar la versión y comprobar ventanas de autenticación y descargas. |
| Microsoft Copilot Studio | Servicio web vigente, experiencia clásica | Acceso a [copilotstudio.microsoft.com](https://copilotstudio.microsoft.com) y al entorno asignado. |
| Power Automate | Servicio web para flujos de nube | Acceso a [make.powerautomate.com](https://make.powerautomate.com/) para consultar el historial de ejecuciones. |
| SharePoint Online | Servicio organizacional | Sitio de laboratorio con biblioteca `Documentos` y lista `SolicitudesTI`. |
| Microsoft Excel para la Web | Servicio organizacional de Microsoft 365 | Edición de la matriz de escenarios de prueba. |

No hay versiones instalables de estos servicios SaaS que puedan fijarse en la estación. Registrar fecha, tenant, entorno y región visibles en lugar de un número de compilación.

## Componentes opcionales

| Componente | Cuándo aplica |
|---|---|
| Microsoft Teams | Solo si el instructor demuestra la publicación en el canal de Teams y Microsoft 365 Copilot. Las prácticas no publican el agente. |
| Microsoft Word para la Web | Solo si se desea revisar el documento de procedimientos antes de cargarlo. |

La guía no exige conectores Premium, AI Builder, orquestación generativa, Power Automate Desktop, gateway local, máquinas virtuales ni contenedores. Tampoco requiere Python, .NET, Node.js, SDK, IDE ni la CLI de Power Platform.

# Herramientas y Dependencias

## Licencias y cuentas

Antes del curso, TI debe confirmar para cada participante:

- Cuenta organizacional Microsoft Entra ID en el tenant de capacitación.
- Licencia o capacidad de Microsoft Copilot Studio suficiente para crear y probar agentes y flujos de agente. Iniciar sesión en Microsoft 365 no basta.
- Derechos para crear y ejecutar flujos de nube con conectores estándar.
- Acceso a SharePoint Online con permisos sobre el sitio de laboratorio.
- MFA y requisitos de dispositivo satisfechos, si los exige el tenant.

El administrador debe verificar el conjunto real de licencias y capacidad asignadas, no solo el nombre comercial de una suscripción. Consultar los [requisitos de licencias de Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/requirements-licensing-subscriptions).

## Matriz de permisos

| Ámbito | Permiso necesario |
|---|---|
| Entorno Power Platform | Acceso al entorno autorizado y rol de creador (Environment Maker o equivalente) para crear agentes, flujos y conexiones propios. |
| Copilot Studio | Crear y modificar agentes, temas, variables, fuentes de conocimiento y herramientas. |
| Flujos de agente | Crear, editar, publicar, ejecutar y consultar el historial de los flujos propios. |
| SharePoint: biblioteca | Lectura sobre la carpeta `Procedimientos`. |
| SharePoint: lista | Agregar, consultar y editar elementos en `SolicitudesTI`. |

Los permisos de creador de entorno no sustituyen los de SharePoint. El participante no necesita ser administrador global ni administrador del entorno.

## Conectores comunes

| Conector o capacidad | Preparación |
|---|---|
| SharePoint | Conexión organizacional con acceso al sitio, la biblioteca y la lista de laboratorio. |
| Microsoft Copilot Studio | Disponible en el entorno y no bloqueado por directivas de datos. |
| Fuente de conocimiento de SharePoint | Permitida por las directivas de datos del entorno. |
| Operaciones integradas de Power Automate | Desencadenador «Cuando un agente llama al flujo», «Responder al agente» y Ámbito. No requieren instalar paquetes. |

Confirmar que las directivas de datos (DLP) del entorno permiten combinar estos conectores. Una clasificación incompatible o un bloqueo puede impedir agregar la herramienta aun cuando las licencias sean correctas. TI debe resolverlo en el entorno autorizado, sin desactivar directivas generales del tenant.

# Instalación y Configuración

## 1. Preparar el acceso web

1. Actualizar el navegador mediante el mecanismo institucional y registrar su versión.
2. Utilizar un perfil con la cuenta organizacional del laboratorio.
3. Abrir [Copilot Studio](https://copilotstudio.microsoft.com), autenticarse y seleccionar el entorno indicado por TI.
4. Confirmar nombre y región del entorno. Mantener ese mismo entorno durante todo el curso.
5. Abrir [Power Automate](https://make.powerautomate.com/) y el sitio de SharePoint asignado con la misma cuenta.

No crear un entorno nuevo con la cuenta del participante como parte del setup. Utilizar el entorno institucional habilitado para la capacitación.

## 2. Aprovisionar el entorno de capacitación

Esta preparación corresponde a TI o al administrador antes de la sesión:

1. Seleccionar o crear un entorno activo, con Dataverse, licenciamiento y capacidad adecuados, separado de procesos productivos. Consultar [Crear y administrar entornos](https://learn.microsoft.com/es-es/power-platform/admin/create-environment).
2. Asignar a los participantes el rol de creador de entorno.
3. Verificar que los participantes pueden crear agentes en Copilot Studio dentro del entorno.
4. Validar las directivas de datos y los conectores de la ruta común.
5. Comunicar el entorno y los recursos asignados mediante un registro sin secretos.

No cambiar al entorno predeterminado únicamente para evitar un error de permisos; utilizar el autorizado por el cliente.

## 3. Revisar las directivas de datos

1. En el [Centro de administración de Power Platform](https://admin.powerplatform.microsoft.com), revisar las directivas de datos que aplican al entorno de capacitación.
2. Verificar que los conectores **SharePoint** y **Microsoft Copilot Studio** pertenezcan al mismo grupo y no estén bloqueados.
3. Confirmar que se permite usar SharePoint como fuente de conocimiento y que los flujos de agente pueden agregarse como herramientas.

Consultar [Directivas de datos para agentes](https://learn.microsoft.com/es-es/microsoft-copilot-studio/admin-data-loss-prevention) y [Directivas de prevención de pérdida de datos](https://learn.microsoft.com/es-es/power-platform/admin/wp-data-loss-prevention).

## 4. Preparar el sitio de SharePoint de laboratorio

1. Crear un sitio de grupo, por ejemplo `Laboratorio Copilot Studio`.
2. En la biblioteca **Documentos**, crear la carpeta `Procedimientos` y cargar [assets/procedimientos-solicitudes-ti.docx](assets/procedimientos-solicitudes-ti.docx).
3. Conceder a los participantes permiso de **Lectura** sobre la biblioteca y de **Edición** sobre la lista del apartado siguiente.

## 5. Crear las conexiones autorizadas

Las conexiones de SharePoint se crean durante la Práctica 5 con la cuenta de cada participante. Antes del curso, validar con una cuenta equivalente a la de un participante que la conexión puede crearse en el entorno sin solicitudes de autenticación pendientes.

No guardar contraseñas, cookies ni tokens en los documentos del curso. Las conexiones de otro entorno o de otro participante no son utilizables.

# Configuración Base para los Laboratorios

## Nombres de los recursos

| Recurso | Nombre | Se crea en |
|---|---|---|
| Agente | `Agente Solicitudes TI - <iniciales>` | Práctica 1 |
| Tema | `Solicitud de servicio de TI` | Práctica 2 |
| Tema | `Consultar requisitos` | Práctica 4 |
| Fuente de conocimiento | `Procedimientos de solicitudes de TI` | Práctica 4 |
| Flujo de agente | `Registrar solicitud de TI - <iniciales>` | Práctica 5 |
| Lista de SharePoint | `SolicitudesTI` | Setup (este documento) |

Usar las iniciales del participante en los nombres para evitar conflictos cuando varias personas trabajan en el mismo entorno.

## Lista de SharePoint: SolicitudesTI

Crear la lista con **Nuevo › Lista › Lista en blanco** y las columnas de [assets/lista-SolicitudesTI-esquema.csv](assets/lista-SolicitudesTI-esquema.csv):

| Columna | Tipo | Configuración |
|---|---|---|
| Título | Texto de una línea; columna predeterminada | Resumen generado por el flujo. Nombre interno: `Title`. |
| TipoSolicitud | Texto de una línea | Acceso a herramienta, Habilitación de permisos o Requerimiento tecnológico. |
| Aplicacion | Texto de una línea | Aplicación o recurso; incluye el nivel de permiso cuando aplica. |
| Justificacion | Varias líneas de texto, sin formato enriquecido | Motivo de la solicitud. |
| CorreoJefe | Texto de una línea | Correo ficticio del jefe inmediato. |
| Solicitante | Texto de una línea | Nombre del usuario del agente. |
| Estado | Texto de una línea | `Registrada` o `Pendiente de autorización`. |

Se usan columnas de texto para simplificar el mapeo de parámetros del flujo. No convertir `Estado` en una columna de elección: la Práctica 7 simula un error de registro de forma controlada y no debe depender de validaciones de la lista. El `ID` lo genera SharePoint; no crear otra columna con ese nombre.

## Aislamiento de los recursos

Si varios participantes comparten la lista `SolicitudesTI`, los registros se distinguen por el campo `Solicitante`. Si el cliente requiere aislamiento completo, preparar una lista por participante con el mismo esquema y comunicar su nombre antes de la Práctica 5.

## Parámetros y archivos de configuración

No se requiere `.env`, variables de entorno del sistema, archivo de conexión local, token de API, aplicación Entra registrada ni certificado. Las conexiones se autentican en el servicio.

Guardar en un registro institucional de preparación los valores reales de entorno, sitio, biblioteca y lista. Las variables `Topic.*` del curso se crean en Copilot Studio durante las prácticas; no se configuran en el sistema operativo.

# Servicios Externos Requeridos

| Servicio | Acceso |
|---|---|
| Microsoft Entra ID | Autenticación, MFA y acceso al tenant. |
| Microsoft Copilot Studio | Creación, prueba y configuración del agente. |
| Power Automate y Power Platform | Flujos de agente, conexiones, historial y entorno. |
| SharePoint Online | Biblioteca de procedimientos y lista de pruebas. |
| Microsoft 365 web | Edición de la matriz de escenarios. |
| GitHub | Consulta o descarga de la documentación del curso. |

TI debe revisar los [requisitos de conectividad de Power Platform](https://learn.microsoft.com/es-es/power-platform/admin/online-requirements), las [direcciones IP de Power Automate](https://learn.microsoft.com/es-es/power-automate/ip-address-configuration) y las [URL de Microsoft 365](https://learn.microsoft.com/es-es/microsoft-365/enterprise/urls-and-ip-address-ranges), junto con proxy, DNS, cookies y políticas de acceso condicional. No abrir puertos entrantes en las computadoras.

No se necesita una suscripción Azure del participante ni acceso a sistemas bancarios. Toda la información del curso es ficticia.

# Validación del Entorno

Completar la siguiente validación con una cuenta de participante antes de iniciar el curso. Una cuenta administradora por sí sola no demuestra que el grupo tendrá acceso.

| Comprobación | Resultado esperado |
|---|---|
| Identidad y entorno | Cuenta correcta y entorno de capacitación visible en Copilot Studio. |
| Creación de agentes | Puede crear un agente temporal y abrir su página Temas. |
| Herramientas | La página Herramientas › Agregar una herramienta muestra conectores y flujos; SharePoint no aparece deshabilitado. |
| Flujos de agente | Puede crear, publicar y probar un flujo de agente temporal. |
| Conocimiento | Puede agregar la carpeta `Procedimientos` como fuente y su estado cambia a Listo. |
| Lista | Puede crear, editar y eliminar un elemento temporal en `SolicitudesTI`. |
| Directivas de datos | La combinación de conectores de la ruta común está permitida. |

## Pruebas mínimas de preparación

1. **Agente:** crear un agente temporal, abrir el panel de prueba y escribir `Hola`.
2. **Conocimiento:** agregar la carpeta `Procedimientos` como fuente al agente temporal y esperar el estado Listo.
3. **Flujo:** crear un flujo de agente temporal con «Cuando un agente llama al flujo» y «Responder al agente» que devuelva el texto `SETUP_OK`; publicarlo y probarlo.
4. **Lista:** agregar y eliminar un elemento temporal en `SolicitudesTI`.
5. **Cierre:** eliminar el agente y el flujo temporales, y registrar el resultado de cada prueba.

Registrar fecha, usuario de laboratorio, entorno, recurso probado y resultado, sin secretos.

# Comandos de Verificación

El curso funciona en navegador y no necesita instalar CLI. Los comandos siguientes son diagnósticos opcionales del puesto; no crean recursos cloud ni acreditan permisos de Copilot Studio.

## Windows PowerShell

```powershell
$PSVersionTable.PSVersion
Get-CimInstance Win32_OperatingSystem |
    Select-Object Caption, Version, OSArchitecture
Get-CimInstance Win32_ComputerSystem |
    Select-Object @{Name='RAM_GB';Expression={[math]::Round($_.TotalPhysicalMemory / 1GB, 1)}}
Get-PSDrive -PSProvider FileSystem |
    Select-Object Name, Root, @{Name='Libre_GB';Expression={[math]::Round($_.Free / 1GB, 1)}}

$serviceHosts = @(
    'copilotstudio.microsoft.com',
    'make.powerautomate.com',
    'login.microsoftonline.com',
    'github.com'
)
foreach ($serviceHost in $serviceHosts) {
    Resolve-DnsName -Name $serviceHost -ErrorAction Continue
    Test-NetConnection -ComputerName $serviceHost -Port 443 -InformationLevel Detailed
}
```

Resultados esperados: sistema de 64 bits, 8 GB de RAM o más, 2 GB libres en la unidad de trabajo, resolución DNS correcta y `TcpTestSucceeded : True` si la red permite salida directa. En redes con proxy obligatorio, contrastar la prueba con el acceso real del navegador.

Registrar la versión de Edge desde `edge://settings/help`. Agregar a la prueba el host real del sitio SharePoint del cliente.

## macOS, terminal

```bash
sw_vers
uname -m
sysctl -n hw.memsize
df -h "$HOME"
curl --version
curl -L -sS -o /dev/null -w 'HTTP final: %{http_code}\n' \
  --connect-timeout 10 --max-time 30 https://copilotstudio.microsoft.com/
```

Resultados esperados: sistema y arquitectura identificados, al menos 8 GB de memoria física —el comando devuelve bytes— y 2 GB libres. Un código HTTP `000` indica que no se recibió respuesta. Una página de autenticación no demuestra acceso al entorno ni a los conectores.

## Verificación autenticada obligatoria

Abrir Copilot Studio con la cuenta asignada y completar las comprobaciones de la sección Validación del Entorno. No instalar módulos de administración ni registrar aplicaciones Entra solo para completar esta validación.

# Troubleshooting

| Problema | Comprobación y corrección |
|---|---|
| No aparece el entorno de capacitación | Confirmar tenant, cuenta y rol de creador asignado. No cambiar a otro entorno sin autorización. |
| Mensaje de licencia o capacidad insuficiente | Revisar la licencia o capacidad de Copilot Studio asignada al participante y al entorno. |
| Una herramienta aparece deshabilitada | Revisar las directivas de datos del entorno; TI debe ajustar la clasificación de conectores. |
| El flujo no aparece como herramienta | Verificar «Cuando un agente llama al flujo», «Responder al agente», respuesta asincrónica desactivada y flujo publicado. |
| La fuente de conocimiento no cambia a Listo | Esperar la indexación y verificar el permiso de lectura sobre la carpeta `Procedimientos`. |
| Error 403 al crear elementos | Conceder permiso de edición sobre `SolicitudesTI` a la cuenta de la conexión. |
| Conexión con autenticación pendiente | Reparar la conexión con la cuenta institucional del entorno correcto. No usar credenciales de otro participante. |
| Aparece una solicitud Premium | Revisar el conector seleccionado; la ruta del curso usa conectores estándar. |

# Checklist Final

- [ ] Equipo de 64 bits, 8 GB de RAM, 2 GB libres y pantalla suficiente.
- [ ] Navegador actualizado y acceso HTTPS a los servicios del curso.
- [ ] Cuenta organizacional y licencia o capacidad de Copilot Studio verificadas.
- [ ] Entorno de capacitación identificado y rol de creador operativo.
- [ ] Directivas de datos compatibles con SharePoint, Copilot Studio y flujos de agente.
- [ ] Sitio de laboratorio con la carpeta `Procedimientos` y el documento cargado.
- [ ] Lista `SolicitudesTI` creada con el esquema y permisos de edición.
- [ ] Pruebas mínimas de preparación registradas y recursos temporales eliminados.
- [ ] Recursos de `assets/` disponibles para los participantes.

# Resumen

La plataforma común utiliza Microsoft Copilot Studio en experiencia clásica, flujos de agente de Power Automate y SharePoint Online con conectores estándar. El entorno queda listo al demostrar que un participante puede crear un agente, agregar la fuente de conocimiento, publicar y probar un flujo de agente y registrar elementos en la lista `SolicitudesTI`.

## Alcance de la revisión y pendientes del cliente

Se revisaron los README de las siete prácticas, el README principal y los recursos de `assets/` para consolidar prerrequisitos, configuraciones y dependencias.

No se tuvo acceso al tenant del cliente. La disponibilidad de licencias, capacidad, directivas de datos, entorno, sitio y lista debe validarse allí antes de impartir el curso. Los comandos se proporcionan para la estación del participante; no sustituyen las pruebas autenticadas.

## Referencias oficiales

- [¿Qué es Microsoft Copilot Studio?](https://learn.microsoft.com/es-es/microsoft-copilot-studio/fundamentals-what-is-copilot-studio)
- [Requisitos de licencias y suscripciones de Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/requirements-licensing-subscriptions)
- [Cuotas y límites de Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/requirements-quotas)
- [Información general de entornos de Power Platform](https://learn.microsoft.com/es-es/power-platform/admin/environments-overview)
- [Crear y administrar entornos](https://learn.microsoft.com/es-es/power-platform/admin/create-environment)
- [Directivas de datos para agentes](https://learn.microsoft.com/es-es/microsoft-copilot-studio/admin-data-loss-prevention)
- [Directivas de prevención de pérdida de datos](https://learn.microsoft.com/es-es/power-platform/admin/wp-data-loss-prevention)
- [Agregar un flujo de agente como herramienta](https://learn.microsoft.com/es-es/microsoft-copilot-studio/flow-agent)
- [Agregar SharePoint como fuente de conocimiento](https://learn.microsoft.com/es-es/microsoft-copilot-studio/knowledge-add-sharepoint)
- [Conector de SharePoint](https://learn.microsoft.com/es-es/connectors/sharepointonline/)
- [Límites y configuración de Power Automate](https://learn.microsoft.com/es-es/power-automate/limits-and-config)
- [Requisitos de conectividad de Power Platform](https://learn.microsoft.com/es-es/power-platform/admin/online-requirements)
- [Resolve-DnsName](https://learn.microsoft.com/es-es/powershell/module/dnsclient/resolve-dnsname)
- [Test-NetConnection](https://learn.microsoft.com/es-es/powershell/module/nettcpip/test-netconnection)
- [Opciones de cURL](https://curl.se/docs/manpage.html)
