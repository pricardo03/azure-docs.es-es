---
title: 'Inicio rápido: Biblioteca cliente de Personalizer para Python | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Introducción a la biblioteca de cliente de Personalizer para Python con un bucle de aprendizaje.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 02c4e0142ed7b3719cc07306f089769c532d6653
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494453"
---
# <a name="quickstart-personalizer-client-library-for-python"></a>Inicio rápido: Biblioteca cliente de Personalizer para Python

Muestre contenido personalizado en este inicio rápido de Python con el servicio Personalizer.

Introducción a la biblioteca de cliente de Personalizer para Python Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas.

 * Clasificar una lista de acciones para su personalización.
 * Informar sobre la puntuación de recompensa para indicar el éxito de la acción con clasificación superior.

[Paquete (pypi)](https://pypi.org/project/azure-cognitiveservices-personalizer/) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="using-this-quickstart"></a>Uso de este inicio rápido


Hay que dar varios pasos para usar este inicio rápido:

* En Azure Portal, cree un recurso de Personalizer
* En la página **Configuración** del recurso Personalizer de Azure Portal, cambie la frecuencia de actualización del modelo.
* En un editor de código, cree un archivo de código y edítelo
* En la línea de comandos o en el terminal, instale el SDK desde la línea de comandos
* En la línea de comandos o en el terminal, ejecute el archivo de código


## <a name="create-a-personalizer-azure-resource"></a>Creación de un recurso de Azure para Personalizer

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para Personalizer mediante [Azure Portal](https://portal.azure.com/) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local. Para más información, consulte [Creación de un recurso de Cognitive Services con Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). También puede:

* Obtener una [clave de prueba](https://azure.microsoft.com/try/cognitive-services) válida durante siete días de forma gratuita. Después de registrarse, estará disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Ver el recurso en [Azure Portal](https://portal.azure.com/)

Después de obtener una clave del recurso o la suscripción de evaluación, cree dos [variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_KEY` para la clave de recurso.
* `PERSONALIZER_ENDPOINT` para el punto de conexión de recurso.

En Azure Portal, los valores de clave y punto de conexión están disponibles en la página **Inicio rápido**.


## <a name="install-the-python-library-for-personalizer"></a>Instalación de la biblioteca de Python para Personalizer

Instale la biblioteca cliente de Personalizer para Python con el siguiente comando:

```console
pip install azure-cognitiveservices-personalizer
```

## <a name="change-the-model-update-frequency"></a>Cambio de la frecuencia de actualización del modelo

En la página **Configuración** del recurso Personalizer de Azure Portal, cambie el valor de **Model update frequency** (Frecuencia de actualización del modelo) a 10 segundos. Así se entrenará el servicio rápidamente, lo que permite ver cómo cambia la acción principal en cada iteración.

![Cambiar la frecuencia de actualización del modelo](./media/settings/configure-model-update-frequency-settings.png)

La primera vez que se instancia un bucle de Personalizer, no hay ningún modelo porque no se ha producido ninguna llamada a API Reward a partir de la que realizar el entrenamiento. Las llamadas de Rank devolverán las mismas probabilidades para cada elemento. La aplicación siempre debe clasificar el contenido mediante la salida de RewardActionId.

## <a name="object-model"></a>Modelo de objetos

El cliente de Personalizer es un objeto PersonalizerClient que se autentica en Azure mediante Microsoft.Rest.ServiceClientCredentials, que contiene la clave.

Para solicitar una clasificación del contenido, cree un RankRequest y, a continuación, páselo al método cliente.Rank. El método Rank devuelve un elemento RankResponse, que contiene el contenido clasificado. 

Para enviar una recompensa a Personalizer, cree un RewardRequest y, a continuación, páselo al método client.Reward. 

La determinación de la recompensa en este inicio rápido es trivial. En un sistema de producción, la determinación de lo que afecta a la [puntuación de recompensa](concept-rewards.md) y cuánto le afecta, puede ser un proceso complejo que decida cambiar con el tiempo. Esta debe ser una de las decisiones de diseño principales en la arquitectura de su instancia de Personalizer. 

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes acciones con la biblioteca cliente de Personalizer para Python:

* [Creación de un cliente de Personalizer](#create-a-personalizer-client)
* [Solicitud de una clasificación](#request-a-rank)
* [Envío de una recompensa](#send-a-reward)

## <a name="create-a-new-python-application"></a>Creación de una nueva aplicación de Python

Cree una aplicación de Python en el IDE o editor que prefiera con el nombre `sample.py`. 

## <a name="add-the-dependencies"></a>Adición de las dependencias

En el directorio del proyecto, abra el archivo **sample.py** en el editor o IDE que prefiera. Agregue la siguiente línea de código:

[!code-python[Add module dependencies](~/samples-personalizer/quickstarts/python/sample.py?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Incorporación de la información de recursos de Personalizer

Cree variables para la clave y el punto de conexión de Azure del recurso extraídos de las variables de entorno, denominadas `PERSONALIZER_RESOURCE_KEY` y `PERSONALIZER_RESOURCE_ENDPOINT`. Si ha creado las variables de entorno una vez iniciada la aplicación, el editor, IDE o shell que se esté ejecutando se tendrá que cerrar y volver a cargar para acceder a la variable. Los métodos se crearán más adelante en este inicio rápido.

El nombre del recurso forma parte de la dirección URL del punto de conexión: `https://<your-resource-name>.api.cognitive.microsoft.com/`.

[!code-python[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/python/sample.py?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Creación de un cliente de Personalizer

A continuación, cree un método para devolver un cliente de Personalizer. El parámetro para el método es `PERSONALIZER_RESOURCE_ENDPOINT` y el elemento ApiKey es `PERSONALIZER_RESOURCE_KEY`.

[!code-python[Create the Personalizer client](~/samples-personalizer/quickstarts/python/sample.py?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>Obtención de opciones de contenido representadas como acciones

Las acciones representan las opciones de contenido que desea que Personalizer clasifique. Agregue los métodos siguientes para obtener una entrada de usuario desde la línea de comandos para la hora del día y la preferencia alimentaria actual.

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=getActions)]

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTimeOfDay)]

[!code-python[Present food taste preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTastePreference)]

## <a name="create-the-learning-loop"></a>Creación del bucle de aprendizaje

El bucle de aprendizaje de Personalizer es un ciclo de llamadas de [clasificación](#request-a-rank) y de [recompensa](#send-a-reward). En este inicio rápido, cada llamada de clasificación para personalizar el contenido, va seguida de una llamada de recompensa para indicar a Personalizer cuál es el grado de precisión con que el servicio ha clasificado el contenido. 

El siguiente código realiza un bucle en un ciclo en el que se pregunta al usuario sus preferencias en la línea de comandos, y se envía esa información a Personalizer para que la clasifique, luego se presenta la selección clasificada al cliente para que elija uno de los elementos de la lista y, después, se envía una recompensa a Personalizer en la que se señala el grado de acierto que tuvo el servicio al clasificar la selección.

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=mainLoop&highlight=9,10,29)]

Observe detalladamente las llamadas de clasificación y recompensa en las secciones siguientes.

Agregue los métodos siguientes, que [obtienen las opciones de contenido](#get-content-choices-represented-as-actions), antes de ejecutar el archivo de código:

* get_user_preference
* get_user_timeofday
* get_actions

## <a name="request-a-rank"></a>Solicitud de una clasificación

Para completar la solicitud de clasificación, el programa solicita las preferencias del usuario para crear un elemento `currentContent`de las opciones de contenido. El proceso puede crear contenido para excluir de la clasificación, que se muestra como `excludeActions`. La solicitud de clasificación necesita las acciones, currentContext, excludeActions y un identificador de evento de clasificación único (como un GUID), para recibir la respuesta clasificada. 

Este inicio rápido tiene características de contexto simples de hora del día y preferencias alimentarias del usuario. En los sistemas de producción, la determinación y [evaluación](concept-feature-evaluation.md) de [acciones y características](concepts-features.md) puede ser una cuestión menos trivial.  

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=rank)]

## <a name="send-a-reward"></a>Envío de una recompensa

Para completar la solicitud de recompensa, el programa obtiene la selección del usuario desde la línea de comandos, asigna un valor numérico a cada selección y, después, envía el identificador único del evento de clasificación y el valor numérico al método de recompensa.

En este inicio rápido se asigna un número simple como recompensa, o cero o 1. En sistemas de producción, determinar cuándo y qué enviar a la llamada de [recompensa](concept-rewards.md) puede ser más complicado, en función de sus necesidades específicas. 

[!code-python[The Personalizer learning loop sends a reward.](~/samples-personalizer/quickstarts/python/sample.py?name=reward&highlight=9)]

## <a name="run-the-program"></a>Ejecución del programa

Ejecute la aplicación con el comando de Python desde el directorio de aplicación.

```console
python sample.py
```

![El programa del inicio rápido realiza dos preguntas para recopilar las preferencias del usuario, conocidas como características, y, después, proporciona la acción principal.](./media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Funcionamiento de Personalizer](how-personalizer-works.md)

* [¿Qué es Personalizer?](what-is-personalizer.md)
* [¿Dónde se puede utilizar Personalizer?](where-can-you-use-personalizer.md)
* [Solución de problemas](troubleshooting.md)
* El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py).
