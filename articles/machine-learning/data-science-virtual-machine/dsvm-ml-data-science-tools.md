---
title: Herramientas de aprendizaje automático y ciencia de datos - Azure | Microsoft Docs
description: Obtenga información sobre los marcos y las herramientas de aprendizaje automático instalados previamente en Data Science Virtual Machine.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: e8876306e4ffbd0fa9a8aafc6d5d757fd3c9c614
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60502114"
---
# <a name="machine-learning-and-data-science-tools"></a>Herramientas de aprendizaje automático y ciencia de datos
Azure Data Science Virtual Machine tiene un amplio conjunto de herramientas y bibliotecas para el aprendizaje automático (ML), disponibles en lenguajes conocidos como Python, R o Julia. 

Estas son algunas de las herramientas de ML y las bibliotecas en Data Science Virtual Machine. 

## <a name="azure-machine-learninghttpsdocsmicrosoftcomazuremachine-learningserviceoverview-what-is-azure-ml-sdk"></a>SDK [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml)
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |   Azure Machine Learning es un servicio en la nube que puede usar para desarrollar e implementar modelos de aprendizaje automático. Puede realizar un seguimiento de los modelos mientras los compila, entrena, escala y administra mediante el SDK de Python. Implemente modelos como contenedores y ejecútelos en la nube, de forma local o en Azure IoT Edge.   |
| Ediciones compatibles     | Windows (entorno de conda: AzureML), Linux (entorno de conda: py36)    |
| Usos típicos      | Plataforma general de ML      |
| ¿Cómo se configura o instala?      |  Se instala con la compatibilidad de GPU   |
| ¿Cómo se usa o ejecuta?      | SDK de Python y la CLI de Azure. Active el entorno de Conda `AzureML` en la edición de Windows *o* a `py36`, en la edición de Linux.      |
| Vínculos a ejemplos      | Se incluyen ejemplos de cuadernos de Jupyter Notebook en el directorio `AzureML` bajo los cuadernos.  |
| Herramientas relacionadas      | Visual Studio Code, Jupyter   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |    XGBoost es una biblioteca de potenciación de gradientes rápida, portátil y distribuida (GBDT, GBRT o GBM) para Python, R, Java, Scala, C++, etc. Se ejecuta en una única máquina, en Hadoop y en Spark.    |
| Ediciones compatibles     | Windows, Linux     |
| Usos típicos      | Biblioteca general de ML      |
| ¿Cómo se configura o instala?      |  Se instala con la compatibilidad de GPU   |
| ¿Cómo se usa o ejecuta?      | Como biblioteca de Python (2.7 y 3.5), paquete de R y en la herramienta de línea de comandos de ruta de acceso (`C:\dsvm\tools\xgboost\bin\xgboost.exe` para Windows y `/dsvm/tools/xgboost/xgboost` para Linux)    |
| Vínculos a ejemplos      | Se incluyen ejemplos en la máquina virtual, en `/dsvm/tools/xgboost/demo` en Linux y `C:\dsvm\tools\xgboost\demo` en Windows.   |
| Herramientas relacionadas      | LightGBM y MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |   Vowpal Wabbit (también llamado "VW") es una biblioteca del sistema de aprendizaje de código abierto, rápida y situada fuera del núcleo.    |
| Ediciones compatibles     | Windows, Linux     |
| Usos típicos      | Biblioteca general de ML      |
| ¿Cómo se configura o instala?      |  Windows: instalador msi; Linux: apt-get |
| ¿Cómo se usa o ejecuta?      | Como una herramienta de línea de comandos de ruta de acceso (`C:\Program Files\VowpalWabbit\vw.exe` en Windows y `/usr/bin/vw` en Linux)    |
| Vínculos a ejemplos      | [Ejemplos de VowPal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| Herramientas relacionadas      |LightGBM, MXNet y XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |  Weka es una colección de algoritmos de ML para tareas de minería de datos. Los algoritmos se pueden aplicar directamente a un conjunto de datos o se pueden llamar desde su propio código de Java. Weka contiene herramientas para el preprocesamiento, la clasificación, la regresión, la agrupación en clústeres, las reglas de asociación y la visualización de datos. |
| Ediciones compatibles     | Windows, Linux     |
| Usos típicos      | Herramienta general de ML     |
| ¿Cómo se usa o ejecuta?      | En Windows, busque Weka en el menú Inicio. En Linux, inicie sesión con X2Go y, a continuación, vaya a **Applications** (Aplicaciones) > **Development** (Desarrollo) > **Weka**. |
| Vínculos a ejemplos      | [Ejemplos de Weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Herramientas relacionadas      |LightGBM, Rattle y XGBoost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |   Rattle es una interfaz gráfica de usuario para la minería de datos con R.   |
| Ediciones compatibles     | Windows, Linux     |
| Usos típicos      | Herramienta general de minería de datos de la IU para R    |
| ¿Cómo se usa o ejecuta?      | Herramienta de interfaz de usuario. En Windows, inicie un símbolo del sistema, ejecute R y, dentro de R, ejecute `rattle()`. En Linux, conéctese a X2Go, inicie un terminal, ejecute R y, dentro de R, ejecute `rattle()`. |
| Vínculos a ejemplos      | [Rattle](https://togaware.com/onepager/) |
| Herramientas relacionadas      |LightGBM, Weka y XGBoost   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| ¿Qué es?   | LightGBM es una plataforma de potenciación de gradientes rápida, distribuida y de alto rendimiento (GBDT, GBRT, GBM o MART), basada en algoritmos de árbol de decisión. Se utiliza para la categorización, clasificación y muchas otras tareas de ML.    |
| Versiones compatibles      | Windows, Linux    |
| Usos típicos      | Marco de gradient boosting de uso general      |
| ¿Cómo se configura o instala?      | En Windows, LightGBM se instala en forma de paquete de Python. En Linux, el archivo ejecutable de la línea de comandos se encuentra en `/opt/LightGBM/lightgbm`; luego se instala el paquete de R y los paquetes de Python.     |
| Vínculos a ejemplos      | [Guía de LightGBM](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| Herramientas relacionadas      | MXNet y XgBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| ¿Qué es?   | H2O es un plataforma de IA de código abierto que admite ML en memoria, distribuido, rápido y escalable.  |
| Versiones compatibles      | Linux   |
| Usos típicos      | ML escalable, distribuido y de uso general   |
| ¿Cómo se configura o instala?      | H2O se instala en `/dsvm/tools/h2o`.      |
| ¿Cómo se usa o ejecuta?      | Conéctese a la máquina virtual con X2Go. Inicie un nuevo terminal y ejecute `java -jar /dsvm/tools/h2o/current/h2o.jar`. Luego, inicie un explorador web y conéctese a `http://localhost:54321`.      |
| Vínculos a ejemplos      | Hay ejemplos disponibles en la máquina virtual de Jupyter en el directorio `h2o`.      |
| Herramientas relacionadas      | Apache Spark, MXNet, XGBoost, Sparkling Water y Deep Water    |

Hay muchas más bibliotecas de ML en Data Science Virtual Machine, como el famoso paquete `scikit-learn`, incluido en Anaconda Python Distribution que se instala en Data Science Virtual Machine. Para extraer del repositorio la lista de paquetes disponibles en Python, R y Julia, ejecute los administradores de paquetes correspondientes.
