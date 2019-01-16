---
title: Supervisión del estado de la máquina virtual con Azure Monitor para VM (versión preliminar) | Microsoft Docs
description: En este artículo se describe cómo entender el estado de la máquina virtual y el sistema operativo subyacente con Azure Monitor para las máquinas virtuales.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: magoedte
ms.openlocfilehash: 737e05f3d936481e06acfc0604ff739b9f01d5db
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54191656"
---
# <a name="understand-the-health-of-your-azure-virtual-machines-with-azure-monitor-for-vms-preview"></a>Descripción del estado de las máquinas virtuales de Azure con Azure Monitor para VM (versión preliminar)
Azure incluye varios servicios que realizan individualmente una tarea o un rol específico en el espacio de supervisión, pero hasta ahora no era posible proporcionar una perspectiva detallada del estado del sistema operativo hospedado en máquinas virtuales de Azure.  Aunque se podían supervisar distintas condiciones mediante Log Analytics o Azure Monitor, no estaban diseñadas para modelar ni representar el estado general de la máquina virtual o de los componentes principales.  Con la característica de estado de Azure Monitor para máquinas virtuales, la disponibilidad y el rendimiento del sistema operativo invitado Windows o Linux se supervisan de forma proactiva con un modelo que representa los principales componentes y sus relaciones, así como los criterios que especifican cómo se debe medir el estado de dichos componentes y, además, le avisa cuando se detecta una condición de estado incorrecto.  

El estado de mantenimiento general de la máquina virtual de Azure y el sistema operativo subyacente pueden observarse desde dos perspectivas con el estado de Azure Monitor para máquinas virtuales, ya sea directamente desde la máquina virtual o bien en todas las máquinas virtuales de un grupo de recursos de Azure Monitor.

En este artículo le ayudamos a comprender cómo puede evaluar, investigar y resolver rápidamente los problemas detectados.

Para obtener más información sobre cómo configurar Azure Monitor para máquinas virtuales, consulte el artículo [Enable Azure Monitor for VMs](vminsights-onboard.md) (Habilitar Azure Monitor para máquinas virtuales).

## <a name="monitoring-configuration-details"></a>Detalles de configuración de supervisión
En esta sección se describen los criterios de estado predeterminados definidos para supervisar las máquinas virtuales Linux y Windows de Azure. Todos los criterios de mantenimiento están preconfigurados para enviar una alerta cuando se dé una condición de mal estado. 

### <a name="windows-vms"></a>Máquinas virtuales Windows

- Megabytes disponibles de memoria 
- Media de segundos del disco por escritura (disco lógico)
- Media de segundos del disco por escritura (disco)
- Media de segundos del disco lógico por lectura
- Media de segundos del disco lógico por transferencia
- Media de segundos del disco por lectura
- Media de segundos del disco por transferencia
- Longitud actual de cola de disco (disco lógico)
- Longitud actual de cola de disco (disco)
- Porcentaje de tiempo de inactividad del disco
- Error o daños en el sistema de archivos
- Poco espacio disponible en el disco lógico (%)
- Poco espacio disponible en el disco lógico (MB)
- Porcentaje de tiempo de inactividad del disco lógico
- Páginas de memoria por segundo
- Porcentaje de ancho de banda usado para la lectura
- Porcentaje total del ancho de banda usado
- Porcentaje de ancho de banda para la escritura
- Porcentaje de memoria asignada en uso
- Porcentaje de tiempo de inactividad del disco físico
- Service Health del cliente DHCP
- Service Health del cliente DNS
- Service Health de RPC
- Service Health de servidor
- Porcentaje de uso total de la CPU
- Service Health del registro de eventos de Windows
- Service Health del firewall de Windows
- Service Health de Administración remota de Windows

