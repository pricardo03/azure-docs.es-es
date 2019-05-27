---
title: Introducción al control de mapa de web en Azure Maps | Microsoft Docs
description: Aprenda a usar la biblioteca Javascript del lado cliente del Control de mapa de Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: aa923fa7e2d5e673e6a2db2b349e54d433d1817b
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957277"
---
# <a name="use-the-azure-maps-map-control"></a>Uso del Control de mapa de Azure Maps

La biblioteca de Javascript del lado cliente del Control de mapa le permite representar mapas y la funcionalidad insertada de Azure Maps en su aplicación web o móvil.

## <a name="create-a-new-map-in-a-web-page"></a>Creación de un nuevo mapa en una página web

Puede insertar un mapa en una página web mediante la biblioteca de Javascript del lado cliente de Control de mapa.

1. Cree un archivo HTML.

2. Cárguelo en el SDK web de Azure Maps. Esto se puede hacer mediante una de estas dos opciones.

     a. Use la versión de CDN hospedada globalmente del SDK web de Azure Maps mediante la adición de los puntos de conexión de direcciones URL a las referencias de script y de hojas de estilo en el elemento `<head>` del archivo:

    ```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
    ```

    b. Opcionalmente, puede cargar localmente el código fuente del SDK web de Azure Maps mediante el paquete NPM [azure-maps-control](https://www.npmjs.com/package/azure-maps-control) y hospedarlo con la aplicación. Este paquete también incluye las definiciones de TypeScript.

    > npm install azure-maps-control

    Agregue las referencias de hoja de estilos y origen de script de Azure Maps al elemento `<head>` del archivo:

    ```HTML
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/css/atlas.min.css" type="text/css">
    <script src="node_modules/azure-maps-control/dist/js/atlas.min.js"></script>
    ```

3. Para representar el mapa para que rellene el cuerpo completo de la página, agregue el siguiente elemento `<style>` al elemento `<head>`.

    ```HTML
    <style>
        html, body {
            margin: 0;
        }

        #myMap {
            height: 100vh;
            width: 100vw;
        }
    </style>
    ```

4. En el cuerpo de la página, agregue un elemento `<div>` y asígnele un `id` de **myMap**.

    ```HTML
    <body>
        <div id="myMap"></div>
    </body>
    ```

5. Para inicializar el control de mapa, defina una nueva sección en el cuerpo HTML y cree un script. Pase el `id` del mapa `<div>` o un `HTMLElement` (por ejemplo, `document.getElementById('myMap')`) como primer parámetro al crear una instancia de la `Map` clase. Use su propia clave de cuenta de Azure Maps o las credenciales de Azure Active Directory (AAD) para autenticar el mapa mediante las [opciones de autenticación](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions). Si necesita crear una cuenta o buscar la clave, consulte [Cómo administrar su cuenta y claves de Azure Maps](how-to-manage-account-keys.md). La opción de **idioma** especifica el idioma que se usará para las etiquetas de mapa y los controles. Para información sobre los idiomas admitidos, consulte [Idiomas admitidos en Azure Maps](supported-languages.md). Si usa una clave de suscripción para la autenticación.

    ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });
    </script>
    ```

    Si usa Azure Active Directory (AAD) para la autenticación:

    ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id>',
                aadTenant: 'msft.ccsctp.net'
            }
        });
    </script>
    ```

    Para obtener más información, consulte [autenticación con Azure Maps](azure-maps-authentication.md) para obtener más detalles.

6. Opcionalmente, puede que agregar los siguientes elementos de la etiqueta meta al encabezado de la página le resulte útil:

    ```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    ```

7. Combinarlo todo el archivo HTML debe tener un aspecto similar al código siguiente:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <style>
            html, body {
                margin: 0;
            }

            #myMap {
                height: 100vh;
                width: 100vw;
            }
        </style>
    </head>
    <body>
        <div id="myMap"></div>

        <script type="text/javascript">
            //Create an instance of the map control and set some options.
            var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });
        </script>
    </body>
    </html>
    ```

8. Abra el archivo en el explorador web y vea el mapa representado. Debe ser similar al código siguiente:

    <iframe height="700" style="width: 100%;" scrolling="no" title="Uso del control de mapa" src="//codepen.io/azuremaps/embed/yZpEYL/?height=557&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">Consulte el Pen <a href='https://codepen.io/azuremaps/pen/yZpEYL/'>cómo usar el control de mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
    </iframe>

## <a name="next-steps"></a>Pasos siguientes

Aprenda a crear e interactuar con un mapa:

> [!div class="nextstepaction"]
> [Creación de un mapa](map-create.md)

Obtenga información sobre cómo aplicar estilos en un mapa:

> [!div class="nextstepaction"]
> [Elección de un estilo de mapa](choose-map-style.md)
