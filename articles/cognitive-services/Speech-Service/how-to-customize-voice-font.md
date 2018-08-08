---
title: ¿Qué es la voz personalizada? Azure Cognitive Services
description: En este artículo se ofrece una visión general de la personalización de voz de Microsoft Text to Speech, que le permite crear una voz de marca reconocible y única.
services: cognitive-services
author: noellelacharite
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
ms.openlocfilehash: 84493ae83515c0458bf5b9e9cf44603300a8b4f7
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284894"
---
# <a name="creating-custom-voice-fonts"></a>Crear fuentes de voz personalizada

La personalización de voz de Microsoft Text to Speech (TTS) le permite crear una voz única y reconocible para su marca: una *fuente de voz.* 

Para crear su fuente de voz, haga que un estudio grabe y cargue los scripts asociados como datos de aprendizaje. A continuación, el servicio crea un modelo de voz único ajustado a la grabación. Asimismo, puede usar esta fuente de voz para sintetizar la voz. 

Puede comenzar con una pequeña cantidad de datos para realizar una prueba de concepto. De todos modos, cuantos más datos proporcione, más natural y profesional será la voz.

La personalización de la voz está disponible para inglés de Estados Unidos (en-US) y chino continental (zh-CN).

## <a name="prerequisites"></a>Requisitos previos

La característica de personalización de voz Text to Speech se encuentra actualmente en su versión preliminar privada. [Complete el formulario de solicitud](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u) para poder obtener acceso.

También necesita una cuenta de Azure y una suscripción a Speech Service. [Cree una suscripción](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) si todavía no lo ha hecho. Conecte su suscripción con el portal de Voz personalizada, tal como se indica a continuación.

