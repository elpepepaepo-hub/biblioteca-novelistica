# Biblioteca Novelística — Guía de instalación y uso

**Edición:** 2026-11-03 · **Estado:** guía de la versión privada 0.5.0 y prototipo visual 0.6.0.

> **Antes de comenzar:** este sitio público de GitHub Pages no instala el complemento, no ejecuta servidores ni contiene los ZIP privados. El acceso a la importación de complementos y a conectores MCP depende de tu cuenta de ChatGPT. La conexión remota y OAuth **no han sido comprobadas de extremo a extremo**. No utilices manuscritos irremplazables en un despliegue experimental.

## Elige tu camino

| Pregunta | Modo sencillo | Modo persistente |
|---|---|---|
| ¿Necesito conocimientos técnicos? | No, salvo la instalación privada del complemento | Sí, o ayuda de alguien con experiencia |
| ¿Necesito pagar alojamiento? | No | Posiblemente; consulta precios antes de contratar |
| ¿Dónde quedan mis fichas? | En el contexto disponible de ChatGPT y en los archivos que tú guardes | En tu base de datos SQLite, cuando tu servidor funcione |
| ¿Se sincroniza automáticamente entre sesiones? | No | Es el objetivo, **aún sin conexión real validada** |
| ¿Quién administra el servidor? | Nadie | Tú |

## A. Modo sencillo: paso a paso, sin programación

1. **Consigue el ZIP privado del complemento.** Solicítalo al autor o distribuidor autorizado. No lo busques en este repositorio público: no está alojado aquí.
2. **Entra en ChatGPT y comprueba los permisos de tu cuenta.** Busca la opción de complementos y, si existe, la función para instalar/importar un complemento privado. No todos los planes, regiones y espacios de trabajo ofrecen la misma opción. Si no aparece, **detente**: el ZIP no puede forzar esa función. Consulta con el administrador de tu cuenta o utiliza los métodos de distribución permitidos por ChatGPT.
3. **Instala el ZIP solo si ChatGPT te ofrece un mecanismo compatible.** Sigue las indicaciones que aparezcan en pantalla. No subas el ZIP a un repositorio público para intentar instalarlo.
4. **Abre Biblioteca Novelística y elige «Modo sencillo».** Si el complemento pregunta qué quieres organizar, responde con el título de la novela y una descripción breve del mundo.
5. **Empieza con una ficha.** Por ejemplo: «Crea una ficha para la protagonista Ana, capitana de la guardia, y marca como canon únicamente lo que yo confirme».
6. **Registra hechos con procedencia.** Indica capítulo, escena, documento y versión cuando los conozcas. Distingue canon, propuesta, alternativa y contradicción.
7. **Pide una auditoría.** Ejemplo: «Revisa las fechas y los poderes de Ana entre los capítulos 2 y 7. Clasifica las inconsistencias como graves, importantes o menores».
8. **Guarda una copia.** Copia los registros en un documento local o solicita una exportación textual/JSON cuando esté disponible. Repite después de cambios importantes.
9. **Al volver otro día, aporta el contexto necesario.** Adjunta o pega tus registros guardados. El modelo no garantiza recordar todos los hechos de conversaciones anteriores.

**Resultado esperado:** asesoría editorial, fichas y detección de contradicciones basadas en el material disponible. **No hay guardado automático externo.**

### Si no puedes instalar el complemento

No es posible asegurar que todos los usuarios puedan importar un ZIP privado. Puedes conservar el archivo hasta disponer de una cuenta o entorno compatible. La web pública es informativa; no reemplaza el instalador.

## B. Modo persistente: paso a paso, con infraestructura propia

**Advertencia de estado:** las instrucciones siguientes describen la ruta de preparación y pruebas. El backend y el asistente existen como prototipos, pero la autenticación OAuth real y el uso de herramientas MCP desde ChatGPT **no están certificados**. No basta con abrir una conversación para que el servidor se despliegue.

### B1. Prepara lo necesario

1. Una computadora con **Python 3.11 o posterior**, terminal y, para despliegue con contenedores, Docker.
2. El **ZIP privado del backend autogestionado v0.5.0**, proporcionado por el distribuidor autorizado.
3. Una cuenta en el alojamiento que elijas: Render, Railway, Fly.io o servidor propio. Revisa sus precios, reglas de almacenamiento y facturación **antes de crear recursos de pago**.
4. Una cuenta en un proveedor de identidad: Auth0, Amazon Cognito o Keycloak (este último exige administrarlo por tu cuenta).
5. Tiempo para realizar pruebas con **datos inventados** y un lugar seguro para guardar copias de seguridad.

### B2. Genera un plan de instalación

1. Descomprime el ZIP **del backend**, no el ZIP de GitHub Pages.
2. Abre una terminal dentro de la carpeta que contiene `pyproject.toml`.
3. Instala las dependencias de Python del proyecto (puedes usar un entorno virtual):

   ```bash
   python -m pip install -e '.[api,remote,mcp,test]'
   ```

4. Ejecuta el asistente interactivo:

   ```bash
   biblioteca-wizard
   ```

5. Elige **persistente**, un alojamiento, **SQLite** y **OAuth**. En despliegues públicos, el asistente debe rechazar configuraciones sin autenticación.
6. También puedes generar un plan de ejemplo sin preguntas:

   ```bash
   biblioteca-wizard --mode persistente --host railway --database sqlite --auth oauth --identity auth0 --output mi-instalacion
   ```

