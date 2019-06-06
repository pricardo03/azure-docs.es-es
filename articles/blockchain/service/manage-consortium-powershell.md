---
title: Administrar a miembros de un consorcio de Azure Blockchain Service mediante Azure PowerShell
description: Obtenga información sobre cómo administrar a miembros del consorcio de Azure Blockchain Service mediante Azure PowerShell.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/10/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 4bb72bc3fe8b85a8d2aed88e02f5f3150abb6899
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66493678"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-by-using-powershell"></a>Administrar a miembros del consorcio en Azure Blockchain Service mediante PowerShell

Puede usar PowerShell para administrar a los miembros del consorcio de cadena de bloques para el servicio de la cadena de bloques de Azure. Los miembros que tienen privilegios de administrador pueden invitar, agregar, quitar y cambiar los roles para todos los participantes en el consorcio de la cadena de bloques. Los miembros que tienen privilegios de usuario pueden ver a todos los participantes en el consorcio de la cadena de bloques y cambiar su nombre para mostrar miembros.

## <a name="prerequisites"></a>Requisitos previos

* Crear un miembro de la cadena de bloques mediante la [portal Azure](create-member.md).
* Para obtener más información acerca de consorcios, miembros y los nodos, consulte [consorcio de Azure Blockchain Service](consortium.md).

## <a name="open-azure-cloud-shell"></a>Apertura de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta.

También puede abrir Cloud Shell en una pestaña independiente del explorador, vaya a [shell.azure.com/powershell](https://shell.azure.com/powershell). Seleccione **copia** para copiar los bloques de código, péguelo en Cloud Shell y seleccione **ENTRAR** para ejecutarlo.

## <a name="install-the-powershell-module"></a>Instalar el módulo de PowerShell

Instale el paquete Microsoft.AzureBlockchainService.ConsortiumManagement.PS desde la Galería de PowerShell.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-the-information-preference"></a>Establezca la preferencia de información

Puede obtener más información al ejecutar los cmdlets estableciendo la variable de preferencia de información. De forma predeterminada, *$InformationPreference* está establecido en *SilentlyContinue*.

Para obtener información más detallada de los cmdlets, establezca la preferencia de PowerShell:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Establecer una conexión Web3

Para administrar los miembros del consorcio, establecer una conexión Web3 a su punto de conexión del miembro de Blockchain Service. Puede usar este script para establecer las variables globales para llamar a los cmdlets de administración consortium.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Reemplace *\<contraseña de la cuenta de miembro\>* con la contraseña de cuenta de miembro que usó cuando creó el miembro.

Buscar los otros valores en el portal de Azure:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya a su miembro de cadena de bloques servicio predeterminado **Introducción** página.

    ![Información general sobre miembros](./media/manage-consortium-powershell/member-overview.png)

    Reemplace *\<cuenta de miembro\>* y *\<RootContract dirección\>* con los valores desde el portal.

1. La dirección de punto de conexión, seleccione **nodos transacción**y, a continuación, seleccione el **nodo de transacciones predeterminado**. El nodo de forma predeterminada tiene el mismo nombre que el miembro de la cadena de bloques.
1. Seleccione **Cadenas de conexión**.

    ![Cadenas de conexión](./media/manage-consortium-powershell/connection-strings.png)

    Reemplace *\<dirección de extremo\>* con el valor de **HTTPS (tecla de acceso 1)** o **HTTPS (tecla de acceso 2)** .

## <a name="manage-the-network-and-smart-contracts"></a>Administrar la red y los contratos inteligentes

Use la red y cmdlets de contrato inteligente para establecer una conexión con los contratos inteligentes la cadena de bloques del extremo de responsables de la administración del consorcio.

### <a name="import-consortiummanagementcontracts"></a>Import-ConsortiumManagementContracts

Usar este cmdlet para conectarse a los contratos inteligentes de la administración consorcio. Estos contratos se usan para administrar y aplicar a los miembros dentro del consorcio.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| RootContractAddress | Dirección del contrato de raíz de los contratos inteligentes de administración consortium | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>Import-Web3Account

Use este cmdlet para crear un objeto para contener la información de cuenta de administración de un nodo remoto.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Dirección de cuenta de miembro de cadena de bloques | Sí |
| ManagedAccountPassword | Contraseña de la dirección de cuenta | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>New-Web3Connection

Use este cmdlet para establecer una conexión con el punto de conexión RPC de un nodo de transacciones.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Dirección de punto de conexión del miembro de cadena de bloques | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="manage-the-consortium-members"></a>Administrar a los miembros del Consorcio

Usar cmdlets de administración miembro consortium para administrar a los miembros dentro del consorcio. Las acciones disponibles dependen de su rol consortium.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Use este cmdlet para obtener detalles de miembro o enumerar a los miembros del consorcio.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| Name | El nombre del miembro de Blockchain Service que se desea recuperar los detalles acerca de. Cuando se escribe un nombre, devuelve los detalles del miembro. Si se omite un nombre, devuelve una lista de todos los miembros del consorcio. | Sin |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

#### <a name="example-output"></a>Salida de ejemplo

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Usar este cmdlet para quitar a un miembro de la cadena de bloques.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| NOMBRE | Nombre de miembro para quitar | Sí |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Usar este cmdlet para establecer atributos de miembro, incluido el nombre para mostrar y el rol de consorcio de cadena de bloques.

Pueden establecer los administradores Consortium **DisplayName** y **rol** para todos los miembros. Un miembro de consorcio con el rol de usuario puede cambiar el nombre para mostrar solo su propia del miembro.

`Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| Name | Nombre del miembro de cadena de bloques | Sí |
| DisplayName | Nuevo nombre para mostrar | Sin |
| AccountAddress | Dirección de la cuenta | Sin |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client |  Objeto Web3Client obtenida New Web3Connection| Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="manage-the-consortium-members-invitations"></a>Administrar las invitaciones de los miembros del Consorcio

Use los cmdlets de administración de invitación de consorcio miembros para administrar las invitaciones de miembros del consorcio. Las acciones disponibles dependen de su rol consortium.

### <a name="new-blockchainmemberinvitation"></a>New-BlockchainMemberInvitation

Use este cmdlet para invitar a nuevos miembros para el consorcio.

`New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Id. de suscripción de Azure del miembro que desea invitar | Sí |
| Rol | La función consortium. Valores pueden ser administrador o usuario. Administrador es el rol de administrador de consorcio. USUARIO es el rol de miembro del consorcio. | Sí |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Use este cmdlet para recuperar o enumerar el estado de la invitación de un miembro del consorcio.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | El identificador de suscripción de Azure del miembro que desea invitar. Si la suscripción es de Id. proporcionado, devuelve la suscripción identificadores detalles de la invitación. Si se omite el identificador de suscripción, devuelve una lista de todas las invitaciones de miembro. | Sin |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

#### <a name="example-output"></a>Salida de ejemplo

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Use este cmdlet para revocar la invitación de un miembro consorcio.

`Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Id. de suscripción de Azure de miembro que se va a revocar | Sí |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Use este cmdlet para establecer el **rol** una invitación existente. Solo los administradores de consorcio pueden cambiar las invitaciones.

`Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Id. de suscripción de Azure del miembro que desea invitar | Sí |
| Rol | Nuevo rol del consorcio de invitación. Los valores pueden ser **usuario** o **ADMIN**. | Sí |
| Miembros |  Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de consorcios, miembros y los nodos, consulte:

> [!div class="nextstepaction"]
> [Azure consortium Blockchain Service](consortium.md)
