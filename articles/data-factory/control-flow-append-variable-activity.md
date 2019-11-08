---
title: Actividad de anexión de variables en Azure Data Factory
description: Aprenda a configurar la actividad de anexión de variables para agregar un valor a una variable de matriz existente definida en una canalización de Data Factory
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.date: 10/09/2018
ms.openlocfilehash: bcecd3411b6e6e751032f33862a671d8adbca046
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73679976"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Actividad de anexión de variables en Azure Data Factory

Use la actividad de anexión de variables para agregar un valor a una variable de matriz existente definida en una canalización de Data Factory.

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | DESCRIPCIÓN | Obligatorio
-------- | ----------- | --------
Nombre | Nombre de la actividad en la canalización | Sí
description | Texto que describe para qué se usa la actividad | no
Tipo | Tipo de actividad es AppendVariable | Sí
value | Cadena literal o valor del objeto de expresión usado para anexar a la variable especificada | Sí
variableName | Nombre de la variable que se va a modificar mediante la actividad; la variable debe ser de tipo 'Array' | Sí

## <a name="next-steps"></a>Pasos siguientes
Obtenga información acerca de una actividad de flujo de control relacionada compatible con Data Factory: 

- [Establecimiento de la actividad de variable](control-flow-set-variable-activity.md)
