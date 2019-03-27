---
title: Introducción a Project Acoustics
titlesuffix: Azure Cognitive Services
description: Project Acoustics es un motor de acústica para experiencias interactivas en 3D, que integra simulación de física de ondas mediante "bake" con controles de diseño interactivo.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 3d99ea5767c7b2e62f7228440201b4a9b6593b02
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58136607"
---
# <a name="what-is-project-acoustics"></a>¿Qué son los elementos acústicos de un proyecto?
Project Acoustics es un motor de acústica de ondas para experiencias interactivas en 3D. Modela los efectos de onda como los de difracción, portaling y reverberación en escenas complejas sin necesidad de marcado manual de la zona. También incluye la integración de middleware de audio y motor de juego. La idea detrás de Project Acoustics es similar a la iluminación estática: simular mediante "bake" y sin conexión, características físicas en detalle para proporcionar una base de referencia física y usar un tiempo de ejecución ligero con controles de diseño expresivo para lograr objetivos artísticos.

![Vista de diseño](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Uso de física de ondas para acústica interactiva
Los métodos acústicos basados en rayos pueden comprobar si hay oclusión mediante el lanzamiento de un único rayo desde el origen al oyente, o manejar la reverberación mediante la estimación del volumen de la escena local con unos pocos rayos. Pero estas técnicas pueden ser poco confiables, porque un guijarro causa la misma oclusión que un peñasco. Los rayos no cuenta con la forma en la que el sonido se desvía al encontrarse con diferentes objetos, un fenómeno conocido como difracción. La simulación de Project Acoustics captura estos efectos utilizando una simulación basada en las ondas. El resultado es más predecible y confiable.

La innovación clave de Project Acoustics es que une la simulación acústica con conceptos de diseño de sonido tradicionales. Traduce los resultados de la simulación en parámetros DSP de audio tradicionales para los efectos de oclusión, portaling y reverberación. El diseñador utiliza controles sobre este proceso de traducción. Para más detalles sobre las tecnologías principales que hay detrás de Project Acoustics, visite la [página del proyecto de investigación](https://www.microsoft.com/en-us/research/project/project-triton/).

![Vista de diseño](media/wave-simulation.gif)

## <a name="setup"></a>Configuración
[Integración de Project Acoustics con Unitiy](unity-integration.md) es un recurso de arrastrar y colocar, e incluye un complemento de motor de audio de Unity. Aumente los controles de origen de audio de Unity adjuntando un componente de controles C# de Project Acoustics a cada objeto de audio.

[Integración de Project Acoustics con Unreal](unreal-integration.md) incluye complementos de editor y juegos para Unreal y un complemento de mezclador Wwise. Un componente de audio personalizado amplía la funcionalidad de Wwise familiar dentro de Unreal con controles de diseño acústico en directo. Los controles de diseño también se exponen en Wwise en el complemento del mezclador.

## <a name="workflow"></a>Flujo de trabajo
* **Diseño previo a la simulación mediante "bake":** Empiece con la configuración de la simulación mediante "bake" seleccionando qué geometría responde a la acústica, por ejemplo omitiendo los huecos pequeños. A continuación, modifique las asignaciones automáticas de material y seleccione las áreas de navegación para guiar el muestreo del oyente. No hay marcado manual para las zonas reverberación/portal/sala.
* **Simulación acústica mediante "bake":** Se ejecuta localmente un paso de análisis, que realiza una voxelización y otros análisis geométricos en la escena en base a las selecciones anteriores. Los resultados se visualizan en el editor para comprobar la configuración de la escena. Al enviar la simulación mediante "bake", se envían a Azure los datos de vóxel y se le enviará de vuelta un recurso de acústica de juego.
* **Tiempo de ejecución:** Cargue el recurso en su nivel y ya estará listo para escuchar los recursos acústicos en su nivel. Diseñe la acústica en vivo en el editor con los controles pormenorizados por origen. También se pueden manejar los controles mediante la creación de scripts de nivel.

## <a name="platforms"></a>Plataformas
Los complementos de tiempo de ejecución de Project Acoustics se pueden implementar actualmente en las siguientes plataformas:
*  Windows
* Android
* Xbox One

## <a name="download"></a>Descargar
* [Complementos y muestras de Project Acoustics](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
  * Para los archivos binarios de Xbox y soporte técnico, póngase en contacto con nosotros mediante el formulario de registro a continuación
* [Foros de Project Acoustics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics)
* [Regístrese para recibir las actualizaciones sobre Project Acoustics](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u)

## <a name="next-steps"></a>Pasos siguientes
* Pruebe un [Inicio rápido de Project Acoustics para Unity](unity-quickstart.md) o para [Unreal](unreal-quickstart.md)
* Explore la [filosofía de diseño de Project Acoustics](design-process.md)

