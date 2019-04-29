---
title: Información general de la asignación de Data Flow en Azure Data Factory
description: Explicación general de la asignación de flujos de datos en Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 5064f47444b4ca6d9194ed66144938e6e3d51a4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261985"
---
# <a name="what-are-mapping-data-flows-in-azure-data-factory"></a>¿Qué son los flujos de datos asignados en Azure Data Factory?

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

La asignación de flujos de datos permite a los ingenieros de datos desarrollar una lógica de transformación de datos gráfica sin tener que escribir código. Los flujos de datos resultantes se ejecutan como actividades en las canalizaciones de Azure Data Factory mediante clústeres de Azure Databricks de escalabilidad horizontal.

La intención de Data Flow de Azure Data Factory es proporcionar una experiencia totalmente visual sin necesidad de codificación. Sus flujos de datos se ejecutarán en su propio clúster de ejecución para realizar el procesamiento de datos de escalabilidad horizontal. Asimismo, Azure Data Factory controla toda la traducción de código, la optimización de rutas de acceso y la ejecución de trabajos de flujo de datos.

Puede comenzar creando flujos de datos en el modo de depuración, para así poder validar la lógica de transformación de forma interactiva. A continuación, agregue una actividad de Data Flow a la canalización para ejecutar y probar el flujo de datos en la depuración de la canalización, o use "Desencadenar ahora" en la canalización para probar el flujo de datos de una actividad de canalización.

A continuación, debe programar y supervisar sus actividades de flujo de datos mediante las canalizaciones de Azure Data Factory que ejecutan la actividad de Data Flow.

La alternancia del modo de depuración en la superficie de diseño de Data Flow permite la compilación interactiva de las transformaciones de datos. Igualmente, el modo de depuración proporciona un entorno de preparación de datos para la construcción del flujo de datos.
