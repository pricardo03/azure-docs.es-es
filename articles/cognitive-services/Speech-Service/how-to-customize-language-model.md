---
title: 'Creación de un modelo de lenguaje con Speech Service: Microsoft Cognitive Services'
description: Aprenda a crear un modelo de lenguaje con Speech Service en Microsoft Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 0161a691cbec45a87ade218d1707a2784d7f1cfc
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283891"
---
# <a name="tutorial-create-a-custom-language-model"></a>Tutorial: Creación de un modelo de lenguaje personalizado

En este documento, puede crear un modelo de lenguaje personalizado, que puede usar a continuación junto con los modelos de voz más avanzados de Microsoft para agregar interacción de voz a la aplicación.

El documento explica lo siguiente:
> [!div class="checklist"]
> * Preparación de los datos
> * Importación del conjunto de datos de lenguaje
> * Creación de un modelo de lenguaje personalizado

Si no tiene una cuenta de Cognitive Services, cree una [cuenta gratuita](https://azure.microsoft.com/try/cognitive-services/) antes de empezar.

## <a name="prerequisites"></a>requisitos previos

Para asegurarse de que la cuenta de Cognitive Services está conectada a una suscripción, abra la página [Cognitive Services Subscriptions](https://customspeech.ai/Subscriptions) (Suscripciones de Cognitive Services).

Puede conectarse a una suscripción de Search Service creada en Azure Portal haciendo clic en el botón **Connect existing subscription** (Conectar a suscripción existente).

Para obtener información sobre cómo crear una suscripción a Speech Service en Azure Portal, consulte la página de [introducción](get-started.md).

## <a name="prepare-the-data"></a>Preparación de los datos

Para crear un modelo de lenguaje personalizado para la aplicación, necesita proporcionar una lista de expresiones de ejemplo al sistema, por ejemplo:

*   "La paciente ha tenido urticaria la semana pasada".
*   "La paciente tenía una cicatriz de herniorrafia bien curada".

No es necesario que las frases sean completas o gramaticalmente correctas, sino que deben reflejar con precisión la producción hablada que se espera que el sistema encuentre en la implementación. Estos ejemplos deben reflejar tanto el estilo como el contenido de la tarea que los usuarios van a realizar con la aplicación.

Los datos del modelo de lenguaje deben escribirse en BOM UTF-8. El archivo de texto debe contener un ejemplo (frase, expresión o consulta) por línea.

Si desea que algunos términos tengan un mayor peso (importancia), puede agregar varias expresiones que incluyan ese término a los datos. 

En la tabla siguiente, se resumen los principales requisitos para los datos del lenguaje.

| Propiedad | Valor |
|----------|-------|
| Codificación de texto | BOM UTF-8|
| Número de expresiones por línea | 1 |
| Tamaño de archivo máximo | 1,5 GB |
| Comentarios | evitar repetir los caracteres más de cuatro veces, por ejemplo "aaaaaa"|
| Comentarios | ningún carácter especial como "\t" o cualquier otro carácter UTF-8 por encima de U+00A1 en la [tabla de caracteres Unicode](http://www.utf8-chartable.de/)|
| Comentarios | Los identificadores URI también se rechazarán ya que no hay una forma única de pronunciar un URI.|

Cuando se importa el texto, este se normaliza para que lo pueda procesar el sistema. Sin embargo, hay algunas normalizaciones importantes que debe hacer el usuario _antes_ para cargar los datos. Consulte las [Transcription guidelines](prepare-transcription.md) (Directrices de transcripción) para determinar el lenguaje apropiado al preparar sus datos.

## <a name="language-support"></a>Compatibilidad con idiomas

Se admiten los siguientes idiomas para los modelos de lenguaje de **conversión de voz en texto**.

Haga clic para ver una lista completa de los [lenguajes admitidos](supported-languages.md)

## <a name="import-the-language-data-set"></a>Importación del conjunto de datos de lenguaje

Haga clic en el botón "Import" (Importar) en la fila "Language Datasets" (Conjuntos de datos de lenguaje); el sitio mostrará una página para cargar un nuevo conjunto de datos.

Cuando esté listo para importar el conjunto de datos de lenguaje, inicie sesión en el [portal de Speech Service](https://customspeech.ai).  A continuación, haga clic en el menú desplegable "Custom Speech" (Voz personalizada) en la cinta de opciones superior y seleccione "Adaptation Data" (Datos de adaptación). La primera vez que se intentan cargar datos en Speech Service, verá una tabla vacía denominada "Datasets" (Conjuntos de datos).

Para importar un conjunto de datos nuevo, haga clic en el botón "Import" (Importar) en la fila "Language Datasets" (Conjunto de datos de lenguaje) y el sitio muestra una página para cargar un nuevo conjunto de datos. Escriba un nombre y una descripción que le ayuden a identificar el conjunto de datos en el futuro y seleccione la configuración regional. A continuación, utilice el botón "Choose File" (Elegir archivo) para buscar el archivo de texto de datos de lenguaje. A continuación, haga clic en "Import" (Importar) y se cargará el conjunto de datos. Dependiendo del tamaño del conjunto de datos, la importación puede tardar varios minutos.

![probar](media/stt/speech-language-datasets-import.png)

Cuando finalice la importación, volverá a la tabla de datos de lenguaje y verá una entrada que se corresponde con su conjunto de datos de lenguaje. Tenga en cuenta que se le ha asignado un identificador único (GUID). Los datos también tendrán un estado que refleja su estado actual. Su estado será "Waiting" (En espera) mientras está en cola para su procesamiento, "Processing" (Procesando) mientras está en proceso de validación y "Complete" (Completo) cuando los datos estén listos para su uso. La validación de datos realiza una serie de comprobaciones sobre el texto del archivo y una cierta normalización del texto de los datos.

Cuando el estado es "Complete" (Completo), puede hacer clic en "View Report" (Ver informe) para ver el informe de verificación de datos de lenguaje. Se muestra el número de expresiones cuya verificación es correcta y las que han producido un error, junto con detalles sobre las expresiones erróneas. En el ejemplo siguiente, dos ejemplos produjeron errores en la verificación debido a caracteres incorrectos (en este conjunto de datos, la primera línea tenía dos caracteres de tabulación y la segunda tenía varios caracteres fuera del juego de caracteres imprimibles ASCII, mientras que la tercera estaba vacía).

![probar](media/stt/speech-language-datasets-report.png)

Cuando el estado del conjunto de datos de lenguaje es "Complete" (Completo), se puede utilizar para crear un modelo de lenguaje personalizado.

![probar](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Crear un modelo de lenguaje personalizado

Una vez que los datos de lenguaje están listos, haga clic en "Language Models" (Modelos de lenguaje) en el menú desplegable "Menú" para iniciar el proceso de creación del modelo de lenguaje personalizado. Esta página contiene una tabla denominada "Language Models" (Modelos de lenguaje) con los modelos de lenguaje personalizados actuales. Si aún no ha creado ningún modelo de lenguaje personalizado, la tabla estará vacía. La configuración regional actual se muestra en la tabla junto a la entrada de datos pertinente.

Debe seleccionar la configuración regional adecuada antes de realizar cualquier acción. La configuración regional actual se indica en el título de la tabla en todas las páginas de la implementación, el modelo y los datos. Para cambiar la configuración regional, haga clic en el botón "Change Locale" (Cambiar configuración regional) que se encuentra bajo el título de la tabla, lo que le llevará a una página de confirmación de configuración regional. Haga clic en "OK" (Aceptar) para volver a la tabla.

En la página "Create Language Model" (Crear el modelo de lenguaje), escriba un nombre y una descripción para ayudarle a realizar un seguimiento de la información relativa a este modelo, como el conjunto de datos utilizado. A continuación, seleccione el "Base Language Model" (Modelo de lenguaje base) en el menú desplegable. Este modelo será el punto de partida para la personalización. Existen dos modelos de idioma base para elegir. El modelo Search and Dictation es adecuado para la voz dirigida a una aplicación, como comandos, consultas de búsqueda o dictado. El modelo Conversational es adecuado para el reconocimiento de la voz hablada en un estilo conversacional. Este tipo de habla normalmente se dirige a otra persona y se produce en centros de llamadas o en reuniones. Un nuevo modelo denominado "Universal" también está disponible públicamente. Universal pretende abordar todos los escenarios y, en última instancia, reemplazar los modelos Search and Dictation y Conversational.

5.  En el ejemplo anterior, después de especificar el modelo de lenguaje base, seleccione el conjunto de datos de lenguaje que desea utilizar para la personalización mediante el menú desplegable "Language Data" (Datos de lenguaje).

![probar](media/stt/speech-language-models-create2.png)

Al igual que con la creación de modelos acústicos, puede optar por realizar pruebas sin conexión de su nuevo modelo cuando finalice el procesamiento. Las evaluaciones de modelo requieren un conjunto de datos acústico.

Para realizar pruebas sin conexión de su modelo de lenguaje, active la casilla situada junto a "Offline Testing" (Pruebas sin conexión). A continuación, seleccione un modelo acústico en el menú desplegable. Si no ha creado ningún modelo acústico personalizado, los modelos acústicos base de Microsoft serán el único modelo del menú. En caso de que haya escogido un modelo base de LM conversacional, necesita usar un AM conversacional aquí. En caso de que utilice un modelo de búsqueda y dictado de LM, tendrá que seleccionar un modelo de búsqueda y dictado de AM.

Por último, seleccione el conjunto de datos acústico que le gustaría usar para realizar la evaluación.

Cuando esté listo para iniciar el procesamiento, presione "Create" (Crear), lo que le llevará a la tabla de modelos de lenguaje. Habrá una nueva entrada en la tabla correspondiente a este modelo. El estado refleja el estado del modelo y pasará por varios estados incluyendo "Waiting" (En espera), "Processing" (Procesando) y "Complete" (Completo).

Cuando el modelo ha alcanzado el estado "Complete" (Completo), se puede implementar en un punto de conexión. Al hacer clic en "View Result" (Ver resultado), se muestran los resultados de las pruebas sin conexión, si realizan.

Si desea cambiar el nombre o la descripción del modelo en algún momento, puede utilizar el vínculo "Edit" (Editar) en la fila correspondiente de la tabla de modelos de lenguaje.

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Speech](https://azure.microsoft.com/try/cognitive-services/)
- [How to recognize speech in C#](quickstart-csharp-dotnet-windows.md) (Reconocimiento de voz en C#)
- [Datos de ejemplo de Git](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
