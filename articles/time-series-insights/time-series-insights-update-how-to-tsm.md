---
title: Modelado de datos en la versión preliminar de Azure Time Series Insights | Microsoft Docs
description: Obtenga más información acerca del modelado de datos de la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 09/04/2019
ms.custom: seodec18
ms.openlocfilehash: 64f892f344780d4af58c70935b9b7b68bad9550d
ms.sourcegitcommit: f272ba8ecdbc126d22a596863d49e55bc7b22d37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72273746"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Modelado de datos en la versión preliminar de Azure Time Series Insights

En este documento se describe cómo trabajar con los modelos de serie temporal después de usar la versión preliminar de Azure Time Series Insights. Aquí se detallan varios escenarios comunes de datos.

Para obtener más información acerca de cómo usar la actualización, lea [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Explorador de versión preliminar de Azure Time Series Insights).

## <a name="types"></a>Tipos

### <a name="create-a-single-type"></a>Crear un solo tipo

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Types** (Tipos) en el menú. Contraiga el panel para centrarse en los tipos de modelos de serie temporal.

    [![Crear un solo tipo](media/v2-update-how-to-tsm/portal-one.png)](media/v2-update-how-to-tsm/portal-one.png#lightbox)

1. Seleccione **+Agregar**.
1. Escriba todos los detalles referentes a los tipos y seleccione **Create** (Crear). Esta acción crea tipos en el entorno.

    [![Agregar un tipo](media/v2-update-how-to-tsm/portal-two.png)](media/v2-update-how-to-tsm/portal-two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Carga en bloque de uno o más tipos

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene el tipo de carga útil.
1. Seleccione **Cargar**.

    [![Cargar JSON](media/v2-update-how-to-tsm/portal-three.png)](media/v2-update-how-to-tsm/portal-three.png#lightbox)

### <a name="edit-a-single-type"></a>Editar un solo tipo

1. Seleccione el tipo y seleccione **Edit** (Editar). 
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Editar un tipo](media/v2-update-how-to-tsm/portal-four.png)](media/v2-update-how-to-tsm/portal-four.png#lightbox)

### <a name="delete-a-type"></a>Eliminar un tipo

1. Seleccione el tipo y **Delete** (Eliminar).
1. Si no hay instancias asociadas a los tipos, este se elimina.

    [![Eliminar un tipo](media/v2-update-how-to-tsm/portal-five.png)](media/v2-update-how-to-tsm/portal-five.png#lightbox)

## <a name="hierarchies"></a>Jerarquías

### <a name="create-a-single-hierarchy"></a>Crear una sola jerarquía

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Hierarchies** (Jerarquías) en el menú. Contraiga el panel para centrarse en las jerarquías de modelos de serie temporal.

    [![Seleccionar jerarquías](media/v2-update-how-to-tsm/portal-six.png)](media/v2-update-how-to-tsm/portal-six.png#lightbox)

1. Seleccione **+Agregar**.

    [![Agregar una jerarquía](media/v2-update-how-to-tsm/portal-seven.png)](media/v2-update-how-to-tsm/portal-seven.png#lightbox)

1. Seleccione **+Add Level** (Agregar nivel) en el panel derecho.

    [![Agregar un nivel](media/v2-update-how-to-tsm/portal-eight.png)](media/v2-update-how-to-tsm/portal-eight.png#lightbox)

1. Escriba los detalles de la jerarquía y seleccione **Create** (Crear).

    [![Crear un nivel](media/v2-update-how-to-tsm/portal-nine.png)](media/v2-update-how-to-tsm/portal-nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Carga en bloque de una o más jerarquías

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la jerarquía.
1. Seleccione **Cargar**.

    [![Carga masiva de jerarquías](media/v2-update-how-to-tsm/portal-ten.png)](media/v2-update-how-to-tsm/portal-ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Editar una sola jerarquía

1. Seleccione la jerarquía y seleccione **Edit** (Editar).
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Editar una sola jerarquía](media/v2-update-how-to-tsm/portal-eleven.png)](media/v2-update-how-to-tsm/portal-eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Eliminar una jerarquía

1. Seleccione la jerarquía y seleccione **Delete** (Eliminar). 
1. Si no hay instancias asociadas a la jerarquía, esta se elimina.

    [![Eliminar una jerarquía](media/v2-update-how-to-tsm/portal-twelve.png)](media/v2-update-how-to-tsm/portal-twelve.png#lightbox)

## <a name="instances"></a>Instancias

### <a name="create-a-single-instance"></a>Crear una sola instancia

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Instances** (Instancias) en el menú. Contraiga el panel para centrarse en las instancias de modelos de serie temporal.

    [![Crear una sola instancia](media/v2-update-how-to-tsm/portal-thirteen.png)](media/v2-update-how-to-tsm/portal-thirteen.png#lightbox)

1. Seleccione **Agregar**.

    [![Agregar una instancia](media/v2-update-how-to-tsm/portal-fourteen.png)](media/v2-update-how-to-tsm/portal-fourteen.png#lightbox)

1. Escriba los detalles de la instancia, seleccione la asociación de tipo y de jerarquía y seleccione **Create** (Crear).

### <a name="bulk-upload-one-or-more-instances"></a>Carga en bloque de una o más instancias

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la instancia.

    [![Carga masiva de una o más instancias](media/v2-update-how-to-tsm/portal-fifteen.png)](media/v2-update-how-to-tsm/portal-fifteen.png#lightbox)

1. Seleccione **Cargar**.

### <a name="edit-a-single-instance"></a>Editar una sola instancia

1. Seleccione la instancia y seleccione **Edit** (Editar). 
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Editar una sola instancia](media/v2-update-how-to-tsm/portal-sixteen.png)](media/v2-update-how-to-tsm/portal-sixteen.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de los modelos de serie temporal, lea [Data modeling](./time-series-insights-update-tsm.md) (Modelado de datos).

- Para obtener más información acerca de la versión preliminar, lea [Visualize data in the Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Visualizar datos en el explorador de la versión preliminar de Azure Time Series Insights).

- Para obtener información sobre las formas JSON admitidas, consulte [Supported JSON shapes](./time-series-insights-send-events.md#supported-json-shapes) (Formas JSON admitidas).
