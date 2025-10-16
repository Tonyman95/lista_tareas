# API de Tareas

## Recursos y URIs
- `/api/v1/tareas/`: coleccion principal de tareas.
- `/api/v1/tareas/{id}/`: recurso individual identificado por su `id`.

## Verbos y codigos HTTP esperados

| Verbo | URI | Descripcion | Codigos exitosos | Codigos de error |
| --- | --- | --- | --- | --- |
| `GET` | `/api/v1/tareas/` | Lista paginada de tareas ordenadas por fecha de creacion descendente. | `200` | `400` |
| `POST` | `/api/v1/tareas/` | Crea una nueva tarea con `titulo` obligatorio y `hecho` opcional. | `201` | `400` |
| `GET` | `/api/v1/tareas/{id}/` | Recupera el detalle de una tarea existente. | `200` | `404` |
| `PATCH` | `/api/v1/tareas/{id}/` | Actualiza parcialmente `titulo` o `hecho` de la tarea. | `200` | `400`, `404` |
| `DELETE` | `/api/v1/tareas/{id}/` | Elimina la tarea indicada. | `204` | `404` |

## Reglas REST aplicadas
- **Stateless**: cada peticion lleva la informacion necesaria (auth, payload); el servidor no mantiene sesion.
- **JSON**: todas las peticiones y respuestas usan `Content-Type: application/json`.
- **Versionado**: la ruta raiz incluye `/api/v1/`, permitiendo evolucionar la API sin romper clientes existentes.
- **Idempotencia**: `GET` nunca cambia el estado, `PATCH` repetido con el mismo body deja el recurso igual, y `DELETE` responde `204` aunque el recurso ya no exista tras la primera eliminacion.

## Arquitectura por capas
```text
[Cliente (curl/SPA)]
        |
      HTTP/JSON
        |
[ API /api/v1 (DRF ViewSets/URLs) ]
        |
[ Logica/Serializers (validacion) ]
        |
[ Modelo Django (ORM) ]
        |
[ DB SQLite (local) ]
```
- **Cliente (curl/SPA)**: origen de las peticiones HTTP con los cuerpos JSON.
- **API /api/v1 (DRF ViewSets/URLs)**: expone los endpoints, resuelve rutas y aplica permisos y paginacion.
- **Logica/Serializers (validacion)**: transforma datos entre objetos Python y JSON, validando campos requeridos.
- **Modelo Django (ORM)**: define la entidad `Tarea` y ofrece operaciones de persistencia abstractas.
- **DB SQLite (local)**: almacen fisico donde se guardan las tareas en disco.

## Componentes relevantes del proyecto
- Modelo `Tarea` (`tareas/models.py`): define `titulo`, `hecho` y `created_at`.
- Serializer `TareaSerializer` (`tareas/serializers.py`): valida el titulo y expone todos los campos.
- ViewSet `TareaViewSet` (`tareas/views.py`): implementa la logica CRUD con ordenamiento por fecha.
- Ruteo (`tareas_api/urls.py`): monta los endpoints bajo `/api/v1/tareas/` con `DefaultRouter`.
