---
title: Cuadernos de Jupyter Notebook de ejemplo
titleSuffix: Azure Machine Learning service
description: Busque y use cuadernos de Jupyter de ejemplo para explorar Azure Machine Learning Service en Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 961983aad0775f9b9d728269e8a57137ff508f02
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57451797"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Uso de cuadernos de Jupyter para explorar Azure Machine Learning Service

Para su comodidad, hemos desarrollado una serie de cuadernos de Jupyter Python que puede usar para explorar Azure Machine Learning Service. 

Aprenda a usar el servicio con la documentación de este sitio y use estos cuadernos para adecuarlos a su situación. 

Utilice una de las siguientes rutas de acceso para ejecutar un servidor de cuadernos con estos cuadernos de ejemplo.  Una vez que el servidor se esté ejecutando, busque los cuadernos de tutoriales en la carpeta **tutorials** o explore las diferentes características en la carpeta **how-to-use-azureml**.


## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Prueba de Azure Notebooks: cuadernos de Jupyter gratuitos en la nube

Es fácil empezar a usar Azure Notebooks. El [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) ya está instalado y configurado en [Azure Notebooks](https://notebooks.azure.com/). Tanto la instalación como las actualizaciones futuras se administran automáticamente mediante servicios de Azure.
  
[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Uso de Data Science Virtual Machine (DSVM)

El [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) y el servidor de cuadernos ya están instalados y configurados en DSVM. 

Después de [crear una instancia de DSVM](how-to-configure-environment.md#dsvm), utilice estos pasos en DSVM para ejecutar los cuadernos.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]


## <a name="use-your-own-jupyter-notebook-server"></a>Uso del propio servidor de Jupyter Notebook

Siga estos pasos para crear un servidor de Jupyter Notebook local en su equipo.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Con las instrucciones de inicio rápido se instalarán los paquetes necesarios para ejecutar los cuadernos de inicio rápido y del tutorial.  Otros cuadernos de ejemplo pueden requerir la instalación de componentes adicionales.  Para más información sobre estos componentes, consulte [Install the Azure Machine Learning SDK for Python](https://docs.microsoft.com/python/api/overview/azure/ml/install) (Instalación del SDK de Azure Machine Learning para Python).

<a name="automated-ml-setup"></a>

## <a name="automated-machine-learning-setup"></a>Configuración del aprendizaje automático automatizado 

_Estos pasos se aplican solo a los cuadernos de la carpeta **how-to-use-azureml/automated-machine-learning**._

Aunque puede usar cualquiera de las opciones anteriores, también puede instalar el entorno y crear un área de trabajo al mismo tiempo con las instrucciones siguientes. 

1. Instale [Mini conda](https://conda.io/miniconda.html). Elija la versión 3.7, o cualquier versión superior. Siga las indicaciones para realizar la instalación. 
   >[!NOTE]
   >Puede usar cualquier sistema conda existente, siempre que tenga la versión 4.4.10 o posterior. Use `conda -V` para mostrar la versión. La versión de conda se puede actualizar con el comando: `conda update conda`. No es preciso instalar mini conda específicamente.

1. Descargue los cuadernos de ejemplo de [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) en forma de archivo zip y extraiga el contenido en un directorio local. Los cuadernos de Machine Learning automático están en la carpeta `how-to-use-azureml/automated-machine-learning`.

1. Configure un nuevo entorno de Conda. 
   1. Abra un símbolo del sistema de Conda en el equipo local.
   
   1. Vaya a los archivos que extrajo en el equipo local.
   
   1. Abra la carpeta **automated-machine-learning**.
   
   1. Ejecute `automl_setup.cmd` en el símbolo del sistema de Conda para Windows o el archivo `.sh` de su sistema operativo. Puede tardar unos 10 minutos en ejecutarse.

      El script de configuración:
      + Crea un nuevo entorno de conda
      + Instala los paquetes necesarios
      + Configura el widget
      + Inicia un cuaderno de Jupyter
      
   >[!NOTE]
   > El script toma el nombre del entorno de conda como parámetro opcional. El nombre del entorno de conda predeterminado es `azure_automl`. El comando exacto depende del sistema operativo. Esto es útil si está creando un nuevo entorno o actualizando a una nueva versión. Por ejemplo puede usar "automl_setup.cmd azure_automl_sandbox" para crear un nombre de entorno azure_automl_sandbox. 
      
1. Una vez que se ha completado el script, verá la página principal de un cuaderno de Jupyter en el explorador.

1. Vaya a la ruta de acceso en que guardó los cuadernos. 

1. Abra la carpeta automated-machine-learning y, después, el cuaderno **configuration.ipynb**. 

1. Ejecute las celdas del cuaderno para registrar el proveedor de recursos de Machine Learning Services y crear un área de trabajo.

Ya está listo para abrir los blocs de notas que guardados en el equipo local y ejecutarlos.


## <a name="next-steps"></a>Pasos siguientes

Explore la [repositorio de cuadernos de GitHub para Azure Machine Learning Service](https://aka.ms/aml-notebooks)

Pruebe estos tutoriales:
+ [Entrenamiento de un modelo de clasificación de imágenes con el servicio Azure Machine Learning](tutorial-train-models-with-aml.md)

+ [Preparación de datos y uso de Machine Learning automatizado para entrenar un modelo de regresión con el conjunto de datos de los taxis de Nueva York](tutorial-data-prep.md)
