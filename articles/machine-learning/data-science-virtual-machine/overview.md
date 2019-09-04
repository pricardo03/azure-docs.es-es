---
title: Introducción a Azure Data Science Virtual Machine para Linux y Windows | Microsoft Docs
description: Componentes y escenarios de análisis fundamentales para instancias de Data Science Virtual Machine en Windows y Linux.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/22/2019
ms.author: vijetaj
ms.openlocfilehash: 22192d67a946f9f466848b65b9cd763b16e6c5f0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70099380"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>¿Qué es Azure Data Science Virtual Machine de Azure para Linux y Windows?

Data Science Virtual Machine (DSVM) es una imagen de máquina virtual personalizada en la plataforma en la nube de Azure diseñada específicamente para realizar ciencia de datos. Tiene muchas ciencias de datos conocidas y otras herramientas preinstaladas y preconfiguradas para impulsar la creación de aplicaciones inteligentes para análisis avanzado. 

Científicos de datos y desarrolladores de Microsoft, junto con la comunidad de ciencia de datos más amplia, han probado rigurosamente las configuraciones de las herramientas. Estas pruebas ayudan a garantizar la estabilidad y la viabilidad general.

El DSVM está disponible en:
+ Windows Server 2016, Windows Server 2012
+ Ubuntu 16.04 LTS y CentOS 7.4

> [!NOTE]
> Todas las herramientas de máquina virtual para el aprendizaje profundo se han incluido en Data Science Virtual Machine. 


## <a name="what-can-i-do-with-the-dsvm"></a>¿Qué puedo hacer con DSVM?
El objetivo de Data Science Virtual Machine es proporcionar a los profesionales de datos de todos los niveles de aptitud y de distintos sectores, un entorno de ciencia de datos preconfigurado, totalmente integrado y libre de problemas. En lugar de implementar un área de trabajo comparable por su cuenta, puede aprovisionar un DSVM. Esta opción puede ahorrarle días, o incluso _semanas_, en los procesos de instalación, configuración y administración de paquetes. Después de que se ha asignado la DSVM, puede comenzar a trabajar inmediatamente en su proyecto de ciencia de datos.

Data Science Virtual Machine está diseñada y configurada para trabajar con un amplio rango de escenarios de uso. Puede escalar o reducir verticalmente el entorno a medida que cambien los requisitos del proyecto. También puede usar el lenguaje que prefiera para programar tareas de ciencia de datos e instalar otras herramientas para personalizar el sistema para ajustarlo a sus necesidades.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Escritorio de análisis preconfigurados en la nube
Data Science Virtual Machine proporciona una configuración de base de referencia para los equipos de ciencia de datos que desean sustituir sus escritorios locales por un escritorio en la nube administrado. Esta línea de base garantiza que todos los científicos de datos en un equipo tienen una configuración coherente con la que comprobar experimentos y promover la colaboración. Al reducirse la carga del administrador del sistema, también se reducen los costos. Esta reducción de la carga ahorra el tiempo necesario para evaluar, instalar y mantener los paquetes de software para realizar análisis avanzados.

### <a name="data-science-training-and-education"></a>Educación y formación de ciencia de datos
Los instructores y educadores que imparten clases de ciencia de datos proporcionan normalmente una imagen de máquina virtual. La imagen garantiza que los alumnos tienen una configuración coherente y que las muestras funcionan según lo previsto. 

Data Science Virtual Machine crea un entorno a petición con una configuración coherente que simplifica los desafíos de incompatibilidad y soporte técnico. En los casos en donde estos entornos tienen que crearse con frecuencia, especialmente para las clases de aprendizaje más cortas, estos entornos se benefician sustancialmente.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Capacidad elástica a petición para proyectos de gran escala
Tanto los hackathons/competiciones como la exploración y el modelado de datos a gran escala requieren capacidad de hardware para la escalabilidad horizontal, normalmente durante poco tiempo. Data Science Virtual Machine puede ayudar a replicar el entorno de la ciencia de datos rápidamente a petición, en servidores con escalabilidad horizontal que permiten experimentos que los recursos informáticos de alta potencia pueden ejecutar.

### <a name="custom-compute-power-for-azure-notebooks"></a>Capacidad de proceso personalizada para Azure Notebooks
[Azure Notebooks](../../notebooks/azure-notebooks-overview.md) es un servicio hospedado gratuito para desarrollar, ejecutar y compartir cuadernos de Jupyter Notebook en la nube sin necesidad de instalación. El nivel de servicio gratuito está limitado a 4 GB de memoria y 1 GB de datos. 