### <a name="linux-vms"></a>Máquinas virtuales con Linux
- Media del disco Segundos de disco/transferencias 
- Media del disco Segundos de disco/lecturas 
- Media del disco Segundos de disco/escrituras 
- Estado del disco
- Espacio disponible en el disco lógico
- % de espacio disponible en el disco lógico
- % de inode disponibles en el disco lógico
- Estado del adaptador de red
- Porcentaje de tiempo de DPC del procesador
- Porcentaje de tiempo del procesador
- Porcentaje de tiempo del procesador total
- Porcentaje de tiempo de DPC total
- Megabytes disponibles de memoria del sistema operativo

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal
Inicie sesión en el [Azure Portal](https://portal.azure.com). 

## <a name="introduction-to-health-experience"></a>Introducción a la experiencia del estado
Antes de profundizar en cómo usar la característica de estado para una sola máquina virtual o un grupo de VM, es importante que brindemos una breve introducción para que pueda comprender cómo se presenta la información y qué representan las visualizaciones.  

## <a name="view-health-directly-from-a-virtual-machine"></a>Visualización del estado directamente desde una máquina virtual 
Para ver el estado de una máquina virtual de Azure, seleccione **Conclusiones (versión preliminar)** desde el panel izquierdo de la máquina virtual. En la página de conclusiones de la VM, el **estado** se abre de forma predeterminada y se muestra la vista de estado de la máquina virtual.  

![Información general del estado de Azure Monitor para máquinas virtuales de una máquina virtual de Azure seleccionada](./media/vminsights-health/vminsights-directvm-health.png)

En la pestaña **Estado** de la sección **Estado de la VM invitada**, la tabla muestra el estado de mantenimiento actual de la máquina virtual y el número total de alertas de estado de la máquina virtual generado por un componente en mal estado. Consulte [Alerting and an alert management](#alerting-and-alert-management) (Creación y administración de alertas) para obtener más detalles.  

Los estados de mantenimiento definidos de una VM son los siguientes: 

* **Correcto** : no se detectó ningún problema para la VM y funciona según lo necesario.  
* **Crítico**: se detectan uno o varios problemas críticos, que deben solucionarse para restaurar la funcionalidad normal según lo previsto. 
* **Advertencia**: se detectan uno o varios problemas que deben solucionarse o la condición de estado puede volverse crítica.  
* **Desconocido**: si el servicio no pudo establecer ninguna conexión con la VM, el estado cambia a un estado desconocido.  

Si selecciona **Ver el diagnóstico de estado**, se abre una página que muestra todos los componentes de la VM, los criterios de estado asociados, los cambios de estado y otros temas importantes detectados mediante la supervisión de componentes relacionados con la máquina virtual. Para más información, consulte [Diagnóstico de mantenimiento](#health-diagnostics). 

En la sección **Estado del componente**, la tabla muestra un estado acumulativo del mantenimiento de las categorías de rendimiento principales supervisadas según los criterios de estado de esas áreas, específicamente la **CPU**, la **memoria**, el **disco** y la **red**.  Si selecciona cualquiera de los componentes, se abre una página en la que se enumeran todos los aspectos de supervisión de los criterios de estado individuales de ese componente y el estado de mantenimiento correspondiente de cada uno.  

Al obtener acceso al estado de una VM de Azure que ejecuta el sistema operativo Windows, los estados de mantenimiento de los cinco principales servicios de Windows se muestran en la sección **Estado de los servicios principales**.  Cuando selecciona cualquiera de los servicios, se abre una página que enumera los criterios de estado que supervisan dicho componente y su estado de mantenimiento.  Al hacer clic en el nombre de los criterios de mantenimiento, se abrirá el panel de propiedades y, desde aquí, puede revisar los detalles de configuración, por ejemplo, si los criterios de estado tienen definida una alerta de Azure Monitor correspondiente. Para más información, consulte [Health Diagnostics and working with health criteria](#health-diagnostics) (Diagnóstico de mantenimiento y trabajo con los criterios de mantenimiento).  

## <a name="aggregate-virtual-machine-perspective"></a>Perspectiva de la máquina virtual agregada
Para ver la colección de mantenimiento de todas las máquinas virtuales de un grupo de recursos, vaya a la lista de navegación del portal, seleccione **Azure Monitor** y, a continuación, seleccione **Máquinas virtuales (versión preliminar)**.  

![Vista de supervisión de conclusiones de la VM de Azure Monitor](./media/vminsights-health/vminsights-aggregate-health.png)

En las listas desplegables **Suscripción** y **Grupo de recursos**, seleccione el grupo de recursos adecuado que incluya las máquinas virtuales relacionadas con el grupo para ver el estado de mantenimiento notificado.  La selección solo se aplica a la característica de mantenimiento y no se aplica a las secciones de rendimiento o asignación.

En la pestaña **Mantenimiento**, puede aprender lo siguiente:

* ¿Cuántas VM se encuentran en un estado crítico o incorrecto, frente a cuántas están en buen estado o no envían datos (es decir, tienen un estado desconocido)?
* ¿Qué máquinas virtuales según el sistema operativo (SO) notifican un estado incorrecto y cuántas?
* ¿Cuántas VM tienen un estado incorrecto debido a un problema detectado con un procesador, un disco, la memoria o un adaptador de red, clasificados según el estado de mantenimiento?  
* ¿Cuántas VM tienen un estado incorrecto debido a un problema detectado con un procesador, un disco, la memoria o un adaptador de red, clasificados según el estado de mantenimiento?

Aquí puede identificar rápidamente los principales problemas críticos detectados por los criterios de mantenimiento que supervisan la VM de forma proactiva, así como revisar los detalles de la alerta de estado de la VM y el artículo de conocimiento asociado a fin de obtener ayuda para diagnosticar y solucionar el problema.  Seleccione cualquiera de las gravedades para abrir la página [Todas las alertas](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) filtrada según esa gravedad.

En la lista **VM distribution by operating system** (Distribución de la máquina virtual por sistema operativo) se muestran las VM que enumera la edición de Windows o la distribución de Linux, junto con su versión. En cada categoría de sistema operativo, las VM se desglosan aún más según el estado de la VM. 

![Perspectiva de la distribución de la máquina virtual de conclusiones de la VM](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

Los estados de mantenimiento definidos de una VM son los siguientes: 

* **Correcto** : no se detectó ningún problema para la VM y funciona según lo necesario.  
* **Crítico**: se detectan uno o varios problemas críticos, que deben solucionarse para restaurar la funcionalidad normal según lo previsto. 
* **Advertencia**: se detectan uno o varios problemas que deben solucionarse o la condición de estado puede volverse crítica.  
* **Desconocido**: si el servicio no pudo establecer ninguna conexión con la VM, el estado cambia a un estado desconocido.  

Puede hacer clic en cualquier elemento de columna (**Número de VM**, **Crítico**, **Advertencia**, **Correcto** o **Desconocido**) para explorar en profundidad la página **Máquinas virtuales** y ver una lista de los resultados filtrados que coincidan con la columna seleccionada. Por ejemplo, si quiere revisar todas las VM que ejecutan la versión **Red Hat Enterprise Linux 7.5**, haga clic en el valor **Número de VM** de ese sistema operativo y se abrirá la página siguiente, donde se enumerarán las máquinas virtuales que coincidan con ese filtro y su estado de mantenimiento conocido actualmente.  

![Ejemplo de la acumulación de VM Red Hat Linux](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)
 
En la página **Máquinas virtuales**, si selecciona el nombre de una VM en la columna **Nombre de VM**, se le dirigirá a la página de la instancia de la VM con más detalles acerca de los problemas con criterios de estado y alertas identificados que afectan a la VM seleccionada.  Desde aquí, puede filtrar los detalles del estado de mantenimiento. Para ello, haga clic en el icono **Estado de mantenimiento** en la esquina superior izquierda de la página para ver qué componentes son incorrectos, o bien puede ver las alertas de estado de la VM generadas por un componente con un estado incorrecto y clasificadas según la gravedad.    

Desde la vista de la lista de VM, si hace clic en el nombre de una VM, se abre la página **Mantenimiento** de esa VM seleccionada, del mismo modo que si selecciona **Conclusiones (versión preliminar)** desde la VM directamente.

![Conclusiones de una máquina virtual de Azure seleccionada](./media/vminsights-health/vminsights-directvm-health.png)

A continuación, se muestra el **estado de mantenimiento** de una acumulación de la máquina virtual y las **alertas**, clasificadas según la gravedad, que representan las alertas de estado de la VM generadas cuando el estado de mantenimiento cambia de correcto a incorrecto según los criterios de estado.  Si selecciona **VMs in critical condition** (VM en una condición crítica), se abrirá una página con una lista de una o varias VM que se encuentran en un estado de mantenimiento crítico.  Al hacer clic en el estado de mantenimiento de una de las VM de la lista, se mostrará la vista **Diagnóstico de mantenimiento** de la VM.  A continuación, puede averiguar los criterios de estado que reflejan un problema de estado de mantenimiento. Cuando se abre la página **Diagnóstico de mantenimiento**, se muestran todos los componentes de la VM y sus criterios de estado asociados con el estado de mantenimiento actual.  Consulte la sección [Diagnóstico de mantenimiento](#health-diagnostics) para obtener más detalles.  

Al seleccionar **Ver todos los criterios de estado**, se abre una página que muestra una lista de todos los criterios de mantenimiento disponibles con esta característica.  La información se puede filtrar aún más en función de las siguientes opciones:

* **Tipo**: existen tres tipos de criterios de estado para evaluar condiciones y acumular el estado de mantenimiento general de la VM supervisada.  
     a. **Unidad**: mide algún aspecto de la máquina virtual. Es posible que este tipo de criterios de estado comprueben un contador de rendimiento para determinar el rendimiento del componente, mediante la ejecución de un script para realizar una transacción sintética, o controlar un evento que indica un error.  De forma predeterminada, el filtro se establece en la unidad.  
    b. **Dependencia**: proporciona la acumulación de estado entre las distintas entidades. Los criterios de este estado permiten que el estado de una entidad dependa del estado de otro tipo de entidad en la que se basa para que la operación se realice correctamente.  
    c. **Agregado**: proporciona un estado de mantenimiento combinado de los criterios de estado similares. Los criterios de estado de unidad y de dependencia normalmente se configurarán a partir de un criterio de estado agregado. Además de proporcionar una mejor organización general de los distintos criterios de mantenimiento de una entidad, los criterios de estado agregados proporcionan un estado único de mantenimiento para las distintas categorías de las entidades.

* **Categoría**: tipo de criterios de estado que se usan para agrupar los criterios de un tipo similar con fines informativos.  Corresponden a los de **disponibilidad** o **rendimiento**.

Puede explorar en profundidad para ver las instancias que tienen un estado incorrecto. Para ello, haga clic en un valor de la columna **Unhealthy Component** (Componente con un estado incorrecto).  En la página, una tabla enumera los componentes que se encuentran en un estado de mantenimiento crítico.    

## <a name="health-diagnostics"></a>Diagnóstico de mantenimiento
La página **Diagnóstico de mantenimiento** le permite ver todos los componentes de la VM, los criterios de estado asociados, los cambios de estado y otros problemas importantes detectados mediante la supervisión de objetos relacionados con la VM. 

![Ejemplo de la página Diagnóstico de mantenimiento para una VM](./media/vminsights-health/health-diagnostics-page-01.png)

Puede iniciar el diagnóstico de mantenimiento de las maneras siguientes.

* Mediante el estado de mantenimiento acumulativo de todas las VM desde la perspectiva de la VM agregada en Azure Monitor.  En la página **Mantenimiento**, haga clic en el icono del estado de mantenimiento **Crítico**, **Advertencia**, **Correcto** o **Desconocido** de la sección **Estado de la VM invitada** y explore en profundidad la página que enumera todas las VM que coincidan con esa categoría filtrada.  Al hacer clic en el valor de la columna **Estado de mantenimiento**, se abrirán los diagnósticos de mantenimiento con ámbito en esa máquina virtual concreta.      

* Mediante el sistema operativo desde la perspectiva de la VM agregada en Azure Monitor. En **VM distribution** (Distribución de VM), al seleccionar cualquiera de los valores de la columna, se abrirá la página **Máquinas virtuales** y se devolverá una lista en la tabla que coincida con la categoría filtrada.  Al hacer clic en el valor de la columna **Estado de mantenimiento**, se abre el diagnóstico de mantenimiento para la máquina virtual seleccionada.    
 
* Desde la VM invitada de la pestaña **Mantenimiento** de Azure Monitor para máquinas virtuales, seleccione **Ver el diagnóstico de estado**. 

El diagnóstico de mantenimiento organiza la información de estado en las siguientes categorías: 

* Disponibilidad
* Rendimiento
 
Todos los criterios de mantenimiento definidos para un destino seleccionado se muestran en la categoría correspondiente. 

El estado de mantenimiento de los criterios de estado se define mediante uno de los tres estados: *Crítico*, *Advertencia* y *Correcto*. Hay otro estado *Desconocido*, que no está asociado con el estado de mantenimiento, pero representa el estado de supervisión conocido por la característica.  

En la tabla siguiente se proporcionan detalles sobre los estados de mantenimiento que se representan en el diagnóstico de mantenimiento.

|Icono |Estado de mantenimiento |Significado |
|-----|-------------|------------|
| |Healthy |El estado de mantenimiento es correcto si se encuentra dentro de las condiciones de mantenimiento definidas. En el caso de un monitor de acumulación primario, el mantenimiento se acumula y refleja el mejor o el peor estado del elemento secundario.|
| |Crítico |El estado de mantenimiento es crítico si no se encuentra dentro de la condición de mantenimiento definida. En el caso de un monitor de acumulación primario, el mantenimiento se acumula y refleja el mejor o el peor estado del elemento secundario.|
| |Advertencia |El estado de mantenimiento es de Advertencia si este se encuentra entre dos umbrales para la condición de mantenimiento definida, donde uno indica el estado *Advertencia* y el otro, el estado *Crítico*. En el caso de un monitor de acumulación primario, si uno o varios de los elementos secundarios están en un estado de advertencia, el elemento primario reflejará el estado *Advertencia*. Si hay un elemento secundario que se encuentra en estado *Crítico* y otro en estado *Advertencia*, la acumulación primaria mostrará un estado de mantenimiento *Crítico*.|
| |Desconocido |El estado de mantenimiento es *Desconocido* cuando no se puede calcular por varios motivos, por ejemplo, no se pueden recopilar los datos, no se inicializó el servicio, etc.| 

La página de diagnóstico de mantenimiento tiene tres secciones principales:

* Modelo de componentes 
* Criterios del mantenimiento
* Cambios de estado 

![Secciones de la página Diagnóstico de mantenimiento](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Modelo de componente
La columna más a la izquierda de la página de diagnóstico Mantenimiento es el modelo de componente. Todos los componentes y sus instancias detectadas, que están asociados a la máquina virtual, se muestran en esta columna. 

En el ejemplo siguiente, los componentes detectados son el disco, el disco lógico, el procesador, la memoria y el sistema operativo. Varias instancias de estos componentes se detectan y se muestran en esta columna, con dos instancias del disco lógico **/**, **/boot** y **/mnt/resource**, una instancia del adaptador de red **eth0**, dos instancias del disco **sda** y **sdb**, dos instancias del procesador **0 y 1** y la **versión 7.4 (Maipo) (sistema operativo) de Red Hat Enterprise Linux Server**. 

![Ejemplo del modelo de componente presentado en el diagnóstico de mantenimiento](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Criterios de mantenimiento
La columna central de la página Diagnóstico de mantenimiento es **Criterios de mantenimiento**. El modelo de estado definido para la VM se muestra en un árbol jerárquico. El modelo de estado de una máquina virtual está compuesto por los criterios de mantenimiento agregados, la dependencia y la unidad.  

![Ejemplo de los criterios de mantenimiento presentados en la página Diagnóstico de mantenimiento](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

Un criterio de mantenimiento mide el mantenimiento de la instancia supervisada con algunos criterios, lo que podría ser un valor de umbral o un estado de una entidad, etc. Dicho criterio tiene dos o tres estados de mantenimiento, tal y como se describe en la sección anterior. En cualquier momento dado, el criterio de mantenimiento solo puede estar en uno de sus estados posibles. 

El mantenimiento general de un destino viene determinado por el mantenimiento de cada uno de sus criterios de mantenimiento definidos en el modelo de mantenimiento. Se tratará de una combinación de criterios de mantenimiento dirigidos directamente al destino y de criterios de estado dirigidos a la acumulación de componentes en el destino mediante un criterio de mantenimiento de dependencia. Esta jerarquía se ilustra en la sección **Criterios de mantenimiento** de la página Diagnóstico de mantenimiento. La directiva de acumulación del mantenimiento es la parte de la configuración de los criterios de mantenimiento agregados y de dependencia. Puede encontrar una lista del conjunto predeterminado de los criterios de mantenimiento que se ejecuta como parte de esta característica en la sección [Detalles de configuración de supervisión](#monitoring-configuration-details).  

En el ejemplo siguiente, el criterio de mantenimiento agregado **Core Windows Services Rollup** (Acumulación de servicios básicos de Windows) para una máquina virtual basada en Windows evalúa el mantenimiento de los servicios más importantes de Windows en función de criterios de mantenimiento del servicio individuales. El estado de cada servicio, como DNS, DHCP, etc., se evalúa y el mantenimiento se acumula según el criterio de mantenimiento de acumulación correspondiente (como se muestra a continuación).  

![Ejemplo de la acumulación del mantenimiento](./media/vminsights-health/health-diagnostics-windows-svc-rollup.png)

El mantenimiento de **Core Windows Services Rollup** (Acumulación de servicios básicos de Windows) pasa a ser **Operating System availability** (Disponibilidad del sistema operativo), que finalmente se acumula en el criterio **Disponibilidad** de la máquina virtual. 

Si quiere, puede modificar la configuración del tipo **Unidad** de los criterios de mantenimiento. Para ello, haga clic en el vínculo de elipse del lado derecho y seleccione **Mostrar detalles** para abrir el panel de configuración. 

![Ejemplo de la configuración de un criterio de mantenimiento](./media/vminsights-health/health-diagnostics-linuxvm-example-03.png)

En este ejemplo del panel de configuración de los criterios de mantenimiento seleccionados, la opción **Logical Disk % Free Space** (Porcentaje de espacio disponible del disco lógico) puede configurarse con un valor numérico diferente para el umbral, ya que es un monitor de dos estados, lo que significa que solo cambia del estado correcto al crítico.  También puede haber un criterio de mantenimiento de tres estados, en el que puede configurar un valor para el umbral de estado de mantenimiento de advertencia y crítico.  

>[!NOTE]
>Si aplica el cambio de configuración de los criterios de mantenimiento a una instancia, también se aplicará a todas las instancias supervisadas.  Por ejemplo, si selecciona **/mnt/resource** y modifica el umbral **Logical Disk % Free Space** (Porcentaje de espacio disponible del disco lógico), este no se aplica únicamente a esa instancia, sino a todas las demás del disco lógico que se detectan y supervisan en la VM.
>

![Ejemplo de la configuración de un criterio de mantenimiento de un monitor de unidad](./media/vminsights-health/health-diagnostics-linuxvm-example-04.png)

Para más información sobre el indicador de estado, se incluyen artículos de conocimientos que le ayudan a identificar problemas, causas y resoluciones.  Haga clic en el vínculo **Ver información** en la página y se abrirá una nueva pestaña en el explorador que muestra el artículo de conocimiento específico.  En cualquier momento, puede revisar todos los artículos de conocimiento sobre criterios de mantenimiento incluidos con Azure Monitor para la característica de mantenimiento de máquinas virtuales [aquí](https://docs.microsoft.com/azure/monitoring/infrastructure-health/).
  
### <a name="state-changes"></a>Cambios de estado
La columna más a la derecha de la página Diagnóstico de mantenimiento es **Cambios de estado**. Enumera todos los cambios de estado asociados con los criterios de mantenimiento que se han seleccionado en la sección **Criterios de mantenimiento** o el cambio de estado de la VM si se ha seleccionado una VM en la columna **Modelo de componente** o **Criterios de mantenimiento** de la tabla. 

![Ejemplo de los cambios de estado presentados en la página Diagnóstico de mantenimiento](./media/vminsights-health/health-diagnostics-page-statechanges.png)

Esta sección está compuesta por el estado de los criterios de mantenimiento y el tiempo asociado ordenados por el estado más reciente.   

### <a name="association-of-component-model-health-criteria-and-state-change-columns"></a>Asociación de las columnas Modelo de componente, Criterios de mantenimiento y Cambio de estado 
Las tres columnas se entrelazan entre sí. Cuando un usuario selecciona una instancia detectada en el modelo de componente, la sección **Criterios de mantenimiento** se filtra a esa vista de componente y, en consecuencia, el **cambio de estado** se actualiza en función de los criterios de mantenimiento seleccionados. 

![Ejemplo de la selección de la instancia supervisada y los resultados](./media/vminsights-health/health-diagnostics-linuxvm-example-02.png)

En el ejemplo anterior, cuando se selecciona **/mnt (Logical Disk)** (/mnt [disco lógico]), se filtra el árbol de los criterios de mantenimiento como **/mnt (Logical Disk)** (/mnt [disco lógico]). Las pestañas **Disponibilidad** y **Rendimiento** también se filtran según corresponde. En la columna **Cambio de estado** se muestra el cambio de estado en función de la disponibilidad de **/mnt (Logical Disk)** (/mnt [disco lógico]). 

Para ver el estado de mantenimiento actualizado, puede actualizar la página Diagnóstico de mantenimiento haciendo en el vínculo **Actualizar**.  Si hay una actualización en el estado de mantenimiento de los criterios de mantenimiento basada en el intervalo de sondeo predefinido, esta tarea permite evitar la espera y refleja el estado de mantenimiento más reciente.  **Estado de criterios de mantenimiento** es un filtro que le permite definir el ámbito de los resultados según el estado de mantenimiento seleccionado: Correcto, Advertencia, Crítico, Desconocido, etc.  La hora **Última actualización** de la esquina superior derecha representa la última vez en que se actualizó la página Diagnóstico de estado.  

## <a name="alerting-and-alert-management"></a>Creación y administración de alertas 
La característica de mantenimiento de Azure Monitor para máquinas virtuales se integra con [Alertas de Azure](../../azure-monitor/platform/alerts-overview.md) y genera una alerta cuando los criterios de mantenimiento predefinidos cambian de un estado correcto a uno incorrecto y dicha condición se detecta. Las alertas se clasifican según la gravedad: de 0 a 4, donde el 0 representa el nivel de gravedad más alto.  

El número total de alertas de mantenimiento de la VM clasificadas según la gravedad está disponible en el panel **Mantenimiento** de la sección **Alertas**. Cuando selecciona el número total de alertas o el número correspondiente a un nivel de gravedad, la página **Alertas** se abre y muestra todas las alertas que coinciden con su selección.  Por ejemplo, si ha seleccionado la fila correspondiente a **Sev level 1** (Nivel de gravedad), verá lo siguiente:

![Ejemplo de todas las alertas de nivel de gravedad 1](./media/vminsights-health/vminsights-sev1-alerts-01.png)

El ámbito de la página **Alertas** no se limita a mostrar las alertas que coinciden con la selección, sino que también se filtran por **tipo de recurso** para mostrar solo las alertas de mantenimiento generadas por el recurso de la máquina virtual.  Esto se refleja en la lista de alertas, en la columna **Recurso de destino**, donde muestra la máquina virtual de Azure para la que se generó la alerta para cuando se cumplió la condición de mal estado de los criterios de mantenimiento determinados.  

Las alertas de otros tipos de recursos o servicios no están diseñadas para incluirse en esta vista, como las alertas del registro basadas en las consultas de Log Analytics o las alertas de métricas que normalmente se verían desde la página [Todas las alertas](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) de Azure Monitor predeterminada. 

Para filtrar esta vista, seleccione valores en los menús desplegables que aparecen en la parte superior de la página.

|Columna |DESCRIPCIÓN | 
|-------|------------| 
|Subscription |Seleccione una suscripción de Azure. Solo se incluyen las alertas de la suscripción seleccionada en la vista. | 
|Grupo de recursos |Seleccione un solo grupo de recursos. Solo las alertas con destinos en el grupo de recursos seleccionado se incluyen en la vista. | 
|Tipo de recurso |Seleccione uno o varios tipos de recurso. De manare predeterminada, se seleccionan las alertas de las **máquinas virtuales** de destino y se incluyen en esta vista. Esta columna solo está disponible tras especificar un grupo de recursos. | 
|Recurso |Seleccione un recurso. Solo las alertas con ese recurso como destino se incluyen en la vista. Esta columna solo está disponible tras especificar un tipo de recurso. | 
|Gravedad |Seleccione una nivel de gravedad de la alerta o seleccione *Todas* para incluir las alertas de todos los niveles de gravedad. | 
|Condición de supervisión |Seleccione una condición del monitor para filtrar las alertas en función de si el sistema las ha *desencadenado* o *resuelto*, en el caso de que la condición ya no esté activa. O bien seleccione *Todas* para incluir alertas de todas las condiciones. | 
|Estado de alerta |Seleccione un estado de alerta (*Nueva*, *Confirmación* o *Cerrada*), o seleccione *Todos* para incluir alertas de todos los estados. | 
|Servicio de supervisión |Seleccione un servicio o seleccione *Todo* para incluir todos los servicios. Solo las alertas de *VM Insights* son compatibles con esta característica.| 
|Intervalo de tiempo| Solo las alertas activadas dentro del período de tiempo seleccionado se incluyen en la vista. Los valores compatibles son Última hora, Últimas 24 horas, Últimos 7 días y Últimos 30 días. | 

La página **Detalles de la alerta** se muestra cuando se selecciona una alerta y proporciona detalles de la alerta, lo que le permite cambiar su estado. Para aprender a administrar alertas, consulte [Creación, visualización y administración de alertas mediante Azure Monitor](../../azure-monitor/platform/alerts-metric.md).  

>[!NOTE]
>Actualmente no se admite la creación de nuevas alertas basadas en criterios de mantenimiento ni la modificación de las reglas de alerta de mantenimiento existentes en Azure Monitor desde el portal.  
>

![Panel Detalles de la alerta de una alerta seleccionada](./media/vminsights-health/alert-details-pane-01.png)

El estado de la alerta también se puede cambiar para una o varias alertas. Para ello, debe seleccionarlas y, luego, seleccionar **Cambiar estado** en la página **Todas las alertas**, en la esquina superior izquierda. En el panel **Cambiar el estado de alerta**, seleccione uno de los estados, agregue una descripción del cambio en el campo **Comentario** y, a continuación, haga clic en **Aceptar** para confirmar los cambios. Mientras se verifica la información y se aplican los cambios, puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.  

## <a name="next-steps"></a>Pasos siguientes
Para identificar cuellos de botella y el uso general con el rendimiento de sus VM, consulte el artículo sobre cómo [ver el rendimiento de la VM de Azure](vminsights-performance.md), o bien, para ver las dependencias de la aplicación detectadas, consulte cómo [ver el Azure Monitor para la asignación de VM](vminsights-maps.md). 
