---
title: 'Inicio rápido de C#: cambiar el modelo y entrenar la aplicación de LUIS'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido de C#, agregará expresiones de ejemplo a una aplicación de automatización de dispositivos del hogar y entrenará la aplicación. Las expresiones de ejemplo son texto de usuario conversacional que se asigna a una intención. Al proporcionar expresiones de ejemplo para las intenciones, enseñará a LUIS qué clases de texto suministrado por el usuario pertenecen a qué intención.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: aab9651092f7ad5dbc78c31db8df4ef3538fedfc
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039979"
---
# <a name="quickstart-change-model-using-c"></a>Guía de inicio rápido: Cambio del modelo mediante C#

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Versión de [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/) más reciente.
* Tener instalado el lenguaje programación C#.
* Los paquetes NuGet [JsonFormatterPlus](https://www.nuget.org/packages/JsonFormatterPlus) y [CommandLine](https://www.nuget.org/packages/CommandLineParser/)

[!INCLUDE [Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Creación de código de inicio rápido 

En Visual Studio, cree una nueva aplicación **Consola de escritorio clásico de Windows** mediante .NET Framework. 

![Tipo de proyecto de Visual Studio](./media/luis-quickstart-cs-add-utterance/vs-project-type.png)

### <a name="add-the-systemweb-dependency"></a>Adición de la dependencia de System.Web

El proyecto de Visual Studio necesita **System.Web**. En el Explorador de soluciones, haga clic con el botón derecho en **Referencias** y seleccione **Agregar referencia...**

![Adición de la referencia a System.web](./media/luis-quickstart-cs-add-utterance/system.web.png)

### <a name="add-other-dependencies"></a>Incorporación de otras dependencias

El proyecto de Visual Studio necesita **JsonFormatterPlus** y **CommandLineParser**. En el Explorador de soluciones, haga clic con el botón derecho en **Referencias** y seleccione **Administrar paquetes NuGet...** Busque y agregue los dos paquetes. 

![Incorporación de dependencias de terceros](./media/luis-quickstart-cs-add-utterance/add-dependencies.png)


### <a name="write-the-c-code"></a>Escritura del código C#
El archivo **Program.cs** deberá ser:

```CSharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp3
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Agregue las dependencias.

   [!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=1-11 "Add the dependencies")]


Agregue los identificadores y las cadenas de LUIS a la clase **Program**.

   [!code-csharp[Add the LUIS IDs and strings](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=19-30&dedent=8 "Add the LUIS IDs and strings")]

Agregue la clase para administrar los parámetros de línea de comandos a la clase **Program**.

   [!code-csharp[Add class to manage command line parameters.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=32-46 "Add class to manage command-line parameters.")]

Agregue el método de solicitud GET a la clase **Program**.

   [!code-csharp[Add the GET request.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=49-59 "Add the GET request.")]


Agregue el método de solicitud POST a la clase **Program**. 

   [!code-csharp[Add the POST request.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=60-76 "Add the POST request.")]

Agregue expresiones de ejemplo del método de archivo a la clase **Program**.

   [!code-csharp[Add example utterances from file.
](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=77-86 "Add example utterances from file.
")]

Después de aplicar los cambios al modelo, entrene el modelo. Agregue el método a la clase **Program**.

   [!code-csharp[After the changes are applied to the model, train the model.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=87-96 "After the changes are applied to the model, train the model.")]

Puede que el entrenamiento no se complete inmediatamente; compruebe su estado para confirmarlo. Agregue el método a la clase **Program**.

   [!code-csharp[Training may not complete immediately, check status to verify training is complete.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=97-103 "Training may not complete immediately, check status to verify training is complete.")]

Para administrar los argumentos de línea de comandos, agregue el código principal. Agregue el método a la clase **Program**.

   [!code-csharp[To manage command line arguments, add the main code.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=104-137 "To manage command-line arguments, add the main code.")]

### <a name="copy-utterancesjson-to-output-directory"></a>Copia de utterances.json en el directorio de salida

En el Explorador de soluciones, haga clic con el botón derecho en `utterances.json` y seleccione **Propiedades**. En la ventana Propiedades, marque **Acción de compilación** para `Content` y **Copiar en el directorio de salida** para `Copy Always`.  

![Marcado del archivo JSON como contenido](./media/luis-quickstart-cs-add-utterance/content-properties.png)

## <a name="build-code"></a>Compilación del código

Compile el código en Visual Studio. 

## <a name="run-code"></a>Ejecución del código

En el directorio /bin/Debug del proyecto, ejecute la aplicación desde una línea de comandos. 

```CMD
ConsoleApp\bin\Debug> ConsoleApp1.exe --add utterances.json --train --status
```

Esta línea de comandos muestra el resultado de llamar a la API de adición de expresiones. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando haya terminado con la guía de inicio rápido, quite todos los archivos creados en ella. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Creación de una aplicación de LUIS mediante programación](luis-tutorial-node-import-utterances-csv.md) 