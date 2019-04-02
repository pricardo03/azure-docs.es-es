---
title: Inicio rápido para ejecutar un cuaderno en la nube
titleSuffix: Azure Machine Learning service
description: Introducción a Azure Machine Learning Service. Use un servidor de cuadernos administrados en la nube para probar su área de trabajo.  El área de trabajo se encuentra en la nube y es el bloque fundamental que se utiliza para experimentar, entrenar e implementar modelos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 03/21/2019
ms.custom: seodec18
ms.openlocfilehash: 761ecefa768914e0c27f46d37050743e4f1f60ff
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58362362"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Inicio rápido: Uso de un servidor de cuadernos basado en la nube para empezar a trabajar con Azure Machine Learning

En este artículo, usará Azure Notebooks para ejecutar el código registrado en el [área de trabajo](concept-azure-machine-learning-architecture.md) de Azure Machine Learning Service. El área de trabajo se encuentra en la nube y es el bloque fundamental que se utiliza para experimentar, entrenar e implementar modelos de aprendizaje automático con Machine Learning. 

Esta guía de inicio rápido usa recursos en la nube y no requiere ninguna instalación. Para usar su propio entorno, consulte [Inicio rápido: Uso de su propio servidor de cuadernos para empezar a trabajar con Azure Machine Learning](quickstart-run-local-notebook.md).  
 
En este artículo de inicio rápido, lleve a cabo las acciones siguientes:

* Conectarse a su área de trabajo con Python en un cuaderno de Jupyter. El cuaderno contiene código para estimar PI y registra los errores en cada iteración. 
* Ver los valores de los errores registrados en el área de trabajo.

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisite"></a>Requisito previo

1. [Cree un área de trabajo de Azure Machine Learning](setup-create-workspace.md#portal) si aún no tiene una.

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).  Vea cómo [encontrar su área de trabajo](how-to-manage-workspace.md#view).

## <a name="use-your-workspace"></a>Uso del área de trabajo

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]



Aprenda cómo un área de trabajo ayuda a administrar los scripts de aprendizaje automático. En esta sección, lleve a cabo estos pasos:

* Va a abrir un cuaderno en Azure Notebooks.
* Ejecutará código que cree algunos valores registrados.
* Verá los valores registrados en el área de trabajo.

En este ejemplo se muestra cómo el área de trabajo puede ayudarle a realizar un seguimiento de la información generada en un script. 

### <a name="open-a-notebook"></a>Abra un cuaderno. 

[Azure Notebooks](https://notebooks.azure.com) ofrece una plataforma de nube gratuita para cuadernos de Jupyter preconfigurada con todo lo que necesita para ejecutar Machine Learning. Desde el área de trabajo, puede iniciar esta plataforma para empezar a usar el área de trabajo de Azure Machine Learning Service.

1. En la página de información general del área de trabajo, seleccione **Introducción a Azure Notebooks** para probar su primer experimento en Azure Notebooks.  Azure Notebooks es un servicio independiente que permite ejecutar cuadernos de Jupyter de manera gratuita en la nube.  Cuando se usa este vínculo al servicio, se agrega a la biblioteca que crea en Azure Notebooks información sobre cómo conectarse al área de trabajo.

   ![Exploración del área de trabajo](./media/quickstart-run-cloud-notebook/explore-aml.png)

1. Inicie sesión en Azure Notebooks.  Asegúrese de hacerlo con la misma cuenta que usó para iniciar sesión en Azure Portal. Su organización puede requerir el [consentimiento del administrador](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) para poder iniciar sesión.

1. Después de iniciar sesión, se abrirá una nueva pestaña y un aviso `Clone Library`. Al clonarse esta biblioteca se cargará un conjunto de cuadernos y otros archivos en su cuenta de Azure Notebooks.  Estos archivos le ayudan a explorar las funcionalidades de Azure Machine Learning.

1. Desactive la opción **Public** (Público) para evitar compartir la información de su área de trabajo con otros usuarios.

1. Seleccione **Clonar**.

   ![Clonación de una biblioteca](./media/quickstart-run-cloud-notebook/clone.png)

1. Si observa que el estado del proyecto indica que está detenido, haga clic en **Run on Free Compute** (Ejecutar en proceso gratuito) para usar el servidor de cuaderno gratuito.

    ![Ejecución de un proyecto en un proceso gratuito](./media/quickstart-run-cloud-notebook/run-project.png)

### <a name="run-the-notebook"></a>Ejecución del cuaderno

En la lista de archivos de este proyecto, verá un archivo `config.json`. Este archivo de configuración contiene información sobre el área de trabajo que creó en Azure Portal.  Este archivo permite que el código se conecte al área de trabajo y agregue información a esta.

1. Seleccione **01.run-experiment.ipynb** para abrir el cuaderno.

1. El área de estado le dice que espere hasta que se haya iniciado el kernel.  El mensaje desaparece una vez que el kernel está listo.

    ![Esperar a que se inicie el kernel](./media/quickstart-run-cloud-notebook/wait-for-kernel.png)

1. Una vez iniciado el kernel, ejecute las celdas de una en una mediante **Mayús + Intro**. O bien, seleccione **Cells** > **Run All** (Celdas > Ejecutar todo) para ejecutar todo el cuaderno. Si ve un asterisco, __*__, junto a una celda, significa que aún se está ejecutando la celda. Cuando finalice el código de esa celda, aparece un número. 

1. Siga las instrucciones que aparecen en el cuaderno para autenticar su suscripción de Azure.

Cuando haya terminado de ejecutar todas las celdas del cuaderno, podrá ver los valores registrados en el área de trabajo.

## <a name="view-logged-values"></a>Visualización de los datos registrados

1. La salida de la celda `run` contiene un vínculo que lleva de vuelta a Azure Portal para que vea los resultados del experimento en el área de trabajo. 

    ![Visualización de experimentos](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Haga clic en el **vínculo a Azure Portal** para ver información sobre la ejecución en el área de trabajo.  Este vínculo abre el área de trabajo en Azure Portal.

1. Los trazados de los valores registrados que ve se crearon automáticamente en el área de trabajo. Cada vez que registre varios valores con el mismo parámetro de nombre, se genera automáticamente un trazado.

   ![Visualización del historial](./media/quickstart-run-cloud-notebook/web-results.png)

Dado que el código para la aproximación de pi usa valores aleatorios, los trazados mostrarán otros valores.  

## <a name="clean-up-resources"></a>Limpieza de recursos 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

También puede mantener el grupo de recursos pero eliminar una sola área de trabajo. Muestre las propiedades del área de trabajo y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Ha creado los recursos necesarios para experimentar con modelos e implementarlos. También ha ejecutado algún código en un cuaderno. Y ha explorado el historial de ejecución desde dicho código en el área de trabajo en la nube.

Para ver de forma detallada la experiencia de flujo de trabajo, siga los tutoriales de Machine Learning para entrenar e implementar un modelo:  

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de un modelo de clasificación de imágenes](tutorial-train-models-with-aml.md)
