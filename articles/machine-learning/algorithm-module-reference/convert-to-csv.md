---
title: 'Convertir a CSV: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Convertir a CSV en Azure Machine Learning para convertir un conjunto de datos a un formato CSV que se pueda descargar, exportar o compartir con módulos de script de R o Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 999f78ab08e1a2c9dd6b28d853e49fbb559fab83
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73493847"
---
# <a name="convert-to-csv-module"></a>Módulo Convertir a CSV

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Use este módulo para convertir un conjunto de datos a un formato CSV que se pueda descargar, exportar o compartir con módulos de script de R o Python.

### <a name="more-about-the-csv-format"></a>Más información sobre el formato CSV 

El formato CSV, que es el acrónimo de "valores separados por comas", es un formato de archivo utilizado por muchas herramientas externas de Machine Learning. CSV es un formato de intercambio común cuando se trabaja con lenguajes de código abierto como R o Python.

Incluso si realiza la mayoría del trabajo en Azure Machine Learning, hay veces en que le resultará útil convertir el conjunto de datos a CSV para usarlo en herramientas externas. Por ejemplo:

+ Descargue el archivo CSV para abrirlo con Excel o importarlo a una base de datos relacional.  
+ Guarde el archivo CSV en el almacenamiento en la nube y conéctese desde Power BI para crear visualizaciones.  
+ Use el formato CSV para preparar los datos para su uso en R y Python. Simplemente haga clic con el botón derecho en la salida del módulo a fin de generar el código necesario para tener acceso a los datos directamente desde Python o un cuaderno de Jupyter. 

Al convertir un conjunto de datos a CSV, el archivo se guarda en el área de trabajo de Azure ML. Puede usar una utilidad de Azure Storage para abrir y usar el archivo directamente o puede hacer clic con el botón derecho en la salida del módulo y descargar el archivo CSV en el equipo o utilizarlo en código R o Python.  

## <a name="how-to-configure-convert-to-csv"></a>Procedimiento para configurar Convertir a CSV


1.  Agregue el módulo [Convertir a CSV](./convert-to-csv.md) a la canalización. Puede encontrar este módulo en el grupo **Conversiones de formato de datos** del diseñador. 

2. Conéctelo a cualquier módulo que genere un conjunto de datos.   
  
3.  Ejecución de la canalización

### <a name="results"></a>Results
  

Haga doble clic en la salida de [Convertir a CSV](./convert-to-csv.md) y seleccione una de estas opciones.  

 + **Result Dataset (Conjuntos de datos del resultado) -> Descargar**: se abre inmediatamente una copia de los datos en formato CSV que se puede guardar en una carpeta local. Si no especifica una carpeta, se aplica un nombre de archivo predeterminado y el archivo CSV se guarda en la biblioteca local **Descargas**.


 + **Result Dataset (Conjuntos de datos del resultado) -> Guardar como conjunto de datos**: guarda el archivo CSV de nuevo en el área de trabajo de Azure ML como un conjunto de datos independiente.

 + **Generate Data Access Code** (Generar código de acceso a datos): Azure ML genera dos conjuntos de código para tener acceso a los datos, ya sea mediante el uso de Python o de R. Para obtener acceso a los datos, copie el fragmento de código en la aplicación. (*Generar código de acceso a datos estará pronto disponible*).

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 