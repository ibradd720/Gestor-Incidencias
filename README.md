# Gestor de incidencias con Docker Compose

Práctica con tres servicios (frontend, backend y PostgreSQL), dos redes y un volumen para no perder los datos.

## Cómo arrancarlo

1. Copiar `.env.sample` a `.env` y poner las contraseñas.
2. `docker compose up -d`
3. Abrir http://localhost:8080

Nota: las imágenes son ARM64 y mi PC es amd64, así que dejé `platform: linux/arm64` y activé la emulación en Docker Desktop.

## Preguntas

1. **¿Por qué el frontend no necesita conectarse a PostgreSQL?**
Solo muestra la web y pasa las peticiones `/api/` al backend, que es el que habla con la base de datos.

2. **¿Por qué el backend pertenece a dos redes?**
Está en medio: por `frontend` recibe las peticiones y por `datos` llega a PostgreSQL.

3. **¿Qué servicio necesita publicar un puerto en el anfitrión?**
Solo el frontend, porque es al que entramos desde el navegador.

4. **¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?**
Deja la base de datos expuesta y no arregla nada, porque el fallo suele ser de redes, nombres o de arranque, no de puertos.

5. **¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?**
Todo está en un archivo y con un comando se levanta o se borra. Es reproducible y se puede subir a Git.

6. **¿Por qué hemos dividido la arquitectura en dos redes?**
Para aislar: el frontend no puede llegar a PostgreSQL porque no comparten red, solo pasa por el backend.

7. **¿Por qué el backend utiliza `db` como dirección de PostgreSQL?**
Porque el nombre del servicio funciona como DNS en la red. `localhost` sería el propio backend y la IP cambia al recrear el contenedor.

8. **¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?**
Con `stop` o `down` los datos se conservan en el volumen. Con `down -v` se borra el volumen y las incidencias se pierden.

9. **¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?**
Perderían todas las incidencias y su historial, y sin copia de seguridad no se recuperarían.