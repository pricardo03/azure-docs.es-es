---
title: Administrar áreas de trabajo en Azure Log Analytics y el portal de OMS | Documentos de Microsoft
description: Puede administrar las áreas de trabajo en Azure Log Analytics y el portal de OMS usando una serie de tareas administrativas en usuarios, cuentas, áreas de trabajo y cuentas de Azure.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 75faceb641382df29a93c44803af48dcfed142d4
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50421176"
---
# <a name="manage-workspaces"></a>Administración de áreas de trabajo

Para administrar el acceso a Log Analytics, tendrá que realizar varias tareas administrativas relacionadas con las áreas de trabajo. En este artículo se proporcionan consejos y procedimientos para administrar áreas de trabajo. En esencia, un área de trabajo es un contenedor donde se incluyen los datos de la cuenta e información básica de la configuración de la cuenta. Tanto usted como otros miembros de la organización pueden usar varias áreas de trabajo para administrar diferentes conjuntos de datos, recopilados a partir de toda la infraestructura de TI o de algunos de sus componentes.

Para crear un área de trabajo, necesitará:

1. Tener una suscripción a Azure.
2. Elegir un nombre de área de trabajo.
3. Asociar el área de trabajo a una de las suscripciones y grupos de recursos.
4. Elegir una ubicación geográfica.

## <a name="determine-the-number-of-workspaces-you-need"></a>Determinación del número de áreas de trabajo necesarias
Un área de trabajo es un recurso de Azure y un contenedor donde los datos se recopilan, se agregan, se analizan y se presentan en Azure Portal.

Puede tener varias áreas de trabajo por suscripción de Azure y puede tener acceso a más de un área de trabajo, con la posibilidad de realizar fácilmente consultas entre ellas. En esta sección se describe cuándo puede resultar útil la creación de más de un área de trabajo.

En la actualidad, un área de trabajo proporciona:

* Una ubicación geográfica para el almacenamiento de datos
* Aislamiento de datos para definir los diferentes derechos de acceso de usuarios
* Ámbito para la configuración de opciones, como la retención y el límite de datos

Desde el punto de vista del consumo, se recomienda crear el menor número de áreas de trabajo que sea posible. Facilita las experiencias de administración y consulta, y las hace más rápidas. Pero, dadas las características anteriores, quizás quiera crear varias áreas de trabajo en los siguientes casos:

* Una empresa internacional que necesita que los datos estén almacenados en regiones concretas por motivos de soberanía de datos o cumplimiento normativo.
* Un usuario de Azure que desea evitar los gastos de transferencia de datos de salida manteniendo un área de trabajo en la misma región que los recursos de Azure que administra.
* Debería asignar los gastos a diferentes departamentos o grupos empresariales en función de su nivel de uso, creando un área de trabajo para cada departamento o grupo empresarial en su propia suscripción de Azure.
* Un proveedor de servicio administrado que necesita mantener los datos de Log Analytics para cada cliente que administra aislados de los datos de otro cliente.
* Como tal, tiene que administrar varios clientes y desea que cada cliente, departamento o grupo empresarial vea sus propios datos, pero no los datos de los demás.

Cuando se usan agentes de Windows para recopilar datos, se puede [configurar cada uno de ellos para que informe a una o varias áreas de trabajo](log-analytics-windows-agents.md).

Si usa System Center Operations Manager, cada grupo de administración de Operations Manager se puede conectar con una sola área de trabajo. Se puede instalar Microsoft Monitoring Agent en los equipos administrados por Operations Manager y hacer que el agente informe tanto a Operations Manager como a una diferente área de trabajo de Log Analytics.

## <a name="workspace-information"></a>Información del área de trabajo

Puede ver los detalles sobre el área de trabajo en Azure Portal. 

1. Si aún no lo ha hecho, inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.  

    ![Azure Portal](media/log-analytics-manage-access/azure-portal-01.png)  

3. En el panel de suscripciones de Log Analytics, seleccione un área de trabajo.

