---
title: 'Convertir a CSV: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar a la función Convert para el módulo CSV en el servicio de Azure Machine Learning para convertir un conjunto de datos en un formato CSV que se puede descargar, exportar o compartir con módulos de script de R o Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 8b8b6758cc2df7a092ce36e9507f84ac534d0e3d
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028731"
---
# <a name="convert-to-csv-module"></a>Convertir en módulo CSV

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para convertir un conjunto de datos en un formato CSV que se puede descargar, exportar o compartir con módulos de script de R o Python.

### <a name="more-about-the-csv-format"></a>Más información sobre el formato CSV 

El formato CSV, que es el acrónimo "valores separados por comas", es un formato de archivo utilizado por el equipo externo muchas herramientas de aprendizaje. CSV es un formato de intercambio común al trabajar con lenguajes de código abierto como R o Python.

Incluso si lo hace la mayoría del trabajo en Azure Machine Learning, hay veces cuando le resultará útil para convertir el conjunto de datos en CSV que use en herramientas externas. Por ejemplo: 

+ Descargue el archivo CSV para abrirlo con Excel, o importarlo en una base de datos relacional.  
+ Guarde el archivo CSV para el almacenamiento en la nube y conectarse a él desde Power BI para crear visualizaciones.  
+ Use el formato CSV para preparar los datos para su uso en R y Python. Simplemente haga clic en la salida del módulo para generar el código necesario para tener acceso a los datos directamente desde Python o un cuaderno de Jupyter. 

Al convertir un conjunto de datos a CSV, el archivo se guarda en el área de trabajo de aprendizaje automático de Azure. Puede usar una utilidad de almacenamiento de Azure para abrir y usar el archivo directamente, o puede haga clic en la salida del módulo y descargar el archivo CSV en el equipo o utilizarlo en código R o Python.  

## <a name="how-to-configure-convert-to-csv"></a>Cómo configurar convertir a CSV

1.  Agregar el [convertir a CSV](./convert-to-csv.md) módulo al experimento. Puede encontrar este módulo en el **conversiones de formato de datos** grupo en la interfaz. 

2. Conéctelo a cualquier módulo que genera un conjunto de datos.   
  
3.  Ejecute el experimento.

### <a name="results"></a>Results
  

Haga doble clic en la salida de [convertir a CSV](./convert-to-csv.md)y seleccione una de estas opciones.  

 + **Conjunto de datos -> descarga**: Se abre inmediatamente una copia de los datos en formato CSV que se puede guardar en una carpeta local. Si no especifica una carpeta, se aplica un nombre de archivo predeterminado y se guarda el archivo CSV en el equipo local **descargas** biblioteca.


 + **Conjunto de datos -> Guardar como conjunto de datos**: Guarda el archivo CSV volver al área de trabajo de aprendizaje automático de Azure como un conjunto de datos independiente.

 + **Generar código de acceso a datos**: Aprendizaje automático de Azure genera dos conjuntos de código para tener acceso a los datos, ya sea mediante el uso de Python o R. Para obtener acceso a los datos, copie el fragmento de código en la aplicación. (*Generar código de acceso a datos vendrá pronto.* )

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 