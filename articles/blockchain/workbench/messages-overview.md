---
title: Introducción a la integración de los mensajes de Azure Blockchain Workbench
description: Introducción al uso de mensajes en Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 11/12/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: f8f3584475415cf9ca19458f6da78d34df37f438
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614368"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>Integración de los mensajes de Azure Blockchain Workbench

Además de proporcionar una API REST, Azure Blockchain Workbench proporciona integración basada en mensajería. Workbench publica eventos centrados en el libro de contabilidad a través de Azure Event Grid, lo que permite a los consumidores de nivel final ingerir datos o realizar acciones en función de estos eventos. Para aquellos clientes que precisan de mensajería de confianza, Azure Blockchain Workbench también entrega mensajes a un punto de conexión de Azure Service Bus.

## <a name="input-apis"></a>API de entrada

Si quiere iniciar las transacciones desde sistemas externos para crear usuarios, crear contratos y actualizar contratos, puede usar las API de entrada de mensajería para realizar transacciones en un libro de contabilidad. Consulte los [ejemplos de integración de mensajería](https://aka.ms/blockchain-workbench-integration-sample) para obtener ejemplos que muestren las API de entrada.

Las siguientes son las API de entrada actualmente disponibles.

### <a name="create-user"></a>Crear usuario

Crea un nuevo usuario.

La solicitud requiere los siguientes campos:

| **Nombre**             | **Descripción**                                      |
|----------------------|------------------------------------------------------|
| requestId            | GUID del cliente proporcionado                                |
| firstName            | Nombre del usuario                              |
| lastName             | Apellidos del usuario                               |
| emailAddress         | Dirección de correo electrónico del usuario                           |
| externalId           | Identificador del objeto de Azure AD del usuario                      |
| connectionId         | Identificador único de la conexión de cadena de bloques |
| messageSchemaVersion | Versión del esquema de mensajería                            |
| messageName          | **CreateUserRequest**                               |

Ejemplo:

``` json
{
    "requestId": "e2264523-6147-41fc-bbbb-edba8e44562d",
    "firstName": "Ali",
    "lastName": "Alio",
    "emailAddress": "aa@contoso.com",
    "externalId": "6a9b7f65-ffff-442f-b3b8-58a35abd1bcd",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateUserRequest"
}
```

Blockchain Workbench devuelve una respuesta con los siguientes campos:

| **Nombre**              | **Descripción**                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------|
| requestId             | GUID del cliente proporcionado |
| userId                | Identificador del usuario que se ha creado |
| userChainIdentifier   | Dirección del usuario que se ha creado en la red de la cadena de bloques. En Ethereum, esta es la dirección del usuario  **en la cadena** . |
| connectionId          | Identificador único de la conexión de cadena de bloques|
| messageSchemaVersion  | Versión del esquema de mensajería |
| messageName           | **CreateUserUpdate** |
| status                | Estado de la solicitud de creación de usuario.  Si se crea correctamente, el valor es **Correcto**. En caso de error, el valor es **Error**.     |
| additionalInformation | Proporciona información adicional en función del estado |

Ejemplo de una respuesta correcta de **creación de usuario** desde Blockchain Workbench:

``` json
{ 
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Success", 
    "additionalInformation": { } 
} 
```

Si la solicitud no se realizó correctamente, se incluyen detalles sobre el error en la información adicional.

``` json
{
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": null, 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Failure", 
    "additionalInformation": { 
        "errorCode": 4000, 
        "errorMessage": "User cannot be provisioned on connection." 
    }
}
```

### <a name="create-contract"></a>Crear contrato

Crea un nuevo contrato.

La solicitud requiere los siguientes campos:

| **Nombre**             | **Descripción**                                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId            | GUID del cliente proporcionado |
| userChainIdentifier  | Dirección del usuario que se ha creado en la red de la cadena de bloques. En Ethereum, es la dirección del usuario **en la cadena**. |
| applicationName      | Nombre de la aplicación |
| workflowName         | Nombre del flujo de trabajo |
| parameters           | Entrada de parámetros para la creación de contratos |
| connectionId         | Identificador único de la conexión de cadena de bloques |
| messageSchemaVersion | Versión del esquema de mensajería |
| messageName          | **CreateContractRequest** |

Ejemplo:

``` json
{ 
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211", 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "applicationName": "AssetTransfer", 
    "workflowName": "AssetTransfer", 
    "parameters": [ 
        { 
            "name": "description", 
            "value": "a 1969 dodge charger" 
        }, 
        { 
            "name": "price", 
            "value": "12345" 
        } 
    ], 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateContractRequest" 
}
```

Blockchain Workbench devuelve una respuesta con los siguientes campos:

| **Nombre**                 | **Descripción**                                                                   |
|--------------------------|-----------------------------------------------------------------------------------|
| requestId                | GUID del cliente proporcionado                                                             |
| contractId               | Identificador único del contrato en Azure Blockchain Workbench |
| contractLedgerIdentifier | Dirección del contrato en el libro de contabilidad                                            |
| connectionId             | Identificador único de la conexión de cadena de bloques                               |
| messageSchemaVersion     | Versión del esquema de mensajería                                                         |
| messageName              | **CreateContractUpdate**                                                      |
| status                   | Estado de la solicitud de creación de contrato.  Valores posibles: **Enviado**, **Confirmado** y **Error**.  |
| additionalInformation    | Proporciona información adicional en función del estado.                              |

Ejemplo de una respuesta de **crear contrato** enviada desde Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Submitted"
    "additionalInformation": { }
}
```

Ejemplo de una respuesta de **crear contrato** confirmada desde Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

Si la solicitud no se realizó correctamente, se incluyen detalles sobre el error en la información adicional.

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": null,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract cannot be provisioned on connection."
    }
}
```

