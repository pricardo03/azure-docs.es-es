---
title: Administración de las claves de punto de conexión de LUIS | Microsoft Docs
description: Use Language Understanding (LUIS) para administrar las claves de API de programación, punto de conexión y externas.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: diberry
ms.openlocfilehash: 127c09a022f5efb95ab6a5ec2db0de633b437a54
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223045"
---
# <a name="manage-your-luis-endpoint-keys"></a>Administración de las claves de punto de conexión de LUIS
Una clave permite crear y publicar la aplicación de LUIS, o bien consultar el punto de conexión. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
## <a name="key-concepts"></a>Conceptos clave
Vea [Keys in LUIS](luis-concept-keys.md) (Claves en LUIS) para comprender los conceptos de claves de creación y punto de conexión de LUIS.

<a name="create-and-use-an-endpoint-key"></a>
## <a name="assign-endpoint-key"></a>Asignar la clave de punto de conexión
En la página **Publicar aplicación**, ya hay una clave en la tabla **Resources and Keys** (Recursos y claves). Esta es la clave de creación (inicio). 

1. Cree una clave de LUIS en [Azure Portal](https://portal.azure.com). Para más instrucciones, consulte [Creating an endpoint key using Azure](luis-how-to-azure-subscription.md) (Creación de una clave de punto de conexión mediante Azure).
 
2. Para agregar la clave de LUIS que ha creado en el paso anterior, haga clic en el botón **Agregar clave** para abrir el cuadro de diálogo **Assign a key to your app** (Asignar una clave a la aplicación). 

    ![Asignar una clave a la aplicación](./media/luis-manage-keys/assign-key.png)
3. Seleccione un inquilino en el cuadro de diálogo. 
 
    > [!Note]
    > En Azure, un inquilino representa el identificador de Azure Active Directory del cliente o la organización que está asociado con un servicio. Si se ha registrado anteriormente en una suscripción a Azure con una cuenta de Microsoft individual, ya dispone de un inquilino. Cuando inicia sesión en Azure Portal, se registra automáticamente en [su inquilino predeterminado](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant). Puede usar este inquilino como considere oportuno, pero es posible que quiera crear una cuenta de administrador organizativo.

4. Elija la suscripción a Azure asociada con la clave de LUIS de Azure que quiera agregar.

5. Seleccione la cuenta de LUIS de Azure. La región de la cuenta se muestra entre paréntesis. 

    ![Elegir la clave](./media/luis-manage-keys/assign-key-filled-out.png)

6. Después de asignar esta clave de punto de conexión, úsela en todas las consultas de punto de conexión. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="publishing-regions"></a>Regiones de publicación
Obtenga más información sobre las [regiones](luis-reference-regions.md) de publicación, incluida la publicación en [Europa](luis-reference-regions.md#publishing-to-europe) y [Australia](luis-reference-regions.md#publishing-to-australia). Las regiones de publicación son diferentes de las regiones de creación. Asegúrese de que crea una aplicación en la región de creación correspondiente a la región de publicación que quiera.

## <a name="unassign-key"></a>Anular la asignación de una clave

* En la **lista de recursos y claves**, haga clic en el icono de la papelera situado junto a la entidad a la que quiera anular la asignación. Después, haga clic en **Aceptar** en el mensaje de confirmación para confirmar la eliminación.
 
    ![Anular la asignación de una entidad](./media/luis-manage-keys/unassign-key.png)

> [!NOTE]
> Si anula la asignación de la clave de LUIS, no la elimina de la suscripción a Azure.

## <a name="next-steps"></a>Pasos siguientes

Use su clave para publicar la aplicación en la página **Publicar aplicación**. Para obtener instrucciones sobre cómo publicarla, vea [Publish app](luis-how-to-publish-app.md) (Publicar la aplicación).