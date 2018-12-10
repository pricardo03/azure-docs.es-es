---
title: 'Comparación de opciones de productos de aprendizaje automático de Microsoft: Azure | Microsoft Docs'
description: Compare la variedad de productos de Microsoft para crear, implementar y administrar modelos de aprendizaje automático. Decida qué productos elegir para su solución.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 12/04/2018
ms.openlocfilehash: ad6cb53bd69218d2af801cb56f882f5ffde3026c
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872559"
---
# <a name="what-are-the-machine-learning-product-options-from-microsoft"></a>¿Cuáles son las opciones de productos de aprendizaje automático de Microsoft?

Microsoft proporciona una variedad de opciones de productos para crear, implementar y administrar modelos de aprendizaje automático. Compare estos productos y elija lo que necesite para desarrollar soluciones de aprendizaje automático de la forma más eficaz.

| Producto de aprendizaje automático | ¿Qué es? | Qué puede hacer con él |
|-|-|-|
| ***En la nube*** | | |
| [Servicio Azure Machine Learning](#azure-machine-learning-services) | Servicio en la nube administrado de ML  | Entrenar, implementar y administrar modelos en Azure con Python y la CLI |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | Interfaz visual de arrastrar y colocar de ML | Compilar, experimentar e implementar modelos mediante algoritmos preconfigurados |
| [Azure Databricks](#azure-databricks) | Plataforma de análisis basada en Spark | Compilar e implementar flujos de trabajo de datos y modelos |
| [Azure Cognitive Services](#azure-cognitive-services) | Servicios de Azure con modelos de inteligencia artificial y ML creados previamente | Agregar fácilmente características inteligentes a las aplicaciones |
| [Azure Data Science Virtual Machine](#azure-data-science-virtual-machine) | Máquina virtual con herramientas de ciencia de datos previamente instaladas | Desarrollar soluciones de ML en un entorno preconfigurado |
| ***Local***<br>*(los servidores locales también se pueden ejecutar en una máquina virtual en la nube)* | | |
| [SQL Server Machine Learning Services](#sql-server-machine-learning-services) | Motor de análisis insertado en SQL | Crear e implementar modelos en SQL Server |
| [Servidor de Microsoft Machine Learning](#microsoft-machine-learning-server) | Servidor empresarial independiente para análisis predictivo | Compilar e implementar modelos generados con R y Python |
| ***Herramientas para desarrolladores*** | | |
| [ML.NET](#mlnet) | SDK de ML de código abierto y multiplataforma | Desarrollar soluciones de ML para aplicaciones .NET |
| [Windows ML](#windows-ml) | Plataforma de ML de Windows 10 | Evaluar modelos entrenados en un dispositivo Windows 10 |

## <a name="azure-machine-learning-service"></a>Servicio Azure Machine Learning

[Azure Machine Learning](overview-what-is-azure-ml.md) es un servicio en la nube totalmente administrado que se utiliza para entrenar, implementar y administrar modelos de ML a escala. Además, es totalmente compatible con las tecnologías de código abierto, por lo que puede utilizar decenas de miles de paquetes de Python de código abierto como TensorFlow, PyTorch y scikit-learn. También hay disponibles herramientas enriquecidas, como [Azure Notebooks](https://notebooks.azure.com/), [Jupyter Notebooks](http://jupyter.org) o la extensión de [Azure Machine Learning para Visual Studio Code](https://visualstudio.microsoft.com/downloads/ai-tools-vscode/), para que sea fácil explorar y transformar datos, y posteriormente, entrenar e implementar modelos. El servicio Azure Machine Learning incluye características que automatizan la generación y la optimización de modelos con facilidad, eficacia y precisión.

Use el servicio Azure Machine Learning para entrenar, implementar y administrar modelos de ML con Python y la CLI en la nube.

>[!Note]
> Puede probar Azure Machine Learning de forma gratuita. No se necesitan tarjeta de crédito ni una suscripción a Azure. Empiece ahora. https://azure.microsoft.com/free/

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

[Azure Machine Learning Studio](../studio/what-is-ml-studio.md) proporciona un área de trabajo interactiva y visual que puede usar para compilar, probar e implementar con facilidad y rapidez modelos mediante algoritmos de aprendizaje automático pregenerados. Machine Learning Studio publica modelos como servicios web que pueden utilizarse fácilmente en aplicaciones personalizadas o herramientas de BI como Excel.
No se requiere escribir código. El modelo de aprendizaje automático se construye conectando conjuntos de datos y módulos de análisis en un lienzo y, a continuación, se implementa con un par de clics.

Use Machine Learning Studio cuando quiera desarrollar e implementar modelos sin necesidad de escribir código.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/what-is-azure-databricks) es una plataforma de análisis basada en Apache Spark optimizada para la plataforma de servicios en la nube de Microsoft Azure. Databricks está integrado con Azure para proporcionar una configuración con un solo clic, flujos de trabajo optimizados y un área de trabajo interactiva que permite la colaboración entre científicos de datos, ingenieros de datos y analistas empresariales.
Use código de Python, Scala, R y SQL en cuadernos web para consultar, visualizar y modelar datos.

Utilice Databricks cuando desea colaborar creando soluciones de aprendizaje automático en Apache Spark.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

[Azure Cognitive Services](/azure/cognitive-services/welcome) es un conjunto de API que permiten compilar aplicaciones que usan métodos naturales de comunicación. Estas API permiten a las aplicaciones ver, escuchar, hablar, comprender e interpretar las necesidades del usuario con solo unas pocas líneas de código. Agregue fácilmente características inteligentes a las aplicaciones, como: 

- Detección de opiniones y emociones
- Reconocimiento de visión y voz
- Language Understanding (LUIS)
- Conocimiento y búsqueda

Cognitive Services se puede usar para desarrollar aplicaciones en distintos dispositivos y plataformas. Las API siguen mejorando y son fáciles de configurar.

## <a name="azure-data-science-virtual-machine"></a>Máquina virtual de ciencia de datos de Azure

[Data Science Virtual Machine](../data-science-virtual-machine/overview.md) es un entorno de máquina virtual personalizado en la nube de Microsoft Azure diseñado específicamente para hacer ciencia de datos. Tiene muchas ciencias de datos conocidas y otras herramientas preinstaladas y preconfiguradas para impulsar la creación de aplicaciones inteligentes para análisis avanzado.

Data Science Virtual Machine se admite como un destino para el servicio Azure Machine Learning.
Está disponible en las versiones de Windows y Linux Ubuntu (el servicio Azure Machine Learning no es compatible con CentOS Linux).
Para obtener información de versiones específicas y una lista de lo que incluye, consulte el artículo de [introducción a Azure Data Science Virtual Machine](../data-science-virtual-machine/overview.md).

Use la máquina virtual de ciencia de datos cuando necesite ejecutar u hospedar trabajos en un único nodo. O bien si necesita escalar de forma remota el procesamiento en un único equipo.

## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Services

[SQL Server Microsoft Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) agrega análisis estadísticos, visualización de datos y análisis predictivo en R y Python para datos relacionales en las bases de datos de SQL Server. Las bibliotecas de R y Python de Microsoft incluyen algoritmos de ML y modelos avanzados, que pueden ejecutarse en paralelo y a escala, en SQL Server.

Use SQL Server Machine Learning Services cuando necesite análisis predictivo e inteligencia artificial integrados en datos relacionales en SQL Server.

## <a name="microsoft-machine-learning-server"></a>Servidor de Microsoft Machine Learning

El [servidor de Microsoft Machine Learning](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) es un servidor empresarial para hospedar y administrar cargas de trabajo paralelas y distribuidas de procesos de R y Python. Microsoft Machine Learning Server se ejecuta en Linux, Windows, Hadoop y Apache Spark, y también se encuentra disponible en [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Además, proporciona un motor de ejecución para las soluciones compiladas mediante paquetes de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) y [MicrosoftML](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package), y agrega a las plataformas R y Python de código abierto compatibilidad con análisis de alto rendimiento, análisis estadísticos, aprendizaje automático y conjuntos de datos de grandes dimensiones. Esta funcionalidad se ofrece a través de paquetes de propiedad que se instalan con el servidor. Para el desarrollo, puede usar IDE, como [Herramientas de R para Visual Studio](https://www.visualstudio.com/vs/rtvs/) y [Herramientas de Python para Visual Studio](https://www.visualstudio.com/vs/python/).

Use Microsoft Machine Learning Server cuando necesite crear y usar modelos generados con R y Python en un servidor, o bien distribuir el entrenamiento de R y Python a escala en un clúster de Hadoop o Spark.

## <a name="mlnet"></a>ML.NET

[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/) es un marco de aprendizaje automático gratuito, de código abierto y multiplataforma que permite crear soluciones de aprendizaje automático personalizadas e integrarlas en las aplicaciones. NET.

Utilice ML.NET cuando desee integrar soluciones de aprendizaje automático en aplicaciones. NET.

## <a name="windows-ml"></a>Windows ML

[Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/) permite usar modelos de aprendizaje automático entrenados en sus aplicaciones, de forma que se evalúan los modelos entrenados localmente en dispositivos Windows 10.

Use Windows ML cuando desee modelos de aprendizaje automático entrenados en aplicación Windows.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre todos los productos de desarrollo de inteligencia artificial (AI) disponibles en Microsoft, consulte la página sobre la [plataforma de inteligencia artificial de Microsoft AI](https://www.microsoft.com/ai).
- Para saber cómo desarrollar soluciones de inteligencia artificial, consulte la página [Microsoft AI School](https://aischool.microsoft.com/learning-paths).
