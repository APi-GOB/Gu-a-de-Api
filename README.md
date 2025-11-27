# Guía de uso para el API

## Índice

* [Cómo identificarse con el sistema](#cómo-identificarse-con-el-sistema)
    * [Cómo obtener un token](#1-cómo-obtener-un-token) [POST]
    * [Cómo usar el token](#2-cómo-usar-el-token)
* [Métodos de consulta](#métodos-de-consulta-disponibles)
    * [Buscar víctima por CURP](#buscar-víctima-por-curp) [POST]
    * [Datos de la víctima por hecho id](#datos-de-la-víctima-por-hecho-id) [GET]
    * [Datos de los agresores por hecho id](#datos-de-los-agresores-por-hecho-id) [GET]
    * [Seguimientos por folio EUV / id de Hecho de violencia](#seguimientos-por-folio-euv--id-de-hecho-de-violencia) [POST]
    * [Seguimientos por folio EUV](#seguimientos-por-folio-euv) [POST]
    * [Folios EUV por enlace estatal](#folios-euv-por-enlace-estatal) [POST]
    * [Folios EUV por estado](#folios-euv-por-estado) [POST]
    * [Hecho por id](#hecho-por-id) [POST]
    * [Hechos por entidad y rango de fecha](#hechos-de-violencia-por-entidad-y-rango-de-fecha) [POST]
    * [Orden de protección por id](#orden-de-protección-por-id)[POST]
    * [Órdenes de protección paginadas](#ordenes-de-protección-paginadas)[POST]
* [Métodos de registro disponibles](#métodos-de-registro-disponibles)
    * [Registro de víctima con CURP](#registro-de-víctima-con-curp) [POST]
    * [Registro de víctima con Datos](#registro-de-víctima-con-datos) [POST]
    * [Registro de víctima sin CURP](#registro-de-víctima-sin-curp) [POST]
    * [Registro de hecho de violencia](#registro-de-hecho-de-violencia) [POST]
    * [Registro de datos de la víctima por hecho id](#registroedición-de-datos-de-la-víctima-por-hecho-de-violencia)[POST]
    * [Registro de dependiente por hecho id](#registro-de-dependiente-por-hecho-de-violencia)[POST]
    * [Registro de red de apoyo por hecho id](#registro-de-red-de-apoyo-por-hecho-de-violencia)[POST]
    * [Registro de mujeres en prisión para un hecho de violencia](#registro-de-mujeres-en-prisión-para-un-hecho-de-violencia) [POST]
    * [Registro de mujeres víctimas de trata para un hecho de violencia](#registro-de-mujeres-víctimas-de-trata-para-un-hecho-de-violencia) [POST]
    * [Registro de mujeres desaparecidas para un hecho de violencia](#registro-de-mujeres-desaparecidas-para-un-hecho-de-violencia) [POST]
    * [Registro una canalización para un hecho de violencia](#registro-una-canalización-para-un-hecho-de-violencia) [POST]
    * [Registro de un agresor para un hecho de violencia](#registro-de-un-agresor-para-un-hecho-de-violencia) [POST]
    * [Registro de orden de protección](#registro-de-una-orden-de-protección)[POST]
* [Métodos de edición disponibles](#métodos-de-edición-disponibles)
    * [Editar hecho de violencia](#editar-hecho-de-violencia) [POST]
    * [Editar datos de la víctima por hecho id](#registroedición-de-datos-de-la-víctima-por-hecho-de-violencia)[POST]
    * [Editar registro de mujeres en prisión para un hecho de violencia](#editar-registro-de-mujeres-en-prisión-para-un-hecho-de-violencia) [POST]
    * [Editar registro de mujeres víctimas de trata](#editar-registro-de-mujeres-víctimas-de-trata) [POST]
    * [Editar orden de protección](#editar-orden-de-protección)
* [Método de consulta personalizados](#métodos-de-consulta-personalizados-disponibles)
    * [Infoname](#infoname) [POST]
* [Métodos de prueba disponibles](#métodos-de-prueba)
    * [Eliminar CURP mediante CURP](#eliminar-curp-mediante-curp) [POST]
    * [Eliminar CURP mediante EUV](#eliminar-curp-mediante-folio-euv) [POST]
    * [Restaurar CURP mediante EUV](#restaura-el-curp-mediante-folio-euv) [POST]
      
    *[Actualizar registro de víctima](#actualizar-registro-de-víctima). [PUT]
  
    *[Actualizar datos de la víctima](#actualizar-datos-de-la-víctima)[PUT]
  
    *[Actualizar dependiente de la víctima](#actualizar-datos-de-la-víctima)[PUT]
  
     *[Campos opcionales de captura](#campos-opcionales-de-captura)
      

## Cómo identificarse con el sistema

### 1. Cómo obtener un token
Para obtener un token, se debe hacer una petición a __/api/tokens/create__ enviando dos parámetros: email y password. Si las credenciales son correctas, se obtendrá un objeto json con la llave para realizar peticiones durante un día.

Con curl se puede realizar así:

```bash
curl --location 'https://localhost/api/tokens/create' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email" : "VALID_EMAIL",
    "password" : "VALID_PASSWORD"
}'
```

La respuesta que se recibe tiene este formato:

```json
{
    "token": "YOUR_ACCESS_TOKEN"
}
```

### PHP Rules

```php
$request->validate([
    'email'     => 'required|email|exists:users,email',
    'password'  => 'required'
]);
```


--------------------------------------------------------------------------------


### 2. Cómo usar el token
El tiempo de vida de un token es de un día (24 horas). El sistema usa el método _bearer token_ por lo que deberá llevar un header de tipo

```bash
header: Authorization: Bearer YOUR_ACCESS_TOKEN
```

## Métodos de consulta disponibles

### Buscar víctima por CURP

#### url

/api/curp

#### props

```json
{
    "curp" : "VALID_CURP"
}
```

#### curl

```bash
curl --location 'http://localhost/api/curp' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data '{
    "curp" : "VALID_CURP"
}'
```

#### respuesta

```json
{
    "victima": {
        "curp": "VALID_CURP",
        "folio_euv": "VALID_EUV"
    }
}
```

### PHP Rules

```php
$request->validate([
    'curp' => 'required'
]);
```

#### tipo de usuario

* cualquiera

--------------------------------------------------------------------------------

### Datos de la víctima por hecho id

#### url
/api/datos-victima-por-hecho/{hecho_id}

### PHP Rules

```php
// aquí ni validación hay O___O, hay que actualizar esto
if(!$hecho){
    throw ValidationException::withMessages([
        'hecho_id' => ['No se encontró el hecho']
    ]);
}
```

--------------------------------------------------------------------------------

### Datos de los agresores por hecho id

#### url
/api/datos-agresor-por-hecho/{hecho_id}

### PHP Rules

```php
// aquí ni validación hay O___O, hay que actualizar esto
if(!$hecho){
    throw ValidationException::withMessages([
        'hecho_id' => ['No se encontró el hecho']
    ]);
}
```

--------------------------------------------------------------------------------

### Seguimientos por folio EUV / id de Hecho de violencia

#### url

/api/seguimientos-por-hecho

#### props

```json
{
    "folio_euv" : "VALID_EUV",
    "hecho_id" : 3
}
```

#### curl

```bash
curl --location 'http://localhost/api/seguimientos-por-hecho' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data '{
    "folio_euv" : "VALID_EUV",
    "hecho_id" : 3
}'
```

#### respuesta

[ver json](api-response-examples/seguimientos-por-hecho-de-violencia.json)

### PHP Rules

```php
// aquí ni validación hay O___O, hay que actualizar esto
if(!$victima){
    throw ValidationException::withMessages([
        'folio_euv' => ['El folio EUV no está registrado en el sistema']
    ]);
}
```

--------------------------------------------------------------------------------

### Seguimientos por folio EUV

#### url

/api/seguimientos-por-euv

#### props

```json
{
    "folio_euv": "VALID_EUV"
}
```

#### curl

```bash
curl --location 'http://localhost/api/seguimientos-por-euv' \
--header 'Accept: application/json' \
--header 'Content: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data '{
    "folio_euv": "VALID_EUV"
}'
```

#### respuesta

[ver json](api-response-examples/hechos-de-violencia-por-euv.json)

### PHP Rules

```php
// aquí ni validación hay O___O, hay que actualizar esto
if(!$victima){
    throw ValidationException::withMessages([
        'folio_euv' => ['El folio EUV no está registrado en el sistema']
    ]);
}
```

#### tipo de usuario

* cualquiera

--------------------------------------------------------------------------------

### Folios EUV por enlace estatal

#### url
/api/folios-euv-por-enlace-estatal

#### props

```json
{
    "fecha_inicial" : "2022-5-27",
    "fecha_final"   : "2024-5-27",
    "enlace_id"     : 41
}
```

#### curl

```bash
curl --location 'http://localhost/api/folios-euv-por-enlace-estatal' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data '{
    "fecha_inicial" : "2022-5-27",
    "fecha_final"   : "2024-5-27",
    "enlace_id"     : 41
}'
```

#### respuesta

```json
[
    "d581d199-9808-316c-bbf3-e83afec4486f",
    "1bd73c3e-3929-35f8-9603-9f108c67d588",
    "b49cc65c-eaa9-36a1-b5c7-95ca59fb4a1c",
    "d191d3da-2a32-3c9d-bfa5-2aaaf41b50d7",
    "f3066d0e-1000-3c87-9825-d761219d16d5"
]
```

### PHP Rules

```php
$user       = $request->user();
$perfil_id  = env('ENLACE_ESTATAL', 3);

if($user->perfiles_id != $perfil_id ){
    throw ValidationException::withMessages([
        'user' => ['El usuario no tiene permisos para realizar esta acción']
    ]);
}

$request->validate([
    'fecha_inicial'  => 'required|date|before:fecha_final',
    'fecha_final'    => 'required|date|before:tomorrow',
    'enlace_id' => [ 
        'required', 
        'exists:users,id', 
        function($attribute, $value, $fail) use ($perfil_id, $user) {
            $enlace = User::find($value);
            if($enlace->perfiles_id != $perfil_id){
                $fail('El usuario no es un enlace estatal');
            }
            else if($enlace->cve_ent != $user->cve_ent){
                $fail('El enlace no pertenece a la entidad federativa del usuario');
            }
        }]
]);
```

#### tipo de usuario

* enlace estatal

--------------------------------------------------------------------------------

### Folios EUV por estado

#### url
/api/folios-euv-por-estado

#### props

```json
{
    "fecha_inicial" : "2022-5-27",
    "fecha_final"   : "2024-5-27",
    "page" : 2,
    "cve_ent" : 21
}
```

#### curl

```bash
curl --location 'http://localhost/api/folios-euv-por-estado' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCES_TOKEN' \
--data '{
    "fecha_inicial" : "2022-5-27",
    "fecha_final"   : "2024-5-27"
}'
```

#### respuesta

```json
{
    "total": 5,
    "page": 1,
    "folios": [
        "fc2eba0f-a220-37d4-a524-b552e81db699",
        "ac8ffb0a-c7e1-3c33-9180-e72f5dd5d5ee",
        "3542dcee-7d42-3b22-bbf5-d0d372327ef7",
        "eba43c76-b2eb-3b7b-a047-3523c3d3c436",
        "1912314a-97ef-31e9-8b05-e4b1c42fd8e9"
    ]
}
```

### PHP Rules

```php
$request->validate([
    'fecha_inicial'  => 'required|date|before:fecha_final',
    'fecha_final'    => 'required|date|before:tomorrow',
    'cve_ent'        => 'required|integer|between:1,32',
    'page'           => 'integer|min:1'
]);
```

#### tipo de usuario

* cualquiera

--------------------------------------------------------------------------------

### Hechos de violencia por entidad y rango de fecha

#### url
/api/hechos

#### props

```json
{
    "fecha_inicial" : "2022-5-27",
    "fecha_final"   : "2024-5-27",
    "page"          : 2,
    "cve_ent"       : 21
}
```


#### respuesta

```json
{
    "total": 14,
    "page": 2,
    "pages": 2,
    "hechos_id": [
        2,
        3,
        4,
        5,
        6,
        7,
        9,
        10,
        11,
        12
    ]
}
```

### PHP Rules

```php
$request->validate([
    'fecha_inicial' => 'nullable|date|before:tomorrow',
    'fecha_final'   => 'nullable|date|before:tomorrow|after:fecha_inicial',
    'page'          => 'nullable|integer|min:1',
    'cve_ent'       => 'nullable|integer|between:1,32'
]);
```

--------------------------------------------------------------------------------

### Hecho por id

#### url
/api/hecho

#### props

```json
{
    
    "id" : 21
}
```


#### respuesta

```json
{
    "id": 1,
    "victimas_id": 1,
    "users_id": 1,
    "fecha_hechos": "2022-10-24",
    "hora_hechos": "08:12:00",
    "descripcion_hechos": "hecho de violencia",
    "lugar_id": 1,
    "lugar_detalle_id": 1,
    "en_domicilio_victima": true,
    "pais_id": 1,
    "cve_ent": 21,
    "cve_mun": 1,
    "calle": "1",
    ...
}
```

### PHP Rules

```php
$request->validate([
    'id' => 'required|integer|exists:hechos,id'
]);
```


#### tipo de usuario

* cualquiera

--------------------------------------------------------------------------------

### Orden de protección por id

#### url
/api/orden-de-proteccion

#### props

```json
{
    
    "id" : 1
}
```


#### respuesta

```json
{
    "id": 1,
    "tipo_id": 2,
    "en_que_consiste": "Descripción de la orden",
    "autoridad_emisora_otro": "Otra autoridad",
    "cve_ent": 15,
    "cve_mun": 1,
    "fecha_de_emision": "2023-10-01",
    "tiempo_indefinido": true,
    "fecha_aproximada_de_termino": "2023-12-31",
    "otro_tipo_de_orden": "Otro tipo de orden",
    "created_at": "2024-11-20T17:24:13.000000Z",
    "updated_at": "2024-11-20T17:37:01.000000Z",
    "fecha_de_inicio": "2023-10-01",
    "deleted_at": null,
    "users_id": 1,
    "hechos_id": 1,
    "autoridad_id": 1,
    "folio": "ABC123",
    "dia_id": 1,
    "tipo_orden_id": 1,
    "tipo_medida_id": 1,
    "fracciones_tipo_orden": [
        1,
        2
    ],
    "fracciones_tipo_medida": [
        1,
        2
    ],
    "tipo": {
        "id": 2,
        "descripcion": "Orden de Protección",
        "es_activo": true,
        "created_at": "2024-11-13T19:56:12.000000Z",
        "updated_at": "2024-11-13T19:56:12.000000Z"
    },
    "tipo_orden": {
        "id": 1,
        "descripcion": "Administrativa",
        "es_activo": true,
        "created_at": "2024-11-13T19:56:12.000000Z",
        "updated_at": "2024-11-13T19:56:12.000000Z"
    },
    "estado": {
        "cve_ent": 15,
        "nom_ent": "México",
        "created_at": "2024-11-13T19:54:13.000000Z",
        "updated_at": "2024-11-13T19:54:13.000000Z",
        "codigo": "mc"
    }
}
```

### PHP Rules

```php
$request->validate([
    "id" => "required|integer|exists:ordenes_de_proteccions,id",
]);
```


#### tipo de usuario

* cualquiera

--------------------------------------------------------------------------------

### Ordenes de protección paginadas

#### url
/api/ordenes-de-proteccion

#### props

```json
{
    "fecha_inicial": "2023-10-01",
    "fecha_final": "2023-10-10",
    "page": 1,
    "cve_ent": 15
}
```


#### respuesta

```json
{
    "total": 2,
    "page": 1,
    "pages": 1,
    "ordenes_id": [
        2,
        1
    ]
}
```

### PHP Rules

```php
$request->validate([
    'fecha_inicial' => 'nullable|date|before:tomorrow',
    'fecha_final'   => 'nullable|date|before:tomorrow|after:fecha_inicial',
    'page'          => 'nullable|integer|min:1',
    'cve_ent'       => 'nullable|integer|between:1,32'
]);
```


#### tipo de usuario

* cualquiera

## Métodos de registro disponibles

### Registro de víctima con CURP

#### url
/api/registrar-victima-con-curp

#### props

```json
{
  "curp"              : "XXXXXXXXXXXXXXXXXX",
  "cve_ent"           : 22,
  "nacionalidad_id"   : 1,
  "extranjera"        : false
}
```

### PHP Rules

```php
$request->validate([
    'cve_ent'           =>  'required|integer|between:1,32',
    'nacionalidad_id'   =>  'integer|exists:nacionalidades,id',
    'extranjera'        =>  'boolean',
    'instancias_id'     =>  'nullable|integer',
    'areas_id'          =>  'nullable|integer',
    'sedes_id'          =>  'nullable|integer',
    'identifica_mujer'  =>  'sometimes|boolean',
    'curp'              =>  ['required', 'size:18', function ($attribute, $value, $fail) {
                                $victima = Victima::where('curp', $value)->first();
                                if($victima){
                                    $fail('La CURP ya existe en el sistema, folio_euv: ' . $victima->folio_euv);
                                }
                            }]
]);

// esto podría integrarse a la validación principal, pero 
// podría se una carga que no es necesaria
if($curpFromRenapo['status'] == 'error'){
    throw ValidationException::withMessages([
        'curp' => [$curpFromRenapo['message']]
    ]);
}
```

--------------------------------------------------------------------------------

### Registro de víctima con Datos

#### url
/api/registrar-victima-con-datos

#### props

```json
{
  "nombre"            : "FRANCISCO",
  "primer_apellido"   : "CAMPOS",
  "segundo_apellido"  : "BAUTISTA",
  "fecha_nacimiento"  : "1965-11-20",
  "sexo"              : "H",
  "cve_ent"           : 24,
  "extranjera"        : false
}
```

### PHP Rules

```php
$request->validate([
    'nombre'            =>  'required',
    'primer_apellido'   =>  'required',
    'segundo_apellido'  =>  'required',
    'fecha_nacimiento'  =>  'required|date',
    'sexo'              =>  'required|string|uppercase|exists:catalogo_sexos,renapo_id',
    'cve_ent'           =>  'required|integer|between:1,32',
    'nacionalidad_id'   =>  'nullable|integer|exists:nacionalidades,id',
    'extranjera'        =>  'boolean',
    'instancias_id'     =>  'nullable|integer',
    'areas_id'          =>  'nullable|integer',
    'sedes_id'          =>  'nullable|integer',
    'identifica_mujer'  =>  'sometimes|boolean',
]);

// esto podría integrarse a la validación principal, pero 
// podría se una carga que no es necesaria
if($curpFromRenapo['status'] == 'error'){
    throw ValidationException::withMessages([
        'curp' => [$curpFromRenapo['message']]
    ]);
}
```

--------------------------------------------------------------------------------

### Registro de víctima sin CURP

#### url
/api/registrar-victima-sin-curp

#### props

```json
{
  "nombre"            : "FRANCISCO",
  "primer_apellido"   : "CAMPOS",
  "segundo_apellido"  : "BAUTISTA",
  "fecha_nacimiento"  : "1965-11-20",
  "sexo"              : "H",
  "cve_ent"           : 24,
  "extranjera"        : false
}
```

### PHP Rules

```php
$request->validate([
    'nombre'            =>  'required',
    'primer_apellido'   =>  'required',
    'segundo_apellido'  =>  'required',
    'fecha_nacimiento'  =>  'required|date',
    'sexo'              =>  'required|string|uppercase|exists:catalogo_sexos,renapo_id',
    'cve_ent'           =>  'required|integer|between:1,32',
    'nacionalidad_id'   =>  'nullable|integer|exists:nacionalidades,id',
    'extranjera'        =>  'boolean',
    'instancias_id'     =>  'nullable|integer',
    'areas_id'          =>  'nullable|integer',
    'sedes_id'          =>  'nullable|integer',
    'identifica_mujer'  =>  'sometimes|boolean',
]);

// esto revisa si ya se ha registrdo alguien con el mismo curp falso
$fakeCurp = crear_curp([
    'nombre'            => $request->nombre,
    'primer_apellido'   => $request->primer_apellido,
    'segundo_apellido'  => $request->segundo_apellido,
    'fecha_nacimiento'  => $request->fecha_nacimiento,
    'sexo'              => $request->sexo,
    'estado'            => Entidad::where('cve_ent', $request->cve_ent)->first()
]);

$victima = Victima::where('curp', $fakeCurp)->first();
if($victima){
    throw ValidationException::withMessages([
        'curp' => ['La CURP ya existe en el sistema, folio_euv: ' . $victima->folio_euv]
    ]);
}
```


--------------------------------------------------------------------------------

### Registro/edición de datos de la víctima por hecho de violencia

#### url
/api/registrar-datos-victima-por-hecho

#### props

```json
{
  "hechos_id"   : 1,
  "edad"        : 33,
  "cve_ent"     : 21,
  "cve_mun"     : 132,
  "cve_loc"     : 1
}
```

### PHP Rules

```php
$request->validate([
    "hechos_id"                     =>  'required|integer|exists:hechos,id',
    "edad"                          =>  'sometimes|integer',
    "telefono"                      =>  'sometimes|string',
    "correo_electronico"            =>  'sometimes|email',
    "calle"                         =>  'sometimes|string',
    "num_exterior"                  =>  'sometimes|string',
    "num_interior"                  =>  'sometimes|string',
    "cve_ent"                       =>  'sometimes|integer|between:1,32',
    'cve_mun'                       => ['sometimes','integer', function($attribute, $value, $fail) use($request){
        $municipio = Municipio::where('cve_ent', '=', $request->cve_ent)->where('cve_mun', '=', $value)->first();
        if (!$municipio) {
            $fail('El municipio no existe');
        }
    }],
    'cve_loc'                       => ['sometimes','integer', function($attribute, $value, $fail) use($request){
        // if cve_mun is not set, fail
        if (!$request->has('cve_mun'))  {
            $fail('El campo cve_mun es requerido para identificar la localidad');
        }

        $localidad = Localidad::where('cve_ent', '=', $request->cve_ent)->where('cve_mun', '=', $request->cve_mun)->where('cve_loc', '=', $value)->first();
        if (!$localidad) {
            $fail('La localidad no existe');
        }
    }],
    'colonias_id'                   => ['sometimes', 'integer', 'exists:colonias,id'],
    'entre_calle_uno'               => 'sometimes|string',
    'entre_calle_dos'               => 'sometimes|string',
    'referencia'                    => 'sometimes|string',
    'estado_conyugal'               => 'sometimes|exists:catalogo_estado_conyugal,id',
    'pais'                          => 'sometimes|exists:catalogo_pais,id',
    'codigo_postal'                 => 'sometimes|integer|exists:colonias,cp',
    'escolaridad_id'                => 'sometimes|exists:catalogo_escolaridad,id',
    'ingreso_economico_id'          => 'sometimes|exists:catalogo_ingreso_economico,id',
    'ocupacion_id'                  => 'sometimes|exists:catalogo_ocupacion,id',
    'presenta_discapacidad'         => 'sometimes|boolean',
    'discapacidad'                  => 'sometimes|array',
    'discapacidad.*'                => 'sometimes|exists:catalogo_discapacidad,id',
    'otra_discapacidad'             => 'sometimes|string',
    'enfermedad'                    => 'sometimes|boolean',
    'cual_enfermedad'               => 'sometimes|string',
    'enfermedad_psiquiatrica'       => 'sometimes|boolean',
    'cual_enfermedad_psiquiatrica'  => 'sometimes|string',
    'tratamiento'                   => 'sometimes|boolean',
    'cual_tratamiento'              => 'sometimes|string',
    'situacion_calle'               => 'sometimes|boolean',
    'migrante'                      => 'sometimes|boolean',
    'pueblo_indigena'               => 'sometimes|boolean',
    'pueblo_indigena_id'            => 'sometimes|exists:catalogo_pueblo_indigena,id',
    'lgbtti'                        => 'sometimes|boolean',
    'identidad_genero_id'           => 'sometimes|exists:catalogo_identidad_genero,id',
    'orientacion_sexual_id'         => 'sometimes|exists:catalogo_orientacion_sexual,id',
    'embarazada'                    => 'sometimes|boolean',
    'numero_semanas_emb'            => 'sometimes|integer',
    'dependientes'                  => 'sometimes|boolean',
    'red_apoyo'                     => 'sometimes|boolean',
    'adiccion'                      => 'sometimes|boolean',
    'cual_adiccion'                 => 'sometimes|array',
    'cual_adiccion.*'               => 'sometimes|exists:catalogo_adiccion,id',
    'persona_afrodescendiente'      => 'sometimes|string',
    'caso_name'                     => 'sometimes|string',
    'realiza_mas_actividades'       => 'sometimes|boolean',
    'discapacidad_por_violencia'    => 'sometimes|boolean',
    'gestacion_id'                  => 'sometimes|exists:catalogo_semanas_gestacion,id',
    'clave_centro_escolar'          => 'sometimes|string',
    'numero_seguro_social'          => 'sometimes|string',
    'servicio_medico_id'            => 'sometimes|exists:catalogo_servicios_medicos,id',
    'otro_servicio_medico'          => 'sometimes|string',
    'tipos_adicciones'              => 'sometimes|array',
    'tipos_adicciones.*'            => 'sometimes|exists:catalogo_adiccion,id'

]);
```

--------------------------------------------------------------------------------

### Registro de dependiente por hecho de violencia

#### url
/api/registrar-dependiente-victima-por-hecho

#### props

```json
{
    "hechos_id"         : 1,
    "nombre"            : "Juanita",
    "vinculo_victima_id": 4
}
```

### PHP Rules

```php
$request->validate([
    "hechos_id"                     =>  'required|integer|exists:hechos,id',
    "nombre"                        =>  'required|string',
    "primer_apellido"               =>  'sometimes|string',
    "segundo_apellido"              =>  'sometimes|string',
    "edad"                          =>  'sometimes|integer',
    "vinculo_victima_id"            =>  'required|integer|exists:catalogo_vinculo_victima,id',
    "esta_en_riesgo"                =>  'sometimes|boolean',
    "presenta_discapacidad"         =>  'sometimes|boolean',
    "extranjera"                    =>  'sometimes|boolean',
    "enfermedad"                    =>  'sometimes|boolean',
    "cual_enfermedad"               =>  'sometimes|string',
    "observaciones"                 =>  'sometimes|string'
]);
```

--------------------------------------------------------------------------------

### Registro de red de apoyo por hecho de violencia

#### url
/api/registrar-red-apoyo-victima-por-hecho

#### props

```json
{
    "hechos_id"             : 1,
    "nombre"                : "Juanita",
    "vinculo_victima_id"    : 4,
    "telefono"              : "2258877731"
}
```

### PHP Rules

```php
$request->validate([
    "hechos_id"                     =>  'required|integer|exists:hechos,id',
    "nombre"                        =>  'required|string',
    "primer_apellido"               =>  'sometimes|string',
    "segundo_apellido"              =>  'sometimes|string',
    "vinculo_victima_id"            =>  'required|integer|exists:catalogo_vinculo_victima,id',
    "telefono"                      =>  'required|string',
    "observaciones"                 =>  'sometimes|string'
]);
```

--------------------------------------------------------------------------------

### Registro de hecho de violencia

#### url
/api/hecho-de-violencia

#### props

```json
{
  "folio_euv"                                     : "{{folio_euv}}",
  "fecha_hechos"                                  : "2024-01-30",
  "hora_hechos"                                   : "22:12",
  "descripcion_hechos"                            : "Hecho de violencia registrado en ....",
  "lugar_id"                                      : 1,
  "lugar_detalle_id"                              : 22,
  "en_domicilio_victima"                          : false,
  "pais_id"                                       : 238,
  "cve_ent"                                       : 11,
  "cve_mun"                                       : 1,
  "calle"                                         : "siempre viva 23",
  "num_exterior_km"                               : "45",
  "cve_loc"                                       : 1,
  "lat"                                           : 19.2843100,
  "lng"                                           : -98.4388500,
  "es_festivo"                                    : false,
  "conoce_la_autoridad"                           : false,
  "tipo_violencia"                                : [1,3,5,8],
  "modalidad_violencia"                           : 6,
  "es_victima_de_delincuencia_organizada"         : false,
  "hay_denuncia"                                  : false,
  "efectos_fisicos"                               : [1,2],
  "consecuencias_sexuales"                        : [],
  "efectos_psicologicos"                          : [],
  "efectos_economicos_y_patrimoniales"            : [],
  "agente_de_lesion"                              : [],
  "area_anatomica_lesionada"                      : [],
  "es_relacionada_con_orientacion_o_identidad"    : false
}
```

### PHP Rules

```php
$request->validate([
    'folio_euv'                                     => 'required|string|exists:victimas,folio_euv',
    'fecha_hechos'                                  => 'required|date_format:Y-m-d|before:tomorrow',
    'hora_hechos'                                   => 'required|date_format:H:i',
    'descripcion_hechos'                            => 'required|string|max:1000',
    'lugar_id'                                      => 'integer|exists:catalogo_localidad,id',
    'lugar_detalle_id'                              => 'integer|exists:catalogo_detalle_localidad,id',
    'en_domicilio_victima'                          => 'boolean',
    'pais_id'                                       => 'integer|exists:catalogo_pais,id',
    'cve_ent'                                       => 'required|integer|between:1,32',
    'cve_mun'                                       => ['integer', function($attribute, $value, $fail) use($request){
        $municipio = Municipio::where('cve_ent', '=', $request->cve_ent)->where('cve_mun', '=', $value)->first();
        if (!$municipio) {
            $fail('El municipio no existe');
        }
    }],
    'calle'                                         => 'string|max:150',
    'num_exterior_km'                               => 'string|max:45',
    'num_interior'                                  => 'string|max:45',
    'cve_loc'                                       => ['integer', function($attribute, $value, $fail) use($request){
        if (!$request->has('cve_mun'))  {
            $fail('El campo cve_mun es requerido para identificar la localidad');
        }

        $localidad = Localidad::where('cve_ent', '=', $request->cve_ent)->where('cve_mun', '=', $request->cve_mun)->where('cve_loc', '=', $value)->first();
        if (!$localidad) {
            $fail('La localidad no existe');
        }
    }],
    'cp'                                            => ['string', 'max:6', 'exists:colonias,cp'],
    'lat'                                           => 'min:-90|max:90',
    'lng'                                           => 'min:-180|max:180',
    'es_festivo'                                    => 'boolean',
    'conoce_la_autoridad'                           => 'boolean',
    'conoce_la_autoridad_detalle'                   => 'string|max:150',
    'tipo_violencia'                                => 'array',
    'modalidad_violencia'                           => 'integer|exists:catalogo_modalidad_violencia,id',
    'es_victima_de_delincuencia_organizada'         => 'boolean',
    'hay_denuncia'                                  => 'boolean',
    'efectos_fisicos'                               => ['array'],
    'efectos_fisicos.*'                             => 'exists:catalogo_efecto_fisico,id',
    'consecuencias_sexuales'                        => ['array'],
    'consecuencias_sexuales.*'                      => 'exists:catalogo_efecto_consecuencia_sexual,id',
    'efectos_psicologicos'                          => ['array'],
    'efectos_psicologicos.*'                        => 'exists:catalogo_efecto_psicologico,id',
    'efectos_economicos_y_patrimoniales'            => ['array'],
    'efectos_economicos_y_patrimoniales.*'          => 'exists:catalogo_efecto_economico,id',
    'agente_de_lesion'                              => ['array'],
    'agenete_de_lesion.*'                           => 'exists:catalogo_efecto_agente_lesion,id',
    'area_anatomica_lesionada'                      => ['array'],
    'area_anatomica_lesionada.*'                    => 'exists:catalogo_efecto_area_anatomica_lesionada,id',
    'es_relacionada_con_orientacion_o_identidad'    => 'boolean',
    'lugar_detalle_otro'                            => 'sometimes|string|max:255',
    'tipo_violencia_otro'                           => 'sometimes|string|max:255',
    'efecto_fisico_otro'                            => 'sometimes|string|max:255',
    'consecuencia_sexual_otro'                      => 'sometimes|string|max:255',
    'efecto_psicologico_otro'                       => 'sometimes|string|max:255',
    'efecto_economico_patrimonial_otro'             => 'sometimes|string|max:255',
    'agente_lesion_otro'                            => 'sometimes|string|max:255',
    'area_anatomica_lesionada_otro'                 => 'sometimes|string|max:255'  
]);
```

--------------------------------------------------------------------------------

### Registro de mujeres en prisión para un hecho de violencia

#### url
/api/registrar-mujer-en-prision

#### props

```json
{
    "hechos_id"             : 5,
    "privada_de_libertad"   : true,
    "calidad_legal_id"      : 1,
    "delitos"               : "robo",
    "victima_de_tortura"    : true,
    "protocolo_de_estambul" : true,
    "victima_de_tortura_despues_del_protocolo_de_estambul" : true,
    "tortura_tipo_id"       : 1,
    "tortura_momento_id"    : 5,
    "autoridad_id"          : 1,
}
```

### PHP rules

```php
$request->validate([
    'hechos_id'             => 'required|exists:hechos,id',
    'privada_de_libertad'   => 'nullable|boolean',
    'calidad_legal_id'      => 'nullable|exists:catalogo_calidad_legals,id',
    'delitos'               => 'nullable|string',
    'victima_de_tortura'    => 'nullable|boolean',
    'protocolo_de_estambul' => 'nullable|boolean',
    'victima_de_tortura_despues_del_protocolo_de_estambul' => 'nullable|boolean',
    'tortura_tipo_id'       => 'nullable|exists:catalogo_tortura_en_prision_tipos,id',
    'tortura_momento_id'    => 'nullable|exists:catalogo_tortura_en_prision_momentos,id',
    'autoridad_id'          => 'nullable|exists:catalogo_autoridades,id',
]);
```


--------------------------------------------------------------------------------

### Registro de mujeres víctimas de trata para un hecho de violencia

#### url
/api/registrar-mujer-victima-de-trata

#### props

```json
{
    "hechos_id"                     : 5,
    "victima_de_trata_de_personas"  : true,
    "accion_omision_dolosa_id"      : 8,
    "fines_reclutamiento_id"        : 4,
    "accion_omision_dolosa_array"   : [1,2,3],
    "fines_reclutamiento_array"     : [1,2, 3]
}
```

### PHP Rules

```php
$request->validate([
    'hechos_id'                     => 'required|exists:hechos,id',
    'victima_de_trata_de_personas'  => 'nullable|boolean',
    'accion_omision_dolosa_id'      => 'nullable|exists:catalogo_accion_omision_dolosa_con_fines_de_explotacions,id',
    'fines_reclutamiento_id'        => 'nullable|exists:catalogo_fines_reclutamientos,id',
    'accion_omision_dolosa_array'   => 'nullable|array',
    'accion_omision_dolosa_array.*' => 'exists:catalogo_accion_omision_dolosa_con_fines_de_explotacions,id',
    'fines_reclutamiento_array'     => 'nullable|array',
    'fines_reclutamiento_array.*'   => 'exists:catalogo_fines_reclutamientos,id'
]);
```


--------------------------------------------------------------------------------

### Registro de mujeres desaparecidas para un hecho de violencia

#### url
/api/registrar-mujer-desaparecida

#### props

```json
{
    "hechos_id"                 : 5,
    "persona_desaparecida"      : true,
    "tipo_de_desaparicion_id"   : 2,
    "fecha_de_desaparicion"     : "2022-11-1",
    "cve_ent"                   : 21,
    "cve_mun"                   : 132,
    "es_institucion"            : true,
    "nombre_institucion"        : "CCI",
    "nombre"                    : "JP",
    "apellido_paterno"          : "MRT",
    "apellido_materno"          : "MKT",
    "edad"                      : 23,
    "vinculo_victima_id"        : 4,
    "estatus_desaparicion_id"   : 1,
    "con_vida"                  : true
}
```

### PHP Rules

```php
$request->validate([
    'hechos_id'                 => 'required|exists:hechos,id',
    'persona_desaparecida'      => 'nullable|boolean',
    'tipo_de_desaparicion_id'   => 'nullable|exists:catalogo_tipo_de_desaparicions,id',
    'fecha_de_desaparicion'     => 'nullable|date',
    'cve_ent'                   => 'nullable|integer|between:1,32',
    'cve_mun'                   => ['nullable', function($attribute, $value, $fail) {
        $municipio = Municipio::where('cve_ent', '=', $this->cve_ent)->where('cve_mun', '=', $value)->first();
        if (!$municipio) {
            $fail('El municipio no existe');
        }
    }],
    'es_institucion'            => 'nullable|boolean',
    'nombre_institucion'        => 'nullable|string',
    'nombre'                    => 'nullable|string',
    'apellido_paterno'          => 'nullable|string',
    'apellido_materno'          => 'nullable|string',
    'edad'                      => 'nullable|integer',
    'vinculo_victima_id'        => 'nullable|exists:catalogo_vinculo_victima,id',
    'estatus_desaparicion_id'   => 'nullable|exists:catalogo_estatus_victima_desaparecidas,id',
    'con_vida'                  => 'nullable|boolean'
]);
```

--------------------------------------------------------------------------------

### Registro una canalización para un hecho de violencia

#### url
/api/registrar-canalizacion

#### props

```json
{
    "hechos_id"                 : 5,
    "instancias_id"             : 251,
    "instancias_envia_id"       : 405,
    "cve_ent"                   : 21,
    "cve_mun"                   : 132,
    "fecha_canalizacion"        : "2022-03-2",
    "observaciones_incompetencia" : "nope",
    "observaciones"             : "nein",
    "acompanamiento"            : false,
    "estatus"                   : "ahí va",
    "nombre"                    : "JJJJ",
    "primer_apellido"           : "PPPP",
    "segundo_apellido"          : "ZZZZZ",
    "tipo_id"                   : 1,
    "servicios_id"              : 1
}
```

### PHP Rules

```php
$request->validate([
    'hechos_id'                 => 'required|exists:hechos,id',
    'instancias_id'             => 'required|exists:catalogo_dependencias,pk_dependencia',
    'instancias_envia_id'       => 'required|exists:catalogo_dependencias,pk_dependencia',
    'cve_ent'                   => 'exists:entidades,cve_ent',
    'cve_mun'                   => ['nullable', function($attribute, $value, $fail) {
        $municipio = Municipio::where('cve_ent', '=', $this->cve_ent)->where('cve_mun', '=', $value)->first();
        if (!$municipio) {
            $fail('El municipio no existe');
        }
    }],
    'fecha_canalizacion'        => 'required|date',
    'observaciones_incompetencia' => 'string',
    'observaciones'             => 'required|string',
    'acompanamiento'            => 'required|boolean',
    'estatus'                   => 'string|max:16',
    'nombre'                    => 'nullable|string|max:128',
    'primer_apellido'           => 'nullable|string|max:128',
    'segundo_apellido'          => 'nullable|string|max:128',
    'tipo_id'                   => 'required|exists:catalogo_tipo_servicio,id',
    'servicios_id'              => 'required|exists:catalogo_servicios,id'
]);
```

--------------------------------------------------------------------------------

### Registro de un agresor para un hecho de violencia

#### url
/api/registrar-agresor

#### props

```json
{
    "hechos_id"                 : 5,
    "nombre"                    : "FFFF",
    "primer_apellido"           : "AAAA",
    "segundo_apellido"          : "AAAA",
    "curp"                      : "XXXXXXXXXXXXXXXXXX",
    "sexo_id"                   : 1,
    "edad"                      : 50,
    "mismo_domicilio_victima"   : true,
    "acceso_a_armas"            : true,
    "tipos_armas"               : [1],
    "vinculo_victima_id"        : 5,
    "identidad_genero_id"       : null,
    "orientacion_sexual_id"     : 1,
    "calle"                     : "FFFFFFFF",
    "num_exterior"              : "23 A",
    "num_interior"              : "5B, piso 3",
    "cve_ent"                   : 21,
    "cve_mun"                   : 132,
    "cve_loc"                   : 1,
    "codigo_postal"             : 74000,
    "colonias_id"               : 16813,
    "entre_calle_uno"           : null,
    "entre_calle_dos"           : null,
    "referencia"                :null,
    "escolaridad_id"            : null,
    "ingreso_economico_id"      : null,
    "ocupacion_id"              : 3,
    "conoce_persona"            : true,
    "consume_drogas"            : true,
    "tipos_drogas"              : [1],
}
```

### PHP Rules

```php
$request->validate([
    'hechos_id'                 => 'required|exists:hechos,id',
    'nombre'                    => 'required|string|max:64',
    'primer_apellido'           => 'required|string|max:64',
    'segundo_apellido'          => 'required|string|max:64',
    'curp'                      => ['required', function($attribute, $value, $fail) {

        // search for the curp in the agresor table, with the same curp and hechos_id
        $agresor = Agresor::where('curp', '=', $value)->where('hechos_id', '=', $this->hechos_id)->first();

        // if the curp exists, fail with a message
        if ($agresor) {
            $fail('La CURP ya existe');
        }

        $curpFromRenapo = buscar_curp($value);

        /// id $curpFromRenapo['status'] == 'error', fail with $curpFromRenapo['message']
        if ($curpFromRenapo['status'] == 'error') {
            $fail($curpFromRenapo['message']);
        }
    }],
    'sexo_id'                   => 'required|exists:catalogo_sexos,id',
    'edad'                      => 'nullable|integer',
    'mismo_domicilio_victima'   => 'nullable|boolean',
    'acceso_a_armas'            => 'nullable|boolean',
    'tipos_armas'               => 'nullable|array',
    'tipos_armas.*'             => 'exists:catalogo_tipos_armas,id',
    'vinculo_victima_id'        => 'nullable|exists:catalogo_vinculo_victima,id',
    'identidad_genero_id'       => 'nullable|exists:catalogo_identidad_genero,id',
    'orientacion_sexual_id'     => 'nullable|exists:catalogo_orientacion_sexual,id',
    'calle'                     => 'nullable|string|max:150',
    'num_exterior'              => 'nullable|string|max:45',
    'num_interior'              => 'nullable|string|max:45',
    'cve_ent'                   => 'nullable|exists:entidades,cve_ent',
    'cve_mun'                   => ['nullable', function($attribute, $value, $fail) {
        $municipio = Municipio::where('cve_ent', '=', $this->cve_ent)->where('cve_mun', '=', $value)->first();
        if (!$municipio) {
            $fail('El municipio no existe');
        }
    }],
    'cve_loc'                   => ['nullable', function($attribute, $value, $fail) {
        $localidad = Localidad::where('cve_ent', '=', $this->cve_ent)->where('cve_mun', '=', $this->cve_mun)->where('cve_loc', '=', $value)->first();
        if (!$localidad) {
            $fail('La localidad no existe');
        }
    }],
    'codigo_postal'             => 'nullable|integer|exists:colonias,cp',
    'colonias_id'               => 'nullable|exists:colonias,id',
    'entre_calle_uno'           => 'nullable|string|max:150',
    'entre_calle_dos'           => 'nullable|string|max:150',
    'referencia'                => 'nullable|string|max:256',
    'escolaridad_id'            => 'nullable|exists:catalogo_escolaridad,id',
    'ingreso_economico_id'      => 'nullable|exists:catalogo_ingreso_economico,id',
    'ocupacion_id'              => 'nullable|exists:catalogo_ocupacion,id',
    'conoce_persona'            => 'required|boolean',
    'consume_drogas'            => 'nullable|boolean',
    'tipos_drogas'              => 'nullable|array',
    'tipos_drogas.*'            => 'exists:catalogo_tipos_drogas,id'
]);
```


--------------------------------------------------------------------------------

### Registro de una orden de protección

#### url
/api/registrar-orden-de-proteccion

#### props

```json
{
    "tipo_id": 1,
    "en_que_consiste": "Descripción de la orden",
    "autoridad_emisora_otro": "Otra autoridad",
    "cve_ent": 15,
    "cve_mun": 1,
    "fecha_de_emision": "2023-10-01",
    "tiempo_indefinido": true,
    "fecha_aproximada_de_termino": "2023-12-31",
    "otro_tipo_de_orden": "Otro tipo de orden",
    "fecha_de_inicio": "2023-10-01",
    "hechos_id": 1,
    "autoridad_id": 1,
    "folio": "ABC123",
    "dia_id": 1,
    "tipo_orden_id": 1,
    "tipo_medida_id": 1,
    "fracciones_tipo_orden": [1, 2],
    "fracciones_tipo_medida": [1, 2]
}
```

### PHP Rules

```php
$request->validate([
    "tipo_id"                       => "required|integer|exists:catalogo_orden_de_proteccion_tipos,id",
    "en_que_consiste"               => "sometimes|string",
    "autoridad_emisora_otro"        => "sometimes|string",
    "cve_ent"                       => "required|integer|between:1,32",
    'cve_mun'                       => ['sometimes','integer', function($attribute, $value, $fail) use($request){
        $municipio = Municipio::where('cve_ent', '=', $request->cve_ent)->where('cve_mun', '=', $value)->first();
        if (!$municipio) {
            $fail('El municipio no existe');
        }
    }],
    "fecha_de_emision"              => "sometimes|date",
    "tiempo_indefinido"             => "sometimes|boolean",
    "fecha_aproximada_de_termino"   => "sometimes|date",
    "otro_tipo_de_orden"            => "sometimes|string",
    "fecha_de_inicio"               => "sometimes|date",
    "hechos_id"                     => "required|integer|exists:hechos,id",
    "autoridad_id"                  => "sometimes|integer|exists:catalogo_autoridades,id",
    "folio"                         => "sometimes|string|max:255",
    "dia_id"                        => "sometimes|integer|exists:catalogo_dias,id",
    "tipo_orden_id"                 => "sometimes|integer|exists:catalogo_tipos_ordenes_proteccion,id",
    "tipo_medida_id"                => "sometimes|integer|exists:catalogo_tipos_medidas_proteccion,id",
    "fracciones_tipo_orden"         => "sometimes|array",
    "fracciones_tipo_orden.*"       => "integer|exists:catalogo_fracciones_tipos_ordenes_proteccion,id",
    "fracciones_tipo_medida"        => "sometimes|array",
    "fracciones_tipo_medida.*"      => "integer|exists:catalogo_fracciones_tipos_medidas_proteccion,id",
]);
```


--------------------------------------------------------------------------------

## Métodos de edición disponibles

### Editar hecho de violencia

#### url
__post__: /api/edita-hecho-de-violencia


#### props

```json
{
    "id" : 1,
    "fecha_hechos" : "2022-10-24",
    "conoce_la_autoridad" : true,
    "conoce_la_autoridad_detalle" : "al juez"
}
```

### PHP Rules

```php
$request->validate([
    'id'                                           => 'required|exists:hechos,id',
    'fecha_hechos'                                 => 'sometimes|required|date_format:Y-m-d|before:tomorrow',
    'hora_hechos'                                  => 'sometimes|required|date_format:H:i',
    'descripcion_hechos'                           => 'sometimes|required|string|max:1000',
    'lugar_id'                                     => 'sometimes|integer|exists:catalogo_localidad,id',
    'lugar_detalle_id'  => ['sometimes', 'integer', function($attribute, $value, $fail) use($request){
        // si no valida lugar_id, no se valida lugar_detalle_id, regresa el error de el lugar_id es requerido
        if (!$request->has('lugar_id')) {
            $fail('El campo lugar_id es requerido para identificar el detalle del lugar');
            return;
        }

        $detalle = CatalogoDetalleLocalidad::where('fk_lugar_ocurrencia', '=', $request->lugar_id)->where('id', '=', $value)->first();

        if (!$detalle) {
            $fail('El detalle del lugar no existe');
        }
    }],
    'en_domicilio_victima'                         => 'sometimes|boolean',
    'pais_id'                                      => 'sometimes|integer|exists:catalogo_pais,id',
    'cve_ent'                                      => 'sometimes|required|integer|between:1,32',
    'cve_mun'                                      => ['sometimes', 'integer', function($attribute, $value, $fail) use($request){
        $municipio = Municipio::where('cve_ent', '=', $request->cve_ent)->where('cve_mun', '=', $value)->first();
        if (!$municipio) {
            $fail('El municipio no existe');
        }
    }],
    'calle'                                        => 'sometimes|string|max:150',
    'num_exterior_km'                              => 'sometimes|string|max:45',
    'num_interior'                                 => 'sometimes|string|max:45',
    'cve_loc'                                      => ['sometimes', 'integer', function($attribute, $value, $fail) use($request){
        // if cve_mun is not set, fail
        if (!$request->has('cve_mun'))  {
            $fail('El campo cve_mun es requerido para identificar la localidad');
        }

        $localidad = Localidad::where('cve_ent', '=', $request->cve_ent)->where('cve_mun', '=', $request->cve_mun)->where('cve_loc', '=', $value)->first();
        if (!$localidad) {
            $fail('La localidad no existe');
        }
    }],
    'cp'                                           => ['sometimes', 'string', 'max:6', 'exists:colonias,cp'],
    'lat'                                          => 'sometimes|min:-90|max:90',
    'lng'                                          => 'sometimes|min:-180|max:180',
    'es_festivo'                                   => 'sometimes|boolean',
    'conoce_la_autoridad'                          => 'sometimes|boolean',   
    'conoce_la_autoridad_detalle'                  => ['sometimes', 'string', 'max:150', function($attribute, $value, $fail) use($request){
        // fail if conoce_la_autoridad is not set or false, and conoce_la_autoridad_detalle is set
        if (!$request->has('conoce_la_autoridad') || !$request->conoce_la_autoridad) {
            $fail('El campo conoce_la_autoridad debe ser verdadero para poder registrar un detalle');
        }
    }], 
    'tipo_violencia'                               => 'sometimes|requied|array',
    'tipo_violencia.*'                             => 'sometimes|exists:catalogo_tipo_violencia,id',
    'modalidad_violencia'                          => 'sometimes|integer|exists:catalogo_modalidad_violencia,id',
    'es_victima_de_delincuencia_organizada'        => 'sometimes|boolean',
    'hay_denuncia'                                 => 'sometimes|boolean',
    'efectos_fisicos'                              => 'sometimes|array',
    'efectos_fisicos.*'                            => 'sometimes|exists:catalogo_efecto_fisico,id',
    'consecuencias_sexuales'                       => 'sometimes|array',
    'consecuencias_sexuales.*'                     => 'sometimes|exists:catalogo_efecto_consecuencia_sexual,id',
    'efectos_psicologicos'                         => 'sometimes|array',
    'efectos_psicologicos.*'                       => 'sometimes|exists:catalogo_efecto_psicologico,id',
    'efectos_economicos_y_patrimoniales'           => 'sometimes|array',
    'efectos_economicos_y_patrimoniales.*'         => 'sometimes|exists:catalogo_efecto_economico,id',
    'agente_de_lesion'                             => 'sometimes|array',
    'agenete_de_lesion.*'                          => 'sometimes|exists:catalogo_efecto_agente_lesion,id',
    'area_anatomica_lesionada'                     => 'sometimes|array',
    'area_anatomica_lesionada.*'                   => 'sometimes|exists:catalogo_efecto_area_anatomica_lesionada,id',
    'es_relacionada_con_orientacion_o_identidad'    => 'sometimes|boolean',
    // if colonias_id is set, cp is required; 
    // then search in Colonia model for id = colonias_id and cp = cp
    // if not found, fail

    'colonias_id'                                  => ['sometimes', 'integer', function($attribute, $value, $fail) use($request){
        if (!$request->has('cp')) {
            $fail('El campo cp es requerido para identificar la colonia');
        }

        $colonia = Colonia::where('id', '=', $value)->where('cp', '=', $request->cp)->first();
        if (!$colonia) {
            $fail('La colonia no existe');
        }
    }],
    // 'culmino_en_muerte': required if tipo_violencia array contains env:CATALOGO_TIPO_VIOLENCIA_VAL_FEMINICIDA or 9. The value is boolean
    'culmino_en_muerte'                           => ['sometimes', 'boolean', function($attribute, $value, $fail) use($request){
        $violenciaFeminicidaId = env('CATALOGO_TIPO_VIOLENCIA_VAL_FEMINICIDA', 9);
        $violenciaFeminicida = in_array($violenciaFeminicidaId, $request->tipo_violencia);
        if ($violenciaFeminicida && !$value) {
            $fail('El campo culmino_en_muerte es requerido si el tipo de violencia es feminicida');
        }
    }], 
    // tipo_muerte_id is required if culmino_en_muerte is true
    'tipo_muerte_id'                              => ['sometimes', 'integer', 'exists:catalogo_tipos_muerte,id', function($attribute, $value, $fail) use($request){
        if ($request->culmino_en_muerte && !$value) {
            $fail('El campo tipo_muerte_id es requerido si el hecho culminó en muerte');
        }
    }],

    // tipo_muerte_otro is required if tipo_muerte_id is equal to env(CATALOGO_TIPOS_MUERTE_VAL_OTRO, 4)
    'tipo_muerte_otro'                            => ['sometimes', 'string', 'max:150', function($attribute, $value, $fail) use($request){
        $tipoMuerteOtroId = env('CATALOGO_TIPOS_MUERTE_VAL_OTRO', 4);
        if ($request->tipo_muerte_id == $tipoMuerteOtroId && !$value) {
            $fail('El campo tipo_muerte_otro es requerido si el tipo de muerte es otro');
        }
    }],

    // tipo_violencia_sexual is required if env(CATALOGO_TIPO_VIOLENCIA_VAL_SEXUAL, 3) is in tipo_violencia &&
    // env(CATALOGO_MODALIDAD_VIOLENCIA_VAL_ESCOLAR, 4) || env(CATALOGO_MODALIDAD_VIOLENCIA_VAL_LABORAL, 8) is in modalidad_violencia
    'tipo_violencia_sexual'                      => ['sometimes', 'string', 'in:Acoso,Hostigamiento', function($attribute, $value, $fail) use($request){
        $violenciaSexualId  = env('CATALOGO_TIPO_VIOLENCIA_VAL_SEXUAL', 3);
        $modalidadEscolarId = env('CATALOGO_MODALIDAD_VIOLENCIA_VAL_ESCOLAR', 4);
        $modalidadLaboralId = env('CATALOGO_MODALIDAD_VIOLENCIA_VAL_LABORAL', 8);
        $violenciaSexual    = in_array($violenciaSexualId, $request->tipo_violencia);
        $modalidadEscolar   = $request->modalidad_violencia == $modalidadEscolarId;
        $modalidadLaboral   = $request->modalidad_violencia == $modalidadLaboralId;
        if ($violenciaSexual && ($modalidadEscolar || $modalidadLaboral) && count($value) == 0) {
            $fail('El campo tipo de violencia sexual es requerido si el tipo de violencia es sexual y la modalidad es escolar o laboral');
        }
    }],

    // tipo_agresor_sexual_id required if env(CATALOGO_MODALIDAD_VIOLENCIA_VAL_ESCOLAR, 4) || env(CATALOGO_MODALIDAD_VIOLENCIA_VAL_LABORAL, 8) is in modalidad_violencia && 
    // tipo_violencia_sexual is not empty
    'tipo_agresor_sexual_id' => ['sometimes', 'integer', 'exists:catalogo_tipos_agresores_sexuales,id', function($attribute, $value, $fail) use($request){
        $modalidadEscolarId = env('CATALOGO_MODALIDAD_VIOLENCIA_VAL_ESCOLAR', 4);
        $modalidadLaboralId = env('CATALOGO_MODALIDAD_VIOLENCIA_VAL_LABORAL', 8);
        $modalidadEscolar   = $request->modalidad_violencia == $modalidadEscolarId;
        $modalidadLaboral   = $request->modalidad_violencia == $modalidadLaboralId;
        // $violenciaIsValid is true if its equal to Acoso u Hostigamiento
        $violenciaIsValid = in_array($request->tipo_violencia_sexual, ['Acoso', 'Hostigamiento']);
        
        // if$ modalidadEscolar || $modalidadLaboral and tipo_violencia_sexual isvalid, then tipo_agresor_sexual_id is required
        if (($modalidadEscolar || $modalidadLaboral) && $violenciaIsValid && !$value) {
            $fail('El campo tipo de agresor sexual es requerido si la modalidad es escolar o laboral y el tipo de violencia sexual es acoso u hostigamiento');
        }
    }],

    // tipo_agresor_sexual_otro is required if :
    // tipo_agresor_sexual_id = env CATALOGO_TIPOS_AGRESORES_SEXUALES_VAL_RELACIONADAS_ESCUELA_SIN_JERARQUIA,4 or
    // tipo_agresor_sexual_id = env CATALOGO_TIPOS_AGRESORES_SEXUALES_VAL_RELACIONADAS_ESCUELA_CON_JERARQUIA,8 or
    // tipo_agresor_sexual_id = env CATALOGO_TIPOS_AGRESORES_SEXUALES_VAL_RELACIONADAS_TRABAJO_SIN_JERARQUIA, 12 or
    // tipo_agresor_sexual_id = env CATALOGO_TIPOS_AGRESORES_SEXUALES_VAL_RELACIONADAS_TRABAJO_CON_JERARQUIA, 16

    'tipo_agresor_sexual_otro' => ['sometimes', 'string', 'max:150', function($attribute, $value, $fail) use($request){
        $tipoAgresorSexualRelacionadasEscuelaSinJerarquia = env('CATALOGO_TIPOS_AGRESORES_SEXUALES_VAL_RELACIONADAS_ESCUELA_SIN_JERARQUIA', 4);
        $tipoAgresorSexualRelacionadasEscuelaConJerarquia = env('CATALOGO_TIPOS_AGRESORES_SEXUALES_VAL_RELACIONADAS_ESCUELA_CON_JERARQUIA', 8);
        $tipoAgresorSexualRelacionadasTrabajoSinJerarquia = env('CATALOGO_TIPOS_AGRESORES_SEXUALES_VAL_RELACIONADAS_TRABAJO_SIN_JERARQUIA', 12);
        $tipoAgresorSexualRelacionadasTrabajoConJerarquia = env('CATALOGO_TIPOS_AGRESORES_SEXUALES_VAL_RELACIONADAS_TRABAJO_CON_JERARQUIA', 16);
        $tipoAgresorSexualId = $request->tipo_agresor_sexual_id;
        $tipoAgresorSexualOtro = in_array($tipoAgresorSexualId, [
            $tipoAgresorSexualRelacionadasEscuelaSinJerarquia,
            $tipoAgresorSexualRelacionadasEscuelaConJerarquia,
            $tipoAgresorSexualRelacionadasTrabajoSinJerarquia,
            $tipoAgresorSexualRelacionadasTrabajoConJerarquia
        ]);
        if ($tipoAgresorSexualOtro && !$value) {
            $fail('El campo tipo de agresor sexual otro es requerido si el tipo de agresor sexual es relacionado a la escuela sin jerarquía, relacionado a la escuela con jerarquía, relacionado al trabajo sin jerarquía o relacionado al trabajo con jerarquía');
        }
    }],
    'lugar_detalle_otro'                            => 'sometimes|string|max:255',
    'tipo_violencia_otro'                           => 'sometimes|string|max:255',
    'efecto_fisico_otro'                            => 'sometimes|string|max:255',
    'consecuencia_sexual_otro'                      => 'sometimes|string|max:255',
    'efecto_psicologico_otro'                       => 'sometimes|string|max:255',
    'efecto_economico_patrimonial_otro'             => 'sometimes|string|max:255',
    'agente_lesion_otro'                            => 'sometimes|string|max:255',
    'area_anatomica_lesionada_otro'                 => 'sometimes|string|max:255'  
]);
```

--------------------------------------------------------------------------------

### Editar registro de mujeres en prisión para un hecho de violencia

#### url
/api/edita-mujer-en-prision

#### props

```json
{
    "id" : 1,
    "delitos" : "robo a mano armada editada",
    "calidad_legal_id" : 2
}
```

### PHP rules

```php
$request->validate([
    'id'                    => 'required|exists:mujeres_en_prisions,id',
    'privativa_de_libertad' => 'sometimes|required|boolean',
    'calidad_legal_id'      => 'sometimes|required|exists:catalogo_calidad_legals,id',
    'delitos'               => 'sometimes|nullable|string',
    'victima_de_tortura'    => 'sometimes|nullable|boolean',
    'protocolo_de_estambul' => 'sometimes|nullable|boolean',
    'victima_de_tortura_despues_del_protocolo_de_estambul' => 'sometimes|nullable|boolean',
    'tortura_tipo_id'       => 'sometimes|nullable|exists:catalogo_tortura_en_prision_tipos,id',
    'tortura_momento_id'    => 'sometimes|nullable|exists:catalogo_tortura_en_prision_momentos,id',
    'autoridad_id'          => 'sometimes|nullable|exists:catalogo_autoridades,id',
]);
```

--------------------------------------------------------------------------------

### Editar registro de mujeres víctimas de trata

#### url
/api/edita-mujer-victima-de-trata

#### props

```json
{
   "id" : 1,
   "fines_reclutamiento_id" : 2,
   "accion_omision_dolosa_array" : [2],
   "fines_reclutamiento_array" : [1]
}
```

### PHP rules

```php
$request->validate([
    'id'                            => 'required|exists:mujeres_victimas_de_trata_de_personas,id',
    'victima_de_trata_de_personas'  => 'sometimes|nullable|boolean',
    'accion_omision_dolosa_id'      => 'sometimes|nullable|exists:catalogo_accion_omision_dolosa_con_fines_de_explotacions,id',
    'fines_reclutamiento_id'        => 'sometimes|nullable|exists:catalogo_fines_reclutamientos,id',
    'accion_omision_dolosa_array'   => 'sometimes|nullable|array',
    'accion_omision_dolosa_array.*' => 'sometimes|exists:catalogo_accion_omision_dolosa_con_fines_de_explotacions,id',
    'fines_reclutamiento_array'     => 'sometimes|nullable|array',
    'fines_reclutamiento_array.*'   => 'sometimes|exists:catalogo_fines_reclutamientos,id'
]);
```

--------------------------------------------------------------------------------

### Editar orden de protección

#### url
/api/edita-orden-de-proteccion

#### props

```json
{
    "tipo_id": 2,
    "en_que_consiste": "Descripción de la orden",
    "autoridad_emisora_otro": "Otra autoridad",
    "cve_ent": 15,
    "cve_mun": 1,
    "fecha_de_emision": "2023-10-01",
    "tiempo_indefinido": true,
    "fecha_aproximada_de_termino": "2023-12-31",
    "otro_tipo_de_orden": "Otro tipo de orden",
    "fecha_de_inicio": "2023-10-01",
    "hechos_id": 1,
    "autoridad_id": 1,
    "folio": "ABC123",
    "dia_id": 1,
    "tipo_orden_id": 1,
    "tipo_medida_id": 1,
    "fracciones_tipo_orden": [
        1,
        2
    ],
    "fracciones_tipo_medida": [
        1,
        2
    ],
    "id": 1
}
```

### PHP rules

```php
$request->validate([
    "id"                            => "required|integer|exists:ordenes_de_proteccions,id",
    "tipo_id"                       => "required|integer|exists:catalogo_orden_de_proteccion_tipos,id",
    "en_que_consiste"               => "sometimes|string",
    "autoridad_emisora_otro"        => "sometimes|string",
    "cve_ent"                       => "required|integer|between:1,32",
    'cve_mun'                       => ['sometimes','integer', function($attribute, $value, $fail) use($request){
        $municipio = Municipio::where('cve_ent', '=', $request->cve_ent)->where('cve_mun', '=', $value)->first();
        if (!$municipio) {
            $fail('El municipio no existe');
        }
    }],
    "fecha_de_emision"              => "sometimes|date",
    "tiempo_indefinido"             => "sometimes|boolean",
    "fecha_aproximada_de_termino"   => "sometimes|date",
    "otro_tipo_de_orden"            => "sometimes|string",
    "fecha_de_inicio"               => "sometimes|date",
    "hechos_id"                     => "required|integer|exists:hechos,id",
    "autoridad_id"                  => "sometimes|integer|exists:catalogo_autoridades,id",
    "folio"                         => "sometimes|string|max:255",
    "dia_id"                        => "sometimes|integer|exists:catalogo_dias,id",
    "tipo_orden_id"                 => "sometimes|integer|exists:catalogo_tipos_ordenes_proteccion,id",
    "tipo_medida_id"                => "sometimes|integer|exists:catalogo_tipos_medidas_proteccion,id",
    "fracciones_tipo_orden"         => "sometimes|array",
    "fracciones_tipo_orden.*"       => "integer|exists:catalogo_fracciones_tipos_ordenes_proteccion,id",
    "fracciones_tipo_medida"        => "sometimes|array",
    "fracciones_tipo_medida.*"      => "integer|exists:catalogo_fracciones_tipos_medidas_proteccion,id",
]);
```


## Métodos de prueba

### Eliminar CURP mediante CURP

#### url

/api/delete-curp

#### props

```json
{
    "curp" : "VALID_CURP"
}
```

#### respuesta

```json
{
    "message": "CURP eliminado",
    "folio_euv": "c6833777-82f0-3f40-9f95-b8c7430d484f"
}
```

### PHP Rules

```php
$request->validate([
    'curp' => 'required|exists:victimas,curp'
]);
```

#### tipo de usuario

* cualquiera

--------------------------------------------------------------------------------

### Eliminar CURP mediante Folio EUV

#### url

/api/delete-curp-from-euv

#### props

```json
{
    "folio_euv" : "VALID_EUV"
}
```

#### respuesta

```json
{
    "message": "CURP eliminado",
    "folio_euv": "d080b0fe-bed7-3be6-bf42-1d78881dd6e4"
}
```

### PHP Rules

```php
$request->validate([
    'folio_euv' => 'required|exists:victimas,folio_euv'
]);
```

#### tipo de usuario

* cualquiera

--------------------------------------------------------------------------------

### Restaura el CURP mediante Folio EUV

#### url

/api/restore-curp-from-euv

#### props

```json
{
    "folio_euv" : "VALID_EUV",
    "curp": "UNIQUE_CURP"
}
```

#### respuesta

```json
{
    "message": "CURP restaurado",
    "folio_euv": "d080b0fe-bed7-3be6-bf42-1d78881dd6e4"
}
```

### PHP Rules

```php
$request->validate([
    'folio_euv' => 'required|exists:victimas,folio_euv',
    'curp'      => 'required|unique:victimas,curp'
]);
```

#### tipo de usuario

* cualquiera

--------------------------------------------------------------------------------

## Métodos de consulta personalizados disponibles


### Infoname

#### url

/api/custom/infoname

#### props

```json
{
    "curp": "VVXI811212MCLVXTXX"
}
```


#### respuesta

[ver json](api-response-examples/infoname.json)


### PHP Rules

```php
$request->validate([
    'curp' => 'required|exists:victimas,curp',
    'folio_euv' => 'nullable|exists:victimas,folio_euv'
]);
`````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````

### Actualizar registro de víctima
url

PUT /api/actualizar-registro-victimas/{id}

props
{
    "nombre": "María",
    "primer_apellido": "García",
    "segundo_apellido": "Lopez",
    "fecha_nacimiento": "1990-05-12",
    "cve_ent": 15,
    "nacionalidad_id": 1,
    "extranjera": false,
    "sexo": "F",
    "folio_euv": "ABC123",
    "curp": "GAML900512MJCRLR00",
    "users_id": 41,
    "sexo_id": 2,
    "from_api": true,
    "identifica_mujer": true,
    "cve_ent_captura": 15,
    "cve_mun_captura": 45,
    "dependencia_captura": "Institución X",
    "dependencia_corresponde": "Institución Y",

    "usuario_corresponde": "Nombre Capturista",
    "usuario_cargo_corresponde": "Cargo del Capturista"
}

curl
curl --location --request PUT 'http://localhost/api/actualizar-registro-victimas/1' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
    "nombre": "María",
    "primer_apellido": "García",
    "segundo_apellido": "Lopez",
    "fecha_nacimiento": "1990-05-12",
    "cve_ent": 15,
    "nacionalidad_id": 1,
    "extranjera": false,
    "sexo": "F",
    "folio_euv": "ABC123",
    "curp": "GAML900512MJCRLR00",
    "users_id": 41,

    "usuario_corresponde": "Nombre Capturista",
    "usuario_cargo_corresponde": "Cargo del Capturista"
}'

respuesta
{
    "message": "Datos de la víctima actualizados correctamente",
    "victima": {
        "id": 1,
        "nombre": "María",
        "primer_apellido": "García",
        "segundo_apellido": "Lopez",
        "folio_euv": "ABC123"
    }
}

### Actualizar datos de la víctima
url

PUT /api/actualizar-datos-victima/{id}

props
{
    "edad": 34,
    "telefono": "4491234567",
    "correo_electronico": "correo@example.com",
    "calle": "Av. Principal",
    "num_exterior": "123",
    "num_interior": "4B",
    "cve_ent": 1,
    "cve_mun": 10,
    "codigo_postal": 20000,
    "colonias_id": 15,
    "estado_conyugal": 2,
    "ocupacion_id": 3,
    "presenta_discapacidad": false,
    "enfermedad": true,
    "cual_enfermedad": "Asma",

    "usuario_corresponde": "Nombre Capturista",
    "usuario_cargo_corresponde": "Cargo Capturista"
}

curl
curl --location --request PUT 'http://localhost/api/actualizar-datos-victima/5' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
    "edad": 34,
    "telefono": "4491234567",
    "correo_electronico": "correo@example.com",

    "usuario_corresponde": "Nombre Capturista",
    "usuario_cargo_corresponde": "Cargo Capturista"
}'

respuesta
{
    "message": "Datos de víctima actualizados correctamente",
    "datos_victima": {
        "id": 5,
        "edad": 34,
        "telefono": "4491234567"
    }
}

### Actualizar dependiente de la víctima
url

PUT /api/actualizar-dependiente-victima/{id}

props
{
    "nombre": "Juan",
    "apellido_paterno": "Pérez",
    "apellido_materno": "Gómez",
    "parentesco_id": 3,
    "telefono": "4499876543",
    "correo": "dependiente@example.com",
    "domicilio": "Calle 5, #321",

    "usuario_corresponde": "Nombre Capturista",
    "usuario_cargo_corresponde": "Cargo Capturista"
}

curl
curl --location --request PUT 'http://localhost/api/actualizar-dependiente-victima/10' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
    "nombre": "Juan",
    "apellido_paterno": "Pérez",

    "usuario_corresponde": "Nombre Capturista",
    "usuario_cargo_corresponde": "Cargo Capturista"
}'

respuesta
{
    "message": "Dependiente actualizado correctamente",
    "data": {
        "id": 10,
        "nombre": "Juan",
        "apellido_paterno": "Pérez"
    }
}

### Campos opcionales de captura

En todos los endpoints de registro y actualización, pueden enviarse opcionalmente los campos:

"usuario_corresponde": "Nombre de quien captura",
"usuario_cargo_corresponde": "Cargo de quien captura"


Estos campos no son obligatorios, pero permiten identificar quién realizó la captura o modificación.








