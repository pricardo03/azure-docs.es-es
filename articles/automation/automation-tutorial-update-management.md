---
title: Administración de actualizaciones y revisiones para las máquinas virtuales de Azure
description: En este artículo se proporciona una introducción al uso de Azure Automation y Update Management para administrar las actualizaciones y revisiones de las máquinas virtuales Windows de Azure.
services: automation
author: zjalexander
ms.service: automation
ms.subservice: update-management
ms.topic: tutorial
ms.date: 12/04/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 7f62e437d80f023972327d98b4add193eb7b8dd6
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73044416"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Administración de actualizaciones y revisiones para las máquinas virtuales de Azure

Puede usar la solución Update Management para administrar las actualizaciones y las revisiones de las máquinas virtuales. En este tutorial aprenderá a evaluar rápidamente el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias, revisar los resultados de la implementación y crear una alerta para verificar que las actualizaciones se aplican correctamente.

Para obtener información de precios, consulte [Precios de Automation para Update Management](https://azure.microsoft.com/pricing/details/automation/).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar máquinas virtuales a Update Management
> * Visualización de la evaluación de la actualización
> * Configurar alertas
> * Programación de una implementación de actualizaciones
> * Ver los resultados de una implementación

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesita:

* Una suscripción de Azure. Si aún no tiene uno, puede [activar su crédito mensual de Azure para suscriptores de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse a una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Azure Automation](automation-offering-get-started.md) para contener los runbooks del monitor y de acción y la tarea del monitor.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md) para incorporar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-update-management"></a>Habilitación de la Administración de actualizaciones

En primer lugar, habilite Update Management en la máquina virtual de este tutorial:

1. En el menú izquierdo de Azure Portal, seleccione **Máquinas virtuales**. Seleccione una máquina virtual de la lista.
2. En la página de la máquina virtual, en **Operaciones**, seleccione **Update Management**. Se abre el panel **Habilitar Update Management**.

Se realiza la validación para determinar si Update Management está habilitado para esta máquina virtual. Esta validación incluye comprobaciones de un área de trabajo de Azure Log Analytics y la cuenta de Automation vinculada, y si la solución Update Management está en el área de trabajo.

Las áreas de trabajo de [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) se usan para recopilar datos que se generan mediante características y servicios como Update Management. El área de trabajo proporciona una única ubicación para revisar y analizar datos desde varios orígenes.

El proceso de validación también comprueba si la máquina virtual se aprovisiona con Microsoft Monitoring Agent (MMA) y Hybrid Runbook Worker de Azure Automation. Este agente se usa para comunicarse con Azure Automation y para obtener información sobre el estado de la actualización. El agente requiere que se abra el puerto 443 para comunicarse con el servicio Azure Automation y para descargar actualizaciones.

Si se detecta que falta alguno de los siguientes requisitos previos durante la incorporación, estos se agregarán automáticamente:

* Área de trabajo de [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)
* Una [cuenta de Automation](./automation-offering-get-started.md)
* [Hybrid Runbook Worker](./automation-hybrid-runbook-worker.md) (habilitado en la máquina virtual)

En **Update Management**, establezca la ubicación, el área de trabajo de Log Analytics y la cuenta de Automation que use. A continuación, Seleccione **Habilitar**. Si estas opciones no están disponibles, significa que está habilitada otra solución de automatización para la máquina virtual. En ese caso, deben utilizarse la misma cuenta de Automation y área de trabajo.

