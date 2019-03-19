---
title: Actividad de establecimiento de variables en Azure Data Factory | Microsoft Docs
description: Aprenda cómo usar la actividad de establecimiento de variables para establecer el valor de una variable existente definida en una canalización de Data Factory
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2018
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: 71abfdff629f36b278488851b546c7371353a4d9
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57575297"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Actividad de establecimiento de variables en Azure Data Factory

Utilice la actividad de establecimiento de variables para establecer el valor de una variable existente de tipo cadena, booleano o matriz definida en una canalización de Data Factory.

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | DESCRIPCIÓN | Obligatorio
-------- | ----------- | --------
Nombre | Nombre de la actividad en la canalización | Sí
description | Texto que describe para qué se usa la actividad | no
Tipo | El tipo de actividad es SetVariable | Sí
value | Cadena literal o valor del objeto de expresión usado para establecer la variable especificada | Sí
variableName | Nombre de la variable que se establecerá en esta actividad | Sí


## <a name="next-steps"></a>Pasos siguientes
Obtenga información acerca de una actividad de flujo de control relacionada compatible con Data Factory: 

- [Actividad de anexión de variables](control-flow-append-variable-activity.md)
