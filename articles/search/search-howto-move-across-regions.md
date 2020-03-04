---
title: Procedimiento para trasladar el recurso de servicio entre regiones
titleSuffix: Azure Cognitive Search
description: En este artículo se mostrará cómo trasladar los recursos de Azure Cognitive Search de una región a otra en la nube de Azure.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 02/18/2020
ms.openlocfilehash: 392c86d8ea24e59d388926d4df581305ea2b531d
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599637"
---
# <a name="move-your-azure-cognitive-search-service-to-another-azure-region"></a>Traslado del servicio Azure Cognitive Search a otra región de Azure

Para trasladar la cuenta del servicio Azure Cognitive de una región a otra, creará una plantilla de exportación para trasladar sus suscripciones. Después de mover la suscripción, tendrá que mover los datos y volver a crear el servicio.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Exportar una plantilla.
> * Modificar la plantilla al agregar los nombres de las cuentas de búsqueda y almacenamiento y la región de destino.
> * Implementar la plantilla para crear las nuevas cuentas de búsqueda y almacenamiento.
> * Comprobar el estado del servicio en la región nueva.
> * Eliminar los recursos en la región de origen.

## <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que los servicios y las características que usa su cuenta se admitan en la región de destino.

- En el caso de las características en vista previa (GB), asegúrese de que la suscripción está en la lista de permitidos para la región de destino. Para más información sobre las características en vista previa (GB), consulte los [almacenes de conocimiento](https://docs.microsoft.com/azure/search/knowledge-store-concept-intro), el [enriquecimiento incremental](https://docs.microsoft.com/azure/search/cognitive-search-incremental-indexing-conceptual) y el [punto de conexión privado](https://docs.microsoft.com/azure/search/service-create-private-endpoint).

## <a name="assessment-and-planning"></a>Evaluación y planeamiento

Al trasladar el servicio de búsqueda a la región nueva, tendrá que [trasladar los datos al servicio de almacenamiento nuevo](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#configure-the-new-storage-account) y, a continuación, recompilar los índices, los conjuntos de aptitudes y los almacenes de conocimiento. Debe registrar la configuración actual y copiar los archivos json para que la recompilación del servicio sea más sencilla y rápida.

## <a name="moving-your-search-services-resources"></a>Traslado de los recursos del servicio de búsqueda

Para empezar, exportará y luego modificará una plantilla de Resource Manager.

### <a name="export-a-template"></a>Exportación de una plantilla

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Vaya a la página Grupo de recursos.

> [!div class="mx-imgBorder"]
> ![Ejemplo de la página Grupo de recursos](./media/search-move-resource/export-template-sample.png)

3. Seleccione **Todos los recursos**.

3. En el menú de navegación de la izquierda, seleccione **Exportar plantilla**.

4. Elija **Descargar** en la página **Exportar plantilla**.

5. Busque el archivo ZIP que descargó desde el portal y descomprímalo en la carpeta que prefiera.

El archivo ZIP contiene los archivos .json que componen la plantilla y los scripts para implementar la plantilla.

### <a name="modify-the-template"></a>Modificación de la plantilla

Modificará la plantilla cambiando las regiones y los nombres de las cuentas de búsqueda y almacenamiento. Los nombres deben seguir las reglas de las convenciones de nomenclatura de cada servicio y región. 

Para obtener los códigos de ubicación de la región, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).  El código de una región es el nombre de la región sin espacios, **Centro de EE. UU.**  = **centralus**.

1. En Azure Portal, haga clic en **Crear un recurso**.

2. En **Buscar en Marketplace**, escriba **implementación de plantillas** y, después, presione **ENTRAR**.

3. Seleccione **Implementación de plantillas**.

4. Seleccione **Crear**.

5. Seleccione **Cree su propia plantilla en el editor**.

6. Seleccione **Cargar archivo** y, después, siga las instrucciones para cargar el archivo **template.json** que descargó y descomprimió en la sección anterior.

7. En el archivo **template.json**, asigne un nombre a las cuentas de búsqueda y almacenamiento de destino mediante el establecimiento del valor predeterminado de los nombres de las cuentas de búsqueda y almacenamiento. 

8. Edite la propiedad **location** en el archivo **template.json** en la región de destino para el servicio de búsqueda y el de almacenamiento. En este ejemplo, la región de destino se establece en `centralus`.

```json
},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Search/searchServices",
            "apiVersion": "2015-08-19",
            "name": "[parameters('searchServices_target_region_search_name')]",
            "location": "centralus",
            "sku": {
                "name": "standard"
            },
            "properties": {
                "replicaCount": 1,
                "partitionCount": 1,
                "hostingMode": "Default"
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[parameters('storageAccounts_tagetstorageregion_name')]",
            "location": "centralus",
            "sku": {
                "name": "Standard_RAGRS",
                "tier": "Standard"
            },
```

### <a name="deploy-the-template"></a>Implementación de la plantilla

1. Guarde el archivo **template.json**.

2. Escriba o seleccione los valores de propiedad:

- **Suscripción**: Seleccione una suscripción de Azure.

- **Grupo de recursos**: Seleccione **Crear nuevo** y asígnele un nombre al grupo de recursos.

- **Ubicación**: Seleccione una ubicación de Azure.

3. Active la casilla **Acepto los términos y condiciones indicados anteriormente** y haga clic en el botón **Select Purchase** (Seleccionar compra).

## <a name="verifying-your-services-status-in-new-region"></a>Comprobación del estado de los servicios en la región nueva

Para comprobar el movimiento, abra el grupo de recursos nuevo y los servicios se mostrarán con la región nueva.

Para mover los datos de la región de origen a la región de destino, consulte las instrucciones que aparecen en este artículo para [trasladar los datos a la cuenta de almacenamiento nueva](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#move-data-to-the-new-storage-account).

## <a name="clean-up-resources-in-your-original-region"></a>Limpieza de los recursos en la región original

Para confirmar los cambios y completar el traslado de la cuenta de servicio, elimine la cuenta de servicio de origen.

## <a name="next-steps"></a>Pasos siguientes

[Creación de un índice](https://docs.microsoft.com/azure/search/search-get-started-portal)

[Creación de un conjunto de aptitudes](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob)

[Creación de un almacén de conocimiento](https://docs.microsoft.com/azure/search/knowledge-store-create-portal)

