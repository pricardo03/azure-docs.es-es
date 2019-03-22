---
title: Herramientas de desarrollo de Data Science Virtual Machine - Azure | Microsoft Docs
description: Obtenga información sobre las herramientas y los entornos de desarrollo integrado que están instalados previamente en Data Science Virtual Machine.
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
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 7983169c2b1123c57a48471e3f4d9ad6f19c84dc
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58314729"
---
# <a name="development-tools-on-the-data-science-virtual-machine"></a>Herramientas de desarrollo en Data Science Virtual Machine

Data Science Virtual Machine (DSVM) proporciona un entorno productivo para el desarrollo mediante la agrupación de varias herramientas e IDE conocidos. A continuación presentamos algunas herramientas que se proporcionan en DSVM. 

## <a name="visual-studio-2017"></a>Visual Studio 2017  

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de uso general      |
| Versiones de DSVM compatibles      |  Windows      |
| Usos típicos      | Desarrollo de software    |
| ¿Cómo se configura/instala en DSVM?      | Carga de trabajo de ciencia de datos (herramientas de Python y R), carga de trabajo de Azure (Hadoop, Data Lake), Node.js, herramientas de SQL Server, [Azure Machine Learning para Visual Studio Code](https://github.com/Microsoft/vs-tools-for-ai)    |
| ¿Cómo se usa o ejecuta?      | Acceso directo de escritorio (`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\devenv.exe`)    |
| Herramientas relacionadas en DSVM      |     Visual Studio Code, RStudio y Juno  |

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de uso general      |
| Versiones de DSVM compatibles      | Windows, Linux     |
| Usos típicos      | Editor de código e integración de Git   |
| ¿Cómo se usa o ejecuta?      | Acceso directo de escritorio (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`) en Windows o acceso directo de escritorio o terminal (`code`) en Linux    |
| Herramientas relacionadas en DSVM      |     Visual Studio 2017, RStudio y Juno  |

## <a name="rstudio--desktop"></a>RStudio Desktop 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de cliente para R    |
| Versiones de DSVM compatibles      | Windows, Linux      |
| Usos típicos      |  Desarrollo de R     |
| ¿Cómo se usa o ejecuta?      | Acceso directo de escritorio (`C:\Program Files\RStudio\bin\rstudio.exe`) en Windows y acceso directo de escritorio (`/usr/bin/rstudio`) en Linux      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y Juno      |

## <a name="rstudio--server"></a>RStudio Server 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE basada en web para R    |
| Versiones de DSVM compatibles      | Linux      |
| Usos típicos      |  Desarrollo de R     |
| ¿Cómo se usa o ejecuta?      | Habilite el servicio con _systemctl enable rstudio-server_ y, a continuación, inicie el servicio con _systemctl start rstudio-server_. A continuación, puede iniciar sesión en RStudio Server en http:\// su-vm-ip:8787.       |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de cliente para el lenguaje Julia   |
| Versiones de DSVM compatibles      | Windows, Linux      |
| Usos típicos      |  Desarrollo de Julia     |
| ¿Cómo se usa o ejecuta?      | Acceso directo de escritorio (`C:\JuliaPro-0.5.1.1\Juno.bat`) en Windows y acceso directo de escritorio (`/opt/JuliaPro-VERSION/Juno`) en Linux      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de cliente para el lenguaje Python    |
| Versiones de DSVM compatibles      | Linux      |
| Usos típicos      |  Desarrollo de Python     |
| ¿Cómo se usa o ejecuta?      | Acceso directo de escritorio (`/usr/bin/pycharm`) en Linux      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y RStudio      |



## <a name="powerbi-desktop"></a>PowerBI Desktop 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | Visualización de datos interactivos y herramienta de BI    |
| Versiones de DSVM compatibles      |  Windows  |
| Usos típicos      |  Visualización de datos y creación de paneles   |
| ¿Cómo se usa o ejecuta?      | Acceso directo de escritorio (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y Juno      |

