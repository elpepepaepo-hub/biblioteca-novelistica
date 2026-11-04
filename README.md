# Biblioteca Novelística — web para GitHub Pages

Sitio **público e informativo** de Biblioteca Novelística (El Cerebro de la Novela).

## Contenido

- `index.html`: presentación y modos sencillo/persistente.
- `funciones.html`: funciones editoriales y estados del canon.
- `instalacion.html`: instrucciones y límites de instalación, sin credenciales.
- `compatibilidad.html`: modelos GPT-6 y planes personales, con enlaces a fuentes oficiales.
- `privacidad.html`, `terminos.html`: borradores legales pendientes de aprobación.
- `soporte.html`: FAQ; contacto específico **pendiente de confirmar**.
- `docs/estado-proyecto.html`: estado técnico y pendientes.
- `docs/HOJA_DE_RUTA.md`: fases pendientes, prioridades y criterios de aceptación.
- `docs/GUIA_DE_INSTALACION.md`: pasos detallados para usuarios principiantes en modo sencillo y persistente.
- `assets/`: estilos e icono SVG.

## Publicar en GitHub Pages

1. Crea un repositorio **público** (por ejemplo `biblioteca-novelistica`).
2. Sube el **contenido** de este ZIP a la raíz del repositorio; **no** subas ZIP privados, código del backend ni contraseñas.
3. Ve a `Settings > Pages > Build and deployment`; selecciona `Deploy from a branch`, rama `main`, carpeta `/(root)`.
4. Comprueba las páginas en `https://TU-USUARIO.github.io/biblioteca-novelistica/` (sustituye el usuario real).
5. Revisa/autoriza los textos legales, define y verifica un correo de soporte, y actualiza las páginas antes de difundirlas como documentos definitivos.

Si deseas mantener el código privado, recuerda que **todo archivo subido a este repositorio público es visible para todos**. Conserva el ZIP real del complemento y su backend exclusivamente en almacenamiento privado.

## Limitaciones técnicas

El sitio no instala complementos, no ejecuta servidores MCP ni crea bases de datos. La versión v0.6.0 es un prototipo visual separado, no una MCP App integrada en ChatGPT. El modo persistente requiere un servidor HTTPS y OAuth configurados y probados por el escritor.

## Compatibilidad y fuentes

Fuentes consultadas 2026-11-03:
- https://help.openai.com/es-419/articles/20001354-gpt-6-and-other-models-in-chatgpt
- https://help.openai.com/es-419/articles/20001256-plugins-in-chatgpt
- https://developers.openai.com/plugins/build/plugins

El directorio está disponible en todos los planes, pero instalar ZIP privados y conectar MCP depende de permisos, cuenta y superficie de ChatGPT. No afirmamos que todos los planes admitan el complemento.

## Licencia y responsabilidad

El código de este sitio no concede licencia sobre el complemento privado. La política legal del proyecto está pendiente de aprobación. Proyecto independiente de Martin Emiliano Ortega Velazquez, no afiliado a OpenAI.

## Guías ampliadas

- [Guía de instalación y uso](docs/GUIA_DE_INSTALACION.md)
- [Hoja de ruta para completar el complemento](docs/HOJA_DE_RUTA.md)

Estas guías explican las limitaciones actuales; no certifican una conexión MCP/OAuth real.
