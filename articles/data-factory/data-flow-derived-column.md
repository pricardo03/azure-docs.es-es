---
title: 'Transformación Columna derivada en Mapping Data Flow: Azure Data Factory | Microsoft Docs'
description: Aprenda cómo transformar datos a escala en Azure Data Factory con la transformación Columna derivada en Mapping Data Flow.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 941c629fd8359edc7fc1cf364a6735314044d95e
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312185"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Transformación Columna derivada en Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Use la transformación Columna derivada para generar nuevas columnas en el flujo de datos o para modificar los campos existentes.

## <a name="derived-column-settings"></a>Configuración de columna derivada

Para reemplazar una columna existente, selecciónela mediante la lista desplegable de la columna. De lo contrario, utilice el campo de selección de columna como un cuadro de texto y escriba el nombre de la nueva columna. Para crear la expresión de la columna derivada, haga clic en el cuadro "Escribir expresión" para abrir el [Generador de expresiones de Data Flow](concepts-data-flow-expression-builder.md).

![Configuración de columna derivada](media/data-flow/dc1.png "Derived column settings")

Para agregar columnas derivadas adicionales, mantenga el mouse sobre una columna derivada existente y haga clic en "+". A continuación, elija "Agregar columna" o "Add column pattern" (Agregar patrón de columna). Los patrones de columna pueden resultar útiles si los nombres de columna son variables de los orígenes. Para más información, consulte [Patrones de columna](concepts-data-flow-column-pattern.md).

![Selección de nueva columna derivada](media/data-flow/columnpattern.png "New derived column selection")

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el [lenguaje de expresiones de Mapping Data Flow](data-flow-expression-functions.md).
