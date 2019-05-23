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
ms.openlocfilehash: 8d21e41ad487ad17598f2320fab5eebae02309e8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66123241"
---
1. Use las instrucciones del artículo [Create an Azure Machine Learning service workspace](../articles/machine-learning/service/setup-create-workspace.md#portal) (Creación de un área de trabajo de Azure Machine Learning Service) para hacer lo siguiente:
    * Crear un entorno Miniconda
    * Instalar el SDK de Azure Machine Learning para Python
    * Crear un área de trabajo
    * Escribir un archivo de configuración de área de trabajo (**aml_config/config.json**).
    
1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Agregue un archivo de configuración de área de trabajo con uno de estos métodos:
    * Copie el archivo **aml_config/config.json** que creó en el paso 1 en el directorio clonado.

    * En [Azure Portal](https://ms.portal.azure.com), seleccione **Descargar config.json** desde la sección **Información general** del área de trabajo. 

    ![Descargar config.json](./media/aml-dsvm-server/download-config.png)

    * Cree un área de trabajo mediante código en [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Inicie el servidor del cuaderno desde el directorio clonado.
    
    ```shell
    jupyter notebook
    ```