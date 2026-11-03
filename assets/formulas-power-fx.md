# Fórmulas Power Fx del curso

| Uso | Práctica | Fórmula |
|---|---|---|
| Texto de la aplicación con nivel de permiso | 2 | `Text(Topic.Aplicacion) & If(IsBlank(Topic.NivelPermiso), "", " - " & Text(Topic.NivelPermiso))` |
| ¿Requiere autorización adicional? | 2 | `Or(Text(Topic.Aplicacion) = "TesoNet (Tesorería)", Text(Topic.Aplicacion) = "Consola de Servidores", Text(Topic.NivelPermiso) = "Administración")` |
| Validar datos obligatorios | 2 | `And(!IsBlank(Topic.Justificacion), !IsBlank(Topic.CorreoJefe))` |
| Estado inicial | 2 y 6 | `If(Topic.RequiereAutorizacion, "Pendiente de autorización", "Registrada")` |
| Pregunta para respuestas generativas | 4 | `"¿Cuáles son los requisitos y el aprobador para una solicitud de " & Text(Topic.TipoSolicitud) & " de " & Topic.AplicacionTexto & "?"` |
| Enviar una opción como texto al flujo | 6 | `Text(Topic.TipoSolicitud)` |

> Las variables de tipo **opción** se convierten a texto con `Text()` antes de compararlas con cadenas o enviarlas a parámetros de tipo Texto.

Referencia: [Crear expresiones con Power Fx en Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/advanced-power-fx)
