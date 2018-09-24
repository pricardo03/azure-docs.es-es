---
title: Ejemplo de Python para derivar nuevas columnas en la preparación de datos de Azure Machine Learning | Microsoft Docs
description: En este documento se proporcionan ejemplos de código de Python para crear nuevas columnas en la preparación de datos de Azure Machine Learning.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 43a66efde2623a848065bbde2be4b56fd6c7fc8b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969315"
---
# <a name="sample-of-custom-column-transforms-python"></a>Ejemplo de transformaciones de columna personalizada (Python) 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


El nombre de esta transformación en el menú es **Add Column (Script)** (Agregar columna [Script]).

Antes de leer este apéndice, vea la [Introducción a la extensibilidad de Python](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Probar la equivalencia y reemplazar valores 
Si el valor de Col1 es inferior a 4, la nueva columna debe tener un valor de 1. Si el valor de Col1 es superior a 4, la nueva columna debe tener un valor de 2. 

```python
    1 if row["Col1"] < 4 else 2
```
## <a name="current-date-and-time"></a>Fecha y hora actual 

```python
    datetime.datetime.now()
```
## <a name="typecasting"></a>Conversión de tipo 
```python
    float(row["Col1"]) / float(row["Col2"] - 1)
```
## <a name="evaluate-for-nullness"></a>Evaluar los valores null 
Si Col1 contiene un valor null, marque la nueva columna como **Bad** (No válida). En caso contrario, márquela como **Good** (Válida). 

```python
    'Bad' if pd.isnull(row["Col1"]) else 'Good'
```
## <a name="new-computed-column"></a>Nueva columna calculada 
```python
    np.log(row["Col1"])
```
## <a name="epoch-computation"></a>Cálculo de tiempo 
Número de segundos transcurridos desde el tiempo Unix (suponiendo que Col1 ya sea una fecha): 
```python
    row["Col1"] - datetime.datetime.utcfromtimestamp(0)).total_seconds()
```

## <a name="hash-a-column-value-into-a-new-column"></a>Aplicación de un algoritmo hash de un valor de columna en una columna nueva
```python
    import hashlib
    hash(row["MyColumnToHashCol1"])

```




