---
title: 'Creación de un modelo de inquilino (versión preliminar): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Genere automáticamente un modelo de inquilino (Custom Speech con datos de Office 365) que aproveche los datos de Office 365 para ofrecer un reconocimiento de voz óptimo para términos específicos de una organización y que sea seguro y compatible.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 10/26/2019
ms.author: erhopf
ms.openlocfilehash: 8ca31dcadebf2dc47d5a4b4db715f26fb38e204e
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816383"
---
# <a name="create-a-tenant-model-preview"></a>Creación de un modelo de inquilino (versión preliminar)

El modelo de inquilino (Custom Speech con datos de Office 365) es un servicio de participación para clientes de Office 365 Enterprise que genera automáticamente un modelo de reconocimiento de voz personalizado a partir de los datos de Office 365 de su organización. El modelo creado está optimizado para términos técnicos, jerga y nombres de personas, todo de un modo seguro y compatible.

> [!IMPORTANT]
> Si su organización se inscribe con el modelo de inquilino, el servicio Voz podrá acceder al modelo de lenguaje de su organización, que se genera a partir de los correos electrónicos y los documentos de grupo públicos de Office 365 que pueden ver todos los miembros de la organización. El administrador de Office 365 de la organización puede activar o desactivar el uso del modelo de lenguaje de toda la organización mediante el portal de administración de Office 365.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Inscribirse para usar un modelo de inquilino en el centro de administración de Microsoft 365
> * Obtener una clave de suscripción del servicio Voz
> * Crear un modelo de inquilino
> * Implementar un modelo de inquilino
> * Usar un modelo de inquilino con el SDK de Voz

## <a name="enroll-using-the-microsoft-365-admin-center"></a>Inscripción mediante el centro de administración de Microsoft 365

Antes de implementar el modelo de inquilino, primero debe inscribirse mediante el centro de administración de Microsoft 365. Esta tarea solo la puede realizar el administrador de Microsoft 365.

