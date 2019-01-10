---
title: Actividad de establecimiento de variables en Azure Data Factory | Microsoft Docs
description: Aprenda cómo usar la actividad de establecimiento de variables para establecer el valor de una variable existente definida en una canalización de Data Factory
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: douglasl
ms.openlocfilehash: cc573028779bcd6b77394bbeefbea58f714b835c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017351"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Actividad de establecimiento de variables en Azure Data Factory

Utilice la actividad de establecimiento de variables para establecer el valor de una variable existente de tipo cadena, booleano o matriz definida en una canalización de Data Factory.

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | DESCRIPCIÓN | Obligatorio
-------- | ----------- | --------
Nombre | Nombre de la actividad en la canalización | SÍ
description | Texto que describe para qué se usa la actividad | no
Tipo | El tipo de actividad es SetVariable | Sí
value | Cadena literal o valor del objeto de expresión usado para establecer la variable especificada | Sí
variableName | Nombre de la variable que se establecerá en esta actividad | Sí


## <a name="next-steps"></a>Pasos siguientes
Obtenga información acerca de una actividad de flujo de control relacionada compatible con Data Factory: 

- [Actividad de anexión de variables](control-flow-append-variable-activity.md)
