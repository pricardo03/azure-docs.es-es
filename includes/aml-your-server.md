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
ms.openlocfilehash: 18ba86ce7876ba8275eb4853e4fc9ea0f35fa186
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302217"
---
1. Complete el [inicio rápido de Azure Machine Learning en Python](../articles/machine-learning/service/quickstart-create-workspace-with-python.md) para instalar el SDK y crear un área de trabajo.  Si lo desea, omita la sección **Uso del cuaderno**.
1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Agregue un archivo de configuración del área de trabajo mediante uno de estos métodos:
    * Copie el archivo **aml_config\config.json** que creó mediante la información de la guía de inicio rápido de los requisitos previos en el directorio clonado.
    * Cree un área de trabajo mediante código en [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
1. Inicie el servidor del cuaderno desde el directorio clonado.
    
    ```shell
    jupyter notebook
    ```