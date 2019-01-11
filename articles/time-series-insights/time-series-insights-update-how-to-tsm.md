---
title: Modelado de datos en la versión preliminar de Azure Time Series Insights | Microsoft Docs
description: Obtenga más información acerca del modelado de datos de la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 1686b52c9dacf5c78335f76bd19b41c4c7d15cee
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555411"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Modelado de datos en la versión preliminar de Azure Time Series Insights

En este documento se describe cómo trabajar con los modelos de serie temporal después de usar la versión preliminar de Azure Time Series Insights. Aquí se detallan varios escenarios comunes de datos.

Para obtener más información acerca de cómo usar la actualización, lea [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Explorador de versión preliminar de Azure Time Series Insights).

## <a name="types"></a>Tipos

### <a name="create-a-single-type"></a>Crear un solo tipo

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Types** (Tipos) en el menú. Contraiga el panel para centrarse en los tipos de modelos de serie temporal.

    ![Portal_one][1]

1. Seleccione **Agregar**.
1. Escriba todos los detalles referentes a los tipos y seleccione **Create** (Crear). Esta acción crea tipos en el entorno.

    ![Portal_two][2]

### <a name="bulk-upload-one-or-more-types"></a>Carga en bloque de uno o más tipos

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene el tipo de carga útil.
1. Seleccione **Cargar**.

    ![Portal_three][3]

### <a name="edit-a-single-type"></a>Editar un solo tipo

Seleccione el tipo y seleccione **Edit** (Editar). Realice los cambios necesarios y seleccione **Save** (Guardar).

![Portal_four][4]

### <a name="delete-a-type"></a>Eliminar un tipo

Seleccione el tipo y **Delete** (Eliminar). Si no hay instancias asociadas a los tipos, este se elimina.

![Portal_five][5]

## <a name="hierarchies"></a>Jerarquías

### <a name="create-a-single-hierarchy"></a>Crear una sola jerarquía

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Hierarchies** (Jerarquías) en el menú. Contraiga el panel para centrarse en las jerarquías de modelos de serie temporal.

    ![Portal_six][6]

1. Seleccione **Agregar**.

    ![Portal_seven][7]

1. Seleccione **Add Level** (Agregar nivel) en el panel derecho.

    ![Portal_eight][8]

1. Escriba los detalles de la jerarquía y seleccione **Create** (Crear).

    ![Portal_nine][9]

### <a name="bulk-upload-one-or-more-hierarchies"></a>Carga en bloque de una o más jerarquías

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la jerarquía.
1. Seleccione **Cargar**.

    ![Portal_ten][10]

### <a name="edit-a-single-hierarchy"></a>Editar una sola jerarquía

Seleccione la jerarquía y seleccione **Edit** (Editar). Realice los cambios necesarios y seleccione **Save** (Guardar).

![Portal_eleven][11]

### <a name="delete-a-hierarchy"></a>Eliminar una jerarquía

Seleccione la jerarquía y seleccione **Delete** (Eliminar). Si no hay instancias asociadas a la jerarquía, esta se elimina.

![Portal_twelve][12]

## <a name="instances"></a>Instancias

### <a name="create-a-single-instance"></a>Crear una sola instancia

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Instances** (Instancias) en el menú. Contraiga el panel para centrarse en las instancias de modelos de serie temporal.

    ![Portal_thirteen][13]

1. Seleccione **Agregar**.

    ![Portal_fourteen][14]

1. Escriba los detalles de la instancia, seleccione la asociación de tipo y de jerarquía y seleccione **Create** (Crear).

### <a name="bulk-upload-one-or-more-instances"></a>Carga en bloque de una o más instancias

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la instancia.

    ![Portal_fifteen][15]

1. Seleccione **Cargar**.

### <a name="edit-a-single-instance"></a>Editar una sola instancia

Seleccione la instancia y seleccione **Edit** (Editar). Realice los cambios necesarios y seleccione **Save** (Guardar).

![Portal_sixteen][16]

### <a name="delete-an-instance"></a>Eliminar una instancia

Seleccione la instancia y seleccione **Delete** (Eliminar). Si no hay eventos asociados a las instancias, esta se elimina.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de los modelos de serie temporal, lea [Data modeling](./time-series-insights-update-tsm.md) (Modelado de datos).
- Para obtener más información acerca de la versión preliminar, lea [Visualize data in the Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Visualizar datos en el explorador de la versión preliminar de Azure Time Series Insights).
- Para obtener información sobre las formas JSON admitidas, consulte [Supported JSON shapes](./time-series-insights-send-events.md#json) (Formas JSON admitidas).

<!-- Images -->
[1]: media/v2-update-how-to-tsm/portal_one.png
[2]: media/v2-update-how-to-tsm/portal_two.png
[3]: media/v2-update-how-to-tsm/portal_three.png
[4]: media/v2-update-how-to-tsm/portal_four.png
[5]: media/v2-update-how-to-tsm/portal_five.png
[6]: media/v2-update-how-to-tsm/portal_six.png
[7]: media/v2-update-how-to-tsm/portal_seven.png
[8]: media/v2-update-how-to-tsm/portal_eight.png
[9]: media/v2-update-how-to-tsm/portal_nine.png
[10]: media/v2-update-how-to-tsm/portal_ten.png
[11]: media/v2-update-how-to-tsm/portal_eleven.png
[12]: media/v2-update-how-to-tsm/portal_twelve.png
[13]: media/v2-update-how-to-tsm/portal_thirteen.png
[14]: media/v2-update-how-to-tsm/portal_fourteen.png
[15]: media/v2-update-how-to-tsm/portal_fifteen.png
[16]: media/v2-update-how-to-tsm/portal_sixteen.png