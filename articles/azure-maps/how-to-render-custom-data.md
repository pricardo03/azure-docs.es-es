---
title: Representación de datos personalizados en un mapa de trama en Azure Maps | Microsoft Docs
description: Represente datos personalizados en un mapa de trama en Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 9b3c0f7b1ff56cb269f6852be8fd2affeca8b8f1
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56119157"
---
# <a name="render-custom-data-on-raster-map"></a>Representación de datos personalizados en un mapa de trama

En este artículo se explica cómo usar el [servicio de imagen estática](https://docs.microsoft.com/rest/api/maps/render/getmapimage) con la funcionalidad de composición de imágenes para admitir las superposiciones encima de un mapa de trama. La composición de imágenes incluye la capacidad de obtener un icono de mapa de trama con datos adicionales, como marcadores personalizados, etiquetas y superposiciones geométricas. Para representar marcadores, etiquetas y geometrías personalizadas, usaremos la aplicación Postman. Abra la aplicación Postman, haga clic en Nuevo | Crear nuevo, seleccione el nombre de una colección o carpeta donde guardar y haga clic en Guardar.

Usaremos las [API del servicio de datos](https://docs.microsoft.com/rest/api/maps/data) de Azure Maps para almacenar y representar superposiciones. 


## <a name="prerequisites"></a>Requisitos previos

### <a name="create-an-azure-maps-account"></a>Crear una cuenta de Azure Maps 

Para seguir los pasos de esta guía, primero necesita consultar el artículo sobre cómo [administrar la cuenta y las claves](how-to-manage-account-keys.md) para crear y administrar la suscripción de la cuenta con el plan de tarifa S1.

## <a name="render-pushpins-with-labels-and-custom-image"></a>Representación de marcadores con etiquetas e imágenes personalizadas

> [!Note]
> Este ejemplo requiere una cuenta de Azure Maps con un plan de tarifa S0 o S1. 

La SKU del plan de tarifa S0 de la cuenta de Azure Maps solo admite una única instancia del parámetro `pins`, que permite a los usuarios representar hasta cinco marcadores especificados en la solicitud de la dirección URL con una imagen personalizada.

Para representar marcadores con etiquetas e imágenes personalizadas, siga estos pasos:

1. Abra la aplicación Postman, haga clic en New | Create new (Nuevo | Crear nuevo) y seleccione Request (Solicitud). Escriba un nombre de solicitud para la representación de marcadores, seleccione una colección o carpeta donde guardarlo y haga clic en Save (Guardar).
    
    ![Carga de geovallas con Postman](./media/tutorial-geofence/postman-new.png)

2. Seleccione el método GET HTTP en la pestaña del generador y escriba la siguiente dirección URL para realizar una solicitud GET.

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.98,%2040.77&pins=custom%7Cla15+50%7Cls12%7Clc003b61%7C%7C%27CentralPark%27-73.9657974+40.781971%7C%7Chttp%3A%2F%2Fazuremapscodesamples.azurewebsites.net%2FCommon%2Fimages%2Fpushpins%2Fylw-pushpin.png
    ```
    A continuación, se muestra la imagen de respuesta que obtendrá.

    ![representar marcadores personalizados con etiquetas](./media/how-to-render-custom-data/render-pins.png)


## <a name="get-data-from-azure-maps-data-storage"></a>Obtener datos del almacenamiento de datos de Azure Maps

> [!Note]
> Este ejemplo requiere una cuenta de Azure Maps con un plan de tarifa S1.

La información sobre la ubicación de los marcadores y la ruta de acceso también se puede obtener con [Data Upload API](https://docs.microsoft.com/rest/api/maps/mapdata/upload). Siga los pasos descritos a continuación para cargar los datos de la ruta de acceso y de los marcadores.

1. En la aplicación Postman, abra una nueva pestaña en la misma colección que ha creado anteriormente. Seleccione el método POST HTTP en la pestaña del generador y escriba la siguiente URL para realizar una solicitud POST:

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

2. Haga clic en Params (Parámetros) y escriba los siguientes pares de clave-valor que se usarán para la dirección URL de la solicitud POST. Reemplace el valor de subscription-key por la clave de suscripción de Azure Maps.
    
    ![Parámetros de clave-valor de Postman](./media/how-to-render-custom-data/postman-key-vals.png)

3. Haga clic en **Body** (Cuerpo), seleccione el formato de entrada sin procesar y elija JSON como formato de entrada en la lista desplegable. Proporcione el siguiente código JSON como datos que se van a cargar:
    
    ```JSON
    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -73.98235,
                  40.76799
                ],
                [
                  -73.95785,
                  40.80044
                ],
                [
                  -73.94928,
                  40.7968
                ],
                [
                  -73.97317,
                  40.76437
                ],
                [
                  -73.98235,
                  40.76799
                ]
              ]
            ]
          }
        },
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "LineString",
            "coordinates": [
              [
                -73.97624731063843,
                40.76560773817073
              ],
              [
                -73.97914409637451,
                40.766826609362575
              ],
              [
                -73.98513078689575,
                40.7585866048861
              ]
            ]
          }
        }
      ]
    }
    ```

4. Haga clic en Send (Enviar) y revise el encabezado de la respuesta. El encabezado de ubicación contiene el identificador URI para acceder o descargar los datos para su uso futuro. También contiene un parámetro `udId` único para los datos cargados.   

  ```HTTP
  https://atlas.microsoft.com/mapData/{udId}/status?api-version=1.0&subscription-key={Subscription-key}
  ```



5. Use el valor de `udid` obtenido de Data Upload API para representar características en el mapa. Para ello, abra una nueva pestaña en la misma colección que ha creado anteriormente. Seleccione el método GET HTTP en la pestaña del generador y escriba la siguiente dirección URL para realizar una solicitud GET:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.96682739257812%2C40.78119135317995&pins=default|la-35+50|ls12|lc003C62|co9B2F15||'Times Square'-73.98516297340393 40.758781646381024|'Central Park'-73.96682739257812 40.78119135317995&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.30||udid-{udId}
    ```