7. Abre la carpeta `mi-instalacion`. Contiene un plan y plantillas, **no** un servidor ya creado ni credenciales reales.

**No selecciones PostgreSQL todavía:** no existe un adaptador funcional validado en esta versión.

### B3. Contrata o configura tu alojamiento

1. Entra **directamente** al panel del proveedor elegido. Crea un proyecto privado y revisa la estimación de costes.
2. Despliega el código del backend con la plantilla Docker correspondiente. El procedimiento exacto cambia según el proveedor y debe seguir su documentación actual.
3. Configura un **volumen o disco persistente** para SQLite. Si los datos quedan en almacenamiento efímero, podrían desaparecer al reiniciar o reconstruir el servicio.
4. Utiliza una sola instancia escritora para SQLite; no escales a múltiples instancias escribiendo en el mismo archivo.
5. Activa HTTPS para la dirección pública. No expongas el puerto de SQLite ni una consola de administración.
6. Guarda los valores sensibles en el gestor de **variables secretas del proveedor**, nunca en ChatGPT, GitHub público o el ZIP.

### B4. Configura el inicio de sesión

1. En el proveedor de identidad crea una aplicación para la biblioteca.
2. Configura **OAuth Authorization Code + PKCE** y la emisión de tokens adecuados para tu servidor.
3. Obtén los datos públicos de configuración: `issuer`, `audience` y dirección `JWKS` (HTTPS). Configura los valores reales en el servidor según `docs/MCP_OAUTH.md` del backend.
4. Configura las URL de redirección **exactas** que te muestre ChatGPT durante el proceso de conexión; no inventes una URL de retorno.
5. No pegues contraseñas, tokens ni claves privadas en el chat. No incluyas secretos en archivos públicos.
6. Antes de conectar un manuscrito, prueba que los tokens caducados, inválidos o emitidos para otra audiencia se rechazan.

**Importante:** el backend valida tokens; no incluye un proveedor OAuth completo. El inicio de sesión de extremo a extremo aún debe probarse con un proveedor real.

### B5. Comprueba el servidor y conecta ChatGPT

1. Verifica que el proceso MCP está en marcha y que el endpoint real es accesible por HTTPS, por ejemplo `https://TU-DOMINIO/mcp` (solo ilustrativo).
2. Comprueba que la configuración del servidor, el recurso OAuth y el descubrimiento de herramientas funcionan con un cliente MCP de pruebas compatible.
3. Abre la configuración de conectores/complementos de ChatGPT **si tu cuenta admite servidores MCP personalizados**. Registra el endpoint real siguiendo la interfaz disponible; no todos los usuarios pueden hacerlo.
4. Completa el consentimiento OAuth desde la pantalla segura del proveedor.
5. Prueba con una **novela ficticia**: crear un personaje, consultar su ficha, proponer un hecho y verificar que no se convierta en canon sin una confirmación válida.
6. Desconecta la cuenta, prueba con otro usuario de ensayo y confirma que no puede leer la primera biblioteca.
7. Solo tras superar esas pruebas, decide si usarás datos reales. Si falla alguna, vuelve al modo sencillo.

### B6. Copias de seguridad y migración

1. Programa respaldos del volumen SQLite y guarda una copia cifrada fuera del servidor.
2. Usa las funciones de exportación del motor cuando estén disponibles.
3. Verifica una **restauración real en una instalación de pruebas** antes de depender de ella. La importación remota todavía necesita validación adicional.
4. Para cambiar de proveedor, prepara la nueva instalación, restaura una copia verificada, revisa permisos y fechas y solo entonces cambia la conexión de ChatGPT.
5. Mantén respaldos anteriores hasta comprobar la migración.

## C. Problemas frecuentes

| Problema | Qué hacer |
|---|---|
| «No veo la opción de importar ZIP» | Comprueba permisos y disponibilidad; no hay instalación universal garantizada. |
| «El servidor responde, pero ChatGPT no conecta» | Comprueba HTTPS, endpoint `/mcp`, permisos del conector, descubrimiento y OAuth. |
| «OAuth dice redirección inválida» | Copia la URI exacta facilitada por ChatGPT en la configuración del proveedor. |
| «Mis datos desaparecieron» | Revisa el volumen persistente; restaura una copia de seguridad probada. |
| «El asistente bloquea mi configuración» | No desactives las validaciones: cambia a OAuth para Internet o usa un entorno local aislado. |
| «La app visual no aparece» | La interfaz v0.6.0 es un prototipo; falta registrarla y probarla como recurso MCP App. |

## D. Seguridad y responsabilidad

- Cada escritor administra su infraestructura, sus costes, sus respaldos y su servicio de identidad.
- Un servidor accesible desde Internet debe exigir autenticación y HTTPS. **Nunca** publiques una instalación sin autenticación.
- No envíes manuscritos, secretos o credenciales a canales públicos para solicitar soporte.
- El creador del complemento no aloja una base de datos central para las novelas.
- No confundas la página informativa de GitHub Pages con el servidor MCP.

## E. Para desarrolladores y administradores

Consulta [HOJA_DE_RUTA.md](HOJA_DE_RUTA.md) para conocer qué falta antes de considerar operativo el modo persistente. Consulta también `docs/INSTALACION_AUTOGESTIONADA.md`, `docs/MCP_OAUTH.md` y `docs/DEPLOY_REMOTO.md` **dentro del ZIP privado del backend**.
