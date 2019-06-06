---
title: Patrones de columna de Azure Data Factory Mapping Data Flow
description: Aprenda a usar patrones de columnas de factoría de datos de Azure en asignación de flujo de datos para crear patrones de plantilla generalizada para transformar los campos en un flujo de datos sin tener en cuenta los metadatos subyacentes de esquema
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 08cdaafe00b7dc586ea75f6ff03fdb89107edee9
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430759"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Patrones de columnas de flujos de datos de asignación de Azure data factory

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Varias transformaciones de Azure Data Factory Data Flow admiten la idea de "Patrones de columnas" para que pueda crear columnas de plantilla basadas en patrones, en lugar de nombres de columna codificados de forma rígida. Puede usar esta característica en el generador de expresiones para definir los patrones para que coincida con las columnas para la transformación en lugar de requerir nombres de campo específico, exacto. Patrones son útiles si los campos de origen entrante cambian con frecuencia, especialmente en el caso de cambio de columnas en los archivos de texto o bases de datos NoSQL. Esta condición se conoce a veces como "Desviación en el esquema".

![patrones de columna](media/data-flow/columnpattern2.png "Column Patterns")

Los patrones de columna son útiles para controlar los tanto los escenarios de desviación en el esquema como los escenarios generales. Es bueno en los casos en los que no puede conocer por completo el nombre de todas y cada una de las columnas. Los patrones pueden coincidir por nombre y tipo de datos de columna y crear una expresión para la transformación que ejecutará dicha operación en cualquier campo del flujo de datos que coincida con sus patrones `name` & `type`.

Cuando agregue una expresión a una transformación que acepta patrones, elija "Agregar columna Pattern" (Agregar patrón de columna). Patrones de columnas permite patrones coincidentes de columna con desplazamiento de esquema.

Al crear patrones de las columnas de la plantilla, use `$$` en la expresión para representar una referencia a cada campo coincidente desde el flujo de datos de entrada.

Si decide usar una de las funciones de expresión regular del generador de expresiones,, a continuación, puede usar posteriormente $1, 2 $, $3... Para hacer referencia a los subpatrones coincidentes desde la expresión regex.

Un ejemplo de escenario de patrón de columnas usa SUM con una serie de campos de entrada. Los cálculos de SUM de agregados se encuentran en la transformación Agregado. A continuación, puede usar suma en cada coincidencia de tipos de campo que coincide con "integer" y, a continuación, utilice $$ para hacer referencia a cada coincidencia en la expresión.
