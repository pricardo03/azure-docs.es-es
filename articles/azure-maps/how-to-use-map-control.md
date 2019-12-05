---
title: Introducción al control de mapa de web en Azure Maps | Microsoft Docs
description: Aprenda a usar la biblioteca Javascript del cliente del control de mapa de Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: ff183261f67ff76f56fc034d8102e3aa3a4838a8
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74480533"
---
# <a name="use-the-azure-maps-map-control"></a>Uso del control de mapa de Azure Maps

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
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/atlas.min.css" type="text/css"> 
    <script src="node_modules/azure-maps-control/dist/atlas.min.js"></script>
    ```

    >[!Note]
    > Las definiciones de Typescript se pueden importar en la aplicación si agrega:
    > ```Javascript
    > import * as atlas from 'azure-maps-control';
    > ```

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

5. Para inicializar el control de mapa, defina una nueva sección en el cuerpo HTML y cree un script. Pase el `id` del mapa `<div>` o un `HTMLElement` (por ejemplo, `document.getElementById('myMap')`) como primer parámetro al crear una instancia de la clase `Map`. Use su propia clave de cuenta de Azure Maps o las credenciales de Azure Active Directory (AAD) para autenticar el mapa mediante las [opciones de autenticación](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions). Si necesita crear una cuenta o buscar la clave, consulte [Cómo administrar su cuenta y claves de Azure Maps](how-to-manage-account-keys.md). La opción de **idioma** especifica el idioma que se usará para las etiquetas de mapa y los controles. Para información sobre los idiomas admitidos, consulte [Idiomas admitidos en Azure Maps](supported-languages.md). Si usa una clave de suscripción para la autenticación.

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
                aadTenant: '<Your AAD Tenant Id>'
            }
        });
    </script>
    ```

    [Aquí](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples) puede encontrar una lista de ejemplos que muestran cómo integrar Azure Active Directory (AAD) con Azure Maps. 
    
    Para obtener más información, consulte el documento [Autenticación con Azure Maps](azure-maps-authentication.md) y también las [muestras de autenticación de Azure AD de Azure Maps](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples).

6. Opcionalmente, puede que agregar los siguientes elementos de la etiqueta meta al encabezado de la página le resulte útil:

    ```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    ```

7. Si combina todo, el archivo HTML debería ser parecido al código siguiente:

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

8. Abra el archivo en el explorador web y vea el mapa representado. Debería ser similar al código siguiente:

    <iframe height="700" style="width: 100%;" scrolling="no" title="Uso del control de mapa" src="//codepen.io/azuremaps/embed/yZpEYL/?height=557&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">Consulte el Pen <a href='https://codepen.io/azuremaps/pen/yZpEYL/'>How to use the map control</a> (Uso del control de mapa) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
    </iframe>

## <a name="localizing-the-map"></a>Localización del mapa

Azure Maps proporciona dos formas diferentes de establecer el idioma y la vista regional del mapa. La primera opción es agregar esta información al espacio de nombres `atlas` global, lo que provocará que estos valores son los predeterminados en todas las instancias de control de mapa de la aplicación. El código siguiente establece el idioma en francés ("fr-FR") y la vista regional en "auto":

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('Auto');
```

La segunda opción consiste en usar esta información a las opciones del mapa al cargar el mapa como:

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'Auto',

    authOptions: {
        authType: 'aad',
        clientId: '<Your AAD Client Id>',
        aadAppId: '<Your AAD App Id>',
        aadTenant: '<Your AAD Tenant Id>'
    }
});
```

> [!Note]
> Con el SDK web es posible cargar varias instancias del mapa con diferentes configuraciones de idioma y región. Además, esta configuración se puede actualizar después de que se haya cargado el mapa mediante la función `setStyle` del mapa. 

Este es un ejemplo de Azure Maps con el idioma establecido en "fr-FR" y la vista regional establecida en "auto".

![Imagen del mapa que muestra las etiquetas en francés](./media/how-to-use-map-control/websdk-localization.png)

[Aquí](supported-languages.md) encontrará una lista completa de los idiomas y vistas regionales admitidos.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a crear e interactuar con un mapa:

> [!div class="nextstepaction"]
> [Creación de un mapa](map-create.md)

Obtenga información sobre cómo aplicar estilos en un mapa:

> [!div class="nextstepaction"]
> [Elección de un estilo de mapa](choose-map-style.md)

Para agregar más datos al mapa:

> [!div class="nextstepaction"]
> [Creación de un mapa](map-create.md)

> [!div class="nextstepaction"]
> [Ejemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-maps)

Para ver una lista de ejemplos que muestran cómo integrar Azure Active Directory (AAD) con Azure Maps, consulte:

> [!div class="nextstepaction"]
> [Muestras de autenticación de Azure AD](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)