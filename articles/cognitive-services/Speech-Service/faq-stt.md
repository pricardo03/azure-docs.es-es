---
title: Preguntas más frecuentes sobre el servicio Speech to Text en Azure
description: Estas son las respuestas a las preguntas más habituales sobre la conversión de voz en texto.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: e5ba01c25646578da22f054659051be3515e9e4b
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281837"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Preguntas más frecuentes sobre la conversión de voz en texto

Si no encuentra respuestas a sus preguntas en estas preguntas más frecuentes, consulte otras opciones de soporte técnico [aquí](support.md).

## <a name="general"></a>General

**Pregunta**: ¿Qué diferencia hay entre los modelos de línea base y los modelos personalizados de conversión de voz a texto?

**Respuesta**: Los modelos de línea base se han entrenado con datos propiedad de Microsoft y ya están implementados en la nube. Los modelos personalizados permiten al usuario adaptar un modelo para que se ajuste mejor a un entorno en particular con ruido ambiental o idioma específicos. Las plantas de fábrica, los automóviles, las calles ruidosas requerirán un modelo acústico adaptado, mientras que los temas específicos como la biología, la física, la radiología, los nombres de productos y los acrónimos personalizados requerirán un modelo de lenguaje adaptado.

**Pregunta**: ¿Dónde empiezo si quiero usar un modelo de línea base?

**Respuesta**: Primero es necesario obtener una [clave de suscripción](get-started.md). Si quiere realizar llamadas REST a los modelos de línea de base implementados previamente, consulte los [detalles aquí](rest-apis.md). Si quiere usar WebSockets, descargue el [SDK](speech-sdk.md).

**Pregunta**: ¿Es siempre necesario crear un modelo de voz personalizado?

**Respuesta**: No, si la aplicación usa lenguaje cotidiano genérico, no es necesario personalizar un modelo. Además, si la aplicación se va a usar en un entorno con poco o ningún ruido de fondo, tampoco es necesaria la personalización. El portal permite a los usuarios implementar modelos de línea base y personalizados, y ejecutar pruebas de precisión en ellos. Los usuarios pueden utilizar esta función para medir la precisión de un modelo de línea base con respecto a uno personalizado.

**Pregunta**: ¿Cómo sabré que el proceso de mi conjunto de datos o modelo se ha completado?

**Respuesta**: Actualmente, el estado del modelo o del conjunto de datos de la tabla es la única manera de saberlo.
Cuando se complete el proceso, el estado será "Succeeded" (Correcto).

**Pregunta**: ¿Puedo crear más de un modelo?

**Respuesta**: No hay ningún límite en el número de modelos que puede haber en la colección.

**Pregunta**: Me he dado cuenta de que me he equivocado. ¿Cómo cancelo la importación de datos o la creación del modelo que está en curso? 

**Respuesta**: Actualmente no se puede revertir un proceso de adaptación acústica o de lenguaje. Los modelos y los datos importados se pueden eliminar después de que estén en estado terminal.

**Pregunta**: ¿Qué diferencia existe entre los modelos de búsqueda y dictado y los modelos de conversación?

**Respuesta**: Hay varios modelos de línea base entre los cuales elegir en Speech Service. El modelo Conversational es adecuado para el reconocimiento de la voz hablada en un estilo conversacional. Este modelo sería ideal para transcribir llamadas al buscar y Dictation es ideal para las aplicaciones activadas por voz. Universal es un modelo nuevo que apunta a abordar ambos escenarios.

**Pregunta**: ¿Puedo actualizar el modelo existente (apilamiento del modelo)?

**Respuesta**: Los modelos existentes no se pueden actualizar. Como solución alternativa, puede combinar el conjunto de datos anterior con el nuevo y readaptarlo.

Los conjuntos de datos anterior y nuevo se deben combinar en un único archivo .zip (si se trata de datos acústicos) o en un archivo .txt si se trata de datos de lenguaje. Una vez realizada la adaptación, se debe anular la implementación del nuevo modelo actualizado para obtener un nuevo punto de conexión.

**Pregunta**: ¿Qué pasa si necesito una mayor simultaneidad para el modelo implementado que la que se ofrece en el portal? 

**Respuesta**: Se puede ampliar el modelo en incrementos de 20 solicitudes simultáneas. 

Póngase en contacto con nosotros si necesita una ampliación mayor.

**Pregunta**: ¿Puedo descargar mi modelo y ejecutarlo localmente?

**Respuesta**: Los modelos no se pueden descargar y ejecutar localmente.

**Pregunta**: ¿Se registran mis solicitudes?

**Respuesta**: Dispone de una opción durante la creación de una implementación para desactivar el seguimiento y, en este caso, no se registrará el audio ni las transcripciones. De lo contrario, las solicitudes se registran en Azure, en almacenamiento seguro. Si tiene más dudas sobre la privacidad que le impidan utilizar Custom Speech Service, póngase en contacto con uno de los canales de soporte técnico.

## <a name="importing-data"></a>Importación de datos

**Pregunta**: ¿Qué límite hay para el tamaño del conjunto de datos? ¿Por qué? 

**Respuesta**: El límite actual de un conjunto de datos es de 2 GB, debido a la restricción en el tamaño de archivo para la carga HTTP. 

