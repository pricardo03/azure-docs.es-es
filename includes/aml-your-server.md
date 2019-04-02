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
ms.openlocfilehash: 8d8b314965253dc00b39d0b068b1d6fb3e4aa471
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58395510"
---
1. Siga las instrucciones que encontrará en [Creación de un área de trabajo de Azure Machine Learning Service](../articles/machine-learning/service/setup-create-workspace.md#portal) para crear un entorno de Miniconda, crear un área de trabajo y escribir un archivo de configuración de área de trabajo (**aml_config/config.json**).

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Agregue un archivo de configuración del área de trabajo mediante uno de estos métodos:
    * Copie el archivo **aml_config/config.json** que creó mediante la información del inicio rápido de los requisitos previos en el directorio clonado.
    * Cree un área de trabajo mediante código en [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
1. Inicie el servidor del cuaderno desde el directorio clonado.
    
    ```shell
    jupyter notebook
    ```