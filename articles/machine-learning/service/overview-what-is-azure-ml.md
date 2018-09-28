---
title: ¿Qué es el servicio Azure Machine Learning?
description: 'Explica los conceptos básicos del aprendizaje automático en la nube, describe para qué puede utilizarlo y define los términos de aprendizaje automático. Introducción a Azure Machine Learning: una solución de integrada y completa de ciencia de datos que permite a los científicos de datos profesionales desarrollar, experimentar e implementar aplicaciones de análisis avanzado a escala de nube.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 09/24/2018
ms.openlocfilehash: dddda5a222bf4e9fd81a3e13987ccdf443302083
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978919"
---
# <a name="what-is-azure-machine-learning-service-preview"></a>¿Qué es el servicio Azure Machine Learning (versión preliminar)?

El servicio Azure Machine Learning (versión preliminar) es un servicio en la nube que puede utilizar para desarrollar e implementar modelos de aprendizaje automático. Con el servicio Azure Machine Learning, puede realizar el seguimiento de los modelos a medida que los crea, entrena, implementa y administra, todo a la gran escala que la nube proporciona.

## <a name="what-is-machine-learning"></a>¿Qué es el aprendizaje automático?

El aprendizaje automático es una técnica de ciencia de datos que permite a los equipos utilizar datos existentes para prever tendencias, resultados y comportamientos futuros. Mediante el aprendizaje automático, los equipos aprenden si necesidad de programarlos explícitamente.

Las previsiones o predicciones del aprendizaje automático pueden hacer que las aplicaciones y los dispositivos sean más inteligentes. Por ejemplo, cuando compra en línea, el aprendizaje automático ayuda a recomendar otros productos según lo que haya adquirido. O, cuando pasa su tarjeta de crédito, el aprendizaje automático compara la transacción con una base de datos de transacciones y ayuda a detectar fraudes. Y cuando la aspiradora robot aspira una sala, el aprendizaje automático le ayuda a decidir si se ha terminado el trabajo.

## <a name="what-is-azure-machine-learning"></a>¿Qué es Azure Machine Learning?

El servicio Azure Machine Learning proporciona un entorno basado en la nube que puede utilizar para desarrollar, entrenar, probar, implementar, administrar y realizar un seguimiento de los modelos de aprendizaje automático.

