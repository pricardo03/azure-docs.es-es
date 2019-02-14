---
title: Solución Start/Stop VMs during off-hours
description: Esta solución de administración de máquinas virtuales inicia y detiene máquinas virtuales de Azure Resource Manager según una programación y realiza una supervisión proactiva desde Log Analytics.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 1/30/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 0473bccbd249f70139d815b8353f1ac271df754f
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55658393"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Solución Start/Stop VMs during off-hours en Azure Automation

La solución Start/Stop VMs during off-hours inicia y detiene las máquinas virtuales de Azure según las programaciones definidas por el usuario, proporciona información detallada mediante Azure Log Analytics y envía mensajes de correo electrónico opcionales mediante [grupos de acciones](../azure-monitor/platform/action-groups.md). Admite tanto máquinas virtuales clásicas como Azure Resource Manager para la mayoría de los escenarios.

Esta solución proporciona una opción de automatización descentralizada de bajo costo para los usuarios que desean optimizar sus costos de máquinas virtuales. Con esta solución, es posible:

- Programar el inicio y la detención de las máquinas virtuales.
- Programar el inicio y la detención de las máquinas virtuales mediante etiquetas de Azure (no se admite en las máquinas virtuales clásicas).
- Detener automáticamente las máquinas virtuales si se detecta un bajo uso de la CPU

Las siguientes son limitaciones a la solución actual:

- Esta solución administrar máquinas virtuales de cualquier región, pero solo se puede utilizar en la misma suscripción que la cuenta de Azure Automation.
- Esta solución está disponible en Azure y AzureGov para cualquier región que admita un área de trabajo de Log Analytics, una cuenta de Azure Automation y alertas. Las regiones de AzureGov no admiten la funcionalidad de correo electrónico en este momento.

