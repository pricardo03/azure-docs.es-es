---
title: 'Modelado de datos en entornos de la versión preliminar: Azure Time Series Insights | Microsoft Docs'
description: Información acerca del modelado de datos en la versión preliminar de Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 10/29/2019
ms.custom: seodec18
ms.openlocfilehash: fb1bfb30d531f8b93489c8fc1dfbc6b8172339a3
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74006454"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Modelado de datos en la versión preliminar de Azure Time Series Insights

En este artículo se describe cómo usar el modelo de serie temporal de la versión preliminar de Azure Time Series Insights. Aquí se detallan varios escenarios comunes de datos.

Para obtener más información acerca de cómo usar la actualización, lea [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Explorador de versión preliminar de Azure Time Series Insights).

## <a name="types"></a>Tipos

### <a name="create-a-single-type"></a>Crear un solo tipo

1. Vaya al panel de selección del modelos de serie temporal y seleccione **Types** (Tipos) en el menú. Contraiga el panel para centrarse en los tipos del modelo de serie temporal.

    [![Panel "Types" (Tipos)](media/v2-update-how-to-tsm/portal-one.png)](media/v2-update-how-to-tsm/portal-one.png#lightbox)

1. Seleccione **+Agregar**.
1. Escriba todos los detalles referentes a los tipos y seleccione **Create** (Crear). Esta acción crea tipos en el entorno.

    [![Selecciones para agregar un tipo](media/v2-update-how-to-tsm/portal-two.png)](media/v2-update-how-to-tsm/portal-two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Carga en bloque de uno o más tipos

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene el tipo de carga útil.
1. Seleccione **Cargar**.

    [![Selecciones para la carga masiva de uno o varios tipos](media/v2-update-how-to-tsm/portal-three.png)](media/v2-update-how-to-tsm/portal-three.png#lightbox)

### <a name="edit-a-single-type"></a>Editar un solo tipo

1. Seleccione el tipo y seleccione **Edit** (Editar). 
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Selecciones para editar un tipo](media/v2-update-how-to-tsm/portal-four.png)](media/v2-update-how-to-tsm/portal-four.png#lightbox)

### <a name="delete-a-type"></a>Eliminar un tipo

1. Seleccione el tipo y **Delete** (Eliminar).
1. Si no hay instancias asociadas a los tipos, este se elimina.

    [![Botón "Delete"](media/v2-update-how-to-tsm/portal-five.png)](media/v2-update-how-to-tsm/portal-five.png#lightbox) (Eliminar)

## <a name="hierarchies"></a>Jerarquías

### <a name="create-a-single-hierarchy"></a>Crear una sola jerarquía

1. Vaya al panel de selección del modelo de serie temporal y seleccione **Hierarchies** (Jerarquías) en el menú. Contraiga el panel para centrarse en las jerarquías del modelo de serie temporal.

    [![Panel "Hierarchies"](media/v2-update-how-to-tsm/portal-six.png)](media/v2-update-how-to-tsm/portal-six.png#lightbox) (Jerarquías)

1. Seleccione **+Agregar**.

    [![Botón "Add"](media/v2-update-how-to-tsm/portal-seven.png)](media/v2-update-how-to-tsm/portal-seven.png#lightbox) (Agregar)

1. Seleccione **+Add Level** (Agregar nivel) en el panel derecho.

    [![Botón "Add Level"](media/v2-update-how-to-tsm/portal-eight.png)](media/v2-update-how-to-tsm/portal-eight.png#lightbox) (Agregar nivel)

1. Escriba los detalles de la jerarquía y seleccione **Create** (Crear).

    [![Detalles de la jerarquía y botón "Create" (Crear)](media/v2-update-how-to-tsm/portal-nine.png)](media/v2-update-how-to-tsm/portal-nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Carga en bloque de una o más jerarquías

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la jerarquía.
1. Seleccione **Cargar**.

    [![Selecciones para la carga masiva de jerarquías](media/v2-update-how-to-tsm/portal-ten.png)](media/v2-update-how-to-tsm/portal-ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Editar una sola jerarquía

1. Seleccione la jerarquía y seleccione **Edit** (Editar).
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Selecciones para la edición de una sola jerarquía](media/v2-update-how-to-tsm/portal-eleven.png)](media/v2-update-how-to-tsm/portal-eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Eliminar una jerarquía

1. Seleccione la jerarquía y, después, **Delete** (Eliminar). 
1. Si no hay instancias asociadas a la jerarquía, esta se elimina.

    [![Botón "Delete"](media/v2-update-how-to-tsm/portal-twelve.png)](media/v2-update-how-to-tsm/portal-twelve.png#lightbox) (Eliminar)

## <a name="instances"></a>Instancias

### <a name="create-a-single-instance"></a>Crear una sola instancia

1. Vaya al panel de selección del modelos de serie temporal y seleccione **Instances** (Instancias) en el menú. Contraiga el panel para centrarse en las instancias del modelo de serie temporal.

    [![Panel "Instances"](media/v2-update-how-to-tsm/portal-thirteen.png)](media/v2-update-how-to-tsm/portal-thirteen.png#lightbox) (Instancias)

1. Seleccione **Agregar**.

    [![Selecciones para agregar una instancia](media/v2-update-how-to-tsm/portal-fourteen.png)](media/v2-update-how-to-tsm/portal-fourteen.png#lightbox)

1. Escriba los detalles de la instancia, seleccione la asociación de tipo y de jerarquía y seleccione **Create** (Crear).

### <a name="bulk-upload-one-or-more-instances"></a>Carga en bloque de una o más instancias

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la instancia.

    [![Selecciones para la carga masiva de una o varias instancias](media/v2-update-how-to-tsm/portal-fifteen.png)](media/v2-update-how-to-tsm/portal-fifteen.png#lightbox)

1. Seleccione **Cargar**.

### <a name="edit-a-single-instance"></a>Editar una sola instancia

1. Seleccione la instancia y seleccione **Edit** (Editar). 
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Selecciones para la edición de una sola instancia](media/v2-update-how-to-tsm/portal-sixteen.png)](media/v2-update-how-to-tsm/portal-sixteen.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre el modelo de serie temporal, consulte [Modelado de datos](./time-series-insights-update-tsm.md).

- Para obtener más información acerca de la versión preliminar, lea [Visualize data in the Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Visualizar datos en el explorador de la versión preliminar de Azure Time Series Insights).

- Para obtener información sobre las formas JSON admitidas, consulte [Formas JSON admitidas](./time-series-insights-send-events.md#supported-json-shapes).
