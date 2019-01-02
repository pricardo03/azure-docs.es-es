---
title: Instalación de paquetes de un cuaderno de Jupyter en Azure
description: Instalación de paquetes de Python, R y F# de un cuaderno de Jupyter que se ejecuta en Azure.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 6f089c12-128b-4dbd-96e3-1320d37eeba4
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 5baa392d098b0b0e40986bc426c88785db025a29
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255302"
---
# <a name="install-packages-from-within-a-notebook"></a>Instalación de paquetes de un cuaderno

Aunque puede configurar el [entorno para el cuaderno en el nivel de proyecto](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), quizá desee instalar los paquetes directamente desde un cuaderno.

Los paquetes instalados desde el cuaderno se aplican solo a la sesión actual del servidor. Las instalaciones de paquetes no se conservan una vez apagado el servidor.

## <a name="python"></a>Python

Los paquetes de Python pueden instalarse con pip o conda usando comandos en celdas de código:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Si la salida del comando indica que ya se cumple el requisito, Azure Notebooks puede incluir el paquete de forma predeterminada. También se puede instalar el paquete mediante un [paso de instalación del entorno del proyecto](configure-manage-azure-notebooks-projects.md#configure-the-project-environment).

## <a name="r"></a>R

Los paquetes de R pueden instalarse desde CRAN o GitHub mediante la función `install.packages` en una celda de código:

```r
install.packages("package_name")
```

También puede instalar versiones preliminares y otros paquetes de desarrollo desde GitHub con la biblioteca devtools:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

Los paquetes de F# puede instalarse desde [nuget.org](https://www.nuget.org) mediante una llamada al administrador de dependencias Paket desde las celdas de código. En primer lugar, cargue el administrador Paket:

```fsharp
#load "Paket.fsx"
```

A continuación, instale los paquetes:

```fsharp
Paket.Package
[ "MathNet.Numerics"
"MathNet.Numerics.FSharp"
]
```

## <a name="next-steps"></a>Pasos siguientes

- [Instrucciones: Configuración y administración de proyectos](configure-manage-azure-notebooks-projects.md)
- [Instrucciones: Presentación con diapositivas](present-jupyter-notebooks-slideshow.md)
