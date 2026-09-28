# Aplicación Web Django — Gestión de Mascotas (CRUD de consulta)

Aplicación web de práctica hecha con **Django + Bootstrap** para listar registros desde SQLite.
Actualmente funciona como **módulo de consulta (Read)**: muestra mascotas en tabla, sin `update` ni `delete` funcionales.

> Descripción original del repo: *"consulta solamente no tiene el update y el delete"*.

## Demo / Qué hace

- `GET /` → lista todas las `Mascota` (`id, especie, raza, nombre`) en tabla Bootstrap.
- `GET /acerca` → página simple de prueba (`index2.html`).
- `GET /base/` → renderiza la plantilla base con navbar + accordion.
- `GET /admin/` → admin de Django con los 10 modelos registrados.

Los botones **Actualizar / Eliminar** en `home/index.html` son solo visuales, no tienen vista ni URL asociada.

## Stack

- Python 3.11
- Django 4.2.5 (según `home/migrations/0001_initial.py`)
- SQLite (`db.sqlite3` incluido)
- Bootstrap 5.3 (vendorizado en `static/css` + `static/js`)
- Templates Django (`templates/base/base.html` como layout)

## Estructura

```
.
├── manage.py
├── proyecto1/          # settings, urls, wsgi, asgi
│   ├── settings.py
│   └── urls.py
├── home/               # app principal
│   ├── models.py       # 10 modelos: Persona, Producto, Orden, Mascota, Libro...
│   ├── views.py        # index(), index2(), index3(), index4()
│   ├── admin.py        # los 10 modelos registrados en admin
│   └── migrations/
├── templates/
│   ├── base/base.html  # navbar + accordion + {% block content %}
│   └── home/index.html # tabla de mascotas {% for c in mascotas %}
├── static/             # Bootstrap local
└── db.sqlite3
```

## Modelos (`home/models.py`)

| Modelo | Campos principales |
|---|---|
| `Mascota` | nombre, especie, raza (es el único usado en vistas) |
| `Persona` | nombre, apellido, fecha_nacimiento |
| `Producto` | nombre, descripcion, precio |
| `Orden` | fecha, total, productos M2M |
| `Libro` | titulo, autor, editorial, paginas |
| `Pelicula` | titulo, director, actores M2M → Persona, fecha_estreno |
| `Articulo` | titulo, autor, contenido, fecha_publicacion |
| `Comentario` | autor FK → Persona, contenido, fecha |
| `Calificacion` | valor, comentario, autor FK → Persona, objeto FK → Producto |
| `Imagen` | nombre, url |

## Cómo correrlo

```bash
# 1. Clonar
git clone https://github.com/nahataen/Django-Mascotas.git
cd Django-Mascotas

# 2. Entorno virtual
python -m venv venv
# Windows:
venv\Scripts\activate
# Linux/Mac:
# source venv/bin/activate

# 3. Dependencias
pip install "Django==4.2.*"

# 4. Migraciones (ya existe db.sqlite3, pero para entorno limpio)
python manage.py migrate

# 5. (Opcional) usuario admin
python manage.py createsuperuser

# 6. Servidor
python manage.py runserver
```

Abre: `http://127.0.0.1:8000/` y `http://127.0.0.1:8000/admin/`

## Rutas (`proyecto1/urls.py`)

```python
path('', views.index, name="index_view")       # lista mascotas
path('acerca', views.index2, name="index_view2")
path('base/', views.index4, name='base_view')
path('admin/', admin.site.urls)
```

## Limitaciones conocidas

1. Sin `update` / `delete` — solo lectura.
2. `views.index3` apunta a `"home/index3.hmtl"` (typo, debería ser `.html`) y `index3.html` está vacío.
3. `detail_category.html` tiene `{% extebds %}` (typo de `extends`) y bloque sin cerrar.
4. `SECRET_KEY` y `DEBUG=True` hardcodeados en `settings.py` — no apto para producción.
5. Bootstrap y `staticfiles/` del admin están commiteados — idealmente se generan con `collectstatic`.
6. `db.sqlite3` commiteado — en proyectos reales va en `.gitignore`.

## Roadmap sugerido

- [ ] Implementar `update` / `delete` de `Mascota` (forms + `UpdateView` / `DeleteView`)
- [ ] Corregir typos en `index3` y `detail_category.html`
- [ ] Agregar `requirements.txt` + `.gitignore` (venv, db.sqlite3, __pycache__)
- [ ] Mover `SECRET_KEY` a variable de entorno
- [ ] Tests básicos en `home/tests.py`

## Nota

Repo de práctica / histórico. Se conserva como referencia de aprendizaje de Django MVT.
