---
title: 'Guía del desarrollador de R para Azure: programación en R | Microsoft Docs'
description: En este artículo se proporciona información general de las distintas formas en que los científicos de datos pueden aprovechar sus habilidades existentes con el lenguaje de programación R en Azure. Azure ofrece muchos servicios que los desarrolladores de R pueden aprovechar para ampliar sus cargas de trabajo de ciencia de datos en la nube.
services: machine-learning
documentationcenter: ''
author: AnalyticJeremy
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: R
ms.topic: article
ms.date: 09/12/2018
ms.author: jepeach
ms.openlocfilehash: bc00bd3b61398355c663d133c0c9a66c2a52aa8d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046945"
---
# <a name="r-developers-guide-to-azure"></a>Guía del desarrollador de R para Azure
<img src="media/r-developers-guide/logo_r.svg" alt="R logo" align="right" width="200" />

Muchos científicos de datos que trabajan cada vez con mayores volúmenes de datos buscan maneras de aprovechar la eficacia de la informática en la nube para sus análisis.  En este artículo se proporciona información general de las distintas formas en que los científicos de datos pueden aprovechar sus habilidades existentes con el [lenguaje de programación R](https://www.r-project.org) en Azure.

Microsoft ha adoptado por completo el lenguaje de programación R como herramienta de primera clase para científicos de datos.  La empresa, al proporcionar muchas opciones diferentes para que los desarrolladores de R ejecuten código en Azure, permite a los científicos de datos ampliar sus cargas de trabajo de ciencia de datos en la nube al abordar proyectos a gran escala.

Vamos a examinar las distintas opciones y los escenarios más atractivos para cada una de ellas.

## <a name="azure-services-with-r-language-support"></a>Servicios de Azure con compatibilidad con el lenguaje R
En este artículo se tratan los siguientes servicios de Azure que admiten el lenguaje R:

|Servicio                                                          |DESCRIPCIÓN                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
|[Data Science Virtual Machine](#data-science-virtual-machine)    |una VM personalizada para usar como estación de trabajo de ciencia de datos o como destino de proceso personalizado|
|[Machine Learning Services en HDInsight](#ml-services-on-hdinsight)            |sistema basado en clústeres para ejecutar análisis de R en grandes conjuntos de datos entre varios nodos   |
|[Azure Databricks](#azure-databricks)                            |entorno de colaboración de Spark que admite R y otros lenguajes               |
|[Azure Machine Learning Studio](#azure-machine-learning-studio)  |ejecución de scripts personalizados de R en experimentos de aprendizaje automático de Azure                      |
|[Azure Batch](#azure-batch)                                      |ofrece una variedad de opciones para ejecutar código de R de manera económica entre varios nodos en un clúster|
|[Azure Notebooks](#azure-notebooks)                              |versión basada en la nube sin costo (pero limitada) de Jupyter Notebook                  |
|[Azure SQL Database](#azure-sql-database)                        |ejecución de scripts de R dentro del motor de base de datos de SQL Server                            |

## <a name="data-science-virtual-machine"></a>Máquina virtual de ciencia de datos
[Data Science Virtual Machine (DSVM)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) es una imagen de VM personalizada en la nube de Azure de Microsoft, diseñada específicamente para realizar la ciencia de datos. Tiene muchas herramientas populares de ciencia de datos, entre otras:
* [Microsoft R Open](https://mran.microsoft.com/open/)
* [Servidor de Microsoft Machine Learning](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop)
* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server)

DSVM se puede aprovisionar con Windows o Linux como sistema operativo.  Puede utilizar DSVM de dos maneras diferentes: como estación de trabajo interactiva o como plataforma de proceso para un clúster personalizado.

### <a name="as-a-workstation"></a>Como estación de trabajo
Si quiere empezar a trabajar con R en la nube de forma rápida y sencilla, esta es la mejor opción.  El entorno le resultará familiar a cualquier persona que haya trabajado con R en una estación de trabajo local.  Sin embargo, en lugar de usar los recursos locales, el entorno de R se ejecuta en una VM en la nube.  Si los datos ya están almacenados en Azure, esto tiene la ventaja adicional de permitir que los scripts de R se ejecuten "más cerca de los datos". En lugar de transferir los datos a través de Internet, puede acceder a los datos a través de la red interna de Azure, que proporciona tiempos de acceso mucho más rápidos.

DSVM puede ser especialmente útil para equipos pequeños de desarrolladores de R.  En lugar de invertir en estaciones de trabajo eficaces para cada desarrollador y requerir que los miembros del equipo sincronicen las versiones de los distintos paquetes de software que van a utilizar, cada desarrollador puede poner en marcha una instancia de DSVM siempre que sea necesario.

### <a name="as-a-compute-platform"></a>Como plataforma de proceso
Además de usarse como estación de trabajo, DSVM también se utiliza como una plataforma de proceso con escalabilidad flexible para los proyectos de R.  Mediante el paquete de R <code>[AzureDSVM](https://github.com/Azure/AzureDSVM)</code>, puede controlar mediante programación la creación y eliminación de instancias de DSVM.  Puede formar las instancias en un clúster e implementar un análisis distribuido que se realice en la nube.  Todo este proceso puede controlarse mediante código de R que se ejecuta en su estación de trabajo local.

Para más información sobre DSVM, consulte [Introducción a Data Science Virtual Machine de Azure para Linux y Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview).

## <a name="ml-services-on-hdinsight"></a>Machine Learning Services en HDInsight
[Microsoft Machine Learning Services](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview) proporciona a los científicos de datos, a los estadísticos y a los programadores de R acceso a petición a métodos escalables y distribuidos para realizar análisis en HDInsight.  Esta solución proporciona las funcionalidades más recientes para realizar análisis basados en R de conjuntos de datos de prácticamente cualquier tamaño, cargados en el almacenamiento de Data Lake o Azure Blob Storage.

Se trata de una solución de categoría empresarial que le permite escalar el código de R en un clúster.  Al aprovechar las funciones en el paquete
<code>[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)</code> de Microsoft, los scripts de R en HDInsight pueden ejecutar funciones de procesamiento de datos en paralelo entre varios nodos en un clúster.  Esto permite que R estudie los datos en una escala mucho mayor de lo posible con un único subproceso de R que se ejecute en una estación de trabajo.

Esta capacidad de escalar hace que Machine Learning Services en HDInsight sea una buena opción para los desarrolladores de R con grandes conjuntos de datos.  Proporciona una plataforma flexible y escalable para ejecutar los scripts de R en la nube.

Para obtener un tutorial sobre cómo crear un clúster de Machine Learning Services, consulte el artículo [Introducción a ML Services en Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started).

## <a name="azure-databricks"></a>Azure Databricks
[Azure Databricks](https://azure.microsoft.com/services/databricks/) es una plataforma de análisis basada en Apache Spark optimizada para la plataforma de servicios en la nube de Microsoft Azure.  Diseñada por los fundadores de Apache Spark, Databricks está integrado con Azure para proporcionar una configuración con un solo clic, flujos de trabajo optimizados y un área de trabajo interactiva que permite la colaboración entre científicos de datos, ingenieros de datos y analistas empresariales.

El sistema de cuadernos de la plataforma habilita la colaboración en Databricks.  Los usuarios pueden crear, compartir y modificar cuadernos con otros usuarios de los sistemas.  Estos cuadernos permiten a los usuarios escribir código que se ejecuta en clústeres de Spark administrados en el entorno de Databricks.  Estos cuadernos son totalmente compatibles con R y dan acceso a los usuarios a Spark a través de paquetes `SparkR` y `sparklyr`.

Dado que Databricks se basa en Spark y se centra enormemente en la colaboración, quienes suelen usar la plataforma son equipos de científicos de datos que trabajan juntos en los análisis complejos de grandes conjuntos de datos.  Dado que los cuadernos en Databricks admiten otros lenguajes además de R, es especialmente útil para los equipos donde los analistas usan distintos lenguajes para su trabajo principal.

En el artículo [¿Qué es Azure Databricks?](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks)
encontrará más detalles sobre la plataforma y una ayuda introductoria.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/) es una herramienta colaborativa de arrastrar y colocar que le permite crear, probar e implementar soluciones de análisis predictivo en la nube.  Permite a los científicos de datos emergentes crear e implementar modelos de aprendizaje automático sin necesidad de escribir mucho código.

Machine Learning Studio es compatible tanto con R como con Python.  Puede usar R con Machine Learning Studio de dos maneras.

### <a name="custom-r-scripts-in-your-experiments"></a>Scripts de R personalizados en sus experimentos
En primer lugar, puede ampliar las funcionalidades de manipulación de datos y aprendizaje automático de Machine Learning Studio al escribir scripts de R personalizados.
Aunque Machine Learning Studio incluye una amplia variedad de módulos para preparar y analizar datos, no es suficiente para las funcionalidades de un lenguaje maduro como R. Por lo tanto, el servicio se diseñó para permitirle presentar sus propios scripts de R personalizados en los casos donde los módulos proporcionados no satisfagan sus necesidades.

Para aprovechar esta funcionalidad, arrastre un módulo "Execute R Script" (Ejecutar script R) y suéltelo en el experimento.  A continuación, use el editor de código en el panel "Propiedades" para escribir un nuevo script de R o pegue un script existente.  Dentro del script, puede hacer referencia a paquetes de R externos.  Puede usar el script para manipular los datos o para entrenar modelos de aprendizaje automático complejos que no formen parte de la biblioteca estándar de modelos de Machine Learning Studio.

Para obtener una introducción exhaustiva sobre el uso de R en experimentos de Machine Learning Studio, eche un vistazo a [Tutorial de inicio rápido en la programación en lenguaje R para Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart).

### <a name="create-manage-and-deploy-experiments-from-your-local-r-environment"></a>Creación, administración e implementación de experimentos desde el entorno local de R
La otra manera de usar R con Machine Learning Studio es utilizar el paquete
<code>[AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)</code> para supervisar y controlar el proceso de experimentación con el entorno de programación de R.  Este paquete, que mantiene Microsoft, le permite cargar y descargar conjuntos de datos hacia y desde Azure Machine Learning para interrogar experimentos, publicar las funciones de R como servicios web de Azure Machine Learning, ejecutar los datos de R a través de servicios web existentes y recuperar la salida.

Este paquete facilita mucho el uso de Azure Machine Learning como plataforma de implementación escalable para el código de R.  En lugar de hacer clic y arrastrar en la interfaz de usuario, puede automatizar todo el proceso de implementación con herramientas que ya conoce.

## <a name="azure-batch"></a>Azure Batch
Para los trabajos de R a gran escala, puede usar [Azure Batch](https://azure.microsoft.com/services/batch/).  Este servicio proporciona administración de procesos y programación de trabajos de escala de nube de manera que pueda escalar la carga de trabajo de R entre decenas, cientos o miles de máquinas virtuales.  Puesto que es una plataforma informática generalizada, hay algunas opciones para ejecutar trabajos de R en Azure Batch.

Una opción consiste en usar el paquete <code>[doAzureParallel](https://github.com/Azure/doAzureParallel)</code> de Microsoft.  Este paquete de R es un back-end paralelo para el paquete `foreach`.  Permite que cada iteración del bucle `foreach` se ejecute en paralelo en un nodo del clúster de Azure Batch.  Para obtener una introducción al paquete, lea la entrada de blog [doAzureParallel: Take advantage of Azure’s flexible compute directly from your R session](https://azure.microsoft.com/blog/doazureparallel/) (doAzureParallel: Sacar partido del proceso flexible de Azure directamente de la sesión de R).

Otra opción para ejecutar un script de R en Azure Batch es agrupar el código con "RScript.exe" como aplicación de Batch en Azure Portal.  Para obtener un tutorial detallado, consulte ["R Workloads on Azure Batch](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/) (Cargas de trabajo de R en Azure Batch).

Una tercera opción es usar [Azure Distributed Data Engineering Toolkit](https://github.com/Azure/aztk) (AZTK), que le permite aprovisionar clústeres de Spark a petición con contenedores de Docker en Azure Batch.  Esto proporciona una forma económica de ejecutar trabajos de Spark en Azure.  Al usar [SparklyR con AZTK](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK), los scripts de R se pueden escalar horizontalmente en la nube de manera fácil y económica.

## <a name="azure-notebooks"></a>Azure Notebooks
[Azure Notebooks](https://notebooks.azure.com) es un método de bajo costo y baja fricción para los desarrolladores de R que prefieren trabajar con cuadernos para llevar su código a Azure.  Es un servicio gratuito para que cualquier persona desarrolle y ejecute código en su explorador usando [Jupyter](https://jupyter.org/), que es un proyecto de código abierto que permite la combinación de prosa de marcado, código ejecutable y gráficos en un único lienzo.

Si bien Azure Notebooks es una opción viable para los proyectos a pequeña escala, tiene algunas limitaciones que lo hacen inapropiado para los proyectos de ciencia de datos a gran escala.  Actualmente, el servicio limita el proceso de cada cuaderno a 4 GB de memoria, y los conjuntos de datos solo pueden ser de 1 GB.  Sin embargo, para la publicación de análisis más pequeños, esta es una opción sencilla y sin costo.

## <a name="azure-sql-database"></a>Azure SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) es un servicio de base de datos relacional en la nube, totalmente administrado e inteligente de Microsoft.  Permite usar toda la eficacia de SQL Server sin los problemas de configuración de la infraestructura.  Esto incluye [Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017), que es una de las incorporaciones más recientes de SQL Server.

Esta característica ofrece un motor de ciencia de datos y análisis predictivo insertado que puede ejecutar código R en una base de datos de SQL Server como procedimientos almacenados, como scripts de T-SQL que contienen instrucciones de R o como código de R que contiene T-SQL.  En lugar de extraer datos de la base de datos y cargarlos en el entorno de R, carga el código de R directamente en la base de datos y deja que se ejecute junto con los datos.

Si bien Machine Learning Services ha formado parte de un entorno local de SQL Server desde 2016, es relativamente nuevo en Azure SQL Database.  Actualmente, se encuentra en [versión preliminar limitada](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap), pero continuará evolucionando.


### <a name="next-steps"></a>Pasos siguientes
* [Running your R code on Azure with mrsdeploy](http://blog.revolutionanalytics.com/2017/03/running-your-r-code-azure.html) (Ejecución de código de R en Azure con mrsdeploy)
* [Machine Learning Server in the Cloud](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-in-the-cloud) (Machine Learning Server en la nube)
* [Additional Resources for Machine Learning Server and Microsoft R](https://docs.microsoft.com/machine-learning-server/resources-more) (Recursos adicionales para Machine Learning Server y Microsoft R)
* [R on Azure](https://github.com/yueguoguo/r-on-azure) (R en Azure): información general de paquetes, herramientas y casos prácticos para el uso de R con Azure

---

<sub>El logotipo de R es &copy; 2016 The R Foundation y se utiliza según las condiciones de la [licencia Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/).</sub>
