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
ms.openlocfilehash: ac5656a8efe1dee932bfe753f8bdc06b787e20af
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75529801"
---
1. [Cree un área de trabajo de Azure Machine Learning](../articles/machine-learning/how-to-manage-workspace.md).

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Utilice cualquiera de estos métodos para agregar un archivo de configuración del área de trabajo al directorio clonado:

    * En [Azure Portal](https://ms.portal.azure.com), seleccione **Descargar config.json** desde la sección **Información general** del área de trabajo. 

    ![Descargar config.json](./media/aml-dsvm-server/download-config.png)

    * Cree un área de trabajo mediante código en el cuaderno [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) en su directorio clonado.

1. Inicie el servidor del cuaderno desde el directorio clonado.

    ```shell
    jupyter notebook
    ```