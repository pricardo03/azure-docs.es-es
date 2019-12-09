---
title: 'Inicio rápido: Reconocimiento de la voz almacenada en Blob Storage en C#: servicio de voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 96062057a139e4ab2c91792b5c451e093f7f4c96
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74828798"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha cargado de un archivo de origen en un blob de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Abra el proyecto en Visual Studio.

El primer paso es asegurarse de que tiene el proyecto abierto en Visual Studio.

1. Inicie Visual Studio 2019.
2. Cargue el proyecto y abra `Program.cs`.

## <a name="add-a-reference-to-newtonsoftjson"></a>Adición de una referencia a NewtonSoftJSon

1. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **helloworld** y seleccione **Administrar paquetes NuGet** para mostrar el Administrador de paquetes NuGet.

1. En la esquina superior derecha, busque el cuadro desplegable **Origen del paquete** y asegúrese de que **`nuget.org`** está seleccionado.

1. En la esquina superior izquierda, seleccione **Examinar**.

1. En el cuadro de búsqueda, escriba *newtonsoft.json* y seleccione **Entrar**.

1. En los resultados de la búsqueda, seleccione el paquete **Newtonsoft.Json** y, después, seleccione **Instalar** para instalar la versión estable más reciente.

1. Acepte todos los contratos y licencias para iniciar la instalación.

   Después de instalar el paquete aparecerá una confirmación en la ventana **Consola del administrador de paquetes**.

## <a name="start-with-some-boilerplate-code"></a>Inicio con código reutilizable

Vamos a agregar código que funcione como el esqueleto del proyecto.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=6-43,138,277)]
(Deberá reemplazar los valores de `YourSubscriptionKey`, `YourServiceRegion` y `YourFileUrl` por los suyos propios).
## <a name="json-wrappers"></a>Contenedores de JSON

Como las API REST toman las solicitudes en formato JSON y también devuelven resultados en formato JSON, se podría interactuar con ellas solo con el uso de cadenas, aunque no se recomienda.
Para facilitar la administración de solicitudes y respuestas, se declararán algunas clases para la serialización y deserialización del código JSON.

Continúe y coloque sus declaraciones detrás de `TranscribeAsync`.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=140-276)]

## <a name="create-and-configure-an-http-client"></a>Creación y configuración de un cliente HTTP
Lo primero que necesitamos es un cliente HTTP con una dirección URL base y un conjunto de autenticación correctos.
Inserte este código en `TranscribeAsync` [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=46-50)].

## <a name="generate-a-transcription-request"></a>Generación de una solicitud de transcripción
A continuación, se generará la solicitud de transcripción. Agregue este código a `TranscribeAsync` [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=52-57)]

## <a name="send-the-request-and-check-its-status"></a>Envío de la solicitud y comprobación de su estado
Ahora, se va a publicar la solicitud en el servicio de voz y se va a comprobar el código de respuesta inicial. Este código de respuesta indicará simplemente si el servicio ha recibido la solicitud. El servicio devolverá una dirección URL en los encabezados de respuesta, que es la ubicación en la que se almacenará el estado de la transcripción.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=59-70)]

## <a name="wait-for-the-transcription-to-complete"></a>Espere a que finalice la operación.
Dado que el servicio procesa la transcripción de forma asincrónica, es necesario sondear su estado cada cierto tiempo. Aquí se va a comprobar cada cinco segundos.

Para comprobar el estado se puede recuperar el contenido en la dirección URL que obtuvimos al publicar la solicitud. Cuando se recupera el contenido, se deserializa en una de nuestras clases auxiliares para facilitar la interacción con él.

Este es el código de sondeo con la visualización del estado de todas las tareas, excepto el de una finalización correcta, que se hará a continuación.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=72-106,121-137)]

## <a name="display-the-transcription-results"></a>Visualización de los resultados de la transcripción
Cuando el servicio haya finalizado correctamente la transcripción, los resultados se almacenarán en otra dirección URL que se puede obtener de la respuesta de estado.

Aquí se realizará una solicitud para descargar esos resultados en un archivo temporal antes de leerlos y deserializarlos.
Una vez que se cargan los resultados, se pueden imprimir en la consola.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=107-120)]

## <a name="check-your-code"></a>Comprobación del código
En este momento, el código debe tener esta apariencia: (Se han agregado algunos comentarios a esta versión) [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-blob/program.cs?range=6-277)]

## <a name="build-and-run-your-app"></a>Compilación y ejecución de la aplicación

Ya está listo para compilar la aplicación y probar el reconocimiento de voz con el servicio Voz.

1. **Compile el código**: en la barra de menús de Visual Studio, elija **Compilar** > **Compilar solución**.
2. **Inicie la aplicación**: en la barra de menús, elija **Depurar** > **Iniciar depuración** o presione **F5**.
3. **Inicie el reconocimiento**: se le pedirá que diga una frase en inglés. La voz se envía al servicio Voz, se transcribe como texto y se representa en la consola.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]
