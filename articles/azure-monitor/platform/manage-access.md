---
title: Administración de áreas de trabajo de Log Analytics en Azure Monitor | Microsoft Docs
description: Puede administrar las áreas de trabajo de Log Analytics en Azure Monitor con diferentes tareas administrativas en usuarios, cuentas, áreas de trabajo y cuentas de Azure.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: magoedte
ms.openlocfilehash: fd47b5de2226d88b6295cb28713db2a5d251e768
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58577253"
---
# <a name="manage-log-data-and-workspaces-in-azure-monitor"></a>Administrar datos de registro y las áreas de trabajo en Azure Monitor
Azure Monitor almacena los datos de registro en un área de trabajo de Log Analytics, que es básicamente un contenedor que incluye información de configuración y datos. Para administrar el acceso a datos del registro, tendrá que realizar varias tareas administrativas relacionadas con las áreas de trabajo. Tanto usted como otros miembros de la organización pueden usar varias áreas de trabajo para administrar diferentes conjuntos de datos, recopilados a partir de toda la infraestructura de TI o de algunos de sus componentes.

En este artículo se explica cómo administrar el acceso a los registros y para administrar las áreas de trabajo que los contienen. 

## <a name="create-a-workspace"></a>Crear un área de trabajo
Para crear un área de trabajo de Log Analytics, necesita:

1. Tener una suscripción a Azure.
2. Elegir un nombre de área de trabajo.
3. Asociar el área de trabajo a una de las suscripciones y grupos de recursos.
4. Elegir una ubicación geográfica.

Consulte los artículos siguientes para obtener más información sobre cómo crear un área de trabajo:

- [Crear un área de trabajo de Log Analytics en Azure portal](../learn/quick-create-workspace.md)
- [Crear un área de trabajo de Log Analytics con la CLI de Azure 2.0](../learn/quick-create-workspace-cli.md)
- [Crear un área de trabajo de Log Analytics con Azure PowerShell](../learn/quick-create-workspace-posh.md)

## <a name="determine-the-number-of-workspaces-you-need"></a>Determinación del número de áreas de trabajo necesarias
Un área de trabajo de Log Analytics es un recurso de Azure y un contenedor donde los datos se recopilan, se agregan, se analizan y se presentan en Azure Monitor. Puede tener varias áreas de trabajo por suscripción de Azure y puede tener acceso a más de un área de trabajo, con la posibilidad de realizar fácilmente consultas entre ellas. En esta sección se describe cuándo puede resultar útil la creación de más de un área de trabajo.

Un área de trabajo de Log Analytics proporciona lo siguiente:

