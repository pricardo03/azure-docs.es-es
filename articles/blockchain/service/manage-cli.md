---
title: Administrar servicio de la cadena de bloques de Azure mediante la CLI de Azure
description: Cómo crear y administrar el servicio de la cadena de bloques de Azure con CLI de Azure
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: d078ca181b2eed4b80d4f12f1c03b42f4e242194
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154454"
---
# <a name="manage-azure-blockchain-service-with-azure-cli"></a>Administrar el servicio de Azure Blockchain con la CLI de Azure

Además de Azure portal, puede usar la CLI de Azure para crear y administrar los miembros de la cadena de bloques y los nodos de la transacción para el servicio de la cadena de bloques de Azure rápidamente.

Asegúrese de que ha instalado la versión más reciente [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) y ha iniciado sesión con una cuenta de Azure en `az login`.

En los ejemplos siguientes, reemplace el ejemplo `<parameter names>` por los suyos propios.

## <a name="create-blockchain-member"></a>Miembro de la cadena de bloques de creación

En el ejemplo se crea a un miembro de cadena de bloques en Azure Blockchain Service que se ejecuta el protocolo de libro de contabilidad de quórum en un consorcio nuevo.

```azurecli
az resource create --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --is-full-object --properties "{ \"location\": \"<myBlockchainLocation>\", \"properties\": {\"password\": \"<myStrongPassword>\", \"protocol\": \"Quorum\", \"consortium\": \"<myConsortiumName>\", \"consortiumManagementAccountPassword\": \"<myConsortiumManagementAccountPassword>\", \"firewallRules\": [ { \"ruleName\": \"<myRuleName>\", \"startIpAddress\": \"<myStartIpAddress>\", \"endIpAddress\": \"<myEndIpAddress>\" } ] }, \"sku\": { \"name\": \"<skuName>\" } }"
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Un nombre único que identifica al miembro de cadena de bloques de Azure Blockchain Service. El nombre se usa para la dirección de punto de conexión público. Por ejemplo, `myblockchainmember.blockchain.azure.com`. |
| **ubicación** | Región de Azure donde se crea el miembro de la cadena de bloques. Por ejemplo, `eastus`. Elija la ubicación más cercana a los usuarios o a sus otras aplicaciones de Azure. |
| **password** | La contraseña de la cuenta de miembro. La contraseña de cuenta de miembro se utiliza para autenticarse en punto de conexión público la cadena de bloques del miembro con la autenticación básica. La contraseña debe cumplir tres de los cuatro requisitos siguientes: longitud debe estar entre 12 y 72 caracteres, caracteres de 1 letra minúscula, 1 letra mayúscula, 1 número y 1 carácter especial que es el número no sign(#), porcentaje (%), coma (,), star(*), comilla inversa (\`), haga doble quote("), comillas simples, dash(-) y semicolumn(;)|
| **protocolo** | Versión preliminar pública admite quórum. |
| **consortium** | Nombre de la sociedad para crear o unirse a. |
| **consortiumManagementAccountPassword** | La contraseña de administración consortium. La contraseña se usa para unir un consorcio. |
| **ruleName** | Nombre de la regla para un intervalo de direcciones IP permitidas. Parámetro opcional para las reglas de firewall.|
| **startIpAddress** | Inicio del intervalo de direcciones IP para la creación de listas blancas. Parámetro opcional para las reglas de firewall. |
| **endIpAddress** | Fin del intervalo de direcciones IP para la creación de listas blancas. Parámetro opcional para las reglas de firewall. |
| **skuName** | Tipo de capa. Utilice S0 para estándar y B0 para básico. |

## <a name="change-blockchain-member-password"></a>Cambiar la contraseña de miembro de cadena de bloques

En el ejemplo se cambia la contraseña de un miembro de cadena de bloques.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.password="<myStrongPassword>" --remove properties.consortiumManagementAccountAddress
```
| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre que identifica al miembro de servicio de la cadena de bloques de Azure. |
| **password** | La contraseña de la cuenta de miembro. La contraseña debe cumplir tres de los cuatro requisitos siguientes: longitud debe estar entre 12 y 72 caracteres, caracteres de 1 letra minúscula, 1 letra mayúscula, 1 número y 1 carácter especial que es el número no sign(#), porcentaje (%), coma (,), star(*), comilla inversa (\`), haga doble quote("), comillas simples, dash(-) y coma (;). |


## <a name="create-transaction-node"></a>Crear el nodo de transacciones

Crear un nodo de transacciones dentro de un miembro existente de la cadena de bloques. Mediante la adición de nodos de la transacción, puede aumentar el aislamiento de seguridad y distribuir la carga. Por ejemplo, podría tener un punto de conexión del nodo de transacciones para diferentes aplicaciones cliente.

```azurecli
az resource create --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers  --is-full-object --properties "{ \"location\": \"<myRegion>\", \"properties\": { \"password\": \"<myStrongPassword>\", \"firewallRules\": [ { \"ruleName\": \"<myRuleName>\", \"startIpAddress\": \"<myStartIpAddress>\", \"endIpAddress\": \"<myEndIpAddress>\" } ] } }"
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre del miembro de cadena de bloques de servicio de Azure Blockchain que también incluye el nuevo nombre de nodo de la transacción. |
| **ubicación** | Región de Azure donde se crea el miembro de la cadena de bloques. Por ejemplo, `eastus`. Elija la ubicación más cercana a los usuarios o a sus otras aplicaciones de Azure. |
| **password** | La contraseña de nodo de la transacción. La contraseña debe cumplir tres de los cuatro requisitos siguientes: longitud debe estar entre 12 y 72 caracteres, caracteres de 1 letra minúscula, 1 letra mayúscula, 1 número y 1 carácter especial que es el número no sign(#), porcentaje (%), coma (,), star(*), comilla inversa (\`), haga doble quote("), comillas simples, dash(-) y coma (;). |
| **ruleName** | Nombre de la regla para un intervalo de direcciones IP permitidas. Parámetro opcional para las reglas de firewall. |
| **startIpAddress** | Inicio del intervalo de direcciones IP para la creación de listas blancas. Parámetro opcional para las reglas de firewall. |
| **endIpAddress** | Fin del intervalo de direcciones IP para la creación de listas blancas. Parámetro opcional para las reglas de firewall.|

## <a name="change-transaction-node-password"></a>Cambiar la contraseña de nodo de transacciones

En el ejemplo se cambia una contraseña de nodo de la transacción.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers  --set properties.password="<myStrongPassword>"
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde existen los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre del miembro de cadena de bloques de servicio de Azure Blockchain que también incluye el nuevo nombre de nodo de la transacción. |
| **password** | La contraseña de nodo de la transacción. La contraseña debe cumplir tres de los cuatro requisitos siguientes: longitud debe estar entre 12 y 72 caracteres, caracteres de 1 letra minúscula, 1 letra mayúscula, 1 número y 1 carácter especial que es el número no sign(#), porcentaje (%), coma (,), star(*), comilla inversa (\`), haga doble quote("), comillas simples, dash(-) y coma (;). |

## <a name="change-consortium-management-account-password"></a>Contraseña de la cuenta de administración de cambios consortium

La cuenta de administración del consorcio se usa para la administración de pertenencia del consorcio. Cada miembro se identifica mediante una cuenta de administración consortium y puede cambiar la contraseña de esta cuenta con el siguiente comando.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.consortiumManagementAccountPassword="<myConsortiumManagementAccountPassword>" --remove properties.consortiumManagementAccountAddress
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre que identifica al miembro de servicio de la cadena de bloques de Azure. |
| **consortiumManagementAccountPassword** | La contraseña de cuenta de administración consortium. La contraseña debe cumplir tres de los cuatro requisitos siguientes: longitud debe estar entre 12 y 72 caracteres, caracteres de 1 letra minúscula, 1 letra mayúscula, 1 número y 1 carácter especial que es el número no sign(#), porcentaje (%), coma (,), star(*), comilla inversa (\`), haga doble quote("), comillas simples, dash(-) y coma (;). |
  
## <a name="update-firewall-rules"></a>Actualizar reglas de firewall

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.firewallRules="[ { \"ruleName\": \"<myRuleName>\", \"startIpAddress\": \"<myStartIpAddress>\", \"endIpAddress\": \"<myEndIpAddress>\" } ]" --remove properties.consortiumManagementAccountAddress
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde existen los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre del miembro de cadena de bloques de servicio de la cadena de bloques de Azure. |
| **ruleName** | Nombre de la regla para un intervalo de direcciones IP permitidas. Parámetro opcional para las reglas de firewall.|
| **startIpAddress** | Inicio del intervalo de direcciones IP para la creación de listas blancas. Parámetro opcional para las reglas de firewall.|
| **endIpAddress** | Fin del intervalo de direcciones IP para la creación de listas blancas. Parámetro opcional para las reglas de firewall.|

## <a name="list-api-keys"></a>Claves de API de la lista

Claves de API pueden usarse para el acceso al nodo similar al nombre de usuario y contraseña. Hay dos claves de API para admitir la rotación de claves. Use el siguiente comando para enumerar las claves de API.

```azurecli
az resource invoke-action --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --action "listApiKeys" --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde existen los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre del miembro de cadena de bloques de servicio de Azure Blockchain que también incluye el nuevo nombre de nodo de la transacción. |

## <a name="regenerate-api-keys"></a>Regenerar claves de API

Use el comando siguiente para volver a generar las claves de API.

```azurecli
az resource invoke-action --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --action "regenerateApiKeys" --resource-type Microsoft.Blockchain/blockchainMembers --request-body '{"keyName":"<keyValue>"}'
```


| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde existen los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre del miembro de cadena de bloques de servicio de Azure Blockchain que también incluye el nuevo nombre de nodo de la transacción. |
| **keyName** | Reemplace \<keyValue\> con key1 o key2. |

## <a name="delete-a-transaction-node"></a>Eliminar un nodo de transacciones

En el ejemplo se elimina un nodo de transacciones de miembro de cadena de bloques.

```azurecli
az resource delete --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde existen los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre del miembro de cadena de bloques de Azure Blockchain Service que también incluye el nuevo nombre de nodo de transacción va a eliminar. |

## <a name="delete-a-blockchain-member"></a>Eliminar a un miembro de la cadena de bloques

En el ejemplo se elimina a un miembro de la cadena de bloques.

```azurecli
az resource delete --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde existen los recursos del servicio de la cadena de bloques de Azure. |
| **name** | Nombre del miembro de cadena de bloques de Azure Blockchain Service va a eliminar. |

## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="grant-access-for-azure-ad-user"></a>Conceda acceso de usuario de Azure AD

```azurecli
az role assignment create --role <role> --assignee <assignee> --scope /subscriptions/<subId>/resourceGroups/<groupName>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **role** | Nombre del rol de Azure AD. |
| **assignee** | Identificador de usuario de Azure AD. Por ejemplo: `user@contoso.com` |
| **scope** | Ámbito de la asignación de roles. Puede ser miembro de cadena de bloques de nodo de transacciones. |

**Ejemplo:**

Conceder acceso al nodo para el usuario de Azure AD para blockchain **miembro**:

```azurecli
az role assignment create \
  --role "myRole" \
  --assignee user@contoso.com \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

**Ejemplo:**

Conceder acceso al nodo para el usuario de Azure AD para blockchain **nodo transacciones**:

```azurecli
az role assignment create \
  --role "MyRole" \
  --assignee user@contoso.com \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1/transactionNodes/contosoTransactionNode1
```

### <a name="grant-node-access-for-azure-ad-group-or-application-role"></a>Conceder acceso al nodo de grupo de Azure AD o aplicación de función

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id>
```
| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **role** | Nombre del rol de Azure AD. |
| **assignee-object-id** | Identificador de aplicación o Id. de grupo de Azure AD. |
| **scope** | Ámbito de la asignación de roles. Puede ser miembro de cadena de bloques de nodo de transacciones. |

**Ejemplo:**

Conceder acceso al nodo para **rol de aplicación**

```azurecli
az role assignment create \
  --role "myRole" \
  --assignee-object-id 22222222-2222-2222-2222-222222222222 \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

### <a name="remove-azure-ad-node-access"></a>Quitar el acceso al nodo de Azure AD

```azurecli
az role assignment delete --role <myRole> --assignee <assignee> --scope /subscriptions/mySubscriptionId/resourceGroups/<myResourceGroup>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>/transactionNodes/<myTransactionNode>
```

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **role** | Nombre del rol de Azure AD. |
| **assignee** | Identificador de usuario de Azure AD. Por ejemplo: `user@contoso.com` |
| **scope** | Ámbito de la asignación de roles. Puede ser miembro de cadena de bloques de nodo de transacciones. |

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configurar los nodos de transacción de servicio de Azure Blockchain con Azure portal](configure-transaction-nodes.md)
