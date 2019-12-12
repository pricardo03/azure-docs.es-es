---
title: Actividad de establecimiento de variables en Azure Data Factory
description: Aprenda cómo usar la actividad de establecimiento de variables para establecer el valor de una variable existente definida en una canalización de Data Factory
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/10/2018
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.openlocfilehash: 88500ecbc56b34551a0cbd3ca94727ba4bbcda9f
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930641"
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
