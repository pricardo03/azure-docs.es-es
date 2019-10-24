---
title: Herramientas de desarrollo
titleSuffix: Azure Data Science Virtual Machine
description: Obtenga información sobre las herramientas y los entornos de desarrollo integrado disponibles en Data Science Virtual Machine.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/3/2019
ms.openlocfilehash: 76a550e95de24bf65b9b6097dd332e535da5b1c4
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330716"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Herramientas de desarrollo en Azure Data Science Virtual Machine

Data Science Virtual Machine (DSVM) agrupa varias herramientas populares en un entorno de desarrollo integrado (IDE) altamente productivo. A continuación presentamos algunas herramientas que se proporcionan en DSVM.

## <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de uso general      |
| Versiones de DSVM compatibles      | Windows      |
| Usos típicos      | Desarrollo de software    |
| ¿Cómo se configura e instala en DSVM?      | Carga de trabajo de ciencia de datos (herramientas de Python y R), carga de trabajo de Azure (Hadoop, Data Lake), Node.js, herramientas de SQL Server, [Azure Machine Learning para Visual Studio Code](https://github.com/Microsoft/vs-tools-for-ai)    |
| Cómo usarla y ejecutarla      | Acceso directo de escritorio (`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\devenv.exe`). De forma gráfica, abra Visual Studio mediante el icono de escritorio o el menú **Inicio**. Busque programas (logotipo de Windows + S), seguido de **Visual Studio**. Desde allí, puede crear proyectos en lenguajes como C#, Python, R o Node.js.   |
| Herramientas relacionadas en DSVM      |     Visual Studio Code, RStudio y Juno  |

> [!NOTE]
> Quizás vea un mensaje que indica que el período de evaluación ha expirado. Escriba las credenciales de su cuenta Microsoft. O cree una nueva cuenta gratuita para acceder a Visual Studio Community.

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de uso general      |
| Versiones de DSVM compatibles      | Windows, Linux     |
| Usos típicos      | Editor de código e integración de Git   |
| Cómo usarla y ejecutarla      | Acceso directo de escritorio (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`) en Windows o acceso directo de escritorio o terminal (`code`) en Linux    |
| Herramientas relacionadas en DSVM      |     Visual Studio 2017, RStudio y Juno  |

## <a name="rstudio--desktop"></a>RStudio Desktop 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de cliente para el lenguaje R   |
| Versiones de DSVM compatibles      | Windows, Linux      |
| Usos típicos      |  Desarrollo de R     |
| Cómo usarla y ejecutarla      | Acceso directo de escritorio (`C:\Program Files\RStudio\bin\rstudio.exe`) en Windows y acceso directo de escritorio (`/usr/bin/rstudio`) en Linux      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y Juno      |

## <a name="rstudio--server"></a>RStudio Server 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de cliente para el lenguaje R   |
| ¿Qué es?   | IDE basada en web para R    |
| Versiones de DSVM compatibles      | Linux      |
| Usos típicos      |  Desarrollo de R     |
| Cómo usarla y ejecutarla      | Habilite el servicio con _systemctl enable rstudio-server_ y, a continuación, inicie el servicio con _systemctl start rstudio-server_. A continuación, inicie sesión en RStudio Server en http:\//your-vm-ip:8787.       |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de cliente para el lenguaje Julia   |
| Versiones de DSVM compatibles      | Windows, Linux      |
| Usos típicos      |  Desarrollo de Julia     |
| Cómo usarla y ejecutarla      | Acceso directo de escritorio (`C:\JuliaPro-0.5.1.1\Juno.bat`) en Windows y acceso directo de escritorio (`/opt/JuliaPro-VERSION/Juno`) en Linux      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | IDE de cliente para el lenguaje Python    |
| Versiones de DSVM compatibles      | Linux      |
| Usos típicos      |  Desarrollo de Python     |
| Cómo usarla y ejecutarla      | Acceso directo de escritorio (`/usr/bin/pycharm`) en Linux      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y RStudio      |



## <a name="power-bi-desktop"></a>Power BI Desktop 

|    |           |
| ------------- | ------------- |
| ¿Qué es?   | Visualización de datos interactivos y herramienta de BI    |
| Versiones de DSVM compatibles      | Windows  |
| Usos típicos      |  Visualización de datos y creación de paneles   |
| Cómo usarla y ejecutarla      | Acceso directo de escritorio (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Herramientas relacionadas en DSVM      |   Visual Studio 2017, Visual Studio Code y Juno      |

