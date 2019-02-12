---
title: Importación de una instancia de Azure Function App como API en Azure API Management | Microsoft Docs
description: En este tutorial se muestra cómo importar una instancia de Azure Function App en Azure API Management como API.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: apimpm
ms.openlocfilehash: e86bd797774448d8e4821ff02d358d420a099442
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55810786"
---
# <a name="import-an-azure-function-app-as-an-api-in-azure-api-management"></a>Importación de una instancia de Azure Function App como API en Azure API Management

Azure API Management admite la importación de instancias de Azure Function App como API nuevas o su anexión a las API existentes. El proceso genera automáticamente una clave de host en Azure Function App que, después, se asigna a un valor con nombre en Azure API Management.

En este artículo se explica cómo importar una instancia de Azure Function App como API en Azure API Management. También se describe el proceso de realización de pruebas.

Aprenderá a:

> [!div class="checklist"]
> * Importar una instancia de Azure Function App como una API
> * Anexar una instancia de Azure Function App a una API
> * Ver la nueva clave de host de Azure Function App y el valor con nombre de Azure API Management
> * Prueba de la API en Azure Portal
> * Prueba de la API en el portal para desarrolladores

## <a name="prerequisites"></a>Requisitos previos

* Complete la guía de inicio rápido [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
* Asegúrese de que tiene una aplicación de Azure Functions en la suscripción. Para más información, consulte [Creación de una instancia de Azure Function App](../azure-functions/functions-create-first-azure-function.md#create-a-function-app). Debe contener funciones con desencadenador HTTP y la configuración del nivel de autorización debe establecerse en *Anónimo* o *Función*.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-new-api-from-azure-function-app"></a> Importación de una instancia de Azure Function App como una API nueva

Siga estos pasos para crear una API desde una instancia de Azure Function App.

1. En la instancia del servicio **Azure API Management**, seleccione **API** en el menú de la izquierda.

2. En la lista **Add a new API** (Agregar una nueva API), seleccione **Function App**.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-01.png)

3. Haga clic en **Examinar** para seleccionar las funciones que se van a importar.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-02.png)

4. Haga clic en la sección **Function App** para elegir en la lista de instancias de Function App.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-03.png)

5. Busque la instancia de Function App de la que desea importar funciones, haga clic en ella y presione **Seleccionar**.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-04.png)

6. Seleccione las funciones que desea importar y haga clic en **Seleccionar**.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-05.png)

    > [!NOTE]
    > Solo se pueden importar funciones que se basen en desencadenador HTTP y que tengan la configuración del nivel de autorización establecida en *Anónimo* o *Función*.

7. Cambie a la vista **Completa** y asigne el **Producto** a la nueva API. Si es necesario, edite otros campos que se rellenan automáticamente.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-06.png)

8. Haga clic en **Create**(Crear).

## <a name="append-azure-function-app-to-api"></a>Anexión de una instancia de Azure Function App a una API existente

Siga estos pasos para anexar Azure Function App a una API existente.

1. En la instancia del servicio **Azure API Management**, seleccione **API** en el menú de la izquierda.

2. Elija una API que desea importar en una instancia de Azure Function App. Haga clic en **...**  y seleccione **Importar** en el menú contextual.

    ![Anexar desde Function App](./media/import-function-app-as-api/append-01.png)

3. Haga clic en el icono **Function App**.

    ![Anexar desde Function App](./media/import-function-app-as-api/append-02.png)

4. En la ventana emergente, haga clic en **Examinar**.

    ![Anexar desde Function App](./media/import-function-app-as-api/append-03.png)

5. Haga clic en la sección **Function App** para elegir en la lista de instancias de Function App.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-03.png)

6. Busque la instancia de Function App de la que desea importar funciones, haga clic en ella y presione **Seleccionar**.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-04.png)

7. Seleccione las funciones que desea importar y haga clic en **Seleccionar**.

    ![Agregar desde Function App](./media/import-function-app-as-api/add-05.png)

8. Haga clic en **Import**.

    ![Anexar desde Function App](./media/import-function-app-as-api/append-04.png)

## <a name="function-app-import-keys"></a> Clave de host generada de Azure Function App

La importación de una instancia de Azure Function App genera automáticamente:
* una tecla del host en Function App con el nombre apim-{*nombre de instancia del servicio Azure API Management*},
* el valor con nombre dentro de la instancia de Azure API Management con el nombre {*nombre de la instancia de Azure Function App*}-clave, que contiene la clave de host creada.

> [!WARNING]
> Si se quita o se cambia el valor de la clave de host de Azure Function App o el valor con nombre de Azure API Management, se interrumpirá la comunicación entre los servicios. Los valores no se sincronizan automáticamente.
>
> Si necesita rotar la clave de host, asegúrese de que también se modifica el valor con nombre de Azure API Management.

### <a name="access-azure-function-app-host-key"></a>Acceso a la clave de host de Azure Function App

1. Vaya a la instancia de Azure Function App.

2. Seleccione **Configuración de Function App** en la información general.

    ![Agregar desde Function App](./media/import-function-app-as-api/keys-02-a.png)

3. La clave se encuentra en la sección **Host Keys** (Claves de host).

    ![Agregar desde Function App](./media/import-function-app-as-api/keys-02-b.png)

### <a name="access-the-named-value-in-azure-api-management"></a>Acceso al valor con nombre en Azure API Management

Vaya a la instancia de Azure API Management y seleccione **Valores con nombre** en el menú de la izquierda. La clave de Azure Function App se almacena ahí.

![Agregar desde Function App](./media/import-function-app-as-api/keys-01.png)

## <a name="test-in-azure-portal"></a> Prueba de la nueva API Management en Azure Portal

Puede llamar a operaciones directamente desde Azure Portal. Mediante Azure Portal es una manera cómoda de ver y probar las operaciones de una API.  

1. Seleccione la API que ha creado en la sección anterior.

2. Seleccione la pestaña **Prueba**.

3. Seleccione una operación.

    La página muestra los campos de parámetros de consulta y los campos para los encabezados. Uno de los encabezados es **Ocp-Apim-Suscripción-Key** para la clave de suscripción del producto que está asociado a esta API. Si ha creado la instancia de API Management, significa que ya es administrador, por lo que la clave se rellena automáticamente. 

4. Seleccione **Enviar**.

    El back-end responde con **200 Aceptar** y algunos datos.

## <a name="test-in-developer-portal"></a>Llamada a una operación desde el portal para desarrolladores

También puede llamar a las operaciones desde el portal para desarrolladores para probar las API. 

1. Seleccione la API que ha creado en el paso Importación y publicación de una API de back-end.

2. Seleccione **Portal para desarrolladores**.

    Se abre el sitio del portal para desarrolladores.

3. Seleccione la **API** que ha creado.

4. Seleccione la operación que desea probar.

5. Seleccione **Pruébelo**.

6. Seleccione **Enviar**.
    
    Después de invocar una operación, el portal para desarrolladores mostrará el **estado de respuesta**, los **encabezados de respuesta** y el **contenido de respuesta**.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Transformación y protección de una API publicada](transform-api.md)
