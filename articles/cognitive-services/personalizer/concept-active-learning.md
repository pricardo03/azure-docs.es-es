---
title: 'Aprendizaje activo: Personalizer'
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: edjez
ms.openlocfilehash: c44afc81a7ec9d05cdc04cc8bc46c77cd51ceaf8
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722519"
---
# <a name="active-learning-and-learning-policies"></a>Aprendizaje activo y las directivas de aprendizaje 

Cuando la aplicación llama a la API Rank, recibe una clasificación del contenido. La lógica de negocios puede usar esta clasificación para determinar si el contenido debe mostrarse al usuario. Cuando muestra el contenido clasificado, significa que es un evento de clasificación _activo_. Cuando la aplicación no muestra el contenido clasificado, significa que es un evento de clasificación _inactivo_. 

La información del evento de clasificación activo se devuelve a Personalizer. Esta información se utiliza para continuar el entrenamiento del modelo mediante la directiva de aprendizaje actual.

## <a name="active-events"></a>Eventos activos

Los eventos activos siempre deben mostrarse al usuario y la llamada de recompensa debe devolverse para cerrar el ciclo de aprendizaje. 

### <a name="inactive-events"></a>Eventos inactivos 

Los eventos inactivos no deberían cambiar el modelo subyacente porque no se le dio al usuario la oportunidad de elegir entre el contenido clasificado.

## <a name="dont-train-with-inactive-rank-events"></a>No entrenar con eventos de clasificación inactivos 

En algunas aplicaciones, es posible que tenga que llamar a la API Rank sin saber aún si la aplicación mostrará los resultados al usuario. 

Esto ocurre cuando:

* Es posible que se pueda representar previamente alguna interfaz de usuario que el usuario pueda o no ver. 
* La aplicación puede estar haciendo una personalización predictiva en la que las llamadas de Rank se realizan con menos contexto en tiempo real y su salida puede utilizarse o no por la aplicación. 

### <a name="disable-active-learning-for-inactive-rank-events-during-rank-call"></a>Deshabilitación del aprendizaje activo para los eventos de clasificación inactivos durante la llamada de Rank

Para deshabilitar el aprendizaje automático, llame a Rank con `learningEnabled = False`.

El aprendizaje para un evento inactivo se activa implícitamente si envía una recompensa para la clasificación.

## <a name="learning-policies"></a>Directivas de aprendizaje

La directiva de aprendizaje determina los *hiperparámetros* específicos del entrenamiento de modelos. Dos modelos de los mismos datos, entrenados en diferentes directivas de aprendizaje, se comportarán de manera diferente.

### <a name="importing-and-exporting-learning-policies"></a>Importación y exportación de directivas de aprendizaje

Puede importar y exportar archivos de directivas de aprendizaje desde Azure Portal. Esto permite guardar las directivas existentes, probarlas, reemplazarlas y archivarlas en el control de código fuente como artefactos para futuras referencias y auditorías.

### <a name="learning-policy-settings"></a>Configuración de directivas de aprendizaje

La configuración de la **directiva de aprendizaje** no está pensada para modificarse. Solo debe cambiar la configuración cuando comprenda de qué manera puede afectar a Personalizer. Cambiar la configuración sin este conocimiento causará efectos secundarios, incluida la invalidación de los modelos de Personalizer.

### <a name="comparing-effectiveness-of-learning-policies"></a>Comparación de la eficacia de las directivas de aprendizaje

Puede comparar el rendimiento de las diferentes directivas de aprendizaje con los datos anteriores en los registros de Personalizer mediante [evaluaciones sin conexión](concepts-offline-evaluation.md).

[Cargue sus propias directivas de aprendizaje](how-to-offline-evaluation.md) para compararlas con la directiva actual de aprendizaje.

### <a name="discovery-of-optimized-learning-policies"></a>Detección de directivas de aprendizaje optimizadas

Personalizer puede crear una directiva de aprendizaje más optimizada cuando se realiza una [evaluación sin conexión](how-to-offline-evaluation.md). Una directiva de aprendizaje más optimizada, que ha demostrado tener mejores recompensas en una evaluación sin conexión, tendrá mejores resultados cuando se utilice en línea en Personalizer.

Después de crear una directiva de aprendizaje optimizada, puede aplicarla directamente a Personalizer para que reemplace a la directiva actual inmediatamente, o puede guardarla para una evaluación posterior y decidir en el futuro si desea descartarla, guardarla o aplicarla más tarde.