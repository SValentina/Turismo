# Primer examen parcial
Sistemas Distribuidos - Mayo 2021

## Pre-requisitos 
Es necesario crear una base de datos llamada `turismo`, o en su defecto modificar el archivo `.env` y agregar la carpeta del proyecto en `xampp/htdocs`.

## Instalación
Seguido de esto se deben migrar las 4 tablas (ciudadanos, domicilios, eventos, permisos) mediante el comando `php artisan migrate`.
Se crearon seeders para cada una de las tablas, por lo tanto ejecutar:
```
php artisan db:seed --class=CiudadanoSeeder
php artisan db:seed --class=DomicilioSeeder
php artisan db:seed --class=EventoSeeder
php artisan db:seed --class=PermisoSeeder
```

## Ejecutando 
Para ejecutar el proyecto sólo se debe correr el comando `php artisan serve`.

## Rutas
Para el proyecto se realizaron dos front-end, uno para el usuario navegante y otro para el usuario admin. 
Se puede acceder a estas vistas mediante la ruta `/`, donde nos permite dirigirnos a cualquiera de las dos opciones, o mediante `/form` para el usuario navegante y `/admin` para el usuario administrador. 

## Construcción 
### Front
Las vistas fueron creadas utilizando Bootstrap y CSS. 

### Back 
Por cada tabla creada se realizó un objeto de la misma, un recurso y un controlador (además de la migración y seeder correspondiente). Ejemplo con la tabla `eventos`:
```
Evento.php
EventoResource.php
EventoController.php
```
También se creó el  `HomeController`, el cual realiza el consumo de la API como se explica a continuación.

### Consumo de APIs
El consumo de las diferentes APIs se llevó a cabo de dos maneras.

#### 1. APIs provincias 

   Se consumió desde el controlador HomeController por medio de la librería Guzzle la cual facilita el consumo de servicios o API Rest con PHP.
   Para esto en el caso de Laravel 7 tuve que importar la librería, lo cual no me sucedió con Laravel 8.     
    `composer require guzzlehttp/guzzle`
    <br>
   La función del controlador que consume la API devuelve la vista `/form` para utilizar directamente los valores de las provincias en el select correspondiente.
   
 ```php
 public function index()
 {
     $response = Http::get('https://apis.datos.gob.ar/georef/api/provincias?orden=nombre&max=24');
     $provincias = $response->json();        

     return view('form', compact('provincias'));
 }
 ```

#### 2. API departamento y API localidades
  En cuanto a las demás APIs fueron consumidas mediante el uso de AJAX, debido a que tenía que conformar la URL con los ids de la provincia y el departamento seleccionado según correspondiese. Además, se realizó otra función AJAX que consulta a la API de acuerdo a la localidad seleccionada y crea dos input hidden con la latitud y longitud para luego ser cargados en el domicilio.

### Librería html2canvas
 Esta librería se utilizó para la formulación del comprobante del registro del permiso de turismo.   
 La función fue obtenida de 
 [Convertir HTML a imagen con html2canvas](https://parzibyte.me/blog/2019/07/11/html-a-imagen-html2canvas-screenshot-de-pagina-web/)
 
 --- 
 
 ###### Autor: Scovenna Valentina
