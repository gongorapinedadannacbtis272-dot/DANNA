# Conectar Reserva Temporal al Atlas de Libreria Honey

- Cluster: el mismo que utiliza Libreria Honey.
- Variable compartida: `MONGO_URI` (copiar el valor de la configuracion existente, sin publicarlo).
- Base de datos de Honey: `LIBRERIA_HONEY` (no se modifica).
- Base de datos de esta actividad: `RESERVA_TEMPORAL_HONEY`.
- Coleccion de esta actividad: `boletos`.
- Duracion inicial: 60 segundos.

## Local
Crea `.env` a partir de `.env.example` y pega en `MONGO_URI` la misma cadena del cluster de Honey.

## Render
En el servicio de Reserva Temporal, configura `MONGO_URI` con el mismo valor del servicio Honey, `MONGO_DB=RESERVA_TEMPORAL_HONEY` y `RESERVA_SEGUNDOS=60`. Las variables de entorno no se comparten automaticamente entre servicios: hay que agregarlas en cada servicio.

No publiques `.env`, URI, usuario ni contrasena en GitHub o capturas.
