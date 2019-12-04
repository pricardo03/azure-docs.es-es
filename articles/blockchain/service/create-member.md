---
title: 'Creación de un miembro de Azure Blockchain Service: Azure Portal'
description: Creación de un miembro de Azure Blockchain Service para un consorcio de una cadena de bloques mediante Azure Portal.
ms.date: 11/18/2019
ms.topic: quickstart
ms.reviewer: janders
ms.openlocfilehash: 90f859e4d2e2621afad9cab11d66e81018bd3147
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455786"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-the-azure-portal"></a>Inicio rápido: Creación de un miembro de cadena de bloques de Azure Blockchain Service mediante Azure Portal

En este inicio rápido se implementan un nuevo miembro de una cadena de bloques y el consorcio en Azure Blockchain Service mediante Azure Portal.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-blockchain-member"></a>Creación de un miembro de cadena de bloques

Cree un miembro de cadena de bloques que ejecute el protocolo de libro de contabilidad de Quorum en un nuevo consorcio o existente.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
1. Seleccione **Cadena de bloques** > **Azure Blockchain Service (versión preliminar)** .

    ![Creación de un servicio](./media/create-member/create-member.png)

    Configuración | DESCRIPCIÓN
    --------|------------
    Subscription | Seleccione la suscripción de Azure que quiere usar con el servicio. Si tiene varias suscripciones, elija aquella en la que se factura el recurso.
    Resource group | Cree un nombre de grupo de recursos o use uno existente en la suscripción.
    Region | Elija la región en que desea crear el miembro. Todos los miembros del consorcio deben estar en la misma ubicación.
    Protocolo | Actualmente, la versión preliminar de Azure Blockchain Service admite el protocolo Quorum.
    Consortium | Si es un nuevo consorcio, escriba un nombre único. Si se va a unirse a un consorcio por medio de una invitación, elija dicho consorcio.
    NOMBRE | Elija un nombre único para el miembro de Azure Blockchain Service. El nombre del miembro de la cadena de bloques solo puede contener letras minúsculas y números. El primer carácter debe ser una letra. El valor debe tener entre 2 y 20 caracteres.
    Member account password (Contraseña de la cuenta del miembro) | cifrar la clave privada de la cuenta de Ethereum que se crea para el miembro. Usará la cuenta del miembro y la contraseña de la cuenta del miembro para la administración del consorcio.
    DESCRIPCIÓN | Descripción del consorcio.
    Precios | La configuración de nodo y el costo del nuevo servicio. Seleccione el vínculo **Cambiar** para elegir entre los niveles **Estándar** y **Básico**.
    Contraseña del nodo | La contraseña del nodo de transacción predeterminado del miembro. Use la contraseña para la autenticación básica al conectarse al punto de conexión público del nodo de transacción predeterminado del miembro de la cadena de bloques.

1. Seleccione **Revisar y crear** para validar la configuración. Seleccione **Crear** para aprovisionar el servicio. El aprovisionamiento tarda aproximadamente 10 minutos.
1. Para supervisar el proceso de implementación, seleccione **Notificaciones** en la barra de herramientas.
1. Después de la implementación, vaya al miembro de cadena de bloques.

Seleccione **Información general** para ver la información básica sobre el servicio, como la dirección y la cuenta del miembro RootContract.

![Información general sobre los miembros de cadena de bloques](./media/create-member/overview.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede usar el miembro que ha creado en el siguiente inicio rápido o tutorial. Cuando ya no necesite los recursos, puede eliminarlos; para ello, elimine el grupo de recursos `myResourceGroup` que creó para el inicio rápido.

Para eliminar el grupo de recursos:

1. En Azure Portal, vaya a **Grupo de recursos** en el panel de navegación izquierdo y seleccione el grupo de recursos que desea eliminar.
2. Seleccione **Eliminar grupo de recursos**. Compruebe la eliminación escribiendo el nombre del grupo de recursos y seleccionando **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha implementado un miembro de Azure Blockchain Service y un nuevo consorcio. Pruebe el siguiente inicio rápido para usar Azure Blockchain Development Kit for Ethereum para establecer la asociación con un consorcio en Azure Blockchain Service.

> [!div class="nextstepaction"]
> [Uso de Visual Studio Code para conectarse a Azure Blockchain Service](connect-vscode.md)
