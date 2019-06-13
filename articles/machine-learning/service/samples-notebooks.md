---
title: Cuadernos de Jupyter Notebook de ejemplo
titleSuffix: Azure Machine Learning service
description: Busque y use cuadernos de Jupyter de ejemplo para explorar el SDK de Python para Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 05/29/2019
ms.custom: seodec18
ms.openlocfilehash: ea4d5a807c25ea0406b49dac8a83ef1a34e0e8b3
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391795"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Uso de cuadernos de Jupyter para explorar Azure Machine Learning Service

El repositorio de [cuadernos de Azure Machine Learning](https://github.com/azure/machinelearningnotebooks) incluye los ejemplos más recientes del SDK de Python para Azure Machine Learning. Estos cuadernos de Juypter Notebook están diseñados para ayudarle a explorar el SDK y para servir como modelos para sus propios proyectos de aprendizaje automático.

Este artículo le muestra cómo acceder al repositorio desde los siguientes entornos:

- [Máquina virtual de Azure Machine Learning Notebook](#azure-machine-learning-notebook-vm)
- [Servidor de Notebook propio](#bring-your-own-jupyter-notebook-server)
- [Data Science Virtual Machine](#data-science-virtual-machine)
- [Azure Notebooks](#azure-notebooks)

> [!NOTE]
> Una vez que haya clonado el repositorio, encontrará los cuadernos del tutorial en la carpeta **tutorials** y los cuadernos de características específicas en la carpeta **how-to-use-azureml**.

## <a name="azure-machine-learning-notebook-vm"></a>Máquina virtual de Azure Machine Learning Notebook

La manera más de fácil de empezar a trabajar con los ejemplos es realizar el [inicio rápido sobre cuadernos en la nube](quickstart-run-cloud-notebook.md). Una vez completado, tendrá un servidor de cuadernos en el que se habrá cargado previamente el SDK y el repositorio de ejemplos. No es necesario realizar ninguna descarga ni instalación.

## <a name="bring-your-own-jupyter-notebook-server"></a>Uso del propio servidor de Jupyter Notebook

Si desea usar su propio servidor de Jupyter Notebook para desarrollo local, siga estos pasos:

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Estas instrucciones le permitirán instalar los paquetes básicos del SDK necesarios para los cuadernos del inicio rápido y el tutorial. Otros cuadernos de ejemplo pueden requerir la instalación de componentes adicionales. Para más información, consulte [Install the Azure Machine Learning SDK for Python](https://docs.microsoft.com/python/api/overview/azure/ml/install) (Instalación del SDK de Azure Machine Learning para Python).

## <a name="data-science-virtual-machine"></a>Máquina virtual de ciencia de datos

Data Science Virtual Machine (DSVM) es una imagen de máquina virtual personalizada diseñada específicamente para realizar ciencia de datos. Si [crea una instancia de DSVM](how-to-configure-environment.md#dsvm), el SDK y el servidor de cuadernos se instalarán y configurarán automáticamente. No obstante, tendrá que crear un área de trabajo y clonar el repositorio de ejemplo.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

## <a name="azure-notebooks"></a>Azure Notebooks

El SDK y el servidor de cuadernos se instalará y configurará en [Azure Notebooks](https://notebooks.azure.com/). Azure Notebooks le proporciona un entorno de cuadernos totalmente administrado y ligero que puede explorar.

Para acceder al repositorio de ejemplos de Azure Notebooks, vaya al área de trabajo de Azure Machine Learning mediante [Azure Portal](https://portal.azure.com). En la sección **Introducción**, seleccione **Introducción a Azure Notebooks**.

## <a name="next-steps"></a>Pasos siguientes

Explore los [cuadernos de ejemplo](https://aka.ms/aml-notebooks) para descubrir lo que Azure Machine Learning Service puede hacer o pruebe estos tutoriales:

- [Entrenamiento de un modelo de clasificación de imágenes con el servicio Azure Machine Learning](tutorial-train-models-with-aml.md)

- [Preparación de datos y uso de Machine Learning automatizado para entrenar un modelo de regresión con el conjunto de datos de los taxis de Nueva York](tutorial-data-prep.md)