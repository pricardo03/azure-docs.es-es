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
ms.openlocfilehash: 02ef0d6c7c8ddc7088938d9c8ea379e3b97f3045
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158722"
---
1. [Cree un área de trabajo del servicio Azure Machine Learning](../articles/machine-learning/service/setup-create-workspace.md).

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```
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