---
title: Cuadernos de Jupyter Notebook de ejemplo
titleSuffix: Azure Machine Learning service
description: Busque y use cuadernos de Jupyter de ejemplo para explorar Azure Machine Learning Service en Python.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6befe3a3fee80dd65fd3ac5be241c558707224e6
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811101"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Uso de cuadernos de Jupyter para explorar Azure Machine Learning Service


Para su comodidad, hemos desarrollado una serie de cuadernos de Jupyter Python que puede usar para explorar Azure Machine Learning Service. 

Aprenda a usar el servicio con la documentación de este sitio y use estos cuadernos para adecuarlos a su situación. 

## <a name="prerequisite"></a>Requisito previo

Complete la [guía de inicio rápido de Azure Machine Learning en Python](quickstart-get-started.md) para crear un área de trabajo e iniciar Azure Notebooks.

## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Prueba de Azure Notebooks: cuadernos de Jupyter gratuitos en la nube

Es fácil empezar a usar Azure Notebooks. El [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) ya está instalado y configurado en [Azure Notebooks](https://notebooks.azure.com/). Tanto la instalación como las actualizaciones futuras se administran automáticamente mediante servicios de Azure.
  
+ Para ejecutar los **principales cuadernos del tutorial**:
  1. Vaya a [Azure Notebooks](https://notebooks.azure.com/).
    
  1. Busque la carpeta de **tutoriales** en la biblioteca **Getting Started** que creó en la guía de inicio rápido de los requisitos previos.
    
  1. Abra el cuaderno que desea ejecutar.
    
+ Para ejecutar **otros cuadernos**:

  1. [Importe los cuadernos de ejemplo](https://aka.ms/aml-clone-azure-notebooks) en Azure Notebooks.

  1. Utilice cualquiera de estos métodos para agregar un archivo de configuración del área de trabajo a la biblioteca:
     + Copie el archivo **config.json** de la biblioteca **Getting Started** a la nueva biblioteca clonada.

     + Cree un área de trabajo mediante código en [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
    
  1. Abra el cuaderno que desea ejecutar.     


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Uso de Data Science Virtual Machine (DSVM)

El [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) y el servidor de cuadernos ya están instalados y configurados en DSVM. Siga estos pasos para ejecutar los cuadernos.

1. [Cree una instancia de DSVM](how-to-configure-environment.md#dsvm).

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

1. Utilice cualquiera de estos métodos para agregar un archivo de configuración del área de trabajo a la biblioteca:
    * Copie el archivo **aml_config\config.json** que creó mediante la información de la guía de inicio rápido de los requisitos previos en el directorio clonado.

    * Cree un área de trabajo mediante código en [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Inicie el servidor del cuaderno desde el directorio clonado.

## <a name="use-your-own-jupyter-notebook-server"></a>Uso del propio servidor de Jupyter Notebook

Siga estos pasos para crear un servidor de Jupyter Notebook local en su equipo.

1. Asegúrese de que ha completado la guía de inicio rápido de los requisitos previos en la que instaló los SDK de Azure Machine Learning.

1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

1. Utilice cualquiera de estos métodos para agregar un archivo de configuración del área de trabajo a la biblioteca:
    * Copie el archivo **aml_config\config.json** que creó mediante la información de la guía de inicio rápido de los requisitos previos en el directorio clonado.
    
    * Cree un área de trabajo mediante código en [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Inicie el servidor del cuaderno desde el directorio clonado.

1. Vaya a la carpeta que contiene el cuaderno.

1. Abra el cuaderno.

<a name="auto"></a>

## <a name="automated-ml-setup"></a>Configuración automatizada de Machine Learning 

**Estos pasos se aplican solo a los cuadernos de la carpeta `automated-machine-learning`.**

Aunque puede usar cualquiera de las opciones anteriores, también puede instalar el entorno y crear un área de trabajo al mismo tiempo con las instrucciones siguientes. 

1. Instale [Mini conda](https://conda.io/miniconda.html). Elija la versión 3.7, o cualquier versión superior. Siga las indicaciones para realizar la instalación. 
   >[!NOTE]
   >Puede usar cualquier sistema conda existente, siempre que tenga la versión 4.4.10 o posterior. Use `conda -V` para mostrar la versión. La versión de conda se puede actualizar con el comando: `conda update conda`. No es preciso instalar mini conda específicamente.

1. Descargue los cuadernos de ejemplo de [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) en forma de archivo zip y extraiga el contenido en un directorio local. Los cuadernos de Machine Learning automático están en la carpeta `how-to-use-azureml/automated-machine-learning`.

1. Configure un nuevo entorno de Conda. 
   1. Abra un símbolo del sistema de Conda en el equipo local.
   
   1. Vaya a los archivos que extrajo en el equipo local.
   
   1. Abra la carpeta `automated-machine-learning`.
   
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

1. Abra la carpeta automated-machine-learning y, después, el cuaderno `configuration.ipynb`. 

1. Ejecute las celdas del cuaderno para registrar el proveedor de recursos de Machine Learning Services y crear un área de trabajo.

Ya está listo para abrir los blocs de notas que guardados en el equipo local y ejecutarlos.


## <a name="next-steps"></a>Pasos siguientes

Explore la [repositorio de cuadernos de GitHub para Azure Machine Learning Service](https://aka.ms/aml-notebooks)

Pruebe estos tutoriales:
+ [Entrenamiento de un modelo de clasificación de imágenes con el servicio Azure Machine Learning](tutorial-train-models-with-aml.md)

+ [Preparación de datos y uso de Machine Learning automatizado para entrenar un modelo de regresión con el conjunto de datos de los taxis de Nueva York](tutorial-data-prep.md)
