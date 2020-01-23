---
title: Administración y actualización de Azure HPC Cache
description: Administración y actualización de Azure HPC Cache mediante Azure Portal
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 1/08/2020
ms.author: rohogue
ms.openlocfilehash: a166a904b2e63419efd5803fd54be1d1b59836fb
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75867079"
---
# <a name="manage-your-cache-from-the-azure-portal"></a>Administración de la memoria caché desde Azure Portal

En la página de información general de caché de Azure Portal se muestran los detalles del proyecto, el estado de la memoria caché y las estadísticas básicas de dicha memoria. También tiene controles para eliminar la memoria caché, vaciar los datos para el almacenamiento a largo plazo o actualizar el software.

Para abrir la página de información general, seleccione el recurso de caché en Azure Portal. Por ejemplo, cargue la página **Todos los recursos** y haga clic en el nombre de la memoria caché.

![Captura de pantalla de la página Información general de una instancia de Azure HPC Cache](media/hpc-cache-overview.png)

Los botones que se encuentran en la parte superior de la página pueden ayudarlo a administrar la memoria caché:

* [**Vaciar**](#flush-cached-data): escribe los datos modificados en destinos de almacenamiento
* [**Actualizar**](#upgrade-cache-software): actualiza el software de caché
* **Refrescar**: vuelve a cargar la página de información general
* [**Eliminar**](#delete-the-cache): destruye permanentemente la memoria caché

Lea más información sobre estas opciones a continuación.

## <a name="flush-cached-data"></a>Vaciado de los datos en caché

El botón **Vaciar** de la página de información general indica a la memoria caché que escriba inmediatamente todos los datos modificados que están almacenados en la memoria caché en los destinos de almacenamiento de back-end. La memoria caché guarda regularmente los datos en los destinos de almacenamiento, por lo que no es necesario hacerlo manualmente a menos que desee asegurarse de que el sistema de almacenamiento de back-end esté actualizado. Por ejemplo, puede usar **Vaciar** antes de tomar una instantánea de almacenamiento o comprobar el tamaño del conjunto de datos.

> [!NOTE]
> Durante el proceso de vaciado, la memoria caché no puede atender las solicitudes del cliente. El acceso a la memoria caché se suspende y se reanuda una vez finalizada la operación.

![Captura de pantalla de los botones superiores con el botón Vaciar resaltado, un mensaje emergente que describe la acción de vaciado y pregunta si desea continuar, y los botones Sí (valor predeterminado) y No.](media/hpc-cache-flush.png)

Cuando se inicia la operación de vaciado de la memoria caché, esta deja de aceptar solicitudes de cliente y su estado de en la página de información general cambia a **Vaciado**.

Los datos de la memoria caché se guardan en los destinos de almacenamiento adecuados. En función de la cantidad de datos que se deban vaciar, el proceso puede tardar unos minutos o más de una hora.

Una vez que todos los datos se guardan en destinos de almacenamiento, la memoria caché empieza de nuevo a tomar las solicitudes de cliente. El estado de la memoria caché vuelve a **Correcto**.

## <a name="upgrade-cache-software"></a>Actualización del software de caché

Si hay disponible una nueva versión de software, se activa el botón **Actualizar**. También puede ver un mensaje en la parte superior de la página sobre la actualización del software.

![Captura de pantalla de la fila superior de botones con el botón Actualizar habilitado](media/hpc-cache-upgrade-button.png)

El acceso de cliente no se interrumpe durante una actualización de software, pero el rendimiento de la memoria caché se ralentiza. Planee la actualización de software durante horas de menor uso o en un período de mantenimiento planeado.

La actualización de software puede tardar varias horas. Las memorias caché configuradas con un mayor rendimiento tardan más tiempo en actualizarse que las que tienen valores de rendimiento de pico más pequeños.

Cuando haya una actualización de software disponible, tendrá una semana aproximadamente para aplicarla manualmente. La fecha de finalización aparece en el mensaje de actualización. Si no actualiza durante ese tiempo, Azure aplica automáticamente la actualización a la memoria caché. El momento de la actualización automática no es configurable. Si le preocupa el impacto en el rendimiento de la memoria caché, debe actualizar el software usted mismo antes de que expire el período de tiempo.

Haga clic en el botón **Actualizar** para comenzar la actualización de software. El estado de la memoria caché cambia a **Actualizando** hasta que se complete la operación.

## <a name="delete-the-cache"></a>Eliminación de la caché

El botón **Eliminar** destruye la memoria caché. Cuando se elimina una memoria caché, todos sus recursos se destruyen y ya no incurren en cargos de cuenta.

Los volúmenes de almacenamiento de back-end que se usan como destinos de almacenamiento no se ven afectados cuando se elimina la caché. Puede agregarlos a una memoria caché futura más adelante, o bien decomisarlos por separado.

> [!NOTE]
> Azure HPC Cache no escribe automáticamente los datos modificados de la caché en los sistemas de almacenamiento de back-end antes de eliminar la caché.
>
> Para asegurarse de que todos los datos en la caché se han escrito en un almacenamiento a largo plazo, siga estos pasos:
>
> 1. [Quite](hpc-cache-edit-storage.md#remove-a-storage-target) todos los destinos de almacenamiento de Azure HPC Cache mediante el botón Eliminar de la página de destinos de almacenamiento. El sistema escribe automáticamente los datos modificados desde la caché en el sistema de almacenamiento de back-end antes de quitar el destino.
> 1. Espere a que se quite por completo el destino de almacenamiento. El proceso puede tardar una hora o más si hay muchos datos para escribir en la caché. Cuando haya terminado, una notificación del portal indicará que la operación de eliminación se ha realizado correctamente y el destino de almacenamiento desaparecerá de la lista.
> 1. Una vez eliminados todos los destinos de almacenamiento afectados, la caché se puede eliminar de forma segura.
>
> Como alternativa, puede usar la opción [flush](#flush-cached-data) para guardar los datos en caché, pero hay un pequeño riesgo de perder el trabajo si un cliente escribe un cambio en la caché una vez completado el vaciado, pero antes de que se destruya la instancia de la caché.

## <a name="cache-metrics-and-monitoring"></a>Supervisión y métricas de caché

La página de información general muestra gráficos para algunas estadísticas básicas de caché: rendimiento de caché, operaciones por segundo y latencia.

![Captura de pantalla de tres gráficos de líneas que muestran las estadísticas mencionadas anteriormente para una memoria caché de ejemplo](media/hpc-cache-overview-stats.png)

Estos gráficos forman parte de las herramientas de supervisión y análisis integradas de Azure. Existen otras herramientas y alertas disponibles en las páginas del encabezado **Supervisión** de la barra lateral del portal. Obtenga más información en la sección del portal de la [documentación de Supervisión de Azure](../azure-monitor/insights/monitor-azure-resource.md#monitoring-in-the-azure-portal).

## <a name="next-steps"></a>Pasos siguientes

<!-- * Learn more about metrics and statistics for hpc cache -->
* Más información sobre las [herramientas de métricas y estadísticas de Azure](../azure-monitor/index.yml)
* Obtener [ayuda con la instancia de Azure HPC Cache](hpc-cache-support-ticket.md)
