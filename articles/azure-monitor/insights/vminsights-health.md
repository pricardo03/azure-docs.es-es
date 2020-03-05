---
title: Descripción del mantenimiento de las máquinas virtuales de Azure | Microsoft Docs
description: En este artículo se describe cómo entender el mantenimiento de la máquina virtual y los sistemas operativos subyacentes con Azure Monitor para VM.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/14/2019
ms.openlocfilehash: 3cecb04a4f299051860c45425f0fc4e13c3722ea
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77656303"
---
# <a name="understand-the-health-of-your-azure-virtual-machines"></a>Descripción del mantenimiento de las máquinas virtuales de Azure

Azure incluye servicios para roles o tareas específicos en el espacio de supervisión, pero no proporciona las perspectivas de mantenimiento detalladas de los sistemas operativos (SO) hospedados en las máquinas virtuales (VM) de Azure. Aunque puede usar Azure Monitor para distintas condiciones, no se ha diseñado para modelar y representar el mantenimiento de los componentes principales ni el mantenimiento general de las máquinas virtuales.

Mediante el uso del mantenimiento de Azure Monitor para VM, puede supervisar activamente la disponibilidad y el rendimiento de un sistema operativo invitado Windows o Linux. La característica de mantenimiento utiliza un modelo que representa los componentes principales y sus relaciones, proporciona los criterios que especifican cómo medir el mantenimiento de los componentes y envía una alerta cuando detecta una condición incorrecta.

El estado de mantenimiento general de una máquina virtual de Azure y el sistema operativo subyacente pueden observarse desde dos perspectivas: directamente desde una máquina virtual o en todas las máquinas virtuales de un grupo de recursos desde Azure Monitor.

En este artículo se muestra cómo evaluar, investigar y resolver incidencias de mantenimiento rápidamente cuando se detectan con la característica de mantenimiento Azure Monitor para VM.

Para obtener más información sobre cómo configurar Azure Monitor para máquinas virtuales, consulte el artículo [Enable Azure Monitor for VMs](vminsights-enable-overview.md) (Habilitar Azure Monitor para máquinas virtuales).

>[!NOTE]
>Recientemente [anunciamos cambios](https://azure.microsoft.com/updates/updates-to-azure-monitor-for-virtual-machines-preview-before-general-availability-release/
) que estamos realizando en la característica de mantenimiento en función de los comentarios que hemos recibido de nuestros clientes de la versión preliminar pública. Dado el número de cambios que realizaremos, vamos a dejar de ofrecer esta característica a los nuevos clientes. Los clientes existentes pueden seguir usando la característica de mantenimiento. Para más información, consulte las [preguntas frecuentes sobre la disponibilidad general](vminsights-ga-release-faq.md). 

## <a name="monitoring-configuration-details"></a>Detalles de configuración de supervisión

En esta sección se describen los criterios de mantenimiento predeterminados para supervisar las máquinas virtuales Linux y Windows de Azure. Todos los criterios de mantenimiento están preconfigurados para enviar una alerta cuando se detecta una condición incorrecta.

| Nombre de monitor | Frecuencia (min) | Duración de retrospectiva (min) | Operator | Umbral | Alerta sobre el estado | severity | Categoría de carga de trabajo | 
|--------------|-----------|----------|----------|-----------|----------------|----------|-------------------|
| Disco lógico en línea | 5 | 15 | <> | 1 (true) | Crítico | Sev1 | Linux | 
| Espacio disponible en el disco lógico | 5 | 15 | < | 200 MB (advertencia)<br> 100 MB (crítico) | Advertencia | Sev1<br> Sev2 | Linux | 
| % de inode disponibles en el disco lógico | 5 | 15 | < | 5 % | Crítico | Sev1 | Linux | 
| % de espacio disponible en el disco lógico | 5 | 15 | < | 5 % | Crítico | Sev1 | Linux | 
| Estado del adaptador de red | 5 | 15 | <> | 1 (true) | Advertencia | Sev2 | Linux | 
| Memoria en megabytes disponibles del sistema operativo | 5 | 10 | < | 2,5 MB | Crítico | Sev1 | Linux | 
| Media del disco Segundos de disco/lecturas | 5 | 25 | > | 0,05 s | Crítico | Sev1 | Linux | 
| Media del disco Segundos de disco/transferencias | 5 | 25 | > | 0,05 s | Crítico | Sev1 | Linux | 
| Media del disco Segundos de disco/escrituras | 5 | 25 | > | 0,05 s | Crítico | Sev1 | Linux | 
| Estado del disco | 5 | 25 | <> | 1 (true) | Crítico | Sev1 | Linux | 
| Porcentaje total de tiempo del procesador total del sistema operativo | 5 | 10 | >= | 95 % | Crítico | Sev1 | Linux | 
| Porcentaje de uso total de la CPU | 5 | 10 | >= | 95 % | Crítico | Sev1 | Windows | 
| Error o daños en el sistema de archivos | 60 | 60 | <> | 4 | Crítico | Sev1 | Windows | 
| Media de segundos del disco lógico por lectura | 1 | 15 | > | 0,04 s | Advertencia | Sev2 | Windows | 
| Media de segundos del disco lógico por transferencia | 1 | 15 | > | 0,04 s | Advertencia | Sev2 | Windows | 
| Promedio de segundos del disco lógico por escritura (disco lógico) | 1 | 15 | > | 0,04 s | Advertencia | Sev2 | Windows | 
| Longitud actual de cola de disco (disco lógico) | 5 | 60 | >= | 32 | Advertencia | Sev2 | Windows | 
| Espacio disponible en el disco lógico (MB) | 15 | 60 | > | 500 MB advertencia<br> 300 MB crítico | Crítico | Sev1<br> Sev2 | Windows | 
| (%) de espacio disponible en el disco lógico | 15 | 60 | > | 10 % advertencia<br> 5 % crítico | Crítico | Sev1<br> Sev2 | Windows |
| Porcentaje de tiempo de inactividad del disco lógico | 15 | 360 | <= | 20% | Advertencia | Sev2 | Windows | 
| Porcentaje de ancho de banda usado para la lectura | 5 | 60 | >= | 60% | Advertencia | Sev2 | Windows | 
| Porcentaje total del ancho de banda usado | 5 | 60 | >= | 75 % | Advertencia | Sev2 | Windows | 
| Porcentaje de ancho de banda para la escritura | 5 | 60 | >= | 60% | Advertencia | Sev2 | Windows | 
| Service Health del cliente DHCP | 5 | 12 | <> | 4 (ejecución) | Crítico | Sev1 | Windows | 
| Service Health del cliente DNS | 5 | 12 | <> | 4 (ejecución) | Crítico | Sev1 | Windows | 
| Service Health del registro de eventos de Windows | 5 | 12 | <> | 4 (ejecución) | Crítico | Sev1 | Windows | 
| Service Health del firewall de Windows | 5 | 12 | <> | 4 (ejecución) | Crítico | Sev1 | Windows | 
| Service Health de RPC | 5 | 12 | <> | 4 (ejecución) | Crítico | Sev1 | Windows | 
| Service Health de servidor | 5 | 12 | <> | 4 (ejecución) | Crítico | Sev1 | Windows | 
| Service Health de Administración remota de Windows | 5 | 12 | <> | 4 (ejecución) | Crítico | Sev1 | Windows | 
| Megabytes disponibles de memoria | 5 | 10 | < | 100 MB | Crítico | Sev1 | Windows | 
| Entradas libres de la tabla de páginas del sistema | 5 | 10 | <= | 5000 | Crítico | Sev1 | Windows | 
| Páginas de memoria por segundo | 5 | 10 | >= | 5000/s | Advertencia | Sev1 | Windows | 
| Porcentaje de memoria asignada en uso | 5 | 10 | > | 80 % | Crítico | Sev1 | Windows | 
| Media de segundos del disco por transferencia | 1 | 15 | > | 0,04 s | Advertencia | Sev2 | Windows | 
| Promedio de segundos del disco por escritura | 1 | 15 | > | 0,04 s | Advertencia | Sev2 | Windows | 
| Current Disk Queue Length | 5 | 60 | >= | 32 | Advertencia | Sev2 | Windows | 
| Porcentaje de tiempo de inactividad del disco | 5 | 60 | >= | 20% | Advertencia | Sev2 | Windows | 

>[!NOTE]
>Duración de retrospectiva representa la frecuencia con la que la ventana de la retrospectiva comprueba los valores de métricas, por ejemplo en los últimos cinco minutos.  

>[!NOTE]
>Frecuencia representa la frecuencia con la que la alerta de métrica comprueba si se cumplen las condiciones, por ejemplo, cada minuto.  Es la velocidad a la que se ejecuta el criterio de mantenimiento y la retrospectiva es la duración en la que se evalúa el criterio de estado. Por ejemplo, el criterio de mantenimiento evalúa si la **utilización de la CPU** de la condición es superior al 95 por ciento con una frecuencia de 5 minutos y permanece superior al 95 % durante 15 minutos (3 ciclos de evaluación consecutivos), el estado se actualiza a una gravedad crítica si aún no lo estaba.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Para iniciar sesión, vaya a [Azure Portal](https://portal.azure.com).

## <a name="introduction-to-azure-monitor-for-vms-health"></a>Introducción al mantenimiento de Azure Monitor para VM

Antes de usar la característica de mantenimiento para una sola máquina virtual o un grupo de máquinas virtuales, es importante comprender cómo se presenta la información y qué representan las visualizaciones.

### <a name="view-health-directly-from-a-vm"></a>Visualización del mantenimiento directamente desde una máquina virtual

Para visualizar el mantenimiento de una máquina virtual de Azure, seleccione **Conclusiones (versión preliminar)** desde el panel izquierdo de la máquina virtual. En la página de conclusiones de la máquina virtual, la pestaña **Mantenimiento** se abre de forma predeterminada y se muestra la vista de mantenimiento de la máquina virtual.

![Información general del estado de Azure Monitor para máquinas virtuales de una máquina virtual de Azure seleccionada](./media/vminsights-health/vminsights-directvm-health-01.png)

En la sección **Estado de la VM invitada**, la tabla incluye una consolidación del estado de los componentes de rendimiento que supervisan los criterios de mantenimiento de la VM, así como el número total de alertas de estado generadas por componentes en mal estado. Estos componentes incluyen **CPU**, **Memoria**, **Disco** y **Red**. Expanda el botón de contenido adicional al lado de Estado de la VM invitada para ver el estado de sus componentes.

![Estado de componentes de Azure Monitor para VM de una máquina virtual de Azure seleccionada.](./media/vminsights-health/vminsights-directvm-health-02.png)

Si selecciona el estado al lado del componente, se abrirá la experiencia Diagnóstico de estado en el contexto del componente seleccionado. Se muestra la composición del estado de ese componente, en la que se describe qué criterios de mantenimiento se usan para calcular el estado. Para más información, consulte [Diagnóstico de mantenimiento y trabajo con los criterios de mantenimiento](#health-diagnostics). Para obtener más información acerca de las alertas, vea [Alertas](#alerts).

En la tabla siguiente se describen los estados de mantenimiento definidos para una VM:

|Icono |Estado de mantenimiento |Significado |
|-----|-------------|---------------|
| |Healthy |La máquina virtual está dentro de las condiciones de mantenimiento definidas. Este estado indica que no se ha detectado ninguna incidencia y la máquina virtual funciona con normalidad. Con un monitor de acumulación primario, el mantenimiento se acumula y refleja el mejor o el peor estado del elemento secundario.|
| |Crítico |El estado no está dentro de las condiciones de mantenimiento definidas, lo que indica que se han detectado una o más incidencias críticas. Se deben solucionar estas incidencias para restaurar la funcionalidad normal. Con un monitor de acumulación primario, el estado de mantenimiento se acumula y refleja el mejor o el peor estado del elemento secundario.|
| |Advertencia |El estado está entre dos umbrales de la condición de mantenimiento definida; uno de ellos indica un estado de advertencia y el otro indica un estado crítico (se pueden configurar tres umbrales de estado de mantenimiento) o cuando una incidencia no crítica puede provocar problemas críticos si no se resuelve. Con un monitor de acumulación primario, si uno o varios de los elementos secundarios están en un estado de advertencia, el elemento primario reflejará un estado de advertencia. Si hay un elemento secundario que se encuentra en estado crítico y otro en estado de advertencia, la acumulación primaria mostrará el estado de mantenimiento como crítico.|
| |Desconocido |No se puede calcular el estado por varias razones. La siguiente sección proporciona detalles adicionales y posibles soluciones. |

Un estado de mantenimiento Desconocido puede deberse a las siguientes incidencias:

- Se ha vuelto a configurar el agente y ya no informa al área de trabajo especificada cuando se habilitó Azure Monitor para VM. Para configurar el agente para que informe al área de trabajo, consulte [Adición o eliminación de un área de trabajo](../platform/agent-manage.md#adding-or-removing-a-workspace).
- Se ha eliminado la máquina virtual.
- Se ha eliminado el área de trabajo asociada a Azure Monitor para VM. Puede recuperar el área de trabajo si tiene ventajas de soporte técnico Premier. Vaya a [Premier](https://premier.microsoft.com/) y abra una solicitud de soporte técnico.
- Se han eliminado las dependencias de la solución. Para volver a habilitar las soluciones Service Map e Infrastructure Insights en el área de trabajo de Log Analytics, vuelva a instalar Service Map con la [plantilla de Azure Resource Manager](vminsights-enable-at-scale-powershell.md#install-the-servicemap-solution). Para volver a instalar la solución Infrastructure Insights, envíe un correo electrónico a vminsights@microsoft.com. 
- Se ha apagado la máquina virtual.
- El servicio de máquina virtual de Azure no está disponible o se están realizando operaciones de mantenimiento.
- Se alcanzó el [límite de datos diarios o de retención](../platform/manage-cost-storage.md) del área de trabajo.

Seleccione **Ver el diagnóstico de mantenimiento** para abrir una página que muestra todos los componentes de una máquina virtual, los criterios de mantenimiento asociados, los cambios de estado y otras incidencias detectadas en la supervisión de los componentes relacionados con la máquina virtual.

Para más información, consulte [Diagnóstico de mantenimiento](#health-diagnostics).

En la sección **Mantenimiento**, una tabla muestra la acumulación del mantenimiento de los componentes de rendimiento supervisados por los criterios de mantenimiento. Estos componentes incluyen **CPU**, **Memoria**, **Disco** y **Red**. Al seleccionar un componente se abre una página que enumera todos los criterios de supervisión y el estado de mantenimiento de dicho componente.

Cuando se accede al mantenimiento de una máquina virtual de Azure que ejecuta Windows, se muestra el estado de mantenimiento de los cinco principales servicios básicos de Windows en **Mantenimiento de servicios básicos**. Al seleccionar cualquiera de los servicios se abre una página que enumera los criterios de mantenimiento que supervisan dicho componente y su estado de mantenimiento.

Al seleccionar el nombre de los criterios de mantenimiento se abre el panel de propiedades. En este panel, puede revisar los detalles de configuración, incluso si los criterios de mantenimiento tienen una alerta de Azure Monitor correspondiente.

Para más información, consulte [Diagnóstico de mantenimiento y trabajo con los criterios de mantenimiento](#health-diagnostics).

### <a name="aggregate-vm-perspective"></a>Perspectiva de agregación de máquinas virtuales

Para ver la colección de mantenimiento de todas las máquinas virtuales de un grupo de recursos, seleccione **Azure Monitor** en la lista de navegación del portal y, a continuación, seleccione **Máquinas virtuales (versión preliminar)** .

![Vista de supervisión de conclusiones de la VM de Azure Monitor](./media/vminsights-health/vminsights-aggregate-health.png)

En las listas desplegables **Suscripción** y **Grupo de recursos**, seleccione el grupo de recursos adecuado que incluya las máquinas virtuales relacionadas con el grupo para ver el estado de mantenimiento notificado. La selección solo se aplica a la característica de mantenimiento y no se aplica a las pestañas **Rendimiento** ni **Asignación**.

Las pestaña **Mantenimiento** proporciona la siguiente información:

* Cuántas máquinas virtuales se encuentran en un estado crítico o incorrecto, frente a cuántas están en buen estado o no envían datos (es decir, tienen un estado Desconocido).
* Qué máquinas virtuales, organizadas según el sistema operativo, notifican un estado incorrecto y cuántas.
* Cuántas máquinas virtuales tienen un estado incorrecto debido a una incidencia detectada con un procesador, un disco, la memoria o un adaptador de red, clasificadas según el estado de mantenimiento.
* Cuántas máquinas virtuales tienen un estado incorrecto debido a una incidencia detectada con un servicio básico del sistema operativo, clasificadas según el estado de mantenimiento.

En la pestaña **Mantenimiento**, puede identificar las incidencias críticas detectadas por los criterios de mantenimiento que supervisan la máquina virtual y puede revisar los detalles de las alertas y los artículos de conocimientos asociados. Estos artículos pueden ayudar en el diagnóstico y la solución de las incidencias. Seleccione cualquiera de las gravedades para abrir la página [Todas las alertas](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) filtrada según esa gravedad.

En la lista **VM distribution by operating system** (Distribución de la máquina virtual por sistema operativo) se muestran las VM que enumera la edición de Windows o la distribución de Linux, junto con su versión. En cada categoría de sistema operativo, las máquinas virtuales se desglosan aún más según el mantenimiento de la máquina virtual.

![Perspectiva de la distribución de la máquina virtual de conclusiones de la VM](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

Seleccione cualquier columna, incluidas **Número de máquinas virtuales**, **Crítico**, **Advertencia**, **Correcto** o **Desconocido**. Puede ver la lista de resultados filtrados en la página **Máquinas virtuales** que coinciden con la columna seleccionada.

Por ejemplo, para revisar todas las máquinas virtuales que ejecutan Red Hat Enterprise Linux versión 7.5, seleccione el valor **Número de máquinas virtuales** de ese sistema operativo y se mostrará una lista de las máquinas virtuales que coinciden con ese filtro y su estado de mantenimiento actual.

![Ejemplo de la acumulación de VM Red Hat Linux](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)

Haga clic en la casilla **Mostrar estado** y se devolverá el estado de mantenimiento de los resultados filtrados de la tabla.  

![Ejemplo de estado de mantenimiento de VM Red Hat Linux.](./media/vminsights-health/vminsights-rollup-vm-rehl-02.png)

Puede hacer clic en el estado de mantenimiento de cualquiera de los elementos de la lista para iniciar Diagnóstico de estado, donde se mostrará el método de evaluación del estado de la VM seleccionada. 

En la página **Máquinas virtuales**, si selecciona el nombre de una máquina virtual en la columna **Nombre de máquina virtual**, se le dirige a la página **Instancia de máquina virtual**. Esta página proporciona más detalles acerca de las alertas y las incidencias de los criterios de mantenimiento que afectan a la máquina virtual seleccionada. Para filtrar los detalles del estado de mantenimiento, seleccione el icono **Estado de mantenimiento** en la esquina superior izquierda de la página para ver qué componentes están en estado incorrecto. También puede ver las alertas de mantenimiento de la máquina virtual generadas por un componente incorrecto clasificadas por gravedad de la alerta.

Desde la vista **Lista de máquinas virtuales**, seleccione el nombre de una máquina virtual para abrir la página **Mantenimiento** de esa máquina virtual, de forma similar a cuando selecciona **Conclusiones (versión preliminar)** desde la máquina virtual directamente.

![Conclusiones de una máquina virtual de Azure seleccionada](./media/vminsights-health/vminsights-directvm-health.png)

En la página **Virtual Machines (versión preliminar) en Azure Monitor** se muestra el estado de mantenimiento consolidado para las VM y las alertas. Este estado de mantenimiento se clasifica por gravedad, lo que representa las alertas de mantenimiento de la máquina virtual generadas cuando cambia el estado de mantenimiento de correcto a incorrecto en función de los criterios. Al seleccionar **Máquinas virtuales en una condición crítica** se abre una página con una lista de una o más máquinas virtuales en un estado de mantenimiento crítico.

Al seleccionar el estado de mantenimiento de una de las máquinas virtuales de la lista, se mostrará la vista **Diagnóstico de mantenimiento** de la máquina virtual. En esta vista puede determinar qué criterios de mantenimiento reflejan una incidencia de estado de mantenimiento. Cuando se abre la página **Diagnóstico de mantenimiento**, se muestran todos los componentes de la máquina virtual y los criterios de mantenimiento asociados con el estado de mantenimiento actual.

Para más información, consulte [Diagnóstico de mantenimiento](#health-diagnostics).

Al seleccionar **Ver todos los criterios de estado**, se abre una página que muestra una lista de todos los criterios de mantenimiento disponibles con esta característica. La información se puede filtrar aún más en función de las siguientes opciones:

* **Tipo**. Existen tres tipos de criterios de mantenimiento para evaluar las condiciones y acumular en el estado de mantenimiento general de una máquina virtual supervisada:
    - **Unidad**. Mide algún aspecto de una máquina virtual. Este tipo de criterios de mantenimiento podría comprobar un contador de rendimiento para determinar el rendimiento del componente mediante la ejecución de un script para realizar una transacción sintética o controlar un evento que indica un error. De forma predeterminada, el filtro se establece en Unidad.
    - **Dependencia**. Proporciona una acumulación del estado de mantenimiento entre las distintas entidades. Este criterio de mantenimiento permite que el mantenimiento de una entidad dependa del mantenimiento de otro tipo de entidad en la que se basa para que la operación se realice correctamente.
    - **Agregado**. Proporciona un estado de mantenimiento combinado de los criterios de mantenimiento similares. Los criterios de mantenimiento Unidad y Dependencia normalmente se configuran a partir de un criterio de mantenimiento agregado. Además de proporcionar una mejor organización general de los distintos criterios de mantenimiento de una entidad, los criterios de estado agregados proporcionan un estado único de mantenimiento para las distintas categorías de las entidades.

* **Categoría**. Este tipo de criterios de mantenimiento se usa para agrupar criterios similares con fines informativos. Estas categorías son **Disponibilidad** y **Rendimiento**.

Para ver las instancias que tienen un estado incorrecto, seleccione un valor de la columna **Componente con un estado incorrecto**. En la página, una tabla enumera los componentes que se encuentran en un estado de mantenimiento crítico.

## <a name="health-diagnostics"></a>Diagnóstico de mantenimiento

La página **Diagnóstico de mantenimiento** permite visualizar el modelo de mantenimiento de una máquina virtual. Esta página enumera todos los componentes de la máquina virtual, los criterios de mantenimiento asociados, los cambios de estado y otras incidencias importantes identificadas por los componentes supervisados relacionados con la máquina virtual.

![Ejemplo de la página Diagnóstico de mantenimiento para una VM](./media/vminsights-health/health-diagnostics-page-01.png)

Puede iniciar el diagnóstico de mantenimiento mediante los métodos siguientes:

* Mediante el estado de mantenimiento acumulativo de todas las máquinas virtuales desde la perspectiva de máquina virtual agregada en Azure Monitor:

    1. En la página **Mantenimiento**, seleccione el icono del estado de mantenimiento **Crítico**, **Advertencia**, **Correcto** o **Desconocido** en la sección **Mantenimiento de la máquina virtual invitada**.
    2. Vaya a la página que enumera todas las máquinas virtuales que coinciden con esa categoría filtrada.
    3. Seleccione el valor de la columna **Estado de mantenimiento**, para abrir el diagnóstico de mantenimiento con ámbito en esa máquina virtual.

* Por sistema operativo desde la perspectiva de máquina virtual agregada en Azure Monitor. En **VM distribution** (Distribución de VM), al seleccionar cualquiera de los valores de la columna, se abrirá la página **Máquinas virtuales** y se devolverá una lista en la tabla que coincida con la categoría filtrada. Al seleccionar el valor de la columna **Estado de mantenimiento**, se abre el diagnóstico de mantenimiento para la máquina virtual seleccionada.
 
* Desde la máquina virtual invitada de la pestaña **Mantenimiento** de Azure Monitor para VM, seleccione **Ver el diagnóstico de mantenimiento**.

El diagnóstico de mantenimiento organiza la información de mantenimiento en dos categorías: disponibilidad y rendimiento.
 
Todos los criterios de mantenimiento definidos para un componente, como el disco lógico o la CPU, se pueden ver sin filtrar en las dos categorías. Estas vistas pueden estar en una vista total de criterios o mediante el filtrado de los resultados por cualquier categoría cuando se selecciona **Disponibilidad** o **Rendimiento**.

Además, se puede ver la categoría de los criterios junto a la columna **Criterios de mantenimiento**. Si los criterios no coinciden con la categoría seleccionada, aparece un mensaje que indica **Ningún criterio de mantenimiento disponible para la categoría seleccionada** en la columna **Criterios de mantenimiento**.

El estado de un criterio de mantenimiento se define mediante uno de los cuatro tipos: **Crítico**, **Advertencia**, **Correcto** y **Desconocido**. Los tres primeros son configurables, lo que significa que puede modificar los valores de umbral de los monitores directamente en el panel de configuración **Criterios de mantenimiento**. Esto también es posible mediante la [operación update monitor](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update) de la API REST de Azure Monitor. **Desconocido** no es configurable y se reserva para escenarios específicos.

La página **Diagnóstico de mantenimiento** tiene tres secciones principales:

* Modelo de componentes
* Criterios del mantenimiento
* Cambios de estado

![Secciones de la página Diagnóstico de mantenimiento](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Modelo de componente

La columna del extremo izquierdo de la página **Diagnóstico de mantenimiento** es el **Modelo de componentes**. Todos los componentes asociados a la máquina virtual se muestran en esta columna junto con su estado de mantenimiento actual.

En el ejemplo siguiente, los componentes detectados son **Disco**, **Disco lógico**, **Procesador**, **Memoria** y **Sistema operativo**. Varias instancias de estos componentes se detectan y se muestran en esta columna.

Por ejemplo, en la imagen siguiente se muestra que la máquina virtual tiene dos instancias de discos lógicos, **C:** y **D:** , que se encuentran en un estado correcto:

![Ejemplo del modelo de componente presentado en el diagnóstico de mantenimiento](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Criterios de mantenimiento

La columna central de la página Diagnóstico de mantenimiento es **Criterios de mantenimiento**. El modelo de estado definido para la VM se muestra en un árbol jerárquico. El modelo de mantenimiento de una máquina virtual está compuesto por los criterios de mantenimiento agregados y de unidad.

![Ejemplo de los criterios de mantenimiento presentados en la página Diagnóstico de mantenimiento](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

Un criterio de mantenimiento mide el mantenimiento de una instancia supervisada, lo que podría ser un valor de umbral, el estado de una entidad, etc. Un criterio de mantenimiento tiene dos o tres umbrales configurables de estado de mantenimiento, tal y como se describió anteriormente. En cualquier momento dado, el criterio de mantenimiento solo puede estar en uno de sus estados posibles.

El modelo de mantenimiento define los criterios que determinan el mantenimiento del destino general y los componentes del destino. Esta jerarquía de criterios se muestra en la sección **Criterios de mantenimiento** de la página **Diagnóstico de mantenimiento**.

La directiva de acumulación de mantenimiento es parte de la configuración de los criterios de mantenimiento agregados (el valor predeterminado está establecido en **Peor**). Puede encontrar un conjunto predeterminado de los criterios de mantenimiento que se ejecuta como parte de esta característica en la sección [Detalles de la configuración de supervisión](#monitoring-configuration-details) de este artículo.

También puede usar la [lista de instancias de supervisión por recurso](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitorinstances/listbyresource) de la API REST de Azure Monitor para obtener una lista de todos los criterios de mantenimiento. Este criterio incluye detalles de configuración que se ejecutan en el recurso de máquina virtual de Azure.

Se puede modificar la configuración del tipo de criterios de mantenimiento **Unidad** seleccionando el vínculo de puntos suspensivos de la derecha. Seleccione **Mostrar detalles** para abrir el panel de configuración.

![Ejemplo de la configuración de un criterio de mantenimiento](./media/vminsights-health/health-diagnostics-vm-example-02.png)

En el panel de configuración de los criterios de mantenimiento seleccionados, si se fija en el ejemplo **Average Disk Seconds Per Write** (Promedio de segundos de disco por escritura), verá que el umbral puede configurarse con un valor numérico diferente. Este es un sistema de supervisión de dos estados, lo que significa que solo puede cambiar de **Correcto** a **Advertencia**.

También puede haber un criterio de mantenimiento de tres estados, en el que puede configurar un valor para el umbral de estado de mantenimiento de advertencia y crítico. También puede modificar un umbral mediante la [configuración del monitor](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update) de la API REST de Azure Monitor.

>[!NOTE]
>Si aplica el cambio de configuración de los criterios de mantenimiento a una instancia, también se aplicará a todas las instancias supervisadas. Por ejemplo, si selecciona **Disk -1 D:** (Disco - 1 D:) y modifica el umbral **Average Disk Seconds Per Write** (Promedio de segundos de disco por escritura), el cambio se aplica a todas las instancias detectadas y supervisadas de la máquina virtual.


![Ejemplo de la configuración de un criterio de mantenimiento de un monitor de unidad](./media/vminsights-health/health-diagnostics-criteria-config-01.png)

Si desea más información sobre los criterios de mantenimiento, se incluyen artículos de conocimientos que le ayudarán a identificar los problemas, sus causas y las soluciones. Seleccione **Ver información** en la página para ver el artículo de conocimientos relacionado.

Para revisar todos los artículos de conocimientos incluidos con Azure Monitor para VM, consulte [Documentación de conocimientos de mantenimiento de Azure Monitor](https://docs.microsoft.com/azure/monitoring/infrastructure-health/).

### <a name="state-changes"></a>Cambios de estado

La columna del extremo derecho de la página **Diagnóstico de mantenimiento** es **Cambios de estado**. Esta columna enumera todos los cambios de estado asociados con los criterios de mantenimiento que se han seleccionado en la sección **Criterios de mantenimiento** o el cambio de estado de la máquina virtual si se ha seleccionado una máquina virtual en la columna **Modelo de componentes** o **Criterios de mantenimiento** de la tabla.

![Ejemplo de los cambios de estado presentados en la página Diagnóstico de mantenimiento](./media/vminsights-health/health-diagnostics-page-statechanges.png)

En la sección siguiente se muestra el estado de los criterios de mantenimiento y la hora asociada. Esta información muestra el estado más reciente en la parte superior de la columna.

### <a name="association-of-component-model-health-criteria-and-state-changes-columns"></a>Asociación de las columnas Modelo de componentes, Criterios de mantenimiento y Cambios de estado

Las tres columnas se entrelazan entre sí. Cuando se selecciona una instancia en la columna **Modelo de componentes**, la columna **Criterios de mantenimiento** se filtra a la vista de ese componente. En consecuencia, la columna **Cambios de estado** se actualiza según los criterios de mantenimiento seleccionados.

![Ejemplo de la selección de la instancia supervisada y los resultados](./media/vminsights-health/health-diagnostics-vm-example-01.png)

Por ejemplo, si selecciona *Disk - 1 D:* en la lista **Modelo de componentes**, **Criterios de mantenimiento** se filtra a *Disk - 1 D:* y  **Cambios de estado** muestra el cambio de estado en función de la disponibilidad de *Disk - 1 D:* .

Para ver el estado de mantenimiento actualizado, puede actualizar la página Diagnóstico de mantenimiento si selecciona el vínculo **Actualizar**. Si hay una actualización en el estado de mantenimiento de los criterios de mantenimiento basada en el intervalo de sondeo predefinido, esta tarea permite evitar la espera y refleja el estado de mantenimiento más reciente. **Estado de criterios de mantenimiento** es un filtro que le permite fijar un ámbito en los resultados en función del estado de mantenimiento seleccionado: Correcto, Advertencia, Crítico, Desconocido y Todos. La hora de la **Última actualización** de la esquina superior derecha representa la última vez que se actualizó la página Diagnóstico de mantenimiento.

## <a name="alerts"></a>Alertas

El mantenimiento de Azure Monitor para VM se integra con las [Alertas de Azure](../../azure-monitor/platform/alerts-overview.md). Genera una alerta cuando se detecta que los criterios predefinidos cambian de un estado correcto a un estado incorrecto. Las alertas se clasifican por gravedad, desde Gravedad 0 a Gravedad 4, siendo Gravedad 0 el nivel más alto.

Las alertas no están asociadas a un grupo de acciones para notificarle cuándo se ha desencadenado la alerta. Un usuario con el rol de propietario en el ámbito de la suscripción debe configurar las notificaciones siguiendo los pasos descritos en la sección de [configuración de alertas](#configure-alerts).

El número total de alertas de mantenimiento de la máquina virtual clasificadas según la gravedad está disponible en el panel **Mantenimiento** de la sección **Alertas**. Cuando selecciona el número total de alertas o el número correspondiente a un nivel de gravedad, la página **Alertas** se abre y muestra todas las alertas que coinciden con su selección.

Por ejemplo, si ha seleccionado la fila correspondiente a **Sev level 1** (Nivel de gravedad 1), verá lo siguiente:

![Ejemplo de todas las alertas de nivel de gravedad 1](./media/vminsights-health/vminsights-sev1-alerts-01.png)

La página **Todas las alertas** no está limitada para mostrar solo las alertas que coinciden con su selección. También se filtra por **Tipo de recurso** para mostrar solo las alertas de mantenimiento generadas por un recurso de máquina virtual. Este formato se refleja en la lista de alertas, en la columna **Recurso de destino**, donde muestra la máquina virtual de Azure que generó la alerta al cumplirse una condición de estado incorrecto.

No se pretende incluir en esta vista las alertas de otros tipos de recursos o servicios. Estas alertas incluyen las alertas de registro, que se basan en consultas del registro o alertas de métricas que se verían normalmente desde la página predeterminada [Todas las alertas](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) de Azure Monitor.

Para filtrar esta vista, seleccione los valores en los menús desplegables que aparecen en la parte superior de la página.

|Columna |Descripción |
|-------|------------|
|Subscription |Seleccione una suscripción de Azure. Solo se incluyen las alertas de la suscripción seleccionada en la vista. |
|Grupo de recursos |Seleccione un solo grupo de recursos. Solo las alertas con destinos en el grupo de recursos seleccionado se incluyen en la vista. |
|Tipo de recurso |Seleccione uno o varios tipos de recurso. De manare predeterminada, se seleccionan las alertas de las **máquinas virtuales** de destino y se incluyen en esta vista. Esta columna solo está disponible tras especificar un grupo de recursos. |
|Resource |Seleccione un recurso. Solo las alertas con ese recurso como destino se incluyen en la vista. Esta columna solo está disponible tras especificar un tipo de recurso. |
|severity |Seleccione un nivel de gravedad de alerta o seleccione **Todo** para incluir alertas de todos los niveles de gravedad. |
|Condición de supervisión |Seleccione una condición de supervisión para filtrar las alertas en función de si el sistema las ha desencadenado o resuelto, en el caso de que la condición ya no esté activa. O bien seleccione **Todas** para incluir alertas de todas las condiciones. |
|Estado de alerta |Seleccione un estado de alerta, **Nueva**, **Confirmación** o **Cerrada** o seleccione **Todas** para incluir alertas de todos los estados. |
|Servicio de supervisión |Seleccione un servicio o seleccione **Todo** para incluir todos los servicios. Solo las alertas de Conclusiones de la máquina virtual son compatibles con esta característica.|
|Intervalo de horas| Solo las alertas activadas dentro del período de tiempo seleccionado se incluyen en la vista. Los valores compatibles son Última hora, Últimas 24 horas, Últimos 7 días y Últimos 30 días. |

La página **Detalles de la alerta** se muestra cuando se selecciona una alerta. Esta página proporciona detalles de la alerta y permite cambiar su estado.

Para aprender a administrar alertas, consulte [Creación, visualización y administración de alertas mediante Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

>[!NOTE]
>Actualmente no se admite la creación de nuevas alertas basadas en criterios de mantenimiento ni la modificación de las reglas de alerta de mantenimiento existentes en Azure Monitor desde el portal.


![Panel Detalles de la alerta de una alerta seleccionada](./media/vminsights-health/alert-details-pane-01.png)

El estado de la alerta también se puede cambiar para una o varias alertas. Para ello, debe seleccionarlas y, luego, seleccionar **Cambiar estado** en la página **Todas las alertas** en la esquina superior izquierda. Seleccione uno de los estados en el panel **Cambiar el estado de la alerta**, agregue una descripción del cambio en el campo **Comentarios** y, a continuación, seleccione **Aceptar** para confirmar los cambios. Cuando se verifica la información y se aplican los cambios, puede realizar un seguimiento del progreso en la opción **Notificaciones** del menú.

### <a name="configure-alerts"></a>Configurar alertas
No es posible administrar ciertas tareas de administración de alertas desde Azure Portal. Estas tareas se deben realizar mediante el uso de la [API REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components). Concretamente:

- Habilitar o deshabilitar una alerta para los criterios de mantenimiento
- Configurar notificaciones para las alertas de los criterios de mantenimiento

Los ejemplos utilizan [ARMClient](https://github.com/projectkudu/armclient) en el equipo Windows. Si no está familiarizado con este método, consulte [Uso de ARMClient](../platform/rest-api-walkthrough.md#use-armclient).

#### <a name="enable-or-disable-an-alert-rule"></a>Habilitación o deshabilitación de una regla de alertas

Para habilitar o deshabilitar una alerta para un criterio de mantenimiento específico, la propiedad **alertGeneration** se debe modificar con un valor de **Deshabilitado** o **Habilitado**.

Para identificar el elemento *monitorId* de un criterio de mantenimiento específico, el ejemplo siguiente muestra cómo realizar una consulta de dicho valor para el criterio **LogicalDisk\Avg Disk Seconds Per Transfer**:

1. En una ventana de terminal, escriba **inicio de sesión de armclient.exe**. Al hacerlo, se le pedirá que inicie sesión en Azure.

2. Escriba el siguiente comando para recuperar todos los criterios de mantenimiento activos de una máquina virtual específica e identifique el valor de la propiedad *monitorId*:

    ```
    armclient GET "subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors?api-version=2018-08-31-preview"
    ```

    En el ejemplo siguiente se muestra la salida del comando *armclient GET*. Tome nota del valor del elemento *MonitorId*. Este valor es necesario para el paso siguiente, en el que debemos especificar el identificador de los criterios de mantenimiento y modificar su propiedad para crear una alerta.

    ```
    "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/monitors/ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "name": "ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "type": "Microsoft.WorkloadMonitor/virtualMachines/monitors"
    },
    {
      "properties": {
        "description": "Monitor the performance counter LogicalDisk\\Avg Disk Sec Per Transfer",
        "monitorId": "Microsoft_LogicalDisk_AvgDiskSecPerTransfer",
        "monitorName": "Microsoft.LogicalDisk.AvgDiskSecPerTransfer",
        "monitorDisplayName": "Average Logical Disk Seconds Per Transfer",
        "parentMonitorName": null,
        "parentMonitorDisplayName": null,
        "monitorType": "Unit",
        "monitorCategory": "PerformanceHealth",
        "componentTypeId": "LogicalDisk",
        "componentTypeName": "LogicalDisk",
        "componentTypeDisplayName": "Logical Disk",
        "monitorState": "Enabled",
        "criteria": [
          {
            "healthState": "Warning",
            "comparisonOperator": "GreaterThan",
            "threshold": 0.1
          }
        ],
        "alertGeneration": "Enabled",
        "frequency": 1,
        "lookbackDuration": 17,
        "documentationURL": "https://aka.ms/Ahcs1r",
        "configurable": true,
        "signalType": "Metrics",
        "signalName": "VMHealth_Avg. Logical Disk sec/Transfer"
      },
      "etag": null,
    ```

3. Escriba el siguiente comando para modificar la propiedad *alertGeneration*:

    ```
    armclient patch subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors/Microsoft_LogicalDisk_AvgDiskSecPerTransfer?api-version=2018-08-31-preview "{'properties':{'alertGeneration':'Disabled'}}"
    ```   

4. Escriba el comando GET utilizado en el paso 2 para comprobar que el valor de la propiedad está establecido en **Deshabilitado**.

#### <a name="associate-an-action-group-with-health-criteria"></a>Asociación de un grupo de acciones con los criterios de mantenimiento

El mantenimiento de Azure Monitor para VM admite notificaciones por SMS y correo electrónico cuando se generan alertas de los criterios de mantenimiento incorrectos. Para configurar las notificaciones, anote el nombre del grupo de acciones configurado para enviar notificaciones por correo electrónico o SMS.

>[!NOTE]
>Esta acción se debe realizar en cada máquina virtual supervisada de la que desea recibir una notificación. No se aplica a todas las máquinas virtuales de un grupo de recursos.

1. En una ventana de terminal, escriba *armclient.exe login*. Al hacerlo, se le pedirá que inicie sesión en Azure.

2. Escriba el siguiente comando para asociar un grupo de acciones a las reglas de alertas:
 
    ```
    $payload = "{'properties':{'ActionGroupResourceIds':['/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/microsoft.insights/actionGroups/actiongroupName']}}"
    armclient PUT https://management.azure.com/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings/default?api-version=2018-08-31-preview $payload
    ```

3. Para comprobar que el valor de la propiedad **actionGroupResourceIds** se ha actualizado correctamente, escriba el siguiente comando:

    ```
    armclient GET "subscriptions/subscriptionName/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings?api-version=2018-08-31-preview"
    ```

    La salida debe ser similar a los siguientes criterios:
    
    ```
    {
      "value": [
        {
          "properties": {
            "actionGroupResourceIds": [
              "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourceGroups/Lab/providers/microsoft.insights/actionGroups/Lab-IT%20Ops%20Notify"
            ]
          },
          "etag": null,
          "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/notificationSettings/default",
          "name": "notificationSettings/default",
          "type": "Microsoft.WorkloadMonitor/virtualMachines/notificationSettings"
        }
      ],
      "nextLink": null
    }
    ```

## <a name="next-steps"></a>Pasos siguientes

- Para identificar las limitaciones y el rendimiento general de la máquina virtual, consulte [Visualización del rendimiento de la máquina virtual de Azure](vminsights-performance.md).

- Para más información sobre las dependencias de las aplicaciones detectadas, consulte [Uso de la asignación de Azure Monitor para VM](vminsights-maps.md).
