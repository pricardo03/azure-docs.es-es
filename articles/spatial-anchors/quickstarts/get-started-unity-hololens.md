---
title: 'Inicio rápido: Creación de una aplicación HoloLens en Unitiy con Azure Spatial Anchors | Microsoft Docs'
description: En este inicio rápido, aprenderá a crear una aplicación HoloLens en Unity con Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: b2bfec47bc92ebf5db1561d8fca33940dc376866
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752947"
---
# <a name="quickstart-create-a-hololens-unity-app-using-azure-spatial-anchors"></a>Inicio rápido: Creación de una aplicación HoloLens en Unity con Azure Spatial Anchors

En este inicio rápido se describe cómo crear una aplicación HoloLens en Unity con [Azure Spatial Anchors](../overview.md). Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación HoloLens compilada con Unity que puede guardar y recuperar un delimitador espacial.

Aprenderá a:

> [!div class="checklist"]
> * Crear una cuenta de Spatial Anchors
> * Preparar la configuración de compilación de Unity
> * Configurar la clave y el identificador de la cuenta de Spatial Anchors
> * Exportar el proyecto de HoloLens de Visual Studio
> * Realizar implementaciones y ejecuciones en un dispositivo HoloLens

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

- Una máquina Windows con <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 o versiones posteriores</a> y <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 o versiones posteriores</a> instalado con la carga de trabajo **desarrollo de Plataforma universal de Windows**.
- Un dispositivo HoloLens con el [modo de desarrollador](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) habilitado.
- La aplicación debe establecer la funcionalidad **SpatialPerception** en **Build Settings**->**Player Settings**->**Publishing Settings**->**Capabilities** (Configuración de compilación > Configuración del reproductor > Configuración de publicación > Funcionalidades).
- La aplicación debe habilitar **Virtual Reality Supported** (Realidad virtual admitida) con **Windows Mixed Reality SDK** en **Build Settings**->**Player Settings**->**XR Settings** (Configuración de compilación > Configuración del reproductor > Configuración de XR).

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Apertura del proyecto de ejemplo en Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Abra Unity y abra el proyecto en la carpeta `Unity`.

Seleccione **File** -> **Build Settings** (Archivo > Configuración de compilación) para abrir **Build Settings** (Configuración de compilación).

En la sección **Platform** (Plataforma), seleccione **Universal Windows Platform** (Plataforma universal de Windows). Después, cambie **Target Device** (Dispositivo de destino) a **HoloLens**.

Seleccione **Switch Platform** (Cambiar plataforma) para cambiar la plataforma a **Universal Windows Platform** (Plataforma universal de Windows).

![Configuración de compilación de Unity](./media/get-started-unity-hololens/unity-build-settings.png)

Cierre la ventana **Build Settings** (Configuración de compilación).

## <a name="configure-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

En el panel **Project** (Proyecto), vaya a `Assets/AzureSpatialAnchorsPlugin/Examples` y abra el archivo de escena `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Para guardar la escena, seleccione **File** -> **Save** (Archivo > Guardar).

## <a name="export-the-hololens-visual-studio-project"></a>Exportar el proyecto de HoloLens de Visual Studio

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Seleccione **Compilar** para abrir un cuadro de diálogo. A continuación, seleccione una carpeta para exportar el proyecto HoloLens de Visual Studio.

Una vez completada la exportación, se mostrará una carpeta que contiene el proyecto HoloLens exportado.

## <a name="deploy-the-hololens-application"></a>Implementación de la aplicación HoloLens

En la carpeta, haga doble clic en `HelloAR U3D.sln` para abrir el proyecto en Visual Studio.

Cambiar **Configuración de la solución** a **Versión**, cambie **Plataforma de la solución** a **x86** y seleccione **Dispositivo**  en las opciones de destino de implementación.

![Configuración de Visual Studio](./media/get-started-unity-hololens/visual-studio-configuration.png)

Encienda el dispositivo HoloLens, inicie sesión y conéctelo al equipo mediante un cable USB.

Seleccione **Depurar** > **Iniciar depuración** para implementar la aplicación y comenzar la depuración.

Siga las instrucciones de la aplicación para colocar y recuperar un delimitador.

En Visual Studio, seleccione **Detener depuración** o presione **Mayús + F5** para detener la depuración.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](../tutorials/tutorial-share-anchors-across-devices.md)
