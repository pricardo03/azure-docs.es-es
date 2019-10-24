---
title: 'Eventos activos e inactivos: Personalizer'
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: aa6f53901f21dcb0726454d641a4a2a66007f9e0
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72429036"
---
# <a name="active-and-inactive-events"></a>Eventos activos e inactivos

Cuando la aplicación llama a la API Rank, recibe la acción que la aplicación debe mostrar en el campo rewardActionId.  A partir de ese momento, Personalizer esperará una llamada de recompensa con el mismo eventId. La puntuación de la recompensas se usará para entrenar el modelo que se empleará para las llamadas a Rank futuras. Si no se recibe ninguna llamada de recompensa para el eventId, se aplicará una recompensa predeterminada. Las recompensas predeterminadas se establecen en Azure Portal.

En algunos casos, es posible que la aplicación tenga que llamar a Rank, incluso para saber si el resultado se va a usar o a mostrar al usuario. Esto puede ocurrir en situaciones donde, por ejemplo, la representación de la página del contenido promocionado se sobrescriba con una campaña de marketing. Si el resultado de la llamada a Rank no se ha usado nunca y el usuario nunca lo ve, sería incorrecto entrenarlo con cualquier recompensa en los resultados todo, cero o de lo contrario.
Normalmente esto sucede cuando:

* Es posible que se pueda representar previamente alguna interfaz de usuario que el usuario pueda o no ver. 
* La aplicación puede estar haciendo una personalización predictiva en la que las llamadas de Rank se realizan con menos contexto en tiempo real y su salida puede utilizarse o no por la aplicación. 

En estos casos, la manera correcta de usar Personalizer es llamar a Rank solicitando al evento que esté _inactivo_. Personalizer no esperará una recompensa para este evento y no se aplicará ningún predeterminada. Después, en la lógica de negocios, si la aplicación usa la información de la llamada de rango, lo único que debe hacer es _activar_ el evento. Desde el momento en que el evento está activo, Personalizer esperará una recompensa para el evento o aplicará una predeterminada si no se realiza ninguna llamada explícita a la API de recompensa.

## <a name="get-inactive-events"></a>Obtención de eventos inactivos

Para deshabilitar el entrenamiento de un evento, llame a Rank con `learningEnabled = False`.

El aprendizaje para un evento inactivo se activa implícitamente si envía una recompensa para el eventId o si llama a la API `activate` para dicho eventId.

## <a name="learning-settings"></a>Configuración del aprendizaje

La configuración del aprendizaje determina los *hiperparámetros* específicos del entrenamiento de modelo. Dos modelos de los mismos datos, entrenados con configuraciones de aprendizaje diferentes se comportarán de manera distinta.

### <a name="import-and-export-learning-policies"></a>Importación y exportación de directivas de aprendizaje

Puede importar y exportar archivos de directivas de aprendizaje desde Azure Portal. Esto permite guardar las directivas existentes, probarlas, reemplazarlas y archivarlas en el control de código fuente como artefactos para futuras referencias y auditorías.

### <a name="learning-policy-settings"></a>Configuración de directivas de aprendizaje

La configuración de la **directiva de aprendizaje** no está pensada para modificarse. Solo debe cambiar la configuración cuando comprenda de qué manera puede afectar a Personalizer. Cambiar la configuración sin este conocimiento causará efectos secundarios, incluida la invalidación de los modelos de Personalizer.

### <a name="comparing-effectiveness-of-learning-policies"></a>Comparación de la eficacia de las directivas de aprendizaje

Puede comparar el rendimiento de las diferentes directivas de aprendizaje con los datos anteriores en los registros de Personalizer mediante [evaluaciones sin conexión](concepts-offline-evaluation.md).

[Cargue sus propias directivas de aprendizaje](how-to-offline-evaluation.md) para compararlas con la directiva actual de aprendizaje.

### <a name="discovery-of-optimized-learning-policies"></a>Detección de directivas de aprendizaje optimizadas

Personalizer puede crear una directiva de aprendizaje más optimizada cuando se realiza una [evaluación sin conexión](how-to-offline-evaluation.md). Una directiva de aprendizaje más optimizada, que ha demostrado tener mejores recompensas en una evaluación sin conexión, tendrá mejores resultados cuando se utilice en línea en Personalizer.

Después de crear una directiva de aprendizaje optimizada, puede aplicarla directamente a Personalizer para que reemplace a la directiva actual inmediatamente, o puede guardarla para una evaluación posterior y decidir en el futuro si desea descartarla, guardarla o aplicarla más tarde.
