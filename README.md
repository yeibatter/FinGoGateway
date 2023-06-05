# Fingo Gateway Application Api

**Fingo Gateway** Es un api que provee la funcionalidad de conectividad con el Backen de Fingo y MiiD

### Operaciones

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

#### OnBoarding - MiiD 
- [Consulta por Formulario y Documento](#ApiConsultaUsuarioForm)
- [Upload Image](#ApiGatewayMiiDUploadImage)
- [Upload Image Multiple](#ApiGatewayMiiDUploadImageMultiple)
- [Enroll Person](#ApiGatewayMiiDEnrollPersonAsyncGateway)
- [Consulta Estado del Proceso de Enrolamiento](#ApiGatewayMiiDGetResultByProcessId)
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

---
## <a name="ApiConsultaUsuarioForm"></a>Api Gateway MiiD - Consulta por Usuario Y Formulario
Dado el Número de identificacion y el identificador del formulario, procede a consultar si tiene procesos aprobados pendientes por procesar

Url - **POST**
https://servicesdev.fingo.credit/mg/Empresa/ConsultaEstadoSolicitud/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
    "tipoIdentificacionId": 2,
    "numeroDocumento": "002",
    "formId" : "5BEB-376D"
}
```

Variables a enviar:
* **tipoIdentificacionId** : Tipo de Identificación de la Empresa
* **numeroDocumento** : Numero de Documento de la empresa
* **formId** : Identificador del Formulario procesado

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "empresa": {
        "tipoIdentificacionId": 3,
        "numeroDocumento": "002",
        "nombre": "Bug 14196",
        "direccion": "Calle 12   13   13 ",
        "email": "autenticacion2miid@gmail.com",
        "telefono": "3992021",
        "telefonoCel": "3102939334",
        "formId": "5BEB-376D",
        "representante": {
            "id": 113,
            "tipoIdentificacionId": 4,
            "numeroDocumento": "002",
            "primerNombre": "Santiago",
            "segundoNombre": "",
            "primerApellido": "Reyes",
            "segundoApellido": "",
            "fechaCreacion": "2023-05-31T14:14:20-05:00"
        }
    },
    "isValid": true,
    "message": ""
}
```

Variables de respuesta:
* **empresa** : Informacion de la Empresa a procesar
* **representante** : Información del representante legal a quien se le realizará el proceso de MiiD. de aca se deberá obtener el numero y tipo de documento
* **isValid** : *Bandera donde se puede evaluar si el proceso es valido o no para continuar*

--
## <a name="ApiGatewayMiiDUploadImage"></a>Api Gateway MiiD - Upload Image
Realiza la Carga de Imagenes enviadas por el SDK de MiiD

Url - **POST**
https://servicesdev.fingo.credit/mg/miid/UploadImage/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body Type
* **form-data**

Form Variables

* **key** :  Binario - Archivo con la captura a enviar
* **sessionid** :  Session Id (El SDK de MiiD genera este valor)

 Ejemplo de Llamado
```console
curl --location 'https://servicesdev.fingo.credit/mg/miid/UploadImage/' \
--header 'Authorization: Bearer eyJ0eXA...' \
--form 'key=@"/Allan/FP_HuellaPW123.bytte"' \
--form 'sessionid="bytte123"'
```

Ejemplo de Respuesta:
```json
{
    "dataKey": "2d401ca2-5b3a-4a41-82d8-101afd7c1095",
    "successOperation": true
}
``` 

Variables de respuesta:
* **dataKey** :  Identificador del archivo cargado para posterior uso!
* **successOperation** : *Bandera donde se puede evaluar si el proceso es valido o no*
--
## <a name="ApiGatewayMiiDUploadImage"></a>Api Gateway MiiD - Upload Image Multple
Realiza la Carga de multiples imagenes enviadas por el SDK de MiiD en un solo contenedor (un archivo contiene multiples imagenes)

Url - **POST**
https://servicesdev.fingo.credit/mg/miid/UploadImageMultiple/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body Type
* **form-data**

Form Variables

* **key** :  Binario - Archivo con las capturas (El SDK de MiiD genera este insumo)
* **sessionid** :  Session Id (El SDK de MiiD genera este valor)

 Ejemplo de Llamado
```console
curl --location 'https://servicesdev.fingo.credit/mg/miid/UploadImageMultiple/' \
--header 'Authorization: Bearer eyJ0eXA...' \
--form 'key=@"/Allan/FP_HuellaPW123.bytte"' \
--form 'sessionid="bytte123"'
```

Ejemplo de Respuesta (Lista de Elementos) :
```json
[
    {
        "dataKey": "709ec226-9a0d-436d-afe7-3d5245734501",
        "dataName": "FP_20_IMG_31648B36-FFCE-4DF4-9EFD-21C3821A0CF2"
    },
    {
        "dataKey": "384e128b-5574-4100-85dd-7c044b1b4662",
        "dataName": "FP_2_IMG_8F221176-69A9-4DE6-B839-1AF5F7F89302"
    },
    {
        "dataKey": "c4a022e5-ee35-4845-a8ff-4091ed977253",
        "dataName": "FP_4_IMG_76E3C264-B3F6-4C67-99F6-5616B0B3AF09"
    },
    {
        "dataKey": "dd055231-c676-48af-ae80-3e45ab229bf4",
        "dataName": "FP_5_IMG_B396407D-1E85-42D4-814A-38332EA6AF13"
    },
    {
        "dataKey": "3375ab22-1a8a-4a86-aab5-6eb91f06383A",
        "dataName": "FP_3_IMG_92973B20-BCCE-4BA2-B5A4-343C6A965D7C"
    }
]
``` 

Variables de respuesta (por cada elemento):
* **dataKey** : Identificador del archivo cargado para posterior uso!
* **dataName** :  Identificador del tipo de archivo cargado para posterior usio
--

## <a name="ApiGatewayMiiDEnrollPersonAsyncGateway"></a>Api Gateway MiiD - Enroll Person
Realiza el procesamiento de los insumos enviados desde el app para realizar un proceso de enrolamiento en el sistema FinGo usando MiiD

* El llamado es Asincrono, por lo que el servicio no tiene altos tiempos de respuesta
* Para conocer el estado del proceso, se debe realizar una consulta al servicio **GetResultByProcessId**

Url - **POST**
https://servicesdev.fingo.credit/mg/miid/EnrollPersonAsyncGateway/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body Type
* **form-data**
## Opción 1 (Recomendada) - Envio DataKeys en el request

* Los datakeys son obtenidos al realizar la carga de un archivo usando el servicio **UploadFile** o **UploadFileMultiple**

Form Variables
### Textos
* **FaceDataKey** : DataKey del archivo con el contenido del rostro
* **FingerXDataKey** : DataKey del archivo con el contenido del dedo capturado
  X es un valor que puede ser 1 hasta 10 o 20-21  
  !Un registro es generado por cada dedo capturado!  

* **DocumentBackImageDataKey** :  DataKey del archivo con el contenido del reverso del documento
* **DocumentBackImageframeDataKey** : DataKey del archivo con el contenido full frame del reverso del documento
* **DocumentFrontImageDataKey** : DataKey del archivo con el contenido del frente del documento
* **DocumentFrontImageframeDataKey** : DataKey del archivo con el contenido full frame del frente del documento     
* **BarcodeBase64**: Codigo de barras PDF417/MRZ codificado 
* **EmailAddress** : Direccion de correo del cliente final
* **ExternalId** : Identificador unico generado por el dispositivo para posteriores consultas
* **FormId** : Identificador del Formulario autorizado
* **DocumentType** : Tipo de Documento (1=CC, 2=NIT, etc)
* **DocumentNumber** : Numero de Documento

Ejemplo de Llamado
```console
curl --location --request POST 'https://servicesdev.fingo.credit/mg/miid/EnrollPersonAsyncGateway' \
--header 'Authorization: Bearer eyJ0eXAiOiJKV1Qi...' \
--form 'facedatakey="30ce7c1e-2792-48da-a7f3-cc759ead3c66"' \
--form 'finger2datakey="050424f6-a45c-4362-8fd3-2999bc055096"' \
--form 'finger3datakey="44bc4c13-0ead-4346-a752-57d7a6b3931c"' \
--form 'finger4datakey="a0fecd2e-f70f-46f8-85f1-83df3f1f1bde"' \
--form 'finger1datakey="05ed66f7-0c16-4df3-821a-3332af68447d"' \
--form 'finger21datakey="57d7a6b3-00ea-446f-821a-cd759ead3c66"' \
--form 'barcodebase64="MDM1ODAwNz.."' \
--form 'emailaddress="allan.diaz@bytte.com.co"' \
--form 'externalid="11EEA344-111123-2047"' \
--form 'documentbackimagedatakey="14677095-18fc-4b0a-b4ca-37f15d8b5488"' \
--form 'documentfrontimagedatakey="0f702976-8251-4900-965e-6faad6c99c28"' \
--form 'documentbackimageframedatakey="a0fecd2e-f70f-46f8-85f1-83df3f1f1bde"' \
--form 'documentfrontimageframedatakey ="0f702976-8251-4900-965e-6faad6c99c28"' \
--form 'FormId="A341-94400-AB-0157D"' \
--form 'DocumentType="1"' \
--form 'DocumentNumber="80208223"'
```

## Opción 2 (No Recomendada pero posible)- Envio Archivos en el request

Form Variables
### Binarios
* **Face** : Binario - archivo con el contenido del rostro
* **FingerX** : Binario - archivo con el contenido del dedo capturado
  X es un valor que puede ser 1 hasta 10 o 20-21  
  !Un registro es generado por cada dedo capturado!  

* **DocumentBackImage** :  Binario - archivo con el contenido del reverso del documento
* **DocumentBackImageframe** :  Binario - archivo con el contenido full frame del reverso del documento
* **DocumentFrontImage** :  Binario - archivo con el contenido del frente del documento
* **DocumentFrontImageframe** :  Binario - archivo con el contenido full frame del frente del documento   
  
### Textos
* **BarcodeBase64**: Codigo de barras PDF417/MRZ codificado 
* **EmailAddress** : Direccion de correo del cliente final
* **ExternalId** : Identificador unico generado por el dispositivo para posteriores consultas
* **FormId** : Identificador del Formulario autorizado
* **DocumentType** : Tipo de Documento (1=CC, 2=NIT, etc)
* **DocumentNumber** : Numero de Documento

Ejemplo de Llamado
```console
curl --location --request POST 'https://servicesdev.fingo.credit/mg/miid/EnrollPersonAsyncGateway' \
--header 'Authorization: Bearer eyJ0eXAiOiJKV1Qi...' \
--form 'Face=@"//imgSelfie.jpg"' \   
--form 'Finger3=@"//insHuella2.jpg"' \ 
--form 'Finger4=@"//insHuella3.jpg"' \ 
--form 'Finger5=@"//insHuella4.jpg"' \ 
--form 'Finger2=@"//insHuella1.jpg"' \ 
--form 'Finger21=@"//insHuella21.jpg"' \ 
--form 'BarcodeBase64="MDIwMTYyOT..."' \
--form 'EmailAddress="allan.diaz@bytte.com.co"' \
--form 'ExternalId="aa-223344-111123-2003"' \
--form 'DocumentBackImage=@"//reversoCO.jpg"' \
--form 'DocumentFrontImage=@"//frenteCO.png"' \
--form 'DocumentBackImageFrame=@"//reversofullframeCO.jpg"' \
--form 'DocumentFrontImageFrame=@"//frentefullframeCO.png"' \
--form 'FormId="A341-94400-AB-0157D"' \
--form 'DocumentType="1"' \
--form 'DocumentNumber="80208223
```
--
Ejemplo de Respuesta (por cualquiera de las 2 opciones):
```json
{
    "id": "668ccde7-d9ea-4648-ab64-02ec51d41139",
    "processId": "aaf25d98-a43f-4052-98a1-588b11c02d90",  --> Identificador de la transaccion  
    "tipoDocumentoId": 1,
    "numeroDocumento": "51778371",
    "startDate": "2023-06-02T09:34:41",  
    "formId": "A341-94400-AB-0157D",
    "externalId": "aa-223344-111123-2003",
    "mail": "allan.diaz@bytte.com.co",
    "status": 0,
    "inProcess": true
}
``` 

Variables de respuesta (por cada elemento):
* **id** : Identificador interno del sistema Fingo (no se debe usar)
* **processId** :  Identificador de la transaccion, este dato se usara para consultar el estado del proceso
* **tipoDocumentoId** : Tipo de Documento del Cliente enviado
* **numeroDocumento** : Numero de documento del Cliente enviado
* **startDate** : Fecha de Inicio del Proceso
* **formId** : Identificador del Formulario a procesar
* **externalId** : Identificador externo enviado desde el dispositivo
* **mail** : Correo del cliente enviado

* **status** : Estado del proceso
    * *0* : En proceso, hay que esperar
    * *1* : Proceso finalizado 

* **inProcess** : Estado actual del proceso 
    * *true* : En proceso, hay que esperar
    * *false* : Proceso finalizado
    
## <a name="ApiGatewayMiiDGetResultByProcessId"></a>Api Gateway MiiD - Consulta Estado del Proceso de Enrolamiento
Dado los siguientes datos, retorna el estado del enrolamiento enviado a MiiD

* **Tipo de Documento**
* **Numero de Documento**
* **Process Id** (Entregado por el servicio **EnrollPerson**)
* **Identificador del formulario** 

Url - **POST**
https://servicesdev.fingo.credit/mg/miid/GetResultByProcessId/

**Cabeceras a Enviar**
* Se debe enviar el header *Authorization*, con el campo *accessToken* Obtenido en el servicio Login
```sh
--header Authorization": Bearer eyJ0eXAi......."
```

Body
```json
{
    "tipoDocumentoId": 1,
    "numeroDocumento": "51778371",
    "formId": "AAEE-F01DE-001-P099-A01",    
    "processId": "aaf25d98-a43f-4052-98a1-588b11c02d90"
}
```

Variables a enviar:
* **tipoDocumentoId** : Tipo de Documento del Cliente
* **numeroDocumento** : Numero de Documento del Cliente
* **formId** : Identificador del formulario 
* **processId** : Identificador de la transaccion generado por el servicio **EnrollPerson**

Si la informacion ingresada es correcta, el Api Retorna
* **status** : 200

```json
{
    "id": "668ccde7-d9ea-4648-ab64-02ec51d41139",
    "processId": "aaf25d98-a43f-4052-98a1-588b11c02d90",
    "tipoDocumentoId": 1,
    "startDate": "2023-06-02T09:34:41",
    "finishDate": "2023-06-02T09:34:55",
    "formId": "AAEE-F01DE-001-P099-A01",
    "externalId": "11223344-111123-2047aa",
    "mail": "allan.diaz@bytte.com.co",
    "status": 1,
    "statusProcess": true,
    "enrollFinger": 1,
    "enrollFace": 1,
    "statusPagare": 0,
    "statusPrducto": 0,
    "numeroDocumento": "51778371",
    "inProcess": false
}
```

Variables de respuesta:
* **id** : Identificador interno del sistema Fingo (no se debe usar)
* **processId** :  Identificador de la transaccion, este dato se usara para consultar el estado del proceso
* **tipoDocumentoId** : Tipo de Documento del Cliente enviado
* **numeroDocumento** : Numero de documento del Cliente enviado
* **startDate** : Fecha de Inicio del Proceso
* **finishDate** : Fecha de Finalizacion del proceso (si no ha finalizado, no se retorna)
* **formId** : Identificador del Formulario a procesar
* **externalId** : Identificador externo enviado desde el dispositivo
* **mail** : Correo del cliente enviado
* **status** : Estado del proceso
    * *0* : Pendiente
    * *1* : Finalizado

* **inProcess** : Estado actual del proceso 
    * *true* : En proceso, hay que esperar
    * *false* : Proceso finalizado
     
* **statusProcess** : Estado del enrolamiento 
    * *true* : Persona enrolada correctamente
    * *false* : No fué exitos el proceso de enrolamiento

* **enrollFinger** : Estado del proceso de Enrolamiento dactilar
    * *0* : No Enrolado Dactilar
    * *1* : Enrolado Dactilar

* **enrollFace** : Estado del proceso de Enrolamiento facial
    * *0* : No Enrolado Facial
    * *1* : Enrolado Facial

* **statusPagare** : Estado de la generacion de pagaré
    * *0* : No Generado
    * *1* : Generado

* **statusPrducto** : Estado de la generacion del producto
    * *0* : No Generado
    * *1* : Generado
--
