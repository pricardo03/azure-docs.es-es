---
title: Inserción de widgets de Azure Video Indexer en las aplicaciones | Microsoft Docs
description: ''
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 4ce1eedac69b06f491510be93fcfbdfbc879ff7e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398139"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Inserción de widgets de Video Indexer en las aplicaciones

Video Indexer admite la inserción de dos tipos de widgets en la aplicación: **Cognitive Insights** y **Player**. 

* El widget **Cognitive Insights** incluye toda la información detallada visual que se extrajo del proceso de indexación de vídeo. 
    El widget de información detallada admite los siguientes parámetros de URL opcionales:

    |NOMBRE|Definición|DESCRIPCIÓN|
    |---|---|---|
    |widgets|Cadenas separadas por coma|Le permite controlar la información detallada que desea representar. <br/>Ejemplo: **widgets=people,brands** representará solo la información detallada de la interfaz de usuario de personas y marcas<br/>Opciones disponibles: personas, palabras clave, anotaciones, marcas, opiniones, transcripción, búsqueda | 
* El widget **Player** le permite transmitir el vídeo con velocidad de bits adaptable.

    El widget Player admite los siguientes parámetros de URL opcionales:

    |NOMBRE|Definición|DESCRIPCIÓN|
    |---|---|---|
    |t|Segundos desde el inicio|Hace que el reproductor comience a reproducir desde un momento de tiempo determinado.<br/>Ejemplo: t=60|
    |captions|Código de idioma|Recupera el título en el idioma dado durante la carga del widget para que esté disponible en el menú de títulos.<br/>Ejemplo: captions=en-Us|
    |showCaptions|Un valor booleano|Hace que el reproductor se cargue con los títulos ya habilitados.<br/>Ejemplo: showCaptions=true|
    |Tipo||Activa una máscara del reproductor de audio (se quita la parte de vídeo).<br/>Ejemplo: type=audio|
    |autoplay|Un valor booleano|Decida si el reproductor debe comenzar a reproducir el vídeo cuando esté cargado (de manera predeterminada es true).<br/>Ejemplo: autoplay=false|
    |language|Código de idioma|Controlar el reproductor controla la localización (el valor predeterminado es en-US).<br/>Ejemplo: language=de-DE|

## <a name="embedding-public-content"></a>Inserción de contenido público

