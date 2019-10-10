---
title: Configuración de las opciones de Personalizer
titleSuffix: Azure Cognitive Services
description: La configuración del servicio incluye la forma en que el servicio trata las recompensas, la frecuencia con el servicio realiza las exploraciones, la frecuencia con que se vuelve a entrenar al modelo y la cantidad de datos que se almacenan.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 09/30/2019
ms.author: diberry
ms.openlocfilehash: bad581fbc53292b5a7c25157ef839e07f33e131e
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827892"
---
# <a name="personalizer-settings"></a>Configuración de Personalizer

La configuración del servicio incluye la forma en que el servicio trata las recompensas, la frecuencia con el servicio realiza las exploraciones, la frecuencia con que se vuelve a entrenar al modelo y la cantidad de datos que se almacenan.

## <a name="create-personalizer-resource"></a>Creación de un recurso de Personalizer

Crear un recurso de Personalizer para cada bucle de comentarios. 

1. Inicie sesión en el [portal de Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer). El vínculo anterior le lleva a la página **Crear** del servicio Personalizer. 
1. Escriba el nombre del servicio, seleccione la suscripción, la ubicación, el plan de tarifa y el grupo de recursos.
1. Confirme la selección y elija **Crear**.

## <a name="configure-service-settings-in-the-azure-portal"></a>Configuración de los valores del servicio en Azure Portal

1. Inicie sesión en el [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer).
1. Busque el recurso Personalizer. 
1. En la sección **Administración de recursos**, seleccione **Configuración**.

    Antes de salir de Azure Portal, copie una de las claves de recurso de la página **Claves**. La necesitará para usar [Personalizer SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer).

### <a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>Configure las opciones de recompensas para el bucle de comentarios según cada caso de uso

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

El modelo más reciente, que se ha entrenado a partir de llamadas a Reward API desde todos los eventos activos, no lo usa automáticamente la llamada a Rank Personalizer. La **frecuencia de actualización del modelo** establece la frecuencia con qué se actualiza el modelo usado por la llamada Rank. 

Las frecuencias elevadas de actualización del modelo son útiles en aquellas situaciones en las que se desea hacer un estrecho seguimiento de los cambios en los comportamientos de los usuarios. Algunos ejemplos incluyen sitios que ofrecen noticias en vivo, contenido viral o pujas en vivo. En estos escenarios se puede usar una frecuencia de 15 minutos. Sin embargo, en la mayor parte de los casos una menor frecuencia de actualización resulta muy efectiva. Las frecuencias de actualización de un minuto son útiles cuando se depura el código de una aplicación mediante Personalizer, se realizan demostraciones o se prueban de forma interactiva ciertos aspectos del aprendizaje automático.

![Frecuencia de actualización del modelo establece la frecuencia con la que se vuelve a entrenar un nuevo modelo de Personalizer.](media/settings/configure-model-update-frequency-settings-15-minutes.png)

Después de cambiar este valor asegúrese de seleccionar **Save** (Guardar).

### <a name="data-retention"></a>Retención de datos

**Período de retención de datos** establece el número de días que Personalizer conserva los registros de datos. Los registros de datos anteriores se necesitan para realizar [evaluaciones sin conexión](concepts-offline-evaluation.md), que se usan para medir la eficacia de Personalizer y optimizar la directiva de aprendizaje.

Después de cambiar este valor asegúrese de seleccionar **Save** (Guardar).

## <a name="export-the-personalizer-model"></a>Exportación del modelo de Personalizer

En la sección de administración de recursos para **Modelo y directiva**, revise las fechas de creación y de última actualización del modelo y exporte el modelo actual. Puede usar Azure Portal o las API de Personalizer para exportar un archivo de modelo para el archivado. 

![Exportación del modelo de Personalizer actual](media/settings/export-current-personalizer-model.png)

## <a name="import-and-export-learning-policy"></a>Importación y exportación de una directiva de aprendizaje

En la sección de administración de recursos para **Modelo y directiva**, importe una nueva directiva de aprendizaje o exporte la actual.
Puede obtener archivos de directiva de aprendizaje de exportaciones anteriores o descargar las directivas optimizadas detectadas durante las evaluaciones sin conexión. La realización de cambios manuales en estos archivos afectará al rendimiento del aprendizaje automático y a la precisión de las evaluaciones sin conexión. Además, Microsoft no podrá avalar la precisión del aprendizaje automático y las evaluaciones, o de las excepciones de servicios derivadas de las directivas editadas manualmente.

## <a name="clear-data-for-your-learning-loop"></a>Limpieza de datos para el bucle de aprendizaje

1. En Azure Portal, en el recurso de personalización, en la página **Modelo y directiva**, seleccione **Borrar datos**.
1. Para borrar todos los datos y restablecer el bucle de aprendizaje al estado original, active las tres casillas.

    ![En Azure Portal, desactive los datos del recurso Personalizer.](./media/settings/clear-data-from-personalizer-resource.png)

    |Configuración|Propósito|
    |--|--|
    |Datos de personalización y recompensa registrados.|Estos datos de registro se usan en las evaluaciones sin conexión. Borre los datos si va a restablecer el recurso.|
    |Restablezca el modelo Personalizer.|Este modelo cambia en cada reentrenamiento. Esta frecuencia de entrenamiento se especifica en la **frecuencia del modelo de carga** en la página **Configuración**. |
    |Establezca la directiva de aprendizaje como predeterminada.|Si ha cambiado la directiva de aprendizaje como parte de una evaluación sin conexión, se restablece a la original.|

1. Seleccione **Borrar datos seleccionados** para comenzar el proceso de limpieza. El estado se comunica en las notificaciones de Azure, en el panel de navegación superior derecha. 

## <a name="next-steps"></a>Pasos siguientes

<!--
[How to use the Personalizer container](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409)
-->
[Obtenga información sobre la disponibilidad de la región](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
