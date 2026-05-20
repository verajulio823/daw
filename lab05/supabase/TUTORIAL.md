
# Actividades Prácticas

En esta sección realizarás actividades prácticas para dominar Supabase. Trabajarás con una base de datos de relación entre usuarios y productos.

---

## Actividad 1: Crear Base de Datos desde Script

### Objetivo
Crear las tablas necesarias ejecutando un script SQL que define la estructura de la base de datos con relaciones entre tablas.

### Pasos

#### Paso 1: Ir al SQL Editor
- En el dashboard de tu proyecto Supabase, busca **"SQL Editor"** en el menú lateral izquierdo
- Se abrirá el editor de consultas SQL

#### Paso 2: Crear una nueva consulta
- Haz clic en el botón **"+ New Query"** (Nueva Consulta)
- Se abrirá un nuevo editor vacío

#### Paso 3: Copiar y pegar el script
Copia el siguiente script SQL que crea las tablas con relaciones:

```sql
-- =========================================
-- TABLA: usuarios
-- =========================================

create table usuarios (
    id uuid primary key default gen_random_uuid(),
    nombre text not null,
    correo text unique not null,
    created_at timestamp with time zone default now()
);

-- =========================================
-- TABLA: productos
-- Un usuario puede tener muchos productos
-- =========================================

create table productos (
    id uuid primary key default gen_random_uuid(),
    nombre text not null,
    descripcion text,
    precio numeric(10,2) not null,

    -- Relación con usuarios
    usuario_id uuid not null references usuarios(id)
        on delete cascade,

    created_at timestamp with time zone default now()
);
```

#### Paso 4: Ejecutar el script
- Haz clic en el botón **"Run"** (Ejecutar) o presiona `Ctrl + Enter`
- Si no hay errores, verás el mensaje **"Success"**
- Las tablas se habrán creado exitosamente

#### Paso 5: Verificar las tablas
- Ve a la sección **"Tables"** en el menú lateral
- Deberías ver las dos nuevas tablas: `usuarios` y `productos`
- Haz clic en cada tabla para verificar que las columnas se crearon correctamente

---

## Actividad 2: Cargar Datos de Prueba

### Objetivo
Insertar datos de prueba en las tablas creadas para practicar con información realista.

### Pasos

#### Paso 1: Crear una nueva consulta
- En el SQL Editor, haz clic en **"+ New Query"**
- Abre un nuevo editor para esta actividad

#### Paso 2: Insertar usuarios
Copia y pega el siguiente script para crear usuarios:

```sql
-- Crear usuarios
insert into usuarios (nombre, correo)
values
('Julio', 'julio@gmail.com'),
('Maria', 'maria@gmail.com');
```

#### Paso 3: Ejecutar la consulta
- Haz clic en **"Run"** o presiona `Ctrl + Enter`
- Verás el mensaje **"Success"** con el número de filas insertadas

#### Paso 4: Insertar productos
En una nueva consulta, copia y pega el siguiente script:

```sql
-- Crear productos
insert into productos (nombre, descripcion, precio, usuario_id)
values
(
    'Laptop Gamer',
    'RTX 4060',
    4500.00,
    (select id from usuarios where correo = 'julio@gmail.com')
),
(
    'Mouse Logitech',
    'Mouse inalámbrico',
    120.50,
    (select id from usuarios where correo = 'julio@gmail.com')
),
(
    'iPhone',
    'iPhone 15',
    5200.00,
    (select id from usuarios where correo = 'maria@gmail.com')
);
```

#### Paso 5: Ejecutar la consulta
- Haz clic en **"Run"** o presiona `Ctrl + Enter`
- Verás el mensaje **"Success"** confirmando que se insertaron 3 productos

#### Paso 6: Verificar los datos
- Ve a la sección de **"Tables"** 
- Haz clic en la tabla `usuarios` para ver los registros insertados
- Haz clic en la tabla `productos` para ver los productos

---

## Actividad 3: Realizar Consultas de Relación

### Objetivo
Escribir consultas SQL que unan datos de las dos tablas usando relaciones (JOIN).

### Pasos

#### Paso 1: Crear una nueva consulta
- En el SQL Editor, haz clic en **"+ New Query"**

#### Paso 2: Realizar un JOIN
Copia y pega la siguiente consulta que relaciona usuarios y productos:

```sql
-- =========================================
-- CONSULTA DE RELACIÓN
-- =========================================

select
    u.nombre as usuario,
    p.nombre as producto,
    p.precio
from productos p
join usuarios u
on p.usuario_id = u.id;
```

#### Paso 3: Ejecutar la consulta
- Haz clic en **"Run"** o presiona `Ctrl + Enter`
- Verás una tabla con los resultados mostrando:
  - **usuario**: Nombre del usuario propietario
  - **producto**: Nombre del producto
  - **precio**: Precio del producto

**Resultado esperado:**

| usuario | producto | precio |
|---------|----------|--------|
| Julio | Laptop Gamer | 4500.00 |
| Julio | Mouse Logitech | 120.50 |
| Maria | iPhone | 5200.00 |

