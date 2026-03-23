# Management

## Promtp para crear el sistema
Rol y Objetivo:
Actúa como un desarrollador Full-Stack experto. Vamos a crear la primera versión (MVP) de un Sistema de Gestión de Empresas utilizando Supabase como nuestro Backend as a Service (BaaS).

Stack Tecnológico y Librerías Requeridas:

Frontend: React.js (con Vite)

Lenguaje: TypeScript (con tipado estricto)

Estilos: Tailwind CSS

Backend / Auth: @supabase/supabase-js

Formularios: react-hook-form

Fechas: date-fns

Enrutamiento: react-router-dom

Iconos: lucide-react

Requerimientos de Arquitectura e Interfaz:

1. Configuración de Supabase y Rutas Protegidas:

Crea un archivo de configuración para inicializar el cliente de Supabase usando variables de entorno (VITE_SUPABASE_URL y VITE_SUPABASE_ANON_KEY).

Implementa un Contexto de Autenticación (AuthContext) que escuche los cambios de sesión de Supabase.

Configura React Router con un componente de "Ruta Protegida" (ProtectedRoute) para asegurar que solo los usuarios logeados puedan ver el dashboard.

2. Sistema de Autenticación (Login):

Crea una pantalla de login moderna.

Usa react-hook-form para manejar un formulario de inicio de sesión con Email y Contraseña, conectándolo con supabase.auth.signInWithPassword.

Incluye un botón destacado de "Iniciar sesión con Google" que ejecute supabase.auth.signInWithOAuth({ provider: 'google' }).

3. Pantalla Principal (Home / Dashboard):

Diseño en Grid (cuadrícula) responsivo con Tailwind.

Muestra un mensaje de bienvenida con el email o nombre del usuario logeado obtenido de Supabase.

Crea tarjetas (cards) clickeables para los módulos de "Calendario" y "Actividades Pendientes" que naveguen a sus respectivas rutas.

4. Módulo de Calendario:

Crea una vista mensual simple usando la librería date-fns para manejar la lógica de los días y meses.

Conecta este módulo a una tabla events en Supabase.

Permite agregar eventos con título, fecha de inicio y fecha de fin.

5. Módulo de Actividades Pendientes (Gestor de Tareas):

Crea un tablero de tareas conectado a una tabla tasks en Supabase.

Usa react-hook-form para el formulario de creación de nuevas tareas.

Campos de la tarea: Título, Descripción, Estado (Pendiente, En progreso, Completada) y user_id (para saber quién la creó).

Crea funciones CRUD (Crear, Leer, Actualizar estado, Eliminar) interactuando directamente con Supabase.



Estructura del Proyecto:
Organiza el código en: /src/components, /src/pages, /src/context, /src/lib (para la config de supabase) y /src/types (para las interfaces de TypeScript de Supabase).


## SQL 

Además de crear las tablas, este código configura algo muy importante llamado RLS (Row Level Security o Seguridad a Nivel de Filas). Esto asegura que, por defecto, un usuario solo pueda ver, editar y borrar sus propias tareas y eventos, manteniendo los datos seguros.

Instrucciones:
Entra a tu proyecto en Supabase.

En el menú de la izquierda, haz clic en "SQL Editor" (el ícono de la terminal o símbolo de mayor que >_).

Haz clic en "New Query" (Nueva consulta).

Copia y pega el siguiente código completo y dale al botón "Run" (o presiona Cmd/Ctrl + Enter).

Código SQL a ejecutar:

