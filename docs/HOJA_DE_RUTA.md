# Biblioteca Novelística — Hoja de ruta técnica

**Corte:** 2026-11-03 · **Estado del complemento instalado:** privado v0.5.0 · **Interfaz visual:** prototipo independiente v0.6.0.

## Propósito y alcance

Cada escritor administrará su propio servidor, su base de datos, su proveedor de identidad y sus costes. El complemento tendrá dos modos: **sencillo**, basado en conversación sin persistencia externa, y **persistente**, con un servidor MCP propio y datos guardados en SQLite. La distribución será por ZIP privado; **no** se contempla publicar el complemento en el directorio público.

El sitio GitHub Pages contiene solo información pública. No debe alojar código backend, ZIP privados, secretos, archivos de bases de datos ni manuscritos.

## Estado de partida

| Componente | Estado conocido | Limitación |
|---|---|---|
| Instrucciones de Biblioteca Novelística | Complemento privado v0.5.0 | Importación/uso sujetos a permisos de ChatGPT |
| Canon, fichas y auditoría | Motor prototipo con pruebas locales | Auditoría semántica integral pendiente |
| SQLite y API | Código local preparado | Operación remota y restauración no verificadas integralmente |
| Instalador guiado | Selección de hosting/identidad, plantillas y bloqueos básicos | No crea cuentas ni despliega por sí solo |
| OAuth/OIDC | Validación JWT programada | Flujo real de consentimiento no verificado |
| MCP remoto | Adaptador preparado | Compatibilidad y sesión ChatGPT sin verificar |
| Interfaz v0.6.0 | HTML/CSS/JS y adaptador experimental | No es aún un recurso MCP App operativo |
| PostgreSQL | No implementado | El asistente debe rechazarlo |

## Orden recomendado para terminar el proyecto

### Hito 1 — Congelar y auditar los paquetes privados

**Objetivo:** disponer de una base reproducible. **Trabajo:** conservar ZIP del complemento v0.5.0, backend v0.5.0 e interfaz v0.6.0; registrar hashes, dependencias y versiones; revisar que los archivos del backend no se publiquen por accidente.

**Prueba de aceptación:** instalación reproducible desde ZIP en un entorno de pruebas, dependencias identificadas y ninguna credencial en el repositorio.

### Hito 2 — Integrar MCP con el backend real

**Objetivo:** exponer herramientas MCP estables. **Trabajo:** comprobar compatibilidad con la versión del SDK MCP, implementar y probar `tools/list`, lectura de novelas/entidades, propuestas, cronologías y auditoría; añadir errores claros y límites de entrada. Anotar herramientas según su comportamiento de lectura/escritura.

**Prueba de aceptación:** cliente MCP de prueba descubre y ejecuta herramientas con datos ficticios, con errores controlados.

### Hito 3 — OAuth/OIDC y autorización robusta

**Objetivo:** impedir accesos ajenos y cambios no autorizados. **Trabajo:** probar al menos un proveedor real (por ejemplo Auth0), Authorization Code + PKCE, issuer/audience/JWKS, caducidad, scopes y aislamiento por identidad; proteger operaciones de escritura. Sustituir la bandera `writer_confirmed` por una confirmación humana comprobable y transaccional.

**Prueba de aceptación:** token inválido rechazado; escritor B no puede leer ni modificar datos de A; una propuesta no pasa a canon sin aprobación válida.

### Hito 4 — Despliegue autogestionado de referencia

**Objetivo:** una instalación reproducible en al menos un proveedor. **Trabajo:** probar un proveedor de hosting de principio a fin; HTTPS, volumen persistente SQLite, reinicio, copia de seguridad, restauración y gastos estimados; mantener los secretos en el panel del proveedor.

**Prueba de aceptación:** reiniciar y volver a desplegar no borra datos; acceso sin autenticación rechazado; recuperación de copia verificada.

### Hito 5 — Conexión real con ChatGPT

**Objetivo:** demostrar el modo persistente dentro de una cuenta compatible. **Trabajo:** registrar el endpoint MCP real, configurar redirecciones OAuth exactas, autorizar, descubrir herramientas y ejecutar pruebas de lectura/escritura con una novela ficticia. Documentar limitaciones de cuenta/plan.

**Prueba de aceptación:** un usuario autorizado crea, consulta y propone registros desde ChatGPT, y otro usuario no puede acceder a ellos. Registrar evidencias sin secretos.

### Hito 6 — MCP App visual

**Objetivo:** transformar la interfaz v0.6.0 en una aplicación real dentro de ChatGPT. **Trabajo:** registrar recurso de UI en el servidor MCP, enlazar herramientas y eventos, integrar autenticación y estados de carga/error, probar cronología, fichas, canon y continuidad. Adaptar a móvil y accesibilidad.

**Prueba de aceptación:** la interfaz se abre en el host compatible y muestra datos reales del usuario correcto; acciones de escritura requieren confirmación y no exponen secretos.

### Hito 7 — Portabilidad y operaciones

**Objetivo:** permitir migrar de proveedor. **Trabajo:** validar exportación, importación remota, versiones de esquema, cifrado de respaldos, restauración, eliminación y documentación de recuperación. Evaluar PostgreSQL solo después de implementar y probar su adaptador.

**Prueba de aceptación:** biblioteca exportada e importada en una instalación limpia conserva entidades, procedencia, estados temporales, canon y ramas sin pérdida.

### Hito 8 — Asistente para no técnicos

**Objetivo:** reducir los pasos manuales sin comprometer seguridad. **Trabajo:** guías específicas por proveedor, verificador de configuración, mensajes comprensibles, estimación de costes y diagnóstico; no realizar compras o cambios de cuenta sin autorización.

**Prueba de aceptación:** una persona sin experiencia completa una instalación de prueba siguiendo la guía, sin compartir credenciales en el chat y sin desactivar controles de seguridad.

## Matriz de pruebas mínimas antes de usar manuscritos reales

- Inicio de sesión correcto, denegado y caducado.
- Aislamiento entre escritores y entre novelas.
- Lectura, propuesta, aprobación y rechazo de canon.
- Contradicciones graves, importantes y menores; no sobrescribir canon.
- Cronologías con saltos temporales y fechas desconocidas.
- Reinicio, despliegue, pérdida simulada de instancia y restauración.
- Exportación/importación con caracteres Unicode y datos extensos.
- Acceso HTTPS, gestión de secretos, límites de uso y registro de errores sin manuscritos completos.
- Interfaz MCP App con errores de red, OAuth vencido y permisos insuficientes.

## Decisiones que siguen abiertas

- Primer alojamiento y primer proveedor OAuth a certificar en una prueba real.
- Política de retención y borrado de respaldos en instalaciones individuales.
- Alcance de generación de imágenes oficiales y almacenamiento de recursos multimedia.
- Implementación de avisos por inactividad: requiere programador de tareas y canal de notificaciones.
- Soporte y mantenimiento de versiones futuras; el autor no operará servidores de los escritores.

## Regla para futuras versiones

No llamar «función operativa» a un prototipo sin prueba en el entorno de destino. Mantener separados: **código escrito**, **pruebas locales**, **despliegue validado** y **conexión comprobada en ChatGPT**. No introducir URLs, tokens o credenciales inventados en `mcp.json`.

Para instrucciones de usuario, consulta [GUIA_DE_INSTALACION.md](GUIA_DE_INSTALACION.md).
