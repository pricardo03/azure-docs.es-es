---
title: Ejemplos y tutoriales para Data Science Virtual Machine en Azure | Microsoft Docs
description: Ejemplos y tutoriales para Data Science Virtual Machine.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: gokuma
ms.openlocfilehash: a1f15b805d2f27152d9ba85608ce0dc1d1aac21e
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392575"
---
# <a name="samples-on-the-data-science-virtual-machines-dsvm"></a>Ejemplos de Data Science Virtual Machines (DSVM)

Las instancias de DSVM incluyen un conjunto completo de código de ejemplo, tanto en forma de Jupyter Notebook como de scripts en lenguajes como Python y R.    
> [!NOTE]
> Consulte la sección [Acceder a Jupyter](#access-jupyter) para obtener más información sobre cómo ejecutar Jupyter Notebook en DSVM.

## <a name="quick-reference-of-samples"></a>Referencia rápida de muestras
| Categoría de ejemplos | DESCRIPCIÓN | Ubicaciones |
| ------------- | ------------- | ------------- |
| Lenguaje **R**  | Ejemplos en **R** que explican escenarios como la conexión con los almacenes de datos en la nube de Azure, comparación de R de código abierto y Microsoft R y puesta en marcha de modelos en Microsoft R Server o SQL Server. <br/> [Captura de pantalla](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| Lenguaje **Python**  | Ejemplos de **Python** que explican escenarios como la conexión con los almacenes de datos en nube de Azure y el uso con **Azure Machine Learning**.  <br/> [Captura de pantalla](#python-language) | <br/>`~notebooks` <br/><br/>|
| Lenguaje **Julia**  | Ejemplo en **Julia** que ofrece detalles sobre el trazado en Julia, el aprendizaje profundo en Julia, llamar a C y Python desde Julia etc. <br/> [Captura de pantalla](#julia-language) |<br/> **Windows**:<br/> `~notebooks/Julia_notebooks`<br/><br/> **Linux**:<br/> `~notebooks/julia`<br/><br/> |
| **Azure Machine Learning** AzureML  | Compile modelos de ML y de aprendizaje profundo con el servicio **Azure Machine Learning** e implemente sus modelos en cualquier lugar. Saque el máximo partido a funcionalidades como la ML automatizada, el ajuste inteligente de hiperparámetros, la administración de modelos o el aprendizaje distribuido. <br/> [Captura de pantalla](#azureml) | <br/>`~notebooks/AzureML`<br/> <br/>|
| Cuadernos **PyTorch**  | Ejemplos de aprendizaje profundo mediante las redes neuronales basadas en **Pytorch**. Hay una variedad de blocs de notas que van desde escenarios principiantes hasta avanzados.  <br/> [Captura de pantalla](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| **TensorFlow**  | Varios ejemplos diferentes de red neuronal y técnicas implementadas mediante la plataforma **TensorFlow**. <br/> [Captura de pantalla](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| **CNTK** <br/> (Microsoft Cognitive Toolkit)  | Los ejemplos de aprendizaje profundo publicados por el equipo de Cognitive Toolkit en Microsoft.  <br/> [Captura de pantalla](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> **Linux**:<br/> `~notebooks/CNTK`<br/> <br/>|
| **caffe2** | Ejemplos de aprendizaje profundo usando las redes neuronales basadas en **caffe2**. Hay varios blocs de notas diseñados para que los usuarios se familiaricen con caffe2 y cómo utilizarlo de forma eficiente, incluidos ejemplos como la creación de conjuntos de datos de procesamiento previo de imagen, creación de conjuntos de datos, regresión y el uso de modelos previamente entrenados. <br/> [Captura de pantalla](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| **H2O**   | Ejemplos basados en Python que usan **H2O** para numerosos problemas de escenarios reales. <br/> [Captura de pantalla](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| Lenguaje **SparkML**  | Ejemplo de uso de características y funcionalidades del kit de herramienta **MLlib** de Spark mediante **pySpark** y **MMLSpark - Microsoft Machine Learning para Apache Spark** en **Apache Spark 2.x**.  <br/> [Captura de pantalla](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| **XGBoost** | Ejemplos de Machine Learning estándar en **XGBoost** para escenarios como clasificación, regresión etc. <br/> [Captura de pantalla](#xgboost) | <br/>**Windows**:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Acceso a Jupyter 

Puede acceder a Jupyter haciendo clic en el icono `Jupyter` en el menú de escritorio o de una aplicación. También puede obtener acceso a Jupyter en las ediciones de Linux de DSVM de forma remota desde un explorador web; para ello, visite **`https://<Full Domain Name or IP Address of the DSVM>:8000`** en Ubuntu.

Consulte la captura de pantalla para agregar la excepción y habilitar el acceso de Jupyter en el explorador.


![Habilitar la excepción de Jupyter](./media/ubuntu-jupyter-exception.png)


Inicie sesión con la misma contraseña que ha usado para el inicio de sesión de DSVM.
<br/>

**Página de inicio de Jupyter**
<br/>![Página de inicio de Jupyter](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>Lenguaje R 
<br/>![Ejemplos de R](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Lenguaje Python
<br/>![Ejemplos de Python](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Lenguaje Julia 
<br/>![Ejemplos de Julia](./media/julia-samples.png)<br/>

## <a name="azureml"></a>AzureML 
<br/>![Ejemplos de AzurekML](./media/azureml-samples.png)<br/>

## <a name="pytorch"></a>PyTorch
<br/>![Ejemplos de PyTorch](./media/pytorch-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![Ejemplos de TensorFlow](./media/tensorflow-samples.png)<br/>


## <a name="cntk"></a>CNTK 
<br/>![Ejemplos de CNTK](./media/cntk-samples.png)<br/>


## <a name="caffe2"></a>caffe2 
<br/>![Ejemplos de caffe2](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![Ejemplos de H2O](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![Ejemplos de SparkML](./media/sparkml-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![Ejemplos de XGBoost](./media/xgboost-samples.png)<br/>

