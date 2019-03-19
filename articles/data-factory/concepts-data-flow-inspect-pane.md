---
title: Inspección y vista previa de datos de Azure Data Factory Mapping Data Flow
description: Azure Data Factory Mapping Data Flow tiene un panel que muestra los metadatos de los flujos de datos (inspección) y la vista previa de los datos cuando está en modo de depuración (vista previa de datos)
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 47cde50e0874f0f73523925b6d1b2f8ee4abaea9
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56727832"
---
# <a name="azure-data-factory-mapping-data-flow-transformation-inspect-tab"></a>Pestaña de inspección de las transformaciones de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Panel Inspeccionar](media/data-flow/agg3.png "Inspect Pane")

El panel de inspección proporciona una vista de los metadatos del flujo de datos que se va a transformar. Podrá ver los recuentos de columnas, las columnas que han cambiado, las columnas que se han agregado, los tipos de datos, el orden de las columnas y las referencias de las columnas. "Inspeccionar" es una vista de solo lectura de los metadatos. Para ver los metadatos en el panel de inspección no es preciso que el modo de depuración esté habilitado.

Al cambiar la forma de los datos mediante transformaciones, verá que los cambios de los metadatos fluyen a través del panel de inspección. Si no hay un esquema definido en la transformación de origen, los metadatos no se verán en el panel de inspección. La falta de metadatos es habitual en escenarios de desviación en el esquema.

![Vista previa de datos](media/data-flow/datapreview.png "Data Preview")

Vista previa de datos es un panel que proporciona una vista de solo lectura de los datos durante su transformación. Puede ver tanto la salida de la transformación como expresiones para validar el flujo de datos. Para poder disfrutar de las vistas previas de datos, es precioso que el modo de depuración esté activado. Al hacer clic en las columnas de la cuadrícula de vista previa de datos, verá otro panel a la derecha. El panel emergente mostrará las estadísticas del perfil de las columnas que seleccione.

![Gráfico de vista previa de datos](media/data-flow/chart.png "Data Preview Chart")

