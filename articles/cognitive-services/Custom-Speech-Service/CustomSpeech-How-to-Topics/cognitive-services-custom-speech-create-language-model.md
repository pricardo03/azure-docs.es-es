---
title: Tutorial para crear un modelo de lenguaje con Custom Speech Service de Microsoft Cognitive Services | Microsoft Docs
description: En este tutorial, aprenderá a crear un modelo de lenguaje con Custom Speech Service de Microsoft Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ms.openlocfilehash: 6af2da9ffc7678a58fcf1c647ba89c586066d2ad
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339103"
---
# <a name="tutorial-create-a-custom-language-model"></a>Tutorial: Creación de un modelo de lenguaje personalizado

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

En este tutorial, va a crear un modelo de lenguaje personalizado para consultas o expresiones de texto que espera que los usuarios digan o escriban en una aplicación. A continuación, puede utilizar este modelo de lenguaje personalizado junto con los modelos de voz más avanzados de Microsoft para agregar interacción de voz a la aplicación.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Preparación de los datos
> * Importación del conjunto de datos de lenguaje
> * Creación de un modelo de lenguaje personalizado

Si no tiene una cuenta de Cognitive Services, cree una [cuenta gratuita](https://cris.ai) antes de empezar.

## <a name="prerequisites"></a>requisitos previos

Para asegurarse de que la cuenta de Cognitive Services está conectada a una suscripción, abra la página [Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Suscripciones de Cognitive Services).

Si no aparece ninguna suscripción, puede hacer que Cognitive Services creen una cuenta haciendo clic en el botón **Get free subscription** (Obtener suscripción gratuita). O puede conectarse a una suscripción de Custom Search Service creada en Azure Portal haciendo clic en el botón **Connect existing subscription** (Conectar a suscripción existente).

Para más información sobre cómo crear una suscripción de Custom Search Service en Azure Portal, consulte [Create a Cognitive Services APIs account in the Azure portal](../../cognitive-services-apis-create-account.md) (Creación de una cuenta de Cognitive Services APIs en Azure Portal).

## <a name="prepare-the-data"></a>Preparación de los datos

Para crear un modelo de lenguaje personalizado para la aplicación, necesita proporcionar una lista de expresiones de ejemplo al sistema, por ejemplo:

*   "Ha tenido urticaria la semana pasada".
*   "La paciente tenía una cicatriz de herniorrafia bien curada".

No es necesario que las frases sean completas o gramaticalmente correctas, y deben reflejar con precisión la información hablada que el usuario espera que el sistema encuentre en la implementación. Estos ejemplos deben reflejar tanto el estilo como el contenido de la tarea que los usuarios van a realizar con la aplicación.

Los datos del modelo de lenguaje deben escribirse en un archivo de texto sin formato con US-ASCII o UTF-8, dependiendo de la configuración regional. Para en-US, se admiten ambas codificaciones. Para zh-CN, se admite solo UTF-8 (BOM es opcional). El archivo de texto debe contener un ejemplo (frase, expresión o consulta) por línea.

Si desea que algunas frases tengan un mayor peso (importancia), puede agregarlas varias veces a los datos. Un buen número de repeticiones es entre 10 y 100. Si lo normaliza a 100, puede ponderar la frase relativa a esto fácilmente.

En la tabla siguiente, se resumen los principales requisitos para los datos del lenguaje.

| Propiedad | Valor |
|----------|-------|
| Codificación de texto | en-US: US-ACSII o UTF-8 o zh-CN: UTF-8|
| Número de expresiones por línea | 1 |
| Tamaño de archivo máximo | 200 MB |
| Comentarios | evitar repetir los caracteres más de 4 veces, por ejemplo "aaaaaa"|
| Comentarios | ningún carácter especial como "\t" o cualquier otro carácter UTF-8 por encima de U+00A1 en la [tabla de caracteres Unicode](http://www.utf8-chartable.de/)|
| Comentarios | Los identificadores URI también se rechazarán ya que no hay una forma única de pronunciar un URI.|

Cuando se importa el texto, este se normaliza para que lo pueda procesar el sistema. Sin embargo, hay algunas normalizaciones importantes que debe hacer el usuario _antes_ para cargar los datos. Consulte las [Transcription guidelines](cognitive-services-custom-speech-transcription-guidelines.md) (Directrices de transcripción) para determinar el lenguaje apropiado al preparar sus datos.

## <a name="import-the-language-data-set"></a>Importación del conjunto de datos de lenguaje

Haga clic en el botón "Import" (Importar) en la fila "Acoustic Datasets" (Conjunto de datos acústico) y el sitio muestra una página para cargar un nuevo conjunto de datos.

Cuando esté listo para importar el conjunto de datos de lenguaje, inicie sesión en el [portal de Custom Speech Service](https://cris.ai).  A continuación, haga clic en el menú desplegable "Custom Speech" (Voz personalizada) en la cinta de opciones superior y seleccione "Adaptation Data" (Datos de adaptación). Si es la primera vez que carga datos a Custom Speech Service, verá una tabla vacía denominada "Datasets" (Conjunto de datos).

Para importar un conjunto de datos nuevo, haga clic en el botón "Import" (Importar) en la fila "Language Datasets" (Conjunto de datos de lenguaje) y el sitio muestra una página para cargar un nuevo conjunto de datos. Escriba un nombre y una descripción que le ayudarán a identificar el conjunto de datos en el futuro. A continuación, utilice el botón "Choose File" (Elegir archivo) para buscar el archivo de texto de datos de lenguaje. A continuación, haga clic en "Import" (Importar) y se cargará el conjunto de datos. Dependiendo del tamaño del conjunto de datos, esto puede tardar varios minutos.

![probar](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-import.png)

Cuando finalice la importación, volverá a la tabla de datos de lenguaje y verá una entrada que se corresponde con su conjunto de datos de lenguaje. Tenga en cuenta que se le ha asignado un identificador único (GUID). Los datos también tendrán un estado que refleja su estado actual. Su estado será "Waiting" (En espera) mientras está en cola para su procesamiento, "Processing" (Procesando) mientras está en proceso de validación y "Complete" (Completo) cuando los datos estén listos para su uso. La validación de datos realiza una serie de comprobaciones sobre el texto del archivo y una cierta normalización del texto de los datos.

Cuando el estado es "Complete" (Completo), puede hacer clic en "View Report" (Ver informe) para ver el informe de verificación de datos de lenguaje. Se muestra el número de expresiones cuya verificación es correcta y las que han producido un error, junto con detalles sobre las expresiones erróneas. En el ejemplo siguiente, dos ejemplos produjeron errores en la verificación debido a caracteres incorrectos (en este conjunto de datos, el primero tenía dos emoticonos y el segundo tenía varios caracteres fuera del juego de caracteres imprimibles ASCII).

![probar](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-report.png)

Cuando el estado del conjunto de datos de lenguaje es "Complete" (Completo), se puede utilizar para crear un modelo de lenguaje personalizado.

![probar](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Crear un modelo de lenguaje personalizado

Una vez que los datos de lenguaje están listos, haga clic en "Language Models" (Modelos de lenguaje) en el menú desplegable "Menú" para iniciar el proceso de creación del modelo de lenguaje personalizado. Esta página contiene una tabla denominada "Language Models" (Modelos de lenguaje) con los modelos de lenguaje personalizados actuales. Si aún no ha creado ningún modelo de lenguaje personalizado, la tabla estará vacía. La configuración regional actual se refleja en el título de la tabla. Si desea crear un modelo de lenguaje para un lenguaje diferente, haga clic en "Change Locale" (Cambiar configuración regional). Se puede encontrar más información sobre los lenguajes admitidos en la sección [Cambio de la configuración regional](cognitive-services-custom-speech-change-locale.md). Para crear un nuevo modelo, haga clic en el vínculo "Create New" (Crear nuevo) debajo del título de la tabla.

En la página "Create Language Model" (Crear el modelo de lenguaje), escriba un nombre y una descripción para ayudarle a realizar un seguimiento de la información relativa a este modelo, como el conjunto de datos utilizado. A continuación, seleccione el "Base Language Model" (Modelo de lenguaje base) en el menú desplegable. Este modelo será el punto de partida para la personalización. Existen dos modelos de idioma base para elegir. _Microsoft Search and Dictation LM_ es adecuado para la voz dirigida a una aplicación, como comandos, consultas de búsqueda o dictado. _Microsoft conversacional LM_ es adecuado para el reconocimiento de la voz hablada en un estilo conversacional. Este tipo de habla normalmente se dirige a otra persona y se produce en centros de llamadas o en reuniones.

Después de especificar el modelo de lenguaje base, seleccione el conjunto de datos de lenguaje que desea utilizar para la personalización mediante el menú desplegable "Language Data" (Datos de lenguaje).

![probar](../../../media/cognitive-services/custom-speech-service/custom-speech-language-models-create2.png)

Al igual que con la creación de modelos acústicos, puede optar por realizar pruebas sin conexión de su nuevo modelo cuando finalice el procesamiento. Debido a que se trata de una evaluación del rendimiento de voz a texto, las pruebas sin conexión requieren un conjunto de datos acústicos.

Para realizar pruebas sin conexión de su modelo de lenguaje, active la casilla situada junto a "Offline Testing" (Pruebas sin conexión). A continuación, seleccione un modelo acústico en el menú desplegable. Si no ha creado ningún modelo acústico personalizado, los modelos acústicos base de Microsoft serán el único modelo del menú. En caso de que haya escogido un modelo base de LM conversacional, necesita usar un AM conversacional aquí. En caso de que utilice un modelo de búsqueda y dictado de LM, tendrá que seleccionar un modelo de búsqueda y dictado de AM.

Por último, seleccione el conjunto de datos acústico que le gustaría usar para realizar la evaluación.

Cuando esté listo para comenzar el procesamiento, haga clic en "Create" (Crear). Esto le devolverá a la tabla de modelos de lenguaje. Habrá una nueva entrada en la tabla correspondiente a este modelo. El estado refleja el estado del modelo y pasará por varios estados incluyendo "Waiting" (En espera), "Processing" (Procesando) y "Complete" (Completo).

Cuando el modelo ha alcanzado el estado "Complete" (Completo), se puede implementar en un punto de conexión. Al hacer clic en "View Result" (Ver resultado), se muestran los resultados de las pruebas sin conexión, si realizan.

Si desea cambiar el nombre o la descripción del modelo en algún momento, puede utilizar el vínculo "Edit" (Editar) en la fila correspondiente de la tabla de modelos de lenguaje.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha desarrollado un modelo de lenguaje personalizado para su uso con texto. Para crear un modelo acústico personalizado para su uso con archivos de audio y transcripciones, continúe con el tutorial sobre la creación de un modelo acústico.

> [!div class="nextstepaction"]
> [Crear un modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
