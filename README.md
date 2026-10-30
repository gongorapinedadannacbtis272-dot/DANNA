# CRUD Flask con carrito y reserva temporal

Proyecto de demostracion para continuar el CRUD inicial desarrollado con:

- Python y Flask.
- MongoDB Atlas.
- HTML, CSS y JavaScript.
- GitHub y Render.

El ejemplo simula la venta de boletos. Cuando una persona agrega un boleto al
carrito, queda reservado durante 60 segundos. En otra ventana aparece como
reservado y no puede seleccionarse. La reserva puede cancelarse, vencer o
confirmarse como compra.

## Funciones incluidas

- Crear, consultar, editar y eliminar boletos.
- Cargar seis registros de demostracion.
- Reservar un boleto de manera segura.
- Identificar quien realizo la reserva.
- Mostrar un contador en el carrito.
- Impedir que otro usuario reserve el mismo boleto.
- Cancelar una reserva.
- Liberar automaticamente reservas vencidas.
- Confirmar la compra.
- Filtrar por disponible, reservado o comprado.

## Estructura del proyecto

```text
proyecto_flask_reserva_temporal/
|
|-- app.py
|-- requirements.txt
|-- Procfile
|-- render.yaml
|-- .env.example
|
|-- templates/
|   |-- base.html
|   `-- index.html
|
`-- static/
    |-- css/
    |   `-- estilos.css
    `-- js/
        `-- app.js
```

## 1. Abrir en Visual Studio Code

1. Descomprime el archivo ZIP.
2. Abre Visual Studio Code.
3. Selecciona **Archivo > Abrir carpeta**.
4. Elige la carpeta `proyecto_flask_reserva_temporal`.
5. Abre una terminal dentro de Visual Studio Code.

## 2. Crear el entorno virtual

### Windows PowerShell

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

### Windows CMD

```bat
python -m venv venv
venv\Scripts\activate
```

### macOS o Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

## 3. Instalar las dependencias

```bash
pip install -r requirements.txt
```

## 4. Conectar al mismo MongoDB Atlas de Libreria Honey

Este proyecto puede usar el **mismo cluster de MongoDB Atlas** que ya utiliza tu aplicacion Libreria Honey. No necesitas crear otro cluster ni otra cuenta.

**Importante:** reutilizamos la misma variable `MONGO_URI` (la cadena de conexion del cluster), pero guardamos los boletos en una base de datos independiente llamada `RESERVA_TEMPORAL_HONEY`. Libreria Honey conserva su base `LIBRERIA_HONEY` y sus colecciones (`libros`, `usuarios`, `mercancia`, etc.). Asi no mezclamos ni modificamos los libros, PDFs o usuarios de Honey.

### En tu computadora (VS Code)

1. Copia `.env.example` y cambia el nombre de la copia a `.env`.
2. Abre el `.env` de Libreria Honey o revisa la variable `MONGO_URI` que ya utilizas en esa aplicacion.
3. Copia **solo el valor de `MONGO_URI`** (la cadena completa de conexion del mismo cluster) al `.env` de este proyecto. No copies contrasenas a este README ni las publiques.
4. Deja las demas variables asi:

```env
MONGO_URI=PEGA_AQUI_LA_MISMA_URI_DE_ATLAS_QUE_USA_HONEY
MONGO_DB=RESERVA_TEMPORAL_HONEY
RESERVA_SEGUNDOS=60
PORT=5000
FLASK_DEBUG=1
```

No subas `.env` a GitHub. Ya esta incluido en `.gitignore`. Si Honey usa una URI distinta por tener otro cluster, confirma en Atlas que ambas aplicaciones apunten al cluster que deseas compartir.

### En Render

En el servicio web de este proyecto, abre **Environment** y configura:

- `MONGO_URI`: pega la misma cadena de conexion que esta guardada en el servicio Render de Libreria Honey.
- `MONGO_DB`: `RESERVA_TEMPORAL_HONEY`.
- `RESERVA_SEGUNDOS`: `60`.

Guarda los cambios y realiza un nuevo deploy. La variable `MONGO_URI` debe configurarse por separado en cada servicio de Render; compartir el cluster no hace que Render copie automaticamente las variables entre aplicaciones.

En MongoDB Atlas comprueba:

- Que el usuario de base de datos tenga permisos de lectura y escritura.
- Que la contrasena sea correcta.
- Que Network Access permita la conexion desde tu equipo.

## 5. Ejecutar el proyecto

```bash
python app.py
```

En macOS o Linux tambien puede utilizarse:

```bash
python3 app.py
```

Abre en el navegador:

```text
http://127.0.0.1:5000
```

Presiona **Cargar 6 boletos de ejemplo** para preparar la demostracion.

## 6. Probar dos compradores

1. En la ventana normal escribe `Comprador 1`.
2. Abre una ventana de incognito en la misma direccion.
3. En incognito escribe `Comprador 2`.
4. Reserva el asiento A-01 desde la primera ventana.
5. Actualiza la segunda ventana.
6. Comprueba que A-01 aparece reservado.
7. Intenta reservarlo desde la segunda ventana.
8. La API debe rechazar la segunda reserva.

Despues pruebe:

- Cancelar la reserva.
- Dejar que el contador llegue a cero.
- Confirmar la compra.

## 7. Rutas principales de la API

| Metodo | Ruta | Funcion |
|---|---|---|
| GET | `/api/salud` | Comprueba Flask y Atlas |
| GET | `/api/boletos` | Consulta boletos y libera vencidos |
| POST | `/api/boletos` | Crea un boleto |
| PUT | `/api/boletos/<id>` | Edita un boleto disponible |
| DELETE | `/api/boletos/<id>` | Elimina un boleto disponible |
| POST | `/api/boletos/semilla` | Carga seis ejemplos |
| POST | `/api/boletos/<id>/reservar` | Crea la reserva temporal |
| POST | `/api/boletos/<id>/cancelar` | Cancela la reserva propia |
| POST | `/api/boletos/<id>/confirmar` | Confirma la compra |
| POST | `/api/boletos/liberar-vencidas` | Libera reservas vencidas |

## 8. Subir a GitHub

```bash
git init
git add .
git commit -m "CRUD Flask con reserva temporal"
git branch -M main
git remote add origin URL_DE_TU_REPOSITORIO
git push -u origin main
```

Revise que `.env` no aparezca dentro del repositorio.

## 9. Publicar en Render

1. Crea un nuevo **Web Service**.
2. Conecta el repositorio de GitHub.
3. Utiliza estos datos:

```text
Build Command: pip install -r requirements.txt
Start Command: gunicorn app:app
```

4. Agrega las variables de entorno:

```text
MONGO_URI = cadena completa de MongoDB Atlas
MONGO_DB = RESERVA_TEMPORAL_HONEY
RESERVA_SEGUNDOS = 60
```

5. Realiza el despliegue.

Para una demostracion rapida use 60 segundos. En el proyecto final puede cambiar
`RESERVA_SEGUNDOS` a 300 para reservar durante cinco minutos.

## Idea central del ejemplo

La reserva no depende solamente del contador de JavaScript. Flask realiza en
MongoDB una operacion que cambia el boleto a reservado solo si todavia esta
disponible. Por eso dos personas no pueden apartar correctamente el mismo
boleto al mismo tiempo.
