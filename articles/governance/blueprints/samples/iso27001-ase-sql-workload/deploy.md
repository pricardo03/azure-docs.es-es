---
title: 'Ejemplo: plano de carga de trabajo de ISO 27001 ASE/SQL: pasos de implementación'
description: Implemente los pasos del ejemplo de plano técnico de carga de trabajo de ISO 27001 App Service entorno/SQL Database.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 4ed51ee5f8cbdc50fa65a189d8f468bd7713a74b
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58804163"
---
# <a name="deploy-the-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>Implementar el ejemplo de plano técnico de carga de trabajo de ISO 27001 App Service entorno/SQL Database

Para implementar el ejemplo de plano técnico de carga de trabajo de Azure planos ISO 27001 App Service entorno/SQL Database, deben seguir los pasos siguientes:

> [!div class="checklist"]
> - Implementar el [servicios compartidos de ISO 27001](../iso27001-shared/index.md) ejemplo plano técnico
> - Crear una nueva instancia de blueprint del ejemplo
> - Marcar la copia del ejemplo como **publicada**
> - Asignar la copia del plano técnico a una suscripción existente

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de empezar.

## <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>Implementar el ejemplo de plano técnico de servicios compartidos de ISO 27001

Antes de que se puede implementar este ejemplo de la instancia de blueprint, el [servicios compartidos de ISO 27001](../iso27001-shared/index.md) ejemplo blueprint debe implementarse en la suscripción de destino. Sin una implementación correcta de la muestra de plano técnico de servicios compartidos de ISO 27001, este ejemplo de plano técnico le faltará dependencias de infraestructura y producirá un error durante la implementación.

> [!IMPORTANT]
> En este ejemplo de plano técnico se debe asignar en la misma suscripción que la [servicios compartidos de ISO 27001](../iso27001-shared/index.md) ejemplo de la instancia de blueprint.

## <a name="create-blueprint-from-sample"></a>Crear plano técnico de ejemplo

En primer lugar, puede implementar el ejemplo de la instancia de blueprint mediante la creación de una nueva instancia de blueprint en su entorno mediante el ejemplo como un inicio.

1. Seleccione **todos los servicios** y busque y seleccione **directiva** en el panel izquierdo. En el **directiva** página, seleccione **planos**.

1. En la página **Introducción** de la izquierda, seleccione el botón **Crear** en _Creación de un plano técnico_.

1. Buscar el **ISO 27001: Carga de trabajo SQL/ASE** ejemplo blueprint en _otros ejemplos_ y seleccione **usar este ejemplo**.

1. Escriba los _conceptos básicos_ del ejemplo de plano técnico:

   - **Nombre del plano técnico**: Proporcione un nombre para la copia de la muestra de plano técnico de carga de trabajo ISO 27001 ASE/SQL.
   - **Ubicación de definición**: Use los puntos suspensivos y seleccione el grupo de administración para guardar la copia de la muestra.

1. Seleccione la pestaña _Artefactos_ en la parte superior de la página **Siguiente: Artefactos** en la parte inferior de la página.

1. Revise la lista de artefactos que componen el ejemplo de plano técnico. Muchos de los artefactos tienen parámetros que definirá más tarde. Seleccione **Guardar borrador** cuando haya terminado de revisar el ejemplo de plano técnico.

## <a name="publish-the-sample-copy"></a>Publicación de la copia de ejemplo

La copia del ejemplo de plano técnico ahora se ha creado en el entorno. Se crea en el modo **Borrador** y debe **publicarse** antes de que se pueda asignar e implementar. La copia de la muestra de plano técnico se puede personalizar para su entorno y necesidades, pero esa modificación puede mover fuera de la norma ISO 27001.

1. Seleccione **todos los servicios** y busque y seleccione **directiva** en el panel izquierdo. En el **directiva** página, seleccione **planos**.

