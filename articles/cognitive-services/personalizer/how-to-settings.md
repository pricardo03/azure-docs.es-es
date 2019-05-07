---
title: Definición de la configuración
titleSuffix: Azure Cognitive Services
description: La configuración del servicio incluye la forma en que el servicio trata las recompensas, la frecuencia con el servicio realiza las exploraciones, la frecuencia con que se vuelve a entrenar al modelo y la cantidad de datos que se almacenan.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 0cd08e1191c68c57975d3e68648134925155e7f2
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025601"
---
# <a name="personalizer-settings"></a>Configuración de Personalizer

La configuración del servicio incluye la forma en que el servicio trata las recompensas, la frecuencia con el servicio realiza las exploraciones, la frecuencia con que se vuelve a entrenar al modelo y la cantidad de datos que se almacenan.

Crear un recurso de Personalizer para cada bucle de comentarios. 

## <a name="configure-service-settings-in-the-azure-portal"></a>Configuración de los valores del servicio en Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque el recurso Personalizer. 
1. En la sección **Administración de recursos**, seleccione **Configuración**.

    ![Inicie sesión en el Portal de Azure. Busque el recurso Personalizer. En la sección Administración de recursos, seleccione Configuración.](media/settings/how-to-configure.png)

### <a name="reward-settings-for-the-feedback-loop"></a>Configuración de recompensas para el bucle de comentarios

Configure el servicio para el uso de recompensas del bucle de comentarios. Los cambios que se realicen en la siguiente configuración restablecerán el modelo actual de Personalizer y volverán a entrenarlo con los datos de los dos últimos días:

![Configuración de las recompensas para el bucle de comentarios](media/settings/configure-model-reward-settings.png)

|Configuración|Propósito|
|--|--|
|Tiempo de espera de las recompensas|Establece el período durante el que Personalizer recopilará los valores de recompensa de una llamada a Rank, desde el momento en que se produce dicha llamada. Para establecer este valor se realiza la pregunta: "¿Cuánto tiempo debe Personalizer esperar llamadas de recompensas?" Todas las recompensas que lleguen después de este espacio temporal se registrarán, pero no se usarán para el aprendizaje.|
|Recompensa predeterminada|Si Personalizer no recibe ninguna llamada de recompensa por durante el período de tiempo de espera de las recompensas asociado a una llamada a Rank, Personalizer asignará la recompensa predeterminada. De forma predeterminada y en la mayoría de los escenarios, la recompensa predeterminada es cero.|
|Agregación de recompensas|Si se reciben varias recompensas para la misma llamada de la API Rank, se usa este método de agregación: **suma** o **más antiguo**. El primero elige la primera puntuación recibida y descarta el resto. Esto es útil si desea una sola recompensa para las posibles llamadas duplicadas. |

Después de cambiar esta configuración asegúrese de seleccionar **Save** (Guardar).

### <a name="exploration-setting"></a>Configuración de la exploración 

Personalization puede detectar nuevos patrones y adaptarlos a los cambios de comportamiento de los usuarios con el paso del tiempo mediante la exploración de alternativas. El valor **Exploration** (Exploración) determina qué porcentaje de las llamadas a Rank se responden con una exploración. 

Los cambios que se realicen en este valor restablecerán el modelo actual de Personalizer y lo volverán a entrenar con los datos de los dos últimos días.

![El valor de exploración determina qué porcentaje de las llamadas a Rank se responden con una exploración](media/settings/configure-exploration-setting.png)

Después de cambiar este valor asegúrese de seleccionar **Save** (Guardar).

### <a name="model-update-frequency"></a>Frecuencia de actualización del modelo

**Frecuencia de actualización del modelo** establece la frecuencia con la que se vuelve a entrenar un nuevo modelo de Personalizer. 

![Frecuencia de actualización del modelo establece la frecuencia con la que se vuelve a entrenar un nuevo modelo de Personalizer.](media/settings/configure-model-update-frequency-settings.png)

Después de cambiar este valor asegúrese de seleccionar **Save** (Guardar).

### <a name="data-retention"></a>Retención de datos

**Período de retención de datos** establece el número de días que Personalizer conserva los registros de datos. Los registros de datos anteriores se necesitan para realizar [evaluaciones sin conexión](concepts-offline-evaluation.md), que se usan para medir la eficacia de Personalizer y optimizar la directiva de aprendizaje.

Después de cambiar este valor asegúrese de seleccionar **Save** (Guardar).

## <a name="export-the-personalizer-model"></a>Exportación del modelo de Personalizer

En la sección de administración de recursos para **Modelo y directiva**, revise las fechas de creación y de última actualización del modelo y exporte el modelo actual.

![Exportación del modelo de Personalizer actual](media/settings/export-current-personalizer-model.png)

## <a name="import-and-export-learning-policy"></a>Importación y exportación de una directiva de aprendizaje

En la sección de administración de recursos para **Modelo y directiva**, importe una nueva directiva de aprendizaje o exporte la actual.

## <a name="next-steps"></a>Pasos siguientes

[Aprendizaje de refuerzo](concepts-reinforcement-learning.md) 