6. La imagen de respuesta debería presentar un aspecto similar al siguiente:

    ![representar datos cargados](./media/how-to-render-custom-data/uploaded-path.png)

## <a name="render-polygon-with-color-and-opacity"></a>Representar polígono con color y opacidad

> [!Note]
> Este ejemplo requiere una cuenta de Azure Maps con un plan de tarifa S1.

Puede modificar la apariencia de un polígono mediante el uso de modificadores de estilo con el [parámetro path](https://docs.microsoft.com/rest-staging/api/maps/render/getmapimage#uri-parameters).

1. En la aplicación Postman, abra una nueva pestaña en la misma colección que ha creado anteriormente. Seleccione el método GET HTTP en la pestaña del generador y escriba la siguiente dirección URL para realizar una solicitud GET a fin de representar un polígono con color y opacidad:
    
    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&sku=S1&zoom=14&height=500&Width=500&center=-74.040701, 40.698666&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.50||-74.03995513916016 40.70090237454063|-74.04082417488098 40.70028420372218|-74.04113531112671 40.70049568385827|-74.04298067092896 40.69899904076542|-74.04271245002747 40.69879568992435|-74.04367804527283 40.6980961582905|-74.04364585876465 40.698055487620714|-74.04368877410889 40.698022951066996|-74.04168248176573 40.696444909137|-74.03901100158691 40.69837271818651|-74.03824925422668 40.69837271818651|-74.03809905052185 40.69903971085914|-74.03771281242369 40.699340668780984|-74.03940796852112 40.70058515602143|-74.03948307037354 40.70052821920425|-74.03995513916016 40.70090237454063
    &subscription-key={subscription--key}
    ```

La imagen de respuesta debería presentar un aspecto similar al siguiente:

![representar un polígono opaco](./media/how-to-render-custom-data/opaque-polygon.png)


## <a name="render-polygon-with-circle-and-push-pins-with-custom-labels"></a>Representar un polígono con círculos y marcadores con etiquetas personalizadas

> [!Note]
> Este ejemplo requiere una cuenta de Azure Maps con un plan de tarifa S1.

Puede aumentar o reducir el tamaño de los marcadores y sus etiquetas con el modificador de estilo de escala "sc". Debe ser un valor mayor que cero. Un valor de 1 es la escala estándar. Los valores mayores que 1 aumentarán el tamaño de los marcadores, mientras que los valores menores que 1 reducirán su tamaño. Para más información sobre los modificadores de estilo, vea los [parámetros path del servicio de imagen estática](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters).

Siga los pasos descritos a continuación para representar un polígono con círculos y marcadores con etiquetas personalizadas:

1. En la aplicación Postman, abra una nueva pestaña en la misma colección que ha creado anteriormente. Seleccione el método GET HTTP en la pestaña del generador y escriba la siguiente dirección URL para realizar una solicitud GET:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co002D62||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

La imagen de respuesta debería presentar un aspecto similar al siguiente:

![representar círculo con marcadores personalizados](./media/how-to-render-custom-data/circle-custom-pins.png)

## <a name="next-steps"></a>Pasos siguientes

* Explore la documentación sobre [Get Map Image API de Azure Maps Get](https://docs.microsoft.com/rest/api/maps/search).
* Para más información sobre las funcionalidades del servicio de datos de Azure Maps, vea la [documentación sobre el servicio](https://docs.microsoft.com/rest/api/maps/data).