---
title: ¿Qué le va a suceder a Azure Batch AI? | Microsoft Docs
description: Obtenga información sobre lo que le va a suceder a Azure Batch AI y la opción de proceso de Azure Machine Learning Service.
ms.service: batch-ai
services: batch-ai
ms.topic: overview
ms.author: jmartens
author: j-martens
ms.date: 2/28/2019
ms.openlocfilehash: edd6a7e5f385d766d51631d77f5889233af2469a
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194509"
---
# <a name="whats-happening-to-azure-batch-ai"></a>¿Qué le va a suceder a Azure Batch AI?

**El servicio Azure Batch AI se va a retirar en marzo.** Las funciones de entrenamiento y puntuación a escala de Batch AI están ahora disponibles en el [Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md), que está disponible con carácter general desde el 4 de diciembre de 2018.

Junto con muchas otras funciones de aprendizaje automático, Azure Machine Learning Service incluye un destino de proceso administrado basado en la nube para el entrenamiento, la implementación y la puntuación de modelos de aprendizaje automático. Este destino de proceso se llama [Proceso de Machine Learning](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Empezar a migrar y usarlo hoy](#migrate). Puede interactuar con Azure Machine Learning Service a través de sus [SDK de Python](../machine-learning/service/quickstart-create-workspace-with-python.md), la interfaz de línea de comandos y [Azure Portal](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Escala de tiempo del soporte técnico

En este momento, puede usar las suscripciones existentes de Azure Batch AI igual que antes. Sin embargo, no son posibles **nuevas suscripciones** y no se van a realizar nuevas inversiones.

A partir de marzo&nbsp;31&#x2c;&nbsp;2019, las suscripciones a Batch AI dejarán de funcionar.

## <a name="compare-to-azure-machine-learning"></a>Comparación con Azure Machine Learning
Se trata de un servicio en la nube que se usa para entrenar, implementar, automatizar y administrar modelos de aprendizaje automático, y todo ello con la gran capacidad de escalado que proporciona la nube. Conozca qué es [Azure Machine Learning Service en esta introducción](../machine-learning/service/overview-what-is-azure-ml.md).
 

En un ciclo de vida típico de desarrollo de un modelo se pueden distinguir las siguientes fases: preparación de datos, entrenamiento, experimentación e implementación. Este ciclo de un extremo a otro se puede orquestar mediante canalizaciones de Machine Learning.

![Diagrama de flujo](./media/overview-what-happened-batch-ai/lifecycle.png)


[Más información sobre cómo funciona el servicio y sus conceptos principales](../machine-learning/service/concept-azure-machine-learning-architecture.md). Muchos de los conceptos del flujo de trabajo de entrenamiento de modelos son similares a los conceptos existentes en Batch AI. 

En concreto, esta es una tabla de cómo debe considerarlos:
 
|Servicio Batch AI|  Servicio Azure Machine Learning|
|:--:|:---:|
|Área de trabajo|Área de trabajo|
|Clúster|   Proceso de tipo `AmlCompute`|
|Servidores de archivos|DataStores|
|Experimentos|Experimentos|
|Trabajos|Ejecuciones (permite ejecuciones anidadas)|
 
Esta es otra vista de la misma tabla que le ayudará a visualizar mejor las cosas:
 
### <a name="batch-ai-hierarchy"></a>Jerarquía de Batch AI
![Diagrama de flujo](./media/overview-what-happened-batch-ai/batchai-heirarchy.png) 
 
### <a name="azure-machine-learning-service-hierarchy"></a>Jerarquía de Azure Machine Learning Service
![Diagrama de flujo](./media/overview-what-happened-batch-ai/azure-machine-learning-service-heirarchy.png) 

## <a name="platform-capabilities"></a>Funcionalidades de plataforma
Azure Machine Learning Service aporta un gran conjunto de nuevas funcionalidades, como una pila de entrenamiento -> implementación de un extremo a otro que puede usar para el desarrollo de inteligencia artificial sin tener que administrar ningún recurso de Azure. En esta tabla se compara la compatibilidad con las características para el entrenamiento entre los dos servicios.

|Característica|Servicio Batch AI|Servicio Azure Machine Learning|
|-------|:-------:|:-------:|
|Elección del tamaño de máquina virtual |CPU/GPU    |CPU/GPU. También admite FPGA para la realización de inferencias|
|Clúster preparado para inteligencia artificial (controladores, Docker, etc.)|  Sí |Sí|
|Preparación del nodo| Sí|    Sin |
|Elección de la familia de sistema operativo   |Parcial    |Sin |
|Máquinas virtuales dedicadas y de prioridad baja  |Sí    |Sí|
|Escalado automático   |Sí    |Sí (de forma predeterminada)|
|Tiempo de espera para la escalabilidad automática  |Sin  |Sí|
|SSH    |Sí|   Sí|
|Montaje de nivel del clúster |Sí (FileShares, Blobs, NFS, personalizado)   |Sí (montar o descargar el almacén de datos)|
|Entrenamiento distribuido|  Sí |Sí|
|Modo de ejecución del trabajo|    Máquina virtual o contenedor|    Contenedor|
|Imagen de contenedor personalizada|    Sí |Sí|
|Cualquier kit de herramientas    |Sí    |Sí (ejecutar script de Python)|
|JobPreparation|    Sí |Aún no|
|Montaje en el nivel de trabajo |Sí (FileShares, Blobs, NFS, personalizado)   |Sí (FileShares, Blobs)|
|Supervisión de trabajos     |Mediante GetJob|    Mediante el historial de ejecución (información más completa, entorno de ejecución personalizado para insertar más métricas)|
|Recuperar registros de trabajos y archivos o modelos |   Mediante ListFiles y API de Storage  |Mediante el servicio de artefacto|
 |Compatibilidad con Tensorboard   |Sin |    Sí|
|Cuotas de nivel de familia de máquinas virtuales |Sí    |Sí (con la capacidad anterior transmitida)|
 
Además de la tabla anterior, hay características en Azure Machine Learning Service que tradicionalmente no se admitían en BatchAI.

|Característica|Servicio Batch AI|Servicio Azure Machine Learning|
|-------|:-------:|:-------:|
|Preparación del entorno    |Sin  |Sí (preparación y carga de Conda en ACR)|
|Ajuste de hiperparámetros  |Sin |    Sí|
|Administración de modelos   |Sin  |Sí|
|Operacionalización e implementación| Sin   |Mediante AKS y ACI|
|Preparación de los datos   |Sin  |Sí|
|Destinos de proceso    |Máquinas virtuales de Azure  |Local, BatchAI (como AmlCompute), DataBricks, HDInsight|
|Automated Machine Learning |Sin |    Sí|
|Procesos  |Sin  |Sí|
|Puntuación por lotes  |Sí    |Sí|
|Compatibilidad con el portal o la CLI|    Sí |Sí|


## <a name="programming-interfaces"></a>Interfaces de programación

En esta tabla se presentan las diversas interfaces de programación disponibles para cada servicio.
    
|Característica|Servicio BatchAI|Servicio Azure Machine Learning|
|-------|:-------:|:-------:|
|SDK    |Java, C#, Python, Nodejs   |Python (basado en la configuración de ejecución y basado en el estimador para plataformas comunes)|
|CLI    |Sí    |Aún no|
|Azure Portal   |Sí    |Sí (excepto envío de trabajos)|
|API DE REST   |Sí    |Sí, pero distribuido entre microservicios|


La actualización del servicio Batch AI en versión preliminar a Azure Machine Learning Service con disponibilidad general le proporciona una mejor experiencia gracias a conceptos que son más fáciles de usar, como estimadores y almacenes de datos. También garantiza Acuerdos de Nivel de Servicio (SLA) de Azure de nivel de disponibilidad general y soporte técnico al cliente.

Azure Machine Learning Service también proporciona nueva funcionalidad, como aprendizaje automático automatizado, ajuste de hiperparámetros y canalizaciones de aprendizaje automático, que son útiles en la mayoría de las cargas de trabajo de AI a gran escala. La posibilidad de poner en marcha un modelo entrenado sin cambiar a un servicio independiente ayuda a completar el bucle de ciencia de datos: desde la preparación de los datos (con el SDK de preparación de datos) hasta la puesta en marcha y la supervisión del modelo.

<a name="migrate"></a>
## <a name="migrate"></a>Migrar

Aprenda a migrar a Azure Machine Learning Service y sepa cómo el código que usa se asigna a dicho servicio en el artículo [Migrar a Azure Machine Learning Service](how-to-migrate.md).

## <a name="get-support"></a>Obtención de soporte técnico

La retirada de Batch AI está prevista para el 31 de marzo y ya se está impidiendo el registro de nuevas suscripciones en el servicio, salvo que se eleve una excepción mediante soporte técnico para que se incluya en la lista de permitidos.  Si tiene alguna pregunta o comentario al migrar a Azure Machine Learning Service, puede ponerse en contacto con nosotros en [Azure Batch AI Training Preview](mailto:AzureBatchAITrainingPreview@service.microsoft.com).

Azure Machine Learning Service es un servicio disponible con carácter general. Esto significa que se incluye con un Acuerdo de Nivel de Servicio (SLA) y diversos planes de soporte técnico para elegir.

Los precios de uso de la infraestructura de Azure, bien mediante el servicio BatchAI o el servicio Azure Machine Learning Service, no deberían variar, ya que, en ambos casos, solo se modifica el precio por el proceso subyacente. Para más información, consulte la [calculadora de precios](https://azure.microsoft.com/pricing/details/machine-learning-service/).

Vea la disponibilidad regional entre los dos servicios en [Azure Portal](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all).


## <a name="next-steps"></a>Pasos siguientes

+ Aprenda [cómo migrar](how-to-migrate.md) Azure Machine Learning Service y cómo el código que usa ahora se asigna al código de este servicio.

+ Lea la [información general de Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Configure un destino de proceso para el entrenamiento de modelos](../machine-learning/service/how-to-set-up-training-targets.md) con Azure Machine Learning Service.

+ Revise la [hoja de ruta de Azure](https://azure.microsoft.com/updates/) para información de otras actualizaciones de servicios de Azure.
