---
title: 'Tutorial: Creación de un modelo acústico con Speech Service'
titlesuffix: Azure Cognitive Services
description: Aprenda a crear un modelo acústico con el servicio Voz en Azure Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 70fc9c34599f27eb5d67b79ef823f8037ae55ba9
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215249"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Tutorial: Creación de un modelo acústico personalizado

La creación de un modelo acústico personalizado es útil si la aplicación está diseñada para su uso en un entorno en particular, como un automóvil, con dispositivos o condiciones de grabación específicos o para una población de usuarios en particular. Los ejemplos incluyen el habla con acento, ruidos de fondo específicos o el uso de un micrófono específico para la grabación.

En este artículo, aprenderá a:
> [!div class="checklist"]
> * Preparación de los datos
> * Importar el conjunto de datos acústico
> * Crear un modelo acústico personalizado

Si no tiene una cuenta de Azure Cognitive Services, cree una [cuenta gratuita](https://azure.microsoft.com/try/cognitive-services) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para asegurarse de que la cuenta de Cognitive Services está conectada a una suscripción, abra la página [Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Suscripciones de Cognitive Services).

Puede conectarse a una suscripción del servicio Voz creada en Azure Portal si selecciona **Connect existing subscription** (Conectar a suscripción existente).

Para información sobre la creación de una suscripción al servicio Voz en Azure Portal, consulte [Prueba gratuita del servicio Voz](get-started.md).

## <a name="prepare-the-data"></a>Preparación de los datos

Para personalizar un modelo acústico para un dominio en particular, se requiere una colección de datos de voz. Esta colección puede ir desde un par de expresiones hasta varios cientos de horas de habla. La colección se compone de un conjunto de archivos de audio de datos de voz y de un archivo de texto de transcripciones de cada archivo de audio. Los datos de audio deben ser representativos del escenario en el que quiera usar el reconocedor.

Por ejemplo: 

* Si quiere reconocer mejor la voz en el entorno de una fábrica con ruido, los archivos de audio deben consistir en personas que hablan en una fábrica con ruido.
* Si está interesado en optimizar el rendimiento de un solo hablante, por ejemplo, quiere transcribir todas las Charlas junto al fuego de FDR, los archivos de audio deben constar de muchos ejemplos de ese único hablante.

Un conjunto de datos acústico para personalizar el modelo acústico consta de dos partes: (1) un conjunto de archivos de audio que contienen los datos de voz y (2) un archivo que contiene las transcripciones de todos los archivos de audio.

### <a name="audio-data-recommendations"></a>Recomendaciones de datos de audio

* Todos los archivos de audio del conjunto de datos deben almacenarse en el formato de audio WAV (RIFF).
* El audio debe tener una frecuencia de muestreo de 8 kHz o 16 kHz y los valores de muestreo deben almacenarse como enteros (shorts) con firma de modulación por impulsos codificados (PCM) de 16 bits sin comprimir.
* Solo se admiten archivos de audio de un solo canal (mono).
* Los archivos de audio pueden tener una duración entre 100 microsegundos y 1 minuto, aunque lo ideal es que oscilen entre 10 y 12 segundos. Lo ideal es que cada archivo de audio empiece y termine con al menos 100 microsegundos de silencio, y que dure entre 500 microsegundos y 1 segundo.
* Si tiene ruido de fondo en los datos, se recomienda tener también algunos ejemplos con segmentos de silencio más largos en sus datos, &mdash;por ejemplo, unos segundos&mdash;, antes o después del contenido de voz.
* Cada archivo de audio debe consistir en una sola expresión, por ejemplo, una sola frase para dictado, una sola consulta o un solo turno de un sistema de diálogo.
* Cada archivo de audio del conjunto de datos debe tener un nombre de archivo único y una extensión. wav.
* El conjunto de archivos de audio debe colocarse en una única carpeta sin subdirectorios y todo el conjunto de archivos de audio debe empaquetarse como un único archivo .zip.

> [!NOTE]
> Las importaciones de datos a través del portal web están actualmente limitadas a 2 GB, por lo que este es el tamaño máximo de un conjunto de datos acústico. Este tamaño corresponde a aproximadamente 17 horas de audio grabadas a 16 kHz o 34 horas de audio grabadas a 8 kHz. En la tabla siguiente se resumen los principales requisitos para los datos del audio:
>

| Propiedad | Valor |
|---------- |----------|
| Formato de archivo | RIFF (WAV) |
| Frecuencia de muestreo | 8000 hercios (Hz) o 16 000 Hz |
| Canales | 1 (mono) |
| Formato de ejemplo | PCM, entero de 16 bits |
| Duración del archivo | 0,1 segundo < duración < 12 segundos | 
| Cuello de latencia | > 0,1 segundo |
| Formato de archivo | .zip |
| Tamaño de archivo máximo | 2 GB |

> [!NOTE]
> Los nombres de archivo deben usar solo caracteres latinos y seguir el formato "filename.extention".

## <a name="language-support"></a>Compatibilidad con idiomas

Para obtener una lista completa de idiomas que se admiten en modelos de lenguaje personalizados de **conversión de voz a texto**, consulte [Idiomas admitidos para el servicio Voz](language-support.md#speech-to-text).

### <a name="transcriptions-for-the-audio-dataset"></a>Transcripciones para el conjunto de datos de audio

Las transcripciones para todos los archivos WAV deben incluirse en un único archivo de texto sin formato. Cada línea del archivo de transcripción debe contener el nombre de uno de los archivos de audio, seguido de la transcripción correspondiente. El nombre de archivo y la transcripción deben estar separados por un carácter de tabulación (\t).

  Por ejemplo: 
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> La transcripción debería estar codificada como una marca BOM UTF-8.

Las transcripciones son texto normalizado para que el sistema pueda procesarlas. Sin embargo, hay algunas normalizaciones importantes que debe hacer el usuario _antes_ para cargar los datos a Custom Speech Service. Para conocer el idioma que debe usar al preparar las transcripciones, consulte [Instrucciones de transcripción para usar el servicio Voz](prepare-transcription.md).

Realice los pasos de las secciones siguientes con el [portal del servicio Voz](https://cris.ai).

## <a name="import-the-acoustic-dataset"></a>Importar el conjunto de datos acústico

Después de preparar los archivos de audio y las transcripciones, están listos para importarse al portal web del servicio.

Para importarlos, primero asegúrese de que ha iniciado sesión en el [portal del servicio Voz](https://cris.ai). A continuación, en la lista desplegable **Custom Speech** de la cinta de opciones, seleccione **Adaptation Data** (Datos de adaptación). Si es la primera vez que carga datos en Custom Speech Service, verá una tabla vacía llamada **Datasets** (Conjunto de datos). 

En la fila **Acoustic Datasets** (Conjunto de datos acústico), seleccione el botón **Import** (Importar) y el sitio mostrará una página para cargar un nuevo conjunto de datos.

![La página de importación de datos acústicos](media/stt/speech-acoustic-datasets-import.png)

En los cuadros **Name** (Nombre) y **Description** (Descripción), escriba la información adecuada. Las descripciones son útiles para hacer un seguimiento de los diferentes conjuntos de datos que se cargan. 

En los cuadros **Transcriptions file (.txt)** (Archivo de transcripciones [.txt]) y **Audio files (.zip)** (Archivos de audio [.zip]), seleccione **Browse** (Examinar) y, luego, seleccione el archivo de transcripción de texto sin formato y el archivo .zip de archivos WAV. Finalizada la preparación, seleccione **Import** (Importar) para cargar los datos. Los datos se cargarán. Con conjuntos de datos grandes, el proceso de importación podría tardar varios minutos.

Finalizada la carga, vuelva a la tabla **Acoustic Datasets** (Conjuntos de datos acústicos). Se muestra una entrada que corresponde al conjunto de datos acústico. Observe que se le ha asignado un identificador único (GUID). Los datos muestran su estado actual: *NotStarted* (No iniciado) mientras está en la cola para su procesamiento, *Running* (En ejecución) mientras está en proceso de validación y *Complete* (Completo) cuando los datos están listos para usar.

La validación de datos incluye una serie de comprobaciones en los archivos de audio para comprobar el formato de archivo, la longitud y la frecuencia de muestreo, y en los archivos de transcripción para comprobar el formato de archivo y realizar alguna normalización de texto.

Cuando el estado es *Succeeded* (Correcto), puede seleccionar **Details** (Detalles) para ver el informe de comprobación de datos acústicos. Se muestra el número de expresiones cuya verificación es correcta y las que han producido un error, junto con detalles sobre las expresiones erróneas. En el ejemplo de la imagen siguiente, la comprobación de dos archivos WAV no se pudo realizar porque el formato de audio no era correcto. En este conjunto de datos, un archivo tiene una velocidad de muestreo incorrecta y el otro tiene un formato de archivo incorrecto.

![La página de detalles de adaptación de datos](media/stt/speech-acoustic-datasets-report.png)

Si quiere cambiar el nombre o la descripción del conjunto de datos, puede seleccionar el vínculo **Edit** (Editar) y cambiar sus entradas. No se puede modificar la transcripción o las entradas de archivos de audio.

## <a name="create-a-custom-acoustic-model"></a>Crear un modelo acústicos personalizado

Después de que el estado del conjunto de datos acústico es *Complete* (Completo), puede usar el conjunto de datos para crear un modelo acústico personalizado. Para ello, seleccione **Acoustic Models** (Modelos acústicos) en la lista desplegable **Custom Speech**. Una tabla llamada **Your models** (Sus modelos) muestra todos los modelos acústicos personalizados. Si es la primera vez que la usa, la tabla estará vacía. El título de la tabla muestra la configuración regional actual. Actualmente, solo puede crear modelos acústicos para inglés de Estados Unidos.

Para crear un modelo, seleccione **Create New** (Crear) bajo el título de la tabla. Al igual que antes, escriba un nombre y una descripción que le ayudarán a identificar este modelo. Por ejemplo, puede usar el campo **Description** (Descripción) para registrar qué modelo de inicio y qué conjunto de datos acústico se han usado para crear el modelo. 

A continuación, en la lista desplegable **Base Acoustic Model** (Modelo acústico base), seleccione un modelo base. Este modelo será el punto de partida para la personalización. Existen dos modelos acústicos base para elegir:
* El modelo **Microsoft Search and Dictation** es adecuado para la voz dirigida a una aplicación, como comandos, consultas de búsqueda o dictado. 
* El modelo **Microsoft Conversational** es adecuado para el reconocimiento de la voz hablada en un estilo conversacional. Este tipo de habla normalmente se dirige a otra persona y se produce en centros de llamadas o en reuniones. 

La latencia de los resultados parciales en los modelos conversacionales es mayor que en los modelos de búsqueda y dictado.

> [!NOTE]
> Actualmente estamos implementando nuestro nuevo modelo **Universal** cuyo objetivo es abordar todos los escenarios. Los modelos antes mencionados también seguirán públicamente disponibles.

A continuación, en la lista desplegable **Acoustic Data** (Datos acústicos), seleccione los datos acústicos que quiere usar para realizar la personalización.

![La página de creación de modelos acústicos](media/stt/speech-acoustic-models-create2.png)

Opcionalmente, puede optar por realizar pruebas de precisión del nuevo modelo cuando finalice el procesamiento. Esta prueba ejecuta una evaluación de voz a texto sobre un conjunto de datos acústico especificado mediante el modelo acústico personalizado y luego informa de los resultados. Para realizar esta prueba, active la casilla **Accuracy Testing** (Pruebas de precisión). A continuación, en la lista desplegable, seleccione un modelo de lenguaje. Si no ha creado ningún modelo de lenguaje personalizado, en la lista desplegable solo se muestran los modelos de lenguaje base. Para seleccionar el modelo de lenguaje más adecuado, consulte [Tutorial: Creación de un modelo de lenguaje](how-to-customize-language-model.md).

Por último, seleccione el conjunto de datos acústico que quiere usar para evaluar el modelo personalizado. Si realiza pruebas de precisión, es importante seleccionar un conjunto de datos acústico diferente al utilizado para la creación del modelo para obtener una idea realista del rendimiento del modelo. Probar la precisión de los datos de aprendizaje no le permitirá evaluar cómo funciona el modelo adaptado en condiciones reales. El resultado será demasiado optimista. Tenga en cuenta también que las pruebas de precisión se limitan a 1000 expresiones. Si el conjunto de datos acústico para las pruebas es mayor, se evaluarán solo las 1000 primeras expresiones.

Cuando esté listo para empezar a ejecutar el proceso de personalización, presione **Create** (Crear).

En la tabla de modelos acústicos se muestra una nueva entrada que corresponde a este nuevo modelo. En la tabla también se muestra el estado del proceso: *Waiting* (Espera), *Processing* (Procesamiento) o *Complete* (Completo).

![La página de modelos acústicos](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Pasos siguientes

- [Obtener la suscripción de evaluación gratuita del servicio Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconocer la voz en C#](quickstart-csharp-dotnet-windows.md)
- [Datos de ejemplo de Git](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
