---
title: Búsqueda interactiva de mapas con Azure Maps | Microsoft Docs
description: 'Inicio rápido de Azure: Creación de una demostración de búsqueda de interactiva de mapas con Azure Maps'
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: quickstart
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 6afe76aca388f1f6bd479f53eb4e18cc62c10584
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59268677"
---
# <a name="create-an-interactive-search-map-by-using-azure-maps"></a>Creación de una búsqueda interactiva de mapas mediante Azure Maps

En este artículo se muestran las funcionalidades de Azure Maps para crear un mapa que dé a los usuarios una experiencia de búsqueda interactiva. Le guía por estos pasos básicos:
* Cree su propia cuenta de Azure Maps.
* Obtenga la clave de cuenta que se usará en la aplicación web de demostración.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-an-account-and-get-your-key"></a>Creación de una cuenta y obtención de la clave

1. En la esquina superior izquierda de [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso**.
2. En el campo **Buscar en Marketplace**, escriba **Maps**.
3. En **Resultados**, seleccione **Maps**. Haga clic en el botón **Crear** que aparece debajo del mapa.
4. En la página **Crear una cuenta de Azure Maps**, escriba los siguientes valores:
   - El **nombre** de la nueva cuenta.
   - La **suscripción** que quiere usar para esta cuenta.
   - El **Grupo de recursos** para esta cuenta. Puede elegir **Crear nuevo** o **Usar existente** para el grupo de recursos.
   - Seleccione el **plan de tarifa** que prefiera.
   - Lea las secciones **Licencia** y **Declaración de privacidad**. Active la casilla para aceptar los términos.
   - Finalmente, seleccione el botón **Crear**.

     ![Creación de una cuenta de Azure Maps en el portal](./media/quick-demo-map-app/create-account.png)

5. Una vez que la cuenta se ha creado correctamente, ábrala y busque la sección de configuración del menú de la cuenta. Haga clic en **Claves** para ver las claves principal y secundaria de la cuenta de Azure Maps. Copie el valor de la **clave principal** en el Portapapeles local para usarlo en la sección siguiente.

## <a name="download-the-application"></a>Descarga de la aplicación

1. Descargue o copie el contenido del archivo [interactiveSearch.html](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/interactiveSearch.html).
2. Guarde el contenido de este archivo localmente como **AzureMapDemo.html**. Ábralo en un editor de texto.
3. Busque la cadena `<Your Azure Maps Key>`. Reemplácela por el valor de **Clave principal** de la sección anterior.

## <a name="open-the-application"></a>Abrir la aplicación

1. Abra el archivo **AzureMapDemo.html** en un explorador de su preferencia.
2. Observe el mapa que se muestra de la Ciudad de Los Ángeles. Acerque y aleje para ver de qué manera el mapa se representa automáticamente con más o menos información según el nivel de zoom. 
3. Cambie el centro predeterminado del mapa. En el archivo **AzureMapDemo.html**, busque la variable denominada **center**. Reemplace el par de valores de longitud y latitud de esta variable con los nuevos valores **[-74.0060, 40.7128]**. Guarde el archivo y actualice el explorador.
4. Pruebe la experiencia de búsqueda interactiva. En el cuadro de búsqueda de la esquina superior izquierda de la aplicación web de demostración, busque **restaurants**.
5. Mueva el mouse sobre la lista de direcciones y ubicaciones que aparecen debajo del cuadro de búsqueda. Observe cómo el correspondiente alfiler en el mapa muestra información sobre esa ubicación. Para proteger la privacidad de empresas privadas, se muestran nombres y direcciones ficticios.

    ![Aplicación web de búsqueda interactiva](./media/quick-demo-map-app/interactive-search.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Los tutoriales incluyen detalles sobre cómo usar y configurar Azure Maps con su cuenta. Si tiene pensado seguir con los tutoriales, no elimine los recursos que se crearon con este inicio rápido. Si no tiene pensado continuar, siga estos pasos para eliminar los recursos:

1. Cierre el explorador donde se ejecuta la aplicación web **AzureMapDemo.html**.
2. Seleccione **Todos los recursos** en el menú izquierdo de Azure Portal. A continuación, seleccione la cuenta de Azure Maps. En la parte superior de la hoja **Todos los recursos**, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, creó una cuenta de Azure Maps y una aplicación de demostración. Eche un vistazo a los siguientes tutoriales para obtener información sobre Azure Maps:

> [!div class="nextstepaction"]
> [Búsqueda de puntos cercanos de interés mediante Azure Maps](tutorial-search-location.md)

Para ver más ejemplos de código y obtener una experiencia de codificación interactiva, consulte estas guías:

> [!div class="nextstepaction"]
> [Búsqueda de una dirección mediante el servicio de búsqueda de Azure Maps](how-to-search-for-address.md)

> [!div class="nextstepaction"]
> [Uso del Control de mapa de Azure Maps](how-to-use-map-control.md)
