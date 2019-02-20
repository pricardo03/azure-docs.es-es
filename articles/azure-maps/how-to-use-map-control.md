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
ms.openlocfilehash: 877393de20788b0aa1c76084b121a82f12715cd3
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118078"
---
# <a name="use-the-azure-maps-map-control"></a>Uso del Control de mapa de Azure Maps

La biblioteca de Javascript del lado cliente del Control de mapa le permite representar mapas y la funcionalidad insertada de Azure Maps en su aplicación web o móvil.

## <a name="create-a-new-map-in-a-web-page"></a>Creación de un nuevo mapa en una página web

Puede insertar un mapa en una página web mediante la biblioteca de Javascript del lado cliente de Control de mapa.

1. Cree un archivo HTML.

2. Cárguelo en el SDK web de Azure Maps. Esto se puede hacer mediante una de estas dos opciones.
    
     a. Use la versión de CDN hospedada globalmente del SDK web de Azure Maps mediante la adición de los puntos de conexión de direcciones URL a las referencias de script y de hojas de estilo en el elemento `<head>` del archivo:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=2" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=2"></script>
    ```

    b. Opcionalmente, puede cargar localmente el código fuente del SDK web de Azure Maps mediante el paquete NPM [azure-maps-control](https://www.npmjs.com/package/azure-maps-control) y hospedarlo con la aplicación. Este paquete también incluye las definiciones de TypeScript.

    > npm install azure-maps-control

    Agregue las referencias de hoja de estilos y origen de script de Azure Maps al elemento `<head>` del archivo:

    ```html
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/css/atlas.min.css" type="text/css" />
    <script src="node_modules/azure-maps-control/dist/js/atlas.min.js"></script>
    ```

3. Para representar el mapa para que rellene el cuerpo completo de la página, agregue el siguiente elemento `<style>` al elemento `<head>`.

    ```html
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

    ```html
    <body>
        <div id="myMap"></div>
    </body>
    ```

5. Para inicializar el control de mapa, defina una nueva sección en el cuerpo HTML y cree un script. Use su propia clave de cuenta de Azure Maps o las credenciales de Azure Active Directory (AAD) para autenticar el mapa mediante las [opciones de autenticación](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.authenticationoptions). Si necesita crear una cuenta o buscar la clave, consulte [Cómo administrar su cuenta y claves de Azure Maps](how-to-manage-account-keys.md). La opción de **idioma** especifica el idioma que se usará para las etiquetas de mapa y los controles. Para información sobre los idiomas admitidos, consulte [Idiomas admitidos en Azure Maps](supported-languages.md). Si usa una clave de suscripción para la autenticación.

    ```html
    <script type='text/javascript'>
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

    ```html
    <script type='text/javascript'>
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id',
                aadTenant: 'msft.ccsctp.net'
            }
        });
    </script>
    ```

6. Opcionalmente, puede que agregar los siguientes elementos de la etiqueta meta al encabezado de la página le resulte útil:

    ```html
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    
    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
    ```

7. Si combina todo con el archivo HTML, el aspecto debería ser parecido a este:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>
    
        <meta charset="utf-8" />
        
        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    
        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
    
        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=2" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=2"></script>
    
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
        
        <script type='text/javascript'>
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

8. Abra el archivo en el explorador web y vea el mapa representado. Debería tener este aspecto:

    <iframe height="700" style="width: 100%;" scrolling="no" title="Uso del control de mapa" src="//codepen.io/azuremaps/embed/yZpEYL/?height=557&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte <a href='https://codepen.io/azuremaps/pen/yZpEYL/'>Uso del control de mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
    </iframe>

## <a name="next-steps"></a>Pasos siguientes

Aprenda a crear e interactuar con un mapa:

> [!div class="nextstepaction"]
> [Creación de un mapa](map-create.md)

Obtenga información sobre cómo aplicar estilos en un mapa:

> [!div class="nextstepaction"]
> [Elección de un estilo de mapa](choose-map-style.md)
