---
title: ¿Qué es el servicio Azure Machine Learning?
description: 'Aprendizaje automático en la nube y para qué lo puede usar. Introducción al servicio Azure Machine Learning: una solución de integrada y completa de ciencia de datos que permite a los científicos de datos profesionales desarrollar, experimentar e implementar aplicaciones de análisis avanzado a escala de nube.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 12/04/2018
ms.openlocfilehash: fc241b284ab9370e03debf3e0851394c32087a57
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52870703"
---
# <a name="what-is-azure-machine-learning-service"></a>¿Qué es el servicio Azure Machine Learning?

El servicio Azure Machine Learning es un servicio en la nube que puede usar para entrenar, implementar, automatizar y administrar modelos de aprendizaje automático y todo ello con la gran capacidad de escalado que proporciona la nube.

## <a name="what-is-machine-learning"></a>¿Qué es el aprendizaje automático?

El aprendizaje automático es una técnica de ciencia de datos que permite a los equipos utilizar datos existentes para prever tendencias, resultados y comportamientos futuros. Mediante el aprendizaje automático, los equipos aprenden si necesidad de programarlos explícitamente.

Las previsiones o predicciones del aprendizaje automático pueden hacer que las aplicaciones y los dispositivos sean más inteligentes. Por ejemplo, cuando compra en línea, el aprendizaje automático ayuda a recomendar otros productos según lo que haya adquirido. O, cuando pasa su tarjeta de crédito, el aprendizaje automático compara la transacción con una base de datos de transacciones y ayuda a detectar fraudes. Y cuando la aspiradora robot aspira una sala, el aprendizaje automático le ayuda a decidir si se ha terminado el trabajo.

## <a name="what-is-azure-machine-learning-service"></a>¿Qué es el servicio Azure Machine Learning?

El servicio Azure Machine Learning proporciona un entorno basado en la nube que puede utilizar para desarrollar, entrenar, probar, implementar, administrar y realizar un seguimiento de los modelos de aprendizaje automático.

