---
title: Ejemplos y tutoriales para Data Science Virtual Machine en Azure | Microsoft Docs
description: Obtenga información sobre los ejemplos y tutoriales que explican cómo realizar tareas comunes y muestran escenarios con Data Science Virtual Machine.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: vijetaj
ms.openlocfilehash: 57ebefb11d23c5ea18d5859ca494944e6a845e04
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565313"
---
# <a name="samples-on-data-science-virtual-machines"></a>Ejemplos de Data Science Virtual Machine

Azure Data Science Virtual Machine incluye un conjunto completo de código de ejemplo. El código de ejemplo está en forma de cuadernos de Jupyter Notebook y scripts en lenguajes como Python y R. 
> [!NOTE]
> Para más información acerca de cómo ejecutar los cuadernos de Jupyter Notebook en Data Science Virtual Machine, consulte la sección [Acceso a Jupyter](#access-jupyter).

## <a name="quick-reference-of-samples"></a>Referencia rápida de los ejemplos
| Categoría de ejemplos | DESCRIPCIÓN | Ubicaciones |
| ------------- | ------------- | ------------- |
| Lenguaje R  | Ejemplos de R que explican escenarios como la forma de conectar con almacenes de datos en la nube de Azure. También explican cómo comparar el código abierto de R y de Microsoft R, así como el modo de usar modelos en Microsoft R Server o SQL Server. <br/> [Lenguaje R](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| Lenguaje Python  | Ejemplos de Python que explican escenarios como la conexión con los almacenes de datos en la nube de Azure y el uso con Azure Machine Learning.  <br/> [Lenguaje Python](#python-language) | <br/>`~notebooks` <br/><br/>|
| Lenguaje Julia  | Ejemplo de Julia que detalla el trazado y aprendizaje profundo de Julia. También explica las llamadas a C y Python desde Julia. <br/> [Lenguaje Julia](#julia-language) |<br/> Windows:<br/> `~notebooks/Julia_notebooks`<br/><br/> Linux:<br/> `~notebooks/julia`<br/><br/> |
| Azure Machine Learning  | Cree modelos de aprendizaje profundo con Machine Learning. Implemente los modelos en cualquier lenguaje. Utilice el ajuste de hiperparámetros inteligente y el aprendizaje automático automatizado. También puede usar la administración de modelos y el aprendizaje distribuido. <br/> [Machine Learning](#azureml) | <br/>`~notebooks/AzureML`<br/> <br/>|
| Cuadernos de PyTorch  | Ejemplos de aprendizaje profundo que usan redes neuronales basadas en PyTorch. Los cuadernos abarcan desde escenarios para principiantes a usuarios avanzados.  <br/> [Cuadernos de PyTorch](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| TensorFlow  |  Varios ejemplos diferentes de red neuronal y técnicas implementadas mediante la plataforma TensorFlow. <br/> [TensorFlow](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| Microsoft Cognitive Toolkit <br/>   | Los ejemplos de aprendizaje profundo publicados por el equipo de Cognitive Toolkit en Microsoft.  <br/> [Cognitive Toolkit](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> Linux:<br/> `~notebooks/CNTK`<br/> <br/>|
| caffe2 | Ejemplos de aprendizaje profundo que usan redes neuronales basadas en caffe2. Varios cuadernos que permiten a los usuarios familiarizarse con caffe2 y cómo usarlo de forma eficaz. Algunos ejemplos son la creación de imágenes de preprocesamiento y conjuntos de datos. También incluyen la regresión y cómo usar modelos previamente entrenados. <br/> [caffe2](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| H2O   | Ejemplos basados en Python que usan H2O para numerosos problemas de escenarios reales. <br/> [H2O](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| Lenguaje SparkML  | Ejemplos que usan el kit de herramientas MLLib de Spark mediante pySpark y MMLSpark - Microsoft Machine Learning para Apache Spark en Apache Spark 2.x.  <br/> [Lenguaje SparkML](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| XGBoost | Ejemplos de aprendizaje automático estándar en XGBoost para escenarios de clasificación, regresión etc. <br/> [XGBoost](#xgboost) | <br/>Windows:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Acceso a Jupyter 

Para acceder a Jupyter, seleccione el icono `Jupyter` en el menú de escritorio o de una aplicación. También se puede acceder a Jupyter en las ediciones para Linux de Data Science Virtual Machine. Puede acceder remotamente desde un explorador web; para ello, visite `https://<Full Domain Name or IP Address of the DSVM>:8000` en Ubuntu.

Para agregar excepciones y hacer que el acceso a Jupyter esté disponible desde un explorador, vea la siguiente captura de pantalla.


![Habilitación de una excepción en Jupyter](./media/ubuntu-jupyter-exception.png)


Inicie sesión con la misma contraseña que ha usado para el inicio de sesión de Data Science Virtual Machine.
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

