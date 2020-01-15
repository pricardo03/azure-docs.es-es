---
title: ¿Qué son los entornos de ML?
titleSuffix: Azure Machine Learning
description: En este artículo, obtendrá información sobre las ventajas de los entornos de aprendizaje automático, que permiten definiciones de dependencias de aprendizaje automático reproducibles, auditables y portátiles en distintos destinos de proceso.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: trbye
author: trevorbye
ms.date: 01/06/2020
ms.openlocfilehash: ad520c7e6503f28de0bd5538662c223575a078fa
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2020
ms.locfileid: "75692628"
---
# <a name="what-are-azure-machine-learning-environments"></a>¿Qué son los entornos de Azure Machine Learning?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Los entornos especifican los paquetes, las variables de entorno y la configuración de software de Python que rodean a los scripts de entrenamiento y puntuación, y los tiempos de ejecución (Python, Spark o Docker). Son entidades administradas y con control de versiones en el área de trabajo de Azure Machine Learning que permiten flujos de trabajo de aprendizaje automático reproducibles, auditables y portátiles en distintos destinos de proceso.

Puede usar un objeto de entorno en el proceso local para desarrollar el script de entrenamiento, volver a usar ese mismo entorno en Proceso de Machine Learning para el entrenamiento de modelos a escala e incluso implementar el modelo con ese mismo entorno.

A continuación se ilustra que el mismo objeto de entorno se puede usar en la configuración de ejecución para el entrenamiento y en la configuración de inferencia e implementación para las implementaciones de servicios web.

![Diagrama del entorno del flujo de trabajo de aprendizaje automático](./media/concept-environments/ml-environment.png)

## <a name="types-of-environments"></a>Tipos de entornos

Los entornos se pueden dividir a grandes rasgos en tres categorías: **mantenidos**, **administrados por el usuario** y **administrados por el sistema**.

Los entornos mantenidos los proporciona Azure Machine Learning y están disponibles en el área de trabajo de forma predeterminada. Contienen colecciones de paquetes de Python y configuraciones que le ayudarán a empezar a usar diferentes marcos de aprendizaje automático. 

Para un entorno administrado por usuarios, es responsable de configurar el entorno e instalar todos los paquetes que necesita el script de entrenamiento en el destino de proceso. Conda no comprobará el entorno ni instalará nada por usted. Tenga en cuenta que si va a definir un entorno propio, tendrá que enumerar `azureml-defaults` con la versión `>= 1.0.45` como dependencia pip. Este paquete contiene la funcionalidad necesaria para hospedar el modelo como un servicio web.

Use entornos administrados por el sistema cuando quiera que [Conda](https://conda.io/docs/) administre el entorno de Python y las dependencias de script. El servicio asume el uso de este tipo de entorno de forma predeterminada, debido a su utilidad en destinos de proceso remotos que no se pueden configurar manualmente.

## <a name="creating-and-managing-environments"></a>Creación y administración de entornos

Los entornos se pueden crear mediante:

* La definición de objetos `Environment` nuevos, ya sea mediante un entorno mantenido o la definición de dependencias propias.
* El uso de objetos `Environment` existentes desde el área de trabajo. Esto permite la coherencia y la posibilidad de reproducir las dependencias.
* Importación desde una definición de entorno de Anaconda existente.

Vea la [guía paso a paso](how-to-use-environments.md#create-an-environment) para obtener ejemplos de código específicos. Los entornos también se administran con facilidad a través del área de trabajo e incluyen la funcionalidad siguiente:

* Los entornos se registran de forma automática en el área de trabajo cuando se envía un experimento. También se pueden registrar manualmente.
* Capture entornos del área de trabajo y úselos para el entrenamiento, la implementación o la modificación de la definición del entorno.
* El control de versiones permite ver los cambios en los entornos en el tiempo y garantiza la reproducibilidad.
* Compilación de imágenes de Docker de forma automática desde los entornos.

Vea la sección [Administración de entornos](how-to-use-environments.md#manage-environments) de la guía paso a paso para obtener ejemplos de código.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [crear y usar entornos](how-to-use-environments.md) en Azure Machine Learning
* Vea la documentación de referencia del SDK de Python para la [clase environment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py).
* Vea la documentación de referencia del SDK de R para [environments](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments).