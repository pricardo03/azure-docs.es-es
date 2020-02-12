---
title: Supervisión de las copias de seguridad con el Explorador de Backup
description: Artículo que describe cómo usar el Explorador de Backup para realizar la supervisión en tiempo real de las copias de seguridad en almacenes, suscripciones, regiones e inquilinos.
ms.reviewer: dcurwin
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: 331d8aeeb828dedb6700a94fafa074c179bef7ab
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76992007"
---
# <a name="monitor-your-backups-with-backup-explorer"></a>Supervisión de las copias de seguridad con el Explorador de Backup

Como las organizaciones cada vez hacen más copias de seguridad de las máquinas en la nube, es importante tener una ubicación central donde ver la información operativa sobre las copias de seguridad en una gran extensión y supervisar las copias de seguridad de forma eficaz.

El libro del Explorador de Backup es un libro de Azure Monitor integrado que permite a los clientes del servicio Backup tener un solo panel para realizar actividades de supervisión operativa relacionadas con la copia de seguridad en toda la extensión de copias de seguridad en Azure (la expansión de inquilinos, ubicaciones, suscripciones, grupos de recursos y almacenes), todo desde una ubicación central. En términos generales, ofrece las funcionalidades siguientes:

* **Perspectiva de escala**: una vista agregada de los elementos de copia de seguridad, los trabajos, las alertas, las directivas y los recursos no configurados para la copia de seguridad en toda la extensión de copia de seguridad. 
* **Análisis en profundidad**: puede elegir obtener información detallada de cada una de las entidades: los trabajos, las alertas, las directivas y los elementos de copia de seguridad, todo ello desde un único lugar.
* **Interfaces accionables**: una vez que se identifica un problema, puede elegir realizar acciones navegando de forma fluida hasta el elemento de copia de seguridad o el recurso de Azure.

Las funcionalidades anteriores se proporcionan de forma integrada a través de la integración nativa con Azure Resource Graph y los libros de Azure Monitor.

> [!NOTE]
> * El Explorador de Backup solo está disponible actualmente para los datos de máquinas virtuales de Azure.
> * El Explorador de Backup está pensado para servir de panel operativo donde ver información acerca de las copias de seguridad en los últimos 7 días (máximo).
> * Actualmente no se admite la personalización de la plantilla del Explorador de Backup. Además, actualmente no se recomienda escribir automatizaciones personalizadas en los datos de Azure Resource Graph.

## <a name="getting-started"></a>Introducción

Para obtener acceso al Explorador de Backup, vaya a cualquiera de los almacenes de Recovery Services y haga clic en el vínculo **Explorador de Backup** en la página de **información general**.

![Vínculo rápido al almacén](media/backup-azure-monitor-with-backup-explorer/vault-quick-link.png)

Al hacer clic en el vínculo, se abre el Explorador de Backup, que proporciona una vista agregada de todos los almacenes y suscripciones a los que tiene acceso. Si usa una cuenta de Azure Lighthouse, puede ver los datos de todos los inquilinos a los que tiene acceso (consulte más información en la sección siguiente sobre las vistas entre inquilinos).

![Página de aterrizaje del explorador](media/backup-azure-monitor-with-backup-explorer/explorer-landing-page.png)

## <a name="backup-explorer-use-cases"></a>Casos de uso del Explorador de Backup

El Explorador de Backup se compone de varias pestañas, cada una de las cuales proporciona información detallada sobre un tipo específico de artefacto de copia de seguridad; por ejemplo, elementos de copia de seguridad, trabajos, directivas, etc. En la sección siguiente se proporciona una breve descripción de cada pestaña. Los vídeos proporcionan casos de uso de ejemplo para cada una de las pestañas, junto con una descripción de los distintos controles disponibles para el usuario.

### <a name="summary"></a>Resumen

La pestaña Resumen permite obtener una vista rápida de la condición general del panorama de copia de seguridad. Puede ver información como el número de elementos que se protegen, el número de elementos para los que no se ha habilitado la protección, cuántos trabajos se realizaron correctamente en las últimas 24 horas, etc. 

Cada una de las demás pestañas representa una entidad distinta, por ejemplo: elementos de copia de seguridad, trabajos de copia de seguridad, alertas de copia de seguridad y directivas de copia de seguridad. También puede ver información acerca de los equipos para los que no se ha configurado el servicio Backup. Al hacer clic en cualquiera de estas pestañas, se mostrará información específica de esa entidad.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYd]

### <a name="backup-items"></a>Elementos de copia de seguridad

Puede ver cada uno de los elementos de copia de seguridad filtrados por suscripción, almacén y otros parámetros. Al hacer clic en el nombre de un elemento de copia de seguridad, puede abrir la hoja de Azure para ese elemento de copia de seguridad. Por ejemplo, en la tabla, es posible que observe que se produjo un error en la última copia de seguridad del elemento "X". Al hacer clic en "X", se abre la hoja de Backup de este elemento, donde puede desencadenar una operación de copia de seguridad a petición.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYc]

### <a name="jobs"></a>Trabajos

Puede ver los detalles de todos los trabajos que se han desencadenado durante los últimos siete días; para ello, vaya a la pestaña Trabajos. Aquí, puede filtrar por operación de trabajo, estado de trabajo y código de error (en el caso de trabajos con errores).

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nOrh]

### <a name="alerts"></a>Alertas

Puede ver los detalles de todas las alertas que se han desencadenado en los almacenes durante los últimos siete días; para ello, haga clic en la pestaña Alertas. Aquí, puede filtrar los registros por el tipo de la alerta (por ejemplo, error de copia de seguridad, error de restauración), el estado actual de la alerta (por ejemplo, activa o resuelta) y la gravedad de la alerta (por ejemplo, crítica, advertencia, información). También puede ir a la máquina virtual de Azure si hace clic en el vínculo a la máquina virtual y realiza la acción necesaria.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nTxe]

### <a name="policies"></a>Directivas

Al hacer clic en la pestaña Directivas, puede ver información clave sobre todas las directivas de copia de seguridad que se han creado en el conjunto de copias de seguridad. Puede ver el número de elementos asociados a cada directiva, junto con la duración de retención y la frecuencia de copia de seguridad especificadas por cada directiva.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nLKV]

### <a name="backup-not-enabled"></a>Copia de seguridad no habilitada

Es importante que el administrador de copias de seguridad de una organización identifique rápidamente qué máquinas de la organización no tienen aún habilitada la copia de seguridad, de modo que se pueda habilitar la copia de seguridad para todas las máquinas que necesiten protección. Al hacer clic en la pestaña **Copia de seguridad no habilitada** obtendrá ayuda con esta tarea.

En esta pestaña, verá una tabla que contiene la lista de elementos que no están protegidos. Si su organización sigue la práctica de asignar diferentes etiquetas a máquinas de producción y máquinas de prueba, o máquinas que atienden funciones diferentes, cada una de las cuales podría necesitar una directiva de copia de seguridad independiente, el filtrado por etiquetas le ayuda a ver información específica de un determinada clase de máquinas. Al hacer clic en el nombre de cualquier elemento, se le redirigirá a la hoja **Configurar la copia de seguridad** de ese elemento, donde puede elegir hacer una copia de seguridad de ese elemento con una directiva de copia de seguridad adecuada.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQXZ]

## <a name="exporting-to-excel"></a>Exportación a Excel

Al hacer clic en el botón de flecha abajo situado en la parte superior derecha de cualquier widget (tabla o gráfico), se exporta el contenido de ese widget como una hoja de Excel, tal cual con los filtros existentes aplicados. Para exportar más filas de una tabla a Excel, puede aumentar el número de filas que se muestran en la página mediante la lista desplegable **Filas por página** en la parte superior de cada pestaña.

## <a name="pinning-to-dashboard"></a>Anclar al panel

Puede hacer clic en el icono de anclaje en la parte superior de cada widget para anclar el widget a su panel de Azure Portal. Esto le ayudará a crear paneles personalizados adaptados para mostrar la información más importante que necesita.

## <a name="cross-tenant-views"></a>Vistas entre inquilinos

Si es usuario de Azure Lighthouse con acceso delegado a las suscripciones en varios entornos de inquilinos, puede usar el filtro de suscripción predeterminado (haciendo clic en el icono de filtro en la parte superior derecha de Azure Portal) para seleccionar todas las suscripciones para las que desee ver datos. Al hacerlo, permitirá al Explorador de Backup agregar información sobre todos los almacenes en estas suscripciones. Obtenga más información acerca de Azure Lighthouse [aquí](https://docs.microsoft.com/azure/lighthouse/overview).

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a usar Azure Monitor para obtener información sobre los datos de copia de seguridad.](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)