> [!NOTE]
> Si usa la solución para las máquinas virtuales clásicas, todas las máquinas virtuales se procesarán secuencialmente por servicio en la nube. Las máquinas virtuales aún se procesarán en paralelo entre diferentes servicios en la nube.
>
> Las suscripciones de Proveedor de soluciones en la nube de Azure (Azure CSP) solo admiten el modelo de Azure Resource Manager, los servicios que no sean de Azure Resource Manager no están disponibles en el programa. Cuando se ejecuta la solución Start/Stop, puede recibir errores porque contiene cmdlets para administrar los recursos clásicos. Para más información sobre CSP, consulte [Servicios disponibles en las suscripciones de CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services#comments). Si usa una suscripción de CSP, debe modificar la variable [**External_EnableClassicVMs**](#variables) a **False** después de la implementación.

## <a name="prerequisites"></a>Requisitos previos

Los runbooks para esta solución funcionan con una [cuenta de ejecución de Azure](automation-create-runas-account.md). La cuenta de ejecución es el método de autenticación preferido, ya que emplea la autenticación mediante certificado, en lugar de una contraseña que puede expirar o cambiar con frecuencia.

## <a name="deploy-the-solution"></a>Implementación de la solución

Realice los siguientes pasos para agregar la solución Start/Stop VMs during off-hours a su cuenta de Automation y, después, configure las variables para personalizar la solución.

1. En una cuenta de Automation, seleccione **Start/Stop VM** en **Recursos relacionados**. Desde aquí, puede hacer clic en **Más información acerca de la solución y cómo habilitarla**. Si ya tiene una solución Start/Stop VM implementada, puede seleccionarla; para ello, haga clic en **Administrar la solución** y búsquela en la lista.

   ![Habilitación desde una cuenta de Automation](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > También puede crearla desde cualquier lugar en Azure Portal; para ello, haga clic en **Crear un recurso**. En la página de Marketplace, escriba una palabra clave como **Iniciar** o **Inciar/Detener**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Como alternativa, puede escribir en una o varias palabras clave del nombre completo de la solución y presione Entrar. Seleccione **Start/Stop VMs during off-hours** en los resultados de la búsqueda.
2. En la página **Start/Stop VMs during off-hours**, revise la información de resumen y luego haga clic en **Crear**.

   ![Azure Portal](media/automation-solution-vm-management/azure-portal-01.png)

3. Aparece la página **Agregar solución**. Se le pide que configure la solución antes de importarla en la suscripción de Automation.

   ![Página Agregar solución de administración de VM](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

4. En el la página **Agregar solución**, seleccione **Área de trabajo**. Seleccione un área de trabajo de Log Analytics que esté vinculada a la suscripción de Azure en la que se encuentra la cuenta de Automation. Si no tiene ningún área de trabajo, seleccione **Crear nueva área de trabajo**. En la página **Área de trabajo de Log Analytics**, siga estos pasos:
   - Especifique un nombre para la nueva **área de trabajo de Log Analytics** como, por ejemplo, "ContosoLAWorkspace".
   - Seleccione la **suscripción** a la que vincularlo en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
   - En **Grupo de recursos**, puede crear un grupo de recursos nuevo o seleccionar uno existente.
   - Seleccione una **ubicación**. Actualmente, las únicas ubicaciones disponibles son: **Sudeste de Australia**, **Centro de Canadá**, **Centro de la India**, **Este de EE. UU.**, **Japón Oriental**, **Sudeste Asiático**, **Sur de Reino Unido**, **Europa Occidental** y **Oeste de EE. UU. 2**.
   - Seleccione un **plan de tarifa**. Elija la opción **Por GB (independiente)**. Log Analytics ha actualizado [precios](https://azure.microsoft.com/pricing/details/log-analytics/) y el nivel Por GB es la única opción.

5. Después de proporcionar la información necesaria en la página **Área de trabajo de Log Analytics**, haga clic en **Crear**. Se puede realizar un seguimiento de su progreso en la opción **Notificaciones** del menú, que le devuelve a la página **Agregar solución** cuando haya finalizado.
6. En la página **Agregar solución**, seleccione **Cuenta de Automation**. Si va a crear una nueva área de trabajo de Log Analytics, puede crear una cuenta de Automation para asociarla, o seleccionar una cuenta de Automation existente que no esté ya vinculada a un área de trabajo de Log Analytics. Seleccione una cuenta de Automation existente o haga clic en **Crear una cuenta de Automation** y, en la página **Agregar cuenta de Automation**, especifique la siguiente información:
   - En el campo **Nombre**, escriba el nombre de la cuenta de Automation.

    Las restantes opciones se rellenan automáticamente en función del área de trabajo de Log Analytics seleccionada. Dichas opciones no se pueden modificar. Una cuenta de ejecución de Azure es el método de autenticación predeterminado para los Runbooks incluidos en esta solución. Después de hacer clic en **Aceptar**, se validan las opciones de configuración y se crea la cuenta de Automation. Puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

7. Por último, en la página **Agregar solución**, seleccione **Configuración**. Aparece la página **Parámetros**.

   ![Página Parámetros para la solución](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   En este caso, se le pedirá que:
   - Especifique los **nombres del grupo de recursos de destino**. Estos valores son nombres de grupos de recursos que contienen máquinas virtuales que se administrarán mediante esta solución. Puede especificar más de un nombre y separarlos con una coma (los valores no distinguen mayúsculas de minúsculas). Se puede usar un carácter comodín si quiere dirigirse a las máquinas virtuales de todos los grupos de recursos de la suscripción. Este valor se almacena en las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames**.
   - Especifique el **lista de exclusiones de máquinas virtuales (cadena)**. Este valor es el nombre de una o varias máquinas virtuales del grupo de recursos de destino. Puede especificar más de un nombre y separarlos con una coma (los valores no distinguen mayúsculas de minúsculas). Se admite el uso de un carácter comodín. Este valor se almacena en la variable **External_ExcludeVMNames**.
   - Seleccione una **programación**. Este valor es la fecha y hora recurrentes para iniciar y detener las máquinas virtuales en los grupos de recursos de destino. De forma predeterminada, la programación se configura para 30 minutos a partir de entonces. No se puede seleccionar otra región. Para configurar su zona horaria concreta después de configurar la solución, consulte el siguiente artículo [Modificación de la programación de inicio y apagado](#modify-the-startup-and-shutdown-schedules).
   - Para recibir **notificaciones por correo electrónico** de un grupo de acciones, acepte el valor predeterminado **Sí** y proporcione una dirección de correo electrónico válida. Si selecciona **No** pero decide en un momento posterior que desea recibir notificaciones por correo electrónico, puede actualizar el [grupo de acciones](../azure-monitor/platform/action-groups.md) que se crea con las direcciones de correo electrónico válidas separadas por un punto y coma. También deberá habilitar las reglas de alerta siguientes:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > El valor predeterminado de los **nombres del grupo de recursos de destino** es **&ast;**. Este se dirige a todas las máquinas virtuales de una suscripción. Si no desea que la solución se dirija a todas las máquinas virtuales de su suscripción, este valor debe actualizarse a una lista de nombres del grupo de recursos antes de habilitar las programaciones.

8. Cuando haya configurado los valores iniciales necesarios para la solución, haga clic en **Aceptar** para cerrar la página **Parámetros** y seleccione **Crear**. Cuando todos los valores se hayan validado, la solución se implementa en su suscripción. Este proceso puede tardar varios segundos en finalizar y se puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

> [!NOTE]
> Si tiene una suscripción de Azure Cloud Solution Provider (CSP de Azure), una vez completada la implementación, acceda a su cuenta de Automation, vaya a **Variables** en **Shared Resources** (Recursos compartidos) y establezca la variable [**External_EnableClassicVMs**](#variables) en **False**. Esta operación detiene la solución para que deje de buscar recursos de la máquina virtual clásica.

## <a name="scenarios"></a>Escenarios

La solución contiene tres escenarios diferentes. Dichos escenarios son:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>Escenario 1: Iniciar o detener las máquinas virtuales según una programación

Este escenario es la configuración predeterminada cuando se implementa la solución por primera vez. Por ejemplo, puede configurarla para que detenga todas las máquinas virtuales de una suscripción por la noche, cuando salga del trabajo, e iniciarlas por la mañana cuando vuelva a la oficina. Al configurar las programaciones **Scheduled-StartVM** y **Scheduled-StopVM** durante la implementación, inician y detienen las máquinas virtuales de destino. Se admite la configuración de esta solución para simplemente detener las máquinas virtuales. Consulte [Modificación de las programaciones de inicio y apagado](#modify-the-startup-and-shutdown-schedules) para información sobre cómo configurar una programación personalizada.

> [!NOTE]
> La zona horaria es su zona horaria actual cuando configura el parámetro de hora de la programación. Sin embargo, en Azure Automation se almacena en formato UTC. No es necesario realizar ninguna conversión de zona horaria ya que se controla durante la implementación.

Mediante la configuración de las siguientes variables, controlará qué máquinas virtuales están en el ámbito: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** y **External_ExcludeVMNames**.

Puede habilitar que el destino de la acción sea una suscripción y un grupo de recursos, o una lista específica de máquinas virtuales, pero no ambas cosas.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Destino de las acciones de inicio y detención a un grupo de recursos y una suscripción

1. Configure las variables **External_Stop_ResourceGroupNames** y **External_ExcludeVMNames** para especificar las máquinas virtuales de destino.
2. Habilite y actualice las programaciones **Scheduled-StartVM** y **Scheduled-StopVM**.
3. Ejecute el runbook **ScheduledStartStop_Parent** con el parámetro ACTION establecido en **start** y el parámetro WHATIF establecido en **True** para obtener una vista previa de los cambios.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Destino de las acciones de inicio y detención por lista de máquinas virtuales

1. Ejecute el runbook **ScheduledStartStop_Parent** con el parámetro ACTION establecido en **start**, agregue una lista separada por comas de máquinas virtuales en el parámetro *VMList* y establezca el parámetro WHATIF en **True**. Obtenga una vista previa de los cambios.
1. Configure el parámetro **External_ExcludeVMNames** con una lista separada por comas de máquinas virtuales (VM1, VM2, VM3).
1. Este escenario no respeta las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupnames**. Para este escenario, es preciso que cree su propia programación de Automation. Para más información, consulte [Programación de un runbook en Azure Automation](../automation/automation-schedules.md).

> [!NOTE]
> El valor de **Target ResourceGroup Names** se almacena como el valor tanto de **External_Start_ResourceGroupNames** como de **External_Stop_ResourceGroupNames**. A fin de lograr un mayor detalle, puede modificar cada una de estas variables para distintos grupos de recursos de destino. Para la acción de inicio, use **External_Start_ResourceGroupNames** y para la acción de detención use **External_Stop_ResourceGroupNames**. Las máquinas virtuales se agregan automáticamente a las programaciones de inicio y detención.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Escenario 2: Iniciar o detener las máquinas virtuales en secuencia mediante el uso de etiquetas

En un entorno que incluya dos, o más, componentes de varias máquinas virtuales que admitan una carga de trabajo distribuida, es importante que se pueda decidir la secuencia en que los componentes se inician o detienen en orden. Para realizar este escenario, lleve a cabo los siguientes pasos:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Destino de las acciones de inicio y detención a un grupo de recursos y una suscripción

1. Agregue una etiqueta **sequencestart** y **sequencestop** con un valor entero positivo a las máquinas virtuales cuyo destino se establece en las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames**. Las acciones de inicio y detención se realizan en orden ascendente. Para aprender a etiquetar una máquina virtual, consulte [Etiquetado de una máquina virtual en Azure](../virtual-machines/windows/tag.md) y [Etiquetado de una máquina virtual Linux en Azure](../virtual-machines/linux/tag.md).
1. Modifique las programaciones de **Sequenced-StartVM** y **Sequenced-StopVM** a una fecha y hora que cumplan sus requisitos, y habilite la programación.
1. Ejecute el runbook **SequencedStartStop_Parent** con el parámetro ACTION establecido en **start** y el parámetro WHATIF establecido en **True** para obtener una vista previa de los cambios.
1. Obtenga una vista previa de la acción y realice los cambios necesarios antes de implementarla en las máquinas virtuales de producción. Cuando esté preparado, ejecute manualmente el runbook con el parámetro establecido en **False** o deje que la programación de Automation **Sequenced-StartVM** y **Sequenced-StopVM** se ejecuten automáticamente siguiendo su programación prescrita.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Destino de las acciones de inicio y detención por lista de máquinas virtuales

1. Agregue una etiqueta **sequencestart** y **sequencestop** con un valor entero positivo para las VM que pretende agregar al parámetro **VMList**.
1. Ejecute el runbook **SequencedStartStop_Parent** con el parámetro ACTION establecido en **start**, agregue una lista separada por comas de máquinas virtuales en el parámetro *VMList* y establezca el parámetro WHATIF en **True**. Obtenga una vista previa de los cambios.
1. Configure el parámetro **External_ExcludeVMNames** con una lista separada por comas de máquinas virtuales (VM1, VM2, VM3).
1. Este escenario no respeta las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupnames**. Para este escenario, es preciso que cree su propia programación de Automation. Para más información, consulte [Programación de un runbook en Azure Automation](../automation/automation-schedules.md).
1. Obtenga una vista previa de la acción y realice los cambios necesarios antes de implementarla en las máquinas virtuales de producción. Cuando esté preparado, ejecute manualmente el runbook monitoring-and-diagnostics/monitoring-action-groups con el parámetro establecido en **False** o deje que la programación de Automation **Sequenced-StartVM** y **Sequenced-StopVM** se ejecuten automáticamente siguiendo su programación prescrita.

### <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a>Escenario 3: Iniciar o detener automáticamente según el uso de CPU

Esta solución puede ayudarle a administrar el costo de la ejecución de máquinas virtuales en su suscripción mediante la evaluación de las máquinas virtuales de Azure que no se utilizan durante los períodos de poca actividad, por ejemplo, las horas no laborables, y apagarlas automáticamente si el uso del procesador es menor de un porcentaje concreto.

De forma predeterminada, la solución está preconfigurada para evaluar la métrica de CPU en porcentaje, con el fin de ver si la utilización media es del 5 %, o menos. Este escenario se controla mediante las siguientes variables y puede modificarse si los valores predeterminados no satisfacen sus requisitos:

- External_AutoStop_MetricName
- External_AutoStop_Threshold
- External_AutoStop_TimeAggregationOperator
- External_AutoStop_TimeWindow

Puede habilitar que el destino de la acción sea una suscripción y un grupo de recursos, o una lista específica de máquinas virtuales, pero no ambas cosas.

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Destino de la acción de detención a un grupo de recursos y una suscripción

1. Configure las variables **External_Stop_ResourceGroupNames** y **External_ExcludeVMNames** para especificar las máquinas virtuales de destino.
1. Habilite y actualice la programación de **Schedule_AutoStop_CreateAlert_Parent**.
1. Ejecute el runbook **AutoStop_CreateAlert_Parent** con el parámetro ACTION establecido en **start** y el parámetro WHATIF establecido en **True** para obtener una vista previa de los cambios.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Destino de las acciones de inicio y detención por lista de máquinas virtuales

1. Ejecute el runbook **AutoStop_CreateAlert_Parent** con el parámetro ACTION establecido en **start**, agregue una lista separada por comas de máquinas virtuales en el parámetro *VMList* y establezca el parámetro WHATIF en **True**. Obtenga una vista previa de los cambios.
1. Configure el parámetro **External_ExcludeVMNames** con una lista separada por comas de máquinas virtuales (VM1, VM2, VM3).
1. Este escenario no respeta las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupnames**. Para este escenario, es preciso que cree su propia programación de Automation. Para más información, consulte [Programación de un runbook en Azure Automation](../automation/automation-schedules.md).

Ahora que tiene una programación para detener las máquinas virtuales en función del uso de la CPU, debe habilitar una de las programaciones siguientes para iniciarlas.

- Elija el destino de acción de inicio por suscripción y grupo de recursos. Vea los pasos descritos en [Escenario 1](#scenario-1-startstop-vms-on-a-schedule) para realizar las pruebas y habilitar las programaciones de **Scheduled-StartVM**.
- Elija el destino de acción de inicio por suscripción, grupo de recursos y etiqueta. Vea los pasos descritos en [Escenario 2](#scenario-2-startstop-vms-in-sequence-by-using-tags) para realizar las pruebas y habilitar la programación **Sequenced-StartVM**.

## <a name="solution-components"></a>Componentes de soluciones

Esta solución incluye runbooks preconfigurados, programaciones e integración con Log Analytics para que pueda personalizar el inicio y el apagado de las máquinas virtuales para adaptarlas a las necesidades de su empresa.

### <a name="runbooks"></a>Runbooks

En la tabla siguiente se enumeran los runbooks implementados en su cuenta de Automation por esta solución. No realice cambios en el código del runbook. En su lugar, escriba un runbook que aporte una nueva funcionalidad.

> [!IMPORTANT]
> No ejecute directamente ningún runbook cuyo nombre tenga anexado "child".

Todos los runbooks primarios incluyen el parámetro All _WhatIf_. Cuando se establece en **True**, _Whatif_ permite detallar el comportamiento exacto del runbook cuando se ejecuta sin el parámetro _WhatIf_ y valida que el destino son las máquinas virtuales correctas. Los runbooks solo realizan sus acciones definidas cuando el parámetro _WhatIf_ está establecido en **False**.

|Runbook | Parámetros | DESCRIPCIÓN|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Se llama desde el runbook primario. Este runbook crea alertas según el recurso para el escenario de AutoStop.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: Verdadero o falso  | Crea o actualiza las reglas de alerta de Azure en las máquinas virtuales de los grupos de recursos o la suscripción de destino. <br> VMList: lista de máquinas virtuales separadas por comas. Por ejemplo, _vm1, vm2, vm3_.<br> *WhatIf* valida la lógica de runbook sin ejecución.|
|AutoStop_Disable | None | Deshabilita la programación predeterminada y las alertas de AutoStop.|
|AutoStop_StopVM_Child | WebHookData | Se llama desde el runbook primario. Las reglas de alertas llaman a este runbook para detener la máquina virtual.|
|Bootstrap_Main | None | Se usa una vez para realizar configuraciones de arranque como webhookURI, a las que normalmente no se puede acceder desde Azure Resource Manager. Este runbook se quita automáticamente tras la implementación correcta.|
|ScheduledStartStop_Child | VMName <br> Acción: Start o Stop <br> ResourceGroupName | Se llama desde el runbook primario. Ejecuta una acción de inicio o detención para la detención programada.|
|ScheduledStartStop_Parent | Acción: Start o Stop <br>VMList <br> WhatIf: Verdadero o falso | Este valor afecta a todas las máquinas virtuales de la suscripción. Edite **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames** para que se ejecute solo en estos grupos de recursos de destino. También puede excluir máquinas virtuales específicas si actualiza la variable **External_ExcludeVMNames**.<br> VMList: lista de máquinas virtuales separadas por comas. Por ejemplo, _vm1, vm2, vm3_.<br> _WhatIf_ valida la lógica de runbook sin ejecución.|
|SequencedStartStop_Parent | Acción: Start o Stop <br> WhatIf: Verdadero o falso<br>VMList| Cree etiquetas denominadas **sequencestart** y **sequencestop** en todas las máquinas virtuales en las que quiera secuenciar la actividad de inicio y detención. Estos nombres de etiqueta distinguen entre mayúsculas y minúsculas. El valor de la etiqueta debe ser un entero positivo (1, 2, 3) que se corresponda con el orden en que se desee que se realice el inicio o la detención. <br> VMList: lista de máquinas virtuales separadas por comas. Por ejemplo, _vm1, vm2, vm3_. <br> _WhatIf_ valida la lógica de runbook sin ejecución. <br> **Nota**: Las máquinas virtuales deben estar en grupos de recursos definidos como External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames y External_ExcludeVMNames, en las variables de Azure Automation. Para que las acciones surtan efecto, deben tener las etiquetas correspondientes.|

### <a name="variables"></a>variables

En la tabla siguiente se enumeran las variables creadas en su cuenta de Automation. Modifique solo las variables cuyo prefijo sea **External**. La modificación de variables con el prefijo **Internal** produce efectos no deseados.

|Variable | DESCRIPCIÓN|
|---------|------------|
|External_AutoStop_Condition | El operador condicional necesario para configurar la condición antes de desencadenar una alerta. Los valores que se aceptan son **GreaterThan**, **GreaterThanOrEqual**, **LessThan** y **LessThanOrEqual**.|
|External_AutoStop_Description | Alerta para detener la máquina virtual si el porcentaje de CPU supera el umbral.|
|External_AutoStop_MetricName | El nombre de la métrica de rendimiento para el que se configura la regla de alertas de Azure.|
|External_AutoStop_Threshold | Umbral para la regla de alertas de Azure especificada en la variable _External_AutoStop_MetricName_. Los valores de porcentaje pueden oscilar entre 1 y 100.|
|External_AutoStop_TimeAggregationOperator | El operador de agregación de tiempo, que se aplica al tamaño de la ventana seleccionada para evaluar la condición. Los valores que se aceptan son **Promedio**, **Mínimo**, **Máximo**, **Total** y **Último**.|
|External_AutoStop_TimeWindow | El tamaño de la ventana en la que Azure analiza la métrica seleccionada para desencadenar una alerta. Este parámetro acepta la entrada en formato timespan. Los valores posibles son de 5 minutos a 6 horas.|
|External_EnableClassicVMs| Especifica si la solución se centra en las máquinas virtuales clásicas. El valor predeterminado es True. Este debe establecerse en False para las suscripciones de CSP.|
|External_ExcludeVMNames | Escriba los nombres de máquina virtual que se van a excluir y sepárelos con una coma sin espacios en blanco.|
|External_Start_ResourceGroupNames | Especifica uno o varios grupos de recursos y separa los valores con una coma, destinados a las acciones de inicio.|
|External_Stop_ResourceGroupNames | Especifica uno o varios grupos de recursos y separa los valores con una coma, destinados a las acciones de detención.|
|Internal_AutomationAccountName | Especifica el nombre de la cuenta de Automation.|
|Internal_AutoSnooze_WebhookUri | Especifica el URI de Webhook llamado para el escenario de AutoStop.|
|Internal_AzureSubscriptionId | Especifica el identificador de la suscripción de Azure.|
|Internal_ResourceGroupName | Especifica el nombre el grupo de recursos de la cuenta de Automation.|

En todos los escenarios, las variables **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** y **External_ExcludeVMNames** son necesarias para dirigirse a las máquinas virtuales, aunque hay que proporcionar una lista separada por comas de las máquinas virtuales para los runbooks **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent** y **ScheduledStartStop_Parent**. Es decir, para que se realicen acciones de iniciar o detener tengan, las máquinas virtuales deben residir en el recurso de destino. La lógica funciona de forma similar a la directiva de Azure, en el sentido de que el destino puede ser la suscripción o el grupo de recursos, y las acciones las pueden heredar las máquinas virtuales recién creadas. Este enfoque evita tener que mantener una programación independiente para cada máquina virtual y administrar los inicios y detenciones en escala.

### <a name="schedules"></a>Programaciones

En la tabla siguiente se enumera cada una de las programaciones predeterminadas que se crean en su cuenta de Automation. Puede modificarlas o crear sus propias programaciones personalizadas. De forma predeterminada, todas las programaciones están deshabilitadas, excepto **Scheduled_StartVM** y **Scheduled_StopVM**.

No se deben habilitar todas las programaciones, ya que ello podría crear acciones de programación que se superpongan. Lo mejor es determinar las optimizaciones que se desean y realizar las modificaciones oportunas. Vea los escenarios de ejemplo en la sección de información general para obtener una explicación.

|Nombre de programación | Frecuencia | DESCRIPCIÓN|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Cada ocho horas | Ejecuta el runbook AutoStop_CreateAlert_Parent cada ocho horas, que a su vez detiene los valores basados en la máquina virtual en External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames y External_ExcludeVMNames en las variables de Azure Automation. Como alternativa, puede especificar una lista separada por comas de las máquinas virtuales mediante el parámetro VMList.|
|Scheduled_StopVM | Definido por el usuario, diario | Ejecuta el runbook Scheduled_Parent con un parámetro de _Stop_ cada día a la hora especificada. Detiene automáticamente todas las máquinas virtuales que cumplan las reglas definidas por las variables de los recursos. Habilite la programación relacionada, **Scheduled-StartVM**.|
|Scheduled_StartVM | Definido por el usuario, diario | Ejecuta el runbook Scheduled_Parent con un parámetro de _Start_ cada día a la hora especificada. Inicia automáticamente todas las máquinas virtuales que cumplan las reglas definidas por las variables adecuadas. Habilite la programación relacionada, **Scheduled-StopVM**.|
|Sequenced-StopVM | 1:00 AM (UTC), todos los viernes | Ejecuta el runbook Sequenced_Parent con un parámetro de _Stop_ todos los viernes a la hora especificada. Detiene secuencialmente (de forma ascendente) todas las máquinas virtuales que tengan la etiqueta **SequenceStop** definida por las variables adecuadas. Para más información sobre los valores de etiqueta y las variables de recurso, consulte la sección Runbooks. Habilite la programación relacionada, **Sequenced-StartVM**.|
|Sequenced-StartVM | 1:00 PM (hora UTC), todos los lunes | Ejecuta el runbook Sequenced_Parent con un parámetro de _Start_ cada lunes a la hora determinada. Inicia secuencialmente (de forma descendente) todas las máquinas virtuales que tengan la etiqueta **SequenceStart** definida por las variables adecuadas. Para más información sobre los valores de etiqueta y las variables de recurso, consulte la sección Runbooks. Habilite la programación relacionada, **Sequenced-StopVM**.|

## <a name="log-analytics-records"></a>Registros de Log Analytics

Automation crea dos tipos de registros en el área de trabajo de Log Analytics: registros de trabajo y flujos de trabajo.

### <a name="job-logs"></a>Registros de trabajo

|Propiedad | DESCRIPCIÓN|
|----------|----------|
|Autor de llamada |  La persona que inicia la operación. Los valores posibles son una dirección de correo electrónico o el sistema para los trabajos programados.|
|Categoría | Clasificación del tipo de datos. Para Automation, el valor será JobLogs.|
|CorrelationId | GUID que es el identificador de correlación del trabajo de Runbook.|
|JobId | GUID que es el identificador del trabajo de Runbook.|
|operationName | Especifica el tipo de operación realizada en Azure. En Automation, el valor es Job.|
|ResourceId | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|ResourceGroup | Especifica el nombre del grupo de recursos del trabajo de Runbook.|
|ResourceProvider | Especifica el servicio de Azure que proporciona los recursos que puede implementar y administrar. Para Automation, el valor es Azure Automation.|
|ResourceType | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|resultType | El estado del trabajo de Runbook. Los valores posibles son:<br>Started<br>Stopped<br>Suspended<br>Con error<br>- Correcto|
|resultDescription | Describe el estado de resultado del trabajo de Runbook. Los valores posibles son:<br>- Se inicia el trabajo<br>- Error del trabajo<br>- Trabajo completado|
|RunbookName | Especifica el nombre del Runbook.|
|SourceSystem | Especifica el sistema de origen para los datos enviados. En Automation, el valor es OpsManager|
|StreamType | Especifica el tipo de evento. Los valores posibles son:<br>- Detallado<br>- Salida<br>error<br>Warning (Advertencia)|
|SubscriptionId | Especifica el identificador de suscripción del trabajo.
|Hora | Fecha y hora en que se ejecuta el trabajo de Runbook.|

### <a name="job-streams"></a>Transmisiones de trabajo

|Propiedad | DESCRIPCIÓN|
|----------|----------|
|Autor de llamada |  La persona que inicia la operación. Los valores posibles son una dirección de correo electrónico o el sistema para los trabajos programados.|
|Categoría | Clasificación del tipo de datos. Para Automation, el valor es JobStreams.|
|JobId | GUID que es el identificador del trabajo de Runbook.|
|operationName | Especifica el tipo de operación realizada en Azure. En Automation, el valor es Job.|
|ResourceGroup | Especifica el nombre del grupo de recursos del trabajo de Runbook.|
|ResourceId | Especifica el identificador de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|ResourceProvider | Especifica el servicio de Azure que proporciona los recursos que puede implementar y administrar. Para Automation, el valor es Azure Automation.|
|ResourceType | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|resultType | El resultado del trabajo de Runbook en el momento en que se generó el evento. Un valor posible es:<br>- En curso|
|resultDescription | Incluye la secuencia de salida del Runbook.|
|RunbookName | El nombre del Runbook.|
|SourceSystem | Especifica el sistema de origen para los datos enviados. En Automation, el valor es OpsManager.|
|StreamType | El tipo de flujo de trabajo. Los valores posibles son:<br>- Progreso<br>- Salida<br>Warning (Advertencia)<br>Error<br>DEBUG<br>- Detallado|
|Hora | Fecha y hora en que se ejecuta el trabajo de Runbook.|

Cuando se realiza cualquier búsqueda de registros que devuelve registros de la categoría **JobLogs** o **JobStreams**, puede seleccionar las vistas **JobLogs** o **JobStreams**, que muestran un conjunto de iconos que resumen las actualizaciones devueltas en la búsqueda.

## <a name="sample-log-searches"></a>Búsquedas de registros de ejemplo

En la tabla siguiente se proporcionan búsquedas de registros de ejemplo para los registros de trabajo recopilados por esta solución.

|Consultar | DESCRIPCIÓN|
|----------|----------|
|Búsqueda de trabajos del runbook ScheduledStartStop_Parent que hayan finalizado correctamente | ```search Category == "JobLogs" | where ( RunbookName_s == "ScheduledStartStop_Parent" ) | where ( ResultType == "Completed" )  | summarize |AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) | sort by TimeGenerated desc```|
|Búsqueda de trabajos del runbook SequencedStartStop_Parent que hayan finalizado correctamente | ```search Category == "JobLogs" | where ( RunbookName_s == "SequencedStartStop_Parent" ) | where ( ResultType == "Completed" ) | summarize |AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) | sort by TimeGenerated desc```|

## <a name="viewing-the-solution"></a>Visualización de la solución

Para tener acceso a la solución, navegue a su cuenta de Automation y seleccione **Área de trabajo** en **RECURSOS RELACIONADOS**. En la página de Log Analytics, seleccione **Soluciones** en **GENERAL**. En la página **Soluciones**, seleccione la solución **Start-Stop-VM[workspace]** en la lista.

Al seleccionar la solución se muestra la página de la solución **Start-Stop-VM[workspace]**. En él puede examinar detalles importantes como el icono **StartStopVM**. Al igual que en el área de trabajo de Log Analytics, este icono muestra un número y una representación gráfica de los trabajos de runbook de la solución que se han iniciado y han finalizado correctamente.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Desde aquí puede realizar más análisis de los registros de trabajos. Para ello, solo debe hacer clic en el icono del anillo. El panel de la solución muestra el historial de trabajos y las consultas de búsqueda de registros predefinidas. Conmute al portal avanzado de Log Analytics para realizar búsquedas basadas en sus consultas de búsqueda.

## <a name="configure-email-notifications"></a>Configuración de notificaciones de correo electrónico

Para cambiar las notificaciones de correo electrónico después de implementar la solución, modifique el grupo de acciones creado durante la implementación.  

> [!NOTE]
> Las suscripciones en la nube de Azure Government no admiten la funcionalidad de correo electrónico de esta solución.

En Azure Portal, vaya a Monitor -> Grupos de acciones. Seleccione el grupo de acciones denominado **StartStop_VM_Notication**.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/azure-monitor.png)

En la página **StartStop_VM_Notification**, haga clic en **Editar detalles** en **Detalles**. Se abre la página **Email/SMS/Push/Voice** (Correo electrónico/SMS/Inserción/Voz). Actualice la dirección de correo electrónico y haga clic en **Aceptar** para guardar los cambios.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/change-email.png)

También puede agregar acciones adicionales al grupo de acciones; para más información sobre grupos de acciones, consulte [grupos de acciones](../azure-monitor/platform/action-groups.md)

El siguiente es un correo electrónico de ejemplo que se envía cuando la solución cierra las máquinas virtuales.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/email.png)

## <a name="add-exclude-vms"></a>Agregar o excluir VM

La solución proporciona la capacidad de agregar VM como destino de la solución o excluir específicamente las máquinas de la solución.

### <a name="add-a-vm"></a>Agregar una VM

Hay un par de opciones que puede usar para asegurarse de que una VM se incluye en la solución Start/Stop cuando se ejecuta.

* Cada uno de los [runbooks](#runbooks) primarios de la solución tiene un parámetro **VMList**. Puede pasar una lista separada por comas de nombres de VM a este parámetro al programar el runbook principal adecuado para su situación, y estas VM se incluirán cuando se ejecute la solución.

* Para seleccionar varias VM, establezca **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames** con los nombres de los grupos de recursos que contienen las VM que quiere iniciar o detener. También se puede establecer este valor como `*`, para que la solución se ejecute en todos los grupos de recursos de la suscripción.

### <a name="exclude-a-vm"></a>Excluir una VM

Para excluir una VM de la solución, puede agregarla a la variable **External_ExcludeVMNames**. Esta variable es una lista separada por comas de VM específicas para excluir de la solución Start/Stop.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Modificación de las programaciones de inicio y apagado

La administración de las programaciones de inicio y apagado en esta solución sigue los mismos pasos descritos en [Programación de un runbook en Azure Automation](automation-schedules.md). Debe haber una programación independiente para iniciar y detener las VM.

Se admite la configuración de la solución para simplemente detener las máquinas virtuales en un momento determinado. En este escenario, creará simplemente una programación **Stop** y ninguna programación **Start** correspondiente. Para ello, necesita lo siguiente:

1. Asegúrese de que ha agregado los grupos de recursos para que las máquinas virtuales se cierren en la variable **External_Stop_ResourceGroupNames**.
2. Cree su propia programación para el tiempo que desea apagar las máquinas virtuales.
3. Navegue hasta el runbook **ScheduledStartStop_Parent** y haga clic en **Programación**. Esto le permite seleccionar la programación que creó en el paso anterior.
4. Seleccione **Parameters and run settings** (Parámetros y configuración de ejecución) y establezca el parámetro ACTION en "Stop".
5. Haga clic en **Aceptar** para guardar los cambios.

## <a name="update-the-solution"></a>Actualización de la solución

Si ha implementado una versión anterior de esta solución, debe eliminarla de la cuenta antes de implementar una versión actualizada. Siga los pasos para [quitar la solución](#remove-the-solution) y luego siga los pasos anteriores para [implementarla](#deploy-the-solution).

## <a name="remove-the-solution"></a>Eliminación de la solución

Si decide que ya no necesita usar la solución, puede eliminarla de la cuenta de Automation. La eliminación de la solución solo quita los runbooks. No elimina las programaciones ni las variables que se crearon cuando se agregó la solución. Esos recursos se deben eliminar manualmente si no se usan con otros runbooks.

Para eliminar la solución, realice los siguientes pasos.

1. En su cuenta de Automation, seleccione **Área de trabajo** en la página izquierda.
1. En la página **Soluciones**, seleccione la solución **Start-Stop-VM[Workspace]**. En la página **VMManagementSolution[Workspace]**, en el menú, seleccione **Eliminar**.<br><br> ![Eliminación de la solución de administración de máquinas virtuales](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. En la ventana **Eliminar solución**, confirme que desea eliminar la solución.
1. Mientras se comprueba la información y se elimina la solución, puede realizar un seguimiento de su progreso en **Notificaciones** en el menú. Cuando comience el proceso para eliminar la solución, se le devuelve a la página **Soluciones**.

No se eliminarán la cuenta de Automation ni el área de trabajo de Log Analytics como parte de este proceso. Si no desea conservar el área de trabajo de Log Analytics, debe eliminarla manualmente, lo que se puede hacer desde Azure Portal:

1. En la pantalla de inicio de Azure Portal, seleccione **Log Analytics**.
1. En la página **Log Analytics**, seleccione el área de trabajo.
1. Seleccione **Eliminar** en el menú de la página de configuración del área de trabajo.

Si no desea conservar los componentes de la cuenta de Azure Automation, puede eliminarlos manualmente. Para obtener la lista de los runbooks, las variables y las programaciones creadas por la solución, consulte los [componentes de la solución](#solution-components).

## <a name="next-steps"></a>Pasos siguientes

- Para aprender a crear diferentes consultas de búsqueda y a revisar los registros de trabajos de Automation con Log Analytics, consulte [Descripción de las búsquedas de registros en Log Analytics](../log-analytics/log-analytics-log-searches.md).
- Para más información acerca de la ejecución de un runbook, cómo supervisar trabajos del runbook y otros detalles técnicos, consulte [Ejecución de un runbook en Automatización de Azure](automation-runbook-execution.md).
- Para más información acerca de Log Analytics y de los orígenes de recopilación de datos, consulte [Recopilación de registros y métricas de Azure para servicios de Log Analytics](../azure-monitor/platform/collect-azure-metrics-logs.md).
