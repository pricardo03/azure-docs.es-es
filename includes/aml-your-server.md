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
ms.date: 09/26/2019
ms.openlocfilehash: 1260ee32c267a2652ad02e74c3978654bf2f9669
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75529800"
---
1. Use las instrucciones del [SDL de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) para instalar el SDK de Azure Machine Learning para Python

1. Cree un [área de trabajo de Azure Machine Learning](../articles/machine-learning/how-to-manage-workspace.md).

1. Escriba un [archivo de configuración](../articles/machine-learning/how-to-configure-environment.md#workspace) (**aml_config/config.json**).

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Inicie el servidor del cuaderno desde el directorio clonado.

    ```shell
    jupyter notebook
    ```
