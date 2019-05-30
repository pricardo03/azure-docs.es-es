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
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 73384868deb8f0e33b233e363c42a12adbcbe402
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237563"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Modelado de datos en la versión preliminar de Azure Time Series Insights

En este documento se describe cómo trabajar con los modelos de serie temporal después de usar la versión preliminar de Azure Time Series Insights. Aquí se detallan varios escenarios comunes de datos.

Para obtener más información acerca de cómo usar la actualización, lea [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Explorador de versión preliminar de Azure Time Series Insights).

## <a name="types"></a>Tipos

### <a name="create-a-single-type"></a>Crear un solo tipo

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Types** (Tipos) en el menú. Contraiga el panel para centrarse en los tipos de modelos de serie temporal.

    [![Crear un tipo único](media/v2-update-how-to-tsm/portal_one.png)](media/v2-update-how-to-tsm/portal_one.png#lightbox)

1. Seleccione **Agregar**.
1. Escriba todos los detalles referentes a los tipos y seleccione **Create** (Crear). Esta acción crea tipos en el entorno.

    [![Agregar un tipo](media/v2-update-how-to-tsm/portal_two.png)](media/v2-update-how-to-tsm/portal_two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Carga en bloque de uno o más tipos

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene el tipo de carga útil.
1. Seleccione **Cargar**.

    [![Carga de JSON](media/v2-update-how-to-tsm/portal_three.png)](media/v2-update-how-to-tsm/portal_three.png#lightbox)

### <a name="edit-a-single-type"></a>Editar un solo tipo

1. Seleccione el tipo y seleccione **Edit** (Editar). 
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Editar un tipo](media/v2-update-how-to-tsm/portal_four.png)](media/v2-update-how-to-tsm/portal_four.png#lightbox)

### <a name="delete-a-type"></a>Eliminar un tipo

1. Seleccione el tipo y **Delete** (Eliminar).
1. Si no hay instancias asociadas a los tipos, este se elimina.

    [![Eliminar un tipo](media/v2-update-how-to-tsm/portal_five.png)](media/v2-update-how-to-tsm/portal_five.png#lightbox)

## <a name="hierarchies"></a>Jerarquías

### <a name="create-a-single-hierarchy"></a>Crear una sola jerarquía

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Hierarchies** (Jerarquías) en el menú. Contraiga el panel para centrarse en las jerarquías de modelos de serie temporal.

    [![Seleccionar las jerarquías](media/v2-update-how-to-tsm/portal_six.png)](media/v2-update-how-to-tsm/portal_six.png#lightbox)

1. Seleccione **Agregar**.

    [![Agregar una jerarquía](media/v2-update-how-to-tsm/portal_seven.png)](media/v2-update-how-to-tsm/portal_seven.png#lightbox)

1. Seleccione **Add Level** (Agregar nivel) en el panel derecho.

    [![Agregar un nivel](media/v2-update-how-to-tsm/portal_eight.png)](media/v2-update-how-to-tsm/portal_eight.png#lightbox)

1. Escriba los detalles de la jerarquía y seleccione **Create** (Crear).

    [![Crear un nivel](media/v2-update-how-to-tsm/portal_nine.png)](media/v2-update-how-to-tsm/portal_nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Carga en bloque de una o más jerarquías

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la jerarquía.
1. Seleccione **Cargar**.

    [![Jerarquías de carga masiva](media/v2-update-how-to-tsm/portal_ten.png)](media/v2-update-how-to-tsm/portal_ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Editar una sola jerarquía

1. Seleccione la jerarquía y seleccione **Edit** (Editar).
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Editar una sola jerarquía](media/v2-update-how-to-tsm/portal_eleven.png)](media/v2-update-how-to-tsm/portal_eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Eliminar una jerarquía

1. Seleccione la jerarquía y seleccione **Delete** (Eliminar). 
1. Si no hay instancias asociadas a la jerarquía, esta se elimina.

    [![Eliminar una jerarquía](media/v2-update-how-to-tsm/portal_twelve.png)](media/v2-update-how-to-tsm/portal_twelve.png#lightbox)

## <a name="instances"></a>Instancias

### <a name="create-a-single-instance"></a>Crear una sola instancia

1. Vaya al panel de selección de los modelos de serie temporal y seleccione **Instances** (Instancias) en el menú. Contraiga el panel para centrarse en las instancias de modelos de serie temporal.

    [![Creación de una sola instancia](media/v2-update-how-to-tsm/portal_thirteen.png)](media/v2-update-how-to-tsm/portal_thirteen.png#lightbox)

1. Seleccione **Agregar**.

    [![Agregar una instancia](media/v2-update-how-to-tsm/portal_fourteen.png)](media/v2-update-how-to-tsm/portal_fourteen.png#lightbox)

1. Escriba los detalles de la instancia, seleccione la asociación de tipo y de jerarquía y seleccione **Create** (Crear).

### <a name="bulk-upload-one-or-more-instances"></a>Carga en bloque de una o más instancias

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la instancia.

    [![Una o varias instancias de carga masiva](media/v2-update-how-to-tsm/portal_fifteen.png)](media/v2-update-how-to-tsm/portal_fifteen.png#lightbox)

1. Seleccione **Cargar**.

### <a name="edit-a-single-instance"></a>Editar una sola instancia

1. Seleccione la instancia y seleccione **Edit** (Editar). 
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Editar una sola instancia](media/v2-update-how-to-tsm/portal_sixteen.png)](media/v2-update-how-to-tsm/portal_sixteen.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de los modelos de serie temporal, lea [Data modeling](./time-series-insights-update-tsm.md) (Modelado de datos).

- Para obtener más información acerca de la versión preliminar, lea [Visualize data in the Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Visualizar datos en el explorador de la versión preliminar de Azure Time Series Insights).

- Para obtener información sobre las formas JSON admitidas, consulte [Supported JSON shapes](./time-series-insights-send-events.md#json) (Formas JSON admitidas).
