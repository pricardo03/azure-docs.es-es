---
title: Importación de una aplicación de Azure Functions como una API en Azure Portal | Microsoft Docs
description: En este tutorial se muestra cómo utilizar Azure API Management para importar una aplicación de Azure Functions como una API.
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
ms.openlocfilehash: 670fa58de7155028b0f72f1f819b9f269e07b9eb
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239059"
---
# <a name="import-an-azure-functions-app-as-an-api"></a>Importación de una aplicación de Azure Functions como una API

En este artículo se muestra cómo importar una aplicación de Azure Functions como una API. En el artículo también se muestra cómo probar la API Azure API Management.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Importación de una aplicación de Functions como una API
> * Prueba de la API en Azure Portal
> * Prueba de la API en el portal para desarrolladores

## <a name="prerequisites"></a>Requisitos previos

+ Complete la guía de inicio rápido [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
+ Asegúrese de que tiene una aplicación de Azure Functions en la suscripción. Para más información, consulte [Creación de una aplicación de función](../azure-functions/functions-create-first-azure-function.md#create-a-function-app).
+ [Cree una definición OpenAPI](../azure-functions/functions-openapi-definition.md) de la aplicación de Azure Functions.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importación y publicación de una API de back-end

1. En **API MANAGEMENT**, seleccione **API**.
2. En la lista **Add a new API** (Agregar una nueva API), seleccione **Aplicación de Functions**.

    ![Aplicación de Functions](./media/import-function-app-as-api/function-app-api.png)
3. Seleccione **Examinar** para ver la lista de aplicaciones de función en su suscripción.
4. Seleccione la aplicación. API Management busca el archivo de Swagger asociado a la aplicación seleccionada, lo captura y luego lo importa. 
5. Agregue un sufijo URL de API. El sufijo es un nombre que identifica esta API específica en esta instancia de API Management. El sufijo debe ser único en esta instancia de API Management.
6. Publique la API asociándola a un producto. En este caso, se usa el producto **Unlimited**. Si desea que la API se publique y esté disponible para los desarrolladores, agréguela a un producto. Puede agregar la API a un producto al crear la API, o bien puede agregarla más adelante.

    Los productos son asociaciones de una o varias API. Puede incluir varias API y ofrecerlas a los desarrolladores mediante el portal para desarrolladores. En primer lugar, los desarrolladores deben suscribirse a un producto para acceder a la API. Cuando un desarrollador se suscribe, obtiene una clave de suscripción que sea adecuada con cualquier API de ese producto. Si ha creado la instancia de API Management, significa que es un administrador. Los administradores están suscritos a todos los productos de forma predeterminada.

    De forma predeterminada, cada instancia de API Management incluye dos productos de ejemplo:

    * **Starter**
    * **Sin límite**   
7. Seleccione **Crear**.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Relleno de las claves de Azure Functions en Azure API Management

Si las aplicaciones importadas de Azure Functions están protegidas con claves, API Management crea automáticamente *valores con nombre* para ellas. API Management no rellena las entradas con secretos. Complete los siguientes pasos para cada entrada:  

1. En la instancia de API Management, seleccione la pestaña **Valores con nombre**.
2. Seleccione una entrada y, después, seleccione **Mostrar valor** en la barra lateral.

    ![Valores con nombre](./media/import-function-app-as-api/apim-named-values.png)

3. Si el texto que aparece en el cuadro **Valor** es similar al **código para el \<nombre de Azure Functions\>**, vaya a **Aplicación de Functions** y, después, vaya a **Funciones**.
4. Seleccione **Administrar** y, a continuación, copie la clave correspondiente según el método de autenticación de la aplicación.

    ![Aplicación de Functions: copiar claves](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Pegue la clave en el cuadro **Valor** y, después, seleccione **Guardar**.

    ![Aplicación de Functions: pegar valores de claves](./media/import-function-app-as-api/apim-named-values-2.png)

## <a name="test-the-new-api-management-api-in-the-azure-portal"></a>Prueba de la nueva API Management en Azure Portal

Puede llamar a operaciones directamente desde Azure Portal. Mediante Azure Portal es una manera cómoda de ver y probar las operaciones de una API.  

1. Seleccione la API que ha creado en la sección anterior.
2. Seleccione la pestaña **Prueba**.
3. Seleccione una operación.

    La página muestra los campos de parámetros de consulta y los campos para los encabezados. Uno de los encabezados es **Ocp-Apim-Suscripción-Key** para la clave de suscripción del producto que está asociado a esta API. Si ha creado la instancia de API Management, significa que ya es administrador, por lo que la clave se rellena automáticamente. 
4. Seleccione **Enviar**.

    El back-end responde con **200 Aceptar** y algunos datos.

## <a name="call-operation"></a>Llamada a una operación desde el portal para desarrolladores

También puede llamar a las operaciones desde el portal para desarrolladores para probar las API. 

1. Seleccione la API que ha creado en el paso [Importación y publicación de una API de back-end](#create-api).
2. Seleccione **Portal para desarrolladores**.

    Se abre el sitio del portal para desarrolladores.
3. Seleccione la **API** que ha creado.
4. Seleccione la operación que desea probar.
5. Seleccione **Pruébelo**.
6. Seleccione **Enviar**.
    
    Después de invocar una operación, el portal para desarrolladores mostrará el **estado de respuesta**, los **encabezados de respuesta** y el **contenido de respuesta**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Transformación y protección de una API publicada](transform-api.md)