#### Paso 4: Realizar consultas adicionales
Practica escribiendo otras consultas:

**Consulta 2: Total gastado por usuario**
```sql
select
    u.nombre as usuario,
    count(p.id) as cantidad_productos,
    sum(p.precio) as total_gastado
from usuarios u
left join productos p
on u.id = p.usuario_id
group by u.id, u.nombre
order by total_gastado desc;
```

**Consulta 3: Productos por rango de precio**
```sql
select
    u.nombre,
    p.nombre,
    p.precio,
    case
        when p.precio < 500 then 'Económico'
        when p.precio >= 500 and p.precio < 2000 then 'Medio'
        else 'Premium'
    end as categoria
from productos p
join usuarios u
on p.usuario_id = u.id
order by p.precio desc;
```

---

## Actividad 4: Probar API con Postman

### Objetivo
Probar las APIs autogeneradas por Supabase usando Postman para acceder a los datos.

---

### Parte A: Obtener las Credenciales API

#### Paso 1: Acceder a Configuración
- En el dashboard de tu proyecto, haz clic en el ícono de **engranaje** ⚙️
- Busca **"API"** o **"Settings"** en el menú lateral

#### Paso 2: Obtener la URL del Proyecto
- En la sección **"Project URL"**, encontrarás tu URL de Supabase
- Cópiala (Ej: `https://ezzmxvqsuhosmuixlfrz.supabase.co`)

#### Paso 3: Obtener la API Key
- Busca la sección **"API Keys"** o **"Project Keys"**
- Verás dos tipos de claves:
  - **`anon` (Anónima)**: Para acceso público con RLS
  - **`service_role`**: Para acceso administrativo (úsala solo en servidor)
- Copia la clave **`anon`** (es la segura para públicamente)

#### Paso 4: Guardar las credenciales
Ten a mano:
- **URL Base**: `https://tu-proyecto.supabase.co`
- **API Key**: Tu clave anon
- **API URL Base**: `https://tu-proyecto.supabase.co/rest/v1`

---

### Parte B: Configurar Postman

