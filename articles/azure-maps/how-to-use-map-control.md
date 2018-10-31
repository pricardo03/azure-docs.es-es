---
title: Uso del Control de mapa de Azure Maps | Microsoft Docs
description: Aprenda a usar la biblioteca Javascript del lado cliente del Control de mapa de Azure Maps.
author: dsk-2015
ms.author: dkshir
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 850f9b28c112c11fd98a8abc81a1811cd26d81cc
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166039"
---
# <a name="use-the-azure-maps-map-control"></a>Uso del Control de mapa de Azure Maps

La biblioteca de Javascript del lado cliente del Control de mapa le permite representar mapas y la funcionalidad insertada de Azure Maps en su aplicación web o móvil.

## <a name="create-a-new-map-in-a-web-page"></a>Creación de un nuevo mapa en una página web

Puede insertar un mapa en una página web mediante la biblioteca de Javascript del lado cliente de Control de mapa.

1. Cree un nuevo archivo y asígnele el nombre **MapSearch.html**.

2. Agregue las referencias de hoja de estilos y origen de script de Azure Maps al elemento `<head>` del archivo:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
    ```

3. Para representar un nuevo mapa en el explorador, agregue una referencia **#map** en el elemento `<style>`:

    ```html
    <style>
        #map {
            width: 100%;
            height: 100%;
        }
    </style>
    ```

4. Para inicializar el control de mapa, defina una nueva sección en el cuerpo HTML y cree un script. Utilice su propia clave de la cuenta de Azure Maps en el script. Si necesita crear una cuenta o buscar la clave, consulte [Cómo administrar su cuenta y claves de Azure Maps](how-to-manage-account-keys.md). El método **setLanguage** especifica el idioma que se usará para las etiquetas de mapa y los controles. Para información sobre los idiomas admitidos, consulte [Idiomas admitidos en Azure Maps](https://docs.microsoft.com/azure/azure-maps/supported-languages).

    ```html
    <div id="map">
        <script>
            atlas.setSubscriptionKey("<_your account key_>");
            atlas.setLanguage("en");
            var map = new atlas.Map("map", {
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```

5. Abra el archivo en el explorador web y vea el mapa representado.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo crear un mapa con un ejemplo completo:

> [!div class="nextstepaction"]
> [Creación de un mapa](map-create.md)

Obtenga información sobre cómo aplicar estilos en un mapa:

> [!div class="nextstepaction"]
> [Elección de un estilo de mapa](choose-map-style.md)
