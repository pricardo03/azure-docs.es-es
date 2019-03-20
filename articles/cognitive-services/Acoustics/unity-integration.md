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
ms.date: 03/14/2019
ms.author: kegodin
ms.openlocfilehash: 7d8edd7b092ee1ed4f953d753b2bbe864e075378
ms.sourcegitcommit: f68b0e128f0478444740172f54e92b453df696be
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58137739"
---
# <a name="project-acoustics-unity-integration"></a>Integración con Unity Project acústica
Este tema de procedimientos explica la integración del complemento proyecto acústica Unity en el proyecto de Unity.

Requisitos de software:
* [Unity 2018.2 +](http://unity3d.com) para Windows
* [Paquete del proyecto Unity acústica](https://www.microsoft.com/en-us/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>Importación del complemento
Importe el paquete UnityPackage de acústica al proyecto. 
* En Unity, vaya a **Assets > Import Package > Custom Package...** (Recursos > Importar paquete > Paquete personalizado)

    ![Importación de paquete](media/import-package.png)  

* Elija **ProjectAcoustics.unitypackage**

Si va a importar el complemento a un proyecto existente, es posible que el proyecto ya tenga un archivo **mcs.rsp** en la raíz del proyecto, que especifica las opciones del compilador de C#. Deberá combinar el contenido de ese archivo con el archivo mcs.rsp que se incluye con el complemento de Elementos acústicos de un proyecto.

## <a name="enable-the-plugin"></a>Habilitación del complemento
La parte de elaboración del kit de herramientas de Elementos acústicos requiere la versión en tiempo de ejecución de scripting .NET 4.x. La importación del paquete actualizará la configuración del reproductor de Unity. Reinicie Unity para que esta configuración surta efecto.

![Configuración del Reproductor](media/player-settings.png)

![.NET 4.5](media/net45.png)

## <a name="set-up-audio-dsp"></a>Configuración de audio DSP
Proyecto acústica incluye runtime audio DSP que se integra en el marco de espacial del motor de audio de Unity. Incluye spatialization basado en la HRTF y panorámica. Habilitar proyecto acústica DSP abriendo la configuración de audio de Unity con **Editar > configuración del proyecto > Audio**, a continuación, seleccione **acústica de proyecto** como el **espacial complemento** para el proyecto. Asegúrese de que el **tamaño del búfer DSP** está establecido en el mejor rendimiento.

![Configuración del proyecto](media/project-settings.png)  

![Espacializador de Elementos acústicos de un proyecto](media/choose-spatializer.png)

A continuación, abra el Audio de Mixer (**Ventana > Audio Mixer**). Asegúrese de tener al menos un mezclador, con un grupo. Si no es así, haga clic en el botón "+" que está a la derecha de **Mixers** (Mezcladores). Haga clic en la parte inferior de la franja de canal en la sección de efectos y agregue el **proyecto acústica Mixer** efecto. Tenga en cuenta que solo se admite un mezclador de Elementos acústicos de un proyecto a la vez.

![Mezclador de audio](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Habilitar acústica en orígenes de sonido
Cree un origen de sonido. Active la casilla de verificación en la parte inferior del panel de inspector de AudioSource que dice **Spatialize** (Espacializar). Asegúrese de que **Spatial Blend** (Combinación espacial) se establece en 3D completo.  

![Origen de audio](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Habilitar diseño acústico
Adjuntar la secuencia de comandos **AcousticsAdjust** a un origen de sonido en su escena para habilitar parámetros de origen adicionales de diseño, haga clic en **Agregar componente** y eligiendo **Scripts > Ajustar acústica** :

![AcousticsAdjust](media/acoustics-adjust.png)

## <a name="next-steps"></a>Pasos siguientes
* [Integrar la escena con acústica de proyecto de Unity](unity-baking.md)
* [Crear una cuenta de Azure Batch](create-azure-account.md) para integrar la escena en la nube
* Explore la [proceso de diseño del proyecto acústica Unity](unity-workflow.md).

