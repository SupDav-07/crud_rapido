# Laboratorio #2: CRUD rápido en Laravel

📅 Fecha: [23/11/2026]

## 📄 Contenido del Repositorio

Este repositorio contiene el proyecto `crud_rapido`, desarrollado con el framework Laravel, correspondiente al **Laboratorio #2: Acercamiento a un CRUD rápido en Laravel** del curso de Desarrollo Web. Incluye la creación de una tabla de **productos** mediante migraciones, y la generación automática de un **CRUD completo** (crear, leer, actualizar y eliminar) con el paquete `ibex/crud-generator`, con vistas en Bootstrap.

## 🛠️ Tecnologías Utilizadas

- **Lenguaje:** PHP 8.3.28
- **Framework:** Laravel 12.x
- **Generador de CRUD:** `ibex/crud-generator` (instalado como dependencia de desarrollo)
- **Front-end:** Bootstrap 5 mediante `laravel/ui`, plantillas Blade
- **Compilador de assets:** Vite (Node.js y npm)
- **Gestor de dependencias:** Composer
- **Servidor local:** WampServer 3.4.0 (Apache 2.4.65)
- **Base de datos:** MySQL (incluido en WampServer), administrada con phpMyAdmin
- **Editor:** Visual Studio Code
- **Control de versiones:** Git / GitHub

## 🖥️ Capturas de Pantalla y Problemas

### Interfaz Principal