1. Inicie sesión en el [centro de administración de Microsoft 365](https://admin.microsoft.com ).
2. En el panel izquierdo, seleccione **Configuración** y, a continuación, **Aplicaciones**.

   ![Inscripción en el modelo de inquilino](media/tenant-language-model/tenant-language-model-enrollment.png)

3. Busque y seleccione **Servicios de Voz de Azure**.

   ![Inscripción en el modelo de inquilino 2](media/tenant-language-model/tenant-language-model-enrollment-2.png)

4. Haga clic en la casilla y guarde.

Si tiene que desactivar el modelo de inquilino, vuelva a esta pantalla, anule la selección de la casilla y vuelva a guardar.

## <a name="get-a-speech-subscription-key"></a>Obtener una clave de suscripción del servicio Voz

Para usar un modelo de inquilino con el SDK de Voz, necesitará un recurso de Voz y su clave de suscripción asociada.

1. Inicie sesión en el [Portal de Azure](https://aka.ms/azureportal).
2. Seleccione **Crear un recurso**.
3. En la barra de búsqueda, escriba: **Voz**.
4. Seleccione **Voz** y, a continuación, haga clic en **Crear**.
5. Siga las instrucciones en pantalla para crear el recurso. Asegúrese de que:
   * La **ubicación** se ha establecido en **eastus** o **westus**.
   * El **plan de tarifa** está establecido en **S0**.
6. Haga clic en **Crear**.
7. Después de unos minutos, se crea el recurso. La clave de suscripción está disponible en la sección **Información general** del recurso.

## <a name="create-a-model"></a>Crear un modelo

Una vez que el administrador haya habilitado el modelo de inquilino para su organización, puede crear un modelo de lenguaje basado en los datos de Office 365.

1. Inicie sesión en [Speech Studio](https://speech.microsoft.com/).
2. En la esquina superior derecha, busque y haga clic en el icono de engranaje (configuración) y, a continuación, seleccione **Tenant Model settings** (Configuración del modelo de inquilino).

   ![Menú Configuración](media/tenant-language-model/tenant-language-settings.png)

3. En este momento, verá un mensaje que le informa si está cualificado para crear un modelo de inquilino.
   > [!NOTE]
   > Los clientes empresariales de Office 365 en Norteamérica son aptos para crear un modelo de inquilino (inglés). Si es un cliente de Caja de seguridad del cliente (CLB), Clave de cliente (CK) u Office 365 Administración Pública, esta característica no está disponible. Para averiguarlo, siga estas instrucciones:
   > * [Caja de seguridad del cliente](https://docs.microsoft.com/office365/securitycompliance/controlling-your-data-using-customer-key#FastTrack)
   > * [Clave de cliente](https://docs.microsoft.com/microsoft-365/compliance/customer-lockbox-requests)
   > * [Office 365 Administración Pública](https://www.microsoft.com/microsoft-365/government)

4. A continuación, seleccione **Participar**. Recibirá un correo electrónico con instrucciones cuando el modelo de inquilino esté listo.

## <a name="deploy-your-model"></a>Implementación del modelo

Cuando el modelo de inquilino esté listo, siga estos pasos para implementarlo:

1. Haga clic en el botón **View model** (Ver modelo) del correo electrónico de confirmación que ha recibido o inicie sesión en [Speech Studio](https://speech.microsoft.com/).
2. En la esquina superior derecha, busque y haga clic en el icono de engranaje (configuración) y, a continuación, seleccione **Tenant Model settings** (Configuración del modelo de inquilino).

   ![Menú Configuración](media/tenant-language-model/tenant-language-settings.png)

3. Haga clic en **Implementar**.
4. Cuando se implementa el modelo, el estado cambia a **Implementado**.

## <a name="use-your-model-with-the-speech-sdk"></a>Uso del modelo con el SDK de Voz

Ahora que ha implementado el modelo, puede usarlo con el SDK de Voz. En esta sección, usará el código de ejemplo proporcionado para llamar al servicio de voz mediante la autenticación de Azure AD.

Echemos un vistazo al código que usará para llamar al SDK de Voz en C#. En este ejemplo, realizará el reconocimiento de voz mediante un modelo de inquilino. En esta guía se da por supuesto que la plataforma ya está configurada. Si necesita ayuda con la configuración, consulte [Inicio rápido: Reconocimiento de voz, C# (.NET Core)](quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore).

Copie este código en el proyecto:

```csharp
namespace PrincetonSROnly.FrontEnd.Samples
{
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Net.Http;
    using System.Text;
    using System.Text.RegularExpressions;
    using System.Threading.Tasks;
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Audio;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json.Linq;

    // Note: ServiceApplicationId is a fixed value.  No need to change.

    public class TenantLMSample
    {
        private const string EndpointUriArgName = "EndpointUri";
        private const string SubscriptionKeyArgName = "SubscriptionKey";
        private const string UsernameArgName = "Username";
        private const string PasswordArgName = "Password";
        private const string ClientApplicationId = "f87bc118-1576-4097-93c9-dbf8f45ef0dd";
        private const string ServiceApplicationId = "18301695-f99d-4cae-9618-6901d4bdc7be";

        public static async Task ContinuousRecognitionWithTenantLMAsync(Uri endpointUri, string subscriptionKey, string audioDirPath, string username, string password)
        {
            var config = SpeechConfig.FromEndpoint(endpointUri, subscriptionKey);

            // Passing client specific information for obtaining LM
            if (string.IsNullOrEmpty(username) || string.IsNullOrEmpty(password))
            {
                config.AuthorizationToken = await AcquireAuthTokenWithInteractiveLoginAsync().ConfigureAwait(false);
            }
            else
            {
                config.AuthorizationToken = await AcquireAuthTokenWithUsernamePasswordAsync(username, password).ConfigureAwait(false);
            }

            var stopRecognition = new TaskCompletionSource<int>();

            // Creates a speech recognizer using file as audio input.
            // Replace with your own audio file name.
            using (var audioInput = AudioConfig.FromWavFileInput(audioDirPath))
            {
                using (var recognizer = new SpeechRecognizer(config, audioInput))
                {
                    // Subscribes to events
                    recognizer.Recognizing += (s, e) =>
                    {
                        Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                    };

                    recognizer.Recognized += (s, e) =>
                    {
                        if (e.Result.Reason == ResultReason.RecognizedSpeech)
                        {
                            Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}");
                        }
                        else if (e.Result.Reason == ResultReason.NoMatch)
                        {
                            Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                        }
                    };

                    recognizer.Canceled += (s, e) =>
                    {
                        Console.WriteLine($"CANCELED: Reason={e.Reason}");
                        if (e.Reason == CancellationReason.Error)
                        {
                            Exception exp = new Exception(string.Format("Error Code: {0}\nError Details{1}\nIs your subscription information updated?", e.ErrorCode, e.ErrorDetails));
                            throw exp;
                        }

                        stopRecognition.TrySetResult(0);
                    };

                    recognizer.SessionStarted += (s, e) =>
                    {
                        Console.WriteLine("\n    Session started event.");
                    };

                    recognizer.SessionStopped += (s, e) =>
                    {
                        Console.WriteLine("\n    Session stopped event.");
                        Console.WriteLine("\nStop recognition.");
                        stopRecognition.TrySetResult(0);
                    };

                    // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
                    await recognizer.StartContinuousRecognitionAsync().ConfigureAwait(false);

                    // Waits for completion.
                    // Use Task.WaitAny to keep the task rooted.
                    Task.WaitAny(new[] { stopRecognition.Task });

                    // Stops recognition.
                    await recognizer.StopContinuousRecognitionAsync().ConfigureAwait(false);
                }
            }
        }

        public static void Main(string[] args)
        {
            var arguments = new Dictionary<string, string>();
            string inputArgNamePattern = "--";
            Regex regex = new Regex(inputArgNamePattern);
            if (args.Length > 0)
            {
                foreach (var arg in args)
                {
                    var userArgs = arg.Split("=");
                    arguments[regex.Replace(userArgs[0], string.Empty)] = userArgs[1];
                }
            }

            var endpointString = arguments.GetValueOrDefault(EndpointUriArgName, $"wss://westus.online.princeton.customspeech.ai/msgraphcustomspeech/conversation/v1");
            var endpointUri = new Uri(endpointString);

            if (!arguments.ContainsKey(SubscriptionKeyArgName))
            {
                Exception exp = new Exception("Subscription Key missing! Please pass in a Cognitive services subscription Key using --SubscriptionKey=\"your_subscription_key\"" +
                    "Find more information on creating a Cognitive services resource and accessing your Subscription key here: https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows");
                throw exp;
            }

            var subscriptionKey = arguments[SubscriptionKeyArgName];
            var username = arguments.GetValueOrDefault(UsernameArgName, null);
            var password = arguments.GetValueOrDefault(PasswordArgName, null);

            var audioDirPath = Path.Combine(Path.GetDirectoryName(System.Reflection.Assembly.GetExecutingAssembly().Location), "../../../AudioSamples/DictationBatman.wav");
            if (!File.Exists(audioDirPath))
            {
                Exception exp = new Exception(string.Format("Audio File does not exist at path: {0}", audioDirPath));
                throw exp;
            }

            ContinuousRecognitionWithTenantLMAsync(endpointUri, subscriptionKey, audioDirPath, username, password).GetAwaiter().GetResult();
        }

        private static async Task<string> AcquireAuthTokenWithUsernamePasswordAsync(string username, string password)
        {
            var tokenEndpoint = "https://login.microsoftonline.com/common/oauth2/token";
            var postBody = $"resource={ServiceApplicationId}&client_id={ClientApplicationId}&grant_type=password&username={username}&password={password}";
            var stringContent = new StringContent(postBody, Encoding.UTF8, "application/x-www-form-urlencoded");
            using (HttpClient httpClient = new HttpClient())
            {
                var response = await httpClient.PostAsync(tokenEndpoint, stringContent).ConfigureAwait(false);

                if (response.IsSuccessStatusCode)
                {
                    var result = await response.Content.ReadAsStringAsync().ConfigureAwait(false);

                    JObject jobject = JObject.Parse(result);
                    return jobject["access_token"].Value<string>();
                }
                else
                {
                    throw new Exception($"Requesting token from {tokenEndpoint} failed with status code {response.StatusCode}: {await response.Content.ReadAsStringAsync().ConfigureAwait(false)}");
                }
            }
        }

        private static async Task<string> AcquireAuthTokenWithInteractiveLoginAsync()
        {
            var authContext = new AuthenticationContext("https://login.windows.net/microsoft.onmicrosoft.com");
            var deviceCodeResult = await authContext.AcquireDeviceCodeAsync(ServiceApplicationId, ClientApplicationId).ConfigureAwait(false);

            Console.WriteLine(deviceCodeResult.Message);

            var authResult = await authContext.AcquireTokenByDeviceCodeAsync(deviceCodeResult).ConfigureAwait(false);

            return authResult.AccessToken;
        }
    }
}
```

A continuación, necesitará volver a compilar y ejecutar el proyecto desde la línea de comandos. Hay algunos parámetros que deberá actualizar antes de ejecutar el comando.

1. Reemplace `<Username>` y `<Password>` por los valores de un usuario de inquilino válido.
2. Reemplace `<Subscription-Key>` por la clave de suscripción del recurso de Voz. Este valor está disponible en la hoja de **información general** del recurso de Voz de [Azure Portal](https://aka.ms/azureportal).
3. Reemplace `<Endpoint-Uri>` por el punto de conexión siguiente. Asegúrese de que reemplaza `{your-region}` por la región donde se creó el recurso de Voz. Se admiten estas regiones: `westus`, `westus2`y `eastus`. La información de la región está disponible en la sección de **información general** del recurso de Voz de [Azure Portal](https://aka.ms/azureportal).
   ```
   "wss://{your region}.online.princeton.customspeech.ai/msgraphcustomspeech/conversation/v1".
   ```
4. Ejecute el comando:
   ```bash
   dotnet TenantLMSample.dll --Username=<Username> --Password=<Password> --SubscriptionKey=<Subscription-Key> --EndpointUri=<Endpoint-Uri>
   ```

## <a name="next-steps"></a>Pasos siguientes

* [Speech Studio](https://speech.microsoft.com/)
* [Acerca del SDK de Voz](speech-sdk.md)
