# §5 · TRAMPAS DE SEGURIDAD CONOCIDAS 🔒 NO TOCAR

> Fallos reales y no evidentes. Verifícalos explícitamente: ninguno se detecta "leyendo el código con
> atención".

### Base de datos

1. **`TRUNCATE` no está sujeto a la seguridad por fila.** PostgreSQL aplica las políticas RLS solo a
   `SELECT`, `INSERT`, `UPDATE` y `DELETE`. Un rol con `TRUNCATE` **borra la tabla completa** aunque
   las políticas lo protejan todo. Si la API no expone el comando, la defensa es la *forma de la API*,
   no el modelo de permisos — y eso cambia el día que se añade otra vía de acceso.
   → **Revócalo explícitamente** a los roles públicos y ajusta los privilegios por defecto para que
   ninguna tabla futura lo herede.

2. **Audita el ACL completo, no solo las políticas.** Un `GRANT ALL` concede también `REFERENCES` y
   `TRIGGER`, que casi nadie revisa. Consulta los privilegios reales en el catálogo del sistema.

3. **Funciones con privilegios elevados.** Una función que se ejecuta con los permisos de su
   propietario y es invocable por usuarios anónimos es una escalada de privilegios. Debe existir un
   test automático que falle si aparece una.

4. **Audita el estado vivo, no las migraciones.** Las migraciones son historia; la base de datos es el
   estado. Una función redefinida varias veces solo se conoce consultándola en vivo.

5. **Índices "sin usar" en una base vacía.** Un panel de rendimiento sobre una base sin datos marcará
   todos los índices como no usados: significa *"no hay datos para juzgarlos"*, no *"sobran"*.
   Borrarlos por esa señal rompe el rendimiento en producción.

### Aplicación

6. **Metadatos EXIF de las imágenes.** Las coordenadas GPS viajan **dentro** del archivo de la foto.
   Publicar la imagen publica la ubicación exacta aunque la app nunca guarde coordenadas.

7. **Limitadores de tasa que "fallan abiertos".** Si el limitador deja pasar todo cuando su
   almacenamiento no responde, un atacante solo necesita tumbar ese almacenamiento. Decide
   explícitamente si debe fallar abierto o cerrado, y documéntalo.

8. **CORS con origen comodín** en funciones de backend: revísalo antes de publicar.

9. **Logs y mensajes de error.** No deben contener datos personales, identificadores, contenido de
   usuario, tokens ni trazas sensibles. Aplica también a los mensajes que ve el usuario final.

### Entorno del agente

10. **Nunca versiones la configuración local del agente**, y **nunca** uses comodines de permisos para
    git. Un patrón tipo "permitir todos los comandos de git" auto-aprueba `push --force`,
    `reset --hard` y `commit --no-verify`: justo lo que los demás controles intentan impedir.

---