1. Inicie sesión en la cuenta de [Video Indexer](https://api-portal.videoindexer.ai/). 
2. Haga clic en el botón "insertar" que aparece debajo del vídeo.

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Después de hacer clic en el botón, aparecerá un estado modal de inserción en la pantalla donde podrá elegir qué widget quiere insertar en la aplicación.
    Si se selecciona un widget (**Player** o **Cognitive Insights**), se genera el código insertado para pegarlo en la aplicación.
 
3. Elija el tipo de widget que desee (**Insights Cognitive** o **Player**).
4. Copie el código para insertar y agregar agréguelo a la aplicación. 

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Inserción de contenido privado

Puede obtener códigos para insertar de elementos emergentes insertados (como se muestra en la sección anterior) solo para vídeos **públicos**. 

Si desea insertar un vídeo **privado**, tiene que pasar un token de acceso en el atributo **src** de **iframe**:

     https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<VideoId>/?accessToken=<accessToken>
    
Utilice [**Get Insights Widget**](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) API para obtener el contenido del widget Cognitive Insights, o use [**Get Video Access Token**](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) y agréguelo como un parámetro de consulta a la dirección URL, tal como se indicó anteriormente. Especifique esta dirección URL como el valor **src** de **iframe**.

Si desea proporcionar funcionalidades de edición de información detallada (como las que tenemos en nuestra aplicación web) en el widget insertado, tendrá que pasar un token de acceso con permisos de edición. Use [**Get Insights Widget**](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) o [**Get Video Access Token**](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) con **&allowEdit=true**. 

## <a name="widgets-interaction"></a>Interacción de widgets

El widget **Cognitive Insights** puede interactuar con un vídeo en la aplicación. En esta sección se muestra cómo conseguir esta interacción.

![Widget](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Comunicaciones entre orígenes

Para que los widgets de Video Indexer se comuniquen con otros componentes, el servicio Video Indexer hace lo siguiente:

- Usa el método HTML5 de la comunicación entre orígenes **postMessage** y 
- Valida el mensaje mediante el origen VideoIndexer.ai. 

Si decide implementar su propio código del reproductor y realizar la integración con los widgets **Cognitive Insights**, es su responsabilidad validar el origen del mensaje que proviene de VideoIndexer.ai.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Inserción de los dos tipos de widgets en la aplicación / blog (recomendado) 

En esta sección se muestra cómo lograr la interacción entre dos widgets de Video Indexer para que cuando un usuario haga clic en el control de la información detallada de la aplicación, el reproductor salte al momento pertinente.

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script> 

1. Copie el código para insertar del widget **Player**.
2. Copie el código para insertar de **Cognitive Insights**.
3. Agregue el [**archivo mediador**](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) para controlar la comunicación entre los dos widgets:

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>

Ahora cuando un usuario hace clic en el control de información en la aplicación, el reproductor salta al momento relevante.

Para más información, consulte [esta demostración](https://api-portal.videoindexer.ai/demo-all-widgets).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Inserte el widget Cognitive Insights y utilice Azure Media Player para reproducir el contenido.

En esta sección se muestra cómo lograr la interacción entre un widget **Cognitive Insights** y una instancia de Azure Media Player mediante el [complemento AMP](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).
 
1. Agregue un complemento de Video Indexer al reproductor de AMP.

        <script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>


2. Cree una instancia de Azure Media Player con el complemento de Video Indexer.

        // Init Source
        function initSource() {
            var tracks = [{
            kind: 'captions',
            // Here is how to load vtt from VI, you can replace it with your vtt url.
            src: this.getSubtitlesUrl("c4c1ad4c9a", "English"),
            srclang: 'en',
            label: 'English'
            }];

            myPlayer.src([
            {
                "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
                "type": "application/vnd.ms-sstr+xml"
            }
            ], tracks);
        }

        // Init your AMP instance
        var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: true,
            controls: true,
            width: "640",
            height: "400",
            poster: "",
            plugins: {
            videobreakedown: {}
            }
        }, function () {
            // Activate the plugin
            this.videobreakdown({
            videoId: "c4c1ad4c9a",
            syncTranscript: true,
            syncLanguage: true
            });

            // Set the source dynamically
            initSource.call(this);
        });

3. Copie el código para insertar de **Cognitive Insights**.

Ahora debería poder comunicarse con Azure Media Player.

Para más información, consulte [esta demostración](https://api-portal.videoindexer.ai/demo-your-amp).

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>Inserción del widget Cognitive Insights de Video Indexer y uso de su propio reproductor (puede ser cualquier reproductor)

Si utiliza su propio reproductor, tiene que encargarse de manipularlo para lograr la comunicación. 

1. Inserte el reproductor de vídeo.

    Por ejemplo, un reproductor HTML5 estándar

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Inserte el widget Cognitive Insights.
3. Implemente la comunicación para el reproductor mediante la escucha del evento "mensaje". Por ejemplo: 

        <script>
    
            (function(){
            // Reference your player instance
            var playerInstance = document.getElementById('vid1');
        
            function jumpTo(evt) {
              var origin = evt.origin || evt.originalEvent.origin;
        
              // Validate that event comes from the videobreakdown domain.
              if ((origin === "https://www.videobreakdown.com") && evt.data.time !== undefined){
                
                // Here you need to call your player "jumpTo" implementation
                playerInstance.currentTime = evt.data.time;
               
                // Confirm arrival to us
                if ('postMessage' in window) {
                  evt.source.postMessage({confirm: true, time: evt.data.time}, origin);
                }
              }
            }
        
            // Listen to message event
            window.addEventListener("message", jumpTo, false);
          
            }())    
        
        </script>


Para más información, consulte [esta demostración](https://api-portal.videoindexer.ai/demo-your-player).

## <a name="adding-subtitles"></a>Adición de subtítulos

Si inserta información detallada de Video Indexer con su propio reproductor AMP, puede utilizar el método **GetVttUrl** para obtener subtítulos. También puede llamar a un método de javascript desde el complemento AMP de Video Indexer **getSubtitlesUrl** (como se ha mostrado anteriormente). 

## <a name="customizing-embeddable-widgets"></a>Personalización de widgets que se pueden insertar

### <a name="cognitive-insights-widget"></a>Widget Cognitive Insights
Puede elegir los tipos de información detallada que desee especificándolos como valor en el siguiente parámetro URL agregado al código insertado que obtiene (de la API o de la aplicación web):

**&widgets=** \<lista de widgets deseados>

Los valores posibles son: personas, palabras clave, opiniones, transcripción, búsqueda.

Por ejemplo, si desea insertar un widget que contiene solo personas y buscar información detallada, dirección URL para insertar de iframe tendrá este aspecto: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?widgets=people,search

También se puede personalizar el título de la ventana de iframe proporcionando **& title=**<YourTitle> a la dirección url de iframe. (Personalizará el html \<title > valor).
Por ejemplo, si desea asignar a la ventana de iframe el título "MyInsights", la dirección URL tendrá este aspecto: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?title=MyInsights. Tenga en cuenta que esta opción solo es relevante en aquellos casos en los que necesite abrir la información detallada en una nueva ventana.

### <a name="player-widget"></a>Widget Player
Si inserta el reproductor de Video Indexer, puede elegir el tamaño del reproductor al especificar el tamaño de iframe.

Por ejemplo:

    <iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/{id}” frameborder="0" allowfullscreen />

De forma predeterminada, el reproductor de Video Indexer tendrá subtítulos generados automáticamente basados en la transcripción del vídeo que se extrajo del vídeo con el idioma de origen que se seleccionó cuando se cargó el vídeo.

Si desea insertar con un idioma diferente, puede agregar **&captions=< Language | ”all” | “false” >** a la URL del reproductor para insertar o poner "all" como valor si desea tener todos los subtítulos de idiomas disponibles.
Si desea que los subtítulos se muestran de forma predeterminada, se puede pasar **&showCaptions=true**

La dirección URL para insertar tendrá un aspecto similar al siguiente: https://www.videoindexer.ai/embed/player/9a296c6ec3/?captions=italian. Si desea deshabilitar los subtítulos, puede pasar "false" como valor en el parámetro captions.

Reproducción automática: de forma predeterminada, el reproductor iniciará la reproducción del vídeo. Puede elegir no pasar &autoplay=false para la dirección URL para insertar anterior.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo ver y editar la información detallada de Video Indexer, consulte [este](video-indexer-view-edit.md) artículo.

## <a name="see-also"></a>Otras referencias

[Introducción a Video Indexer](video-indexer-overview.md)
