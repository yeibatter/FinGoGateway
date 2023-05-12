# Fingo Gateway Application Api

**Fingo Gateway** Es un api que provee la funcionalidad de conectividad con el Backen de Fingo y MiiD

## Operaciones

 - [Authorization - Login](#ApiGatewayAutorization)
 - [Credit Card - Consulta Cliente](#ApiGatewayCreditCardConsultaCliente)
 - [Credit Card - Consulta Producto](#ApiGatewayCreditCardConsultaProducto)
 - [Bloqueo Temporal Tarjeta](#ApiGatewayCreditCardDesactivarTarjeta)
 - [Activacion Bloqueo Tarjeta Temporal](#ApiGatewayCreditCardActivarTarjeta)
 - [Bloqueo Tarjeta Definitivo](#ApiGatewayCreditCardBloqueoDefinitivo)
 - [Decifrar Numero de Tarjeta](#ApiGatewayCreditCardDecifrarTarjeta)

#### Gestion del Usuario
- [Cambio de Contraseña](#ApiGatewayCreditCardUserUpdatePassword)
- [Cambio de Correo](#ApiGatewayCreditCardUserUpdateMail)
- [Cambio de Telefono](#ApiGatewayCreditCardUserUpdatePhone)

---
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
    "productos": [
        {
            "bin": "41464713",
            "tarjeta": "AE667957EA7F9356A91D9783A308EB7A",
            "tarjetaNumero": "3509",
            "cvv": "0000",
            "fechaVencimiento": "1900-01-01",
            "estado1": "O-O PENDIENTE ENTREGA",
            "saldo": 0,
            "cupoTotalAprobado": 2000000,
            "cupoDisponible": 2000000,
            "cupoDisponibleAvance": 2000000,
            "fechaCorte": "2023-10-14T19:00:00-05:00",
            "fechaLimitePago": "2023-10-30T19:00:00-05:00",
            "pagoMinimo": 0,
            "valorUltimoPago": 0,
            "diasMora": 0,
            "valorUltimaMora": 0,
            "saldoUltimoCorte": 0,
            "tipoTarjeta": "TARJETA PERSONAL",
            "codigoTipoTarjeta": "1",
            "afinidad": "47"
        }
    ],
    "isValid": true,
    "message": " - ",
    "errorCode": "00",
    "errorMessage": "OPERACION EXITOSA"
}
```

Variables de respuesta:
* **Productos** = **Objeto Productos con la información relacionada a este**
* **message** : Mensaje en caso de algun error
* **errorMessage** : Mensaje de error en caso de existir
* **errorCode** : Codigo de Error en caso de existir
* **isValid** : *Bandera donde se puede evaluar si la consulta es valida o no*

Si el cliente no tiene productos, el Api Retorna
* **status** : 200
```json
{    
    "isValid": false,    
    "errorMessage": "No existe el cliente con documento 8020821264"
}
```
Variables de respuesta:
* **errorMessage** : Descripcion del Error presentado
* **message** : Mensaje del Error presentado
* **isValid** : *Bandera donde se puede evaluar si el cliente es valido o no*

---
## <a name="ApiGatewayCreditCardConsultaMovimientos"></a>Api Gateway Credit Card - Consulta Movimientos
Dado el Número de tarjeta y un rango de fechas, retorna los movimientos de la tarjeta seleccionada
Responde el listado de Movimientos

Url - Consulta Producto - **POST**

https://servicesdev.fingo.credit/mg/CreditCard/ConsultaMovimientos/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
  "tarjetaNumero": "AE667957EA7F9356AA21F89B0DF29910",
  "fechaInicial": "2022-11-08T13:14:41.105Z",
  "fechaFinal": "2023-03-08T13:14:41.105Z"
}
```

Variables a enviar:
* **tarjetaNumero** : Número de tarjeta obtenido del arreglo retornado en el servicio **Consulta Productos**
* **fechaInicial** : Es la Fecha Inicial del rango de busqueda de movimientos
* **fechaFinal** : Es la Fecha final del rango de busqueda de movimientos

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "transacciones": [
        {
            "fechaMovimiento": "2022-11-03",
            "numeroAutorizacion": "817063",
            "descripcion": "COMPRA",
            "valor": "0013438900",
            "plazo": "00",
            "cuotasPendientes": "00",
            "saldo": "0013438900",
            "tasa": "0",
            "numeroComprobante": "0",
            "codigoEstadoMovimiento": "L",
            "estadoMovimiento": "SIN FACTURAR"
        },
        {
            "fechaMovimiento": "2022-11-01",
            "numeroAutorizacion": "0",
            "descripcion": "10 COMISION AVANCES",
            "valor": "0000650000",
            "plazo": "1",
            "cuotasPendientes": "1",
            "saldo": "0000000000",
            "tasa": "0",
            "numeroComprobante": "19640",
            "codigoEstadoMovimiento": "L",
            "estadoMovimiento": "SIN FACTURAR"
        },
        {
            "fechaMovimiento": "2022-11-01",
            "numeroAutorizacion": "628433",
            "descripcion": "AVANCE NACIONAL",
            "valor": "0004000000",
            "plazo": "60",
            "cuotasPendientes": "60",
            "saldo": "0003910000",
            "tasa": "241",
            "numeroComprobante": "19654",
            "codigoEstadoMovimiento": "L",
            "estadoMovimiento": "SIN FACTURAR",
            "fillerTrn1": "OFICINA ESTANDAR"
        },
        {
            "fechaMovimiento": "2022-11-02",
            "numeroAutorizacion": "0",
            "descripcion": "PAGO RECIBIDO",
            "valor": "0092000000",
            "plazo": "1",
            "cuotasPendientes": "1",
            "saldo": "0000000000",
            "tasa": "0",
            "numeroComprobante": "1",
            "codigoEstadoMovimiento": "L",
            "estadoMovimiento": "SIN FACTURAR",
            "fillerTrn1": "OFCINA VIRTUAL IDPAY"
        },
        {
            "fechaMovimiento": "2022-11-02",
            "numeroAutorizacion": "393855",
            "descripcion": "VENTA NACIONAL",
            "valor": "0002300000",
            "plazo": "1",
            "cuotasPendientes": "1",
            "saldo": "0002300000",
            "tasa": "0",
            "numeroComprobante": "4189",
            "nombreEstablecimiento": "PONQUES CASCABEL",
            "codigoEstadoMovimiento": "L",
            "estadoMovimiento": "SIN FACTURAR"
        },
        {
            "fechaMovimiento": "2022-11-01",
            "numeroAutorizacion": "702513",
            "descripcion": "VENTA NACIONAL",
            "valor": "0005244400",
            "plazo": "1",
            "cuotasPendientes": "1",
            "saldo": "0005244400",
            "tasa": "0",
            "numeroComprobante": "2161",
            "nombreEstablecimiento": "ATELIER DE LA PLAZA",
            "codigoEstadoMovimiento": "L",
            "estadoMovimiento": "SIN FACTURAR"
        }
    ],
    "isValid": true,
    "message": "-",
    "errorCode": "00",
    "errorMessage": "OPERACION EXITOSA"
}
```

Variables de respuesta:
* **Movimientos** = **Objeto Movimientos con la información relacionada a este**
* **message** : Mensaje en caso de algun error
* **errorMessage** : Mensaje de error en caso de existir
* **errorCode** : Codigo de Error en caso de existir
* **isValid** : *Bandera donde se puede evaluar si la consulta es valida o no*

---
--- 
## <a name="ApiGatewayCreditCardDesactivarTarjeta"></a>Api Gateway Credit Card - Bloqueo Tarjeta Temporal
Dado el Número de tarjeta, se realiza el proceso de bloqueo Temporal de la tarjeta

Url - Desactivar Tarjeta/Producto - **POST**

https://servicesdev.fingo.credit/mg/CreditCard/DesactivarTarjeta/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
  "tarjetaNumero": "AE667957EA7F9356044FF44FD69DA973"
}
```

Variables a enviar:
* **tarjetaNumero** : Número de tarjeta obtenido del arreglo retornado en el servicio **Consulta Productos**

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "isValid": true,
    "message": "TRANSACCION EXITOSA"
}
```

Variables de respuesta:
* **message** : Mensaje en caso de algun error
* **isValid** : *Bandera donde se puede evaluar si el proceso es valido o no*
---

## <a name="ApiGatewayCreditCardActivarTarjeta"></a>Api Gateway Credit Card - Activacion Bloqueo Tarjeta Temporal
Dado el Número de tarjeta, se realiza el proceso de activacion de la tarjeta dado por un bloqueo temporal

Url - Desactivar Tarjeta/Producto - **POST**

https://servicesdev.fingo.credit/mg/CreditCard/ActivarTarjeta/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
  "tarjetaNumero": "AE667957EA7F9356044FF44FD69DA973"
}
```

Variables a enviar:
* **tarjetaNumero** : Número de tarjeta obtenido del arreglo retornado en el servicio **Consulta Productos**

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "isValid": true,
    "message": "TRANSACCION EXITOSA"
}
```

Variables de respuesta:
* **message** : Mensaje en caso de algun error
* **isValid** : *Bandera donde se puede evaluar si el proceso es valido o no*
---

## <a name="ApiGatewayCreditCardBloqueoDefinitivo"></a>Api Gateway Credit Card - Bloqueo Definitivo
Dado el Número de tarjeta, se realiza el proceso de Bloqueo definitivo
Este proceso no se puede deshacer y genera la reexpedicion de una nueva tarjeta

Url - Bloqueo definitivo Tarjeta/Producto - **POST**

https://servicesdev.fingo.credit/mg/CreditCard/BloqueoDefinitivo/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
  "tarjetaNumero": "AE667957EA7F9356044FF44FD69DA973"
}
```

Variables a enviar:
* **tarjetaNumero** : Número de tarjeta obtenido del arreglo retornado en el servicio **Consulta Productos**

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "isValid": true,
    "message": "TRANSACCION EXITOSA"
}
```

Variables de respuesta:
* **message** : Mensaje en caso de algun error
* **isValid** : *Bandera donde se puede evaluar si el proceso es valido o no*

---
## <a name="ApiGatewayCreditCardDecifrarTarjeta"></a>Api Gateway - Decifrar Tarjeta
Dado el Número de tarjeta Cifrado, Retorna el numero de Tarjeta en claro

Url - Desactivar Tarjeta/Producto - **POST**

https://servicesdev.fingo.credit/mg/CreditCard/DecifrarTarjeta/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
  "tarjetaNumero": "AE667957EA7F9356044FF44FD69DA973"
}
```

Variables a enviar:
* **tarjetaNumero** : Número de tarjeta cifrado obtenido del arreglo retornado en el servicio **Consulta Productos**

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "numero": "4146********1234",
    "isValid": true
}
```

Variables de respuesta:
* **numero** : Número de tarjeta con mascara
* **message** : Mensaje en caso de algun error
* **isValid** : *Bandera donde se puede evaluar si el proceso es valido o no*
---

---
## <a name="ApiGatewayCreditCardUserUpdatePassword"></a>Api Gateway User - Cambio de Contraseña
Dado el Número de identificacion y el Tipo, procede a realizar el cambio de contraseña

Url - **POST**
https://servicesdev.fingo.credit/Graph/api/UpdatePassword/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
    "documentType": "1",
    "documentNumber": "41683015",
    "password": "6987"
}
```

Variables a enviar:
* **documentType** : Tipo de Documento del Cliente
* **documentNumber** : Numero de Documento del Cliente
* **password** : Nuevo Password

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{    
    "isValid": true,
    "message": "Update user - ID: 'd8d66755-ac5f-416d-863f-05da6c9ea45a' OK"
}
```

Variables de respuesta:
* **message** : Mensaje en caso de algun error
* **isValid** : *Bandera donde se puede evaluar si el proceso es valido o no*
--

## <a name="ApiGatewayCreditCardUserUpdateMail"></a>Api Gateway User - Cambio de Correo
Dado el Número de identificacion y el Tipo, procede a realizar el cambio de correo electronico

Url - **POST**
https://servicesdev.fingo.credit/Graph/api/UpdateMail/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
  "documentType": "1",  
  "documentNumber": "51383518",  
  "emailAddress": "cliente.fingo@gmail.com"  
}
```

Variables a enviar:
* **documentType** : Tipo de Documento del Cliente
* **documentNumber** : Numero de Documento del Cliente
* **emailAddress** : Nuevo Correo electronico del cliente

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{    
    "isValid": true,
    "message": "Update user - ID: 'd8d66755-ac5f-416d-863f-05da6c9ea45a' OK"
}
```

Variables de respuesta:
* **message** : Mensaje en caso de algun error
* **isValid** : *Bandera donde se puede evaluar si el proceso es valido o no*
--
## <a name="ApiGatewayCreditCardUserUpdatePhone"></a>Api Gateway User - Cambio de Telefono
Dado el Número de identificacion y el Tipo, procede a realizar el cambio de Telefono

Url - **POST**
https://servicesdev.fingo.credit/Graph/api/UpdatePhoneNumber/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
  "documentType": "1",
  "documentNumber": "41683015",  
  "countrycode" : "57",
  "phonenumber" : "3105626067"  
}
```

Variables a enviar:
* **documentType** : Tipo de Documento del Cliente
* **documentNumber** : Numero de Documento del Cliente
* **countrycode** : Codigo del Pais del telefono del cliente
* **phonenumber** : Nuevo Numero de telefono del cliente

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{    
    "isValid": true,
    "message": "Update user - ID: 'd8d66755-ac5f-416d-863f-05da6c9ea45a' OK"
}
```

Variables de respuesta:
* **message** : Mensaje en caso de algun error
* **isValid** : *Bandera donde se puede evaluar si el proceso es valido o no*
