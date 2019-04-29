---
title: Patrones de columna de Azure Data Factory Mapping Data Flow
description: Los patrones de columna de Azure Data Factory Mapping Data Flow se utilizan para crear patrones de plantilla generalizados para transformar los campos de un flujo de datos sin tener en cuenta los metadatos del esquema subyacentes
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 53d3300ea11a86c34909ba6ce0fd6c8c0c38b4b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269720"
---
# <a name="azure-data-factory-mapping-data-flow-concepts"></a>Conceptos de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Varias transformaciones de Azure Data Factory Data Flow admiten la idea de "Patrones de columnas" para que pueda crear columnas de plantilla basadas en patrones, en lugar de nombres de columna codificados de forma rígida. Esta característica se puede usar en Generador de expresiones para definir patrones que coinciden con las columnas para la transformación, en lugar de requerir nombres de campo específicos y exactos. Los patrones son útiles cuando los campos de origen de entrada cambian a menudo, en concreto en el caso de que cambien columnas en archivos de texto o bases de datos NoSQL. A veces esto se denomina "Desviación en el esquema".

![patrones de columna](media/data-flow/columnpattern2.png "Column Patterns")

Los patrones de columna son útiles para controlar los tanto los escenarios de desviación en el esquema como los escenarios generales. Es bueno en los casos en los que no puede conocer por completo el nombre de todas y cada una de las columnas. Los patrones pueden coincidir por nombre y tipo de datos de columna y crear una expresión para la transformación que ejecutará dicha operación en cualquier campo del flujo de datos que coincida con sus patrones `name` & `type`.

Cuando agregue una expresión a una transformación que acepta patrones, elija "Agregar columna Pattern" (Agregar patrón de columna). Patrones de columnas permite patrones coincidentes de columna con desplazamiento de esquema.

Al crear patrones de las columnas de la plantilla, use `$$` en la expresión para representar una referencia a cada campo coincidente desde el flujo de datos de entrada.

Si elige usar una de las funciones de expresión regular del Generador de expresiones, posteriormente podrá usar $1, $2, $3... para hacer referencia a los patrones secundarios que coinciden con su expresión regular.

Un ejemplo de escenario de patrón de columnas usa SUM con una serie de campos de entrada. Los cálculos de SUM de agregados se encuentran en la transformación Agregado. Posteriormente puede usar SUM en todas las coincidencias de tipos de campos que coincidan con "integer" y, después, usar $$ para hacer referencia a cada coincidencia en la expresión.
