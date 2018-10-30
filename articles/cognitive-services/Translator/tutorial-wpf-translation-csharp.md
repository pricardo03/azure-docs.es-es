---
title: 'Tutorial: Creación de una aplicación WPF con-Translator Text API en C#'
titleSuffix: Azure Cognitive Services
description: En este tutorial, crearemos una aplicación de WPF con C# para aprender a utilizar Translator Text API para traducir texto, obtener una lista localizada de los idiomas admitidos y mucho más.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: tutorial
ms.date: 07/20/2018
ms.author: erhopf
ms.openlocfilehash: e302c1aa1cd4021b0d449fd981181b790546c0f8
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647483"
---
# <a name="tutorial-write-a-wpf-application-for-translator-text-using-c35"></a>Tutorial: Escritura de una aplicación de WPF para Translator Text con C#

En este tutorial, va a compilar una herramienta interactiva de traducción de texto mediante Translator Text API (v3), que forma parte de Microsoft Cognitive Services en Azure. Aprenderá a:

> [!div class="checklist"]
> * Obtener una lista de los idiomas admitidos por el servicio
> * Realizar una traducción del texto escrito por el usuario de un idioma a otro

Esta aplicación también incluye la integración con otros dos servicios de Microsoft Cognitive Services.

|||
|-|-|
|[Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/)|Se utiliza como opción para detectar automáticamente el idioma de origen del texto que se va a traducir.|
|[Bing Spell Check](https://azure.microsoft.com/services/cognitive-services/spell-check/)|Para el texto de origen en inglés, se utiliza para corregir errores de ortografía, de modo que la traducción sea más precisa.

![[El programa tutorial en ejecución]](media/translator-text-csharp-session.png)

## <a name="prerequisites"></a>Requisitos previos

Se requiere [Visual Studio 2017](https://www.visualstudio.com/downloads/) para ejecutar este código en Windows. (La edición gratuita de Community Edition funcionará).

También necesita claves de suscripción para los tres servicios de Azure usados en el programa. Puede obtener una clave para el servicio de Translator Text desde el panel de Azure. Hay un plan de tarifa gratuito que permite traducir hasta dos millones de caracteres al mes sin cargo.

Tanto el servicio de Text Analytics como el de Bing Spell Check ofrecen evaluaciones gratuitas, para las que puede registrarse en [Probar Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). También puede crear una suscripción para uno de los servicios a través del panel de Azure. Text Analytics tiene un plan gratuito.

Código fuente de este tutorial está disponible a continuación. Las claves de suscripción se deben copiar en el código fuente como las variables `TEXT_TRANSLATION_API_SUBSCRIPTION_KEY`, y así sucesivamente, en `MainWindow.xaml.cs`.

> [!IMPORTANT]
> El servicio de Text Analytics está disponible en varias regiones. El URI en el código fuente del tutorial se encuentra en la región `westus`, que es la región que se utiliza para las evaluaciones gratuitas. Si tiene una suscripción en otra región, actualice este URI en consecuencia.

## <a name="source-code"></a>Código fuente

Este es el código fuente para Microsoft Translator Text API. Para ejecutar la aplicación, copie el código fuente en el archivo apropiado en un nuevo proyecto de WPF en Visual Studio.

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Este es el archivo de código subyacente que proporciona la funcionalidad de la aplicación.

```csharp
using System;
using System.Windows;
using System.Net;
using System.Net.Http;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using System.Text;

// NOTE: Add assembly references to System.Runtime.Serialization, System.Web, and System.Web.Extensions.

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace MSTranslatorTextDemo
{
    /// <summary>
    /// This WPF application demonstrates the use of the Microsoft Translator Text API to translate a brief text string from
    /// one language to another. The languages are selected from a drop-down menu. The text of the translation is displayed.
    /// The source language may optionally be automatically detected. English text is spell-checked.
    /// </summary>
    public partial class MainWindow : Window
    {
        // Translator text subscription key from Microsoft Azure dashboard
        const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

        public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
        const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
        const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

        private string[] languageCodes;     // array of language codes

        // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
        private SortedDictionary<string, string> languageCodesAndTitles =
            new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

        public MainWindow()
        {
            // at least show an error dialog if there's an unexpected error
            AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

            if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
                || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
                || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
            {
                MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                    "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                    "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
                System.Windows.Application.Current.Shutdown();
            }
            else
            {
                InitializeComponent();          // start the GUI
                GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
                PopulateLanguageMenus();        // fill the drop-down language lists
            }
        }

        // Global exception handler to display error message and exit
        private static void HandleExceptions(object sender, UnhandledExceptionEventArgs args)
        {
            Exception e = (Exception)args.ExceptionObject;
            MessageBox.Show("Caught " + e.Message, "Error", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }

        // ***** POPULATE LANGUAGE MENUS
        private void PopulateLanguageMenus()
        {
            // Add option to automatically detect the source language
            FromLanguageComboBox.Items.Add("Detect");

            int count = languageCodesAndTitles.Count;
            foreach (string menuItem in languageCodesAndTitles.Keys)
            {
                FromLanguageComboBox.Items.Add(menuItem);
                ToLanguageComboBox.Items.Add(menuItem);
            }

            // set default languages
            FromLanguageComboBox.SelectedItem = "Detect";
            ToLanguageComboBox.SelectedItem = "English";
        }

        // ***** DETECT LANGUAGE OF TEXT TO BE TRANSLATED
        private string DetectLanguage(string text)
        {
            string uri = TEXT_ANALYTICS_API_ENDPOINT + "languages?numberOfLanguagesToDetect=1";

            // create request to Text Analytics API
            HttpWebRequest detectLanguageWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_ANALYTICS_API_SUBSCRIPTION_KEY);
            detectLanguageWebRequest.Method = "POST";

            // create and send body of request
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            string jsonText = serializer.Serialize(text);

            string body = "{ \"documents\": [ { \"id\": \"0\", \"text\": " + jsonText + "} ] }";
            byte[] data = Encoding.UTF8.GetBytes(body);
            detectLanguageWebRequest.ContentLength = data.Length;

            using (var requestStream = detectLanguageWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);

            HttpWebResponse response = (HttpWebResponse)detectLanguageWebRequest.GetResponse();

            // read and parse JSON response
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);

            // fish out the detected language code
            var languageInfo = jsonResponse["documents"][0]["detectedLanguages"][0];
            if (languageInfo["score"] > (decimal)0.5)
                return languageInfo["iso6391Name"];
            else
                return "";
        }

        // ***** CORRECT SPELLING OF TEXT TO BE TRANSLATED
        private string CorrectSpelling(string text)
        {
            string uri = BING_SPELL_CHECK_API_ENDPOINT + "?mode=spell&mkt=en-US";

            // create request to Bing Spell Check API
            HttpWebRequest spellCheckWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            spellCheckWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", BING_SPELL_CHECK_API_SUBSCRIPTION_KEY);
            spellCheckWebRequest.Method = "POST";
            spellCheckWebRequest.ContentType = "application/x-www-form-urlencoded"; // doesn't work without this

            // create and send body of request
            string body = "text=" + System.Web.HttpUtility.UrlEncode(text);
            byte[] data = Encoding.UTF8.GetBytes(body);
            spellCheckWebRequest.ContentLength = data.Length;
            using (var requestStream = spellCheckWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);
            HttpWebResponse response = (HttpWebResponse)spellCheckWebRequest.GetResponse();

            // read and parse JSON response and get spelling corrections
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);
            var flaggedTokens = jsonResponse["flaggedTokens"];

            // construct sorted dictionary of corrections in reverse order in string (right to left)
            // so that making a correction can't affect later indexes
            var corrections = new SortedDictionary<int, string[]>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
            for (int i = 0; i < flaggedTokens.Length; i++)
            {
                var correction = flaggedTokens[i];
                var suggestion = correction["suggestions"][0];  // consider only first suggestion
                if (suggestion["score"] > (decimal)0.7)         // take it only if highly confident
                    corrections[(int)correction["offset"]] = new string[]   // dict key   = offset
                        { correction["token"], suggestion["suggestion"] };  // dict value = {error, correction}
            }

            // apply the corrections in order from right to left
            foreach (int i in corrections.Keys)
            {
                var oldtext = corrections[i][0];
                var newtext = corrections[i][1];

                // apply capitalization from original text to correction - all caps or initial caps
                if (text.Substring(i, oldtext.Length).All(char.IsUpper)) newtext = newtext.ToUpper();
                else if (char.IsUpper(text[i])) newtext = newtext[0].ToString().ToUpper() + newtext.Substring(1);

                text = text.Substring(0, i) + newtext + text.Substring(i + oldtext.Length);
            }

            return text;
        }

        // ***** GET TRANSLATABLE LANGUAGE CODES
        private void GetLanguagesForTranslate()
        {
            // send request to get supported language codes
            string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
            WebRequest WebRequest = WebRequest.Create(uri);
            WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
            WebRequest.Headers.Add("Accept-Language", "en");
            WebResponse response = null;
            // read and parse the JSON response
            response = WebRequest.GetResponse();
            using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
            {
                var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
                var languages = result["translation"];

                languageCodes = languages.Keys.ToArray();
                foreach (var kv in languages)
                {
                    languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
                }
            }
        }

        // ***** PERFORM TRANSLATION ON BUTTON CLICK
        private async void TranslateButton_Click(object sender, EventArgs e)
        {
            string textToTranslate = TextToTranslate.Text.Trim();

            string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
            string fromLanguageCode;

            // auto-detect source language if requested
            if (fromLanguage == "Detect")
            {
                fromLanguageCode = DetectLanguage(textToTranslate);
                if (!languageCodes.Contains(fromLanguageCode))
                {
                    MessageBox.Show("The source language could not be detected automatically " +
                        "or is not supported for translation.", "Language detection failed",
                        MessageBoxButton.OK, MessageBoxImage.Error);
                    return;
                }
            }
            else
                fromLanguageCode = languageCodesAndTitles[fromLanguage];

            string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

            // spell-check the source text if the source language is English
            if (fromLanguageCode == "en")
            {
                if (textToTranslate.StartsWith("-"))    // don't spell check in this case
                    textToTranslate = textToTranslate.Substring(1);
                else
                {
                    textToTranslate = CorrectSpelling(textToTranslate);
                    TextToTranslate.Text = textToTranslate;     // put corrected text into input field
                }
            }

            // handle null operations: no text or same source/target languages
            if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
            {
                TranslatedTextLabel.Content = textToTranslate;
                return;
            }

            // send HTTP request to perform the translation
            string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
            string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

            System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
                request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();

                var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
                var translations = result[0]["translations"];
                var translation = translations[0]["text"];

                // Update the translation field
                TranslatedTextLabel.Content = translation;
            }
        }
    }
}
```

### <a name="mainwindowxaml"></a>MainWindow.xaml

Este archivo define la interfaz de usuario para la aplicación, un formulario de WPF. Si desea diseñar su propia versión del formulario, no necesita este código XAML.

```xml
<Window x:Class="MSTranslatorTextDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:MSTranslatorTextDemo"
        mc:Ignorable="d"
        Title="Microsoft Translator" Height="400" Width="700" BorderThickness="0">
    <Grid>
        <Label x:Name="label" Content="Microsoft Translator" HorizontalAlignment="Left" Margin="39,6,0,0" VerticalAlignment="Top" Height="49" FontSize="26.667"/>
        <TextBox x:Name="TextToTranslate" HorizontalAlignment="Left" Height="23" Margin="42,160,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="600" FontSize="14" TabIndex="3"/>
        <Label x:Name="EnterTextLabel" Content="Text to translate:" HorizontalAlignment="Left" Margin="40,129,0,0" VerticalAlignment="Top" FontSize="14"/>
        <Label x:Name="toLabel" Content="Translate to:" HorizontalAlignment="Left" Margin="304,58,0,0" VerticalAlignment="Top" FontSize="14"/>

        <Button x:Name="TranslateButton" Content="Translate" HorizontalAlignment="Left" Margin="39,206,0,0" VerticalAlignment="Top" Width="114" Height="31" Click="TranslateButton_Click" FontSize="14" TabIndex="4" IsDefault="True"/>
        <ComboBox x:Name="ToLanguageComboBox"
                HorizontalAlignment="Left"
                Margin="306,88,0,0"
                VerticalAlignment="Top"
                Width="175" FontSize="14" TabIndex="2">

        </ComboBox>
        <Label x:Name="fromLabel" Content="Translate from:" HorizontalAlignment="Left" Margin="40,58,0,0" VerticalAlignment="Top" FontSize="14"/>
        <ComboBox x:Name="FromLanguageComboBox"
            HorizontalAlignment="Left"
            Margin="42,88,0,0"
            VerticalAlignment="Top"
            Width="175" FontSize="14" TabIndex="1"/>
        <Label x:Name="TranslatedTextLabel" Content="Translation appears here" HorizontalAlignment="Left" Margin="39,255,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="85" BorderThickness="0"/>
    </Grid>
</Window>
```

## <a name="service-endpoints"></a>Puntos de conexión de servicio

El servicio de Microsoft Translator tiene varios puntos de conexión que proporcionan distintos elementos de la funcionalidad de traducción. Los que se utilizan en este tutorial son los siguientes:

|||
|-|-|
|`Languages`|Devuelve el conjunto de idiomas admitidos actualmente por otras operaciones de Translator Text API.|
|`Translate`|Dado el texto de origen, un código de idioma de origen y un código de idioma de destino, devuelve una traducción del texto de origen en el idioma de destino.|

## <a name="the-translation-app"></a>La aplicación de traducción

La interfaz de usuario de la aplicación de traductor se basa en Windows Presentation Foundation (WPF). Para crear un nuevo proyecto de WPF en Visual Studio, siga estos pasos.

* En el menú **Archivo**, haga clic en **Nuevo > Proyecto**.
* En la ventana Nuevo proyecto, abra **Instalado > Plantillas > Visual C#**. Aparece una lista de las plantillas de proyecto disponibles en el centro del cuadro de diálogo.
* Asegúrese de que **.NET Framework 4.5.2** esté seleccionado en el menú desplegable situado sobre la lista de plantillas de proyecto.
* Haga clic en **Aplicación de WPF (.NET Framework)** en la lista de plantillas de proyecto.
* Con los campos de la parte inferior del cuadro de diálogo, asigne un nombre al nuevo proyecto y a la solución que lo contiene.
* Haga clic en **Aceptar** para crear el nuevo proyecto y la solución.

![[Creación de una nueva aplicación de WPF en Visual Studio]](media/translator-text-csharp-new-project.png)

Agregue referencias a los siguientes ensamblados de .NET Framework para el proyecto.

* System.Runtime.Serialization
* System.Web
* System.Web.Extensions

Además, instale el paquete NuGet `Newtonsoft.Json` en el proyecto.

Ahora busque el archivo `MainWindow.xaml` en el Explorador de soluciones y ábralo. En un principio, está en blanco. Así es como se debería ver la interfaz de usuario terminada, anotada con los nombres de los controles en azul. Use los mismos nombres para los controles en la interfaz de usuario, ya que también aparecen en el código.

![[Vista anotada de ventana principal en el diseñador de Visual Studio]](media/translator-text-csharp-xaml.png)

> [!NOTE]
> El código fuente de este tutorial incluye el origen XAML de este formulario. Puede pegarlo en el proyecto, en lugar de compilar el formulario en Visual Studio.

* `FromLanguageComboBox` *(cuadro combinado)*: muestra una lista de los idiomas admitidos por Microsoft Translator para la traducción de texto. El usuario selecciona el idioma del que está traduciendo.
* `ToLanguageComboBox` *(cuadro combinado)*: muestra la misma lista de idiomas que `FromComboBox`, pero se utiliza para seleccionar el idioma al que se traduce.
* `TextToTranslate` *(cuadro de texto)*: el usuario escribe aquí el texto que se va a traducir.
* `TranslateButton` *(botón)*: el usuario hace clic en este botón (o presiona la tecla ENTRAR) para traducir el texto.
* `TranslatedTextLabel` *(etiqueta)*: aquí aparece la traducción del texto del usuario.

Si crea su propia versión de este formulario, no es necesario para que sea *exactamente* igual a esta. Pero asegúrese de que las listas desplegables de idiomas sean lo suficientemente anchos con el fin de evitar que los nombres de idioma queden cortados.

## <a name="the-mainwindow-class"></a>Clase MainWindow

El archivo de código subyacente `MainWindow.xaml.cs` es donde va el código que hace que el programa haga lo que hace. El trabajo se produce en dos momentos:

* Cuando se inicia el programa y se crea una instancia de `MainWindow`, se recupera la lista de idiomas mediante del traductor y las API y se rellenan los menús desplegables con ellos. Esta tarea se realiza una vez, al principio de cada sesión.

* Cuando el usuario hace clic en el botón **Translate** (Traducir), se recuperan las selecciones de idioma del usuario y el texto que escribieron y, después, se llama a `Translate` API para realizar la traducción. También es posible llamar a otras funciones para determinar el idioma del texto y para corregir la ortografía antes de la traducción.

Eche un vistazo al comienzo de la clase:

```csharp
public partial class MainWindow : Window
{
    // Translator text subscription key from Microsoft Azure dashboard
    const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

    public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
    const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
    const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

    private string[] languageCodes;     // array of language codes

    // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
    private SortedDictionary<string, string> languageCodesAndTitles =
        new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

    public MainWindow()
    {
        // at least show an error dialog if there's an unexpected error
        AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

        if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
            || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
            || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
        {
            MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }
        else
        {
            InitializeComponent();          // start the GUI
            GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
            PopulateLanguageMenus();        // fill the drop-down language lists
        }
    }
// more to come
}
```

Dos de las variables de miembro declaradas aquí contienen información acerca de los idiomas:

|||
|-|-|
|`languageCodes`<br>matriz de cadena|Almacena en caché los códigos de idioma. El servicio de Translator utiliza códigos cortos, como `en` para inglés, para identificar los idiomas.|
|`languageCodesAndTitles`<br>SortedDictionary|Asigna los nombres "descriptivos" en la interfaz de usuario a los códigos cortos que se utilizan en la API. Se mantienen ordenados por orden alfabético sin tener en cuenta las mayúsculas y minúsculas.|

El primer código ejecutado por la aplicación es el constructor `MainWindow`. En primer lugar, se configura el método `HandleExceptions` como controlador de errores global. De esta manera, hay al menos una alerta de error si no se controla la excepción.

A continuación, se comprueba que las claves de suscripción de la API tengan todas exactamente 32 caracteres. Si no es así, el motivo más probable es que *alguien* no ha pegado sus claves de API. En este caso, se muestra un mensaje de error y se sale. (Claro que, pasar esta prueba no significa que las claves sean válidas).

Si hay claves que tengan al menos la longitud adecuada, la llamada `InitializeComponent()` da inicio a la interfaz de usuario al ubicar, cargar y crear instancias de la descripción de XAML de la ventana de la aplicación principal.

Por último, se configuran los menús desplegables de idioma. Esta tarea requiere tres llamadas de método independientes, que se explican con detalle en las secciones siguientes.

## <a name="get-supported-languages"></a>Obtener idiomas admitidos

El servicio de Microsoft Translator admite un total de 61 idiomas en el momento de escribir este documento, y puede que se agreguen más oportunamente. Por lo que es preferible no codificar de forma rígida los idiomas admitidos en el programa. En su lugar, pregunte al servicio de Translator qué idiomas admite. Cualquier idioma compatible se puede traducir a cualquier otro idioma compatible.

Llame a la API `Languages` para obtener la lista de idiomas admitidos.

La API `Languages` toma un parámetro de consulta GET opcional, *scope*. *scope* puede tener uno de tres valores: `translation`, `transliteration` o `dictionary`. Este código usa el valor `translation`.

La API `Languages` también toma un encabezado HTTP opcional, `Accept-Language`. El valor de este encabezado determina el idioma en que se devuelven los nombres de los idiomas admitidos. El valor debe ser una etiqueta de idioma BCP 47 bien formada. Este código usa el valor `en` para obtener los nombres de los idiomas en inglés.

La API `Languages` devuelve una respuesta JSON que es similar a la siguiente.

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
}
```

Para poder extraer los códigos de idioma (por ejemplo, `af`) y los nombres de idioma (por ejemplo, `Afrikaans`), este código utiliza el método NewtonSoft.Json [JsonConvert.DeserializeObject](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm).

Con estos conocimientos, se crea el siguiente método para recuperar los códigos de idioma y sus nombres.

```csharp
private void GetLanguagesForTranslate()
{
    // send request to get supported language codes
    string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
    WebRequest WebRequest = WebRequest.Create(uri);
    WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
    WebRequest.Headers.Add("Accept-Language", "en");
    WebResponse response = null;
    // read and parse the JSON response
    response = WebRequest.GetResponse();
    using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
    {
        var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
        var languages = result["translation"];

        languageCodes = languages.Keys.ToArray();
        foreach (var kv in languages)
        {
            languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
        }
    }
}
```

`GetLanguagesForTranslate()` primero crea la solicitud HTTP. El parámetro de la cadena de consulta `scope=translation` solicita solo esos idiomas admitidos para la traducción de texto. Se agrega la clave de suscripción de Text Translation API a los encabezados de la solicitud. Se agrega el encabezado `Accept-Language` con el valor `en` para que se devuelvan los idiomas admitidos en inglés.

Una vez completada la solicitud, la respuesta de JSON se analiza y se convierte en un diccionario y, después, los códigos de idioma se agregan a la variable miembro `languageCodes`. Se recorren los pares de clave/valor que contienen los códigos de idioma y los nombres de idioma descriptivos, y se agregan a la variable miembro `languageCodesAndTitles`. (En los menús desplegables del formulario se muestran los nombres descriptivos, pero se necesitan los códigos para solicitar la traducción).

## <a name="populate-the-language-menus"></a>Relleno de los menús de idioma

La mayor parte de la interfaz de usuario se define en XAML, por lo que no es necesario hacer mucho para configurarla, además de llamar a `InitializeComponent()`. Lo otro que necesita hacer es agregar los nombres de idiomas descriptivos en las listas desplegables **Translate from** (Traducir de) y **Translate to** (Traducir a), que se realiza en `PopulateLanguageMenus()`.

```csharp
private void PopulateLanguageMenus()
{
    // Add option to automatically detect the source language
    FromLanguageComboBox.Items.Add("Detect");

    int count = languageCodesAndTitles.Count;
    foreach (string menuItem in languageCodesAndTitles.Keys)
    {
        FromLanguageComboBox.Items.Add(menuItem);
        ToLanguageComboBox.Items.Add(menuItem);
    }

    // set default languages
    FromLanguageComboBox.SelectedItem = "Detect";
    ToLanguageComboBox.SelectedItem = "English";
}
```

Rellenar los menús es tan sencillo como iterar por el diccionario `languageCodesAndTitles` y agregar cada clave, que es el nombre "descriptivo", a ambos menús. Después de rellenar los menús, se establecen los idiomas de origen y destino predeterminados en **Detect** (Detectar) (para detectar automáticamente el idioma) e **English** (Inglés).

> [!TIP]
> Sin una selección predeterminada para los menús, el usuario puede hacer clic en **Translate** (Traducir) sin elegir el idioma de origen ni el de destino. Los valores predeterminados eliminan la necesidad de tratar este problema.

Ahora que `MainWindow` se ha inicializado y se ha creado la interfaz de usuario, el código espera hasta que el usuario hace clic en el botón **Translate** (Traducir).

## <a name="perform-translation"></a>Realizar la traducción

Cuando el usuario hace clic en **Translate**, WPF invoca el controlador de eventos `TranslateButton_Click()`, que se muestra aquí.

```csharp
private async void TranslateButton_Click(object sender, EventArgs e)
{
    string textToTranslate = TextToTranslate.Text.Trim();

    string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
    string fromLanguageCode;

    // auto-detect source language if requested
    if (fromLanguage == "Detect")
    {
        fromLanguageCode = DetectLanguage(textToTranslate);
        if (!languageCodes.Contains(fromLanguageCode))
        {
            MessageBox.Show("The source language could not be detected automatically " +
                "or is not supported for translation.", "Language detection failed",
                MessageBoxButton.OK, MessageBoxImage.Error);
            return;
        }
    }
    else
        fromLanguageCode = languageCodesAndTitles[fromLanguage];

    string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

    // spell-check the source text if the source language is English
    if (fromLanguageCode == "en")
    {
        if (textToTranslate.StartsWith("-"))    // don't spell check in this case
            textToTranslate = textToTranslate.Substring(1);
        else
        {
            textToTranslate = CorrectSpelling(textToTranslate);
            TextToTranslate.Text = textToTranslate;     // put corrected text into input field
        }
    }

    // handle null operations: no text or same source/target languages
    if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
    {
        TranslatedTextLabel.Content = textToTranslate;
        return;
    }

    // send HTTP request to perform the translation
    string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
    string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

    System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
    var requestBody = JsonConvert.SerializeObject(body);

    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = HttpMethod.Post;
        request.RequestUri = new Uri(uri);
        request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
        request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
        request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();

        var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
        var translations = result[0]["translations"];
        var translation = translations[0]["text"];

        // Update the translation field
        TranslatedTextLabel.Content = translation;
    }
}
```

El primer paso consiste en recuperar del formulario los idiomas de origen y de destino, junto con el texto que el usuario ha escrito.

Si el idioma de origen se establece en **Detect** (Detectar), se llama a `DetectLanguage()` para determinar el idioma del texto. El texto puede estar en un idioma que las API de Translator no admitan (se pueden detectar muchos más idiomas de los que se pueden traducir) o puede que Text Analytics API no pueda detectarlo. En ese caso, se muestra un mensaje para informar al usuario y devolver el texto sin traducir.

Si el idioma de origen es el inglés (así se haya especificado o detectado), se revisa la ortografía del texto con `CorrectSpelling()` y se aplican las correcciones. El campo de entrada se vuelve a rellenar con el texto corregido para que el usuario sepa que se ha realizado la corrección. (El usuario puede preceder el texto que se va a traducir con un guion para suprimir la corrección ortográfica).

Si el usuario no ha escrito ningún texto, o si los idiomas de origen y destino son los mismos, no es necesaria ninguna traducción y la solicitud se puede omitir.

El código para realizar la solicitud de traducción debe parecer familiar: cree el URI, cree una solicitud, envíela y analice la respuesta. Para mostrar el texto, envíelo al control `TranslatedTextLabel`.

A continuación, pasamos texto a `Translate` API en una matriz JSON serializada en el cuerpo de una solicitud POST. La matriz JSON puede contener varios fragmentos de texto para traducir, pero aquí solo se requiere uno.

El encabezado HTTP denominado `X-ClientTraceId` es opcional. El valor debe ser un GUID. El identificador de seguimiento proporcionado por el cliente es útil para realizar el seguimiento de las solicitudes cuando las cosas no funcionan según lo previsto. Sin embargo, para que sea útil, el cliente debe registrar el valor de X-ClientTraceID. Un identificador de seguimiento del cliente y la fecha de las solicitudes pueden ayudar a Microsoft a diagnosticar los problemas que pueden producirse.

> [!NOTE]
> Este tutorial se centra en el servicio Microsoft Translator, por lo que no se abordan los métodos `DetectLanguage()` y `CorrectSpelling()` con detalle.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de Microsoft Translator Text API](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
