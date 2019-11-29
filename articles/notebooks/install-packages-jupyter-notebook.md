---
title: Instalación de paquetes de un cuaderno de Jupyter en Azure
description: Instalación de paquetes de Python, R y F# de un cuaderno de Jupyter que se ejecuta en Azure.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: 5d85c8e936ce7c8bf38ec7bc9c27d9066cc8b155
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277546"
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

Luego, cargue el generador de paquetes:
```fsharp
#load "Paket.Generated.Refs.fsx"
```

Abra la biblioteca:
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>Pasos siguientes

- [How to: Configure and manage projects](configure-manage-azure-notebooks-projects.md) (Configuración y administración de proyectos)
- [Instrucciones: Presentación con diapositivas](present-jupyter-notebooks-slideshow.md)
