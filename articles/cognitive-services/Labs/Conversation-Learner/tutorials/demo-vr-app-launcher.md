---
title: 'Aplicación Conversation Learner de demostración, iniciador de aplicaciones de realidad virtual: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo crear una aplicación Conversation Learner de demostración.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3e41125bf7da9ee64d666d22cb275af01af54012
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381806"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demostración: Iniciador de aplicaciones de realidad Virtual

En esta demostración se ilustra un iniciador de aplicaciones de realidad virtual, que admite comandos como "start Skype and put in on the wall" (Iniciar Skype y colocarlo en el muro). Un usuario debe indicar el nombre y la ubicación de una aplicación para poder iniciarla. El inicio de la aplicación se realiza mediante una llamada API. Cuando el usuario reconoce el nombre de una aplicación, entityDetectionCallback comprueba si la aplicación solicitada coincide con una o varias aplicaciones de la lista de aplicaciones instaladas. Controla el caso en que la aplicación solicitada no está instalada y cuando el nombre de la aplicación es ambiguo (coincidencias con más de una aplicación instalada).

## <a name="requirements"></a>Requisitos

Para poder realizar este tutorial debe ejecutar el bot VRAppLauncher.

    npm run demo-vrapp

### <a name="open-the-demo"></a>Abrir la demostración

En la lista de aplicaciones de la interfaz de usuario web, haga clic en VRAppLauncher. 

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
    - Tenga en cuenta que LUIS lo reconoce como una entidad.
5. Haga clic en Score Actions (Acciones de puntuación).
3. Haga clic para seleccionar "where do you want it placed?" (¿Dónde desea que se ubique?).
4. Escriba "on the wall" (En el muro).
    - Tenga en cuenta que LUIS lo reconoce como PlacementLocation.
2. Escriba Score Actions (Acciones de puntuación).
6. Seleccione "LaunchApp".
7. Sistema: "starting outlook on the wall" (Iniciando Outlook en el muro).
    - Tenga en cuenta que esto desencadena una llamada API. El código de esta llamada se encuentra en C:\<\installedpath>\src\demos\demoVRAppLauncher.ts. Sin embargo, realmente no tiene la lógica para iniciar Outlook para esta demostración.
    - Borra las entidades AppName y PlacementLocation. Devuelve la cadena anterior como respuesta.
4. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Vamos a iniciar otra sesión de entrenamiento para administrar entidades desconocidas y ambiguas.

1. Haga clic en New Train Dialog (Haga clic en diálogo de entrenamiento).
1. Escriba "start OneNote". 
    - Lo reconoce como el nombre de una aplicación porque el elemento EntityDetectionCallback definido en el código adopta el nombre que el usuario ha escrito y lo resuelve como un nombre de aplicación asociándolo con la lista de aplicaciones definida en el código. Devuelve entonces el conjunto de todas las aplicaciones coincidentes. 
    - Si la lista de coincidencias es cero, significa que la aplicación no está instalada. La coloca en unknownAppName.
    - Si encuentra más de una aplicación, las copia en DisambigAppNames y borra la entidad AppName.
2. Haga clic en Score Action (Acción de puntuación).
3. Haga clic para seleccionar "Sorry, I don't know the app $UknownAppName" (No conozco la aplicación $UknownAppName).
4. Escriba "start amazon" (Iniciar Amazon). Probaremos la otra ruta de acceso.
5. Haga clic en Score Actions (Acciones de puntuación).
    - Tenga en cuenta que Amazon Video y Amazon Music se encuentran ahora en la memoria de DisambigAppNames. Y OneNote se ha borrado.
3. Haga clic para seleccionar "There are few apps that sound like that..." (Hay algunas aplicaciones que suenan de forma similar a...).
    - Tenga en cuenta que la puntuación no es muy alta, dado que solo tenemos algunos diálogos de entrenamiento hasta este punto. Parece que se necesita algo más aquí para hacer que el modelo sea más decisivo.
2. Escriba Score Actions (Acciones de puntuación).
4. Haga clic en Done Teaching (Aprendizaje completado).

Ahora ha visto cómo llevar a cabo la resolución de entidades. La demostración también muestra las devoluciones de llamada API y una plantilla para recopilar información, comprobar la presencia y ambigüedad y llevar a cabo la acción correcta en función de lo anterior.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementar un bot de Conversation Learner](../deploy-to-bf.md)