1. Inicie sesión en el [portal de Voz personalizada](https://customvoice.ai) con la misma cuenta de Microsoft que usa para obtener acceso.

2. Vaya a "Suscripciones", bajo su nombre de cuenta en la esquina superior derecha.

    ![Suscripciones](media/custom-voice/subscriptions.png)

3. En la página "Suscripciones", elija "Conectar suscripción existente".

     ![Conectar la suscripción existente](media/custom-voice/connect-existing-sub.png)

4. Pegue su clave de suscripción en la tabla, tal como se muestra a continuación. Cada suscripción tiene dos claves y puede usar cualquiera de ellas.

     ![Agregar suscripción](media/custom-voice/add-subscription.png)

Ya tiene todo listo.

## <a name="prepare-recordings-and-transcripts"></a>Preparar las grabaciones y transcripciones

Un conjunto de datos de aprendizaje de voz consiste en un conjunto de archivos de audio, junto con un archivo de texto que contiene las transcripciones de todos estos archivos de audio.

Puede preparar esos archivos en cualquier dirección: puede escribir un script y hacer que lo lea un locutor, o bien use el audio disponible públicamente y transcríbalo a texto. En este último caso, deberá editar las disfluencias de los archivos de audio, como las muletillas ("um") y otros sonidos de relleno, tartamudeos, palabras entre dientes o pronunciaciones erróneas.

Para crear una buena fuente de voz, es importante que las grabaciones se realicen en una sala silenciosa con un micrófono de alta calidad. El volumen constante, la velocidad de la conversación, el tono al hablar y las particularidades expresivas del habla son esenciales para compilar una gran voz digital. Para crear una voz que se vaya a usar en una producción, le recomendamos que use un estudio de grabación profesional y un locutor. Para más información, consulte [Cómo grabar ejemplos de voz para una voz personalizada](record-custom-voice-samples.md).

### <a name="audio-files"></a>Archivos de audio

Cada archivo de audio debe consistir en una sola expresión (por ejemplo, una sola frase o un solo giro de un sistema de diálogo). Todos los archivos deben estar en el mismo idioma (las voces personalizadas en varios lenguajes no son compatibles). Los archivos de audio también deben tener un nombre de archivo numérico exclusivo con la extensión de nombre de archivo `.wav`.

Los archivos de audio deben prepararse de la siguiente manera. Otros formatos no son compatibles y serán rechazados.

| **Propiedad** | **Valor** |
| ------------ | --------- |
| Formato de archivo  | RIFF (WAV)|
| Frecuencia de muestreo| al menos 16 000 Hz |
| Formato de ejemplo| PCM, 16 bits |
| Nombre de archivo    | Numérico, con la extensión `.wav` |
| Formato de archivo| Zip      |
| Tamaño de archivo máximo|200 MB|

Coloque el conjunto de archivos de audio en una única carpeta sin subdirectorios y haga un paquete con el conjunto completo como un solo archivo ZIP.

> [!NOTE]
> Se rechazarán los archivos de onda con una frecuencia de muestreo inferior a 16 000 Hz. En los casos donde un archivo ZIP contenga ondas con distintas frecuencias de muestreo, solo se importarán las que sean iguales o superiores a 16 000 Hz.
> Actualmente el portal importa archivos ZIP de hasta 200 MB. Sin embargo, pueden cargarse varios archivos. La cantidad máxima de conjuntos de datos permitida es de 10 archivos ZIP para usuarios con una suscripción gratuita, y de 50 para usuarios con suscripciones estándar.

### <a name="transcripts"></a>Transcripciones

El archivo de transcripción es un archivo de texto sin formato (ANSI/UTF-8/UTF-8-BOM/UTF-16-LE/UTF-16-BE). Cada línea del archivo de transcripción debe tener el nombre de un archivo de audio, seguido de un carácter de tabulación (elemento de código 9) y, finalmente, su transcripción. No se permiten líneas en blanco.

Por ejemplo: 

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

El sistema de voz personalizado normaliza las transcripciones convirtiendo el texto a minúsculas y eliminando los signos de puntuación extraños. Es importante que las transcripciones sean 100 % precisas para las grabaciones de audio correspondientes.

> [!TIP]
> Al compilar voces de producción de texto a voz, seleccione expresiones (o scripts de escritura) teniendo en cuenta tanto la cobertura fonética como la eficiencia.

## <a name="upload-your-datasets"></a>Cargar los conjuntos de datos

Después de preparar el archivo de audio y las transcripciones, cárguelos a través del [portal del servicio de voz personalizada](https://customvoice.ai).

> [!NOTE]
> Los conjuntos de datos no se pueden editar después de que se hayan cargado. Si, por ejemplo, olvidó incluir las transcripciones de algunos de los archivos de audio, o si elige accidentalmente el género equivocado, debe cargar todo el conjunto de datos de nuevo. Compruebe el conjunto de datos y la configuración antes de comenzar la carga.

1. Inicie sesión en el portal.

2. Seleccione **Datos** en la página principal de la voz personalizada. 

    ![Mis proyectos](media/custom-voice/my-projects.png)

    Aparecerá la tabla Mis datos de voz. Comprobará que está vacía si aún no ha cargado ningún conjunto de datos de voz.

3. Haga clic en **Importar datos** para abrir la página y cargar un nuevo conjunto de datos.

    ![Importar datos de voz](media/custom-voice/import-voice-data.png)

4. Escriba un nombre y una descripción en los campos proporcionados. 

5. Seleccione la configuración regional de sus fuentes de voz. Asegúrese de que la información de la configuración regional coincida con el idioma de los datos de grabación y los scripts. 

6. Seleccione el género del hablante cuya voz está utilizando.

7. Elija el script y los archivos de audio que quiera cargar. 

8. Haga clic en **Importar** para cargar sus datos. Si tiene que cargar conjuntos de datos más grandes, la importación puede tardar varios minutos.

> [!NOTE]
> Los usuarios que tengan una suscripción gratuita pueden cargar dos conjuntos de datos a la vez. En cambio, los usuarios con una suscripción estándar pueden subir cinco conjuntos de datos simultáneamente. Si alcanza el límite, espere hasta que al menos uno de los conjuntos de datos finalice la importación, e inténtelo de nuevo.

Cuando se complete la carga, la tabla Mis datos de voz aparecerá de nuevo. Asimismo, verá una entrada que corresponde al conjunto de datos recién cargado. 

Los conjuntos de datos se validan automáticamente después de la carga. La validación de datos incluye una serie de comprobaciones en los archivos de audio para comprobar su formato de archivo, el tamaño y la frecuencia de muestreo. Las comprobaciones en los archivos de transcripción revisan el formato del archivo y realizan cierta normalización del texto. Las expresiones se transcriben mediante el reconocimiento de voz, y el texto resultante se compara con la transcripción que proporcionó.

![Mis datos de voz](media/custom-voice/my-voice-data.png)

En la siguiente tabla se muestran los estados de procesamiento de los conjuntos de datos importados. 

| Estado | Significado
| ----- | -------
| `NotStarted` | El conjunto de datos ha sido recibido y está en la cola para su procesamiento.
| `Running` | El conjunto de datos está siendo validado.
| `Succeeded` | El conjunto de datos ha sido validado y ahora se puede usar para compilar una fuente de voz.

Una vez completada la validación, puede ver el número total de expresiones coincidentes para cada uno de sus conjuntos de datos en la columna Expresiones.

Puede descargar un informe para comprobar la puntuación de las pronunciaciones y el nivel de ruido de cada una de las grabaciones. La puntuación de las pronunciaciones oscila de 0 a 100; una puntuación por debajo de 70 normalmente indica un error en el discurso o que el script no coincide. Un acento marcado puede reducir la puntuación de las pronunciaciones, y afectar a la voz digital que se ha creado.

Una relación de la señal y el ruido (SNR) más alta indica un ruido más bajo en el audio. Por lo general, el audio puede alcanzar una SNR de más de 50 puntos si se graba en estudios profesionales. Un audio que tenga un valor de SNR por debajo de 20 puntos puede provocar un ruido obvio en la voz generada.

Puede volver a grabar cualquier expresión que tenga una puntuación baja debido a la pronunciación o a la pobre relación entre el ruido y la señal. Si no es posible volver a grabar, puede excluir esas expresiones de su conjunto de datos.

## <a name="build-your-voice-font"></a>Compilar la fuente de voz

Una vez que el conjunto de datos haya sido validado, puede usarlo para compilar su fuente de voz personalizada. 

1. Elija **Modelos** en el menú desplegable "Voz personalizada". 
 
    Aparecerá la tabla Mis fuentes de voz, que enumera las fuentes de voz personalizadas que ya ha creado.

1. Haga clic en **Crear voces** bajo el título de la tabla. 

    Una vez hecho esto, aparecerá la página para crear una fuente de voz. La configuración regional actual se mostrará en la primera fila de la tabla. Cambie la configuración regional para crear una voz en otro idioma. La configuración regional debe ser la misma que la de los conjuntos de datos que se utilizan para crear la voz.

1. Tal como hizo cuando cargó el conjunto de datos, escriba un nombre y una descripción que le permitan identificar este modelo. 

    El nombre que escriba aquí será el nombre que use para especificar la voz en su solicitud de síntesis de voz como parte de la entrada de SSML, por lo tanto, elija cuidadosamente. Solo se permiten letras, números y algunos signos de puntuación como "-", "_" "(', ')".

    Un uso común del campo Descripción es registrar los nombres de los conjuntos de datos que se usaron para crear el modelo.

1. Elija el género de su fuente de voz. Debe coincidir con el género del conjunto de datos.

1. Seleccione los conjuntos de datos que desea usar para el aprendizaje de la fuente de voz. Todos los conjuntos de datos utilizados deben ser del mismo hablante.

1. Haga clic en **Crear** para comenzar a crear su fuente de voz.

    ![Crear modelo](media/custom-voice/create-model.png)

El nuevo modelo aparece en la tabla Mis fuentes de voz. 

![Mis fuentes de voz](media/custom-voice/my-voice-fonts.png)

El estado que se muestra refleja el proceso de conversión del conjunto de datos a una fuente de voz, tal como se muestra aquí.

| Estado | Significado
| ----- | -------
| `NotStarted` | La solicitud para crear la fuente de voz está en cola para su procesamiento.
| `Running` | La fuente de voz está siendo creada.
| `Succeeded` | La fuente de voz ha sido creada y ya puede ser implementada.

El tiempo de aprendizaje varía según el volumen de datos de audio procesados. El intervalo de tiempo típicos varía, aproximadamente, desde los 30 minutos para unos cientos de expresiones, hasta 40 horas para 20.000 expresiones.

> [!NOTE]
> Los usuarios que tengan una suscripción gratuita pueden entrenar una fuente de voz a la vez. En cambio, los usuarios que tengan una suscripción estándar pueden entrenar tres voces simultáneamente. Si alcanza el límite, espere hasta que al menos uno de las fuentes de voz finalice el aprendizaje, e inténtelo de nuevo.

## <a name="test-your-voice-font"></a>Pruebe la fuente de voz

Una vez que la fuente de voz se haya creado correctamente, puede probarla antes de implementarla para su uso. Haga clic en **Probar** en la columna Operaciones. La página de prueba aparece para la fuente de voz seleccionada. Tenga en cuenta que la tabla estará vacía si aún no ha enviado ninguna solicitud de prueba para la voz.

![Mis fuentes de voz (2ª parte)](media/custom-voice/my-voice-fonts2.png)

Haga clic en el botón **Probar con texto** que se encuentra debajo del título de la tabla, para mostrar un menú emergente que le permitirá enviar solicitudes de texto. Puede enviar su solicitud de prueba en texto sin formato o SSML. El tamaño máximo de entrada es de 1.024 caracteres, incluidas todas las etiquetas de la solicitud SSML. Recuerde que el idioma del texto debe ser el mismo que el de la fuente de voz.

![Probar la fuente de voz](media/custom-voice/voice-font-testing.png)

Después de completar el cuadro de texto y confirmar el modo de entrada, haga clic en **Sí** para enviar su solicitud de prueba y regresar a la página de prueba. La tabla ahora incluye una entrada que corresponde a su nueva solicitud y la columna de estado que es ahora familiar. Es posible que se tarden unos minutos en sintetizar la voz. Cuando la columna de estado muestra el valor "Correcto", puede descargar la entrada de texto (un archivo `.txt`) y la salida de audio (un archivo `.wav`) y escuchar esta última para comprobar la calidad.

![Probar la fuente de voz (2ª parte)](media/custom-voice/voice-font-testing2.png)

## <a name="create-and-use-a-custom-endpoint"></a>Crear y usar un punto de conexión personalizado

Una vez que haya creado y probado con éxito su modelo de voz, impleméntelo en un punto de conexión personalizado de Text to Speech. A continuación, use ese punto de conexión en lugar del punto de conexión habitual al realizar solicitudes de Text to Speech a través de la API REST. Recuerde que solo se puede llamar al punto de conexión personalizado mediante la suscripción que utilizó para implementar la fuente.

Para crear un punto de conexión, elija **Puntos de conexión** en el menú de la voz personalizada, que se encuentra en la parte superior de la página. Aparecerá la página Mis voces implementadas, con su tabla de puntos de conexión de voz personalizados, si corresponde. La configuración regional actual se refleja en la primera fila de la tabla. Para crear una implementación en un idioma diferente, cambie la configuración regional que se muestra. (Debe coincidir con la voz que está implementando).

Haga clic en el botón **Implementar voces** para crear un nuevo punto de conexión. Escriba el nombre y la descripción del punto de conexión personalizado.

En el menú Suscripción, elija la suscripción que quiera usar. Los usuarios que tengan una suscripción gratuita solo pueden tener un modelo implementado a la vez. En cambio, los usuarios que tengan una suscripción estándar pueden crear hasta 20 puntos de conexión, cada uno de ellos con su propia voz personalizada.

![Crear punto de conexión](media/custom-voice/create-endpoint.png)

Después de seleccionar el modelo que se implementará, haga clic en **Crear**- La página Mis voces implementadas vuelve a aparecer, ahora con una entrada para el punto de conexión nuevo. El proceso para crear instancias del nuevo punto de conexión puede llevar unos minutos. Cuando el estado de la implementación muestra el valor Completado, quiere decir que el punto de conexión está listo para su uso.

![Mi voces implementadas](media/custom-voice/my-deployed-voices.png)

Cuando el estado de la implementación muestra el valor Correcto, quiere decir que el punto de conexión de la fuente de voz implementada aparece en la tabla Mis voces implementadas. Puede usar este URI directamente en una solicitud HTTP.

Las pruebas en línea del punto de conexión también están disponibles a través del portal de voz personalizada. Para probar el punto de conexión, seleccione**Pruebas de puntos de conexión** en el menú desplegable de Voz personalizada. Aparecerá la página para probar los puntos de conexión. Elija una voz personalizada implementada y escriba el texto que se dirá (ya sea en texto sin formato o en formato SSML) en el cuadro de texto.

> [!NOTE] 
> Al usar SSML, la etiqueta `<voice>` debe especificar el nombre que asignó a su voz personalizada cuando la creó.

Haga clic en **Reproducir** para escuchar el texto que se habla en su fuente de voz personalizada.

![Pruebas de puntos de conexión](media/custom-voice/endpoint-testing.png)

El punto de conexión personalizado es técnicamente idéntico al punto de conexión estándar que se usa en las solicitudes de texto a voz. Consulte la [API REST](rest-apis.md) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconocer la voz en C#](quickstart-csharp-dotnet-windows.md)
