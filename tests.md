# Evidencias de pruebas

Las pruebas de la API se ejecutaron con Thunder Client (alternativa de postman) sobre un servidor local (`python manage.py runserver`). Cada captura se encuentra en la carpeta `imagenes/` y muestra el estado correcto del endpoint probado.

1. **Listar tareas (`GET /api/v1/tareas/`)**  
   ![Listado de tareas](imagenes/listar_tareas.png)

2. **Crear tarea (`POST /api/v1/tareas/`)**  
   ![Creacion de tarea](imagenes/crear_tarea.png)

3. **Detalle de tarea (`GET /api/v1/tareas/{id}/`)**  
   ![Detalle de tarea](imagenes/detalle_tarea.png)

4. **Actualizar tarea (`PATCH /api/v1/tareas/{id}/`)**  
   ![Actualizacion de tarea](imagenes/actualizar_tarea.png)

5. **Eliminar tarea (`DELETE /api/v1/tareas/{id}/`)**  
   ![Eliminacion de tarea](imagenes/eliminar_tarea.png)

### Escenarios de error

1. **404 Not Found (`GET /api/v1/tareas/9999/`)**  
   ![Recurso inexistente](imagenes/404.png)  
   

2. **400 Bad Request (`PATCH /api/v1/tareas/2/`)**  
   ![Validacion de titulo vacio](imagenes/400.png)
