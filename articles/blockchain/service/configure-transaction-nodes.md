---
title: Configurar los nodos de transacción de servicio de Azure Blockchain
description: Cómo configurar los nodos de la transacción de servicio de Azure Blockchain
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: dffeb81ae1eb244c38639a1241c0581e6fcdf94a
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027966"
---
# <a name="configure-azure-blockchain-service-transaction-nodes"></a>Configurar los nodos de transacción de servicio de Azure Blockchain

Para interactuar con el servicio de la cadena de bloques de Azure, hacerlo a través de conectarse a uno o varios nodos de la transacción en el miembro de la cadena de bloques.  Con el fin de interactuar con los nodos de la transacción, deberá configurar los nodos para el acceso.

## <a name="prerequisites"></a>Requisitos previos

* [Crear a un miembro de la cadena de bloques de Azure](create-member.md)

## <a name="transaction-node-overview"></a>Información general del nodo de transacciones

Nodos de transacciones se utilizan para enviar las transacciones de la cadena de bloques a Azure Blockchain Service a través de un punto de conexión público. El nodo de transacciones predeterminado contiene la clave privada de la cuenta de Ethereum registrada en la cadena de bloques y, por lo tanto, no se puede eliminar.

Para ver los detalles de nodo de transacción predeterminado:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Navegue al miembro de servicio de la cadena de bloques de Azure. Seleccione **nodos transacción**.

    ![Seleccione el nodo de transacciones predeterminado](./media/configure-transaction-nodes/nodes.png)

    Detalles de información general incluyen las direcciones de punto de conexión público y la clave pública.

## <a name="create-transaction-node"></a>Crear el nodo de transacciones

Puede agregar hasta nueve nodos adicionales de transacción al miembro de cadena de bloques, para un total de diez nodos de la transacción. Mediante la adición de nodos de la transacción, puede aumentar la escalabilidad o distribuir la carga. Por ejemplo, podría tener un punto de conexión del nodo de transacciones para diferentes aplicaciones cliente.

Para agregar un nodo de transacciones:

1. En Azure portal, vaya al miembro de servicio de la cadena de bloques de Azure y seleccione **nodos transacción > agregar**.
1. Complete la configuración del nuevo nodo de la transacción.

    ![Agregar nodo de transacciones](./media/configure-transaction-nodes/add-node.png)

    | Configuración | DESCRIPCIÓN |
    |---------|-------------|
    | NOMBRE | Nombre de nodo de la transacción. El nombre se utiliza para crear la dirección DNS para el punto de conexión del nodo de transacciones. Por ejemplo, `newnode-myblockchainmember.blockchain.azure.com`. El nombre de nodo no puede cambiarse una vez que se crea. |
    | Contraseña | Establecer una contraseña segura. Usar la contraseña para tener acceso al extremo del nodo de transacciones con la autenticación básica.