Para liberar todos los límites, puede asociar un proyecto de Notebooks a una instancia de Data Science Virtual Machine o a cualquier otra máquina virtual que se ejecute en un servidor de Jupyter. Si inicia sesión en Azure Notebooks con una cuenta mediante Azure Active Directory (por ejemplo, una cuenta corporativa), Notebooks muestra automáticamente las instancias de Data Science Virtual Machine en las suscripciones asociadas a esa cuenta. Puede [asociar una instancia de Data Science Virtual Machine a Azure Notebooks](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier) para ampliar la potencia de proceso disponible.

### <a name="short-term-experimentation-and-evaluation"></a>Evaluación y experimentación a corto plazo
Puede usar DSVM para evaluar o aprender herramientas como estas con un esfuerzo mínimo de configuración:

- Servidor de Microsoft Machine Learning
- SQL Server
- Visual Studio Tools
- Jupyter
- Kits de herramientas de aprendizaje profundo y de aprendizaje automático
- Nuevas herramientas populares en la comunidad 

Dado que DSVM se puede configurar rápidamente, puede aplicarlo en otros escenarios de uso a corto plazo. Estos escenarios incluyen la replicación de experimentos publicados, la ejecución de demostraciones y el seguimiento de tutoriales en sesiones en línea y tutoriales de conferencia.

### <a name="deep-learning"></a>Aprendizaje profundo
En Data Science Virtual Machine, los modelos de entrenamiento pueden usar algoritmos de aprendizaje profundo en hardware basado en unidades de procesamiento de gráficos (GPU). Aprovechando las funcionalidades de escalado de máquinas virtuales de la plataforma Azure, DSVM le ayuda a usar hardware basado en GPU en la nube según sus necesidades. Puede cambiar a una máquina virtual basada en GPU cuando vaya a entrenar modelos grandes o cuando necesite cálculos a alta velocidad y desee mantener el mismo disco del SO.  

La edición de Windows Server 2016 de la DSVM viene preinstalada con controladores de GPU, marcos y las versiones de GPU de los marcos de aprendizaje profundo. En la edición Linux, el aprendizaje profundo en las GPU está habilitado en las DSVM de CentOS y Ubuntu. 

También puede implementar la edición de Ubuntu, CentOS o Windows 2016 de Data Science VM en una máquina virtual de Azure no basada en GPU. En este caso, todos los marcos de aprendizaje profundo se revertirán al modo CPU.
 
[Más información sobre los marcos de aprendizaje profundo y de inteligencia artificial](dsvm-deep-learning-ai-frameworks.md).

<a name="included"></a>

## <a name="whats-included-on-the-dsvm"></a>¿Qué incluye DSVM?
La máquina virtual de ciencia de datos tiene muchas herramientas de ciencia de datos y aprendizaje profundo conocidas ya instaladas y configuradas. También incluye herramientas que facilitan trabajar con diversos productos y análisis de datos de Azure. Estos productos incluyen Microsoft Machine Learning Server (R y Python) para la creación de modelos predictivos y SQL Server 2017 para la exploración a gran escala de los conjuntos de datos. Data Science Virtual Machine incluye otras herramientas tanto de la comunidad de código abierto como de Microsoft, junto con [código de ejemplo y cuadernos](dsvm-samples-and-walkthroughs.md). 

Esta es una lista de las herramientas y plataformas:
+ [Lenguajes de programación admitidos](dsvm-languages.md)

+ [Plataformas de datos compatibles](dsvm-data-platforms.md)

+ [Herramientas de desarrollo e IDE](dsvm-tools-development.md)

+ [Marcos de aprendizaje profundo e inteligencia artificial](dsvm-deep-learning-ai-frameworks.md)

+ [Herramientas de Machine Learning y ciencia de datos](dsvm-ml-data-science-tools.md)

+ [Herramientas de ingesta de datos](dsvm-tools-ingestion.md)

+ [Herramientas de visualización y exploración de datos](dsvm-tools-explore-and-visualize.md)

En la tabla siguiente se detalla y compara los componentes principales incluidos en las ediciones de Windows y Linux de la máquina virtual de ciencia de datos.

