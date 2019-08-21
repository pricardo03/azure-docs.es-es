---
title: 'Tutorial: configuración del entorno y el área de trabajo.'
titleSuffix: Azure Machine Learning service
description: En esta serie de tutoriales, realizará los pasos completos para empezar a trabajar con el SDK de Python para Azure Machine Learning que se ejecuta en cuadernos de Jupyter.  La primera parte cubre la creación de un entorno de servidor de cuaderno en la nube, así como la creación de un área de trabajo para administrar los experimentos y los modelos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 07/20/2019
ms.openlocfilehash: 70fee90fb82618a409d2566a3235ad8ca42e1760
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934426"
---
# <a name="tutorial-set-up-environment-and-workspace"></a>Tutorial: configuración del entorno y el área de trabajo

En este tutorial, realizará los pasos de un extremo a otro para empezar a trabajar con el SDK de Python para Azure Machine Learning que se ejecuta en cuadernos de Jupyter. Este tutorial es la **parte uno de una serie de tutoriales de dos partes**, y abarca la instalación y la configuración del entorno de Python, así como la creación de un área de trabajo para administrar los experimentos y los modelos de aprendizaje automático. En la [**segunda parte**](tutorial-1st-experiment-sdk-train.md) se usa este área de trabajo para entrenar varios modelos de aprendizaje automático e introducir el proceso de administración de modelos mediante Azure Portal y el SDK.

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Creará un área de trabajo de aprendizaje automático para usarla en el siguiente tutorial
> * Cree una VM de Jupyter Notebook basada en la nube que tenga instalado y preconfigurado el SDK de Python de Azure Machine Learning.

## <a name="prerequisites"></a>Requisitos previos

El único requisito previo de este tutorial es una suscripción a Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="create-a-workspace"></a>Crear un área de trabajo

Un área de trabajo es un recurso básico de la nube que se usa para experimentar, entrenar e implementar modelos de aprendizaje automático. Vincula la suscripción y el grupo de recursos de Azure con un objeto fácilmente consumido del SDK. Si ya tiene un área de trabajo de Azure Machine Learning Service, vaya a la [siguiente sección](#azure). En caso contrario, créela ahora.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="azure"></a>Creación de un servidor de cuadernos en la nube

En este ejemplo se usa el servidor de cuadernos en la nube en el área de trabajo para obtener una experiencia sin instalación y configurada previamente. Si prefiere tener control sobre su entorno, los paquetes y las dependencias, use [su propio entorno](how-to-configure-environment.md#local).

En el área de trabajo puede crear un recurso en la nube para empezar a trabajar con cuadernos de Jupyter Notebook. Este recurso es una máquina virtual Linux basada en la nube configurada previamente con todo lo necesario para ejecutar Azure Machine Learning Service.

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).  Si no está seguro de cómo localizar el área de trabajo en el portal, consulte cómo [encontrar el área de trabajo](how-to-manage-workspace.md#view).

1. En la página del área de trabajo en Azure Portal, seleccione **Máquinas virtuales de Notebook** a la izquierda.

1. Seleccione **+Nuevo** para crear una máquina virtual de Jupyter Notebook.

     ![Seleccionar nueva máquina virtual](./media/tutorial-1st-experiment-sdk-setup/add-workstation.png)

1. Proporcione un nombre para la máquina virtual. Seleccione **Crear**.

    > [!NOTE]
    > El nombre de la máquina virtual del cuaderno debe tener entre 2 y 16 caracteres. Los caracteres válidos son letras, dígitos y el carácter -.  El nombre también debe ser único en la suscripción de Azure.

1. Espere hasta que el estado cambie a **En ejecución**.

### <a name="launch-jupyter-web-interface"></a>Inicio de la interfaz web de Jupyter

Una vez que la máquina virtual se esté ejecutando, use la sección **Máquinas virtuales de Notebook** para iniciar la interfaz web de Jupyter.

1. Seleccione **Jupyter** en la columna **URI** de la máquina virtual.

    ![Iniciar el servidor de Jupyter Notebook](./media/tutorial-1st-experiment-sdk-setup/start-server.png)

    El vínculo inicia el servidor de cuadernos y abre la página web de Jupyter Notebook en una nueva pestaña del explorador.  Este vínculo solo funcionará para el usuario que cree la máquina virtual. Cada usuario del área de trabajo debe crear su propia máquina virtual.

1. En la página web de Jupyter Notebook, el nombre de la carpeta principal es su nombre de usuario.  Seleccione esta carpeta.

    > [!TIP]
    > Esta carpeta se encuentra en el [contenedor de almacenamiento](concept-workspace.md#resources) del área de trabajo en lugar de en la propia máquina virtual del cuaderno.  Puede eliminar la máquina virtual del cuaderno y conservar todo el trabajo.  Cuando cree una máquina virtual de cuaderno más adelante, se cargará esta misma carpeta. Si comparte el área de trabajo con otras personas, estas verán su carpeta y usted las suyas.

1. Abra el subdirectorio `samples-*` y luego abra `tutorials/tutorial-1st-experiment-sdk-train.ipynb` para ejecutar la **segunda parte** del tutorial.

## <a name="end"></a> Limpieza de recursos

No complete esta sección si tiene pensado continuar con la **parte 2** del tutorial.

### <a name="stop-the-notebook-vm"></a>Detención de la máquina virtual de Notebook

Si usó un servidor de cuadernos en la nube, detenga la máquina virtual cuando no la esté usando a fin de reducir el costo.

1. En el área de trabajo, seleccione **Máquinas virtuales de Notebook**.

   ![Detener el servidor de máquina virtual](./media/tutorial-1st-experiment-sdk-setup/stop-server.png)

1. En la lista, seleccione la máquina virtual.

1. Seleccione **Detener**.

1. Cuando esté listo para volver a usar el servidor, seleccione **Iniciar**.

### <a name="delete-everything"></a>Eliminar todo el contenido

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

También puede mantener el grupo de recursos pero eliminar una sola área de trabajo. Muestre las propiedades del área de trabajo y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha completado estas tareas:

* Ha creado un área de trabajo de Azure Machine Learning Service.
* Ha creado y configurado un servidor de cuadernos en la nube en el área de trabajo.

Continúe con la **parte 2** de este tutorial para entrenar un modelo de aprendizaje automático sencillo.

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento del primer modelo](tutorial-1st-experiment-sdk-train.md)
