---
title: Integración e implementación de Unity para Project Acoustics
titlesuffix: Azure Cognitive Services
description: Se explica la integración del complemento de Unity de Project Acoustics en el proyecto de Unity.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 2deddfd4e6c03b53306d8fbab3340dce464158b0
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612738"
---
# <a name="project-acoustics-unity-integration"></a>Integración de Unity con Project Acoustics
Se explica la integración del complemento de Unity de Project Acoustics en el proyecto de Unity.

Requisitos de software:
* [Unity 2018.2+](https://unity3d.com) para Windows
* [Paquete de Unity de Project Acoustics](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>Importación del complemento
Importe el paquete UnityPackage de acústica al proyecto. 
* En Unity, vaya a **Assets > Import Package > Custom Package...** (Recursos > Importar paquete > Paquete personalizado)

    ![Captura de pantalla del menú de importación de paquetes de Unity](media/import-package.png)  

* Elija **ProjectAcoustics.unitypackage**

Si va a importar el complemento a un proyecto existente, es posible que el proyecto ya tenga un archivo **mcs.rsp** en la raíz del proyecto, que especifica las opciones del compilador de C#. Deberá combinar el contenido de ese archivo con el archivo mcs.rsp que se incluye con el complemento de Elementos acústicos de un proyecto.

## <a name="enable-the-plugin"></a>Habilitación del complemento
La parte de elaboración del kit de herramientas de Elementos acústicos requiere la versión en tiempo de ejecución de scripting .NET 4.x. La importación del paquete actualizará la configuración del reproductor de Unity. Reinicie Unity para que esta configuración surta efecto.

![Captura de pantalla del panel de configuración del jugador de Unity](media/player-settings.png)

![Captura de pantalla del panel de configuración del jugador de Unity con .NET 4.5 seleccionado](media/net45.png)

## <a name="set-up-audio-dsp"></a>Configuración del DSP de audio
Project Acoustics incluye un DSP de audio en tiempo de ejecución que se integra en el marco del espacializador del motor de audio de Unity. Proporciona tanto espacialización de panoramización como basada en HRTF. Para habilitar el DSP de Project Acoustics, abra la configuración de audio de Unity con **Edit > Project Settings > Audio** (Editar > Configuración del proyecto > Audio) y, luego, seleccione **Project Acoustics** como **complemento espacializador** para el proyecto. Además, asegúrese de que el **tamaño de búfer de DSP** está establecido en el mejor rendimiento.

![Captura de pantalla del panel de configuración de proyectos de Unity](media/project-settings.png)  

![Captura de pantalla del panel de configuración del espacializador de Unity con el espacializador de Project Acoustics seleccionado](media/choose-spatializer.png)

A continuación, abra el mezclador de audio (**Window > Audio Mixer** [Ventana > Mezclador de audio]). Asegúrese de tener al menos un mezclador, con un grupo. Si no es así, haga clic en el botón "+" que está a la derecha de **Mixers** (Mezcladores). Haga clic con el botón derecho en la parte inferior de la fila de canales en la sección de efectos y agregue el efecto **Project Acoustics Mixer** (Mezclador de Project Acoustics). Tenga en cuenta que solo se admite un mezclador de Elementos acústicos de un proyecto a la vez.

![Captura de pantalla del mezclador de audio de Unity que aloja el mezclador de Project Acoustics](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Habilitación de la acústica en los orígenes de sonido
Cree un origen de sonido. Active la casilla de verificación en la parte inferior del panel de inspector de AudioSource que dice **Spatialize** (Espacializar). Asegúrese de que **Spatial Blend** (Combinación espacial) se establece en 3D completo.  

![Captura de pantalla del panel de fuente de audio de Unity](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Habilitación del diseño acústico
Adjunte el script **AcousticsAdjust** a una fuente de sonido de la escena para habilitar parámetros de diseño de fuente adicionales; para ello, haga clic en **Add Component** (Agregar componente) y elija **Scripts > Acoustics Adjust** (Scripts > Ajuste de acústica):

![Captura de pantalla del script AcousticsAdjust de Unity](media/acoustics-adjust.png)

## <a name="next-steps"></a>Pasos siguientes
* [Simulación mediante "bake" de la escena con Project Acoustics para Unity](unity-baking.md)
* [Creación de una cuenta de Azure Batch](create-azure-account.md) para integrar la escena en la nube
* Explore el [proceso de diseño de Unity para Project Acoustics](unity-workflow.md).

