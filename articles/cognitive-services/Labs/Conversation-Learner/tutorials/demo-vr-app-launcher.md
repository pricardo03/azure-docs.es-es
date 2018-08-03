---
title: 'Modelo de Conversation Learner de demostración, iniciador de aplicaciones de realidad virtual: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo crear un modelo de Conversation Learner de demostración.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 896ec007c03e30e5c20a5344430be040271bc00b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171157"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demostración: Iniciador de aplicaciones de realidad Virtual

Esta demostración muestra el iniciador de un modelo de realidad virtual que admite comandos como "start Skype and put it on the wall" (Iniciar Skype y colocarlo en el muro). Un usuario debe indicar el nombre y la ubicación de una aplicación para poder iniciarla. El inicio del modelo se realiza mediante una llamada API. Cuando el usuario reconoce el nombre de una aplicación, entityDetectionCallback comprueba si la aplicación solicitada coincide con una o varias aplicaciones de la lista de aplicaciones instaladas. Controla el caso en que la aplicación solicitada no está instalada y cuando el nombre de la aplicación es ambiguo (coincidencias con más de una aplicación instalada).

## <a name="video"></a>Vídeo

[![Vista previa de la aplicación de realidad virtual de demostración](http://aka.ms/cl-demo-vrapp-preview)](http://aka.ms/blis-demo-vrapp)

## <a name="requirements"></a>Requisitos

Para poder realizar este tutorial debe ejecutar el bot VRAppLauncher:

    npm run demo-vrapp
    
### <a name="open-the-demo"></a>Abrir la demostración

En la lista de modelos de la interfaz de usuario web, haga clic en VRAppLauncher. 

## <a name="entities"></a>Entidades

Hemos creado cuatro entidades:

- AppName: por ejemplo, Skype.
- PlacementLocation: por ejemplo, el muro.
- UnknownAppName: una entidad de programación que el sistema establece cuando no reconoce un nombre de entidad que el usuario indica; por ejemplo, porque no se ha instalado.
- DisAmbigAppNames: una matriz de dos o más nombres de aplicaciones instaladas que coinciden con el que el usuario ha indicado. 

![](../media/tutorial_vrapplauncher_entities.PNG)

### <a name="actions"></a>Acciones

Se ha creado un conjunto de acciones que incluye una API que llamó a LaunchApp, que iniciará la llamada a la función para iniciar una aplicación.

![](../media/tutorial_vrapplauncher_actions.PNG)

### <a name="training-dialogs"></a>Diálogos de entrenamiento
Hemos definido una serie de cuadros de diálogo de entrenamiento.

![](../media/tutorial_vrapplauncher_dialogs.PNG)

Por ejemplo, vamos a probar una sesión de instrucción.

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
1. Escriba "hi" (hola).
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "which app do you want to start?" (¿Qué aplicación desea iniciar?).
4. Escriba "outlook".
    - LUIS la reconoce como una entidad.
5. Haga clic en Score Actions (Acciones de puntuación).
3. Haga clic para seleccionar "where do you want it placed?" (¿Dónde desea que se ubique?).
4. Escriba "on the wall" (En el muro).
    - LUIS lo reconoce como PlacementLocation.
2. Escriba Score Actions (Acciones de puntuación).
6. Seleccione "LaunchApp".
7. Sistema: "starting outlook on the wall" (Iniciando Outlook en el muro).
    - Esto desencadena una llamada API. El código de esta llamada se encuentra en C:\<\installedpath>\src\demos\demoVRAppLauncher.ts. Sin embargo, no contiene realmente el código para iniciar Outlook en esta demostración.
    - Borra las entidades AppName y PlacementLocation. Devuelve la cadena anterior como respuesta.
4. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Vamos a iniciar otra sesión de entrenamiento para administrar entidades desconocidas y ambiguas.

1. Haga clic en New Train Dialog (Haga clic en diálogo de entrenamiento).
1. Escriba "start OneNote". 
    - El modelo reconoce a OneNote como un nombre de aplicación. La función `EntityDetectionCallback` definida en el código resuelve el nombre que escribe el usuario para un nombre de aplicación al hacerlo coincidir con la lista de aplicaciones definida en el código. Devuelve entonces el conjunto de todas las aplicaciones coincidentes. 
    - Si la lista de coincidencias es cero, significa que la aplicación no está instalada. La coloca en unknownAppName.
    - Si encuentra más de una aplicación, la copia en `DisambigAppNames` y borra la entidad AppName.
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "Sorry, I don't know the app $UknownAppName" (No conozco la aplicación $UknownAppName).
4. Escriba "start Amazon" (Iniciar Amazon). Probaremos la otra ruta de acceso.
5. Haga clic en Score Actions (Acciones de puntuación).
    - Amazon Video y Amazon Music ahora están en la memoria de `DisambigAppNames` y se borró OneNote.
3. Haga clic para seleccionar "There are few apps that sound like that..." (Hay algunas aplicaciones que suenan de forma similar a...).
    - La puntuación no es muy alta porque solo tenemos definidos algunos diálogos de entrenamiento hasta este punto. Definir más cuadros de diálogo haría que el modelo sea más decisivo.
2. Escriba Score Actions (Acciones de puntuación).
4. Haga clic en Done Teaching (Aprendizaje completado).

Ahora ha visto cómo llevar a cabo la resolución de entidades. La demostración también muestra las devoluciones de llamada API y una plantilla para recopilar información, comprobar la presencia y ambigüedad y llevar a cabo la acción correcta en consecuencia.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementar un bot de Conversation Learner](../deploy-to-bf.md)
