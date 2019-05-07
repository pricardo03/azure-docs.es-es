---
title: 'Inicio rápido: Ejecución de un cuaderno en la nube'
titleSuffix: Azure Machine Learning service
description: Introducción a Azure Machine Learning Service. Use un servidor de cuadernos administrados en la nube para probar su área de trabajo.  El área de trabajo se encuentra en la nube y es el bloque fundamental que se utiliza para experimentar, entrenar e implementar modelos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 3d4127226037bf28ba677a49f6444ca987118cb9
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65023924"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Inicio rápido: Uso de un servidor de cuadernos basado en la nube para empezar a trabajar con Azure Machine Learning

Cree un servidor de cuadernos en la nube y úselo.  En este inicio rápido, va a ejecutar un código de Python que registra valores en el [área de trabajo de Azure Machine Learning Service](concept-azure-machine-learning-architecture.md). El área de trabajo se encuentra en la nube y es el bloque fundamental que se utiliza para experimentar, entrenar e implementar modelos de aprendizaje automático con Machine Learning. 

En este inicio rápido se muestra cómo crear un recurso de nube en el área de trabajo de Azure Machine Learning, configurado con el entorno de Python necesario para ejecutar Azure Machine Learning. Para usar su propio entorno, consulte [Inicio rápido: Uso de su propio servidor de cuadernos para empezar a trabajar con Azure Machine Learning](quickstart-run-local-notebook.md).  
 
En este artículo de inicio rápido, lleve a cabo las acciones siguientes:

* Cree un servidor de cuadernos en la nube en el área de trabajo.
* Inicie la interfaz web de Jupyter.
* Abra un cuaderno que contiene código para estimar PI y registre los errores en cada iteración.
* Ejecute el cuaderno.
* Ver los valores de los errores registrados en el área de trabajo.  En este ejemplo se muestra cómo el área de trabajo puede ayudarle a realizar un seguimiento de la información generada en un script. 

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Requisitos previos

