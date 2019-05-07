---
title: Cuadernos de Jupyter Notebook de ejemplo
titleSuffix: Azure Machine Learning service
description: Busque y use cuadernos de Jupyter de ejemplo para explorar Azure Machine Learning Service en Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: cd88fd85ce6d18287c700a54e42b6237a42ea5c9
ms.sourcegitcommit: eea74d11a6d6ea6d187e90e368e70e46b76cd2aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2019
ms.locfileid: "65035368"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Uso de cuadernos de Jupyter para explorar Azure Machine Learning Service

Para su comodidad, hemos desarrollado una serie de cuadernos de Jupyter Python que puede usar para explorar Azure Machine Learning Service. 

Aprenda a usar el servicio con la documentación de este sitio y use estos cuadernos para adecuarlos a su situación. 

Utilice una de las siguientes rutas de acceso para ejecutar un servidor de cuadernos con estos cuadernos de ejemplo.  Una vez que el servidor se esté ejecutando, busque los cuadernos de tutoriales en la carpeta **tutorials** o explore las diferentes características en la carpeta **how-to-use-azureml**.

## <a name="a-managed-cloud-notebook-server"></a>Un servidor de cuadernos administrado en la nube

Es fácil empezar con su propio servidor de cuadernos basado en la nube. El cuaderno de ejemplo y el [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) ya están instalados y configurados para el usuario una vez creado este recurso en la nube.  

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

* Los ejemplos están disponibles en la página web del cuaderno.

## <a name="a-data-science-virtual-machine-dsvm"></a>Una instancia de Data Science Virtual Machine (DSVM)

El [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) y el servidor de cuadernos ya están instalados y configurados en DSVM. 

Después de [crear una instancia de DSVM](how-to-configure-environment.md#dsvm), utilice estos pasos en DSVM para ejecutar los cuadernos.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

## <a name="your-own-jupyter-notebook-server"></a>Su propio servidor de Jupyter Notebook

Siga estos pasos para crear un servidor de Jupyter Notebook local en su equipo.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Con las instrucciones de configuración se instalarán los paquetes necesarios para ejecutar los cuadernos de inicio rápido y del tutorial.  Otros cuadernos de ejemplo pueden requerir la instalación de componentes adicionales.  Para más información sobre estos componentes, consulte [Install the Azure Machine Learning SDK for Python](https://docs.microsoft.com/python/api/overview/azure/ml/install) (Instalación del SDK de Azure Machine Learning para Python).

## <a name="azure-notebooks"></a>Azure Notebooks

Los cuadernos de ejemplo y el [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) ya están instalados y configurados en [Azure Notebooks](https://notebooks.azure.com/). Tanto la instalación como las actualizaciones futuras se administran automáticamente mediante servicios de Azure.

Utilice [Azure Portal](https://portal.azure.com) para comenzar a usar Azure Notebooks.  Abra el área de trabajo y en la sección **Introducción**, seleccione **Introducción a Azure Notebooks**.

## <a name="next-steps"></a>Pasos siguientes

+ Explore los cuadernos de ejemplo de Azure Machine Learning Service en este repositorio de GitHub: https://aka.ms/aml-notebooks

Pruebe estos tutoriales:
+ [Entrenamiento de un modelo de clasificación de imágenes con el servicio Azure Machine Learning](tutorial-train-models-with-aml.md)

+ [Preparación de datos y uso de Machine Learning automatizado para entrenar un modelo de regresión con el conjunto de datos de los taxis de Nueva York](tutorial-data-prep.md)