![Ventana para habilitar la solución Update Management](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

La habilitación de la solución puede tardar unos minutos. Durante este tiempo, no cierre la ventana del explorador. Después de habilitar la solución, la información sobre las actualizaciones que faltan en la máquina virtual se pasa a los registros de Azure Monitor. Los datos pueden tardar entre 30 minutos y 6 horas en estar disponibles para el análisis.

## <a name="view-update-assessment"></a>Ver evaluación de la actualización

Una vez habilitado Update Management, se abre el panel **Update Management**. Si falta alguna actualización, verá una lista de las actualizaciones que faltan en la pestaña **Actualizaciones que faltan**.

En **Vínculo de información**, seleccione el vínculo de información de la actualización para abrir el artículo de soporte técnico sobre la actualización en una ventana nueva. Encontrará información importante acerca de la actualización en esta ventana.

![Ver el estado de la actualización](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Haga clic en cualquier otro lugar de la actualización para abrir el panel **Búsqueda de registros** de la actualización seleccionada. La consulta para la búsqueda de registros está predefinida para esa actualización específica. Puede modificar esta consulta o crear su propia consulta para ver información detallada sobre las actualizaciones implementadas o las que faltan en su entorno.

![Ver el estado de la actualización](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Configurar alertas

En este paso, aprenderá a configurar una alerta, cosa que le permitirá saber el estado de una implementación de actualizaciones.

### <a name="alert-conditions"></a>Condiciones de alerta

En su cuenta de Automation, en **Supervisión** vaya a **Alertas** y, a continuación, haga clic en **+ Nueva regla de alertas**.

La cuenta de Automation ya se ha seleccionado como recurso. Si desea cambiarlo, puede hacer clic en **Seleccionar** y, en la página **Seleccionar un recurso**, seleccionar **Cuentas de Automation** en el menú desplegable **Filtrar por tipo de recurso**. Seleccione su cuenta de Automation y, después, seleccione **Listo**.

Haga clic en **Agregar condición** para seleccionar la señal adecuada para su implementación de actualizaciones. En la siguiente tabla se muestran los detalles de las dos señales disponibles para las implementaciones de actualización:

|Nombre de señal|Dimensiones|DESCRIPCIÓN|
|---|---|---|
|**Ejecuciones de implementación de actualizaciones totales**|- Nombre de implementación de actualizaciones</br>- Estado|Esta señal se usa para alertar sobre el estado general de una implementación de actualizaciones.|
|**Ejecuciones de máquina de implementación de actualizaciones totales**|- Nombre de implementación de actualizaciones</br>- Estado</br>- Equipo de destino</br>- Identificador de ejecución de implementación de actualizaciones|Esta señal se usa para alertar sobre el estado de una implementación de actualizaciones dirigida a máquinas específicas|

Para los valores de dimensión, seleccione un valor válido de la lista. Si el valor que busca no está en la lista, haga clic en el signo **\+** situado junto a la dimensión y escriba el nombre personalizado. A continuación, puede seleccionar el valor que desea buscar. Si desea seleccionar todos los valores de una dimensión, haga clic en el botón **Seleccionar \*** . Si no elige un valor para una dimensión, dicha dimensión se omitirá durante la evaluación.

![Configuración de la lógica de señal](./media/automation-tutorial-update-management/signal-logic.png)

En **Lógica de alerta**, en **Umbral**, escriba **1**. Cuando haya finalizado, seleccione **Listo**.

### <a name="alert-details"></a>Detalles de alertas

En **2. Definir detalles de la alerta**, escriba un nombre y una descripción para la alerta. En **Gravedad**, seleccione **Informativo (gravedad 2)** para ejecuciones correctas, o bien **Informativo (gravedad 1)** para ejecuciones con errores.

![Configuración de la lógica de señal](./media/automation-tutorial-update-management/define-alert-details.png)

En **Grupos de acciones**, seleccione **Crear nuevo**. Un grupo de acciones es un conjunto de acciones que puede usar en varias alertas. Por ejemplo, notificaciones por correo electrónico, runbooks, webhooks y muchas más. Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../azure-monitor/platform/action-groups.md).

En el cuadro **Nombre del grupo de acción**, escriba un nombre para la alerta y un nombre corto. El nombre corto se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.

En **Acciones**, escriba un nombre para la acción, como **Notificaciones por correo electrónico**. En **Tipo de acción**, seleccione **Correo electrónico/SMS/Push/Voz**. En **DETALLES**, seleccione **Editar detalles**.

En el panel **Correo electrónico/SMS/Push/Voz**, escriba un nombre. Seleccione la casilla de verificación **Correo electrónico** y, a continuación, escriba una dirección de correo electrónico válida.

![Configuración de un grupo de acciones de correo electrónico](./media/automation-tutorial-update-management/configure-email-action-group.png)

En el panel **Correo electrónico/SMS/Push/Voz**, seleccione **Aceptar**. En el panel **Agregar grupo de acciones**, seleccione **Aceptar**.

Para personalizar el asunto de las alertas por correo electrónico, en **Crear regla**, en **Personalizar acciones**, seleccione **Asunto de correo electrónico**. Cuando finalice, seleccione **Crear regla de alertas**. La regla le indica cuando la implementación de una actualización es correcta y qué máquinas formaron parte de la ejecución de la implementación de actualizaciones.

## <a name="schedule-an-update-deployment"></a>Programación de una implementación de actualizaciones

A continuación, programe una implementación que se ajuste a la ventana de programación y servicio de su versión para instalar las actualizaciones. Puede elegir los tipos de actualizaciones que quiere incluir en la implementación. Por ejemplo, puede incluir actualizaciones de seguridad o críticas y excluir paquetes acumulativos de actualizaciones.

Para programar una nueva implementación de actualizaciones para la máquina virtual, vaya a **Update Management** y, a continuación, seleccione **Programar implementación de actualizaciones**.

En **Nueva implementación de actualizaciones**, especifique la siguiente información:

* **Nombre**: escriba un nombre único para la implementación de actualizaciones.

* **Sistema operativo**: seleccione el sistema operativo que es el destino de la implementación de actualizaciones.

* **Grupos que se deben actualizar (versión preliminar)** : Defina una consulta basada en una combinación de suscripción, grupos de recursos, ubicaciones y etiquetas para crear un grupo dinámico de VM de Azure e incluirlo en la implementación. Para más información, consulte los [grupos dinámicos](automation-update-management-groups.md).

* **Máquinas para actualizar**: Seleccione una búsqueda guardada, un grupo importado o elija la máquina en la lista desplegable y seleccione equipos individuales. Si elige **Máquinas**, la preparación de la máquina se muestra en la columna **PREPARACIÓN DE ACTUALIZACIONES DEL AGENTE**. Para información sobre los distintos métodos de creación de grupos de equipos en los registros de Azure Monitor, consulte el artículo sobre los [Grupos de equipos en los registros de Azure Monitor](../azure-monitor/platform/computer-groups.md)

* **Actualizar clasificación**: seleccione los tipos de software que la implementación de actualizaciones incluyó en la implementación. Para este tutorial, seleccione todos los tipos.

  Los tipos de clasificación son:

   |OS  |type  |
   |---------|---------|
   |Windows     | Actualizaciones críticas</br>Actualizaciones de seguridad</br>Paquetes acumulativos de actualizaciones</br>Feature Packs</br>Service Packs</br>Actualizaciones de definiciones</br>Herramientas</br>Actualizaciones        |
   |Linux     | Actualizaciones críticas y de seguridad</br>Otras actualizaciones       |

   Para ver una descripción de los tipos de clasificación, consulte [Actualización de clasificaciones](automation-view-update-assessments.md#update-classifications).

* **Actualizaciones para incluir/excluir**: abre la página para **incluir/excluir**. Las actualizaciones que se incluirán o excluirán están en pestañas independientes.

> [!NOTE]
> Es importante saber que las exclusiones invalidan las inclusiones. Por ejemplo, si define una regla de exclusión de `*`, no se instalan revisiones ni paquetes, ya que se excluyen todas. Las revisiones excluidas aparecen todavía como que faltan en la máquina. Para las máquinas Linux, si se incluye un paquete, pero tiene un paquete dependiente que se ha excluido, el paquete no se instala.

* **Configuración de la programación**: se abre el panel de **configuración de la programación**. La hora de inicio predeterminada es 30 minutos después de la hora actual. Puede establecer la hora de inicio en cualquier momento a partir de 10 minutos en el futuro.

   También puede especificar si la implementación se produce una vez o configurar una programación periódica. Seleccione **Una vez** en **Periodicidad**. Deje el valor predeterminado de 1 día y seleccione **Aceptar**. Esta acción configura una programación periódica.

* **Scripts previos + scripts posteriores**: seleccione los scripts que se ejecutarán antes y después de la implementación. Para obtener más información, consulte [Administración de scripts previos y posteriores](pre-post-scripts.md).

* **Ventana de mantenimiento (minutos)** : Deje el valor predeterminado. Las de ventanas de mantenimiento controlan la cantidad de tiempo permitido para que se instalen las actualizaciones. Tenga en cuenta los siguientes detalles al especificar una ventana de mantenimiento.

  * Las ventanas de mantenimiento controlan el número de actualizaciones que se intentan instalar.
  * Update Management no detiene la instalación de nuevas actualizaciones si se está aproximando el final de una ventana de mantenimiento.
  * Update Management no finaliza las actualizaciones en curso si se supera la ventana de mantenimiento.
  * Si la ventana de mantenimiento se supera en Windows, suele deberse a que una actualización de Service Pack tarda mucho tiempo en instalarse.

  > [!NOTE]
  > Para evitar que las actualizaciones se apliquen fuera de una ventana de mantenimiento en Ubuntu, vuelva a configurar el paquete de actualizaciones desatendidas para deshabilitar las actualizaciones automáticas. Para más información sobre cómo configurar el paquete, consulte el tema [Actualizaciones automáticas](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) en la Guía de Ubuntu Server.

* **Opciones de reinicio**: este valor determina cómo deben controlarse los reinicios. Las opciones disponibles son la siguientes:
  * Reboot if required (Default) [Reiniciar si es necesario (predeterminada)]
  * Always reboot (Reiniciar siempre)
  * Never reboot (No reiniciar nunca)
  * Only reboot (solo reiniciar), no se instalarán las actualizaciones

> [!NOTE]
> Las claves del Registro mostradas en la sección sobre las [claves del Registro usadas para administrar el reinicio](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) pueden provocar un evento de reinicio si **Control de reinicio** se establece en **No reiniciar nunca**.

Cuando haya terminado de configurar la programación, seleccione **Crear**.

![Panel de configuración de la programación de actualizaciones](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Volverá al panel de estado. Seleccione **Implementaciones de actualizaciones programadas** para mostrar la programación de implementación que creó.

> [!NOTE]
> Update Management admite la implementación de actualizaciones de origen y la descarga previa de revisiones. Esto requiere cambios en los sistemas a los que se aplican revisiones. Para aprender a configurar estos valores en sus sistemas, consulte [soporte técnico para propios y para descargas previas](automation-configure-windows-update.md).

Las **implementaciones de actualizaciones** también se pueden crear mediante programación. Para información sobre cómo crear una **implementación de actualizaciones** con la API REST, consulte [Creación de configuraciones de actualización de software](/rest/api/automation/softwareupdateconfigurations/create). También hay un runbook de ejemplo que puede usarse para crear una **implementación de actualizaciones** semanal. Para más información acerca de este runbook, consulte [Create a weekly update deployment for one or more VMs in a resource group](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) (Crear una implementación de actualización semanal para una o más VM en un grupo de recursos).

## <a name="view-results-of-an-update-deployment"></a>Visualización de los resultados de una implementación de actualizaciones

Después de que se inicie la implementación programada, puede ver su estado en la pestaña **Implementaciones de actualizaciones** en **Update Management**. Si la implementación se está ejecutando, su estado es **En curso**. Cuando finalice la implementación, si se realiza correctamente, el estado cambia a **Correcto**. Si se producen errores con una o varias actualizaciones en la implementación, el estado es **Error parcial**.

Seleccione la implementación de actualizaciones completada para ver el panel correspondiente.

![Panel de estado de implementación de actualizaciones de la implementación específica](./media/automation-tutorial-update-management/manageupdates-view-results.png)

En **Resultados de actualización** encontrará un resumen del número total de actualizaciones y los resultados de la implementación en la máquina virtual. En la tabla de la derecha se muestra un análisis detallado de cada actualización y los resultados de la instalación.

En la lista siguiente se muestran los valores disponibles:

* **No intentado**: la actualización no se instaló porque no había tiempo disponible suficiente de acuerdo con la duración definida para la ventana de mantenimiento.
* **Correcto**: actualización realizada correctamente.
* **Error**: error en la actualización.

Seleccione **Todos los registros** para ver todas las entradas de registro que creó la implementación.

Seleccione **Salida** para ver el flujo de trabajo del runbook responsable de administrar la implementación de actualizaciones en la máquina virtual de destino.

Seleccione **Errores** para ver información detallada acerca de los errores de la implementación.

Una vez que la implementación de actualizaciones se realiza correctamente, se envía un correo electrónico similar al del siguiente ejemplo para mostrar la correcta realización de la implementación:

![Configuración del grupo de acciones de correo electrónico](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Incorporar máquinas virtuales a Update Management
> * Visualización de la evaluación de la actualización
> * Configurar alertas
> * Programación de una implementación de actualizaciones
> * Ver los resultados de una implementación

Continúe hacia la introducción sobre la solución Update Management.

> [!div class="nextstepaction"]
> [Solución Update Management](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)

