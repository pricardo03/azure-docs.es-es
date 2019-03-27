---
title: Inicio rápido de Project Acoustics con Unreal
titlesuffix: Azure Cognitive Services
description: Con el contenido de ejemplo, experimente con los controles de diseño de Project Acoustics en Unreal y Wwise y realice la implementación en Windows Desktop.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/14/2019
ms.author: kegodin
ms.openlocfilehash: 1314e393d292145ef112e700abf6ab1ef199db7d
ms.sourcegitcommit: f68b0e128f0478444740172f54e92b453df696be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58138174"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Inicio rápido de Project Acoustics en Unreal o Wwise
En este inicio rápido, experimentará con los controles de diseño de Project Acoustics con el contenido de ejemplo proporcionado para Unreal Engine y Wwise.

Requisitos de software:
* [Unreal Engine 4.21](https://www.unrealengine.com/)
* [Wwise 2018.1.6](https://www.audiokinetic.com/products/wwise/)

## <a name="download-the-sample-package"></a>Descarga del paquete de ejemplo
Descargue el [paquete de ejemplo Project Acoustics Unreal + Wwise](http://www.microsoft.com/downloads/details.aspx?FamilyID=f03dff5a-5780-462e-87ef-e6d039d0748d). El paquete de ejemplo contiene un proyecto de Unreal Engine, el proyecto de Wwise para ese proyecto de Unreal y el complemento Wwise para Project Acoustics.

## <a name="set-up-the-project-acoustics-sample-project"></a>Configuración del proyecto de ejemplo de Project Acoustics
Para configurar el proyecto de ejemplo de Project Acoustics en Unreal o Wwise, deberá instalar primero el complemento de Project Acoustics en Wwise. A continuación, implemente los archivos binarios de Wwise en el proyecto de Unreal y ajuste el complemento Unreal de Wwise para admitir Project Acoustics.

### <a name="install-the-project-acoustics-wwise-plugin"></a>Instalación del complemento Wwise para Project Acoustics
Abra Wwise Launcher y, en la pestaña **Plugins** (Complementos), en **Install New Plugins** (Instalar nuevos complementos), seleccione **Add From Directory** (Agregar del directorio). Elija el directorio `AcousticsWwisePlugin\ProjectAcoustics` que se incluyó en el paquete descargado.

![Instalación del complemento Wwise](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Adición de archivos binarios de Wwise al proyecto de ejemplo de Unreal para Project Acoustics
En Wwise Launcher, haga clic en la pestaña **Unreal Engine**, haga clic en el menú lateral junto a **Recent Unreal Engine Projects** (Proyectos de Unreal Engine recientes) y seleccione **Browse for project** (Examinar proyecto). Abra el archivo `.uproject` del proyecto de Unreal de ejemplo en el paquete `AcousticsSample\AcousticsGame\AcousticsGame.uproject`.

![Pestaña Unreal de Wwise](media/wwise-unreal-tab.png)

A continuación, junto al proyecto de ejemplo de Project Acoustics, haga clic en **Integrate Wwise in Project** (Integrar Wwise en el proyecto).

![Proyecto Unreal de acústica de juegos en Wwise](media/wwise-acoustics-game-project.png)

### <a name="extend-wwises-unreal-plugin-functionality"></a>Ampliación de la funcionalidad del complemento Unreal de Wwise
El complemento Unreal de Project Acoustics requiere la exposición de comportamiento adicional de la API del complemento Unreal de Wwise. Ejecute el archivo por lotes proporcionado con el complemento Unreal de Project Acoustics para automatizar estas modificaciones:
* Dentro de `AcousticsGame\Plugins\ProjectAcoustics\Resources`, ejecute `PatchWwise.bat`.

    ![Revisión del script de Wwise](media/patch-wwise-script.png)

* Si no tiene instalado el SDK de DirectX, deberá convertir en comentario la línea que contiene DXSDK_DIR en `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`.

    ![Convertir en comentario DXSDK](media/directx-sdk-comment.png)

### <a name="open-the-unreal-project"></a>Abra el proyecto de Unreal. 
Le pide que vuelva a generar los módulos; diga que sí.

Si no se puede abrir el proyecto por errores de compilación, compruebe que ha instalado el complemento Wwise de Project Acoustics en la misma versión de Wwise usada en el proyecto de ejemplo de Project Acoustics.

## <a name="experiment-with-project-acoustics-design-controls"></a>Experimentación con los controles de diseño de Project Acoustics
Para escuchar cómo suena la escena, haga clic en el botón de reproducción en el editor de Unreal. En el escritorio, use W, A, S, D y el mouse para desplazarse. Para ver los métodos abreviados de teclado de otros controles, presione **F1**. Estas son algunas actividades de diseño que puede probar:

### <a name="modify-occlusion-and-transmission"></a>Modificación de la oclusión y la transmisión
En cada actor de sonido de Unreal, hay controles de diseño de Project Acoustics por cada fuente:

![DemoSceneSoundSourceDesignControls](media/demo-scene-sound-source-design-controls.png)

Si el multiplicador de **oclusión** es mayor que 1 (el valor predeterminado es 1), se exagera la oclusión. Si se establece en menos de 1, el efecto de oclusión se hace más sutil.

Para permitir la transmisión a través de la pared, mueva el control deslizante de **transmisión (dB)** a la posición de desactivación, su nivel más bajo. 

### <a name="modify-wetness-for-a-source"></a>Modificación del sonido con efectos de una fuente
Para cambiar cómo el sonido con efectos varía ligeramente con la distancia, use **Perceptual Distance Warp** (Ajuste del salto perceptual). Project Acoustics calcula los niveles de efecto a lo largo del espacio de simulación, que varía ligeramente con la distancia y proporciona indicios de la distancia perceptual. Al aumentar el salto de distancia se exagera este efecto ya que aumentan los niveles de efectos relacionados con la distancia. Los valores de distorsión inferiores a 1 hacen que el cambio en la reverberación basada en la distancia sea más sutil. Este efecto también se puede ajustar de forma más precisa mediante el ajuste del **sonido con efectos (dB)**.

Aumente el tiempo de decadencia a lo largo del espacio mediante el ajuste del valor **Decay Time Scale** (Escala de tiempo de decadencia). Considere el caso donde el resultado de la simulación es un tiempo de decadencia de 1,5 s. Al establecer **Decay Time Scale** (Escala de tiempo de decadencia) en 2, el resultado será un tiempo de decadencia aplicado a la fuente de 3 s.

### <a name="modify-distance-based-attenuation"></a>Modificación de la atenuación basada en la distancia
El complemento mezclador de Wwise para Project Acoustics respeta la atenuación basada en la distancia por fuente creada en Wwise. Al cambiar esta curva, cambia el nivel de vía de sonido sin efectos. El complemento de Project Acoustics ajustará el nivel de sonido con efectos para mantener la mezcla efectos-sin efectos especificada por la simulación y los controles de diseño.

![DemoSoundsAttenuation](media/demo-sounds-attenuation.png)

Project Acoustics realiza el cálculo en un cuadro de "región de simulación" centrado alrededor de la ubicación de cada jugador simulado. Los recursos acústicos del paquete de ejemplo se simularon mediante "bake" con un radio en la región de simulación de 45 m, y las atenuaciones se diseñaron para descender hasta 0 antes de 45 m. Aunque este descenso no es un requisito estricto, tiene la salvedad de que solo la geometría a 45 m del oyente ocluirá los sonidos.

## <a name="next-steps"></a>Pasos siguientes
* [Integre el complemento de Project Acoustics](unreal-integration.md) en el proyecto de Unreal
* [Cree una cuenta de Azure](create-azure-account.md) para sus propias elaboraciones


