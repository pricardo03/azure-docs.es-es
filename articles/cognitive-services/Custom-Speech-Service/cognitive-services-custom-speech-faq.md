---
title: 'Preguntas más frecuentes: Custom Speech Service'
titlesuffix: Azure Cognitive Services
description: Estas son las respuestas a las preguntas más habituales sobre Custom Speech Service.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: faq
ms.date: 11/21/2016
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 2de2efe3287cbb0b915434c2c9ad982fe162d741
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223735"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Preguntas más frecuentes sobre Custom Speech Service

Si no puede encontrar las respuestas en estas preguntas más frecuentes, formúlelas en la comunidad de Custom Speech Service en [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) y [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>General

**Pregunta**: ¿Cómo sabré que el proceso de mi conjunto de datos o modelo se ha completado?

**Respuesta**: Actualmente, el estado del modelo o del conjunto de datos de la tabla es la única manera de saberlo.
Cuando se complete el proceso, el estado será "Listo".
Estamos trabajando en métodos mejorados para la comunicación del estado de procesamiento, como la notificación de correo electrónico.

**Pregunta**: ¿Puedo crear más de un modelo a la vez?

**Respuesta**: No existe límite en el número de modelos que puede haber en su colección, pero solo puede crear uno cada vez en cada página.
Por ejemplo, no puede iniciar un proceso de creación de modelo de lenguaje si actualmente hay un modelo de lenguaje que se encuentra en la etapa de proceso.
Sin embargo, puede haber un modelo acústico y un modelo de lenguaje en proceso a la vez. 

**Pregunta**: Me he dado cuenta de que me he equivocado. ¿Cómo cancelo la importación de datos o la creación del modelo que está en curso? 

**Respuesta**: Actualmente no se puede revertir un proceso de adaptación acústica o de lenguaje.
Los datos importados se pueden eliminar después de que se complete la importación.

**Pregunta**: ¿Qué diferencia existe entre los modelos de búsqueda y dictado y los modelos de conversación?

**Respuesta**: Existen dos modelos de lenguaje y acústico base entre los que se puede elegir en Custom Speech Service:
consultas de búsqueda o dictado. Microsoft conversacional AM es adecuado para el reconocimiento de la voz hablada en un estilo conversacional.
Este tipo de habla normalmente se dirige a otra persona, como el que se produce en los centros de llamadas o en reuniones.

**Pregunta**: ¿Puedo actualizar el modelo existente (apilamiento del modelo)?

**Respuesta**: No se puede actualizar un modelo existente con nuevos datos.
Si tiene un nuevo conjunto de datos y desea personalizar un modelo existente, debe volver a adaptarlo con los nuevos datos y el conjunto de datos anterior que utilizó.
Los conjuntos de datos anteriores y nuevos deben combinarse en un solo .zip (si son datos acústicos) o un archivo .txt si se trata de datos de lenguaje. Una vez realizada la adaptación, el nuevo modelo actualizado debe volver a implementarse para obtener un nuevo punto de conexión.

**Pregunta**: ¿Qué ocurre si necesito una simultaneidad mayor que la predeterminada? 

**Respuesta**: El modelo se puede escalar en incrementos de 5 solicitudes simultáneas, que llamamos unidades de escalado. Cada unidad de escalado garantiza que el modelo pueda procesar 5 secuencias de audio al mismo tiempo. Puede comprar hasta 100 unidades de escalado (o 500 solicitudes simultáneas).

Póngase en contacto con nosotros si necesita más que eso.

**Pregunta**: ¿Puedo descargar mi modelo y ejecutarlo localmente?

**Respuesta**: No se permiten descargar los modelos y ejecutarlos localmente.

**Pregunta**: ¿Se registran mis solicitudes?

**Respuesta**: Dispone de una opción durante la creación de una implementación para desactivar el seguimiento y, en este caso, no se registrará el audio ni las transcripciones. De lo contrario, las solicitudes se registran en Azure, en almacenamiento seguro. Si tiene más dudas sobre la privacidad que le impidan utilizar Custom Speech Service, póngase en contacto con nosotros.

## <a name="importing-data"></a>Importación de datos

**Pregunta**: ¿Qué límite hay para el tamaño del conjunto de datos? ¿Por qué? 

**Respuesta**: El límite actual de un conjunto de datos es de 2 GB, debido a la restricción en el tamaño de archivo para la carga HTTP. 

**Pregunta**: ¿Puedo comprimir mis archivos de texto para cargar un archivo de texto mayor? 

**Respuesta**: No, actualmente solo se permiten los archivos de texto no comprimidos.

**Pregunta**: El informe de datos indica que ha habido expresiones erróneas. ¿Esto es un problema?

**Respuesta**: Si no se pudieron importar correctamente solo unas cuantas expresiones, no supone un problema.
Si la gran mayoría de las expresiones de un conjunto de datos acústico o de lenguaje (por ejemplo, >95 %) se importan correctamente, se puede usar el conjunto de datos. Sin embargo, es aconsejable comprender la causa del error de las expresiones y corregir los problemas.
Los problemas más comunes, como los errores de formato, son fáciles de corregir. 

## <a name="creating-am"></a>Creación de AM

**Pregunta**: ¿Cuántos datos acústicos necesito?

**Respuesta**: Se recomienda empezar con 30 minutos a una hora de datos acústicos.

**Pregunta**: ¿Qué tipo de datos debo recopilar?

**Respuesta**: Debe recopilar datos lo más cercanos posibles al escenario de la aplicación y caso de uso.
Esto significa que la colección de datos debe coincidir con la aplicación y los usuarios de destino en términos de dispositivo o dispositivos, entornos y tipos de hablante. En general, debe recopilar datos de un intervalo lo más amplio posible de hablantes. 

**Pregunta**: ¿Cómo debo recopilarlos? 

**Respuesta**: Puede crear una aplicación de recopilación de datos autónoma o utilizar algún software de grabación de audio existente.
También puede crear una versión de la aplicación que registre los datos de audio y utilizarla. 

**Pregunta**: ¿Debo transcribir los datos de adaptación? 

**Respuesta**: Los datos deben transcribirse. Puede transcribirlos usted mismo o utilizar un servicio de transcripción profesional. Algunos de ellos utilizan transcriptores profesionales y otros utilizan crowdsourcing. También podemos recomendar un servicio de transcripción bajo petición.

**Pregunta**: ¿Cuánto se tarda en crear un modelo acústico personalizado?

**Respuesta**: El tiempo de proceso para crear un modelo acústico personalizado es aproximadamente el mismo que la duración del conjunto de datos acústico.
Por lo tanto, un modelo acústico personalizado creado a partir de un conjunto de datos de cinco horas tardará unas cinco horas en procesarse. 

## <a name="offline-testing"></a>Pruebas sin conexión

**Pregunta**: ¿Puedo realizar una prueba sin conexión de mi modelo acústico personalizado utilizando un modelo de lenguaje personalizado?

**Respuesta**: Sí, basta con seleccionar el modelo de lenguaje personalizado en el menú desplegable al configurar la prueba sin conexión.

**Pregunta**: ¿Puedo realizar una prueba sin conexión de mi modelo de lenguaje personalizado utilizando un modelo acústico personalizado?

**Respuesta**: Sí, basta con seleccionar el modelo acústico personalizado en el menú desplegable al configurar la prueba sin conexión.

**Pregunta**: ¿Qué es Word Error Rate y cómo se calcula?

**Respuesta**: Word Error Rate es la métrica de evaluación para el reconocimiento de voz. Se cuenta como el número total de errores, lo que incluye las inserciones, las eliminaciones y las sustituciones, dividido por el número total de palabras en la transcripción de referencia.

**¿Pregunta**: Ya conozco los resultados de la prueba de mi modelo personalizado. ¿Es un número correcto o incorrecto?

**Respuesta**: Los resultados muestran una comparación entre el modelo de línea base y el personalizado.
Debe aspirar a superar el modelo de línea base para que la personalización sea útil.

**Pregunta**: ¿Cómo averiguo la métrica WER de los modelos base para ver si ha habido una mejora? 

**Respuesta**: Los resultados de la prueba sin conexión muestran la precisión de la línea base, la precisión del modelo personalizado y la mejora sobre la línea base.

## <a name="creating-lm"></a>Creación de LM

**Pregunta**: ¿Cuántos datos necesito cargar?

**Respuesta**: Esta es una pregunta difícil de responder de forma precisa, ya que depende de lo diferentes que sean el vocabulario y las frases que se usan en la aplicación de los modelos de idioma de partida. Para todas las palabras nuevas, es útil proporcionar el máximo de ejemplos posible de utilización de estas palabras. Para frases comunes que se usan en la aplicación, también es útil incluirlas en los datos de lenguaje, ya que indican al sistema que escuche estos términos también.
Es habitual que haya al menos cien y, normalmente, varios cientos de expresiones en el conjunto de datos de lenguaje o más.
Además, si se espera que determinados tipos * de consultas sean más habituales que otras, puede insertar varias copias de las consultas comunes en el conjunto de datos.

**Pregunta**: ¿Puedo simplemente cargar una lista de palabras?

**Respuesta**: La carga de una lista de palabras colocará las palabras en el vocabulario, pero no enseñará al sistema cómo se utilizan normalmente las palabras.
Al proporcionar expresiones completas o parciales (oraciones o frases que es probable que los usuarios digan), el modelo de lenguaje puede aprender las palabras nuevas y cómo se utilizan. El modelo de lenguaje personalizado es bueno no solo para colocar palabras nuevas en el sistema, sino también para ajustar la probabilidad de palabras conocidas para la aplicación. Al proporcionar expresiones completas se ayuda al sistema a aprender. 

-----

 * [Información general](cognitive-services-custom-speech-home.md)
 * [Introducción](cognitive-services-custom-speech-get-started.md)
 * [Glosario](cognitive-services-custom-speech-glossary.md)
