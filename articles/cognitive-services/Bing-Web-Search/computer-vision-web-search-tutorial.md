---
title: Tutorial de la aplicación móvil Visual Search
description: Abra la aplicación de origen en C# que implementa la búsqueda visual mediante Computer Vision API, Bing Web Search API de Cognitive Services y el marco multiplataforma Xamarin.Forms.
services: bing-web-search, computer-vision
author: Aristoddle
manager: bking
ms.service: cognitive-services
ms.devlang: csharp
ms.topic: article
ms.date: 06/22/2017
ms.author: t-jolan
ms.openlocfilehash: 54dabaeaad2e49ba7cc138636054bc3dfa4b8379
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380767"
---
# <a name="visual-search-mobile-app-tutorial"></a>Tutorial de la aplicación móvil Visual Search

## <a name="introduction"></a>Introducción  
En este tutorial se exploran los puntos de conexión [Computer Vision API](https://azure.microsoft.com/services/cognitive-services/computer-vision/) y [Bing Web Search API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) y se muestra cómo se pueden usar para compilar una aplicación de búsqueda visual básica con [Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/).  En general, en este tutorial se abordan los temas siguientes: 

> [!div class="checklist"]
> * Configuración de Visual Studio para desarrollar aplicaciones de Xamarin.Forms
> * Uso de [Xamarin Media Plugin](https://github.com/jamesmontemagno/MediaPlugin) para capturar e importar datos de imagen
> * Análisis del texto de una imagen mediante Computer Vision API
> * Envío de solicitudes de búsqueda a Bing Web Search API
> * Análisis de las respuestas JSON de ambas API con [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) (con LINQ y deserialización del modelo de objetos)
> * Creación de una interfaz de usuario de Xamarin.Forms multiplataforma para búsqueda visual 

## <a name="prerequisites"></a>requisitos previos

Este ejemplo se desarrolló con Xamarin.Forms con [Visual Studio 2017](https://www.visualstudio.com/downloads/). Puede utilizarse Visual Studio de 2017 Community Edition. Para obtener más información sobre el uso de Xamarin con Visual Studio, consulte la [documentación de Xamarin](https://developer.xamarin.com/guides/cross-platform/getting_started/).

Este ejemplo usar los paquetes NuGet siguientes:

> [!div class="checklist"]
> * [Xamarin Media Plugin](https://github.com/jamesmontemagno/MediaPlugin)
> * [Json.NET](https://github.com/JamesNK/Newtonsoft.Json)

La aplicación utiliza las siguientes Cognitive Services APIs:

> [!div class="checklist"]
> * [Bing Web Search API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) 
> * [Computer Vision API](https://azure.microsoft.com/services/cognitive-services/computer-vision/)

Para obtener las claves para la evaluación durante 30 días para estas API, consulte [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Para obtener más información sobre cómo obtener claves para uso comercial, consulte la [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/).

## <a name="setup-for-development"></a>Instalación para desarrollo  

### <a name="installing-xamarin-on-windows"></a>Instalación de Xamarin en Windows
Con cualquier edición de Visual Studio 2017 instalada, abra el Instalador de Visual Studio, seleccione el menú de hamburguesa asociado con la instalación de Visual Studio y elija **Modificar**.  

![Instalador de Visual Studio](./media/computer-vision-web-search-tutorial/visual-studio-installer.png) 

Desplácese hacia abajo hasta Dispositivos móviles y juegos y habilite **Desarrollo para dispositivos móviles con .NET**, si aún no está habilitado.

![Instalador de Visual Studio con Xamarin.Forms seleccionado](./media/computer-vision-web-search-tutorial/xamarin-forms-is-enabled.png)

Por último, haga clic en **Modificar** en la esquina inferior derecha de la ventana y espere a que Xamarin se instale.

### <a name="installing-xamarin-on-macos"></a>Instalación de Xamarin en macOS
Xamarin viene preempaquetado con Visual Studio para Mac. No se debería ser necesaria ninguna instalación.

## <a name="building-and-running-the-app"></a>Compilación y ejecución de la aplicación

Un archivo de solución de Visual Studio *(.sln)* para la aplicación Visual Search se puede descargar de [Visual Search App with Cognitive Services](https://azure.microsoft.com/resources/samples/cognitive-services-xamarin-forms-computer-vision-search/) (Aplicación Visual Search con Cognitive Services). Puede descargar el archivo ZIP con un explorador Web, clonarlo a la estación de trabajo desde GitHub o descargarlo mediante Visual Studio.

Para empezar a trabajar con el ejemplo, abra `VisualSearchApp.sln` en Visual Studio.  Inicializar los componentes necesarios puede tardar unos instantes.

La aplicación requiere dos bibliotecas de terceros: **Json.NET** y **Xamarin Media Plugin**. Puede instalar estas bibliotecas directamente en Visual Studio con el Administrador de paquetes NuGet. Elija **Herramientas > Administrador de paquetes NuGet > Administrar paquetes NuGet para la solución**, o haga clic con el botón derecho en la solución en el Explorador de soluciones y elija **Administrar paquetes NuGet** en el menú contextual.

En la ventana NuGet, busque e instale **Xamarin Media plugin** (Xam.Plugin.Media) versión 2.6.2 y **Json.NET** (Newtonsoft.Json) 10.0.3. Asegúrese de seleccionar todos los proyectos durante la instalación.

Para compilar la aplicación para todas las plataformas disponibles, presione **Ctrl+Mayús+B**, o haga clic en **Compilar** en el menú de la cinta de opciones y elija **Compilar solución**.  Para obtener ayuda para compilar y probar el código para iOS en un sistema Windows, consulte [esta guía](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/).

Antes de ejecutar la aplicación, debe seleccionar una Configuración, Plataforma y Proyecto de destino.  Las aplicaciones de Xamarin.Forms se compilan en código nativo para Windows, Android e iOS. En este tutorial se incluyen capturas de pantalla de la versión de Windows de la muestra, pero todas las versiones son funcionalmente equivalentes. La configuración de implementación que se usa en esta guía se muestra aquí.  

![Visual Studio configurado para compilar para un teléfono Android](./media/computer-vision-web-search-tutorial/configuration-selection.png)

Una vez que el proceso de compilación se haya realizado correctamente y haya seleccionado una plataforma de destino, haga clic en el botón **Iniciar** en la barra de herramientas o presione **F5**. Visual Studio implementa la solución en la plataforma de destino y la inicia.

Aparece la página Agregar claves. (Esta página se define en `AddKeysPage.xaml`).  

![Imagen de la página Agregar claves](./media/computer-vision-web-search-tutorial/add-keys-page.png)  

A continuación, pegue las claves de Computer Vision API y Bing Web Search API. Computer Vision API también requiere que seleccione el servidor donde se hospeda el punto de conexión.

> [!TIP]
> Para omitir esta página, especifique esta información en las ubicaciones correspondientes en `App.xaml.cs`. 

La aplicación valida las claves mediante una consulta de prueba y, a continuación, lo lleva a la página OCR Select (Selección de OCR) (definida en `OcrSelectPage.xaml`).
   
![Imagen de la página OCR Select](./media/computer-vision-web-search-tutorial/ocr-select-page.png)  

En la parte superior de esta pantalla, puede elegir si el texto que desea reconocer está imprimo o escrito a mano.

> [!TIP]
> Mantenga el elemento del que intenta reconocer texto lo más nivelado posible y asegúrese de que tenga una iluminación uniforme sin ningún reflejo. Hemos observado que el OCR escrito a mano a veces funciona mejor para las fuentes Script u otro texto "decorativo".

A continuación, haga clic en **Take Photo** (Tomar foto) o **Import Photo** (Importar foto) para tomar una foto con la cámara del dispositivo o elegir una foto almacenada en el dispositivo.

Después de tomar o elegir una foto, la imagen se pasa a Computer Vision API. La página Words Found (Palabras encontradas) (definida en `OcrResultsPage.xaml`) muestra las palabras que se reconocen en la imagen.

![Imagen de la página OCR Results](./media/computer-vision-web-search-tutorial/ocr-results-page.png)  

> [!NOTE]
> La imagen que se utiliza para estos resultados se encuentra en el repositorio de origen como `SamplePhotos\TestImage.jpg`.

Al hacer clic en un elemento en la página Words Found, aparece la página Web Results (Resultados web) (`WebResultsPage.xaml`) y muestra los principales resultados de Bing para esa búsqueda.

![Imagen de la página de resultados web](./media/computer-vision-web-search-tutorial/web-results-page.png)  
Por último, elija un elemento en la página de resultados web para abrir el vínculo en una vista web incrustada. (O vuelva para elegir otro resultado).

![Imagen de la página de vista web](./media/computer-vision-web-search-tutorial/web-view-page.png)  

Puede interactuar con la página tal y como haría en cualquier explorador web, o volver a la página de resultados web. 

## <a name="review-and-learn"></a>Revisión y comprensión

Ahora que tiene una idea de cómo funciona la aplicación Visual Search, veamos exactamente cómo se usan las dos Microsoft Cognitive Services APIs.  Independientemente de si usa este ejemplo como punto de partida para su propia aplicación o simplemente como una sección de procedimientos para las API, es útil desplazarse por la aplicación pantalla por pantalla para examinar exactamente cómo funciona.

### <a name="add-keys-page"></a>Página Add Keys
La interfaz de usuario de la página Add Keys (Agregar claves) se define en `AddKeysPage.xaml`, y su lógica principal se define en `AddKeysPage.xaml.cs`. Puesto que las claves se validan mediante una solicitud de prueba, es el momento oportuno para establecer cómo usar los puntos de conexión de Cognitive Services en C#.  

La estructura básica de esta interacción es: 

1. Crear instancias de los objetos *HttpResponseMessage* y *HttpClient* desde *System.Net.Http*
2. Adjuntar los encabezados deseados (definidos en la referencia de API de cada punto de conexión) al objeto *HttpClient*
3. Enviar una solicitud GET o POST con los datos, agregando los parámetros necesarios para el URI del punto de conexión
4. Comprobar que la respuesta se realizó correctamente
5. Pasar la respuesta para su posterior procesamiento

La función que comprueba la validez de la clave de Bing Search APIA es `CheckBingSearchKey()`, como se muestra aquí.

```csharp
async Task CheckBingSearchKey(object sender = null, EventArgs e = null)
{
    HttpResponseMessage response;
    HttpClient SearchApiClient = new HttpClient();

    SearchApiClient.DefaultRequestHeaders.Add(AppConstants.OcpApimSubscriptionKey, BingSearchKeyEntry.Text);

    try
    {
        response = await SearchApiClient.GetAsync(AppConstants.BingWebSearchApiUrl + "q=test");

        if (response.StatusCode != System.Net.HttpStatusCode.Unauthorized)
        {
            BingSearchKeyEntry.BackgroundColor = Color.Green;
            AppConstants.BingWebSearchApiKey = BingSearchKeyEntry.Text;
            bingSearchKeyWorks = true;
        }
        else
        {
            BingSearchKeyEntry.BackgroundColor = Color.Red;
            bingSearchKeyWorks = false;
        }
    }
    catch( Exception exception )
    {
        BingSearchKeyEntry.BackgroundColor = Color.Red;
        Console.WriteLine($"ERROR: {exception.Message}");
    }
}
```

Una función similar, `CheckComputerVisionKey()`, se utiliza para validar la clave de Computer Vision.

### <a name="ocr-select-page"></a>Página OCR Select

La página OCR Select (`OcrSelectPage.xaml`) tiene dos funciones importantes. En primer lugar, determina qué tipo de OCR se realiza en la foto de destino. En segundo lugar, permite al usuario capturar o abrir la imagen que va a procesar.

Tradicionalmente, la segunda tarea es complicada en una aplicación multiplataforma, porque cada plataforma requiere un código diferente. Xamarin Media Plugin controla esto con unas pocas líneas de código.

La siguiente función incorpora un ejemplo de uso de Xamarin Media Plugin para la captura de fotos.  Allí se puede:

1. Asegurar de que una cámara está disponible en el dispositivo actual
2. Crear una instancia de un objeto `StoreCameraMediaOptions` para especificar dónde desea guardar la imagen capturada
3. Capturar una imagen y obtener un objeto `MediaFile` que contiene los datos de imagen
4. Desempaquetar `MediaFile` en una matriz de bytes
5. Devolver la matriz de bytes para su posterior procesamiento

Aquí está `TakePhoto()`, la función que usa Xamarin Media Plugin para la captura de fotos.  

```csharp
async Task<byte[]> TakePhoto()
{
    MediaFile photoMediaFile = null;
    byte[] photoByteArray = null;

    if (CrossMedia.Current.IsCameraAvailable)
    {
        var mediaOptions = new StoreCameraMediaOptions
        {
            PhotoSize = PhotoSize.Medium,
            AllowCropping = true,
            SaveToAlbum = true,
            Name = $"{DateTime.UtcNow}.jpg"
        };
        photoMediaFile = await CrossMedia.Current.TakePhotoAsync(mediaOptions);
        photoByteArray = MediaFileToByteArray(photoMediaFile);
    }
    else
    {
        await DisplayAlert("Error", "No camera found", "OK");
        Console.WriteLine($"ERROR: No camera found");
    }
    return photoByteArray;
}
```
La utilidad de importación de fotos funciona de manera similar. Estas dos funcionalidades pueden encontrarse en `OcrSelectPage.xaml.cs`. 
 
> [!NOTE]
> El punto de conexión de Handwritten OCR (OCR de escritura a mano) solo puede controlar fotos con menos de 4 MB. Para evitar problemas con el tamaño de archivo, se ha reducido la foto al 50 % de su tamaño original estableciendo la opción `PhotoSize = PhotoSize.Medium` en el objeto `StoreCameraMediaOptions`.  Si el dispositivo no toma fotos con una calidad excepcionalmente alta y obtiene errores, puede intentar `PhotoSize = PhotoSize.Small` en su lugar.

Esta es la función de utilidad que se usa para convertir un *MediaFile* en una matriz de bytes.

```csharp
byte[] MediaFileToByteArray(MediaFile photoMediaFile)
{
    using (var memStream = new MemoryStream())
    {
        photoMediaFile.GetStream().CopyTo(memStream);
        return memStream.ToArray();
    }
}
```

### <a name="ocr-results-page"></a>Página de OCR Results

La página OCR Results (Resultados de OCR) muestra texto que se extrae de la imagen mediante el [método SelectToken](http://www.newtonsoft.com/json/help/html/SelectToken.htm) de **Json.NET**.  Los dos puntos de conexión de OCR funcionan de forma ligeramente diferente, por lo que resulta útil analizar ambos.  

Puesto que Computer Vision API solo se hospeda en algunas ubicaciones de servidor, su punto de conexión se debe construir dinámicamente en tiempo de ejecución. La función `SetOcrLocation` (parte de la clase estática *AppConstants* que se encuentra en `AppConstants.cs`) establece la ubicación del punto de conexión del URI. Corresponde a la selección del usuario en la página Add Keys (Agregar claves) o usa el valor establecido en `App.xaml.cs`. 

```csharp
public static void SetOcrLocation(string location)
{
    ComputerVisionApiOcrUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/ocr?language=en&detectOrientation=true";
    ComputerVisionApiHandwritingUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true";
}
```

Veamos estas solicitudes de API más detalladamente. La API de OCR Computer Vision es capaz de analizar el texto desde un lenguaje indeterminado, pero le indicamos que busque texto en inglés para mejorar los resultados. También se permite que la API detecte la orientación del texto. Usar una orientación fija podría mejorar los resultados de análisis, pero la detección de orientación puede resultar útil en una aplicación móvil.

Puede aprender más acerca de los parámetros disponibles con este punto de conexión desde la [referencia de API de reconocimiento óptico de caracteres impresos](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc).  

La API Handwritten OCR está todavía en versión preliminar y actualmente solo funciona con texto en inglés.  Por esta razón, su único parámetro actualmente es una marca que indica si se debe o no analizar texto escrito a mano. La API Handwritten OCR puede analizar texto impreso o escrito a mano, pero `handwriting=false` produce mejores resultados en texto impreso. 

Puesto que esta aplicación está en inglés, se podría haber usado Handwritten OCR para este ejemplo y establecido la marca `handwriting` según lo que el usuario nos indiqué qué reconocer.  Ambos puntos de conexión se utilizaron con fines ilustrativos.  

Puede aprender más acerca de los parámetros disponibles con este punto de conexión desde la [referencia de API de reconocimiento óptico de caracteres escritos a mano](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).

Ahora, echemos un vistazo a las funciones que llaman a estas API.

`FetchPrintedWordList()` utiliza el punto de conexión Print OCR (OCR impreso) para analizar el texto impreso a partir de imágenes. La solicitud HTTP sigue una estructura similar a la llamada que se usa en la página Add Keys para validar la clave, pero es necesario enviar una foto. Una foto es demasiado grande para pasar una cadena de consulta, por lo que se debe usar una solicitud POST HTTP en lugar de una solicitud GET. Es necesario codificar la fotográfica (que se encuentra en la memoria como una matriz de bytes) en un objeto `ByteArrayContent` y agregar un encabezado a este objeto que define el tipo de datos que se envía. 

Puede leer sobre los tipos de contenido aceptables en la [referencia de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).  

```csharp
async Task<ObservableCollection<string>> FetchPrintedWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiOcrUrl, content);
        }

        string ResponseString = await response.Content.ReadAsStringAsync();
        JObject json = JObject.Parse(ResponseString);

        // Here, we pull down each "line" of text and then join it to 
        // make a string representing the entirety of each line.  
        // In the Handwritten endpoint, you are able to extract the 
        // "line" without any further processing.  If you would like 
        // to simply get a list of all extracted words,* you can do 
        // this with:
        // json.SelectTokens("$.regions[*].lines[*].words[*].text) 
        IEnumerable<JToken> lines = json.SelectTokens("$.regions[*].lines[*]");
        if (lines != null)
        {
            foreach (JToken line in lines)
            {
                IEnumerable<JToken> words = line.SelectTokens("$.words[*].text");
                if (words != null)
                {
                    wordList.Add(string.Join(" ", words.Select(x => x.ToString())));
                }
            }
        }
    }
    return wordList;
}
```
> [!TIP]
> Tenga en cuenta la manera en que se usa el [método SelectToken](http://www.newtonsoft.com/json/help/html/SelectToken.htm) de **Json.NET** para extraer el texto del objeto de respuesta. `SelectToken` es ideal cuando solo se necesita una parte específica de la respuesta JSON, que luego puede pasarse a la función siguiente. En otras partes del código, se deserializan las respuestas JSON en objetos de modelo definidos en `ModelObjects.cs`.

La diferencia principal entre la solicitud Print OCR y Handwritten OCR es que el servicio Print OCR devuelve el texto reconocido como parte de la respuesta HTTP, mientras que el servicio Handwritten OCR requiere una solicitud adicional para obtener la información. La solicitud inicial de Handwritten OCR devuelve un estado HTTP 202, lo cual indica solo que ha empezado el procesamiento. Esta respuesta contiene un punto de conexión que el cliente debe comprobar para obtener la respuesta completa cuando esté disponible. Consulte `FetchHandwrittenWordList()` para ver cómo funciona.

```csharp
async Task<ObservableCollection<string>> FetchHandwrittenWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        // Make the POST request to the handwriting recognition URL
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiHandwritingUrl, content);
        }

        // Fetch results
        IEnumerable<string> operationLocationValues;
        string statusUri = string.Empty;
        if (response.Headers.TryGetValues("Operation-Location", out operationLocationValues))
        {
            statusUri = operationLocationValues.FirstOrDefault();

            // Ping status URL, wait for processing to finish 
            JObject obj = await FetchResultFromStatusUri(statusUri);
            IEnumerable<JToken> strings = obj.SelectTokens("$.recognitionResult.lines[*].text");
            foreach (string s in strings)
            {
                wordList.Add((string)s);
            }
        }
    }
    return wordList;
}
```

`FetchResultFromStatusUri()` es la segunda parte del proceso Handwritten OCR. Hace ping en el URI que se extrae de los metadatos de la respuesta inicial, bien hasta que se obtiene un resultado o hasta que se agota el tiempo de espera de la función.  Es importante llamar a esta función de forma asincrónica en su propio subproceso. En caso contrario, este método bloquearía la interfaz de usuario hasta que se completara el procesamiento.

```csharp
// Takes in the url to check for handwritten text parsing results, and pings it per second until processing is finished
// Returns the JObject holding data for a successful parse
async Task<JObject> FetchResultFromStatusUri(string statusUri)
{
    JObject obj = null;
    int timeoutcounter = 0;
    HttpResponseMessage response = await visionApiClient.GetAsync(statusUri);
    string responseString = await response.Content.ReadAsStringAsync();
    obj = JObject.Parse(responseString);
    while ((!((string)obj.SelectToken("status")).Equals("Succeeded")) && (timeoutcounter++ < 60))
    {
        await Task.Delay(1000);
        response = await visionApiClient.GetAsync(statusUri);
        responseString = await response.Content.ReadAsStringAsync();
        obj = JObject.Parse(responseString);
    } 
    return obj;
}
```

### <a name="web-results-page"></a>Página Web Results
Después de que el usuario selecciona un término de búsqueda que se muestra en la página OCR Results (Resultados de OCR), se pasa a la página Web Results (Resultados web).  Aquí se construye una solicitud de [Bing Web Search API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/), se envía al punto de conexión del servicio y se deserializa la respuesta JSON mediante el método [DeserializeObject](http://www.newtonsoft.com/json/help/html/DeserializeObject.htm) de **Json.NET**.  

```csharp
async Task<WebResultsList> GetQueryResults()
{
    // URL-encode the query term
    var queryString = System.Net.WebUtility.UrlEncode(queryTerm);

    // Here we encode the URL that will be used for the GET request to 
    // find query results.  Its arguments are as follows:
    // 
    // - [count=20] This sets the number of webpage objects returned at 
    //   "$.webpages" in the JSON response.  Currently, the API asks for 
    //   20 webpages in the response
    //
    // - [mkt=en-US] This sets the market where the results come from.
    //   Currently, the API looks for english results based in the 
    //   United States.
    //
    // - [q=queryString] This sets the string queried using the Search 
    //   API.   
    //
    // - [responseFilter=Webpages] This filters the response to only 
    //   include Webpage results.  This tag can take a comma seperated 
    //   list of response types that you are looking for.  If left 
    //   blank, all responses (webPages, News, Videos, etc) are 
    //   returned.
    //
    // - [setLang=en] This sets the languge for user interface strings. 
    //   To learn more about UI strings, check the Web Search API 
    //   reference.
    //
    // - [API Reference] https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference
    string uri = AppConstants.BingWebSearchApiUrl + $"count=20&mkt=en-US&q={queryString}&responseFilter=Webpages&setLang=en";

    // Make the HTTP Request
    WebResultsList webResults = null;
    HttpResponseMessage httpResponseMessage = await searchApiClient.GetAsync(uri);
    var responseContentString = await httpResponseMessage.Content.ReadAsStringAsync();
    JObject json = JObject.Parse(responseContentString);
    JToken resultBlock = json.SelectToken("$.webPages");
    if (resultBlock != null)
    {
        webResults = JsonConvert.DeserializeObject<WebResultsList>(resultBlock.ToString());
    }
    return webResults;
}
```

Bing Web Search API funciona mejor cuando se proporciona toda la información posible acerca de lo que el usuario podría desear. En concreto, casi siempre se deben utilizar los parámetros `mkt` y `setLang` (aquí establecidos en inglés de Estados Unidos) para identificar la ubicación y el idioma preferido del usuario.

> [!NOTE]
> Se mantuvo la sencillez de la consulta de Bing Web Search para asegura que el código fuente fuera fácil de entender.  En una aplicación real, debe agregar los encabezados siguientes a la solicitud HTTP para obtener mejores resultados. 
> * User-Agent  
> * X-MSEdge-ClientID  
> * X-Search-ClientIP  
> * X-Search-Location  
>
> Puede aprender más acerca de los valores de estos encabezado en la [referencia de Bing Web Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers)

## <a name="next-steps"></a>Pasos siguientes
Microsoft Cognitive Services proporciona muchos servicios adicionales que pueden integrarse fácilmente en esta aplicación.  Por ejemplo, podría realizar estas acciones:

* Agregar [Bing Entity Search](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) para aumentar los resultados de las búsquedas web
* Intercambiar [Bing Custom Search](https://azure.microsoft.com/services/cognitive-services/bing-custom-search/) en lugar de Bing Web Search
* Usar la funcionalidad de información de imágenes de [Bing Image Search](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) para obtener más información sobre la imagen capturada y encontrar imágenes similares en la web
* Emplear [Bing Spell Check](https://azure.microsoft.com/services/cognitive-services/spell-check/) para mejorar aún más la calidad del texto analizado
* Integrar la [Microsoft Translator](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) para ver el texto extraído en distintos idiomas
* Mezclar y combinar los demás servicios del [portal de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/); ¡no hay límites!
