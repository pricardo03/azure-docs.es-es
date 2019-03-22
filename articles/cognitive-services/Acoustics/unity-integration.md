---
title: Proyecto acústica Unity integración e implementación
titlesuffix: Azure Cognitive Services
description: Este tema de procedimientos explica la integración del complemento proyecto acústica Unity en el proyecto de Unity.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: aa7c5c513d65310bf9bffab29c1d18e7e7a85b49
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316293"
---
# <a name="project-acoustics-unity-integration"></a>Integración con Unity Project acústica
Este tema de procedimientos explica la integración del complemento proyecto acústica Unity en el proyecto de Unity.

Requisitos de software:
* [Unity 2018.2 +](http://unity3d.com) para Windows
* [Paquete del proyecto Unity acústica](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>Importación del complemento
Importe el paquete UnityPackage de acústica al proyecto. 
* En Unity, vaya a **Assets > Import Package > Custom Package...** (Recursos > Importar paquete > Paquete personalizado)

    ![Menú de la captura de pantalla de Unity Importar paquete](media/import-package.png)  

* Elija **ProjectAcoustics.unitypackage**

Si va a importar el complemento a un proyecto existente, es posible que el proyecto ya tenga un archivo **mcs.rsp** en la raíz del proyecto, que especifica las opciones del compilador de C#. Deberá combinar el contenido de ese archivo con el archivo mcs.rsp que se incluye con el complemento de Elementos acústicos de un proyecto.

## <a name="enable-the-plugin"></a>Habilitación del complemento
La parte de elaboración del kit de herramientas de Elementos acústicos requiere la versión en tiempo de ejecución de scripting .NET 4.x. La importación del paquete actualizará la configuración del reproductor de Unity. Reinicie Unity para que esta configuración surta efecto.

![Panel de la captura de pantalla de configuración de Reproductor de Unity](media/player-settings.png)

![Panel de la captura de pantalla de configuración de Reproductor de Unity con .NET 4.5 seleccionado](media/net45.png)

## <a name="set-up-audio-dsp"></a>Configuración de audio DSP
Proyecto acústica incluye runtime audio DSP que se integra en el marco de espacial del motor de audio de Unity. Incluye spatialization basado en la HRTF y panorámica. Habilitar proyecto acústica DSP abriendo la configuración de audio de Unity con **Editar > configuración del proyecto > Audio**, a continuación, seleccione **acústica de proyecto** como el **espacial complemento** para el proyecto. Asegúrese de que el **tamaño del búfer DSP** está establecido en el mejor rendimiento.

![Panel de la captura de pantalla de configuración del proyecto Unity](media/project-settings.png)  

![Panel de configuración de captura de pantalla de Unity espacial con espacial acústica del proyecto seleccionado](media/choose-spatializer.png)

A continuación, abra el Audio de Mixer (**Ventana > Audio Mixer**). Asegúrese de tener al menos un mezclador, con un grupo. Si no es así, haga clic en el botón "+" que está a la derecha de **Mixers** (Mezcladores). Haga clic en la parte inferior de la franja de canal en la sección de efectos y agregue el **proyecto acústica Mixer** efecto. Tenga en cuenta que solo se admite un mezclador de Elementos acústicos de un proyecto a la vez.

![Captura de pantalla de Mixer de Audio Unity Mezclador de sonido del proyecto de hospedaje](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Habilitar acústica en orígenes de sonido
Cree un origen de sonido. Active la casilla de verificación en la parte inferior del panel de inspector de AudioSource que dice **Spatialize** (Espacializar). Asegúrese de que **Spatial Blend** (Combinación espacial) se establece en 3D completo.  

![Panel de la captura de pantalla de origen de Audio de Unity](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Habilitar diseño acústico
Adjuntar la secuencia de comandos **AcousticsAdjust** a un origen de sonido en su escena para habilitar parámetros de origen adicionales de diseño, haga clic en **Agregar componente** y eligiendo **Scripts > Ajustar acústica** :

![Captura de pantalla de Unity AcousticsAdjust script](media/acoustics-adjust.png)

## <a name="next-steps"></a>Pasos siguientes
* [Integrar la escena con acústica de proyecto de Unity](unity-baking.md)
* [Crear una cuenta de Azure Batch](create-azure-account.md) para integrar la escena en la nube
* Explore la [proceso de diseño del proyecto acústica Unity](unity-workflow.md).