#### Paso 1: Descargar Postman
- Ve a [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
- Descarga e instala Postman
- Abre la aplicación

#### Paso 2: Crear una nueva colección
- Haz clic en **"Collections"** en el menú lateral
- Haz clic en **"+ Create Collection"**
- Nombra la colección: `Supabase API`
- Haz clic en **"Create"**

#### Paso 3: Crear variables de entorno
- Haz clic en el ícono de **engranaje** (Settings) en la esquina superior derecha
- Ve a **"Environments"**
- Haz clic en **"+ Create Environment"**
- Nombre: `Supabase Local`
- Agrega las siguientes variables:

| Variable | Value |
|----------|-------|
| `base_url` | https://ezzmxvqsuhosmuixlfrz.supabase.co/rest/v1 |
| `api_key` | Tu clave API anon (pegada aquí) |

- Haz clic en **"Save"**

#### Paso 4: Seleccionar el entorno
- En la esquina superior derecha de Postman, selecciona tu entorno `Supabase Local`

---

### Parte C: Realizar Peticiones HTTP

#### Petición 1: Obtener todos los usuarios (GET)

**Paso 1: Crear una nueva petición**
- En tu colección `Supabase API`, haz clic en **"+ Add Request"**
- Nombre: `Get Usuarios`
- Método: **GET**

**Paso 2: Ingresar URL**
```
{{base_url}}/usuarios
```

**Paso 3: Agregar Headers**
En la pestaña **"Headers"**, agrega:

| Key | Value |
|-----|-------|
| `apikey` | `{{api_key}}` |
| `Authorization` | `Bearer {{api_key}}` |
| `Content-Type` | `application/json` |

**Paso 4: Enviar petición**
- Haz clic en **"Send"**
- Deberías recibir una respuesta JSON con todos los usuarios:
```json
[
  {
    "id": "uuid-1",
    "nombre": "Julio",
    "correo": "julio@gmail.com",
    "created_at": "2026-05-19T..."
  },
  {
    "id": "uuid-2",
    "nombre": "Maria",
    "correo": "maria@gmail.com",
    "created_at": "2026-05-19T..."
  }
]
```

---

#### Petición 2: Obtener todos los productos (GET)

**Paso 1: Crear una nueva petición**
- Nombre: `Get Productos`
- Método: **GET**
- URL: `{{base_url}}/productos`

**Paso 2: Agregar Headers**
Usa los mismos headers que en la petición anterior

**Paso 3: Enviar petición**
- Haz clic en **"Send"**
- Recibirás todos los productos con sus relaciones

---

#### Petición 3: Obtener usuarios con sus productos (SELECT Expandido)

**Paso 1: Crear una nueva petición**
- Nombre: `Get Usuarios with Productos`
- Método: **GET**

**Paso 2: Ingresar URL con expand**
```
{{base_url}}/usuarios?select=*,productos(*)
```

**Paso 3: Agregar Headers**
Usa los mismos headers

**Paso 4: Enviar petición**
- Recibirás usuarios con un array de productos relacionados

---

#### Petición 4: Crear un nuevo usuario (POST)

**Paso 1: Crear una nueva petición**
- Nombre: `Create Usuario`
- Método: **POST**
- URL: `{{base_url}}/usuarios`

**Paso 2: Agregar Headers**
| Key | Value |
|-----|-------|
| `apikey` | `{{api_key}}` |
| `Authorization` | `Bearer {{api_key}}` |
| `Content-Type` | `application/json` |
| `Prefer` | `return=representation` |

**Paso 3: Agregar cuerpo (Body)**
- Ve a la pestaña **"Body"**
- Selecciona **"raw"** y **"JSON"**
- Pega:
```json
{
  "nombre": "Carlos",
  "correo": "carlos@gmail.com"
}
```

**Paso 4: Enviar petición**
- Haz clic en **"Send"**
- Recibirás el nuevo usuario creado con su ID

---

#### Petición 5: Actualizar un usuario (PATCH)

**Paso 1: Crear una nueva petición**
- Nombre: `Update Usuario`
- Método: **PATCH**
- URL: `{{base_url}}/usuarios?id=eq.UUID-DEL-USUARIO`
  - Reemplaza `UUID-DEL-USUARIO` con el ID de Julio

**Paso 2: Agregar Headers**
Usa los mismos headers que en POST

**Paso 3: Agregar Body**
```json
{
  "nombre": "Julio Eduardo"
}
```

**Paso 4: Enviar petición**
- El usuario será actualizado

---

#### Petición 6: Filtrar datos (GET con filtro)

**Paso 1: Crear una nueva petición**
- Nombre: `Filter Productos por Precio`
- Método: **GET**

**Paso 2: Ingresar URL con filtro**
```
{{base_url}}/productos?precio=gt.500&select=*,usuarios(nombre)
```
*Esto obtiene productos con precio mayor a 500*

**Paso 3: Agregar Headers**
Usa los headers estándar

**Paso 4: Enviar petición**

---

### Operadores de Filtro Útiles

| Operador | Ejemplo | Significado |
|----------|---------|------------|
| `eq` | `precio=eq.4500` | Igual a |
| `neq` | `precio=neq.4500` | No igual a |
| `gt` | `precio=gt.1000` | Mayor que |
| `gte` | `precio=gte.1000` | Mayor o igual que |
| `lt` | `precio=lt.1000` | Menor que |
| `lte` | `precio=lte.1000` | Menor o igual que |
| `like` | `nombre=like.%Laptop%` | Contiene |
| `in` | `id=in.(val1,val2)` | En lista |

---

### Parámetros de Consulta Útiles

| Parámetro | Ejemplo | Resultado |
|-----------|---------|-----------|
| `select` | `?select=nombre,precio` | Obtiene solo esas columnas |
| `expand` | `?select=*,usuarios(*)` | Incluye datos relacionados |
| `order` | `?order=precio.desc` | Ordena por precio descendente |
| `limit` | `?limit=10` | Obtiene máximo 10 registros |
| `offset` | `?offset=5&limit=10` | Paginación |

---

### Checklist de Actividades

Marca las actividades completadas:

- [ ] ✅ Crear tablas desde script SQL
- [ ] ✅ Insertar usuarios de prueba
- [ ] ✅ Insertar productos de prueba
- [ ] ✅ Ejecutar JOIN entre tablas
- [ ] ✅ Realizar consultas avanzadas
- [ ] ✅ Obtener credenciales API
- [ ] ✅ Configurar Postman
- [ ] ✅ GET todos los usuarios
- [ ] ✅ GET todos los productos
- [ ] ✅ GET usuarios con productos expandido
- [ ] ✅ POST crear nuevo usuario
- [ ] ✅ PATCH actualizar usuario
- [ ] ✅ GET con filtros

---

## Consejos Importantes

✅ **Recomendaciones:**
- Usa contraseñas fuertes para tu base de datos
- Haz copias de seguridad regularmente
- Establece políticas de Row Level Security (RLS) para proteger datos
- Revisa los miembros del proyecto periódicamente
- Usa variables de entorno para guardar credenciales sensibles

⚠️ **Advertencias:**
- No compartas tu contraseña de base de datos con nadie
- Ten cuidado al compartir acceso de Owner
- Revisa los logs de auditoría para cambios importantes
- Considera habilitar 2FA en tu cuenta Supabase
- Nunca expongas tu API Key en código público (repositorios)

---

## Enlaces Útiles
- [Documentación oficial de Supabase](https://supabase.com/docs)
- [Panel de Control de Supabase](https://app.supabase.com)
- [Comunidad de Supabase](https://discord.gg/supabase)
- [Postman Documentación](https://learning.postman.com/)
- [REST API de Supabase](https://supabase.com/docs/guides/api)

---

**Última actualización:** Mayo 2026