1. Seleccione la página **Definiciones del plano técnico** de la izquierda. Use los filtros para buscar su copia de la muestra de plano técnico y, a continuación, selecciónelo.

1. Seleccione **Publicar plano técnico** en la parte superior de la página. En la nueva página a la derecha, proporcione un **versión** para la copia de la muestra de la instancia de blueprint. Esta propiedad es útil si realiza una modificación posteriormente. Proporcionar **cambiar notas** como "primera versión publicada del ejemplo ISO 27001 de plano técnico". A continuación, seleccione **Publicar** en la parte inferior de la página.

## <a name="assign-the-sample-copy"></a>Asignación de la copia de ejemplo

Una vez que la copia del ejemplo de plano técnico se haya **publicado** correctamente, se podrá asignar a una suscripción dentro del grupo de administración donde se guardó. En este paso se proporcionan los parámetros para hacer que cada implementación de la copia del ejemplo de plano técnico sea única.

1. Seleccione **todos los servicios** y busque y seleccione **directiva** en el panel izquierdo. En el **directiva** página, seleccione **planos**.

1. Seleccione la página **Definiciones del plano técnico** de la izquierda. Use los filtros para buscar su copia de la muestra de plano técnico y, a continuación, selecciónelo.

1. Seleccione **Asignar plano técnico** en la parte superior de la página de definición del plano técnico.