### <a name="create-contract-action"></a>Crear acción de contrato

Crea una nueva acción de contrato.

La solicitud requiere los siguientes campos:

| **Nombre**                 | **Descripción**                                                                                                           |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId                | GUID del cliente proporcionado |
| userChainIdentifier      | Dirección del usuario que se ha creado en la red de la cadena de bloques. En Ethereum, es la dirección del usuario **en la cadena**. |
| contractLedgerIdentifier | Dirección del contrato en el libro de contabilidad |
| workflowFunctionName     | Nombre de la función de flujo de trabajo |
| parameters               | Entrada de parámetros para la creación de contratos |
| connectionId             | Identificador único de la conexión de cadena de bloques |
| messageSchemaVersion     | Versión del esquema de mensajería |
| messageName              | **CreateContractActionRequest** |

Ejemplo:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398",
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "workflowFunctionName": "modify",
    "parameters": [
        {
            "name": "description",
            "value": "a 1969 dodge charger"
        },
        {
            "name": "price",
            "value": "12345"
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionRequest"
}
```

Blockchain Workbench devuelve una respuesta con los siguientes campos:

| **Nombre**              | **Descripción**                                                                   |
|-----------------------|-----------------------------------------------------------------------------------|
| requestId             | GUID del cliente proporcionado|
| contractId            | Identificador único del contrato en Azure Blockchain Workbench |
| connectionId          | Identificador único de la conexión de cadena de bloques |
| messageSchemaVersion  | Versión del esquema de mensajería |
| messageName           | **CreateContractActionUpdate** |
| status                | Estado de la solicitud de acción de contrato. Valores posibles: **Enviado**, **Confirmado** y **Error**.                         |
| additionalInformation | Proporciona información adicional en función del estado. |

Ejemplo de una respuesta de **crear acción de contrato** enviada desde Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Ejemplo de una respuesta de **crear acción de contrato** confirmada desde Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Committed"
    "additionalInformation": { }
}
```

Si la solicitud no se realizó correctamente, se incluyen detalles sobre el error en la información adicional.

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract action cannot be provisioned on connection."
    }
}
```

### <a name="input-api-error-codes-and-messages"></a>Mensajes y códigos de error de la API de entrada

**Código de error 4000: error de solicitud incorrecta**
- ConnectionId no válido
- Error de deserialización de CreateUserRequest
- Error de deserialización de CreateContractRequest
- Error de deserialización de CreateContractActionRequest
- La aplicación {identificada por el nombre de la aplicación} no existe.
- La aplicación {identificada por el nombre de la aplicación} no tiene ningún flujo de trabajo.
- UserChainIdentifier no existe.
- El contrato {identificado por el identificador del libro de contabilidad} no existe.
- El contrato {identificado por el identificador del libro de contabilidad} no tiene la función {nombre de la función del flujo de trabajo}.
- UserChainIdentifier no existe.

**Código de error 4090: error de conflicto**
- El usuario ya existe.
- El contrato ya existe.
- La acción de contrato ya existe.

**Código de error 5000: error interno del servidor**
- Mensajes de excepción

## <a name="event-notifications"></a>Notificaciones de eventos

Las notificaciones de eventos se pueden usar para notificar a los usuarios y a los sistemas de nivel final de eventos que se producen en Blockchain Workbench y la red de cadena de bloques a la que está conectada. Las notificaciones de eventos se pueden consumir directamente en código o se pueden utilizar con herramientas como Logic Apps y Flow para desencadenar el flujo de datos hacia los sistemas de nivel final.

Consulte la [Referencia de mensajes de notificación](#notification-message-reference) para más detalles sobre los distintos mensajes que se pueden recibir.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Consumo de eventos de Event Grid con Azure Functions

Si un usuario desea usar Event Grid para recibir notificaciones sobre eventos que se producen en Blockchain Workbench, puede consumir eventos de Event Grid mediante el uso de Azure Functions.

1. Cree una **aplicación de función de Azure** en Azure Portal.
2. Cree una nueva función.
3. Busque la plantilla de Event Grid. Se muestra el código de la plantilla básica para leer el mensaje. Modifique el código según sea necesario.
4. Guarde la función. 
5. Seleccione la instancia de Event Grid desde el grupo de recursos de Blockchain Workbench.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Consumo de eventos de Event Grid con Logic Apps

1.  Cree una nueva **aplicación lógica de Azure** en Azure Portal.
2.  Al abrir la aplicación de la lógica de Azure en el portal, se le pedirá seleccionar un desencadenador. Seleccione **Azure Event Grid: cuando se produce un evento de recurso**.
3. Cuando se muestra el diseñador de flujo de trabajo, se le pedirá que inicie sesión.
4. Seleccione la suscripción. Recurso como **Microsoft.EventGrid.Topics**. Seleccione el **Nombre de recurso** desde el nombre del recurso del grupo de recursos de Azure Blockchain Workbench.
5. Seleccione la instancia de Event Grid desde el grupo de recursos de Blockchain Workbench.

## <a name="using-service-bus-topics-for-notifications"></a>Uso de temas de Service Bus para notificaciones

Los temas de Service Bus se pueden utilizar para notificar a los usuarios acerca de los eventos que se producen en Blockchain Workbench. 

1.  Vaya a Service Bus en el grupo de recursos de Workbench.
2.  Seleccione **Temas**.
3.  Seleccione **workbench-external**.
4.  Cree una nueva suscripción a este tema. Obtenga una clave para la suscripción.
5.  Cree un programa, que se suscribe a los eventos de esta suscripción.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Consumo de mensajes de Service Bus con Logic Apps

1. Cree una nueva **aplicación lógica de Azure** en Azure Portal.
2. Al abrir la aplicación de la lógica de Azure en el portal, se le pedirá seleccionar un desencadenador. Escriba **Service Bus** en el cuadro de búsqueda y seleccione el desencadenador adecuado para el tipo de interacción que desea tener con Service Bus. Por ejemplo, **Service Bus: cuando se recibe un mensaje en una suscripción de tema (autocompletar)**.
3. Cuando se muestra el diseñador de flujo de trabajo, especifique la información de conexión para Service Bus.
4. Seleccione la suscripción y especifique el tema **workbench-external**.
5. Desarrolle la lógica de la aplicación que utiliza el mensaje de este desencadenador.

## <a name="notification-message-reference"></a>Referencia de mensajes de notificación

En función del valor de **OperationName**, los mensajes de notificación tienen uno de los siguientes tipos de mensaje.

### <a name="accountcreated"></a>AccountCreated

Indica que se ha solicitado agregar una nueva cuenta a la cadena especificada.

| NOMBRE    | DESCRIPCIÓN  |
|----------|--------------|
| UserId  | Identificador del usuario que se ha creado. |
| ChainIdentifier | Dirección del usuario que se ha creado en la red de la cadena de bloques. En Ethereum, esto sería la dirección del usuario **en la cadena**. |

``` csharp
public class NewAccountRequest : MessageModelBase
{
  public int UserID { get; set; }
  public string ChainIdentifier { get; set; }
}
```

### <a name="contractinsertedorupdated"></a>ContractInsertedOrUpdated

Indica que se ha realizado una solicitud para insertar o actualizar un contrato en un libro de contabilidad distribuida.

| NOMBRE | DESCRIPCIÓN |
|-----|--------------|
| ChainID | Identificador único de la cadena asociada a la solicitud |
| BlockId | Identificador único de un bloque en el libro de contabilidad |
| ContractId | Identificador único del contrato |
| ContractAddress |       Dirección del contrato en el libro de contabilidad |
| TransactionHash  |     Hash de la transacción en el libro de contabilidad |
| OriginatingAddress |   Dirección del remitente de la transacción |
| ActionName       |     Nombre de la acción |
| IsUpdate        |      Identifica si se trata de una actualización. |
| Parámetros       |     Lista de objetos que identifican el nombre, valor y tipo de datos de los parámetros que se envían a una acción |
| TopLevelInputParams |  En escenarios en los que un contrato está conectado a uno o más contratos, estos son los parámetros del contrato de nivel superior. |

``` csharp
public class ContractInsertOrUpdateRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public int ContractId { get; set; }
    public string ContractAddress { get; set; }
    public string TransactionHash { get; set; }
    public string OriginatingAddress { get; set; }
    public string ActionName { get; set; }
    public bool IsUpdate { get; set; }
    public List<ContractProperty> Parameters { get; set; }
    public bool IsTopLevelUpdate { get; set; }
    public List<ContractInputParameter> TopLevelInputParams { get; set; }
}
```

#### <a name="updatecontractaction"></a>UpdateContractAction

Indica que se ha realizado una solicitud para ejecutar una acción en un contrato específico en un libro de contabilidad distribuida.

| NOMBRE                     | DESCRIPCIÓN                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContractActionId         | Identificador único de esta acción de contrato |
| ChainIdentifier          | Identificador único de la cadena |
| ConnectionId             | Identificador único de la conexión |
| UserChainIdentifier      | Dirección del usuario que se ha creado en la red de la cadena de bloques. En Ethereum, es la dirección del usuario **en la cadena**. |
| ContractLedgerIdentifier | Dirección del contrato en el libro de contabilidad |
| WorkflowFunctionName     | Nombre de la función de flujo de trabajo |
| WorkflowName             | Nombre del flujo de trabajo |
| WorkflowBlobStorageURL   | Dirección URL del contrato en el almacenamiento de blobs |
| ContractActionParameters | Parámetros de la acción de contrato |
| TransactionHash          | Hash de la transacción en el libro de contabilidad |
| Estado de aprovisionamiento      | Estado de aprovisionamiento actual de la acción.</br>0: Creada</br>1: En proceso</br>2: Completa</br> "Completa" indica que se agregó correctamente una confirmación desde el libro de contabilidad. |

```csharp
public class ContractActionRequest : MessageModelBase
{
    public int ContractActionId { get; set; }
    public int ConnectionId { get; set; }
    public string UserChainIdentifier { get; set; }
    public string ContractLedgerIdentifier { get; set; }
    public string WorkflowFunctionName { get; set; }
    public string WorkflowName { get; set; }
    public string WorkflowBlobStorageURL { get; set; }
    public IEnumerable<ContractActionParameter> ContractActionParameters { get; set; }
    public string TransactionHash { get; set; }
    public int ProvisioningStatus { get; set; }
}
```

### <a name="updateuserbalance"></a>UpdateUserBalance

Indica que se ha realizado una solicitud para actualizar el saldo del usuario en un libro de contabilidad distribuida específico.

> [!NOTE]
> Este mensaje se genera solo para los libros que requieren fondos de cuentas.
> 

| NOMBRE    | DESCRIPCIÓN                              |
|---------|------------------------------------------|
| Dirección | Dirección del usuario que recibe los fondos |
| Saldo | Saldo del usuario         |
| ChainID | Identificador único de la cadena     |


``` csharp
public class UpdateUserBalanceRequest : MessageModelBase
{
    public string Address { get; set; }
    public decimal Balance { get; set; }
    public int ChainID { get; set; }
}
```

### <a name="insertblock"></a>InsertBlock

El mensaje indica que se ha realizado una solicitud para agregar un bloque en un libro de contabilidad distribuida.

| NOMBRE           | DESCRIPCIÓN                                                            |
|----------------|------------------------------------------------------------------------|
| ChainId        | Identificador único de la cadena a la que se agregó el bloque             |
| BlockId        | Identificador único del bloque en Azure Blockchain Workbench |
| BlockHash      | Hash del bloque                                                 |
| BlockTimeStamp | Marca de tiempo del bloque                                            |

``` csharp
public class InsertBlockRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string BlockHash { get; set; }
    public int BlockTimestamp { get; set; }
}
```

### <a name="inserttransaction"></a>InsertTransaction

El mensaje proporciona detalles sobre una solicitud para agregar una transacción en un libro de contabilidad distribuida.

| NOMBRE            | DESCRIPCIÓN                                                            |
|-----------------|------------------------------------------------------------------------|
| ChainId         | Identificador único de la cadena a la que se agregó el bloque             |
| BlockId         | Identificador único del bloque en Azure Blockchain Workbench |
| TransactionHash | Hash de la transacción                                           |
| De            | Dirección del remitente de la transacción                      |
| Para              | Dirección del destinatario deseado de la transacción              |
| Valor           | Valor incluido en la transacción                                 |
| IsAppBuilderTx  | Identifica si se trata de una transacción de Blockchain Workbench.                         |

``` csharp
public class InsertTransactionRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string TransactionHash { get; set; }
    public string From { get; set; }
    public string To { get; set; }
    public decimal Value { get; set; }
    public bool IsAppBuilderTx { get; set; }
}
```

### <a name="assigncontractchainidentifier"></a>AssignContractChainIdentifier

Proporciona detalles sobre la asignación de un identificador de cadena para un contrato. Por ejemplo, en la cadena de bloques Ethereum, la dirección de un contrato en el libro de contabilidad.

| NOMBRE            | DESCRIPCIÓN                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| ContractId      | Identificador único del contrato en Azure Blockchain Workbench |
| ChainIdentifier | Identificador del contrato en la cadena                             |

``` csharp
public class AssignContractChainIdentifierRequest : MessageModelBase
{
    public int ContractId { get; set; }
    public string ChainIdentifier { get; set; }
}
```

## <a name="classes-used-by-message-types"></a>Clases que usan los tipos de mensaje

### <a name="messagemodelbase"></a>MessageModelBase

Modelo base de todos los mensajes.

| NOMBRE          | DESCRIPCIÓN                          |
|---------------|--------------------------------------|
| nombreOperación | El nombre de la operación           |
| RequestId     | Identificador único para la solicitud |

``` csharp
public class MessageModelBase
{
    public string OperationName { get; set; }
    public string RequestId { get; set; }
}
```

### <a name="contractinputparameter"></a>ContractInputParameter

Contiene el nombre, el valor y el tipo de un parámetro.

| NOMBRE  | DESCRIPCIÓN                 |
|-------|-----------------------------|
| NOMBRE  | Nombre del parámetro  |
| Valor | Valor del parámetro |
| Escriba  | Tipo del parámetro  |

``` csharp
public class ContractInputParameter
{
    public string Name { get; set; }
    public string Value { get; set; }
    public string Type { get; set; }
}
```

#### <a name="contractproperty"></a>ContractProperty

Contiene el identificador, el nombre, el valor y el tipo de una propiedad.

| NOMBRE  | DESCRIPCIÓN                |
|-------|----------------------------|
| Id    | Identificador de la propiedad    |
| NOMBRE  | El nombre de la propiedad  |
| Valor | Valor de la propiedad. |
| Escriba  | Tipo de la propiedad  |

``` csharp
public class ContractProperty
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Value { get; set; }
    public string DataType { get; set; }
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Patrones de integración de contratos inteligentes](integration-patterns.md)