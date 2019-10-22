---
title: Creación de un miembro de Azure Blockchain Service mediante Azure Portal
description: Use Azure Blockchain Service para crear a un miembro de cadena de bloques.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/14/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: c87f9a044d6655b2062f2c2657d3b465699e0dd2
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329258"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-the-azure-portal"></a>Inicio rápido: Creación de un miembro de cadena de bloques de Azure Blockchain Service mediante Azure Portal

Azure Blockchain Service es una plataforma de cadena de bloques que puede ejecutar su lógica de negocios dentro de un contrato inteligente. En este inicio rápido se muestra cómo empezar a crear un miembro de la cadena de bloques mediante Azure Portal.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-blockchain-member"></a>Creación de un miembro de cadena de bloques

Cree un miembro de cadena de bloques que ejecute el protocolo de libro de contabilidad de Quorum en un nuevo consorcio o existente.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
1. Seleccione **Cadena de bloques** > **Azure Blockchain Service**.
1. Rellene la plantilla.

    ![Creación de un servicio](./media/create-member/create-member.png)

    Configuración | DESCRIPCIÓN
    --------|------------
    Blockchain Member (Miembro de cadena de bloques) | Elija un nombre único que identifique al miembro de cadena de bloques de Azure Blockchain Service. El nombre del miembro de la cadena de bloques solo puede contener letras minúsculas y números. El primer carácter debe ser una letra. El valor debe tener entre 2 y 20 caracteres.
    Subscription | Seleccione la suscripción de Azure que quiere usar con el servicio. Si tiene varias suscripciones, elija aquella en la que se factura el recurso.
    Resource group | Un nuevo nombre de grupo de recursos o uno existente de la suscripción.
    Region | La ubicación debe ser la misma para todos los miembros del consorcio.
    Member account password (Contraseña de la cuenta del miembro) | cifrar la clave privada de la cuenta de Ethereum que se crea para el miembro. Usará la cuenta del miembro y la contraseña de la cuenta del miembro para la administración del consorcio.
    Nombre del consorcio | Si es un nuevo consorcio, escriba un nombre único. Si se va a unir a un consorcio por medio de una invitación, el valor es el consorcio al que se une.
    DESCRIPCIÓN | Descripción del consorcio.
    Protocolo |  La versión preliminar admite el protocolo de quorum.
    Precios | La configuración de nodo del nuevo servicio. Seleccione **Estándar**. La configuración predeterminada es 2 nodos de validación y 1 nodo de transacción.
    Contraseña del nodo de transacción | La contraseña del nodo de transacción predeterminado del miembro. Use la contraseña para la autenticación básica al conectarse al punto de conexión público del nodo de transacción predeterminado del miembro de la cadena de bloques.

1. Seleccione **Crear** para aprovisionar el servicio. El aprovisionamiento tarda aproximadamente 10 minutos.
1. Para supervisar el proceso de implementación, seleccione **Notificaciones** en la barra de herramientas.
1. Después de la implementación, vaya al miembro de cadena de bloques.

Seleccione **Información general** para ver la información básica sobre el servicio, como la dirección y la cuenta del miembro RootContract.

![Información general sobre los miembros de cadena de bloques](./media/create-member/overview.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede usar el miembro que ha creado en el siguiente inicio rápido o tutorial. Cuando ya no necesite los recursos, puede eliminarlos; para ello, elimine el grupo de recursos `myResourceGroup` que creó mediante Azure Blockchain Service.

Para eliminar el grupo de recursos:

1. En Azure Portal, vaya a **Grupo de recursos** en el panel de navegación izquierdo y seleccione el grupo de recursos que desea eliminar.
2. Seleccione **Eliminar grupo de recursos**. Compruebe la eliminación escribiendo el nombre del grupo de recursos y seleccionando **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Uso de MetaMask para conectarse a un contrato inteligente e implementarlo](connect-metamask.md)