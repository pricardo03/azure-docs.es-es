---
title: 'Ejemplo: los servicios compartidos de ISO 27001 blueprint: pasos de implementación'
description: Implemente los pasos de la muestra de plano técnico de servicios compartidos de ISO 27001.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 64259d029ce67ecfd5db4b97fc165be0ee391ab8
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2019
ms.locfileid: "58008082"
---
# <a name="deploy-the-azure-blueprints-iso-27001-shared-services-blueprint-sample"></a>Implementar el ejemplo de plano técnico de servicios compartidos de Azure planos ISO 27001

Para implementar el ejemplo de plano técnico de servicios compartidos de Azure planos ISO 27001, deben seguir los pasos siguientes:

> [!div class="checklist"]
> - Crear una nueva instancia de blueprint del ejemplo
> - Marcar la copia de la muestra como **publicada**
> - Asignar su copia de la instancia de blueprint a una suscripción existente

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de empezar.

## <a name="create-blueprint-from-sample"></a>Crear plano técnico de ejemplo

En primer lugar, puede implementar el ejemplo de la instancia de blueprint mediante la creación de una nueva instancia de blueprint en su entorno mediante el ejemplo como un inicio.

1. Seleccione **todos los servicios** y busque y seleccione **directiva** en el panel izquierdo. En el **directiva** página, seleccione **planos**.

1. Desde el **Introducción** página a la izquierda, seleccione el **crear** situado bajo _crear una instancia de blueprint_.

1. Buscar el **ISO 27001: Servicios compartidos** ejemplo blueprint en _otros ejemplos_ y seleccione **usar este ejemplo**.

1. Escriba el _Fundamentos_ del ejemplo de plano técnico:

   - **Nombre de instancia de blueprint**: Proporcione un nombre para la copia de la muestra de plano técnico de servicios compartidos de ISO 27001.
   - **Ubicación de la definición**: Use los puntos suspensivos y seleccione el grupo de administración para guardar la copia de la muestra.

1. Seleccione el _artefactos_ ficha en la parte superior de la página o **siguiente: Artefactos** en la parte inferior de la página.

1. Revise la lista de artefactos que componen el ejemplo de la instancia de blueprint. Muchos de los artefactos tienen parámetros que definirá más tarde. Seleccione **Guardar borrador** cuando haya terminado de revisar el ejemplo de la instancia de blueprint.

## <a name="publish-the-sample-copy"></a>Publicar la copia de ejemplo

Ahora se creó la copia de la muestra de plano técnico en su entorno. Se crea en **borrador** modo y debe ser **publicada** antes de se puede asignar e implementado. La copia de la muestra de plano técnico se puede personalizar para su entorno y necesidades, pero esa modificación puede mover fuera de la norma ISO 27001.

1. Seleccione **todos los servicios** y busque y seleccione **directiva** en el panel izquierdo. En el **directiva** página, seleccione **planos**.

1. Seleccione el **definiciones del plano** página a la izquierda. Use los filtros para buscar su copia de la muestra de plano técnico y, a continuación, selecciónelo.

1. Seleccione **publicar plano** en la parte superior de la página. En la nueva página a la derecha, proporcione un **versión** para la copia de la muestra de la instancia de blueprint. Esta propiedad es útil si realiza una modificación posterior. Proporcionar **cambiar notas** como "primera versión publicada del ejemplo ISO 27001 de plano técnico". A continuación, seleccione **publicar** en la parte inferior de la página.

## <a name="assign-the-sample-copy"></a>Asignar la copia de ejemplo

Una vez que la copia de la muestra de plano técnico ha sido correctamente **publicada**, se puede asignar a una suscripción dentro del grupo de administración se ha guardado. Este paso es donde se proporcionan parámetros para que cada implementación de la copia de la muestra de la instancia de blueprint único.

1. Seleccione **todos los servicios** y busque y seleccione **directiva** en el panel izquierdo. En el **directiva** página, seleccione **planos**.

