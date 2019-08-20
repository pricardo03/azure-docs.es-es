---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 01/25/2019
ms.openlocfilehash: 6c93d1243db1b3c4277a54cf71e10f6bbc648d26
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68846023"
---
- El SDK de Azure Machine Learning para Python instalado. Siga las instrucciones que se indican en el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) para hacer lo siguiente:


1. Siga las instrucciones que se indican en el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) para hacer lo siguiente:
    * Creación de un entorno de Miniconda [Creación y administración de áreas de trabajo de Azure Machine Learning Service]
    * Instalar el SDK de Azure Machine Learning para Python

1. Cree un área de trabajo de [Azure Machine Learning Service](../articles/machine-learning/service/how-to-manage-workspace.md).

1. Escriba un [archivo de configuración](../articles/machine-learning/service/how-to-configure-environment.md#workspace) (**aml_config/config.json**).

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Inicie el servidor del cuaderno desde el directorio clonado.

    ```shell
    jupyter notebook
    ```