* Una ubicación geográfica para almacenar los datos.
* Aislamiento de los datos para definir derechos de acceso de usuario diferente en modo basado en el área de trabajo. No es relevante cuando se trabaja en modo basado en el recurso.
* Ámbito para la configuración de opciones, como [plan de tarifa](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [retención](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) y [datos límite](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#daily-cap).
* Los cargos relacionados con la retención y la ingesta de datos se realizan en el recurso de área de trabajo.

Desde el punto de vista del consumo, se recomienda crear el menor número de áreas de trabajo que sea posible. Facilita las experiencias de administración y consulta, y las hace más rápidas. Pero, dadas las características anteriores, quizás quiera crear varias áreas de trabajo en los siguientes casos:

* Una empresa internacional que necesita que los datos de registro estén almacenados en regiones concretas por motivos de soberanía de datos o cumplimiento normativo.
* Un usuario de Azure que desea evitar los gastos de transferencia de datos de salida manteniendo un área de trabajo en la misma región que los recursos de Azure que administra.
* Un proveedor de servicio administrado que necesita mantener los datos de Log Analytics para cada cliente que administra aislados de los datos de otro cliente.
* Administran varios clientes y desea que cada cliente o departamento o grupo empresarial vea sus propios datos, pero no los datos de otras personas, y no hay ninguna necesidad empresarial de un cliente entre consolidado / departamento o grupo empresarial vista. ".

Cuando se usan agentes de Windows para recopilar datos, se puede [configurar cada uno de ellos para que informe a una o varias áreas de trabajo](../../azure-monitor/platform/agent-windows.md).

Si usa System Center Operations Manager, cada grupo de administración de Operations Manager se puede conectar con una sola área de trabajo. Se puede instalar Microsoft Monitoring Agent en los equipos administrados por Operations Manager y hacer que el agente informe tanto a Operations Manager como a una diferente área de trabajo de Log Analytics.

Una vez que se define la arquitectura de área de trabajo, debe aplicar esta directiva en los recursos de Azure con [Azure Policy](../../governance/policy/overview.md). Esto puede proporcionar una definición integrada que se aplicará automáticamente a todos los recursos de Azure. Por ejemplo, podría establecer una directiva para asegurarse de que todos los recursos de Azure en una región determinada envían sus registros de diagnóstico a un área de trabajo determinado.

## <a name="view-workspace-details"></a>Ver detalles del área de trabajo
Mientras que analizar datos en el área de trabajo de Log Analytics desde la **Azure Monitor** menú en el portal de Azure, crear y administrar áreas de trabajo en el **las áreas de trabajo de Log Analytics** menú.
 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione las áreas de trabajo de **Log Analytics**.  

    ![Azure Portal](media/manage-access/azure-portal-01.png)  

3. Seleccione el área de trabajo de la lista.

4. La página del área de trabajo muestra los detalles sobre el área de trabajo, introducción, configuración y vínculos para obtener más información.  

    ![Información detallada acerca del área de trabajo](./media/manage-access/workspace-overview-page.png)  


## <a name="workspace-permissions-and-scope"></a>Ámbito y los permisos del área de trabajo
Los datos que un usuario tiene acceso a dependen de varios factores que se enumeran en la tabla siguiente. Se describen en las secciones siguientes.

| Factor | DESCRIPCIÓN |
|:---|:---|
| [Modo de acceso](#access-modes) | Método usado por el usuario a tiene acceso el área de trabajo.  Define el ámbito de los datos disponibles y el modo de control de acceso que se aplica. |
| [Modo de control de acceso](#access-control-mode) | Configuración en el área de trabajo que define si los permisos se aplican en el nivel de área de trabajo o un recurso. |
| [Permisos](#manage-accounts-and-users) | Permisos aplicados a individuales o grupos de usuarios para el área de trabajo o el recurso. Define los datos que el usuario tendrá acceso a. |
| [RBAC de nivel de tabla](#table-level-rbac) | Permisos granulares opcionales que se aplica a todos los usuarios independientemente de su modo de acceso o el modo de control de acceso. Define qué tipos de datos puede tener acceso un usuario. |



## <a name="access-modes"></a>Modos de acceso
El _modo de acceso_ hace referencia a cómo un usuario accede a un área de trabajo de Log Analytics y define el ámbito de datos que pueden tener acceso. 

**Workspace-centric**: En este modo, un usuario puede ver todos los registros en el área de trabajo que tienen permisos. Las consultas en este modo se limitan a todos los datos en todas las tablas en el área de trabajo. Este es el modo de acceso utilizado cuando se accede a los registros con el área de trabajo como el ámbito, como cuando se selecciona **registros** desde el **Azure Monitor** menú en el portal de Azure.

**Centrada en los recursos**: Al acceder a un recurso determinado, por ejemplo, al seleccionar el área de trabajo **registros** desde un menú de recursos en Azure portal, puede ver los registros para solo ese recurso en todas las tablas que tienen acceso a. Las consultas de este modo se limitan a solo los datos asociados a ese recurso. Este modo también permite el control de acceso granular basada en roles (RBAC). 

> [!NOTE]
> Los registros están disponibles para las consultas centrada en los recursos solo si fueran correctamente asociados con el recurso pertinente. Actualmente, los siguientes recursos tienen limitaciones: 
> - Equipos fuera de Azure
> - Service Fabric
> - Application Insights
> - Contenedores
> - Registros personalizados creados por la API del recopilador de datos HTTP
>
> Puede probar si los registros están correctamente asociados con su recurso mediante la ejecución de una consulta y la inspección de los registros que le interesa. Si el identificador de recurso correcto se encuentra en la [_ResourceId](log-standard-properties.md#_resourceid) propiedad y, a continuación, datos están disponibles para consultas centrada en los recursos.

### <a name="comparing-access-modes"></a>Comparación de los modos de acceso

La siguiente tabla resume los modos de acceso:

| | Centrado en el área de trabajo | Centrada en los recursos |
|:---|:---|:---|
| ¿Quién está pensado para cada modelo? | Administración central. Administradores que necesitan para configurar la recopilación de datos y los usuarios que necesitan tener acceso a una amplia variedad de recursos. También requiere actualmente para los usuarios que tienen para tener acceso a los registros de recursos fuera de Azure. | Equipos de la aplicación. Administradores de recursos de Azure que se está supervisando. |
| ¿Qué es necesario un usuario para ver los registros? | Permisos para el área de trabajo. Consulte **permisos del área de trabajo** en [administre cuentas y usuarios](#manage-accounts-and-users). | Acceso de lectura al recurso. Consulte **permisos de recurso** en [administre cuentas y usuarios](#manage-accounts-and-users). Los permisos pueden ser heredado (por ejemplo, a partir del grupo de recursos contiene) o se asigna directamente al recurso. Permiso para los registros para el recurso se asignará automáticamente. |
| ¿Qué es el ámbito de los permisos? | Área de trabajo. Los usuarios con acceso al área de trabajo pueden consultar todos los registros en esa área de trabajo de las tablas que tienen permisos. Consulte [control de acceso de la tabla](#table-level-rbac) | Recursos de Azure. Usuario puede consultar los registros de recursos tenga acceso desde cualquier área de trabajo, pero no se puede consultar los registros de otros recursos. |
| ¿Cómo pueden los registros de acceso de usuario? | Iniciar **registros** desde **Azure Monitor** menú o **las áreas de trabajo de Log Analytics**. | Iniciar **registros** en el menú para el recurso de Azure. |


## <a name="access-control-mode"></a>Modo de control de acceso
El _modo de control de acceso_ es una opción en cada áreas de trabajo que define cómo se determinan los permisos para esa área de trabajo.

**Requieren permisos de área de trabajo**:  Este modo de control no admite RBAC granular. Para que un usuario pueda acceder el área de trabajo, debe tener permisos en el área de trabajo o en tablas específicas. 

Si un usuario tiene acceso el área de trabajo en modo basado en el área de trabajo, tendrán acceso a todos los datos de todas las tablas que ha concedido acceso a. Si un usuario tiene acceso en modo de recursos centrado en el área de trabajo, tendrá acceso a datos solo para ese recurso en todas las tablas que ha concedido acceso a.

Se trata de la configuración predeterminada para todas las áreas de trabajo creadas antes de marzo de 2019.

**Usar permisos de recurso o el área de trabajo**: Este modo de control permite RBAC granular. Los usuarios se les concederá acceso a solo los datos asociados a los recursos que pueden ver a través de los permisos de Azure, los recursos para los que tienen `read` permiso. 

Cuando un usuario tiene acceso el área de trabajo en modo basado en el área de trabajo, se aplicarán los permisos de área de trabajo. Cuando un usuario tiene acceso en modo de recursos centrado en el área de trabajo, se comprobarán los permisos de recursos solo y se omitirán los permisos del área de trabajo. Habilitación de RBAC para un usuario quitarlas de los permisos del área de trabajo y permitir que sus permisos de recursos que lo haya reconocido.

Se trata de la configuración predeterminada para todas las áreas de trabajo creadas después de marzo de 2019.

> [!NOTE]
> Si un usuario tiene permisos de recursos solo al área de trabajo, sólo podrán obtener acceso al área de trabajo mediante [modo centrada en los recursos](#access-modes).


### <a name="define-access-control-mode-in-azure-portal"></a>Definir el modo de control de acceso en Azure portal
Puede ver el modo de control de acceso de área de trabajo actual en el **Introducción** página del área de trabajo en el **área de trabajo de Log Analytics** menú.

![Modo de control de acceso de área de trabajo de vista](media/manage-access/view-access-control-mode.png)

Puede cambiar esta configuración en el **propiedades** página del área de trabajo. Cambiar la configuración se deshabilitará si no tiene permisos para configurar el área de trabajo.

![Cambiar el modo de acceso de área de trabajo](media/manage-access/change-access-control-mode.png)

### <a name="define-access-control-mode-in-powershell"></a>Definir el modo de control de acceso en PowerShell

Use el comando siguiente para examinar el modo de control de acceso para todas las áreas de trabajo en la suscripción:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {$_.Name + ": " + $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions} 
```

Use el siguiente script para establecer el modo de control de acceso para un área de trabajo específica:

```powershell
$WSName = "my-workspace"
$Workspace = Get-AzResource -Name $WSName -ExpandProperties
if ($Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $Workspace.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $Workspace.ResourceId -Properties $Workspace.Properties -Force
```

Use el siguiente script para establecer el modo de control de acceso para todas las áreas de trabajo en la suscripción

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {
if ($_.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $_.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $_.ResourceId -Properties $_.Properties -Force
```

### <a name="define-access-mode-in-resource-manager-template"></a>Definir el modo de acceso en la plantilla de Resource Manager
Para configurar el modo de acceso en una plantilla de Azure Resource Manager, establezca el **enableLogAccessUsingOnlyResourcePermissions** marca en el área de trabajo a uno de los siguientes valores de características.

- **false**: Establezca el área de trabajo en permisos centrada en el área de trabajo. Se trata la configuración predeterminada si no se establece la marca.
- **true**: Establezca el área de trabajo en permisos centrada en los recursos.


## <a name="manage-accounts-and-users"></a>Administración de cuentas y usuarios
Los permisos para el área de trabajo que se aplican a un usuario determinado se definen mediante su modo de acceso y el [modo de control de acceso](#access-control-mode) del área de trabajo. **Permisos del área de trabajo** se aplican cuando un usuario tiene acceso a cualquier área de trabajo mediante **centrada en el área de trabajo** en [centrada en el área de trabajo de modo](#access-modes). **Permisos de recurso** se aplican cuando un usuario accede a un área de trabajo con **usar permisos de recurso o el área de trabajo** [modo de control de acceso](#access-control-mode) mediante [modo centrada en los recursos ](#access-modes).

### <a name="workspace-permissions"></a>Permisos del área de trabajo
Cada área de trabajo puede tener varias cuentas asociadas, y cada cuenta puede tener acceso a varias áreas de trabajo. El acceso se administra a través del [acceso basado en rol de Azure](../../role-based-access-control/role-assignments-portal.md). 


Las siguientes actividades también requieren permisos de Azure:

| .                                                          | Permisos de Azure necesarios | Notas |
|-----------------------------------------------------------------|--------------------------|-------|
| Agregar y quitar soluciones de supervisión                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Es necesario conceder estos permisos en el nivel de suscripción o grupo de recursos. |
| Cambiar el plan de tarifa                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Ver los datos en los iconos de soluciones *Backup* y *Site Recovery* | Administrador o coadministrador | Accede a los recursos implementados mediante el modelo de implementación clásica |
| Creación de un área de trabajo en Azure Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


#### <a name="manage-access-to-log-analytics-workspace-using-azure-permissions"></a>Administrar el acceso al área de trabajo de Log Analytics mediante permisos de Azure 
Para conceder acceso al área de trabajo de Log Analytics mediante permisos de Azure, siga los pasos que se describen en [Uso de asignaciones de roles para administrar el acceso a los recursos de la suscripción de Azure](../../role-based-access-control/role-assignments-portal.md).

Azure tiene dos roles de usuario integrados para áreas de trabajo de Log Analytics:
- Lector de Log Analytics
- Colaborador de Log Analytics

Los miembros del rol *Lector de Log Analytics* pueden:
- Ver y buscar todos los datos de supervisión 
- Ver la configuración de supervisión, incluida la visualización de la configuración de Azure Diagnostics en todos los recursos de Azure.

El rol Lector de Log Analytics incluye las siguientes acciones de Azure:

| Type    | Permiso | DESCRIPCIÓN |
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

Debe realizar las asignaciones en el nivel de recursos (área de trabajo) para garantizar el control de acceso preciso.  Use [roles personalizados](../../role-based-access-control/custom-roles.md) para crear roles con los permisos específicos necesarios.

### <a name="resource-permissions"></a>Permisos de recursos 
Cuando inicia una consulta de los usuarios de un área de trabajo mediante el acceso centrada en los recursos, tendrá los permisos siguientes en el recurso:

| Permiso | DESCRIPCIÓN |
| ---------- | ----------- |
| `Microsoft.Insights/logs/<tableName>/read`<br><br>Ejemplos:<br>`Microsoft.Insights/logs/*/read`<br>`Microsoft.Insights/logs/Heartbeat/read` | Capacidad para ver todos los datos de registro para el recurso.  |


Normalmente, se concede este permiso de un rol que incluya  _\*/lectura o_ _\*_ permisos como integrado [lector](../../role-based-access-control/built-in-roles.md#reader) y [ Colaborador](../../role-based-access-control/built-in-roles.md#contributor) roles. Tenga en cuenta que funciones personalizadas que incluyen acciones específicas o dedicados roles integrados no incluyen este permiso.

Consulte [definir el control de acceso por tabla](#table-level-rbac) siguiente si desea crear el control de acceso diferentes para distintas tablas.


## <a name="table-level-rbac"></a>RBAC de nivel de tabla
**RBAC de nivel de tabla** le permite proporcionar un control más granular a los datos en un área de trabajo de Log Analytics además de los otros permisos. Este control le permite definir tipos de datos específicos que son accesibles sólo a un conjunto específico de usuarios.

Implementar control de acceso de la tabla con [Azure roles personalizados](../../role-based-access-control/custom-roles.md) para conceder o denegar acceso específico para [tablas](../log-query/log-query-overview.md#how-azure-monitor-log-data-is-organized) en el área de trabajo. Estos roles se aplican a las áreas de trabajo con centrada en el área de trabajo o centrada en los recursos [modos de control de acceso a](#access-control-mode) independientemente del usuario [modo de acceso](#access-modes).

Crear un [rol personalizado](../../role-based-access-control/custom-roles.md) con las siguientes acciones para definir el acceso al control de acceso de la tabla.

- Para conceder acceso a una tabla, incluirlo en el **acciones** sección de la definición de rol.
- Para denegar el acceso a una tabla, incluirlo en el **NotActions** sección de la definición de rol.
- Use * para especificar todas las tablas.

Por ejemplo, para crear un rol con acceso a la _latido_ y _AzureActivity_ tablas, crear un rol personalizado con las siguientes acciones:

```
"Actions":  [
              "Microsoft.OperationalInsights/workspaces/query/Heartbeat/read",
              "Microsoft.OperationalInsights/workspaces/query/AzureActivity/read"
  ],
```

Para crear un rol con acceso únicamente a _SecurityBaseline_ y ninguna otra tabla, cree un rol personalizado con las siguientes acciones:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read"
    ],
    "NotActions":  [
        "Microsoft.OperationalInsights/workspaces/query/*/read"
    ],
```

### <a name="custom-logs"></a>Registros personalizados
 Los registros personalizados se crean mediante los orígenes de datos, como los registros personalizados y HTTP Data Collector API. Es la manera más fácil de identificar el tipo de registro mediante la comprobación de las tablas que aparecen en [registros personalizados del esquema del registro](../log-query/get-started-portal.md#understand-the-schema).

 Actualmente no se puede conceder o denegar el acceso a los registros personalizados individuales, pero puede conceder o denegar el acceso a todos los registros personalizados. Para crear un rol con acceso a todos los registros personalizados, cree un rol personalizado con las siguientes acciones:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read"
    ],
```

### <a name="considerations"></a>Consideraciones

- Si un usuario se le conceden global leer permiso con los roles de lector o colaborador estándar que incluyen la  _\*/lectura_ acción, invalidará el control de acceso por tabla y dar acceso a todos los datos de registro.
- Si se concede a un usuario, pero ningún otro permiso de acceso de cada tabla, podrán tener acceso a datos de registro de la API, pero no desde el portal de Azure. Para proporcionar acceso al portal de Azure, use el lector de Log Analytics como su rol de base.
- Los administradores de la suscripción tendrá acceso a todos los tipos de datos, independientemente de cualquier otra configuración de permisos.
- Los propietarios del área de trabajo se tratan como cualquier otro usuario para controlar el acceso por tabla.
- Debe asignar roles a los grupos de seguridad en lugar de usuarios individuales para reducir el número de asignaciones. Esto también le ayudará usar herramientas de administración de grupo existentes para configurar y comprobar el acceso.




## <a name="next-steps"></a>Pasos siguientes
* Consulte la [información general sobre el agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md) para recopilar datos de los equipos de su centro de datos u otro entorno de nube.
* Consulte [Recopilación de datos de máquinas virtuales de Azure](../../azure-monitor/learn/quick-collect-azurevm.md) para configurar la recopilación de datos de máquinas virtuales de Azure.  

