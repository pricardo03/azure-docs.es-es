---
title: Administración de consortium Blockchain Service Azure con PowerShell
description: Cómo administrar a miembros del consorcio de servicio de la cadena de bloques de Azure con PowerShell
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: bef0c5d776e8ab6424b8604a49782088c45b0538
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028236"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-using-powershell"></a>Administrar a miembros del consorcio en Azure Blockchain Service mediante PowerShell

Puede usar PowerShell para administrar a los miembros del consorcio de cadena de bloques para el servicio de la cadena de bloques de Azure. Los miembros con privilegios de administrador pueden invitar, agregar, quitar y cambia los roles para todos los participantes en el consorcio de la cadena de bloques. Los miembros con privilegios de usuario pueden ver a todos los participantes en el consorcio de la cadena de bloques y pueden cambiar su nombre para mostrar miembros.

## <a name="prerequisites"></a>Requisitos previos

* [Crear a un miembro de cadena de bloques con Azure portal](create-member.md)
* Para obtener más información acerca de consorcios, miembros y los nodos, consulte [consortium de servicio de Azure Blockchain](consortium.md)

## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. 

También puede ir a [https://shell.azure.com/powershell](https://shell.azure.com/powershell) para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.

## <a name="install-powershell-module"></a>Instalar el módulo de PowerShell

Instale el paquete Microsoft.AzureBlockchainService.ConsortiumManagement.PS desde la Galería de PowerShell.

```powershell
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="establish-a-web3-connection"></a>Establecer una conexión Web3

Para administrar los miembros del consorcio, deberá establecer una conexión Web3 con el punto de conexión de servicio de Azure Blockchain miembro. Puede usar este script para establecer las variables globales que se pueden usar cuando se llama a los cmdlets de administración consortium.

```powershell
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Reemplace \<contraseña de la cuenta de miembro\> con la contraseña de cuenta de miembro que usó cuando creó el miembro.

Buscar los otros valores en el portal de Azure:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Navegue al miembro predeterminado de Azure Blockchain Service **Introducción** página.

    ![Información general sobre miembros](./media/manage-consortium-powershell/member-overview.png)

    Reemplace \<cuenta de miembro\>, y \<RootContract dirección\> con los valores desde el portal.

1. La dirección de punto de conexión, seleccione **nodos transacción** y seleccione un nodo de transacciones.
1. Seleccione **las cadenas de conexión**.

    ![Cadenas de conexión](./media/manage-consortium-powershell/connection-strings.png)

    Reemplace \<dirección de extremo\> con el valor de **HTTPS (tecla de acceso 1)** o **HTTPS (tecla de acceso 2)**.

## <a name="network-and-smart-contract-management"></a>Administración de redes y contrato inteligente

Use la red y cmdlets de contrato inteligente para establecer una conexión con las cadena de bloques extremo contratos inteligentes responsables de la administración del consorcio.

### <a name="import-consortiummanagementcontracts"></a>Import-ConsortiumManagementContracts

Se conecta a los contratos inteligentes de administración consortium, que se usan para administrar y aplicar a los miembros dentro del consorcio.

```powershell
Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| RootContractAddress | La dirección del contrato de raíz de los contratos inteligentes de administración consortium | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

**Ejemplo**

```powershell
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>Import-Web3Account

Use este cmdlet para crear un objeto para almacenar la cuenta de administración de información nodo remoto.

```powershell
Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Dirección de cuenta de miembro de cadena de bloques | Sí |
| ManagedAccountPassword | Contraseña de la dirección de cuenta | Sí |

**Ejemplo**

```powershell
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>New-Web3Connection

Establece una conexión al punto de conexión RPC de un nodo de transacciones.

```powershell
New-Web3Connection [-RemoteRPCEndpoint <String>]
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Dirección de punto de conexión del miembro de cadena de bloques | Sí |


**Ejemplo**

```powershell
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="consortium-member-management"></a>Administración de miembros del Consorcio

Usar cmdlets de administración miembro consortium para administrar a los miembros dentro del consorcio. Las acciones disponibles dependen de su rol consortium.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Obtiene los detalles de miembro o lista de miembros del consorcio.

```powershell
Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| NOMBRE | El nombre del miembro de servicio de la cadena de bloques de Azure que desea recuperar los detalles en. Si proporciona un nombre de miembro, se devuelven los detalles del miembro. Si se omite el nombre, se devuelve una lista de todos los miembros del consorcio. | Sin  |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

**Ejemplo**

```powershell
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

**Salida del ejemplo**

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Quita a un miembro de la cadena de bloques.

```powershell
Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| NOMBRE | Nombre de miembro para quitar | Sí |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

**Ejemplo**

```powershell
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Cadena de bloques se establece como rol de nombre y el consorcio de mostrar los atributos de miembro.

Pueden establecer los administradores Consortium **DisplayName** y **rol** para todos los miembros. Miembros del consorcio con el rol de usuario solo pueden cambiar el nombre para mostrar sus propios del miembro.

```powershell
Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| NOMBRE | Nombre del miembro de cadena de bloques | Sí |
| DisplayName | Nuevo nombre para mostrar | Sin  |
| AccountAddress | Dirección de la cuenta | Sin  |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client |  Objeto Web3Client obtenida New Web3Connection| Sí |

**Ejemplo**

```powershell
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="consortium-member-invitation-management"></a>Administración de invitación de consorcio miembros

Usar cmdlets de administración de invitación de consorcio miembros para administrar las invitaciones de miembros del consorcio. Las acciones disponibles dependen de su rol consortium.

### <a name="new-blockchainmemberinvitation"></a>New-BlockchainMemberInvitation

Invitar a nuevos miembros para el consorcio.

```powershell
New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Id. de suscripción de Azure de miembro invitado | Sí |
| Rol | Rol del consorcio. Valores pueden ser administrador o usuario. Administrador es el rol de administrador de consorcio. USUARIO es el rol de miembro del consorcio. | Sí |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

**Ejemplo**

```powershell
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Recupera o enumera el estado de la invitación miembro consortium.

```powershell
Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Id. de suscripción de Azure de miembro invitado. Si se proporciona el SubscriptionID, se devuelven detalles de la invitación del Id. de suscripción. Si se omite el SubscriptionID, se devuelve una lista de todas las invitaciones de miembro. | Sin  |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

**Ejemplo**

```powershell
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

**Salida del ejemplo**

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Revoca una invitación de miembros del consorcio.

```powershell
Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Id. de suscripción de Azure de miembro para revocar | Sí |
| Miembros | Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

**Ejemplo**

```powershell
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Establece el **rol** una invitación existente. Solo los administradores de consorcio pueden cambiar las invitaciones.

```powershell
Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>
```

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-----------|-------------|:--------:|
| SubscriptionId | Id. de suscripción de Azure de miembro invitado | Sí |
| Rol | Nuevo rol del consorcio de invitación. Los valores pueden ser **usuario** o **ADMIN** | Sí |
| Miembros |  Objeto de miembros obtenido de importación ConsortiumManagementContracts | Sí |
| Web3Account | Objeto Web3Account obtenida Web3Account de importación | Sí |
| Web3Client | Objeto Web3Client obtenida New Web3Connection | Sí |

**Ejemplo**

```powershell
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de consorcios, miembros y los nodos, consulte:

> [!div class="nextstepaction"]
> [Azure consortium Blockchain Service](consortium.md)