- Un área de trabajo de Azure Machine Learning.  [Cree el área de trabajo](setup-create-workspace.md#portal) ahora si no tiene ya una.

## <a name="create-a-cloud-based-notebook-server"></a>Creación de un servidor de cuadernos en la nube

 En el área de trabajo puede crear un recurso en la nube para empezar a trabajar con cuadernos de Jupyter Notebook. Este recurso le ofrece una plataforma basada en la nube previamente configurada con todo lo que necesita para ejecutar Azure Machine Learning Service.

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).  Si no está seguro de cómo localizar el área de trabajo en el portal, consulte cómo [encontrar el área de trabajo](how-to-manage-workspace.md#view).

1. En la página del área de trabajo en Azure Portal, seleccione **Máquinas virtuales de Notebook** a la izquierda.

1. Seleccione **+Nuevo** para crear una máquina virtual de Jupyter Notebook.

     ![Seleccionar nueva máquina virtual](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. Proporcione un nombre para la máquina virtual. Seleccione **Crear**. 

    ![Creación de una máquina virtual nueva](media/quickstart-run-cloud-notebook/create-new-workstation.png)

1. Espere aproximadamente 4-5 minutos y luego seleccione **Actualizar**.  Pruebe a actualizar cada 30 segundos aproximadamente hasta que el estado sea **En ejecución**.

    ![Actualizar](media/quickstart-run-cloud-notebook/refresh.png)

## <a name="launch-jupyter-web-interface"></a>Inicio de la interfaz web de Jupyter

Una vez que la máquina virtual se esté ejecutando, use la sección **Máquinas virtuales de Notebook** para iniciar la interfaz web de Jupyter.

1. Seleccione **Jupyter** en la columna **URI** de la máquina virtual.  

    ![Iniciar el servidor de Jupyter Notebook](./media/quickstart-run-cloud-notebook/start-server.png)

    El vínculo inicia el servidor de cuadernos y abre la página web de Jupyter Notebook en una nueva pestaña del explorador.  Este vínculo solo funcionará para el usuario que cree la máquina virtual.

1. En la página web de Jupyter Notebook, seleccione la carpeta **samples/quickstart** para ver el cuaderno del inicio rápido.

## <a name="run-the-notebook"></a>Ejecución del cuaderno

Ejecute un cuaderno que calcule Pi y que registre el error en el área de trabajo.

1. Seleccione **01.run-experiment.ipynb** para abrir el cuaderno.

1. Puede que aparezca un mensaje que indica que no se ha establecido el kernel.  Seleccione **Python 3.6 - AzureML** y **Set Kernel** (Establecer kernel).

   ![Establecer kernel](./media/quickstart-run-cloud-notebook/set-kernel.png)

1. El área de estado le dice que espere hasta que se haya iniciado el kernel. El mensaje desaparece una vez que el kernel está listo.

    ![Esperar a que se inicie el kernel](./media/quickstart-run-cloud-notebook/wait-for-kernel.png)

1.  Haga clic en la primera celda de código y seleccione **Ejecutar**.

    > [!NOTE]
    > Las celdas de código tienen corchetes delante de ellas. Si los corchetes están vacíos (__[  ]__), no se ha ejecutado el código. Si el código está en ejecución, verá un asterisco (__[*]__). Una vez completado el código, aparecerá un número **[1]**.  El número le indica el orden en que se ejecutaron las celdas.
    >
    > Use **MAYÚS+ENTRAR** como acceso directo para ejecutar una celda.

    ![Ejecutar la primera celda de código](media/quickstart-run-cloud-notebook/cell1.png)

1. Ejecute la segunda celda de código. Si ve instrucciones para autenticarse, copie el código y siga el vínculo para iniciar sesión. Una vez que inicie sesión, el explorador recordará esta configuración.  

    > [!TIP]
    > Asegúrese de no copiar el espacio que aparece después del código.  

    ![Autenticar](media/quickstart-run-cloud-notebook/authenticate.png)

1. Cuando haya terminado, aparecerá el número de celda __[2]__.  Si tuvo que iniciar sesión, verá un mensaje de estado de autenticación correcta.   Si no tiene que iniciar sesión, no verá ningún resultado en esta celda, solo el número que aparece para mostrar que la celda se ejecutó correctamente.

    ![Mensaje de operación correcta](media/quickstart-run-cloud-notebook/success.png)

1. Ejecute el resto de celdas de código.  A medida que finaliza la ejecución de cada celda, verá que aparece su número de celda. Solo la última celda puede mostrar cualquier otro resultado.  En la celda de código más grande, podrá ver que `run.log` se usa en varios lugares. Cada `run.log` agrega su valor al área de trabajo.


## <a name="view-logged-values"></a>Visualización de los datos registrados

1. La salida de la celda `run` contiene un vínculo que lleva de vuelta a Azure Portal para que vea los resultados del experimento en el área de trabajo. 

    ![Visualización de experimentos](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Haga clic en el **vínculo a Azure Portal** para ver información sobre la ejecución en el área de trabajo.  Este vínculo abre el área de trabajo en Azure Portal.

1. Los trazados de los valores registrados que ve se crearon automáticamente en el área de trabajo. Cada vez que registre varios valores con el mismo parámetro de nombre, se genera automáticamente un trazado.

   ![Visualización del historial](./media/quickstart-run-cloud-notebook/web-results.png)

Dado que el código para la aproximación de pi usa valores aleatorios, los trazados mostrarán otros valores.  

## <a name="clean-up-resources"></a>Limpieza de recursos 

### <a name="stop-the-notebook-vm"></a>Detención de la máquina virtual de Notebook

Detenga la máquina virtual de Notebook cuando no esté utilizándola para reducir los costos.  

1. En el área de trabajo, seleccione **Máquinas virtuales de Notebook**.

   ![Detener el servidor de máquina virtual](./media/quickstart-run-cloud-notebook/stop-server.png)

1. En la lista, seleccione la máquina virtual.

1. Seleccione **Detener**.

1. Cuando esté listo para usar el servidor de nuevo, seleccione **Iniciar**.

### <a name="delete-everything"></a>Eliminar todo el contenido

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

También puede mantener el grupo de recursos pero eliminar una sola área de trabajo. Muestre las propiedades del área de trabajo y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha completado estas tareas:

* Creación de una máquina virtual de Notebook
* Inicio de un servidor de Jupyter Notebook en la máquina virtual de Notebook
* Abra un cuaderno que contiene código para estimar PI y registre los errores en cada iteración.
* Ejecute el cuaderno.
* Ver los valores de los errores registrados en el área de trabajo.  En este ejemplo se muestra cómo el área de trabajo puede ayudarle a realizar un seguimiento de la información generada en un script. 

Para ver de forma detallada la experiencia de flujo de trabajo, siga los tutoriales de Machine Learning para entrenar e implementar un modelo:  

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de un modelo de clasificación de imágenes](tutorial-train-models-with-aml.md)