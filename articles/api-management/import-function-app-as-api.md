---
title: Importación de una instancia de Function App como una API desde Azure Portal | Microsoft Docs
description: En este tutorial se muestra cómo utilizar API Management (APIM) para importar una aplicación de función como una API.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/15/2018
ms.author: apimpm
ms.openlocfilehash: 0ee83446bb08e66c7f325bdd5585b8cc0484a74e
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090935"
---
# <a name="import-a-function-app-as-an-api"></a>Importación de una aplicación de función como una API

Este artículo muestra cómo importar una aplicación de función como una API. El artículo también muestra cómo probar la API de APIM.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Importación de una aplicación de función como una API
> * Prueba de la API en Azure Portal
> * Pruebe la API en el Portal para desarrolladores

## <a name="prerequisites"></a>Requisitos previos

+ Completar la guía de inicio rápido siguiente: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)
+ Asegúrese de que haya una instancia de Azure Function App en su suscripción. Para más información, consulte [Creación de una aplicación de función](../azure-functions/functions-create-first-azure-function.md#create-a-function-app)
+ [Creación de una definición de OpenAPI](../azure-functions/functions-openapi-definition.md) de su instancia de Azure Function App

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importación y publicación de una API de back-end

1. Seleccione **API** en **API MANAGEMENT**.
2. Seleccione **Aplicación de función** en la lista **Add a new API** (Agregar una nueva API).

    ![Aplicación de función](./media/import-function-app-as-api/function-app-api.png)
3. Presione **Examinar** para ver la lista de aplicaciones de función en su suscripción.
4. Seleccione la aplicación. APIM busca el swagger asociado a la aplicación seleccionada, lo captura y lo importa. 
5. Agregue un sufijo URL de API. El sufijo es un nombre que identifica esta API concreta en esta instancia de APIM. Debe ser exclusivo en esta instancia de APIM.
6. Publique la API asociándola a un producto. En este caso, se usa el producto "*Unlimited*".  Si desea que la API se publique y esté disponible para los desarrolladores, agréguela a un producto. Puede hacerlo durante la creación de la API o configurarla más adelante.

    Los productos son asociaciones de una o varias API. Puede incluir varias API y ofrecerlas a los desarrolladores mediante el portal para desarrolladores. En primer lugar, los desarrolladores deben suscribirse a un producto para acceder a la API. Al suscribirse, obtienen una clave de suscripción que funciona con cualquier API de ese producto. Si creó la instancia de APIM, ya es un administrador, así que, de forma predeterminada, está suscrito a todos los productos.

    De forma predeterminada, cada instancia de API Management incluye dos productos de ejemplo:

    * **Starter**
    * **Sin límite**   
7. Seleccione **Crear**.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Relleno de las claves de Azure Functions en Azure API Management

Si las funciones importadas de Azure están protegidas con claves, Azure API Management crea automáticamente **valores con nombre** para ellas, pero no rellena las entradas con secretos. Con cada entrada, debe realizar los pasos siguientes.  

1. Vaya a la pestaña **Named values** (Valores con nombre) de la instancia de API Management.
2. Haga clic en una entrada y presione **Show value** (Mostrar valor) en la barra lateral.

    ![Valores con nombre](./media/import-function-app-as-api/apim-named-values.png)

3. Si el contenido es similar a *código de {nombre de función de Azure}*, vaya a la aplicación de Azure Functions importada y desplácese a la función de Azure.
4. Vaya a la sección **Manage** (Administrar) de la función de Azure deseada y copie la clave pertinente, según el método de autenticación de la función de Azure.

    ![Aplicación de función](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Pegue la clave en el cuadro de texto de **Named values** (Valores con nombre) y haga clic en **Save** (Guardar).

    ![Aplicación de función](./media/import-function-app-as-api/apim-named-values-2.png)

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Prueba de la nueva API APIM en Azure Portal

Se puede llamar a las operaciones directamente desde Azure Portal, lo que proporciona una forma cómoda de ver y probar las operaciones de una API.  

1. Seleccione la API que creó en los pasos anteriores.
2. Presione la pestaña **Prueba**.
3. Seleccione alguna operación.

    La página muestra los campos de parámetros de consulta y los campos para los encabezados. Uno de los encabezados es "Ocp-Apim-Suscripción-Key", para la clave de suscripción del producto que está asociado a esta API. Si ha creado la instancia APIM, ya es administrador, por lo que la clave se rellena automáticamente. 
1. Presione **Enviar**.

    Back-end responde con **200 Aceptar** y algunos datos.

## <a name="call-operation"></a>Llamada a una operación desde el portal para desarrolladores

También se pueden llamar a las operaciones desde **portal para desarrolladores** para probar las API. 

1. Seleccione la API que creó en el paso "Importación y publicación de una API de back-end".
2. Presione **Portal para desarrolladores**.

    Se abre el sitio "Portal para desarrolladores".
3. Seleccione la **API** que ha creado.
4. Haga clic en la operación que desea probar.
5. Presione **Try it** (Probarlo).
6. Presione **Enviar**.
    
    Después de invocar una operación, el portal para desarrolladores mostrará el **estado de respuesta**, los **encabezados de respuesta** y el **contenido de respuesta**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Transformación y protección de una API publicada](transform-api.md)