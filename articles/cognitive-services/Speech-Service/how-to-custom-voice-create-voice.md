---
title: Creación de una voz personalizada - servicios de voz
titlesuffix: Azure Cognitive Services
description: Cuando esté listo para cargar los datos, vaya al portal de voz personalizado. Cree o seleccione un proyecto de voz personalizado. El proyecto debe compartir el idioma o configuración regional adecuada y las propiedades de género como los datos pretende usar para el entrenamiento de voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: fad69c4108d747c44eccf37b81adf2c7c615cb58
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65156851"
---
# <a name="create-a-custom-voice"></a>Creación de una voz personalizada

En [preparar los datos de voz personalizados](how-to-custom-voice-prepare-data.md), se describen los tipos de datos diferentes que puede usar para entrenar una voz personalizados y los requisitos de formato diferente. Una vez que haya preparado los datos, puede empezar a cargarlos en el [portal personalizado voz](http://aka.ms/custom-voice-portal), o a través de la API de entrenamiento de voz personalizado. Aquí se describen los pasos del entrenamiento de una voz personalizada a través del portal.

> [!NOTE]
> Esta página se supone que ha leído [empezar a trabajar con voz personalizada](how-to-custom-voice.md) y [preparar los datos de voz personalizado](how-to-custom-voice-prepare-data.md)y ha creado un proyecto de voz personalizado.

Comprobar los idiomas admitidos de voz personalizados: [lenguaje para la personalización](language-support.md#customization).

## <a name="upload-your-datasets"></a>Cargar los conjuntos de datos

Cuando esté listo para cargar los datos, vaya a la [portal personalizado voz](http://aka.ms/custom-voice-portal). Cree o seleccione un proyecto de voz personalizado. El proyecto debe compartir el idioma o configuración regional adecuada y las propiedades de género como los datos pretende usar para el entrenamiento de voz. Por ejemplo, seleccione `en-GB` si las grabaciones de audio tiene se realiza en inglés con un énfasis de Reino Unido.

Vaya a la **datos** ficha y haga clic en **cargar datos**. En el asistente, seleccione el tipo de datos correcto que coincida con lo que haya preparado.

Cada conjunto de datos que cargue debe cumplir los requisitos para el tipo de datos que elija. Es importante dar formato correctamente a los datos antes de cargarse. Esto garantiza que el servicio de voz personalizado se procesará con precisión los datos. Vaya a [preparar los datos de voz personalizado](how-to-custom-voice-prepare-data.md) y asegúrese de que los datos se ha formateado con razón.

> [!NOTE]
> Los usuarios de suscripción gratuita (F0) pueden cargar dos conjuntos de datos al mismo tiempo. Los usuarios de la suscripción estándar (S0) pueden cargar simultáneamente cinco conjuntos de datos. Si alcanza el límite, espere hasta que al menos uno de los conjuntos de datos finalice la importación. A continuación, inténtelo de nuevo.

> [!NOTE]
> El número máximo de conjuntos de datos permitida la importación por suscripción es 10 .zip gratuitamente archivos de usuarios de la suscripción (F0) y 500 para usuarios de la suscripción estándar (S0).

Los conjuntos de datos se validan automáticamente una vez que presione el botón cargar. Validación de datos incluye la serie de comprobaciones en los archivos de audio para comprobar su formato de archivo, el tamaño y velocidad de muestreo. Corrija los errores si existe y vuelva a enviarlo. Cuando se inició correctamente la solicitud de importación de datos, debería ver una entrada en la tabla de datos que se corresponde con el conjunto de datos que acaba de cargar.

En la siguiente tabla se muestran los estados de procesamiento de los conjuntos de datos importados:

| Estado | Significado |
| ----- | ------- |
| Processing | El conjunto de datos se ha recibido y se está procesando. |
| Succeeded | El conjunto de datos se ha validado y ahora se puede usar para crear un modelo de voz. |
| Con error | El conjunto de datos ha error durante el procesamiento debido a muchas razones, por ejemplo, errores de archivo, problemas de datos o problemas de red. |

Una vez completada la validación, puede ver el número total de grabaciones de voz coincidentes para cada uno de los conjuntos de datos en el **grabaciones de voz** columna. Si el tipo de datos que ha seleccionado requiere prolongada audio segmentación, esta columna sólo refleja las grabaciones de voz que se ha segmentado por usted basándose en sus expedientes o a través del servicio de transcripción de voz. Aún más, puede descargar el conjunto de datos valida para ver el detalle de los resultados de las declaraciones que se importó correctamente y sus expedientes de asignación. Sugerencia: long audio segmentación puede tardar más de una hora para completar el procesamiento de datos.

Para conjuntos de datos en-US y zh-CN, puede descargar un informe para comprobar las puntuaciones de pronunciación y el nivel de ruido para cada uno de sus grabaciones aún más. La puntuación de las pronunciaciones abarca un rango del 0 al 100. Una puntuación por debajo de 70 normalmente indica un error en el discurso o que el guion no coincide. Un acento marcado puede reducir la puntuación de las pronunciaciones, y afectar a la voz digital que se ha creado.

Una relación de la señal y el ruido (SNR) más alta indica un ruido más bajo en el audio. Por lo general, el audio puede alcanzar una SNR de más de 50 puntos si se graba en estudios profesionales. Un audio que tenga un valor de SNR por debajo de 20 puntos puede provocar un ruido obvio en la voz generada.

Puede volver a grabar cualquier expresión que tenga una puntuación baja debido a la pronunciación o a la pobre relación entre el ruido y la señal. Si no es posible volver a realizar la grabación, puede excluir esas expresiones de su conjunto de datos.

## <a name="build-your-custom-voice-model"></a>Generar el modelo de voz personalizados

Una vez se haya validado el conjunto de datos, puede usarlo para generar el modelo de voz personalizados.

1.  Vaya a **texto a voz > voz personalizados > aprendizaje**.

2.  Haga clic en **entrenar modelo**.

3.  A continuación, escriba un **nombre** y **descripción** para ayudar a identificar este modelo.

    Elija un nombre con cuidado. El nombre que escriba aquí será el nombre que use para especificar la voz en su solicitud de síntesis de voz como parte de la entrada de SSML. Solo letras, números y algunos caracteres de puntuación, como - \_y (',') se permiten. Use nombres diferentes para los modelos de voz diferentes.

    Un uso habitual del campo **Descripción** es registrar los nombres de los conjuntos de datos que se usaron para crear el modelo.

4.  Desde el **seleccionar datos de entrenamiento** página, elija uno o varios conjuntos de datos que le gustaría usar para el entrenamiento. Compruebe el número de grabaciones de voz antes de que los envíe. Puede iniciar con cualquier número de grabaciones de voz para los modelos de voz en-US y zh-CN. Para otras configuraciones regionales, debe seleccionar más de 2.000 grabaciones de voz podrá entrenar una voz.

    > [!NOTE]
    > Los nombres duplicados de audio se quitará el entrenamiento. Asegúrese de que los conjuntos de datos que seleccione no contienen los mismos nombres de audio en varios archivos zip.

    > [!TIP]
    > Uso de los conjuntos de datos desde el altavoz del mismo es necesario para resultados de calidad. Cuando los conjuntos de datos que ha enviado para el entrenamiento contienen un número total de menos de 6.000 declaraciones distintos, a entrenar el modelo de voz a través de la técnica estadística síntesis paramétricos. En el caso de que los datos de entrenamiento exceden un número total de 6.000 declaraciones diferentes, se iniciará un proceso de entrenamiento con la técnica de síntesis de concatenación. Normalmente la tecnología de concatenación puede producir resultados más natural y aumenta la fidelidad de voz. [Póngase en contacto con el equipo de voz personalizado](mailto:speechsupport@microsoft.com) si desea entrenar un modelo con la tecnología más reciente de TTS neuronal que puede producir una voz digital equivalente a disponibles públicamente [voces neuronales](language-support.md#neural-voices).

5.  Haga clic en **Train** para empezar a crear el modelo de voz.

La tabla de entrenamiento, se muestra una nueva entrada que corresponde a esta recién creada el modelo. La tabla también muestra el estado: Procesamiento, se ha realizado correctamente, con errores.

El estado que se muestra refleja el proceso de convertir el conjunto de datos a un modelo de voz, como se muestra aquí.

| Estado | Significado |
| ----- | ------- |
| Processing | Se está creando el modelo de voz. |
| Succeeded | El modelo de voz se ha creado y se puede implementar. |
| Con error | El modelo de voz ha error en aprendizaje debido a muchas razones, problemas de red o problemas de datos no vistos por ejemplo. |

El tiempo de aprendizaje varía según el volumen de datos de audio procesados. El intervalo de tiempo típicos varía, aproximadamente, desde los 30 minutos para unos cientos de expresiones, hasta 40 horas para 20.000 expresiones. Cuando se complete correctamente el entrenamiento del modelo, puede empezar a probarla.

> [!NOTE]
> Los usuarios de suscripción gratuita (F0) pueden entrenar una fuente de voz al mismo tiempo. Los usuarios de la suscripción estándar (S0) pueden entrenar tres voces simultáneamente. Si alcanza el límite, espere hasta que al menos una de las fuentes de voz finalice el aprendizaje e inténtelo de nuevo.

> [!NOTE]
> El número máximo de modelos de voz pueda estar formado por suscripción es 10 modelos para los usuarios de suscripción gratuita (F0) y 100 para los usuarios de la suscripción estándar (S0).

## <a name="test-your-voice-model"></a>Probar el modelo de voz

Una vez que la fuente de voz se haya creado correctamente, podrá probarla antes de implementarla para su uso.

1.  Vaya a **texto a voz > voz personalizados > pruebas**.

2.  Haga clic en **Agregar prueba**.

3.  Seleccione uno o varios modelos que le gustaría probar.

4.  Proporcione el texto que desea la voice(s) hablar. Si ha seleccionado para probar varios modelos al mismo tiempo, se usará el mismo texto para las pruebas para los distintos modelos.

    > [!NOTE]
    > Recuerde que el idioma del texto debe ser el mismo que el de la fuente de voz. Se pueden probar solo los modelos entrenados correctamente. En este paso, se admite texto sin formato.

5.  Haga clic en **Create**(Crear).

Una vez que ha enviado la solicitud de prueba, volverá a la página de prueba. La tabla ahora incluye una entrada que corresponde a su nueva solicitud y la columna de estado. Es posible que se tarden unos minutos en sintetizar la voz. Cuando la columna Estado indica **Succeeded**, puede reproducir el audio, o descargar la entrada de texto (un archivo .txt) y (un archivo .wav) de salida de audio y aún más escuchar lo último para la calidad.

También puede encontrar los resultados de pruebas en la página de detalles de cada modelos que ha seleccionado para las pruebas. Vaya a la **entrenamiento** ficha y haga clic en el nombre del modelo para especificar la página de detalles del modelo.

## <a name="create-and-use-a-custom-voice-endpoint"></a>Crear y usar un punto de conexión de voz personalizados

Una vez que haya creado y probado con éxito su modelo de voz, impleméntelo en un punto de conexión personalizado de Text to Speech. A continuación, use ese punto de conexión en lugar del punto de conexión habitual al realizar solicitudes de Text to Speech a través de la API de REST. El punto de conexión personalizado se puede llamar solo a la suscripción que ha usado para implementar la fuente.

Para crear un nuevo extremo de voz personalizados, vaya a **texto a voz > voz personalizados > implementación**. Seleccione **agregar punto de conexión** y escriba un **nombre** y **descripción** para el punto de conexión personalizado. A continuación, seleccione el modelo de voz personalizados que le gustaría asociar a este punto de conexión.

Después de haber activado el **agregar** botón, en la tabla de punto de conexión, verá una entrada para el nuevo punto de conexión. El proceso para crear instancias del nuevo punto de conexión puede llevar unos minutos. Cuando el estado de la implementación muestra el valor **Completado**, quiere decir que el punto de conexión está listo para su uso.

> [!NOTE]
> Los usuarios de suscripción gratuita (F0) pueden tener solo un modelo implementado. Los usuarios de la suscripción estándar (S0) pueden crear hasta 50 puntos de conexión, cada uno con su propia voz personalizados.

> [!NOTE]
> Para usar su voz personalizada, especifique el nombre del modelo de voz, utilice el URI personalizado directamente en una solicitud HTTP y usar la misma suscripción para pasar a través de la autenticación del servicio TTS.

Una vez implementado el punto de conexión, el nombre de punto de conexión aparece como un vínculo. Haga clic en el vínculo para mostrar información específica para el punto de conexión, como la clave de punto de conexión, la dirección URL del extremo y el código de ejemplo.

Las pruebas en línea del punto de conexión también están disponibles a través del portal de voz personalizada. Para probar el punto de conexión, elija **comprobar el punto de conexión** desde el **detalles del punto de conexión** página. Aparecerá la página para probar los puntos de conexión. Escriba el texto que se hablará (en texto sin formato o [formato SSML](speech-synthesis-markup.md) en el cuadro de texto. Seleccione **Reproducir** para escuchar el texto que se habla en su fuente de voz personalizada. Esta característica de pruebas se les cargan en el uso de síntesis de voz personalizado.

El punto de conexión personalizado es técnicamente idéntico al punto de conexión estándar que se usa en las solicitudes de texto a voz. Consulte la [API REST](rest-text-to-speech.md) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

* [Guía: Registre sus ejemplos de voz](record-custom-voice-samples.md)
* [Referencia de API de texto a voz](rest-text-to-speech.md)
