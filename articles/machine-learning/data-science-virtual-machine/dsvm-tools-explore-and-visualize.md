---
title: Herramientas de visualización y exploración de datos - Azure | Microsoft Docs
description: Herramientas de visualización y exploración de datos de Data Science Virtual Machine.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 693be80e493a0ba259d147f432dc9d6c07ba876d
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427520"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Herramientas de visualización y exploración de datos en Data Science Virtual Machine

Un paso clave en la ciencia de datos pasa por comprender los datos. Las herramientas de visualización y de exploración de datos permiten acelerar la comprensión de los datos. Estas son algunas herramientas proporcionadas en DSVM que facilitan este paso clave. 

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| ¿Qué es?   | Motor de consultas SQL de código abierto en Big Data    |
| Versiones de DSVM compatibles      | Windows, Linux  |
| ¿Cómo se configura/instala en DSVM?      |  Solo se instala en `/dsvm/tools/drill*` en modo incrustado   |
| Usos típicos      |  Exploración de datos in situ sin necesidad de ETL. Consulta de distintos orígenes y formatos de datos, como CSV, JSON, tablas relacionales o Hadoop     |
| ¿Cómo se usa o ejecuta?      | Acceso directo de escritorio  <br/> [Comience a usar Drill en 10 minutos](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| Herramientas relacionadas en DSVM      |   Rattle, Weka y SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |  Weka es una colección de algoritmos de aprendizaje automático para las tareas de minería de datos. Los algoritmos se pueden aplicar directamente a un conjunto de datos o se pueden llamar desde su propio código de Java. Weka contiene herramientas para el preprocesamiento, la clasificación, la regresión, la agrupación en clústeres, las reglas de asociación y la visualización de datos. |
| Ediciones compatibles de DSVM     | Windows, Linux     |
| Usos típicos      | Herramienta general de ML     |
| ¿Cómo se usa o ejecuta?      | En Windows, busque Weka en el menú Inicio. En Linux, inicie sesión con X2Go y, después, vaya a las aplicaciones -> desarrollo -> Weka. |
| Vínculos a ejemplos      | [Ejemplos de Weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Herramientas relacionadas en DSVM      |LightGBM, Rattle y Xgboost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| ¿Qué es?   |   Una interfaz gráfica de usuario para la minería de datos con R   |
| Ediciones compatibles de DSVM     | Windows, Linux     |
| Usos típicos      | Herramienta general de minería de datos de la IU para R    |
| ¿Cómo se usa o ejecuta?      | Herramienta de interfaz de usuario. En Windows, inicie un símbolo del sistema, ejecute R y, dentro de R, ejecute `rattle()`. En Linux, conéctese a X2Go, inicie un terminal, ejecute R y, dentro de R, ejecute `rattle()`. |
| Vínculos a ejemplos      | [Rattle](https://togaware.com/onepager/) |
| Herramientas relacionadas en DSVM      |LightGBM, Weka y Xgboost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| ¿Qué es?   | Visualización de datos interactivos y herramienta de BI    |
| Versiones de DSVM compatibles      | Windows  |
| Usos típicos      |  Visualización de datos y creación de paneles   |
| ¿Cómo se usa o ejecuta?      | Acceso directo de escritorio (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Herramientas relacionadas en DSVM      |   2019 de Visual Studio, Visual Studio Code y Juno      |

