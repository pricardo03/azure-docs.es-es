---
title: Guía de inicio rápido en Azure Portal
titleSuffix: Azure Machine Learning service
description: Introducción a Azure Machine Learning Service. Use Azure Portal para crear un área de trabajo, que es el bloque fundamental en la nube que se utiliza para experimentar, entrenar e implementar modelos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 14c500d77cc0e67aaade5e6be490f599f39bfad5
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807727"
---
# <a name="quickstart-use-the-azure-portal-to-get-started-with-azure-machine-learning"></a>Guía de inicio rápido: Uso de Azure Portal para empezar a trabajar con Azure Machine Learning

En esta guía de inicio rápido, aprenderá a usar Azure Portal para crear un área de trabajo de Azure Machine Learning. Esta área de trabajo se encuentra en la nube y es el bloque fundamental que se utiliza para experimentar, entrenar e implementar modelos de aprendizaje automático con Machine Learning. Esta guía de inicio rápido usa recursos en la nube y no requiere ninguna instalación. Para configurar en su lugar un servidor de Jupyter Notebook propio, vea [Inicio rápido: Uso de Python para empezar a usar Azure Machine Learning](quickstart-create-workspace-with-python.md).  
 
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]

En este artículo de inicio rápido, lleve a cabo las acciones siguientes:

* Crear un área de trabajo en la suscripción de Azure.
* Pruébela con Python en un cuaderno de Azure Notebooks y registre los valores en varias iteraciones.
* Verá los valores registrados en el área de trabajo.

Los siguientes recursos de Azure se agregan automáticamente al área de trabajo cuando estén disponibles por regiones:

  - [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
  - [Azure Storage](https://azure.microsoft.com/services/storage/)
  - [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
  - [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

Los recursos que cree se pueden usar como requisitos previos para otros tutoriales y artículos de procedimientos de Machine Learning. Al igual que en otros servicios de Azure, existen límites en determinados recursos asociados con Machine Learning. Un ejemplo es el tamaño del clúster de proceso. Obtenga más información sobre los [límites predeterminados y cómo aumentar la cuota](how-to-manage-quotas.md).

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).


## <a name="create-a-workspace"></a>Crear un área de trabajo 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

En la página del área de trabajo, seleccione `Explore your Azure Machine Learning service Workspace`.

 ![Exploración del área de trabajo](./media/quickstart-get-started/explore_aml.png)


## <a name="use-the-workspace"></a>Uso del área de trabajo

Vea ahora cómo un área de trabajo lo ayuda a administrar los scripts de aprendizaje automático. En esta sección, lleve a cabo estos pasos:

* Va a abrir un cuaderno en Azure Notebooks.
* Ejecutará código que cree algunos valores registrados.
* Verá los valores registrados en el área de trabajo.

En este ejemplo se muestra cómo el área de trabajo puede ayudarle a realizar un seguimiento de la información generada en un script. 

### <a name="open-a-notebook"></a>Abra un cuaderno. 

Azure Notebooks ofrece una plataforma de nube gratuita para cuadernos de Jupyter Notebook preconfigurada con todo lo que necesita para ejecutar Machine Learning.  

Seleccione `Open Azure Notebooks` para probar su primer experimento.

 ![Apertura de Azure Notebooks](./media/quickstart-get-started/explore_ws.png)

Su organización puede requerir el [consentimiento del administrador](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) para poder iniciar sesión.

Inicie sesión en Azure Notebooks con la misma cuenta que usó para iniciar sesión en Azure Portal.  Después de iniciar sesión, se abrirá una nueva pestaña y un aviso `Clone Library`. Seleccione `Clone`.


### <a name="run-the-notebook"></a>Ejecución del cuaderno

Además de los dos cuadernos, se ve un archivo `config.json`. Este archivo de configuración contiene información sobre el área de trabajo que ha creado.  

Seleccione `01.run-experiment.ipynb` para abrir el cuaderno.

Ejecute las celdas de una en una (Mayús+Entrar). O bien, seleccione `Cells` > `Run All` para ejecutar todo el cuaderno. Un asterisco, __*__, junto a una celda, indica que está en ejecución. Cuando finalice el código de esa celda, aparece un número. 

Cuando haya terminado de ejecutar todas las celdas del cuaderno, podrá ver los valores registrados en el área de trabajo.

## <a name="view-logged-values"></a>Visualización de los datos registrados

Cuando se hayan ejecutado todas las celdas del cuaderno, vuelva a la página del portal.  

Seleccione `View Experiments`.

![Visualización de experimentos](./media/quickstart-get-started/view_exp.png)

Cierre la ventana emergente `Reports`.

Seleccione `my-first-experiment`.

Consulte la información sobre la ejecución que acaba de realizar. Desplácese hacia abajo en la página para encontrar la tabla de ejecuciones. Seleccione el vínculo del número de ejecución.

 ![Vínculo del historial de ejecución](./media/quickstart-get-started/report.png)

Consulte los trazados que se crearon automáticamente de los valores registrados. Cada vez que registre varios valores con el mismo parámetro de nombre, se genera automáticamente un trazado.

   ![Visualización del historial](./media/quickstart-get-started/plots.png)

Dado que el código para la aproximación de pi usa valores aleatorios, los trazados mostrarán otros valores.  

## <a name="clean-up-resources"></a>Limpieza de recursos 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

También puede mantener el grupo de recursos pero eliminar una sola área de trabajo. Muestre las propiedades del área de trabajo y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Ha creado los recursos necesarios para experimentar con modelos e implementarlos. También ha ejecutado algún código en un cuaderno. Y ha explorado el historial de ejecución desde dicho código en el área de trabajo en la nube.

Para ver de forma detallada la experiencia de flujo de trabajo, siga los tutoriales de Machine Learning para entrenar e implementar un modelo:  

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de un modelo de clasificación de imágenes](tutorial-train-models-with-aml.md)
