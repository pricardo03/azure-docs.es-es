---
title: 'Administración de directivas de aprendizaje: Personalizer'
description: En este artículo se incluyen respuestas a preguntas para solucionar problemas sobre Personalizer.
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: 707ba9fe7b73ef74e21321533ba02b11f2bad850
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75840718"
---
# <a name="manage-learning-policy"></a>Administración de directivas de aprendizaje

La configuración de una directiva de aprendizaje determina los _hiperparámetros_ del entrenamiento de modelos. La directiva de aprendizaje se define en un archivo `.json`.

## <a name="import-a-new-learning-policy"></a>Importación de una nueva directiva de aprendizaje

1. Abra [Azure Portal](https://portal.azure.com) y seleccione su recurso de Personalizer.
1. Seleccione **Configuración de modelo y aprendizaje** en la sección **Administración de recursos**.
1. En **Importar configuración de aprendizaje**, seleccione el archivo que creó con el formato JSON especificado anteriormente y, después, haga clic en el botón **Cargar**.

    Espere a recibir la notificación de que la directiva de aprendizaje se ha cargado correctamente.

## <a name="export-a-learning-policy"></a>Exportación de una directiva de aprendizaje

1. Abra [Azure Portal](https://portal.azure.com) y seleccione su recurso de Personalizer.
1. Seleccione **Configuración de modelo y aprendizaje** en la sección **Administración de recursos**.
1. En **Importar configuración de aprendizaje**, haga clic en el botón **Exportar configuración de aprendizaje**. De este modo, se guarda el archivo `json` en su equipo local.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los [conceptos](concept-active-learning.md#learning-settings) de la directiva de aprendizaje

[Obtenga información sobre la disponibilidad de la región](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)