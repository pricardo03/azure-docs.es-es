---
title: 'Tutorial: Creación de su primer experimento de Machine Learning: Configuración'
titleSuffix: Azure Machine Learning service
description: En esta serie de tutoriales, realizará los pasos completos para empezar a trabajar con el SDK de Python para Azure Machine Learning que se ejecuta en cuadernos de Jupyter.  La primera parte cubre la creación de un entorno de servidor de cuaderno en la nube, así como la creación de un área de trabajo para administrar los experimentos y los modelos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 08/28/2019
ms.openlocfilehash: f1003324e9f4b3762b5d8eca703af4a1fbd4613a
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308817"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment-with-the-python-sdk"></a>Tutorial: Creación del primer experimento de ML con el SDK de Python

En este tutorial, realizará los pasos de un extremo a otro para empezar a trabajar con el SDK de Python para Azure Machine Learning que se ejecuta en cuadernos de Jupyter. Este tutorial es la **parte uno de una serie de tutoriales de dos partes**, y abarca la instalación y la configuración del entorno de Python, así como la creación de un área de trabajo para administrar los experimentos y los modelos de aprendizaje automático. En la [**segunda parte**](tutorial-1st-experiment-sdk-train.md) se usa este área de trabajo para entrenar varios modelos de aprendizaje automático e introducir el proceso de administración de modelos mediante Azure Portal y el SDK.

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Crear un [área de trabajo de Azure Machine Learning](concept-workspace.md) para usarla en el siguiente tutorial.
> * Cree una VM de Jupyter Notebook basada en la nube que tenga instalado y preconfigurado el SDK de Python de Azure Machine Learning.

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="create-a-workspace"></a>Crear un área de trabajo

Un área de trabajo de Azure Machine Learning es un recurso básico de la nube que se usa para experimentar, entrenar e implementar modelos de aprendizaje automático. Vincula la suscripción y el grupo de recursos de Azure con un objeto fácilmente consumido del SDK. Si ya tiene un área de trabajo de Azure Machine Learning Service, vaya a la [siguiente sección](#azure). En caso contrario, créela ahora.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="azure"></a>Creación de un servidor de cuadernos en la nube

En este ejemplo se usa el servidor de cuadernos en la nube en el área de trabajo para obtener una experiencia sin instalación y configurada previamente. Si prefiere tener control sobre su entorno, los paquetes y las dependencias, use [su propio entorno](how-to-configure-environment.md#local).

En el área de trabajo puede crear un recurso en la nube para empezar a trabajar con cuadernos de Jupyter Notebook. Este recurso es una máquina virtual Linux basada en la nube configurada previamente con todo lo necesario para ejecutar Azure Machine Learning Service.

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).  Si no está seguro de cómo localizar el área de trabajo en el portal, consulte cómo [encontrar el área de trabajo](how-to-manage-workspace.md#view).

1. En la página del área de trabajo en Azure Portal, seleccione **Máquinas virtuales de Notebook** a la izquierda.

1. Seleccione **+Nuevo** para crear una máquina virtual de Jupyter Notebook.

     ![Seleccionar nueva máquina virtual](./media/tutorial-1st-experiment-sdk-setup/add-workstation.png)

1. Proporcione un nombre para la máquina virtual. 
   + El nombre de la máquina virtual del cuaderno debe tener entre 2 y 16 caracteres. Los caracteres válidos son letras, dígitos y el carácter -.  
   + El nombre también debe ser único en la suscripción de Azure.

1. Seleccione **Crear**. La configuración de la máquina virtual puede tardar un momento.

1. Espere hasta que el estado cambie a **En ejecución**.
   Una vez que la máquina virtual se esté ejecutando, use la sección **Máquinas virtuales de Notebook** para iniciar la interfaz web de Jupyter.

1. Seleccione **Jupyter** en la columna **URI** de la máquina virtual.

    ![Iniciar el servidor de Jupyter Notebook](./media/tutorial-1st-experiment-sdk-setup/start-server.png)

   El vínculo inicia el servidor de cuadernos y abre la página web de Jupyter Notebook en una nueva pestaña del explorador.  Este vínculo solo funcionará para el usuario que cree la máquina virtual. Cada usuario del área de trabajo debe crear su propia máquina virtual.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha completado estas tareas:

* Ha creado un área de trabajo de Azure Machine Learning Service.
* Ha creado y configurado un servidor de cuadernos en la nube en el área de trabajo.

En la **segunda parte** del tutorial, se ejecuta el código en `tutorial-1st-experiment-sdk-train.ipynb` para entrenar un modelo de Machine Learning. 

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento del primer modelo](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Si no está pensando en la segunda parte de este tutorial o en otros tutoriales, debe [detener la máquina virtual del servidor de cuadernos en la nube](tutorial-1st-experiment-sdk-train.md#clean-up-resources) cuando no se use para reducir el costo.
