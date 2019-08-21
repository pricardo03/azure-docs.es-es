---
title: Inicio rápido de Project Acoustics con Unreal
titlesuffix: Azure Cognitive Services
description: Con el contenido de ejemplo, experimente con los controles de diseño de Project Acoustics en Unreal y Wwise y realice la implementación en Windows Desktop.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 927ce403130460c302f546038ff3a0c3a16e0368
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933007"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Inicio rápido de Project Acoustics en Unreal o Wwise
En este inicio rápido, experimentará con los controles de diseño de Project Acoustics con el contenido de ejemplo proporcionado para Unreal Engine y Wwise.

Requisitos de software para el uso de contenido de ejemplo:
* [Unreal Engine](https://www.unrealengine.com/) 4.22
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2019.1.2

## <a name="download-the-sample-package"></a>Descarga del paquete de ejemplo
Descargue el [paquete de ejemplo Project Acoustics Unreal + Wwise](https://www.microsoft.com/download/details.aspx?id=58090). El paquete de ejemplo contiene un proyecto de Unreal Engine, el proyecto de Wwise para ese proyecto de Unreal y el complemento Wwise para Project Acoustics.

## <a name="set-up-the-project-acoustics-sample-project"></a>Configuración del proyecto de ejemplo de Project Acoustics
Para configurar el proyecto de ejemplo de Project Acoustics en Unreal o Wwise, deberá instalar primero el complemento de Project Acoustics en Wwise. A continuación, implemente los archivos binarios de Wwise en el proyecto de Unreal y ajuste el complemento Unreal de Wwise para admitir Project Acoustics.

### <a name="install-the-project-acoustics-wwise-plugin"></a>Instalación del complemento Wwise para Project Acoustics
Abra Wwise Launcher y, en la pestaña **Plugins** (Complementos), en **Install New Plugins** (Instalar nuevos complementos), seleccione **Add From Directory** (Agregar del directorio). Elija el directorio `AcousticsWwisePlugin\ProjectAcoustics` que se incluyó en el paquete descargado.

![Captura de pantalla de Wwise Launcher con la opción de instalación del complemento Wwise](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Adición de archivos binarios de Wwise al proyecto de ejemplo de Unreal para Project Acoustics
En Wwise Launcher, haga clic en la pestaña **Unreal Engine**, haga clic en el menú lateral junto a **Recent Unreal Engine Projects** (Proyectos de Unreal Engine recientes) y seleccione **Browse for project** (Examinar proyecto). Abra el archivo `.uproject` del proyecto de Unreal de ejemplo en el paquete `AcousticsSample\AcousticsGame\AcousticsGame.uproject`.

![Captura de pantalla de la pestaña Unreal de Wwise Launcher](media/wwise-unreal-tab.png)

A continuación, junto al proyecto de ejemplo de Project Acoustics, haga clic en **Integrate Wwise in Project** (Integrar Wwise en el proyecto).

![Captura de pantalla de Wwise Launcher que muestra el proyecto Unreal de acústica de juegos](media/wwise-acoustics-game-project.png)

### <a name="extend-wwises-unreal-plugin-functionality"></a>Ampliación de la funcionalidad del complemento Unreal de Wwise
El complemento Unreal de Project Acoustics requiere la exposición de comportamiento adicional de la API del complemento Unreal de Wwise. Ejecute el archivo por lotes proporcionado con el complemento Unreal de Project Acoustics para automatizar estas modificaciones:
* Dentro de `AcousticsGame\Plugins\ProjectAcoustics\Resources`, ejecute `PatchWwise.bat`.

    ![Captura de pantalla de una ventana del Explorador de Windows que muestra el proyecto de revisión del script de Wwise](media/patch-wwise-script.png)

* Si no tiene instalado el SDK de DirectX, dependiendo de la versión de Wwise que utilice, es posible que tenga que convertir en comentario la línea que contiene `DXSDK_DIR` en `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`:

    ![Captura de pantalla del editor de código que muestra DXSDK convertido en comentario](media/directx-sdk-comment.png)

* Si compila con Visual Studio 2019, para solucionar un error de vinculación con Wwise, cambie de forma manual el valor `VSVersion` predeterminado en `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs` a `vc150`:

    ![Captura de pantalla del editor de código donde se muestra el cambio de VSVersion a vc150](media/vsversion-comment.png)

### <a name="open-the-unreal-project"></a>Abra el proyecto de Unreal. 
Le pide que vuelva a generar los módulos; diga que sí.

>Si no se puede abrir el proyecto por errores de compilación, compruebe que ha instalado el complemento Wwise de Project Acoustics en la misma versión de Wwise usada en el proyecto de ejemplo de Project Acoustics.

>Si usa una versión de [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) anterior a 2019.1, no podrá generar bancos de sonidos con el proyecto de ejemplo Project Acoustics.  Es necesario integrar Wwise versión 2019.1 en el proyecto de ejemplo para usarlo correctamente.

## <a name="experiment-with-project-acoustics-design-controls"></a>Experimentación con los controles de diseño de Project Acoustics
Para escuchar cómo suena la escena, haga clic en el botón de reproducción en el editor de Unreal. En el escritorio, use W, A, S, D y el mouse para desplazarse. Para ver los métodos abreviados de teclado de otros controles, presione **F1**. Estas son algunas actividades de diseño que puede probar:

### <a name="modify-occlusion-and-transmission"></a>Modificación de la oclusión y la transmisión
En cada actor de sonido de Unreal, hay controles de diseño de Project Acoustics por cada fuente:

![Captura de pantalla de los controles de diseño de acústica del editor de Unreal](media/demo-scene-sound-source-design-controls.png)

Si el multiplicador de **oclusión** es mayor que 1 (el valor predeterminado es 1), se exagera la oclusión. Si se establece en menos de 1, el efecto de oclusión se hace más sutil.

Para permitir la transmisión a través de la pared, mueva el control deslizante de **transmisión (dB)** a la posición de desactivación, su nivel más bajo. 

### <a name="modify-wetness-for-a-source"></a>Modificación del sonido con efectos de una fuente
Para cambiar cómo el sonido con efectos varía ligeramente con la distancia, use **Perceptual Distance Warp** (Ajuste del salto perceptual). Project Acoustics calcula los niveles de efecto a lo largo del espacio de simulación, que varía ligeramente con la distancia y proporciona indicios de la distancia perceptual. Al aumentar el salto de distancia se exagera este efecto ya que aumentan los niveles de efectos relacionados con la distancia. Los valores de distorsión inferiores a 1 hacen que el cambio en la reverberación basada en la distancia sea más sutil. Este efecto también se puede ajustar de forma más precisa mediante el ajuste del **sonido con efectos (dB)** .

Aumente el tiempo de decadencia a lo largo del espacio mediante el ajuste del valor **Decay Time Scale** (Escala de tiempo de decadencia). Considere el caso donde el resultado de la simulación es un tiempo de decadencia de 1,5 s. Al establecer **Decay Time Scale** (Escala de tiempo de decadencia) en 2, el resultado será un tiempo de decadencia aplicado a la fuente de 3 s.

### <a name="modify-distance-based-attenuation"></a>Modificación de la atenuación basada en la distancia
El complemento mezclador de Wwise para Project Acoustics respeta la atenuación basada en la distancia por fuente creada en Wwise. Al cambiar esta curva, cambia el nivel de vía de sonido sin efectos. El complemento de Project Acoustics ajustará el nivel de sonido con efectos para mantener la mezcla efectos-sin efectos especificada por la simulación y los controles de diseño.

![Captura de pantalla del panel de curva de atenuación de Wwise con la atenuación a cero antes del límite de simulación](media/demo-sounds-attenuation.png)

Project Acoustics realiza el cálculo en un cuadro de "región de simulación" centrado alrededor de la ubicación de cada jugador simulado. Los recursos acústicos del paquete de ejemplo se simularon mediante "bake" con un radio en la región de simulación de 45 m, y las atenuaciones se diseñaron para descender hasta 0 antes de 45 m. Aunque este descenso no es un requisito estricto, tiene la salvedad de que solo la geometría a 45 m del oyente ocluirá los sonidos.

## <a name="next-steps"></a>Pasos siguientes
* [Integre el complemento de Project Acoustics](unreal-integration.md) en el proyecto de Unreal
* [Cree una cuenta de Azure](create-azure-account.md) para sus propias elaboraciones


