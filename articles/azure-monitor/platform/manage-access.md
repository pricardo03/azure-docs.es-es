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
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 6c8f48ce71e11d1de0c28b4dab5327ab03e54f28
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231791"
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
* Ámbito para la configuración de opciones, como el [nivel de precios](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), la [retención](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) y el [límite de datos](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#daily-cap). 

Desde el punto de vista del consumo, se recomienda crear el menor número de áreas de trabajo que sea posible. Facilita las experiencias de administración y consulta, y las hace más rápidas. Pero, dadas las características anteriores, quizás quiera crear varias áreas de trabajo en los siguientes casos:

* Una empresa internacional que necesita que los datos estén almacenados en regiones concretas por motivos de soberanía de datos o cumplimiento normativo.
* Un usuario de Azure que desea evitar los gastos de transferencia de datos de salida manteniendo un área de trabajo en la misma región que los recursos de Azure que administra.
* Debería asignar los gastos a diferentes departamentos o grupos empresariales en función de su nivel de uso, creando un área de trabajo para cada departamento o grupo empresarial en su propia suscripción de Azure.
* Un proveedor de servicio administrado que necesita mantener los datos de Log Analytics para cada cliente que administra aislados de los datos de otro cliente.
* Como tal, tiene que administrar varios clientes y desea que cada cliente, departamento o grupo empresarial vea sus propios datos, pero no los datos de los demás.

Cuando se usan agentes de Windows para recopilar datos, se puede [configurar cada uno de ellos para que informe a una o varias áreas de trabajo](../../azure-monitor/platform/agent-windows.md).

Si usa System Center Operations Manager, cada grupo de administración de Operations Manager se puede conectar con una sola área de trabajo. Se puede instalar Microsoft Monitoring Agent en los equipos administrados por Operations Manager y hacer que el agente informe tanto a Operations Manager como a una diferente área de trabajo de Log Analytics.

## <a name="workspace-information"></a>Información del área de trabajo

Puede ver los detalles sobre el área de trabajo en Azure Portal. 

1. Si aún no lo ha hecho, inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.  

    ![Azure Portal](media/manage-access/azure-portal-01.png)  

3. En el panel de suscripciones de Log Analytics, seleccione un área de trabajo.

4. La página del área de trabajo muestra información detallada de introducción y sobre la configuración, así como vínculos a información adicional.  

    ![Información detallada acerca del área de trabajo](./media/manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Administración de cuentas y usuarios
Cada área de trabajo puede tener varias cuentas asociadas, y cada cuenta puede tener acceso a varias áreas de trabajo. El acceso se administra a través del [acceso basado en rol de Azure](../../role-based-access-control/role-assignments-portal.md). Estos derechos de acceso se aplican en el acceso a Azure Portal y a la API.


Las siguientes actividades también requieren permisos de Azure:

| .                                                          | Permisos de Azure necesarios | Notas |
|-----------------------------------------------------------------|--------------------------|-------|
| Agregar y quitar soluciones de administración                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Es necesario conceder estos permisos en el nivel de suscripción o grupo de recursos. |
| Cambiar el plan de tarifa                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Ver los datos en los iconos de soluciones *Backup* y *Site Recovery* | Administrador o coadministrador | Accede a los recursos implementados mediante el modelo de implementación clásica |
| Creación de un área de trabajo en Azure Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Administración del acceso a Log Analytics mediante permisos de Azure
Para conceder acceso al área de trabajo de Log Analytics mediante permisos de Azure, siga los pasos que se describen en [Uso de asignaciones de roles para administrar el acceso a los recursos de la suscripción de Azure](../../role-based-access-control/role-assignments-portal.md).

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

Se recomienda que realice las asignaciones en el nivel de recurso (área de trabajo) para asegurarse de que el control de acceso es preciso.  Use [roles personalizados](../../role-based-access-control/custom-roles.md) para crear roles con los permisos específicos necesarios.

## <a name="next-steps"></a>Pasos siguientes
* Consulte la [información general sobre el agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md) para recopilar datos de los equipos de su centro de datos u otro entorno de nube.
* Consulte [Recopilación de datos de máquinas virtuales de Azure](../../azure-monitor/learn/quick-collect-azurevm.md) para configurar la recopilación de datos de máquinas virtuales de Azure.  
* [Add Log Analytics solutions from the Solutions Gallery](../../azure-monitor/insights/solutions.md) (Incorporación de soluciones de Log Analytics desde la galería de soluciones) para agregar funcionalidad y recopilar datos.

