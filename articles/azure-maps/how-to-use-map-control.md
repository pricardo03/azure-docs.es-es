---
title: Uso del Control de mapa de Azure Maps | Microsoft Docs
description: Aprenda a usar la biblioteca Javascript del lado cliente del Control de mapa de Azure Maps.
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 619dda44b5ad74ba1dcb62cfb3318687aa6ec4d5
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43340142"
---
# <a name="how-to-use-the-azure-maps-map-control"></a>Cómo usar el Control de mapa de Azure Maps
La biblioteca de Javascript del lado cliente del Control de mapa le permite representar mapas y la funcionalidad insertada de Azure Maps en su aplicación web o móvil. 

## <a name="create-a-new-map-in-a-web-page"></a>Creación de un nuevo mapa en una página web

Puede insertar un mapa en una página web mediante la biblioteca de Javascript del lado cliente de Control de mapa.

1. Cree un nuevo archivo y asígnele el nombre MapSearch.html.

2. Agregue las referencias de hoja de estilos y origen de script de Azure Maps al elemento `<head>` del archivo:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Para representar un nuevo mapa en el explorador, agregue una referencia **#map** en el elemento `<style>`.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Para inicializar el control de mapa, defina una nueva sección en el cuerpo HTML y cree un script. Utilice su propia clave de la cuenta de Azure Maps en el script. Si necesita crear una cuenta o buscar la clave, consulte [Cómo administrar su cuenta y claves de Azure Maps](how-to-manage-account-keys.md)

    ```html
    <div id="map">
        <script>
            var MapsAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": MapsAccountKey,
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Abra el archivo en el explorador web y vea el mapa representado.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se ha explicado cómo crear un mapa básico con su clave de Azure Maps. Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos: 

* [Creación de un mapa](map-create.md)
* [Elección de un estilo de mapa](choose-map-style.md)
