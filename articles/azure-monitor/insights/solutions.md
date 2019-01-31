---
title: Soluciones de administración en Azure Monitor | Microsoft Docs
description: Las soluciones de administración de Azure Monitor son una colección de reglas de lógica, visualización y adquisición de datos que proporcionan métricas que giran en torno a una determinada área de problemas.  Este artículo proporciona información sobre la instalación y el uso de soluciones de administración.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/07/2018
ms.author: bwren
ms.openlocfilehash: 0103225803ca98cb9d8e27eb57f150e0660cd7ab
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299143"
---
# <a name="management-solutions-in-azure-monitor"></a>Soluciones de administración en Azure Monitor
Las soluciones de administración aprovechan los servicios de Azure para proporcionar información adicional sobre el funcionamiento de una aplicación o servicio determinados. Este artículo proporciona una breve descripción de las soluciones de administración en Azure y los detalles sobre su uso e instalación.

Las soluciones de administración suelen recopilar datos de registro y proporcionar consultas y vistas para analizar los datos recopilados. También pueden aprovechar otros servicios como Azure Automation para realizar acciones relacionadas con la aplicación o el servicio.

Puede agregar soluciones de administración a su instancia de Azure Monitor para todas las aplicaciones y los servicios que use. Suelen estar disponibles sin costo, pero la recopilación de datos puede suponer cargos por uso. Además de las soluciones que proporciona Microsoft, los asociados y los clientes pueden [crear soluciones de administración](solutions-creating.md) para utilizarlas en su propio entorno o ponerlas a disposición de los clientes en la comunidad.

## <a name="use-management-solutions"></a>Uso de soluciones de administración
Abra la página **Introducción** del área de trabajo de Log Analytics para mostrar un icono para cada solución instalada en el área de trabajo. 

1. Inicie sesión en el Portal de Azure.
1. Abra **Todos los servicios** y busque **Monitor**.
1. En el menú **Insights**, seleccione **Más**.
1. Use los cuadros de lista desplegable en la parte superior de la pantalla para cambiar el área de trabajo o el intervalo de tiempo usado para los iconos.
1. Haga clic en el icono de una solución para abrir la vista que incluye un análisis más detallado de los datos recopilados.

![Información general](media/solutions/overview.png)