1. Seleccione el **definiciones del plano** página a la izquierda. Use los filtros para buscar su copia de la muestra de plano técnico y, a continuación, selecciónelo.

1. Seleccione **asignar blueprint** en la parte superior de la página de definición de plano técnico.

1. Proporcione los valores de parámetro para la asignación del plano técnico:

   - Aspectos básicos

     - **Suscripciones**: Seleccione una o varias suscripciones que están en el grupo de administración que guardan la copia de la muestra de plano técnico. Si selecciona más de una suscripción, se creará una asignación para cada mediante los parámetros especificados.
     - **Nombre de la asignación**: El nombre se rellena automáticamente según el nombre de la instancia de blueprint.
       Cambiar según sea necesario o déjelo tal cual.
     - **Ubicación**: Seleccione una región para la identidad administrada que se creará en. Azure Blueprint usa esta identidad administrada para implementar todos los artefactos del plano técnico asignado. Para más información, consulte [Identidades administradas para recursos de Azure](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Versión de la definición de plano técnico**: Elegir un **publicada** versión de la copia de la muestra de la instancia de blueprint.

   - Asignación de bloqueo

     Seleccione el bloqueo de plano técnico para su entorno. Para más información, consulte [Bloqueo de recursos en planos técnicos](../../concepts/resource-locking.md).

   - Identidad administrada

     Deje el valor predeterminado _asignado por el sistema_ administra la opción de identidad.

   - Parámetros de plano técnico

     Los parámetros definidos en esta sección se usan por muchos de los artefactos de la definición del plano técnico para proporcionar coherencia.

     - **Nombre de la organización**: Escriba un nombre corto para su organización. Esta propiedad se utiliza principalmente para asignar nombres a los recursos.
     - **Prefijo de dirección de subred de servicios compartidos**: Proporcione el valor de la notación CIDR para redes juntos los recursos implementados.
     - **Ubicación de servicios compartida**: Determina qué ubicación se implementan los artefactos en. No todos los servicios están disponibles en todas las ubicaciones. Artefactos de implementación de estos servicios ofrecen una opción de parámetro para la ubicación para implementar ese artefacto que desea.
     - **Ubicación permitida (directiva: Iniciativa del plano de la ISO 27001)**: Valor que indica las ubicaciones permitidas para grupos de recursos y recursos.
     - **Área de trabajo de análisis de registros para los agentes de máquina virtual (directiva: Iniciativa del plano de la ISO 27001)**: Especifica el identificador de recurso de un área de trabajo. Este parámetro utiliza un `concat` función para construir el identificador de recurso.

   - Parámetros de artefacto

     Los parámetros definidos en esta sección se aplican al artefacto en la que está definido. Estos parámetros son [parámetros dinámicos](../../concepts/parameters.md#dynamic-parameters) desde que se definen durante la asignación de la instancia de blueprint. Para obtener una lista completa o los parámetros de artefactos y sus descripciones, consulte [tabla de parámetros de artefacto](#artifact-parameters-table).

1. Una vez escritos todos los parámetros, seleccione **asignar** en la parte inferior de la página.

> [!WARNING]
> El servicio de plano técnico de Azure y los ejemplos de plano técnico integrados son **libre de costo**. Recursos de Azure son [el precio por producto](https://azure.microsoft.com/en-us/pricing/). Use la [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/) para estimar el costo de la ejecución de los recursos implementados por este ejemplo de la instancia de blueprint.

## <a name="artifact-parameters-table"></a>Tabla de parámetros de artefacto

En la tabla siguiente proporciona una lista de la instancia de blueprint los parámetros de artefactos:

|Nombre del artefacto|Tipo de artefacto|Nombre de parámetro|DESCRIPCIÓN|
|-|-|-|-|
|[Versión preliminar]: Implementar el agente de Log Analytics para conjuntos de escalado de máquina virtual Linux (VMSS)|Asignación de directiva|Opcional: Lista de imágenes de máquina virtual que admitieron el sistema operativo Linux para agregar al ámbito|(Opcional) Valor predeterminado es _["none"]_.|
|[Versión preliminar]: Implementar el agente de Log Analytics en máquinas virtuales Linux|Asignación de directiva|Opcional: Lista de imágenes de máquina virtual que admitieron el sistema operativo Linux para agregar al ámbito|(Opcional) Valor predeterminado es _["none"]_.|
|[Versión preliminar]: Implementar el agente de Log Analytics para conjuntos de escalado de máquinas virtuales (VMSS) de Windows|Asignación de directiva|Opcional: Lista de imágenes de máquina virtual que admitieron el sistema operativo de Windows para agregar al ámbito|(Opcional) Valor predeterminado es _["none"]_.|
|[Versión preliminar]: Implementar el agente de Log Analytics en máquinas virtuales Windows|Asignación de directiva|Opcional: Lista de imágenes de máquina virtual que admitieron el sistema operativo de Windows para agregar al ámbito|(Opcional) Valor predeterminado es _["none"]_.|
|Tipos de recursos permitidos|Asignación de directiva|Tipos de recursos permitidos|Lista de tipos de recursos permitidos para implementarse. Esta lista se compone de todos los tipos de recursos implementados en los servicios compartidos.|
|SKU de cuenta de almacenamiento permitida|Asignación de directiva|Permite las SKU de almacenamiento|Lista de diagnóstico de los registros permiten las SKU de la cuenta de almacenamiento. Valor predeterminado es _["Standard_LRS"]_.|
|SKU de máquina virtual permitida|Asignación de directiva|Lista de SKU de máquina virtual pueden implementarse. Valor predeterminado es _["Standard_DS1_v2", "Standard_DS2_v2"]_.|
|Iniciativa de plano técnico para ISO 27001|Asignación de directiva|Tipos de recursos para los registros de diagnóstico de auditoría|Lista de tipos de recursos para auditar si la opción de registro de diagnóstico no está habilitada. Los valores aceptables se pueden encontrar en [esquemas de los registros de diagnóstico de Azure Monitor](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type).|
|Grupo de recursos de log Analytics|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-sharedsvsc-log-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de log Analytics|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de Log Analytics|Plantilla de Resource Manager|Nivel de servicio|Establece el nivel del área de trabajo de Log Analytics. Valor predeterminado es _PerNode_.|
|Plantilla de Log Analytics|Plantilla de Resource Manager|Retención del registro en días|Retención de datos en días. Valor predeterminado es _365_.|
|Plantilla de Log Analytics|Plantilla de Resource Manager|Ubicación|Región que se usa para crear el área de trabajo de Log Analytics. Valor predeterminado es _oeste de EE.UU. 2_.|
|Grupo de recursos de red|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-sharedsvcs-net-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de red|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de Azure Firewall|Plantilla de Resource Manager|IP privada de Azure Firewall|Configura la dirección IP privada de la [firewall de Azure](../../../../firewall/overview.md). Este valor también se usa como tabla de rutas predeterminadas en la subred de servicios compartidos. Debe ser parte de la notación CIDR definida en **prefijo de dirección de subred de Firewall de Azure**. Valor predeterminado es _10.0.4.4_.|
|Plantilla de Azure Firewall|Plantilla de Resource Manager|Retención del registro en días|Retención de datos en días. Valor predeterminado es _365_.|
|Plantilla del grupo de seguridad de red|Plantilla de Resource Manager|Retención del registro en días|Retención de datos en días. Valor predeterminado es _365_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Prefijo de dirección de Virtual Network|La notación CIDR para la red virtual. Valor predeterminado es _10.0.0.0/16_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Habilitar DDoS Protection en la red virtual|Configura protección contra DDoS para la red virtual. Valor predeterminado es _true_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Comparten el prefijo de dirección de subred de servicios|La notación CIDR para la subred de servicios compartidos. Valor predeterminado es _10.0.0.0/24_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Prefijo de la dirección de subred DMZ|La notación CIDR para la subred de red Perimetral. Valor predeterminado es _10.0.1.0/24_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Prefijo de dirección de subred de Application Gateway|La notación CIDR para la subred de puerta de enlace de aplicaciones. Valor predeterminado es _10.0.2.0/24_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Prefijo de dirección de subred de la puerta de enlace de Virtual Network|La notación CIDR para la subred de puerta de enlace de red virtual. Valor predeterminado es _10.0.3.0/24_.|
|Plantilla de Virtual Network y de la tabla de rutas|Plantilla de Resource Manager|Prefijo de la dirección de subred de Azure Firewall|La notación CIDR para la [firewall de Azure](../../../../firewall/overview.md) subred. Debe incluir el **dirección IP privada de firewall de Azure** parámetro.|
|Grupo de recursos de Key Vault|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-sharedsvcs-kv-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de Key Vault|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de Key Vault|Plantilla de Resource Manager|Nombre de usuario del administrador de jumpbox|Nombre de usuario para el jumpbox. Debe coincidir con el mismo valor de propiedad en **Jumpbox plantilla**. Valor predeterminado es _jb-admin-user_.|
|Plantilla de Key Vault|Plantilla de Resource Manager|Contraseña o clave SSH del administrador de jumpbox|Clave o contraseña de la cuenta en el jumpbox. Debe coincidir con el mismo valor de propiedad en **Jumpbox plantilla**. No tiene valor predeterminado de valor y no puede dejarse en blanco.|
|Plantilla de Key Vault|Plantilla de Resource Manager|Nombre de usuario del administrador de dominio|Nombre de usuario usado para tener acceso a la máquina virtual de Active Directory y unirse a otras máquinas virtuales a un dominio. Debe coincidir con **usuario Administrador de dominio** valor de propiedad en **plantilla de servicios de dominio de Active Directory**. Valor predeterminado es _usuario de administrador de dominio_.|
|Plantilla de Key Vault|Plantilla de Resource Manager|Contraseña del administrador de dominio|Contraseña del usuario del Administrador de dominio. No tiene valor predeterminado de valor y no puede dejarse en blanco.|
|Plantilla de Key Vault|Plantilla de Resource Manager|Id. de objeto de AAD|El identificador de objeto AAD de la cuenta que requiere acceso a la instancia de Key Vault. No tiene valor predeterminado de valor y no puede dejarse en blanco. Para buscar este valor desde el portal de Azure, busque y seleccione "Usuarios" en _servicios_. Use la _nombre_ cuadro Filtrar por el nombre de cuenta y seleccione esa cuenta. En el _perfil de usuario_ , seleccione el icono "Haga clic para copiar" junto a la _Id. de objeto_.  |
|Plantilla de Key Vault|Plantilla de Resource Manager|Retención del registro en días|Retención de datos en días. Valor predeterminado es _365_.|
|Plantilla de Key Vault|Plantilla de Resource Manager|SKU de Key Vault|Especifica la SKU del almacén de claves que se crea. Valor predeterminado es _Premium_.|
|Grupo de recursos de Jumpbox|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-sharedsvcs-jb-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de Jumpbox|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de jumpbox|Plantilla de Resource Manager|Nombre de usuario del administrador de jumpbox|El nombre de usuario utilizado para tener acceso a máquinas virtuales de jumpbox. Debe coincidir con el mismo valor de propiedad en **plantilla de Key Vault**. Valor predeterminado es _jb-admin-user_.|
|Plantilla de jumpbox|Plantilla de Resource Manager|Contraseña de administrador de Jumpbox (identificador de recurso de Key Vault)|El identificador de recurso del almacén de claves. Use "/ subscriptions/{subscriptionId}/resourceGroups/{orgName}-sharedsvcs-kv-rg/providers/Microsoft.KeyVault/vaults/{orgName}-sharedsvcs-kv" y reemplace `{subscriptionId}` con el identificador de suscripción y `{orgName}` con el  **Nombre de la organización** parámetro del plano.|
|Plantilla de jumpbox|Plantilla de Resource Manager|Contraseña de administrador de Jumpbox (nombre de secreto de Key Vault)|Nombre de usuario del Administrador de jumpbox. Debe coincidir con el valor en **plantilla de Key Vault** propiedad **Jumpbox admin username**.|
|Plantilla de jumpbox|Plantilla de Resource Manager|Sistema operativo de jumpbox|Determina el sistema operativo de la máquina virtual de jumpbox. Valor predeterminado es _Windows_.|
|Grupo de recursos de servicios de dominio de Active Directory|Grupos de recursos|NOMBRE|**Bloqueado** -concatena el **nombreDeOrganización** con `-sharedsvcs-adds-rg` para que sea el grupo de recursos único.|
|Grupo de recursos de servicios de dominio de Active Directory|Grupos de recursos|Ubicación|**Bloqueado** -utiliza el parámetro de plano técnico.|
|Plantilla de Active Directory Domain Services|Plantilla de Resource Manager|Nombre de usuario del administrador de dominio|Nombre de usuario para el jumpbox ADDS. Debe coincidir con el mismo valor de propiedad en **plantilla de Key Vault**. Valor predeterminado es _usuario administrador agrega_.|
|Plantilla de Active Directory Domain Services|Plantilla de Resource Manager|Contraseña de administrador de dominio (identificador de recurso de Key Vault)|El identificador de recurso del almacén de claves. Use "/ subscriptions/{subscriptionId}/resourceGroups/{orgName}-sharedsvcs-kv-rg/providers/Microsoft.KeyVault/vaults/{orgName}-sharedsvcs-kv" y reemplace `{subscriptionId}` con el identificador de suscripción y `{orgName}` con el  **Nombre de la organización** parámetro del plano.|
|Plantilla de Active Directory Domain Services|Plantilla de Resource Manager|Contraseña de administrador de dominio (nombre de secreto de Key Vault)|Nombre de usuario de administrador de dominio. Debe coincidir con el valor en **plantilla de Key Vault** propiedad **nombre de usuario de administrador de dominio**.|
|Plantilla de Active Directory Domain Services|Plantilla de Resource Manager|Nombre de dominio|Nombre de Active Directory creado por el ejemplo. Valor predeterminado es _contoso.com_.|
|Plantilla de Active Directory Domain Services|Plantilla de Resource Manager|Usuario Administrador de dominio|Nombre de usuario para la cuenta de administrador de AD y para unir dispositivos al dominio de AD. Debe coincidir con **AD admin username** valor de propiedad en **plantilla de Key Vault**. Valor predeterminado es _usuario de administrador de dominio_.|
|Plantilla de Active Directory Domain Services|Plantilla de Resource Manager|Contraseña del administrador de dominio|Establezca los detalles de Key Vault para almacenar la contraseña. No tiene valor predeterminado de valor y no puede dejarse en blanco.|

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado los pasos para implementar el ejemplo de plano técnico de servicios compartidos de ISO 27001, visite los siguientes artículos para obtener información sobre la arquitectura y la asignación de controles:

> [!div class="nextstepaction"]
> [Plano técnico de servicios compartidos de ISO 27001: información general sobre](./index.md)
> [plano técnico de servicios compartidos de ISO 27001: asignación de controles](./control-mapping.md)

Artículos de suma sobre planos técnicos y cómo utilizarlos:

- Obtenga información sobre la [ciclo de vida del plano](../../concepts/lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar la [blueprint orden secuenciación](../../concepts/sequencing-order.md).
- Obtenga información sobre cómo hacer uso de [bloqueo de recursos del plano](../../concepts/resource-locking.md).
- Obtenga información sobre cómo [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).