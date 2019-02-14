---
title: 'Ejemplo: Elementos acústicos de un proyecto'
titlesuffix: Azure Cognitive Services
description: En este tutorial se describe la escena de ejemplo de Unity para Elementos acústicos de un proyecto, incluida la implementación en escritorio y VR.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: sample
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: ecd19ec343b2744a70456fd1d3ad226856768d71
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880718"
---
# <a name="unity-sample-walkthrough"></a>Tutorial de ejemplo de Unity
Este es un tutorial del ejemplo de Elementos acústicos de un proyecto. Para más información sobre qué es Elementos acústicos de un proyecto, consulte la [introducción a Elementos acústicos de un proyecto](what-is-acoustics.md). Para ayuda sobre cómo agregar el paquete de Elementos acústicos de un proyecto a un proyecto de Unity que ya existe, use la [guía de introducción](getting-started.md).

## <a name="requirements-for-running-the-sample-project"></a>Requisitos para ejecutar el proyecto de ejemplo
* Unity 2018.2+, con versión en tiempo de ejecución de scripting .NET 4.x
* Editor Unity para Windows de 64 bits
* El ejemplo admite destinos de Android, UWP y escritorio de Windows, incluidos casos de realidad virtual
* Se requiere una suscripción de Azure Batch para el proceso de elaboración

## <a name="sample-project-setup"></a>Configuración del proyecto de ejemplo
Descargue e importe **MicrosoftAcoustics.Sample.unitypackage**. En la importación, se actualiza la configuración del proyecto (incluido **Espacializador** y **Versión en tiempo de ejecución de scripting**) para cumplir con los requisitos del complemento. Cuando haya finalizado esto, verá un error en la consola de Unity desde **AcousticsGeometry.cs** sobre cambiar la versión en tiempo de ejecución de scripting a **Equivalente a .NET 4.x**. Este cambio de configuración se hace como parte de la importación del paquete, pero es necesario reiniciar Unity para que surta efecto. Reinicie Unity ahora.

## <a name="running-the-sample"></a>Ejecución del ejemplo
El ejemplo incluye una escena de demostración, **Assets/AcousticsDemo/ProjectAcousticsDemo.unity**. Esta escena tiene tres orígenes de sonido. De forma predeterminada, solo se reproduce un único origen de sonido y los otros dos están en pausa. Se encuentran en **Orígenes de sonido** en la **jerarquía**. Para ayudar a crear un script de navegación genérico, Main Camera (Cámara principal) es un objeto secundario del objeto CameraHolder. 

![Vista Jerarquía](media/SampleHierarchyView.png)

La escena ya se elaboró y tiene un archivo ACE asociado con el objeto prefabricado **MicrosoftAcoustics** en la **jerarquía**. 

Para saber cómo suena la escena, haga clic en el botón de reproducción del editor de Unity. En el escritorio, use W, A, S, D y el mouse para desplazarse. Para comparar cómo suena la escena con y sin acústica, presione el botón **R** hasta que el texto superpuesto cambie a color rojo en indique "Acoustics: Disabled" (Acústica: deshabilitada). Para ver los métodos abreviados de teclado de otros controles, presione **F1**. Todos los controles también se pueden usar si se hace clic con el botón derecho para seleccionar la acción que se va a realizar y, después, se hace clic para realizarla.

## <a name="targeting-other-platforms"></a>Otras plataformas como destino
El ejemplo contiene configuraciones para ejecutarse en el escritorio de Windows, UWP, Windows Mixed Reality, Android y Oculus Go. De manera predeterminada, el proyecto está configurado para el escritorio de Windows. Para establecer una plataforma de VR como destino, vaya a la configuración del reproductor (**Edit > Project Settings > Player**) (Editar > Configuración del proyecto > Reproductor), busque **XR Settings** (Configuración de XR) y active la casilla **Virtual Reality Supported** (Realidad virtual compatible).

![Habilitación de la realidad virtual](media/VRSupport.png)  

Conecte un casco de realidad virtual al equipo. Vaya a **File > Build Settings** (Archivo > Configuración de generación) y haga clic en **Build and Run** (Generar y ejecutar) para implementar el ejemplo en el casco de realidad virtual. Recorra la escena con los controladores de movimiento del casco o intente usar las teclas W, A, S, D.    
Para establecer Android y Oculus Go como destino, elija Android en el menú **Build Settings** (Configuración de generación). Haga clic en **Switch Target** (Cambiar destino) y luego en **Build and Run** (Generar y ejecutar). Con esto, la escena de ejemplo se implementará en el dispositivo Android conectado. Para obtener información sobre el desarrollo de Unity para Android, vea la [documentación de Unity](https://docs.unity3d.com/Manual/android-GettingStarted.html).

![Android como destino](media/TargetAndroid.png)  

## <a name="next-steps"></a>Pasos siguientes
* [Cree una cuenta de Azure](create-azure-account.md) para sus propias elaboraciones
* Explore el [proceso de diseño](design-process.md)

