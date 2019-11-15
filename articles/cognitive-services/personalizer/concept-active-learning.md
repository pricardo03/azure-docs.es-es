---
title: 'Eventos activos e inactivos: Personalizer'
titleSuffix: Azure Cognitive Services
description: En este artículo se describe el uso de los eventos activos e inactivos, la configuración de aprendizaje y las directivas de aprendizaje en el servicio de Personalizer.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: 1641a1020193395d7d2ddb9c4893bd7bc89cdcd0
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681873"
---
# <a name="active-and-inactive-events"></a>Eventos activos e inactivos

Cuando la aplicación llama a la API Rank, recibe la acción que la aplicación debe mostrar en el campo **rewardActionId**.  A partir de ese momento, Personalizer espera una llamada de recompensa con el mismo eventId. La puntuación de la recompensas se usará para entrenar el modelo para las llamadas a Rank futuras. Si no se recibe ninguna llamada de recompensa para el eventId, se aplica una recompensa predeterminada. Las recompensas predeterminadas se configuran en Azure Portal.

En algunos escenarios, es posible que la aplicación tenga que llamar a Rank, incluso antes de saber si el resultado se va a usar o a mostrar al usuario. Esto puede ocurrir en situaciones donde, por ejemplo, la representación de la página del contenido promocionado se sobrescribe con una campaña de marketing. Si el resultado de la llamada a Rank no se usó nunca y el usuario nunca lo vio, no envíe la llamada de recompensa correspondiente.

Normalmente, estos escenarios se producen cuando:

* Está preprocesando la interfaz de usuario que el usuario podría o no ver. 
* La aplicación está haciendo una personalización predictiva en la que las llamadas de Rank se realizan con poco contexto en tiempo real y la aplicación puede utilizar o no la salida. 

En estos casos, use Personalizer para llamar a Rank solicitando al evento que esté _inactivo_. Personalizer no esperará una recompensa para este evento y no aplicará ninguna predeterminada. Después, en la lógica de negocios, si la aplicación usa la información de la llamada a Rank, lo único que debe hacer es _activar_ el evento. En cuanto el evento está activo, Personalizer espera una recompensa de evento. Si no se realiza ninguna llamada explícita a la API Rank, Personalizer aplica una recompensa predeterminada.

## <a name="inactive-events"></a>Eventos inactivos

Para deshabilitar el entrenamiento de un evento, llame a Rank mediante `learningEnabled = False`. Para un evento inactivo, el aprendizaje se activa implícitamente si envía una recompensa para el eventId o si llama a la API `activate` para dicho eventId.

## <a name="learning-settings"></a>Configuración del aprendizaje

La configuración del aprendizaje determina los *hiperparámetros* del entrenamiento de modelo. Dos modelos de los mismos datos que se entrenan con configuraciones de aprendizaje diferentes se comportarán de manera distinta.

### <a name="import-and-export-learning-policies"></a>Importación y exportación de directivas de aprendizaje

Puede importar y exportar archivos de directivas de aprendizaje desde Azure Portal. Use este método para guardar las directivas existentes, probarlas, reemplazarlas y archivarlas en el control de código fuente como artefactos para futuras referencias y auditorías.

### <a name="understand-learning-policy-settings"></a>Comprensión de la configuración de directivas de aprendizaje

La configuración de la directiva de aprendizaje no está pensada para modificarse. Cambie la configuración solo si comprende cómo afecta a Personalizer. Sin este conocimiento, podría causar problemas, como la invalidación de los modelos de Personalizer.

### <a name="compare-learning-policies"></a>Comparación de directivas de aprendizaje

Puede comparar el rendimiento de las diferentes directivas de aprendizaje con datos anteriores en los registros de Personalizer mediante [evaluaciones sin conexión](concepts-offline-evaluation.md).

[Cargue sus propias directivas de aprendizaje](how-to-offline-evaluation.md) para compararlas con la directiva actual de aprendizaje.

### <a name="optimize-learning-policies"></a>Optimización de directivas de aprendizaje

Personalizer puede crear una directiva de aprendizaje optimizada en una [evaluación sin conexión](how-to-offline-evaluation.md). Una directiva de aprendizaje optimizada que tiene mejores recompensas en una evaluación sin conexión, tendrá mejores resultados cuando se utilice en línea en Personalizer.

Después de optimizar una directiva de aprendizaje, puede aplicarla directamente en Personalizer para que reemplace inmediatamente a la directiva actual. También puede guardar la directiva optimizada para su evaluación adicional y, posteriormente, decidir si descartarla, guardarla o aplicarla.