1. Seleccione **Crear**.

    Aprovisionamiento de un nuevo nodo de la transacción se tarda unos diez minutos. Nodos de transacciones adicionales se incurre en costos. Para obtener más información sobre los costos, consulte [los precios de Azure](https://aka.ms/ABSPricing).

## <a name="endpoints"></a>Puntos de conexión

Los nodos de la transacción tienen un nombre DNS único y puntos de conexión públicos.

Para ver los detalles del punto de un nodo de transacción:

1. En el portal de Azure, navegue hasta uno de los nodos de transacción de servicio de Azure Blockchain miembro y seleccione **Introducción**.

    ![Puntos de conexión](./media/configure-transaction-nodes/endpoints.png)

Los puntos de conexión del nodo de transacciones son seguras y requieran la autenticación. Puede conectarse a un punto de conexión de transacción mediante la autenticación de Azure AD, la autenticación básica HTTPS y mediante una clave de acceso a través de HTTPS o Websocket a través de SSL.

### <a name="azure-active-directory-access-control"></a>Control de acceso de Azure Active Directory

Puntos de conexión de Azure Blockchain Service transacción nodo admiten la autenticación de Azure Active Directory (Azure AD). Puede conceder el usuario, grupo y entidad de servicio acceso de Azure AD al punto de conexión.

Para conceder a Azure de control de acceso de AD al punto de conexión:

1. En el portal de Azure, vaya al miembro de servicio de la cadena de bloques de Azure y seleccione **nodos transacción > control de acceso (IAM) > Agregar > Agregar asignación de roles**.
1. Cree una nueva asignación de roles para un usuario, grupo o entidad de servicio (roles de aplicación).

    ![Agregar rol de IAM](./media/configure-transaction-nodes/add-role.png)

    | Configuración | . |
    |---------|-------------|
    | Rol | Seleccione **propietario**, **colaborador**, o **lector**.
    | Asignar acceso a | Seleccione **usuario, grupo o entidad de servicio de Azure AD**.
    | Seleccionar | Busque el usuario, grupo o entidad de servicio que desea agregar.

1. Seleccione **guardar** para agregar la asignación de roles.

Para obtener más información sobre el control de acceso de Azure AD, consulte [administrar el acceso a recursos de Azure mediante RBAC y el portal de Azure](../../role-based-access-control/role-assignments-portal.md)

Para obtener más información sobre cómo conectarse mediante la autenticación de Azure AD, consulte [conectarse a su nodo mediante la autenticación de AAD](configure-aad.md).

### <a name="basic-authentication"></a>Autenticación básica

Para la autenticación básica de HTTPS, se pasan las credenciales de nombre y la contraseña del usuario en el encabezado HTTPS de la solicitud al punto de conexión.

Puede ver los detalles del extremo de autenticación básica de un nodo de transacción en el portal de Azure. Navegue hasta uno de los nodos de transacción de servicio de Azure Blockchain miembro y seleccione **autenticación básica** en la configuración.

![Autenticación básica](./media/configure-transaction-nodes/basic.png)

El nombre de usuario es el nombre del nodo y no se puede cambiar.

Para usar la dirección URL, reemplace \<contraseña\> con la contraseña establecida cuando se aprovisionó el nodo. Puede actualizar la contraseña, seleccione **restablecer contraseña**.

### <a name="access-keys"></a>Claves de acceso

Para la autenticación de clave de acceso, la clave de acceso se incluye en la dirección URL del extremo. Cuando se aprovisiona el nodo de la transacción, se generan dos claves de acceso. Cualquier clave de acceso puede usarse para la autenticación. Habilitar las dos claves cambiar y girar las claves.

Puede ver los detalles clave de acceso del nodo de una transacción y copiar las direcciones de punto de conexión que incluyen las claves de acceso. Navegue hasta uno de los nodos de transacción de servicio de Azure Blockchain miembro y seleccione **claves de acceso** en la configuración.

### <a name="firewall-rules"></a>Reglas de firewall

Las reglas de Firewall permiten limitar las direcciones IP que pueden intentar autenticarse en el nodo de la transacción.  Si no hay reglas de firewall están configuradas para el nodo de la transacción, no son accesibles por cualquier entidad.  

Para ver las reglas de firewall de un nodo de transacciones, navegue hasta uno de los nodos de transacción de servicio de Azure Blockchain miembro y seleccione **reglas de Firewall** en la configuración.

Puede agregar reglas de firewall mediante la especificación de un nombre de regla, IP inicial dirección y una dirección IP final de direcciones en el **reglas de Firewall** cuadrícula.

![Reglas de firewall](./media/configure-transaction-nodes/firewall-rules.png)

Para habilitar:

* **Dirección IP única:** Configurar la misma dirección IP para el inicio y final de las direcciones IP.
* **Intervalo de direcciones IP:** Configurar el inicio y final del intervalo de direcciones IP. Por ejemplo, un intervalo comenzando 10.221.34.0 y terminando en 10.221.34.255 permitiría la subred 10.221.34.xxx todo.
* **Permitir todas las direcciones IP:** Configure la dirección IP inicial a 0.0.0.0 y la dirección IP final a 255.255.255.255.

## <a name="connection-strings"></a>Cadenas de conexión

Sintaxis de cadena de conexión para el nodo de la transacción se proporcionan para basic authentication o mediante claves de acceso. Se proporcionan las cadenas de conexión, incluidas las claves de acceso a través de HTTPS y WebSockets.

Puede ver las cadenas de conexión de un nodo de transacciones y copiar las direcciones de punto de conexión. Navegue hasta uno de los nodos de transacción de servicio de Azure Blockchain miembro y seleccione **las cadenas de conexión** en la configuración.

![Cadenas de conexión](./media/configure-transaction-nodes/connection-strings.png)

## <a name="sample-code"></a>Código de ejemplo

Código de ejemplo se proporciona para habilitar rápidamente la conexión a su nodo de transacciones a través de Web3, Nethereum, Web3js y Truffle.

Puede ver el código de conexión de ejemplo de un nodo de transacciones y cópiela para usarla con herramientas de desarrollo más populares. Navegue hasta uno de los nodos de transacción de servicio de Azure Blockchain miembro y seleccione **código de ejemplo** en la configuración.

Elija la pestaña Web3 o Nethereum para ver el ejemplo de código que desea usar.

![Código de ejemplo](./media/configure-transaction-nodes/sample-code.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configurar los nodos de la transacción mediante la CLI de Azure](manage-cli.md)