4. La página del área de trabajo muestra información detallada de introducción y sobre la configuración, así como vínculos a información adicional.  

    ![Información detallada acerca del área de trabajo](./media/log-analytics-manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Administración de cuentas y usuarios
Cada área de trabajo puede tener varias cuentas asociadas, y cada cuenta puede tener acceso a varias áreas de trabajo. El acceso se administra a través del [acceso basado en rol de Azure](../active-directory/role-based-access-control-configure.md). Estos derechos de acceso se aplican en el acceso a Azure Portal y a la API.


Las siguientes actividades también requieren permisos de Azure:

| .                                                          | Permisos de Azure necesarios | Notas |
|-----------------------------------------------------------------|--------------------------|-------|
| Agregar y quitar soluciones de administración                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Es necesario conceder estos permisos en el nivel de suscripción o grupo de recursos. |
| Cambiar el plan de tarifa                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Ver los datos en los iconos de soluciones *Backup* y *Site Recovery* | Administrador o coadministrador | Accede a los recursos implementados mediante el modelo de implementación clásica |
| Creación de un área de trabajo en Azure Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Administración del acceso a Log Analytics mediante permisos de Azure
Para conceder acceso al área de trabajo de Log Analytics mediante permisos de Azure, siga los pasos que se describen en [Uso de asignaciones de roles para administrar el acceso a los recursos de la suscripción de Azure](../active-directory/role-based-access-control-configure.md).

Azure tiene dos roles de usuario integrados para Log Analytics:
- Lector de Log Analytics
- Colaborador de Log Analytics

Los miembros del rol *Lector de Log Analytics* pueden:
- Ver y buscar todos los datos de supervisión 
- Ver la configuración de supervisión, incluida la visualización de la configuración de Azure Diagnostics en todos los recursos de Azure.

El rol Lector de Log Analytics incluye las siguientes acciones de Azure:

| Escriba    | Permiso | DESCRIPCIÓN |
| ------- | ---------- | ----------- |
| . | `*/read`   | Capacidad para ver todos los recursos de Azure y la configuración de los recursos. Incluye la visualización de: <br> Estado de la extensión de la máquina virtual <br> Configuración de Azure Diagnostics en los recursos <br> Todas las propiedades y opciones de configuración de todos los recursos |
| . | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Capacidad de realizar consultas de búsqueda de registros v2 |
| . | `Microsoft.OperationalInsights/workspaces/search/action` | Capacidad de realizar consultas de búsqueda de registros v1 |
| . | `Microsoft.Support/*` | Capacidad de abrir casos de soporte técnico |
|No acción | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Evita la lectura de la clave del área de trabajo necesaria para usar la API de recopilación de datos e instalar agentes. Esto impide que los usuarios agreguen nuevos recursos al área de trabajo. |


Los miembros del rol *Colaborador de Log Analytics* pueden:
- Leer todos los datos de supervisión como puede el lector de Log Analytics  
- Crear y configurar cuentas de Automation  
- Agregar y quitar soluciones de administración    
    > [!NOTE] 
    > Para llevar a cabo correctamente estas dos últimas acciones, este permiso debe concederse en el nivel de suscripción o grupo de recursos.  

- Leer las claves de las cuentas de almacenamiento   
- Configurar la recopilación de registros de Azure Storage  
- Editar la configuración de supervisión de los recursos de Azure:
  - Agregar la extensión de máquina virtual a las máquinas virtuales
  - Configurar Azure Diagnostics en todos los recursos de Azure

> [!NOTE] 
> Puede usar la capacidad para agregar una extensión de máquina virtual a una máquina virtual para obtener el control total sobre una máquina virtual.

El rol Colaborador de Log Analytics incluye las siguientes acciones de Azure:

| Permiso | DESCRIPCIÓN |
| ---------- | ----------- |
| `*/read`     | Capacidad para ver todos los recursos y la configuración de los recursos. Incluye la visualización de: <br> Estado de la extensión de la máquina virtual <br> Configuración de Azure Diagnostics en los recursos <br> Todas las propiedades y opciones de configuración de todos los recursos |
| `Microsoft.Automation/automationAccounts/*` | Capacidad para crear y configurar cuentas de Azure Automation, incluido agregar y editar runbooks |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Agregar, actualizar y eliminar extensiones de máquina virtual, incluida la extensión de Microsoft Monitoring Agent y el agente de OMS para la extensión de Linux |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Ver la clave de la cuenta de almacenamiento. Necesaria para configurar Log Analytics para leer los registros de cuentas de Azure Storage |
| `Microsoft.Insights/alertRules/*` | Agregar, actualizar y eliminar reglas de alertas |
| `Microsoft.Insights/diagnosticSettings/*` | Agregar, actualizar y eliminar la configuración de diagnósticos en los recursos de Azure |
| `Microsoft.OperationalInsights/*` | Agregar, actualizar y eliminar la configuración de áreas de trabajo de Log Analytics |
| `Microsoft.OperationsManagement/*` | Agregar y eliminar soluciones de administración |
| `Microsoft.Resources/deployments/*` | Crear y eliminar implementaciones. Necesario para agregar y eliminar soluciones, áreas de trabajo y cuentas de Automation |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Crear y eliminar implementaciones. Necesario para agregar y eliminar soluciones, áreas de trabajo y cuentas de Automation |

Para agregar y quitar usuarios de un rol de usuario, es necesario tener permisos `Microsoft.Authorization/*/Delete` y `Microsoft.Authorization/*/Write`.

Use estos roles para conceder a los usuarios acceso en distintos ámbitos:
- Suscripción: acceso a todas las áreas de trabajo de la suscripción
- Grupo de recursos: acceso a todas las áreas de trabajo del grupo de recursos
- Recurso: acceso solo al área de trabajo especificada

Se recomienda que realice las asignaciones en el nivel de recurso (área de trabajo) para asegurarse de que el control de acceso es preciso.  Use [roles personalizados](../active-directory/role-based-access-control-custom-roles.md) para crear roles con los permisos específicos necesarios.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Vincular un área de trabajo existente a una suscripción de Azure
Todas las áreas de trabajo creadas después del 26 de septiembre de 2016 deben estar vinculadas a una suscripción de Azure en el momento de la creación. Las áreas de trabajo creadas antes de esta fecha deben estar vinculadas a un área de trabajo la próxima vez que inicie sesión. Al crear el área de trabajo desde Azure Portal, o al vincular el área de trabajo a una suscripción de Azure, su instancia de Azure Active Directory quedará vinculada como su cuenta de organización.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Para vincular un área de trabajo a una suscripción de Azure en Azure Portal
1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.  

2. En el panel de suscripciones de Log Analytics, haga clic en **Agregar**.  

    ![lista de áreas de trabajo](./media/log-analytics-manage-access/workspace-link-existing-01.png)

3. Desde el panel del **área de trabajo de Log Analytics**, haga clic en **Vincular existente**.  

4. Haga clic en **Configurar los valores obligatorios**.  

5. Aparecerá una lista con las áreas de trabajo que aún no están vinculadas con su cuenta de Azure. Seleccione el área de trabajo.  
   
6. Si es necesario, puede cambiar los valores de los elementos siguientes:
   * Subscription
   * Grupos de recursos
   * Ubicación
   * Plan de tarifa  

7. Haga clic en **OK**. Ahora, el área de trabajo está vinculada a su cuenta de Azure.

> [!NOTE]
> Si no ve el área de trabajo que quiere vincular, significa que su suscripción de Azure no tiene acceso al área de trabajo que creó mediante el portal de OMS.  Para conceder acceso a esta cuenta desde el portal OMS, consulte [Agregar un usuario a un área de trabajo existente](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Actualizar el área de trabajo a un plan de pago
En OMS, existen tres tipos de planes de áreas de trabajo: **Free** (Gratis), **Standalone** (Independiente) y **OMS**.  Si dispone del plan *Free* (Gratis), se puede enviar un máximo de 500 MB de datos por día a Log Analytics.  Si supera esta cantidad, debe cambiar el área de trabajo a un plan de pago para evitar la pérdida de datos una vez superado este límite. Puede cambiar de tipo de plan en cualquier momento.  Para más información sobre los precios de OMS, consulte [Precios de Operations Management Suite](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Uso de los derechos de una suscripción de OMS
Para usar los derechos que proceden de la adquisición de OMS E1, OMS E2 OMS o un complemento de OMS para System Center, elija el plan *OMS* de OMS Log Analytics.

Cuando adquiere una suscripción de OMS, los derechos se agregan a su contrato Enterprise. Cualquier suscripción de Azure creada en virtud de este contrato puede usar estos derechos. Todas las áreas de trabajo de estas suscripciones usan los derechos de OMS.

Para asegurarse de que el uso de un área de trabajo se realiza con arreglo a los derechos de la suscripción de OMS, deberá:

1. Crear el área de trabajo en una suscripción de Azure incluida en el contrato Enterprise que incluye la suscripción de OMS

2. Seleccionar el plan *OMS* para el área de trabajo

> [!NOTE]
> Si el área de trabajo se creó antes del 26 de septiembre de 2016 y el plan de precios de Log Analytics es *Premium*, este área de trabajo usa los derechos del complemento de OMS para System Center. También puede usar los derechos, cambiando al plan de tarifa *OMS*.
>
>

Los derechos de la suscripción de OMS no son visibles en Azure Portal. Podrá ver estos derechos y usos en Enterprise Portal.  

Si necesita cambiar la suscripción de Azure a la que está vinculada el área de trabajo, puede usar el cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) de Azure PowerShell.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Uso del compromiso de Azure en contratos Enterprise
Si no tiene una suscripción de OMS, pagará por separado por cada componente de OMS y el uso aparecerá en la factura de Azure.

Si tiene un compromiso monetario de Azure sobre la inscripción Enterprise a la que están vinculadas sus suscripciones de Azure, el uso de Log Analytics se descontará de este importe.

Si necesita cambiar la suscripción de Azure a la que está vinculada el área de trabajo, puede usar el cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) de Azure PowerShell.  

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-azure-portal"></a>Cambio de un área de trabajo a un plan de tarifa de pago en Azure Portal
1. En Azure Portal, en el panel de suscripciones de Log Analytics, seleccione un área de trabajo.

2. En el panel de área de trabajo, en **General**, seleccione **Plan de tarifa**.  

3. En **Plan de tarifa**, seleccione un plan y haga clic en **Seleccionar**.  
    ![Plan de precios seleccionado](./media/log-analytics-manage-access/workspace-pricing-tier-info.png)

> [!NOTE]
> Si el área de trabajo está vinculada a una cuenta de Automation, antes de seleccionar el plan de tarifa *Independiente (por GB)* debe eliminar todas las soluciones de **Automation and Control** y desvincular la cuenta de Automation. En la hoja del área de trabajo, en **General**, haga clic en **Soluciones** para ver y eliminar soluciones. Para desvincular la cuenta de Automation, haga clic en el nombre de la cuenta de Automation en la hoja **Plan de tarifa**.
>
>

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-oms-portal"></a>Cambio de un área de trabajo a un plan de tarifa de pago en el portal de OMS

Para cambiar el plan de tarifa mediante el portal de OMS, debe tener una suscripción de Azure.

1. En el portal de OMS, haga clic en el icono **Configuración**.

2. Haga clic en la pestaña **Cuentas** y, luego, en la pestaña **Azure Subscription & Data Plan** (Suscripción y plan de datos de Azure).

3. Haga clic en el plan de tarifa que quiere usar.

4. Haga clic en **Save**(Guardar).  

    ![Planes de datos y suscripción](./media/log-analytics-manage-access/subscription-tab.png)

Su nuevo plan de datos se muestra en la cinta de opciones del portal OMS, que se encuentra en la parte superior de la página web.

![cinta de opciones de OMS](./media/log-analytics-manage-access/data-plan-changed.png)

## <a name="next-steps"></a>Pasos siguientes
* Consulte [Recopilar datos de equipos en su entorno con Log Analytics](log-analytics-concept-hybrid.md) para recopilar datos de los equipos de su centro de datos o de otro entorno en la nube.
* Consulte [Recopilación de datos de máquinas virtuales de Azure](log-analytics-quick-collect-azurevm.md) para configurar la recopilación de datos de máquinas virtuales de Azure.  
* [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md) (Incorporación de soluciones de Log Analytics desde la galería de soluciones) para agregar funcionalidad y recopilar datos.