| **Herramienta**                                                           | **Edición de Windows** | **Edición de Linux** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) con paquetes populares preinstalados   |Y                      | Y             |
| [Microsoft Machine Learning Server (R y Python)](https://docs.microsoft.com/machine-learning-server/) Developer Edition incluye: <br />  Marco de alto rendimiento distribuido y paralelo de &nbsp;&nbsp;&nbsp;&nbsp; [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) (R y Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp;[MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package), nuevos algoritmos de aprendizaje automático de última generación <br />  &nbsp;&nbsp;&nbsp;&nbsp;[Operacionalización de R y Python](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |S                      | S |
| [Microsoft Office](https://products.office.com/business/office-365-proplus-business-software) ProPlus con activación compartida: Excel, Word y PowerPoint   |S                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7 y 3.5 con los paquetes más usados preinstalados    |S                      |S              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) con paquetes populares para lenguaje Julia preinstalados                         |S                      |S              |
| Bases de datos relacionales                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Herramientas de base de datos                                                       |  SQL Server Management Studio <br/> SQL Server Integration Services<br/> [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br />  Controladores ODBC/JDBC|  [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (herramienta de consulta), <br />  bcp, sqlcmd <br />  Controladores ODBC/JDBC|
| Análisis de base de datos escalable con los servicios de aprendizaje automático de SQL Server (R y Python) | S     |N              |
| [Servidor de Jupyter Notebook](https://jupyter.org/) con los siguientes kernels:                                  | S     | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; R | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; Python | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; Julia | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; PySpark | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | S (solo Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp; SparkR     | N | S |
| JupyterHub (servidor de cuadernos multiusuario)| N | S |
| JupyterLab (servidor de cuadernos multiusuario) | N | S (solo Ubuntu) |
| Herramientas de desarrollo, entornos de desarrollo integrados y editores de código:| | |
| &nbsp;&nbsp;&nbsp;&nbsp;[Visual Studio 2019 (Community Edition)](https://www.visualstudio.com/community/) con el complemento de Git, Azure HDInsight (Hadoop), Azure Data Lake, SQL Server Data Tools, [Node.js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs) y [ Herramientas de R para Visual Studio (RTVS)](https://microsoft.github.io/RTVS-docs/) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Visual Studio Code](https://code.visualstudio.com/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Atom](https://atom.io/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Juno (entorno de desarrollo integrado de Julia)](https://junolab.org/)| S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; Vim y Emacs | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; Git y Git Bash | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; OpenJDK | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; .NET Framework | S | N |
| Power BI Desktop | S | N |
| SDK para obtener acceso al conjunto de aplicaciones Cortana Intelligence y Azure | S | S |
| Herramientas de administración y movimiento de datos: | | |
| &nbsp;&nbsp;&nbsp;&nbsp; Explorador de Azure Storage | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [CLI de Azure](https://docs.microsoft.com/cli/azure) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; Azure PowerShell | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp;[Controlador FUSE de blobs](https://github.com/Azure/azure-storage-fuse) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Adlcopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Herramienta de migración de datos de Azure Cosmos DB](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Microsoft Data Management Gateway](https://msdn.microsoft.com/library/dn879362.aspx): movimiento de datos entre un entorno local y la nube | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Herramientas de línea de comandos de Unix/Linux | S | S |
| [Apache Drill](https://drill.apache.org) para la exploración de datos | S | S |
| Herramientas de aprendizaje automático: |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Integración con [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R y Python) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [XGBoost](https://github.com/dmlc/xgboost) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Weka](https://www.cs.waikato.ac.nz/ml/weka/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Rattle](https://togaware.com/rattle/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [LightGBM](https://github.com/Microsoft/LightGBM) | N | S (solo Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [CatBoost](https://tech.yandex.com/catboost/) | N | S (solo Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [H2O](https://www.h2o.ai/h2o/), [Sparkling Water](https://www.h2o.ai/sparkling-water/) | N | S (solo Ubuntu) |
| Herramientas de aprendizaje profundo que funcionan en una GPU o CPU: |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) (Windows 2016) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorFlow](https://www.tensorflow.org/) | S (Windows 2016) | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Horovod](https://github.com/uber/horovod) | N | S (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [MXNet](https://mxnet.io/) | S (Windows 2016) | S|
| &nbsp;&nbsp;&nbsp;&nbsp; [Caffe y Caffe2](https://github.com/caffe2/caffe2) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Chainer](https://chainer.org/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Torch](http://torch.ch/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Theano](https://github.com/Theano/Theano) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Keras](https://keras.io/)| N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [PyTorch](https://pytorch.org/)| N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Servidor de modelos MXNet](https://github.com/awslabs/mxnet-model-server) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorFlow Serving](https://www.tensorflow.org/serving/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorRT](https://developer.nvidia.com/tensorrt) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [CUDA, cuDNN y controlador NVIDIA](https://developer.nvidia.com/cuda-toolkit) | S | S |

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información en estos artículos:

+ Windows:
  + [Configuración de una instancia de DSVM de Windows](provision-vm.md)
  + [Diez cosas que puede hacer en Windows Data Science Virtual Machine](vm-do-ten-things.md)

+ Linux:
  + [Configuración de una instancia de DSVM de Linux (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Configuración de una instancia de DSVM de Linux (CentOS)](linux-dsvm-intro.md)
  + [Ciencia de datos en una DSVM de Linux](linux-dsvm-walkthrough.md)