- **Creación del proyecto (`composer create-project laravel/laravel:^12.0 crud_rapido`):** se crea el proyecto dentro de `C:\wamp64\www\`.
- <!-- Captura de la creación del proyecto -->

- **Configuración del `.env` y migraciones (`php artisan migrate`):** se configuran las credenciales de la base de datos `crud_rapido` y se ejecutan las migraciones iniciales.
- <!-- Captura de php artisan migrate exitoso -->

- **Modelo y migración de productos (`php artisan make:model Product -m`):** se crea el modelo `Product` y la migración de la tabla `products` con los campos `description`, `price` y `stock`.
- <!-- Captura de la migración de products -->

- **Instalación del generador (`composer require ibex/crud-generator --dev`):** se instala el paquete y se publica su configuración con `php artisan vendor:publish --tag=crud`.
- <!-- Captura de la instalación del paquete -->

- **Generación del CRUD (`php artisan make:crud products bootstrap`):** se generan el controlador, el modelo y las vistas de productos.
- <!-- Captura de make:crud -->

- **Rutas del CRUD (`Route::resource('products', ProductController::class)`):** define las siete rutas del recurso `products` (index, create, store, show, edit, update y destroy).
- <!-- Captura de php artisan route:list -->

- **Listado de productos (`/products`):** vista principal con la tabla de productos y el botón **Create New**.
- <!-- Captura del listado -->

- **Crear un producto (`/products/create`):** formulario para registrar descripción, precio y stock.
- <!-- Captura del formulario de crear -->

- **Editar un producto (`/products/{id}/edit`):** formulario para modificar un producto existente.
- <!-- Captura del formulario de editar -->

- **Eliminar un producto:** botón de eliminación en cada fila del listado.
- <!-- Captura del listado después de eliminar -->

- **Tabla `products` en phpMyAdmin:** registros guardados en la base de datos `crud_rapido`.
- <!-- Captura de phpMyAdmin -->

### Problemas encontrados y soluciones

- **Versión de PHP incorrecta en la terminal:** la terminal usaba PHP 8.0.30 y Laravel 12 requiere PHP 8.2 o superior. Se solucionó configurando el `PATH` de la terminal integrada de VS Code (`terminal.integrated.env.windows`) con `C:\wamp64\bin\php\php8.3.28`.
- **Generador incompatible con una versión antigua de Laravel:** en un primer proyecto creado con una versión vieja de Laravel, Composer instaló la versión 1.x del generador y `make:crud` falló con el error `Call to undefined method ...::getColumns()`. Se solucionó creando el proyecto de nuevo con Laravel 12.
- **Error de longitud de cadena en las migraciones:** se agregó `Schema::defaultStringLength(191);` en el método `boot()` de `app/Providers/AppServiceProvider.php`.
- **Tabla `products` ya existente al migrar:** se corrigió la migración para que definiera la tabla una sola vez y se ejecutó `php artisan migrate:fresh` para recrear las tablas.
- **Error `View [layouts.app] not found`:** las vistas generadas usan la plantilla `layouts.app`, que crea el paquete `laravel/ui`. Se solucionó con `composer require laravel/ui`, `php artisan ui bootstrap --auth` y `npm install`.
- **Error `The [pcntl] extension is required to run Pail`:** al ejecutar `composer run dev` en Windows, el servicio de logs (`pail`) no puede iniciar porque requiere una extensión que no existe en Windows. No afecta al CRUD, ya que el servidor, la cola y Vite siguen funcionando.

## 📁 Estructura de Carpetas o Directorios

```
crud_rapido/
├── app/
│   ├── Http/
│   │   └── Controllers/
│   │       ├── Auth/                       # Controladores de autenticación (laravel/ui)
│   │       └── ProductController.php       # Controlador del CRUD (generado)
│   ├── Models/
│   │   └── Product.php                     # Modelo Product con $fillable (generado)
│   └── Providers/
│       └── AppServiceProvider.php          # Schema::defaultStringLength(191)
├── config/
│   └── crud.php                            # Configuración publicada del generador
├── database/
│   └── migrations/
│       └── ..._create_products_table.php   # Migración: description, price, stock
├── resources/
│   └── views/
│       ├── layouts/
│       │   └── app.blade.php               # Plantilla principal (laravel/ui)
│       └── product/                        # Vistas del CRUD (index, create, edit, show)
├── routes/
│   └── web.php                             # Route::resource('products', ...)
├── .env.example                            # Plantilla de variables de entorno
├── composer.json                           # Dependencias de PHP
├── package.json                            # Dependencias de Node.js
└── README.md                               # Documentación del proyecto
```

## ▶️ Instrucciones de Ejecución / Uso

1. Clonar el repositorio.
2. Copiar la carpeta del proyecto dentro de `C:\wamp64\www\` (o la carpeta `htdocs` de tu servidor local).
3. Iniciar WampServer y verificar que Apache y MySQL estén activos (ícono en verde). Comprobar con `php -v` que la terminal use PHP 8.2 o superior.
4. Abrir una terminal dentro de la carpeta del proyecto e instalar las dependencias:
   ```bash
   composer install
   npm install
   ```
5. Crear el archivo de variables de entorno y generar la clave de la aplicación:
   ```bash
   copy .env.example .env
   php artisan key:generate
   ```
6. Editar el archivo `.env` con los datos de la base de datos:
   ```env
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=crud_rapido
   DB_USERNAME=root
   DB_PASSWORD=
   ```
7. Limpiar la caché de configuración y ejecutar las migraciones:
   ```bash
   php artisan config:clear
   php artisan migrate
   ```
8. Levantar el servidor de desarrollo:
   ```bash
   composer run dev
   ```
9. Abrir el navegador y acceder a:
   - `http://127.0.0.1:8000/products` (listado de productos)
   - `http://127.0.0.1:8000/products/create` (crear un producto)

> **Notas:**
> - El archivo `.env` y las carpetas `vendor` y `node_modules` no se suben al repositorio, por eso es necesario generarlos con los pasos anteriores.
> - Si aparece el error de `pail` al ejecutar `composer run dev`, se puede ignorar: no afecta al funcionamiento del CRUD.

## 👤 Autor y Contexto

- **Nombre:** René
- **Institución:** Universidad Tecnológica de Panamá (UTP)
- **Curso:** Desarrollo Web
- **Docente:** Ing. Irina Fong
- **Fecha de Realización:** [23/11/2026]

## 🔗 Referencias

- Presentación del laboratorio: "Acercamiento a un CRUD rápido en Laravel" (Ing. Irina Fong)
- Documentación oficial de Laravel: https://laravel.com/docs/12.x
- Generador de CRUD: https://github.com/awais-vteams/laravel-crud-generator
- Paquete `laravel/ui`: https://github.com/laravel/ui
