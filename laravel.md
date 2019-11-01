# LARAVEL

## ESTRUCTURA DE CARPETAS

```
/
├── app
|   |─── http
│   |     └─── controllers
│   |              |─── controller1.php
│   |              └─── controller2.php...
│   |─── modelo1.php
│   └─── modelo2.php---
├── resources
│      └── views
│            |─── view1.blade.php
│            └─── view2.blade.php...
├── routes 
.     └── web.php (controlador primario)
.
.
```

## CONTROLADOR PRINCIPAL: WEB.PHP

Este archivo es el encargado de gestionar todas las redirecciones de nuestra página.

    Route:get('ruta-para-el-usuario', 'Controlador@funcion')->name('nombre-para-la-ruta');    

Esto es una ruta a través del método GET. Todas las rutas tienen 3 parámetros:
1. La ruta que verá el usuario en su navegador.
2. El controlador y la función que queremos ejecutar.
3. El nombre. Este parámetro sirve para hacer nuestras redirecciones en la página. De esta forma, sí en elgún momento queremos cambiar la ruta que ve el usuario o el nombre de la función, no tendremos que tocar nuestro código.

También encontramos rutas por los siguientes métodos:

#### POST

Este enrutamiento es obligatorio para los formularios.

    Route:post('ruta-para-el-usuario', 'Controlador@funcion')->name('nombre-para-la-ruta');    

#### PUT o PATCH

Estos métodos sirven para la recepción de datos para la actualización de datos. Laravel reconoce los dos ya que aún no están implementados en HTML y todavía no se ha dedicido como se va a llamar este nuevo método.
Al no estar implementados en HTML, blade nos ofrece una forma de especificar que queremos que los datos vayan a través de este método:

```
<form action="{{route('user.update')}}" method="POST">
@csrf
@method('PUT')
.
.
.
</form>
```

###### \* '@csrf' nos crea una protección en nuestro formulario contra inyección de código. Si no lo ponemos, nos dará un error al enviarlo.

### RESTFULL

Se dice que un servidor es RESTfull cuando cuenta con las 7 operaciones principales: [index](#index), [store](#store), [create](#create), [show](#show), [update](#update), [destroy](#destroy) y [edit](#edit).

```
Route::get('usuario', 'UsuarioController@index')->name('user.index');
Route::post('usuario', 'UsuarioController@store')->name('user.store');
Route::get('usuario/crear', 'UsuarioController@create')->name('user.create');
Route::get('usuario/{id}', 'UsuarioController@show')->name('user.show');
Route::patch('usuario/{id}', 'UsuarioController@update')->name('user.update');
Route::delete('usuario/{id}', 'UsuarioController@destroy')->name('user.destroy');
Route::get('usuario/{id}/editar', 'UsuarioController@edit')->name('user.edit');
```

#### DELETE

Se supone que es para cuando se envian datos que se van a eliminar de la base de datos pero... si no hay formulario no entiendo para que necesitamos este método...

## CONTROLADORES

Lo más fácil para crear un controlador es generarlo automáticamente. Para ello tenemos que ejecutar el siguiente comando:

    php artisan make:controller <controller-name>

Un controlador tendrá (como mínimo) las funciones de las operaciones principales, es decir, 7 funciones.

#### INDEX

Este método pedirá los datos necesarios para la página principal y la mostrará.

#### STORE

Con store gestionamos añadir usuario. Recogeremos los datos que nos lleguen y haremos una inserción con ellos en nuestra base de datos.

```
public function store(Request $r) {
    $user = new myUser();
    $user->username = $r->get("username");
    .
    .
    .
    $user->save();
    $data["mensaje"] = "Usuario añadido con éxito";
    return view('ruta', $data);
}
```

#### CREATE

Con este método mostraremos la vista del formulario que necesitemos. Gestionamos los datos con [store](#store)

```
public function create() {
    return view('viewName');
}
```

#### SHOW

Mira, a mi que me la explique Alfredo por favor porque yo no la he usado para nada. Pero vamos, imagino que será para que un usuario vea su propio perfil.

#### EDIT

Con este método recogemos los datos del id que nos llega por GET y mostramos un formulario para modificarlos.

```
public function edit(Request $r) {
    $data["xxxxx"] = Class::find($r->id);
    return view('viewName', $data);
}
```

#### UPDATE

Usamos update para gestionar la actualización de los datos.

```
public function update(Request $r) {
    $user = myUser::find($r->id);
    $user->username = $r->username;
    .
    .
    .
    $user->save();
    $data["user"] = myUser::find($r->id);
    $data["mensaje"] = "Usuario editado con éxito";
    return view('viewName', $data);
}
```

#### DESTROY

Con destroy eliminamos el registro que hayamos seleccionado.

```
public function destroy(Request $r) {
    $user = myUser::find($r->id);
    $user->delete();
    .
    .
    .
    return redirect()->action('UserController@index', $data);
}
```

Una vez que eliminemos el registro es posible que tengamos que hacer una redirección para que los datos se regarguen. Esto se hace con `redirect()->action('Controller@function', $data);`