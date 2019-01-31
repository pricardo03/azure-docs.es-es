---
title: ¿Qué son los elementos acústicos de un proyecto?
titlesuffix: Azure Cognitive Services
description: El complemento Project Acoustics para Unity proporciona oclusión, reverberación y espacialización para proyectos destinados a realidad virtual y pantallas tradicionales.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 175203269f524f5c680345602e6382f5c26f1bd1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178909"
---
# <a name="what-is-project-acoustics"></a>¿Qué son los elementos acústicos de un proyecto?
El complemento Project Acoustics para Unity proporciona oclusión, reverberación y espacialización para proyectos destinados a realidad virtual y pantallas tradicionales. Ofrece una manera de diseñar acústicas para juegos que establece en capas las intenciones del diseñador mediante una simulación de onda basada en leyes físicas.

## <a name="why-use-acoustics-in-virtual-environments"></a>¿Por qué usar la acústica en entornos virtuales?
Los seres humanos usan indicaciones audiovisuales para comprender el mundo que les rodea. En los mundos virtuales, la combinación de audio espacial con la acústica aumenta la inmersión del usuario. La herramienta de acústica descrita aquí analiza mundos virtuales para crear una simulación acústica realista y admite un proceso de diseño posterior a la simulación. El análisis incluye la geometría y los materiales de cada superficie del mundo. La simulación incluye parámetros como la dirección de llegada (portaling), la potencia de reverberación, los tiempos de decadencia y los efectos de oclusión y obstrucción.

## <a name="how-does-this-approach-to-acoustics-work"></a>¿Cómo funciona esta aproximación a la acústica?
El sistema se basa en un procesamiento del mundo virtual sin conexión, lo que permite una simulación más compleja que si el análisis se realiza en tiempo de ejecución. El procesamiento sin conexión genera una tabla de búsqueda de parámetros acústicos. Un diseñador especifica las reglas que se aplican a los parámetros en tiempo de ejecución. El ajuste de estas reglas puede lograr efectos hiperrealistas de alta intensidad emocional o escenas hiporrealistas para obtener sonidos de audio más en segundo plano.

## <a name="design-process-comparison"></a>Comparativa del proceso de diseño
El complemento Project Acoustics admite un nuevo proceso de diseño para la acústica de escenas de Unity. Para explicar este nuevo proceso de diseño, vamos a compararlo con uno de los enfoques más populares de hoy en día en acústica.

### <a name="typical-approach-to-acoustics-today"></a>Enfoque típico para acústica en la actualidad
En un enfoque habitual para la acústica hoy en día, se dibujan volúmenes de reverberación:

![Vista de diseño](media/reverbZonesAltSPace2.png)

A continuación, se ajustan los parámetros de cada zona:

![Vista de diseño](media/TooManyReverbParameters.png)

Por último, se agrega lógica de trazas de rayos para obtener los filtrados de oclusión y obstrucción correctos en toda la escena y la lógica de búsqueda de rutas de portaling. Este código puede aumentar el tiempo de ejecución. También tiene problemas con la suavidad alrededor de las esquinas y tiene casos extremos en escenas con formas irregulares.

### <a name="an-alternative-approach-with-physics-based-design"></a>Enfoque alternativo con un diseño basado en leyes físicas
Con el enfoque que proporciona el complemento Project Acoustics para Unity, se proporcionan la forma y los materiales de una escena estática. Dado que la escena se voxeliza y el proceso no usa trazas de rayos, no es necesario proporcionar una malla acústica estanca o simplificada. Tampoco es necesario marcar la escena con volúmenes de reverberación. El complemento carga la escena en la nube, donde se utiliza la simulación de onda basada en leyes físicas. El resultado se integra en el proyecto como una tabla de búsqueda y se puede modificar para los efectos de estética o de juego.

![Vista de diseño](media/GearsWithVoxels.jpg)

## <a name="requirements"></a>Requisitos
* Unity 2018.2+ para preparativos acústicos y Unity 5.2+ para diseño de sonido e implementación
* Windows 64-bit Unity Editor
* Suscripción de Azure Batch para preparativos acústicos
* El entorno de ejecución de scripting de Unity se debe establecer en ".NET 4.x equivalent"

## <a name="platform-support"></a>Compatibilidad con plataformas
* Escritorio de Windows (x86 y AMD64)
* Plataforma universal de Windows (x86, AMD64 y ARM)
* Android (x86 y ARM64)

## <a name="download"></a>Descargar
Si está interesado en evaluar el complemento de acústica, se puede registrar [aquí](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) para unirse a la versión preliminar del diseñador.

## <a name="next-steps"></a>Pasos siguientes
* Más información sobre el [proceso de diseño](design-process.md)
* Introducción a la [integración de la acústica en un proyecto de Unity](getting-started.md)

