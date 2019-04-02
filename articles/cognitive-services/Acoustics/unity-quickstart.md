---
title: Inicio rápido de Project Acoustics con Unity
titlesuffix: Azure Cognitive Services
description: Con el contenido de ejemplo, experimente con los controles de diseño de Project Acoustics en Unity y realice la implementación en Windows Desktop.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 468c5584d21c226d6ffce55ff3981e629d872c56
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317194"
---
# <a name="project-acoustics-unity-quickstart"></a>Inicio rápido de Project Acoustics con Unity
Use el contenido de ejemplo de Project Acoustics para Unity para experimentar con los controles de diseño basado en la simulación.

Requisitos de software:
* [Unity 2018.2+](http://unity3d.com) para Windows
* [Paquete de contenido de ejemplo de Project Acoustics](https://www.microsoft.com/download/details.aspx?id=57346)

¿Qué se incluye en el paquete de ejemplo?
* Escena de Unity con geometría, orígenes de sonido y controles de juego
* Complemento de Project Acoustics 
* Recursos acústicos de simulación mediante "bake" para la escena

## <a name="import-the-sample-package"></a>Importación del paquete de ejemplo
Importe el paquete de ejemplo a un nuevo proyecto de Unity. 
* En Unity, vaya a **Assets > Import Package > Custom Package...** (Recursos > Importar paquete > Paquete personalizado)

    ![Captura de pantalla de opciones de importación de paquete de Unity](media/import-package.png)  

* Elija **ProjectAcoustics.unitypackage**

Si va a importar el paquete en un proyecto existente, consulte [Integración con Unitiy](unity-integration.md) para pasos adicionales y notas.

## <a name="restart-unity"></a>Reinicio de Unity
La parte de elaboración del kit de herramientas de Elementos acústicos requiere la versión en tiempo de ejecución de scripting .NET 4.x. La importación del paquete actualizará la configuración del reproductor de Unity. Reinicie Unity para que esta configuración surta efecto.

Puede comprobar si este valor se ha hecho efectivo abriendo la **configuración del jugador**:

![Captura de pantalla del panel de configuración del jugador de Unity](media/player-settings.png)

![Captura de pantalla del panel de configuración del jugador de Unity con .NET 4.5 seleccionado](media/net45.png)

## <a name="experiment-with-design-controls"></a>Experimentación con los controles de diseño
Abra la escena de ejemplo en la carpeta **ProjectAcousticsSample** y haga clic en el botón de reproducción en el editor de Unity. Use W, A, S, D y el mouse para desplazarse. Para comparar cómo suena la escena con y sin acústica, presione el botón **R** hasta que el texto superpuesto cambie a color rojo en indique "Acoustics: Disabled" (Acústica: deshabilitada). Para ver los métodos abreviados de teclado de otros controles, presione **F1**. Todos los controles también se pueden usar si se hace clic con el botón derecho para seleccionar la acción que se va a realizar y luego se hace clic con el izquierdo para realizarla.

El script **AcousticsAdjust** se adjunta a los orígenes de sonido en la escena de ejemplo, lo que habilita los parámetros de diseño por origen. 

![Captura de pantalla del script AcousticsAdjust de Unity](media/acoustics-adjust.png)

A continuación se exploran algunos de los efectos que se pueden producir con los controles proporcionados. Para información detallada acerca de cada control, consulte el [Tutorial de diseño de Project Acoustics en Unity](unreal-workflow.md).

### <a name="modify-distance-based-attenuation"></a>Modificación de la atenuación basada en la distancia
El DSP de audio que proporciona el complemento espacializador de **Project Acoustics** para Unity respeta la atenuación basada en la distancia de cada origen, integrada en el editor de Unity. Los controles de la atenuación basada en la distancia están en el componente **Audio Source** (Origen de datos) que se encuentra en el panel **Inspector** de orígenes de sonido, en **3D Sound Settings** (Ajustes de sonido 3D):

![Captura de pantalla del panel de opciones de atenuación de distancia de Unity](media/distance-attenuation.png)

Project Acoustics realiza el cálculo en un cuadro de "región de simulación" centrado alrededor de la ubicación del jugador. Puesto que los recursos acústicos en el paquete de ejemplo se obtuvieron de una simulación mediante "bake" en la que el tamaño de la región de simulación era de 45 m alrededor del jugador, la atenuación del sonido debe diseñarse para caiga a 0 en aproximadamente 45 m.

### <a name="modify-occlusion-and-transmission"></a>Modificación de la oclusión y la transmisión
* Si el multiplicador de **oclusión** es mayor que 1 (el valor predeterminado es 1), se exagera la oclusión. Si se establece en menos de 1, el efecto de oclusión se hace más sutil.

* Para permitir la transmisión a través de la pared, mueva el control deslizante de **transmisión (dB)** a la posición de desactivación, su nivel más bajo. 

### <a name="modify-wetness-for-a-source"></a>Modificación del sonido con efectos de una fuente
* Para cambiar cómo el sonido con efectos varía ligeramente con la distancia, use **Perceptual Distance Warp** (Ajuste del salto perceptual). **Project Acoustics** calcula los niveles de efecto a lo largo del espacio de simulación, que varía ligeramente con la distancia y proporciona indicaciones de la distancia perceptual. Al aumentar el salto de distancia se exagera este efecto ya que aumentan los niveles de efectos relacionados con la distancia. Los valores de distorsión inferiores a 1 hacen que el cambio en la reverberación basada en la distancia sea más sutil. Este efecto también se puede ajustar de forma más precisa mediante el ajuste del **sonido con efectos (dB)**.

* Aumente el tiempo de decadencia a lo largo del espacio mediante el ajuste del valor **Decay Time Scale** (Escala de tiempo de decadencia). Si el resultado de la simulación de un determinado par de ubicación de oyente de origen es un tiempo de decadencia de 1,5 s y el valor **Decay Time Scale** (Escala de tiempo de decadencia) está establecido en 2, el tiempo de decadencia aplicado en el origen es 3 s.

## <a name="next-steps"></a>Pasos siguientes
* Lea la información con detalles completos sobre los [controles de diseño de Project Acoustics basados en Unity](unity-workflow.md)
* Explore los conceptos que subyacen al [proceso de diseño](design-process.md)
* [Cree una cuenta de Azure](create-azure-account.md) para explorar los procesos previos a la simulación mediante "bake" y la simulación misma