[ ![Flujo de trabajo del servicio Azure Machine Learning](./media/overview-what-is-azure-ml/aml.png) ] (./media/overview-what-is-azure-ml/aml.png#lightbox)

El servicio Azure Machine Learning es totalmente compatible con las tecnologías de código abierto, por lo que puede utilizar decenas de miles de paquetes Python de código abierto con componentes de aprendizaje automático como TensorFlow y scikit-learn.
Las herramientas enriquecidas, como los [cuadernos de Jupyter Notebook](http://jupyter.org) o [Visual Studio Code Tools for AI](https://visualstudio.microsoft.com/downloads/ai-tools-vscode/), facilitan la exploración interactiva de datos, su transformación y, a continuación, el desarrollo y la prueba de modelos.
El servicio Azure Machine Learning también incluye características que [automatizan la generación y la optimización de modelos](tutorial-auto-train-models.md) para ayudarle a crear modelos con facilidad, eficiencia y precisión.

El servicio Azure Machine Learning le permite comenzar a entrenar en su máquina local y luego escalar horizontalmente a la nube. Con compatibilidad nativa para [Azure Batch AI](https://azure.microsoft.com/services/batch-ai/) y con [ servicios avanzados de optimización de hiperparámetros](how-to-tune-hyperparameters.md), puede crear mejores modelos más rápido, mediante el uso de la potencia de la nube. 

Cuando tenga el modelo adecuado, puede implementar fácilmente en un contenedor como Docker. Esto significa que es fácil de implementar en [Azure Container Instances](how-to-deploy-to-aci.md) o [Azure Kubernetes Service](how-to-deploy-to-aks.md), o puede utilizar el contenedor en sus propias implementaciones, ya sea en local o en la nube.
Puede administrar los modelos implementados y realizar un seguimiento de varias ejecuciones mientras experimenta para encontrar la mejor solución.

[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>¿Qué puedo hacer con el servicio Azure Machine Learning?

El servicio Azure Machine Learning puede generar automáticamente un modelo y ajustarlo automáticamente.
Para ver un ejemplo, consulte [Tutorial: Automatically train a classification model with Azure Automated Machine Learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento automático de un modelo de clasificación con Azure Machine Learning).

O ben, mediante el <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> de Azure Machine Learning para Python, junto con los paquetes Python de código abierto, se pueden crear y entrenar modelos de aprendizaje profundo y de aprendizaje automático sumamente precisos en un área de trabajo de Azure Machine Learning.
Puede elegir entre muchos componentes de aprendizaje automático disponibles en los paquetes de Python de código abierto, como los siguientes:

- <a href="http://scikit-learn.org/stable/" target="_blank">Scikit-learn</a>
- <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>
- <a href="https://pytorch.org" target="_blank">PyTorch</a>
- <a href="https://www.microsoft.com/cognitive-toolkit/" target="_blank">CNTK</a>
- <a href="http://mxnet.io" target="_blank">MXNet</a>

Cuando ya tiene un modelo, lo va a usar para crear un contenedor (como Docker) que puede implementarse localmente para pruebas, y después como un servicio web de producción en [Azure Container Instances](how-to-deploy-to-aci.md) o [Azure Kubernetes Service](how-to-deploy-to-aks.md).

Después, puede administrar los modelos implementados mediante [Azure Portal](https://portal.azure.com/) o la [extensión de la CLI de Azure Machine Learning](https://review.docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli).
Puede evaluar las métricas del modelo, volver a entrenar e implementar nuevas versiones del modelo, todo ello al tiempo que realiza un seguimiento de los experimentos del modelo.

Para comenzar a usar el servicio Azure Machine Learning, consulte la sección [Pasos siguientes](#next-steps) a continuación.

## <a name="how-is-azure-machine-learning-service-different-from-studio"></a>¿Cómo difiere el servicio Azure Machine Learning de Studio?

Azure Machine Learning Studio es un área de trabajo visual de colaborativa y de arrastrar y colocar donde puede crear, probar e implementar soluciones de aprendizaje automático sin necesidad de escribir código. Utiliza algoritmos de aprendizaje automático y módulos de control de datos precompilados y preconfigurados.

Utilice Machine Learning Studio cuando desee experimentar con modelos de aprendizaje automático de forma rápida y sencilla, y los algoritmos de aprendizaje automático integrados son suficientes para sus soluciones.

Utilice el servicio Machine Learning si trabaja en un entorno Python, si desea tener más control sobre los algoritmos de aprendizaje automático o si desea utilizar bibliotecas de aprendizaje automático de código abierto.

> [!NOTE]
> Los modelos creados en Azure Machine Learning Studio no pueden implementarse o administrarse por el servicio Azure Machine Learning.

## <a name="free-trial"></a>Evaluación gratuita
Si no es un suscriptor, puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Puede obtener créditos para gastarlos en servicios de Azure. Después de que se agoten los créditos, puede mantener la cuenta y usar los [servicios gratuitos de Azure](https://azure.microsoft.com/free/). No se le realizará ningún cargo en su tarjeta de crédito a menos que cambie explícitamente la configuración y lo solicite. Además, puede [activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): su suscripción a MSDN le proporciona crédito todos los meses que puede usar con servicios de Azure de pago.

## <a name="next-steps"></a>Pasos siguientes

- Cree un área de trabajo de aprendizaje automático mediante el artículo [Uso de Azure Portal como introducción](quickstart-get-started.md).
 
- Siga el tutorial completo, [Train an image classification model with Azure Machine Learning](tutorial-train-models-with-aml.md) (Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning), para obtener información sobre cómo entrenar e implementar modelos con el servicio Azure Machine Learning.

- Para más información sobre cómo permitir que Azure Machine Learning genere y ajuste automáticamente su modelo, consulte [Tutorial: Train a classification model with automated machine learning in Azure Machine Learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento de un modelo de clasificación con el aprendizaje automático en Azure Machine Learning).

- Para un análisis técnico y en profundidad del servicio, consulte [Arquitectura y conceptos del servicio Azure Machine Learning](concept-azure-machine-learning-architecture.md).

- Para más información sobre otros productos de aprendizaje automático de Microsoft, consulte [Otros productos de aprendizaje automático de Microsoft](./overview-more-machine-learning.md).


<!-- 

An intro to AML or an end-to-end quickstart video could go here.

In this 9-minute video, learn how you can benefit your app. You'll learn about key features and what a typical workflow looks like. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minutes covers key features and use-cases.
+ 3-4 minutes covers service provisioning. 
+ 4-6 minutes covers Import Data wizard used to create an index using the built-in real estate dataset.

-->