1. Proporcione los valores de parámetro para la asignación de plano técnico:

   - Aspectos básicos

     - **Suscripciones**: seleccione una o varias de las suscripciones que están en el grupo de administración donde guardó la copia del ejemplo de plano técnico. Si selecciona más de una suscripción, se creará una asignación para cada una mediante los parámetros especificados.
     - **Nombre de asignación**: El nombre se rellena automáticamente según el nombre de la instancia de blueprint.
       Cambiar según sea necesario o déjelo tal cual.
     - **Ubicación**: seleccione una región para la identidad administrada en la que se va a crear. Azure Blueprint usa esta identidad administrada para implementar todos los artefactos del plano técnico asignado. Para más información, consulte [Identidades administradas para recursos de Azure](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Versión de definición del plano técnico**: Elegir un **publicada** versión de la copia de la muestra de la instancia de blueprint.

   - Asignación de bloqueo

     Seleccione el bloqueo de plano técnico para su entorno. Para más información, consulte [Bloqueo de recursos en planos técnicos](../../concepts/resource-locking.md).

   - Identidad administrada

     Deje el valor predeterminado _asignado por el sistema_ administra la opción de identidad.

   - Parámetros de plano técnico

     Los parámetros definidos en esta sección se usan por muchos de los artefactos de la definición del plano técnico para proporcionar coherencia.

     - **Nombre de la organización**: Escriba un nombre corto para su organización. Esta propiedad se utiliza principalmente para asignar nombres a los recursos.
     - **Compartido Id. de suscripción del servicio**: Id. de suscripción donde el [servicios compartidos de ISO 27001](../iso27001-shared/index.md) muestra de plano técnico se asigna.
     - **Prefijo de dirección de subred predeterminada**: La notación CIDR para la subred de red virtual predeterminada.
       Valor predeterminado es _10.1.0.0/16_.
     - **Ubicación de la carga de trabajo**: Determina qué ubicación se implementan los artefactos en. No todos los servicios están disponibles en todas las ubicaciones. Artefactos de implementación de estos servicios ofrecen una opción de parámetro para la ubicación para implementar ese artefacto que desea.

   - Parámetros de artefacto

     Los parámetros definidos en esta sección se aplican al artefacto en el que se define. Estos parámetros son [parámetros dinámicos](../../concepts/parameters.md#dynamic-parameters), ya que se definen durante la asignación del plano técnico. Para obtener una lista completa o los parámetros de artefactos y sus descripciones, consulte [tabla de parámetros de artefacto](#artifact-parameters-table).

1. Una vez que se hayan especificado todos los parámetros, seleccione **Asignar** en la parte inferior de la página. Se crea la asignación del plano técnico y comience la implementación del artefacto. La implementación tarda aproximadamente una hora. Para comprobar el estado de implementación, abra la asignación del plano técnico.

> [!WARNING]
> El servicio de plano técnico de Azure y los ejemplos de plano técnico integrados son **libre de costo**. Recursos de Azure son [el precio por producto](https://azure.microsoft.com/pricing/). Use la [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/) para estimar el costo de la ejecución de los recursos implementados por este ejemplo de la instancia de blueprint.

## <a name="artifact-parameters-table"></a>Tabla de parámetros de artefacto

En la tabla siguiente proporciona una lista de la instancia de blueprint los parámetros de artefactos:

|Nombre del artefacto|Tipo de artefacto|Nombre de parámetro|DESCRIPCIÓN|
|-|-|-|-|
|Grupo de recursos de log Analytics|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-workload-log-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de log Analytics|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de Log Analytics|Plantilla de Resource Manager|Nivel de servicio|Establece el nivel del área de trabajo de Log Analytics. Valor predeterminado es _PerNode_.|
|Plantilla de Log Analytics|Plantilla de Resource Manager|Retención del registro en días|Retención de datos en días. Valor predeterminado es _365_.|
|Plantilla de Log Analytics|Plantilla de Resource Manager|Ubicación|Región que se usa para crear el área de trabajo de Log Analytics. Valor predeterminado es _oeste de EE.UU. 2_.|
|Grupo de recursos de red|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-workload-net-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de red|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla del grupo de seguridad de red|Plantilla de Resource Manager|Retención del registro en días|Retención de datos en días. Valor predeterminado es _365_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|IP privada de Azure Firewall|Configura la dirección IP privada de la [firewall de Azure](../../../../firewall/overview.md). Debe ser parte de la notación CIDR definida en _ISO 27001: Servicios compartidos_ parámetro artefacto **prefijo de dirección de subred de Firewall de Azure**. Valor predeterminado es _10.0.4.4_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Id. de suscripción de servicios compartidos|Valor que se usa para habilitar el emparejamiento de VNET entre una carga de trabajo y servicios compartidos.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Prefijo de dirección de Virtual Network|La notación CIDR para la red virtual. Valor predeterminado es _10.1.0.0/16_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Prefijo de dirección de subred predeterminado|La notación CIDR para la subred de red virtual predeterminada. Valor predeterminado es _10.1.0.0/16_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Dirección IP de ADDS|Dirección IP de la primera máquina virtual se agrega. Este valor se utiliza como DNS VNET personalizado.|
|Grupo de recursos de Key Vault|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-workload-kv-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de Key Vault|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de Key Vault|Plantilla de Resource Manager|Id. de objeto de AAD|El identificador de objeto AAD de la cuenta que requiere acceso a la instancia de Key Vault. No tiene valor predeterminado de valor y no puede dejarse en blanco. Para buscar este valor desde el portal de Azure, busque y seleccione "Usuarios" en _servicios_. Use la _nombre_ cuadro Filtrar por el nombre de cuenta y seleccione esa cuenta. En el _perfil de usuario_ , seleccione el icono "Haga clic para copiar" junto a la _Id. de objeto_.|
|Plantilla de Key Vault|Plantilla de Resource Manager|Retención del registro en días|Retención de datos en días. Valor predeterminado es _365_.|
|Plantilla de Key Vault|Plantilla de Resource Manager|SKU de Key Vault|Especifica la SKU del almacén de claves que se crea. Valor predeterminado es _Premium_.|
|Plantilla de Key Vault|Plantilla de Resource Manager|Nombre de usuario administrador de Azure SQL Server|El nombre de usuario utilizado para tener acceso a Azure SQL Server. Debe coincidir con el mismo valor de propiedad en **plantilla de Azure SQL Database**. Valor predeterminado es _usuario de administrador de sql_.|
|Grupo de recursos de Azure SQL Database|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-workload-azsql-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de Azure SQL Database|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de Azure SQL Database|Plantilla de Resource Manager|Nombre de usuario administrador de Azure SQL Server|Nombre de usuario para el servidor SQL Azure. Debe coincidir con el mismo valor de propiedad en **plantilla de Key Vault**. Valor predeterminado es _usuario de administrador de sql_.|
|Plantilla de Azure SQL Database|Plantilla de Resource Manager|Contraseña de administrador de Azure SQL Server (identificador de recurso de Key Vault)|El identificador de recurso del almacén de claves. Use "/ subscription/{subscriptionId}/resourceGroups/{orgName}-workload-kv/providers/Microsoft.KeyVault/vaults/{orgName}-workload-kv" y reemplace `{subscriptionId}` con el identificador de suscripción y `{orgName}` con el  **Nombre de la organización** parámetro del plano.|
|Plantilla de Azure SQL Database|Plantilla de Resource Manager|Contraseña de administrador de Azure SQL Server (nombre de secreto de Key Vault)|Nombre de usuario SQL de administrador del servidor. Debe coincidir con el valor en **plantilla de Key Vault** propiedad **nombre de usuario de administrador de Azure SQL Server**.|
|Plantilla de Azure SQL Database|Plantilla de Resource Manager|Retención del registro en días|Retención de datos en días. Valor predeterminado es _365_.|
|Plantilla de Azure SQL Database|Plantilla de Resource Manager|Id. de objeto de administrador AAD|Id. de objeto AAD del usuario que se asignará como un administrador de Active Directory. No tiene valor predeterminado de valor y no puede dejarse en blanco. Para buscar este valor desde el portal de Azure, busque y seleccione "Usuarios" en _servicios_. Use la _nombre_ cuadro Filtrar por el nombre de cuenta y seleccione esa cuenta. En el _perfil de usuario_ , seleccione el icono "Haga clic para copiar" junto a la _Id. de objeto_.|
|Plantilla de Azure SQL Database|Plantilla de Resource Manager|Inicio de sesión de administrador AAD|Actualmente, no se puede establecer las cuentas de Microsoft (como live.com o outlook.com) como administrador. Solo los usuarios y grupos de seguridad de su organización pueden establecerse como administrador. No tiene valor predeterminado de valor y no puede dejarse en blanco. Para buscar este valor desde el portal de Azure, busque y seleccione "Usuarios" en _servicios_. Use la _nombre_ cuadro Filtrar por el nombre de cuenta y seleccione esa cuenta. En el _perfil de usuario_ página, copie el _nombre de usuario_.|
|Grupo de recursos de App Service Environment|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-workload-ase-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de App Service Environment|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de App Service Environment|Plantilla de Resource Manager|Nombre de dominio|Nombre de Active Directory creado por el ejemplo. Valor predeterminado es _contoso.com_.|
|Plantilla de App Service Environment|Plantilla de Resource Manager|Ubicación de ASE|Ubicación de App Service Environment. Valor predeterminado es _oeste de EE.UU. 2_.|
|Plantilla de App Service Environment|Plantilla de Resource Manager|Retención del registro de Application Gateway en días|Retención de datos en días. Valor predeterminado es _365_.|

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado los pasos para implementar el ejemplo de plano técnico de carga de trabajo de ISO 27001 App Service entorno/SQL Database, visite los siguientes artículos para obtener información sobre la arquitectura y la asignación de controles:

> [!div class="nextstepaction"]
> [Plano de carga de trabajo de ISO 27001 App Service entorno/SQL Database: información general sobre](./index.md)
> [plano de la carga de trabajo de ISO 27001 App Service entorno/SQL Database: asignación de controles](./control-mapping.md)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Más información sobre el [ciclo de vida del plano técnico](../../concepts/lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).