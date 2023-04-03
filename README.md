# Fingo Gateway Application Api

**Fingo Gateway** Es un api que provee la funcionalidad de conectividad con el Backen de Fingo y MiiD

## Operaciones

 - [Authorization - Login](#ApiGatewayAutorization)
 - [Credit Card - Consulta Cliente](#ApiGatewayCreditCardConsultaCliente)
 - [Credit Card - Consulta Producto](#ApiGatewayCreditCardConsultaProducto)

**Fingo Gateway**  es un servicio comercial, para su uso, contacte a  [info@bytte.com.co](mailto:info@bytte.com.co) para obtener instrucciones y credenciales de acceso

**FinGo y MiiD** Son Marcas registradas por Bytte S.A.S 

## Detalle de Operaciones:
## <a name="ApiGatewayAutorization"></a>Api Gateway Autorization

Permite la Autenticacion/Autorizacion del cliente en el sistema Fingo

Url - Login User - **POST**
https://servicesdev.fingo.credit/graph/api/LoginUser/

```json
{
    "documentType": "1",
    "documentNumber": "800195310",
    "password": "7894"
}
```

Variables a enviar:
* **documentType** = Es el Tipo de documento del Cliente FinGo
* **documentNumber** : Es el numero de documento del cliente Fingo
* **password** : Es la Clave asignada previamente por el cliente Fingo

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "accessToken": "eyJhbG....",
    "tokenType": "Bearer",
    "expiresIn": 3600,
    "idToken": "eyJhbGciOiJSUzI1....",
    "documentType": "1",
    "documentNumber": "800195310",
    "givenName": "Allan Yeisson",
    "surname": "Diaz Ballesteros",
    "displayName": "Allan Yeisson Diaz Ballesteros",
    "emailAddress": "allan.diaz1f@gmail.com",
    "nickName": "pacchino2",
    "isValid": true
}
```

Variables de respuesta:
* **Tipo de Respuesta** = **Token Bearer**
* **accessToken** : Token que será usado posteriormente para llamados a otros servicios del Gateway
* **expiresIn** : Tiempo de Vigencia del Token en segundos
* **idToken** : Identificador del Token
* **documentType** : Tipo de Documento del Cliente autenticado
* **documentNumber** : Número de Documento del Cliente autenticado
* **givenName** : Nombres del Cliente
* **surname** : Apellidos del Cliente
* **displayName** : Nombre completo del Cliente
* **emailAddress** : Direccion de correo del Cliente
* **nickName** : NickName asignado por el Cliente
* **isValid** : *Bandera donde se puede evaluar si el cliente es valido o no*

Si la informacion ingresada No es correcta, el Api Retorna
* **status** : 401
```json
{
    "isValid": false,
    "message": "AADB2C90225: The username or password provided in the request are invalid. Correlation ID: e3147abd-8b47-4701-834b-9ccb2483c6b7 Timestamp: 2023-04-03 15:09:00Z ",
    "errorMessage": "access_denied"
}
```
Variables de respuesta:
* **errorMessage** : Descripcion del Error presentado
* **message** : Mensaje del Error presentado
* **isValid** : *Bandera donde se puede evaluar si el cliente es valido o no*


---
## <a name="ApiGatewayCreditCardConsultaCliente"></a>Api Gateway Credit Card - ConsultaCliente
Dado el Tipo de documento y Numero de documento del cliente autenticado, 
responde si este posee productos y retorna su descripción

Url - Consulta Cliente - **POST**

https://servicesdev.fingo.credit/mg/CreditCard/ConsultaCliente/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
    "numeroDocumento": "800195310",
    "tipoDocumento": 1
}
```

Variables a enviar:
* **documentType** = Es el Tipo de documento del Cliente FinGo
* **documentNumber** : Es el numero de documento del cliente Fingo

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "cliente": {
        "tipoIdentificacionId": 1,
        "tipoPersonaId": 2,
        "codigoZonaId": 1,
        "tipoEstadoCivilId": 1,
        "numeroDocumento": "802082126",
        "primerNombre": "Allan",
        "primerApellido": "Diaz",
        "telefonoMovil": "3016336165",
        "indicativo": "1",
        "email": "ogva_tjwvl83@jyplo.com",
        "direccionCorrespondencia1": "carrera 33 n 22 - 11",
        "cupoAsignado": 1000000,
        "nombreCorto": "Allan Diaz",
        "nombreRealce": "Allan Diaz",
        "telefonoCasa": "0",
        "telefonoOficina": "0",
        "actividadEconomica": "0090",
        "tipoEstadoCivil": {
            "id": 1,
            "descripcion": "SOLTERO"
        },
        "tipoIdentificacion": {
            "id": 1,
            "descripcion": "CEDULA DE CIUDADANIA",
            "descripcionCorta": "CC"
        },
        "tipoPersona": {
            "id": 2,
            "descripcion": "NATURAL",
            "descripcionCorta": "N"
        },
        "codigoZona": {
            "id": 1,
            "descripcion": "ZONA UNO"
        },
        "vendedor": {
            "id": 1,
            "nombre": "VENDEDOR UNICO",
            "numeroDocumento": "000001"
        }
    },
    "isValid": true,
    "message": "",
    "errorCode": "0",
    "errorMessage": ""
}
```

Variables de respuesta:
* **cliente** = **Objeto Cliente con la información relacionada a este**
* **message** : Mensaje en caso de algun error
* **errorMessage** : Mensaje de error en caso de existir
* **errorCode** : Codigo de Error en caso de existir
* **isValid** : *Bandera donde se puede evaluar si el cliente es valido o no*

Si el cliente no existe, el Api Retorna
* **status** : 200
```json
{    
    "isValid": false,
    "message": "No existe el cliente con documento 8020821264",
    "errorMessage": "No existe el cliente con documento 8020821264"
}
```
Variables de respuesta:
* **errorMessage** : Descripcion del Error presentado
* **message** : Mensaje del Error presentado
* **isValid** : *Bandera donde se puede evaluar si el cliente es valido o no*


---
## <a name="ApiGatewayCreditCardConsultaProducto"></a>Api Gateway Credit Card - Consulta Producto
Dado el Tipo de documento y Numero de documento del cliente autenticado, 
responde si este posee productos y retorna su descripción

Url - Consulta Producto - **POST**

https://servicesdev.fingo.credit/mg/CreditCard/ConsultaProductos/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
    "numeroDocumento": "800195310",
    "tipoDocumento": 1
}
```

Variables a enviar:
* **documentType** = Es el Tipo de documento del Cliente FinGo
* **documentNumber** : Es el numero de documento del cliente Fingo

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "cliente": {
        "tipoIdentificacionId": 1,
        "tipoPersonaId": 2,
        "codigoZonaId": 1,
        "tipoEstadoCivilId": 1,
        "numeroDocumento": "802082126",
        "primerNombre": "Allan",
        "primerApellido": "Diaz",
        "telefonoMovil": "3016336165",
        "indicativo": "1",
        "email": "ogva_tjwvl83@jyplo.com",
        "direccionCorrespondencia1": "carrera 33 n 22 - 11",
        "cupoAsignado": 1000000,
        "nombreCorto": "Allan Diaz",
        "nombreRealce": "Allan Diaz",
        "telefonoCasa": "0",
        "telefonoOficina": "0",
        "actividadEconomica": "0090",
        "tipoEstadoCivil": {
            "id": 1,
            "descripcion": "SOLTERO"
        },
        "tipoIdentificacion": {
            "id": 1,
            "descripcion": "CEDULA DE CIUDADANIA",
            "descripcionCorta": "CC"
        },
        "tipoPersona": {
            "id": 2,
            "descripcion": "NATURAL",
            "descripcionCorta": "N"
        },
        "codigoZona": {
            "id": 1,
            "descripcion": "ZONA UNO"
        },
        "vendedor": {
            "id": 1,
            "nombre": "VENDEDOR UNICO",
            "numeroDocumento": "000001"
        }
    },
    "isValid": true,
    "message": "",
    "errorCode": "0",
    "errorMessage": ""
}
```

Variables de respuesta:
* **cliente** = **Objeto Cliente con la información relacionada a este**
* **message** : Mensaje en caso de algun error
* **errorMessage** : Mensaje de error en caso de existir
* **errorCode** : Codigo de Error en caso de existir
* **isValid** : *Bandera donde se puede evaluar si el cliente es valido o no*

Si el cliente no existe, el Api Retorna
* **status** : 200
```json
{    
    "isValid": false,
    "message": "No existe el cliente con documento 8020821264",
    "errorMessage": "No existe el cliente con documento 8020821264"
}
```
Variables de respuesta:
* **errorMessage** : Descripcion del Error presentado
* **message** : Mensaje del Error presentado
* **isValid** : *Bandera donde se puede evaluar si el cliente es valido o no*
