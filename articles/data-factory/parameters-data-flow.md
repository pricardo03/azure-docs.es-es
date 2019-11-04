---
title: Parámetros de flujo de datos de asignación de Azure Data Factory
description: Obtenga información sobre cómo parametrizar una asignación de Data Flow desde canalizaciones de Data Factory.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 0a1051d67bf45e96f82833ef8190008204cdc90b
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "72387533"
---
# <a name="mapping-data-flow-parameters"></a>Parámetros de flujo de datos de asignación



La asignación de instancias de Data Flow en Azure Data Factory admite el uso de parámetros. Es posible definir parámetros en la definición del flujo de datos que, después, se pueden usar en las expresiones. Posteriormente, la canalización que realiza la llamada puede establecer los valores de parámetro mediante la actividad de ejecución de Data Flow. Dispone de tres opciones para establecer los valores en las expresiones de la actividad del flujo de datos:

* Usar el lenguaje de expresiones del flujo de control de canalizaciones para establecer un valor dinámico
* Usar el lenguaje de expresiones del flujo de datos para establecer un valor dinámico
* Usar cualquiera de estos dos lenguajes de expresiones para establecer un valor literal estático

Use esta funcionalidad para hacer que los flujos de datos sean flexibles y reutilizables y estén destinados para un uso general. Puede parametrizar las expresiones y los valores del flujo de datos con estos parámetros.

> [!NOTE]
> Para usar expresiones del flujo de control de canalizaciones, el parámetro del flujo de datos debe ser de tipo cadena.

## <a name="create-parameters-in-mapping-data-flow"></a>Creación de parámetros en flujo de datos de asignación

Para agregar parámetros a un flujo de datos, haga clic en la parte en blanco del lienzo del flujo de datos para ver las propiedades generales. En el panel de configuración, verá una pestaña denominada Parámetros. Haga clic en el botón Nuevo para generar un nuevo parámetro. Para cada parámetro, debe asignar un nombre, seleccionar un tipo y opcionalmente establecer un valor predeterminado.

![Creación de parámetros de Data Flow](media/data-flow/create-params.png "Creación de parámetros de Data Flow")

Los parámetros se pueden usar en cualquier expresión de flujo de datos. Los parámetros comienzan por $ y son inmutables. Encontrará la lista de parámetros disponibles en el Generador de expresiones en la pestaña Parámetros.

![Expresión de parámetros de flujo de datos](media/data-flow/parameter-expression.png "Expresión de parámetros de flujo de datos")

## <a name="use-parameters-in-your-data-flow"></a>Uso de parámetros en el flujo de datos

* Puede usar valores de parámetro en las expresiones de transformación. Encontrará la lista de parámetros en la pestaña Parámetros del Generador de expresiones. ![Uso de parámetros de Data Flow](media/data-flow/params9.png "UUso de parámetros de Data Flow")

* Los parámetros también se usan para configurar valores dinámicos para los valores de configuración de transformación Origen y Receptor. Al hacer clic en los campos configurables, aparece el vínculo Agregar contexto dinámico. Al hacer clic en él, se le dirigirá a un Generador de expresiones donde puede usar parámetros para emplear valores dinámicos. ![Contenido dinámico de flujo de datos](media/data-flow/params6.png "DContenido dinámico de flujo de datos")

## <a name="set-mapping-data-flow-parameters-from-pipeline"></a>Establecimiento de parámetros de flujo de datos de asignación desde la canalización

Una vez que haya creado el flujo de datos con parámetros, podrá ejecutarlo desde una canalización con la actividad de ejecución de Data Flow. Después de agregar la actividad al lienzo de la canalización, se le mostrarán los parámetros de flujo de datos disponibles en la pestaña Parámetros de la actividad.

![Establecimiento de un parámetro de Data Flow](media/data-flow/parameter-assign.png "Establecimiento de un parámetro de Data Flow")

Si el tipo de datos del parámetro es una cadena, al hacer clic en el cuadro de texto para establecer los valores del parámetro, puede especificar una expresión de canalización o de flujo de datos. Si opta por una expresión de canalización, aparecerá el panel de expresiones de canalización. Asegúrese de incluir funciones de canalización en la sintaxis de interpolación de cadenas mediante `'@{<expression>}'`, por ejemplo:

```'@{pipeline().RunId}'```

Si el parámetro no es de tipo cadena, se le mostrará el Generador de expresiones de Data Flow. Aquí puede escribir cualquier expresión o valores literales que coincidan con el tipo de datos del parámetro. A continuación se muestran ejemplos de una expresión de flujo de datos y una cadena literal del Generador de expresiones:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Cada flujo de datos de asignación puede tener cualquier combinación de parámetros de expresión de canalización y flujo de datos. 

![Ejemplo de parámetros de flujo de datos](media/data-flow/parameter-example.png "Ejemplo de parámetros de flujo de datos")



## <a name="next-steps"></a>Pasos siguientes
* [Actividad de ejecución de Data Flow](control-flow-execute-data-flow-activity.md)
* [Expresiones del flujo de control](control-flow-expression-language-functions.md)