Las soluciones de administración pueden contener varios tipos de recursos de Azure; puede ver todos los recursos que incluyen las soluciones igual que cualquier otro recurso. Por ejemplo, las consultas de registro incluidas en la solución se enumeran bajo **Consultas de solución** en [Explorador de consultas](../log-query/get-started-portal.md#load-queries). Puede usar esas consultas al llevar a cabo análisis ad hoc con Log Analytics.

## <a name="list-installed-management-solutions"></a>Lista de soluciones de administración instaladas 
Utilice el procedimiento siguiente para enumerar las soluciones de administración instaladas en su suscripción.

1. Inicie sesión en el Portal de Azure.
1. Abra **Todos los servicios** y busque **Soluciones**.
4. Se mostrarán las soluciones instaladas en todas las áreas de trabajo. Al nombre de la solución le sigue el nombre del área de trabajo de Log Analytics donde está instalada.
1. Use los cuadros de lista desplegable de la parte superior de la pantalla para filtrar por suscripción o grupo de recursos.


![Lista de todas las soluciones](media/solutions/list-solutions-all.png)

Haga clic en el nombre de una solución para abrir su página de resumen. Esta página muestra las vistas incluidas en la solución y proporciona diferentes opciones para la solución y su área de trabajo. Para ver la página de resumen de una solución, siga uno de los procedimientos descritos anteriormente para mostrar las soluciones y haga clic en el nombre concreto.

![Propiedades de la solución](media/solutions/solution-properties.png)



## <a name="install-a-management-solution"></a>Instalación de una solución de administración
Las soluciones de administración de Microsoft y asociados están disponibles en [Azure Marketplace](https://azuremarketplace.microsoft.com). Puede buscar las soluciones disponibles e instalarlas mediante el procedimiento siguiente. Al instalar una solución, debe seleccionar un [área de trabajo de Log Analytics](../platform/manage-access.md) donde se instalará la solución y se recopilarán los datos.

1. Desde la [lista de soluciones para la suscripción](#list-installed-management-solutions), haga clic en **Agregar**. 
1. A la derecha de **Soluciones de administración de**, haga clic en **Más**. 
1. Localice la solución de administración que quiere y lea su descripción.
1. Haga clic en **Crear** para iniciar el proceso de instalación.
1. Cuando se inicia el proceso de instalación, se le pedirá que proporcione la configuración necesaria, que varía para cada solución.

![Instalar una solución](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Instalación de una solución desde la comunidad
Los miembros de la comunidad pueden enviar soluciones de administración a las plantillas de inicio rápido de Azure. Puede instalar estas soluciones directamente o descargar plantillas para hacerlo más adelante.

1. Siga el proceso descrito en [Área de trabajo de Log Analytics y cuenta de Automation](#log-analytics-workspace-and-automation-account) para vincular un área de trabajo y una cuenta.
2. Visite [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/). 
3. Busque una solución que le interese.
4. Seleccione la solución en los resultados para ver su información.
5. Haga clic en botón **Deploy to Azure** (Implementar en Azure).
6. Se le pedirá que proporcione información como el grupo de recursos y la ubicación, además de los valores de los parámetros de la solución.
7. Haga clic en **Adquirir** para instalar la solución.


## <a name="log-analytics-workspace-and-automation-account"></a>Área de trabajo de Log Analytics y cuenta de Automation
Todas las soluciones de administración requieren un [área de trabajo de Log Analytics](../platform/manage-access.md) para almacenar los datos recopilados por la solución y para hospedar sus vistas y búsquedas de registros. Algunas soluciones también requieren una [cuenta de Automation](../../automation/automation-security-overview.md#automation-account-overview) que contenga runbooks y recursos relacionados. El área de trabajo y la cuenta deben cumplir los siguientes requisitos.

* Cada instalación de la solución solo puede utilizar un área de trabajo de Log Analytics y una cuenta de Automation. Puede instalar la solución por separado en varias áreas de trabajo.
* Si una solución requiere una cuenta de Automation, el área de trabajo de Log Analytics y la cuenta de Automation solución deben estar vinculadas. Un área de trabajo de Log Analytics solo puede estar vinculada a una cuenta de Automation y viceversa.
* Para poder vincularse, el área de trabajo de Log Analytics y la cuenta de Automation deben encontrarse en el mismo grupo de recursos y la misma región. La excepción es un área de trabajo que se encuentre en la región Este de EE. UU. y una cuenta de Automation del Este de EE. UU. 2.

### <a name="create-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Creación de un vínculo entre un área de trabajo de Log Analytics y una cuenta de Automation
La forma de especificar el área de trabajo de Log Analytics y una cuenta de Automation dependerá del método de instalación de la solución.

* Al instalar una solución desde Azure Marketplace, se le pedirá una cuenta de Automation y un área de trabajo. Se creará un vínculo entre ellas, si no están ya vinculadas.
* Para las soluciones externas a Azure Marketplace, debe vincular el área de trabajo de Log Analytics y la cuenta de Automation antes de instalar la solución. Para ello, seleccione cualquier solución en Azure Marketplace y seleccione el área de trabajo de Log Analytics y la cuenta de Automation. No tendrá que instalar la solución en sí, ya que el vínculo se crea en cuanto se seleccionan el área de trabajo de Log Analytics y la cuenta de Automation. Cuando se cree el vínculo, podrá usar esa área de trabajo de Log Analytics y la cuenta de Automation para cualquier solución.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Comprobación de un vínculo entre un área de trabajo de Log Analytics y una cuenta de Automation
Puede comprobar el vínculo entre un área de trabajo de Log Analytics y una cuenta de Automation mediante el procedimiento siguiente.

1. Seleccione la cuenta de Automation en Azure Portal.
1. Desplácese hasta la sección **Recursos relacionados** del menú.
1. Si el **Área de trabajo** está habilitada, esta cuenta se vincula a un área de trabajo de Log Analytics. Puede hacer clic en **Área de trabajo** para ver sus detalles.

## <a name="remove-a-management-solution"></a>Eliminación de una solución de administración
Para eliminar una solución instalada, busque en la [lista de soluciones instaladas](#list-installed-monitoring-solutions). Haga clic en el nombre de la solución para abrir su página de resumen y haga clic en **Eliminar**.




## <a name="next-steps"></a>Pasos siguientes
* Obtenga un [lista de soluciones de administración de Microsoft](solutions-inventory.md).
* Aprenda a [crear consultas](../../azure-monitor/log-query/log-query-overview.md) para analizar los datos recopilados por las soluciones de administración.