[ ![Flujo de trabajo del servicio Azure Machine Learning](./media/overview-what-is-azure-ml/aml.png) ] (./media/overview-what-is-azure-ml/aml.png#lightbox)

El servicio Azure Machine Learning es totalmente compatible con las tecnologías de código abierto, por lo que puede utilizar decenas de miles de paquetes Python de código abierto con componentes de aprendizaje automático como TensorFlow y scikit-learn.
La compatibilidad con herramientas enriquecidas, como [Jupyter Notebooks](http://jupyter.org) o [Azure Machine Learning para Visual Studio Code](https://visualstudio.microsoft.com/downloads/ai-tools-vscode/), facilita la exploración interactiva de datos, su transformación y, a continuación, el desarrollo y la prueba de modelos.
El servicio Azure Machine Learning también incluye características que [automatizan la generación y la optimización de modelos](tutorial-auto-train-models.md) para ayudarle a crear modelos con facilidad, eficiencia y precisión.

El servicio Azure Machine Learning le permite comenzar a entrenar en su máquina local y luego escalar horizontalmente a la nube. Con muchos [destinos de proceso](how-to-set-up-training-targets.md) como, por ejemplo, procesos de Azure Machine Learning y [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks), y con los [servicios avanzados de ajuste de hiperparámetros](how-to-tune-hyperparameters.md), puede compilar mejores modelos de forma más rápida gracias al potencial de la nube.

Cuando tenga el modelo adecuado, puede implementar fácilmente en un contenedor como Docker. Esto significa que es fácil de implementar en [Azure Container Instances](how-to-deploy-to-aci.md) o [Azure Kubernetes Service](how-to-deploy-to-aks.md), o puede utilizar el contenedor en sus propias implementaciones, ya sea en local o en la nube.
Puede administrar los modelos implementados y realizar un seguimiento de varias ejecuciones mientras experimenta para encontrar la mejor solución.
Una vez implementado, el modelo puede devolver predicciones en [tiempo real](how-to-consume-web-service.md) o de forma [asincrónica](how-to-run-batch-predictions.md) para grandes cantidades de datos.

Y con las [canalizaciones de aprendizaje automático](concept-ml-pipelines.md) avanzadas, puede colaborar en todos los pasos de preparación de datos, entrenamiento y evaluación de modelos, e implementación.

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>¿Qué puedo hacer con el servicio Azure Machine Learning?

El servicio Azure Machine Learning puede entrenar automáticamente un modelo y ajustarlo automáticamente.
Para ver un ejemplo, consulte [Tutorial: Automatically train a classification model with Azure Automated Machine Learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento automático de un modelo de clasificación con Azure Machine Learning).

O bien, mediante el <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> de Azure Machine Learning para Python, junto con los paquetes Python de código abierto, se pueden crear y entrenar modelos de aprendizaje profundo y de aprendizaje automático sumamente precisos en un área de trabajo del servicio Azure Machine Learning.
Puede elegir entre muchos componentes de aprendizaje automático disponibles en los paquetes de Python de código abierto, como los siguientes:

- <a href="https://scikit-learn.org/stable/" target="_blank">Scikit-learn</a>
- <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>
- <a href="https://pytorch.org" target="_blank">PyTorch</a>
- <a href="https://www.microsoft.com/en-us/cognitive-toolkit/" target="_blank">CNTK</a>
- <a href="http://mxnet.io" target="_blank">MXNet</a>

Una vez que tiene un modelo, puede usarlo para crear un contenedor (como, por ejemplo, Docker) que se puede implementar de forma local para la realización de pruebas. Una vez que se realiza la prueba, se puede implementar el modelo como un servicio web de producción en [Azure Container Instances](how-to-deploy-to-aci.md) o en [Azure Kubernetes Service](how-to-deploy-to-aks.md).

Después, puede administrar los modelos implementados mediante el [SDK para Python de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) o [Azure Portal](https://portal.azure.com/).
Puede evaluar las métricas del modelo, volver a entrenar e implementar nuevas versiones del modelo, todo ello al tiempo que realiza un seguimiento de los experimentos del modelo.

Para comenzar a usar el servicio Azure Machine Learning, consulte la sección [Pasos siguientes](#next-steps) a continuación.

## <a name="how-is-azure-machine-learning-service-different-from-studio"></a>¿Cómo difiere el servicio Azure Machine Learning de Studio?

Azure Machine Learning Studio es un área de trabajo visual de colaborativa y de arrastrar y colocar donde puede crear, probar e implementar soluciones de aprendizaje automático sin necesidad de escribir código. Utiliza algoritmos de aprendizaje automático y módulos de control de datos precompilados y preconfigurados.

Utilice Machine Learning Studio cuando desee experimentar con modelos de aprendizaje automático de forma rápida y sencilla, y los algoritmos de aprendizaje automático integrados son suficientes para sus soluciones.

Utilice el servicio Machine Learning si trabaja en un entorno Python, si desea tener más control sobre los algoritmos de aprendizaje automático o si desea utilizar bibliotecas de aprendizaje automático de código abierto.

> [!NOTE]
> Los modelos creados en Azure Machine Learning Studio no pueden implementarse o administrarse por el servicio Azure Machine Learning.

## <a name="free-trial"></a>Evaluación gratuita
Si no es un suscriptor, puede [abrir una cuenta de Azure de forma gratuita](https://aka.ms/amlfree). Puede obtener créditos para gastarlos en servicios de Azure. Después de que se agoten los créditos, puede mantener la cuenta y usar los [servicios gratuitos de Azure](https://azure.microsoft.com/free/). No se le realizará ningún cargo en su tarjeta de crédito a menos que cambie explícitamente la configuración y lo solicite. Además, puede [activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) que le proporciona crédito todos los meses que puede usar con servicios de Azure de pago.

## <a name="next-steps"></a>Pasos siguientes

- Cree un área de trabajo del servicio Machine Learning y empiece a trabajar [mediante Azure Portal](quickstart-get-started.md) o [en Python](quickstart-create-workspace-with-python.md).

- Realice el tutorial completo, [Entrenamiento e implementación de un modelo de clasificación de imágenes con Azure Machine Learning](tutorial-train-models-with-aml.md).

- [Uso de Azure Machine Learning para generar y ajustar automáticamente un modelo](tutorial-auto-train-models.md).

- Aprenda sobre [la canalización de aprendizaje automático ](/azure/machine-learning/service/concept-ml-pipelines) para crear, optimizar y administrar los escenarios de aprendizaje automático.

- Lea detenidamente el artículo [Arquitectura y conceptos del servicio Azure Machine Learning](concept-azure-machine-learning-architecture.md).

- Para más información sobre otros productos de aprendizaje automático de Microsoft, consulte [Otros productos de aprendizaje automático de Microsoft](./overview-more-machine-learning.md).


<!-- 

An intro to AML or an end-to-end quickstart video could go here.

In this 9-minute video, learn how you can benefit your app. You'll learn about key features and what a typical workflow looks like. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minutes covers key features and use-cases.
+ 3-4 minutes covers service provisioning. 
+ 4-6 minutes covers Import Data wizard used to create an index using the built-in real estate dataset.

-->
