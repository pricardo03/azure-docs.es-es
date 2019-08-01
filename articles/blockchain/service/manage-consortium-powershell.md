---
title: Administración de los miembros de un consorcio de Azure Blockchain Service mediante Azure PowerShell
description: Aprenda a administrar los miembros de un consorcio de Azure Blockchain Service mediante Azure PowerShell.
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66493678"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-by-using-powershell"></a>Administración de los miembros del consorcio en Azure Blockchain Service mediante PowerShell

Puede usar PowerShell para administrar los miembros del consorcio de una cadena de bloques para Azure Blockchain Service. Los miembros que tienen privilegios de administrador pueden invitar, agregar, quitar y cambiar los roles para todos los participantes en el consorcio de la cadena de bloques. Los miembros que tienen privilegios de usuario pueden ver a todos los participantes en el consorcio de la cadena de bloques y cambiar su nombre para mostrar miembros.

## <a name="prerequisites"></a>Requisitos previos

* Cree un miembro de la cadena de bloques con [Azure Portal](create-member.md).
* Para obtener más información acerca de los consorcios, los miembros y los nodos, consulte [Consorcio de Azure Blockchain Service](consortium.md).

## <a name="open-azure-cloud-shell"></a>Apertura de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta.

También puede ir abrir Cloud Shell en una pestaña independiente del explorador si va a [shell.azure.com/powershell](https://shell.azure.com/powershell). Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, seleccione **Entrar** para ejecutarlos.

## <a name="install-the-powershell-module"></a>Instalación del módulo de PowerShell

Instale el paquete Microsoft.AzureBlockchainService.ConsortiumManagement.PS desde la Galería de PowerShell.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-the-information-preference"></a>Establecimiento de la preferencia de información

Puede obtener más información al ejecutar los cmdlets estableciendo la variable de preferencia de información. De forma predeterminada, *$InformationPreference* está establecida en *SilentlyContinue*.

Para obtener información más detallada de los cmdlets, establezca la preferencia de PowerShell como sigue:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Establecimiento de una conexión Web3

Para administrar los miembros del consorcio, establezca una conexión Web3 a su punto de conexión del miembro de Blockchain Service. Puede usar este script para establecer las variables globales para llamar a los cmdlets de administración del consorcio.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Reemplace la *\<contraseña de la cuenta de miembro\>* con la contraseña de cuenta de miembro que usó cuando creó el miembro.

Busque los otros valores registrados en Azure Portal:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya a su página de **Introducción** del miembro del servicio Blockchain predeterminado.

    ![Información general sobre miembros](./media/manage-consortium-powershell/member-overview.png)

    Reemplace *\<Member account\>* (Cuenta de miembro) y *\<RootContract address\>* (Dirección de RootContract) con los valores del portal.

1. Para la dirección de punto de conexión, seleccione **Nodos de transacción** y, a continuación, seleccione el **nodo de transacciones predeterminado**. El nodo predeterminado tiene el mismo nombre que el miembro de la cadena de bloques.
1. Seleccione **Cadenas de conexión**.

    ![Cadenas de conexión](./media/manage-consortium-powershell/connection-strings.png)

    Reemplace *\<Endpoint address\>* (Dirección de punto de conexión) con el valor de **HTTPS (tecla de acceso 1)** o **HTTPS (tecla de acceso 2)** .

## <a name="manage-the-network-and-smart-contracts"></a>Administración de la red y los contratos inteligentes

Use los cmdlets de los contratos inteligentes y la red para establecer una conexión con los contratos inteligentes del punto de conexión de la cadena de bloques responsables de la administración del consorcio.

### <a name="import-consortiummanagementcontracts"></a>Import-ConsortiumManagementContracts

Use este cmdlet para conectarse a los contratos inteligentes de la administración del consorcio. Estos contratos se usan para administrar y aplicar a los miembros dentro del consorcio.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| RootContractAddress | Dirección de contrato raíz de los contratos inteligentes de la administración del consorcio | Sí |
| Web3Client | Objeto Web3Client obtenido de New-Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>Import-Web3Account

Use este cmdlet para crear un objeto y contener la información de la cuenta de administración de un nodo remoto.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Dirección de la cuenta del miembro de la cadena de bloques | Sí |
| ManagedAccountPassword | Contraseña de la dirección de la cuenta | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>New-Web3Connection

Use este cmdlet para establecer una conexión con el punto de conexión RPC de un nodo de transacciones.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Dirección del punto de conexión del miembro de la cadena de bloques | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="manage-the-consortium-members"></a>Administración de los miembros del consorcio

Use los cmdlets de administración de los miembros del consorcio para administrar los miembros dentro del consorcio. Las acciones disponibles dependen del rol del consorcio.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Use este cmdlet para obtener detalles de los miembros o enumerar los miembros del consorcio.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| NOMBRE | El nombre del miembro de Blockchain Service cuyos detalles se desea recuperar. Cuando se escribe un nombre, devuelve los detalles del miembro. Si se omite un nombre, devuelve una lista de todos los miembros del consorcio. | Sin |
| Miembros | Objeto Members obtenido de Import-ConsortiumManagementContracts | Sí |
| Web3Client | Objeto Web3Client obtenido de New-Web3Connection | Sí |

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

Use este cmdlet para quitar a un miembro de la cadena de bloques.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| NOMBRE | Nombre del miembro que quitar | Sí |
| Miembros | Objeto Members obtenido de Import-ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenido de Import-Web3Account | Sí |
| Web3Client | Objeto Web3Client obtenido de New-Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Use este cmdlet para establecer los atributos de los miembros, incluidos el nombre para mostrar y el rol del consorcio de la cadena de bloques.

Los administradores del consorcio pueden establecer el valor de **DisplayName** y de **Role** de todos los miembros. Un miembro del consorcio con el rol de usuario puede cambiar solo el nombre para mostrar de su propio miembro.

`Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| NOMBRE | Nombre de un miembro de la cadena de bloques | Sí |
| DisplayName | Nuevo nombre para mostrar | Sin |
| AccountAddress | Dirección de la cuenta | Sin |
| Miembros | Objeto Members obtenido de Import-ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenido de Import-Web3Account | Sí |
| Web3Client |  Objeto Web3Client obtenido de New-Web3Connection| Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="manage-the-consortium-members-invitations"></a>Administración de las invitaciones de los miembros del consorcio

Use los cmdlets de administración de invitaciones a miembros del consorcio para administrarlas. Las acciones disponibles dependen del rol del consorcio.

### <a name="new-blockchainmemberinvitation"></a>New-BlockchainMemberInvitation

Use este cmdlet para invitar a nuevos miembros al consorcio.

`New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Identificador de suscripción de Azure del miembro que desea invitar | Sí |
| Rol | Rol del consorcio. Los valores pueden ser ADMIN o USER. ADMIN es el rol de administrador del consorcio. USER es el rol de miembro del consorcio. | Sí |
| Miembros | Objeto Members obtenido de Import-ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenido de Import-Web3Account | Sí |
| Web3Client | Objeto Web3Client obtenido de New-Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Use este cmdlet para recuperar o enumerar el estado de la invitación de un miembro del consorcio.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Identificador de suscripción de Azure del miembro que desea invitar. Si se proporciona el identificador de la suscripción, devuelve los detalles de la invitación de dicho identificador. Si el identificador de suscripción se omite, devuelve una lista de todas las invitaciones de miembros. | Sin |
| Miembros | Objeto Members obtenido de Import-ConsortiumManagementContracts | Sí |
| Web3Client | Objeto Web3Client obtenido de New-Web3Connection | Sí |

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

Use este cmdlet para revocar la invitación de un miembro del consorcio.

`Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Identificador de suscripción de Azure del miembro que desea revocar | Sí |
| Miembros | Objeto Members obtenido de Import-ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenido de Import-Web3Account | Sí |
| Web3Client | Objeto Web3Client obtenido de New-Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Use este cmdlet para establecer el **rol** de una invitación existente. Solo los administradores del consorcio pueden cambiar las invitaciones.

`Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Identificador de suscripción de Azure del miembro que desea invitar | Sí |
| Rol | Nuevo rol del consorcio para la invitación. Los valores pueden ser **USER** o **ADMIN**. | Sí |
| Miembros |  Objeto Members obtenido de Import-ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenido de Import-Web3Account | Sí |
| Web3Client | Objeto Web3Client obtenido de New-Web3Connection | Sí |

#### <a name="example"></a>Ejemplo

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los consorcios, los miembros y los nodos, consulte:

> [!div class="nextstepaction"]
> [Consorcio de Azure Blockchain Service](consortium.md)
