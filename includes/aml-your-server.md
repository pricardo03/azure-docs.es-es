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
ms.openlocfilehash: ce8b117a3cbe0e3a5c4265729ccf5c0264241013
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391773"
---
1. Use las instrucciones del artículo [Create an Azure Machine Learning service workspace](../articles/machine-learning/service/setup-create-workspace.md#portal) (Creación de un área de trabajo de Azure Machine Learning Service) para hacer lo siguiente:
    * Crear un entorno Miniconda
    * Instalar el SDK de Azure Machine Learning para Python
    * Crear un área de trabajo
    * Escribir un archivo de configuración de área de trabajo (**aml_config/config.json**).

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Inicie el servidor del cuaderno desde el directorio clonado.

    ```shell
    jupyter notebook
    ```