**Pregunta**: ¿Puedo comprimir mis archivos de texto para cargar un archivo de texto mayor? 

**Respuesta**: No, actualmente solo se permiten los archivos de texto no comprimidos.

**Pregunta**: El informe de datos indica que ha habido expresiones erróneas. ¿Cuál es el problema?

**Respuesta**: No es un problema no poder cargar el 100 % de las expresiones en un archivo.
Si la gran mayoría de las expresiones de un conjunto de datos acústico o de lenguaje (por ejemplo, >95 %) se importan correctamente, se puede usar el conjunto de datos. Sin embargo, es aconsejable comprender la causa del error de las expresiones y corregir los problemas. Los problemas más comunes, como los errores de formato, son fáciles de corregir. 

## <a name="creating-am"></a>Creación de AM

**Pregunta**: ¿Cuántos datos acústicos necesito?

**Respuesta**: Se recomienda empezar con 30 minutos a una hora de datos acústicos.

**Pregunta**: ¿Qué datos debo recopilar?

**Respuesta**: Recopile datos lo más cercanos posibles al escenario de aplicación y caso de uso.
La colección de datos debe coincidir con la aplicación y los usuarios de destino en términos de dispositivo o dispositivos, entornos y tipos de hablante. En general, debe recopilar datos de un intervalo lo más amplio posible de hablantes. 

**Pregunta**: ¿Cómo debo recopilarlos? 

**Respuesta**: Puede crear una aplicación de recopilación de datos autónoma o utilizar algún software de grabación de audio existente.
También puede crear una versión de la aplicación que registre los datos de audio y utilizarla. 

**Pregunta**: ¿Debo transcribir los datos de adaptación? 

**Respuesta**: Por supuesto. Puede transcribirlos usted mismo o utilizar un servicio de transcripción profesional. Algunos usuarios prefieren usar transcriptores profesionales, mientras que otros usan la colaboración abierta distribuida o ellos mismos realizan las transcripciones.

## <a name="accuracy-testing"></a>Pruebas de precisión

**Pregunta**: ¿Puedo realizar una prueba sin conexión de mi modelo acústico personalizado utilizando un modelo de lenguaje personalizado?

**Respuesta**: Sí, basta con seleccionar el modelo de lenguaje personalizado en el menú desplegable al configurar la prueba sin conexión.

**Pregunta**: ¿Puedo realizar una prueba sin conexión de mi modelo de lenguaje personalizado utilizando un modelo acústico personalizado?

**Respuesta**: Sí, basta con seleccionar el modelo acústico personalizado en el menú desplegable al configurar la prueba sin conexión.

**Pregunta**: ¿Qué es Word Error Rate (WER) y cómo se calcula?

**Respuesta**: Word Error Rate (WER) es la métrica de evaluación para el reconocimiento de voz. Se cuenta como el número total de errores, lo que incluye las inserciones, las eliminaciones y las sustituciones, dividido por el número total de palabras en la transcripción de referencia. Más detalles [aquí](https://en.wikipedia.org/wiki/Word_error_rate).

**Pregunta**: ¿Cómo determino si los resultados de una prueba de precisión son buenos?

**Respuesta**: Los resultados muestran una comparación entre el modelo de línea base y el personalizado.
Debe aspirar a superar el modelo de línea base para que la personalización sea útil.

**Pregunta**: ¿Cómo averiguo la métrica Word Error Rate de los modelos base para ver si ha habido una mejora? 

**Respuesta**: Los resultados de la prueba sin conexión muestran la precisión de la línea base, la precisión del modelo personalizado y la mejora sobre la línea base.

## <a name="creating-lm"></a>Creación de LM

**Pregunta**: ¿Cuántos datos necesito cargar?

**Respuesta**: Depende de la diferencia que exista entre el vocabulario y las frases que se usan en la aplicación y los modelos de lenguaje iniciales. Para todas las palabras nuevas, es útil proporcionar el máximo de ejemplos posible de utilización de estas palabras. Para frases comunes que se usan en la aplicación, también es útil incluir frases en los datos de lenguaje, ya que indican al sistema que escuche estos términos también. Es habitual que haya al menos cien y, normalmente, varios cientos de expresiones en el conjunto de datos de lenguaje o más. Además, si se espera que determinados tipos de consultas sean más habituales que otras, puede insertar varias copias de las consultas comunes en el conjunto de datos.

**Pregunta**: ¿Puedo simplemente cargar una lista de palabras?

**Respuesta**: La carga de una lista de palabras colocará las palabras en el vocabulario, pero no enseñará al sistema cómo se utilizan normalmente las palabras.
Al proporcionar expresiones completas o parciales (oraciones o frases que es probable que los usuarios digan), el modelo de lenguaje puede aprender las palabras nuevas y cómo se utilizan. El modelo de lenguaje personalizado es bueno no solo para colocar palabras nuevas en el sistema, sino también para ajustar la probabilidad de palabras conocidas para la aplicación. Al proporcionar expresiones completas se ayuda al sistema a aprender mejor. 

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas](troubleshooting.md)
* [Notas de la versión](releasenotes.md)
