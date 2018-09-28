---
title: Tutorial para crear un modelo acústico con Custom Speech Service de Microsoft Cognitive Services | Microsoft Docs
description: En este tutorial, aprenderá a crear un modelo acústico con Custom Speech Service de Microsoft Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 60fea175e8dffeefeb9cb3ecaadad5d8fc7caeb9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971556"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Tutorial: Creación de un modelo acústico personalizado

En este tutorial, va a crear un modelo acústico personalizado para los datos de voz que espera que la aplicación reconozca. La creación de un modelo acústico personalizado es útil si la aplicación está diseñada para su uso en un entorno particular, como una fábrica ruidosa, o para una población de usuarios en particular.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Preparación de los datos
> * Importar el conjunto de datos acústico
> * Crear un modelo acústico personalizado

Si no tiene una cuenta de Cognitive Services, cree una [cuenta gratuita](https://cris.ai) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para asegurarse de que la cuenta de Cognitive Services está conectada a una suscripción, abra la página [Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Suscripciones de Cognitive Services).

Si no aparece ninguna suscripción, puede hacer que Cognitive Services creen una cuenta haciendo clic en el botón **Get free subscription** (Obtener suscripción gratuita). O puede conectarse a una suscripción de Custom Search Service creada en Azure Portal haciendo clic en el botón **Connect existing subscription** (Conectar a suscripción existente).

Para más información sobre cómo crear una suscripción de Custom Search Service en Azure Portal, consulte [Create a Cognitive Services APIs account in the Azure portal](../../cognitive-services-apis-create-account.md) (Creación de una cuenta de Cognitive Services APIs en Azure Portal).

## <a name="prepare-the-data"></a>Preparación de los datos

Para personalizar el modelo acústico para un dominio en particular, se requiere una colección de datos de voz. Esta colección se compone de un conjunto de archivos de audio de los datos de voz y un archivo de texto de transcripciones de cada archivo de audio. Los datos de audio deben ser representativos del escenario en el que quiere usar el reconocedor.

Por ejemplo:

*   Si quiere reconocer mejor la voz en el entorno de una fábrica con ruido, los archivos de audio deben consistir en personas que hablan en una fábrica con ruido.
<a name="Preparing data to customize the acoustic model"></a>
*   Si está interesado en optimizar el rendimiento de un solo hablante, por ejemplo, quiere transcribir todas las Charlas junto al fuego de FDR, los archivos de audio deben consistir en muchos ejemplos de ese único hablante.

Un conjunto de datos acústicos para personalizar el modelo acústico consta de dos partes: (1) un conjunto de archivos de audio que contienen los datos de voz y (2) un archivo que contiene las transcripciones de todos los archivos de audio.

### <a name="audio-data-recommendations"></a>Recomendaciones de datos de audio

*   Todos los archivos de audio del conjunto de datos deben almacenarse en el formato de audio WAV (RIFF).
*   El audio debe tener una frecuencia de muestreo de 8 kHz o 16 kHz y los valores de muestreo deben almacenarse como enteros (shorts) con firma de 16 bits PCM sin comprimir.
*   Solo se admiten archivos de audio de un solo canal (mono).
*   Los archivos de audio deben tener una duración de entre 100 ms y 1 minuto. Lo ideal es que cada archivo de audio empiece y termine con al menos 100 ms de silencio, y lo normal es que transcurra entre 500 ms y 1 segundo.
*   Si tiene ruido de fondo en los datos, se recomienda tener también algunos ejemplos con segmentos de silencio más largos, por ejemplo, unos segundos, en sus datos, antes o después del contenido de voz.
*   Cada archivo de audio debe consistir en una sola expresión, por ejemplo, una sola frase para dictado, una sola consulta o un solo giro de un sistema de diálogo.
*   Cada archivo de audio del conjunto de datos debe tener un nombre de archivo único y la extensión "wav".
*   El conjunto de archivos de audio debe colocarse en una única carpeta sin subdirectorios y todo el conjunto de archivos de audio debe empaquetarse como un único archivo ZIP.

> [!NOTE]
> Las importaciones de datos a través del portal web están actualmente limitadas a 2 GB, por lo que este es el tamaño máximo de un conjunto de datos acústicos. Esto corresponde a aproximadamente 17 horas de audio grabadas a 16 kHz o 34 horas de audio grabadas a 8 kHz. En la tabla siguiente, se resumen los principales requisitos para los datos del audio.
>

| Propiedad | Valor |
|---------- |----------|
| Formato de archivo | RIFF (WAV) |
| Frecuencia de muestreo | 8000 o 16000 Hz |
| Canales | 1 (mono) |
| Formato de ejemplo | PCM, entero de 16 bits |
| Duración del archivo | 0,1 segundo < duración < 60 segundos |
| Cuello de latencia | > 0,1 segundo |
| Formato de archivo | Zip |
| Tamaño de archivo máximo | 2 GB |

### <a name="transcriptions"></a>Transcripciones

Las transcripciones para todos los archivos WAV deben incluirse en un único archivo de texto sin formato. Cada línea del archivo de transcripción debe tener el nombre de uno de los archivos de audio, seguido de la transcripción correspondiente. El nombre de archivo y la transcripción deben estar separados por un carácter de tabulación (\t).

  Por ejemplo: 
```
  speech01.wav  speech recognition is awesome

  speech02.wav  the quick brown fox jumped all over the place

  speech03.wav  the lazy dog was not amused
```

Las transcripciones serán texto normalizado para que puedan ser procesadas por el sistema. Sin embargo, hay algunas normalizaciones importantes que debe hacer el usuario _antes_ para cargar los datos a Custom Speech Service. Consulte la sección sobre [directrices de transcripción](cognitive-services-custom-speech-transcription-guidelines.md) para conocer el lenguaje apropiado al preparar sus transcripciones.

Los siguientes pasos se realizan utilizando el [portal de Custom Speech Service](https://cris.ai). 

## <a name="import-the-acoustic-data-set"></a>Importar el conjunto de datos acústico

Una vez que los archivos de audio y las transcripciones han sido preparadas, están listas para ser importadas al portal web del servicio.

Para ello, primero asegúrese de que ha iniciado sesión en el [portal de Custom Speech Service](https://cris.ai). A continuación, haga clic en el menú desplegable "Custom Speech" (Voz personalizada) en la cinta de opciones superior y seleccione "Adaptation Data" (Datos de adaptación). Si es la primera vez que carga datos a Custom Speech Service, verá una tabla vacía denominada "Datasets" (Conjunto de datos). 

Haga clic en el botón "Import" (Importar) en la fila "Acoustic Datasets" (Conjunto de datos acústico) y el sitio muestra una página para cargar un nuevo conjunto de datos.

![probar](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-import.png)

Escriba un _nombre_ y una _descripción_ en los cuadros de texto correspondientes. Estos datos son útiles para hacer un seguimiento de los diferentes conjuntos de datos que se cargan. A continuación, haga clic en "Elegir archivo" para el "Archivo de transcripción" y en "Archivos WAV", y seleccione el archivo de transcripción de texto sin formato y el archivo zip de los archivos WAV, respectivamente. Una vez acabado esto, haga clic en "Importar" para cargar los datos. A continuación, se cargarán los datos. Para los conjuntos de datos más grandes, esto puede tardar varios minutos.

Cuando finalice la carga, volverá a la tabla "Acoustic Datasets" (Conjunto de datos acústico) y verá una entrada que se corresponde al conjunto de datos acústicos. Tenga en cuenta que se le ha asignado un identificador único (GUID). Los datos también tendrán un estado que refleja su estado actual. Su estado será "Waiting" (En espera) mientras está en cola para su procesamiento, "Processing" (Procesando) mientras está en proceso de validación y "Complete" (Completo) cuando los datos estén listos para su uso.

La validación de datos incluye una serie de comprobaciones en los archivos de audio para comprobar el formato de archivo, la longitud y la frecuencia de muestreo, y en los archivos de transcripción para comprobar el formato de archivo y realizar alguna normalización de texto.

Cuando el estado es "Completo", puede hacer clic en "Detalles" para ver el informe de comprobación de datos acústicos. Se muestra el número de expresiones cuya verificación es correcta y las que han producido un error, junto con detalles sobre las expresiones erróneas. En el ejemplo siguiente, dos archivos WAV no pudieron realizar la comprobación debido a un formato de audio incorrecto (en este conjunto de datos, uno tenía una frecuencia de muestreo incorrecta y el otro tenían el formato de archivo incorrecto).

![probar](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-report.png)

En algún momento, si desea cambiar el nombre o la descripción del conjunto de datos, puede hacer clic en el vínculo "Editar" y cambiar estas entradas. Observe que no se pueden modificar los archivos de audio o las transcripciones.

## <a name="create-a-custom-acoustic-model"></a>Crear un modelo acústicos personalizado

Cuando el estado del conjunto de datos acústico es "Complete" (Completo), se puede utilizar para crear un modelo de lenguaje personalizado. Para ello, haga clic en "Acoustic Models" (Modelos acústicos) en el menú desplegable "Custom Speech". Verá una tabla denominada "Your models" (Sus modelos) que se muestra todos los modelos acústicos personalizados. Esta tabla estará vacía si se trata de su primer uso. La configuración regional actual se refleja en el título de la tabla. Actualmente, los modelos acústicos solo pueden crearse para inglés de Estados Unidos.

Para crear un nuevo modelo, haga clic en "Create New" (Crear nuevo) debajo del título de la tabla. Al igual que antes, escriba un nombre y una descripción que le ayudarán a identificar este modelo. Por ejemplo, el campo "Description" (Descripción) puede utilizarse para registrar qué modelo de inicio y qué conjunto de datos acústicos se han utilizado para crear el modelo. A continuación, seleccione un modelo acústico base en el menú desplegable. El modelo de base es el punto inicial de la personalización. Existen dos modelos acústicos base para elegir. _Microsoft Search and Dictation AM_ es adecuado para la voz dirigida a una aplicación, como comandos, consultas de búsqueda o dictado. El modelo _Microsoft Conversational_es adecuado para el reconocimiento de la voz hablada en un estilo conversacional. Este tipo de habla normalmente se dirige a otra persona y se produce en centros de llamadas o en reuniones. Observe que la latencia de los resultados parciales en los modelos conversacionales es mayor que en los modelos de búsqueda y dictado.

A continuación, seleccione los datos acústicos que desea utilizar para realizar la personalización mediante el menú desplegable.

![probar](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-create2.png)

Opcionalmente, puede optar por realizar pruebas sin conexión de su nuevo modelo cuando finalice el procesamiento. Esto ejecutará una evaluación de voz a texto sobre un conjunto de datos acústicos especificados mediante el modelo acústico personalizado e informará de los resultados. Para realizar esta prueba, active la casilla “Accuracy Testing” (Pruebas de precisión). A continuación, seleccione un modelo de lenguaje en el menú desplegable. Si no ha creado ningún modelo de lenguaje personalizado, solo los modelos de lenguaje base estarán en la lista desplegable. Consulte la [descripción](cognitive-services-custom-speech-create-language-model.md) de los modelos de lenguaje base en la guía y seleccione el que sea más adecuado.

Por último, seleccione el conjunto de datos acústico que le gustaría usar para realizar evaluar el modelo personalizado. Si realiza pruebas de precisión, es importante seleccionar un dato acústico diferente al utilizado para la creación del modelo para obtener una idea realista del rendimiento del modelo. Tenga en cuenta también que las pruebas sin conexión se limitan a 1000 expresiones. Si el conjunto de datos acústico para las pruebas es mayor, se evaluarán solo las 1000 primeras expresiones.

Cuando esté listo para empezar a ejecutar el proceso de personalización, presione "Create" (Crear).

Ahora verá una nueva entrada en la tabla de modelos acústicos correspondiente a este nuevo modelo. El estado del proceso se refleja en la tabla. Los estados son “Waiting” (En espera), “Processing” (Procesando) y “Complete” (Completo).

![probar](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha desarrollado un modelo acústico personalizado para su uso con archivos de audio y transcripciones. Para crear un archivo de lenguaje personalizado para su uso con archivos de texto, continúe con el tutorial sobre la creación de un modelo de lenguaje personalizado.

> [!div class="nextstepaction"]
> [Crear un modelo acústico personalizado](cognitive-services-custom-speech-create-language-model.md)