-- 1. Crear tabla de Tareas (Tasks)
CREATE TABLE tasks (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    title TEXT NOT NULL,
    description TEXT,
    status TEXT DEFAULT 'Pendiente' CHECK (status IN ('Pendiente', 'En progreso', 'Completada')),
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- 2. Crear tabla de Eventos del Calendario (Events)
CREATE TABLE events (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    title TEXT NOT NULL,
    start_date TIMESTAMP WITH TIME ZONE NOT NULL,
    end_date TIMESTAMP WITH TIME ZONE NOT NULL,
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- 3. Activar la Seguridad a Nivel de Filas (RLS) en ambas tablas
ALTER TABLE tasks ENABLE ROW LEVEL SECURITY;
ALTER TABLE events ENABLE ROW LEVEL SECURITY;

-- 4. Crear Políticas (Policies) para la tabla 'tasks'
-- Esto permite que los usuarios solo interactúen con sus propias tareas
CREATE POLICY "Usuarios pueden ver sus propias tareas" ON tasks FOR SELECT USING (auth.uid() = user_id);
CREATE POLICY "Usuarios pueden crear sus propias tareas" ON tasks FOR INSERT WITH CHECK (auth.uid() = user_id);
CREATE POLICY "Usuarios pueden actualizar sus propias tareas" ON tasks FOR UPDATE USING (auth.uid() = user_id);
CREATE POLICY "Usuarios pueden eliminar sus propias tareas" ON tasks FOR DELETE USING (auth.uid() = user_id);

-- 5. Crear Políticas (Policies) para la tabla 'events'
-- Esto permite que los usuarios solo interactúen con sus propios eventos
CREATE POLICY "Usuarios pueden ver sus propios eventos" ON events FOR SELECT USING (auth.uid() = user_id);
CREATE POLICY "Usuarios pueden crear sus propios eventos" ON events FOR INSERT WITH CHECK (auth.uid() = user_id);
CREATE POLICY "Usuarios pueden actualizar sus propios eventos" ON events FOR UPDATE USING (auth.uid() = user_id);
CREATE POLICY "Usuarios pueden eliminar sus propios eventos" ON events FOR DELETE USING (auth.uid() = user_id);

¿Qué hace exactamente este código?
REFERENCES auth.users(id): Conecta mágicamente cada tarea y evento con el sistema de usuarios integrado de Supabase. Si un usuario se elimina, sus tareas se borran en cascada (ON DELETE CASCADE).

auth.uid() = user_id: Es la regla de oro. Significa que "el ID del usuario que está intentando leer/escribir debe coincidir con el ID guardado en la fila". Si no coincide, Supabase bloquea la acción directamente en la base de datos, sin importar lo que haga el Frontend.

Con tu prompt en Replit y estas tablas en Supabase, ya tienes todo el ecosistema listo para arrancar con el MVP.

## 1. Crear la tabla de Roles
Primero, creamos una tabla que relacione a cada usuario con su rol. Ejecuta esto en tu editor SQL: 

-- Crear tabla de roles de usuario
CREATE TABLE user_roles (
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE PRIMARY KEY,
    role TEXT NOT NULL DEFAULT 'empleado' CHECK (role IN ('admin', 'empleado'))
);

-- Activar RLS en la tabla de roles
ALTER TABLE user_roles ENABLE ROW LEVEL SECURITY;

-- Los usuarios pueden leer su propio rol, pero solo los admins podrían editarlo
CREATE POLICY "Usuarios pueden ver su propio rol" ON user_roles FOR SELECT USING (auth.uid() = user_id);


## 2. Actualizar las políticas de las Tareas (Tasks)
Ahora le decimos a Postgres: "Un usuario normal solo puede ver sus tareas, pero un Admin puede ver las de todos".

-- Política: Los Admins pueden ver TODAS las tareas
CREATE POLICY "Admins pueden ver todas las tareas" ON tasks FOR SELECT 
USING (
    EXISTS (
        SELECT 1 FROM user_roles 
        WHERE user_roles.user_id = auth.uid() AND user_roles.role = 'admin'
    )
);

-- Política: Los Admins pueden editar TODAS las tareas (para reasignarlas, cambiar estado, etc.)
CREATE POLICY "Admins pueden editar todas las tareas" ON tasks FOR UPDATE 
USING (
    EXISTS (
        SELECT 1 FROM user_roles 
        WHERE user_roles.user_id = auth.uid() AND user_roles.role = 'admin'
    )
);

