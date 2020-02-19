---
title: Directivas de punto de conexión de servicio de red virtual de Azure | Microsoft Docs
description: Aprenda a filtrar el tráfico de la red virtual para los recursos de servicio de Azure con las directivas de punto de conexión de servicio.
services: virtual-network
documentationcenter: na
author: sumeetmittal
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sumi
ms.openlocfilehash: 1aa4328a6d5367ef356ce33807289a873c93d90f
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77056706"
---
# <a name="virtual-network-service-endpoint-policies-preview"></a>Directivas de punto de conexión de servicio de red virtual (versión preliminar)

Las directivas de punto de conexión de servicio de la red virtual le permiten filtrar el tráfico de red virtual a los servicios de Azure, lo que permite únicamente recursos de servicio específicos de Azure, sobre los puntos de conexión de servicio. Las directivas de punto de conexión ofrecen un control de acceso pormenorizado para el tráfico de red virtual a los servicios de Azure.

Esta característica está disponible en __versión preliminar__ en todas las regiones públicas de Azure para Azure Storage.

Para las notificaciones más actualizadas para la versión preliminar, consulte la página [Actualizaciones de Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network).

> [!NOTE]  
> Durante la versión preliminar, las directivas de punto de conexión de servicio de red virtual no pueden tener el mismo nivel de disponibilidad y confiabilidad que las características que se encuentran en las versiones de disponibilidad general. Para obtener más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="key-benefits"></a>Ventajas principales

Las directivas de puntos de conexión de servicio de red virtual proporcionan las ventajas siguientes:

- __Seguridad mejorada para el tráfico de la red virtual a los servicios de Azure__

  [Las etiquetas de servicio de Azure para grupos de seguridad de red](https://aka.ms/servicetags) le permiten restringir el tráfico de salida de la red virtual a servicios de Azure específicos. Sin embargo, esto permite el tráfico a cualquier recurso de ese servicio de Azure. 
  
  Con las directivas de punto de conexión, ahora puede restringir el acceso de salida de red virtual a solo recursos específicos de Azure. Esto proporciona un control de seguridad mucho más pormenorizado para proteger los datos a los que se accede en la red virtual. 

- __Directivas escalables y altamente disponibles para filtrar el tráfico del servicio de Azure__

   Las directivas de punto de conexión proporcionan una solución horizontal escalable y de alta disponibilidad para filtrar el tráfico de servicio de Azure desde las redes virtuales, a través de los puntos de extremo de servicio. No se requiere ninguna sobrecarga adicional para mantener los dispositivos de red central para este tráfico en las redes virtuales.

## <a name="configuration"></a>Configuración

- Puede configurar las directivas de punto de conexión para restringir el tráfico de la red virtual a recursos de servicio específicos de Azure. Para la versión preliminar, se admiten las directivas de punto de conexión para Azure Storage. 
- La directiva de puntos de conexión se configura en una subred de una red virtual. Los puntos de conexión de servicio deben estar habilitados en la subred para aplicar la directiva, para todos los servicios de Azure enumerados en la directiva.
- La directiva de punto de conexión le permite crear una lista de permitidos de recursos de servicio de Azure específicos, mediante el formato resourceID. Puede restringir el acceso a todos los recursos en una suscripción o grupo de recursos. También puede restringir el acceso a recursos específicos. 
- De forma predeterminada, si no hay directivas adjuntas a una subred con puntos de conexión, puede acceder a todos los recursos del servicio. Una vez configurada una directiva en esa subred, solo se puede acceder a los recursos especificados en la directiva desde las instancias de proceso de esa subred. Se denegará el acceso a todos los demás recursos, para el servicio específico. 
- Puede filtrar el tráfico a los recursos de servicio de Azure en las regiones donde se configura el punto de conexión de servicio. De forma predeterminada, se podrá tener acceso a recursos de servicio en otras regiones. 

  > [!NOTE]  
  > Para bloquear completamente el acceso de salida de la red virtual a los recursos de Azure en las regiones de punto de conexión de servicio, también necesita reglas de grupo de seguridad de red configuradas para permitir el tráfico solo a las regiones de punto de conexión de servicio, mediante [etiquetas de servicio](security-overview.md#service-tags).

- Puede aplicar varias directivas a una subred. Cuando se asocian varias directivas a la subred, para el mismo servicio, se permitirá el tráfico de red virtual a los recursos especificados en cualquiera de estas directivas. Se denegará el acceso a todos los demás recursos del servicio, que no se estén especificados en ninguna de las directivas. 

  > [!NOTE]  
  > La directiva solo permite el acceso a los recursos de servicio enumerados en una red virtual. Al agregar recursos específicos a la directiva, se deniega automáticamente todo el tráfico restante al servicio. Asegúrese de que todas las dependencias de recursos de servicio de las aplicaciones puedan identificarse y enumerarse en la directiva.

  > [!WARNING]  
  > Los servicios de Azure implementados en la red virtual, como Azure HDInsight, acceden a otros servicios de Azure, como Azure Storage, para saber los requisitos de infraestructura. Restringir la directiva de punto de conexión a recursos específicos podría interrumpir el acceso a estos recursos de infraestructura para los servicios implementados en la red virtual. Para servicios específicos, consulte [Limitaciones](#limitations). Durante la versión preliminar, las directivas de punto de conexión de servicio no son compatibles con ningún servicio administrado de Azure que se implemente en la red virtual.

- Para Azure Storage: 
  -  Puede restringir el acceso mediante la enumeración de *resourceId* de Azure Resource Manager de la cuenta de almacenamiento. Esto trata el tráfico a blobs, tablas, colas, archivos y a Azure Data Lake Storage Gen2.

     Por ejemplo, las cuentas de almacenamiento de Azure se pueden enumerar en la definición de la directiva de punto de conexión como se indica a continuación:
    
     Para permitir una cuenta de almacenamiento específica:         
     `subscriptions/subscriptionId/resourceGroups/resourceGroup/providers/Microsoft.Storage/storageAccounts/storageAccountName`
      
     Para permitir todas las cuentas en un grupo de recursos y suscripciones: `/subscriptions/subscriptionId/resourceGroups/resourceGroup`
     
     Para permitir todas las cuentas en una suscripción: `/subscriptions/subscriptionId`
    
- Solo las cuentas de almacenamiento que utilizan el modelo de recursos de Azure pueden especificarse en la directiva de punto de conexión.  

  > [!NOTE]  
  > Se bloquea el acceso a las cuentas de almacenamiento clásicas con directivas de punto de conexión.

- La ubicación principal de la cuenta indicada debe estar en las regiones del par de replicación geográfica del punto de conexión de servicio para la subred. 

  > [!NOTE]  
  > Las directivas permiten especificar los recursos de servicio de otras regiones. El acceso de red virtual a los servicios de Azure solo se filtra para las regiones del par de replicación geográfica. Si los grupos de seguridad de red no están restringidos a las regiones del par de replicación geográfica para Azure Storage, la red virtual puede acceder a todas las cuentas de almacenamiento fuera de las regiones del par de replicación geográfica.

- El acceso secundario a RA-GRS se permitirá automáticamente si la cuenta principal aparece en la lista. 
- Las cuentas de almacenamiento pueden estar en la misma suscripción o en una diferente o en un inquilino de Azure Active Directory que la red virtual. 

## <a name="limitations"></a>Limitaciones

- Solo puede implementar las directivas de punto de conexión de servicio en las redes virtuales implementadas con el modelo de implementación de Azure Resource Manager.
- Las redes virtuales deben estar en la misma región que la directiva de punto de conexión de servicio.
- Solo puede aplicar la directiva de punto de conexión de servicio en una subred si los puntos de conexión de servicio están configurados para los servicios de Azure enumerados en la directiva.
- No puede utilizar las directivas de punto de conexión de servicio para el tráfico de la red local a los servicios de Azure.
- Las directivas de punto de conexión no deben aplicarse a subredes con servicios de Azure administrados, con dependencia de los servicios de Azure para los requisitos de infraestructura. 

  > [!WARNING]  
  > Los servicios de Azure implementados en la red virtual, como Azure HDInsight, acceden a otros servicios de Azure, como Azure Storage, para saber los requisitos de infraestructura. Restringir la directiva de punto de conexión a recursos específicos podría interrumpir el acceso a estos recursos de infraestructura para los servicios de Azure implementados en la red virtual.
  
  - Algunos servicios de Azure pueden implementarse en subredes con otras instancias de proceso. Asegúrese de que las directivas de punto de conexión no se aplican a la subred si los servicios administrados que se enumeran a continuación se implementan en la subred.
   
    - HDInsight de Azure
    - Azure Batch (Azure Resource Manager)
    - Azure Active Directory Domain Services (Azure Resource Manager)
    - Azure Application Gateway (Azure Resource Manager)
    - Azure VPN Gateway (Azure Resource Manager)
    - Azure Firewall

  - Algunos servicios de Azure se implementan en subredes dedicadas. Las directivas de punto de conexión se bloquean en todos estos servicios, que se enumeran a continuación, durante la versión preliminar. 

     - Azure App Service Environment
     - Azure Rediscache
     - Azure API Management
     - Instancia administrada de Azure SQL
     - Azure Active Directory Domain Services
     - Azure Application Gateway (clásico)
     - Azure VPN Gateway (clásico)

- Azure Storage: no se admiten las cuentas de almacenamiento clásicas en las directivas de punto de conexión. Las directivas denegarán el acceso a todas las cuentas de almacenamiento clásicas de forma predeterminada. Si su aplicación necesita acceso a Azure Resource Manager y a las cuentas de almacenamiento clásicas, las directivas de punto de conexión no deben usarse para este tráfico. 

## <a name="nsgs-with-service-endpoint-policies"></a>Grupos de seguridad de red con directivas de punto de conexión de servicio
- De forma predeterminada, los grupos de seguridad de red permiten el tráfico saliente de Internet, incluido el tráfico de red virtual a los servicios de Azure.
- Si desea denegar todo el tráfico de Internet saliente y permitir únicamente el tráfico a recursos de servicios de Azure: 

  Paso 1: Configure los grupos de seguridad de red para permitir el tráfico de salida solo a los servicios de Azure en las regiones de punto de conexión, mediante las *etiquetas de servicios de Azure*. Para más información, consulte las [etiquetas de servicio para grupos de seguridad de red](https://aka.ms/servicetags).
      
  Por ejemplo, las reglas de grupos de seguridad de red que restringen el acceso solo a las regiones de punto de conexión se parecen al siguiente ejemplo:

  ```
  Allow AzureStorage.WestUS2,
  Allow AzureStorage.WestCentralUS,
  Deny all
  ```

  Paso 2: Aplique la directiva de punto de conexión de servicio con acceso únicamente a los recursos de servicios de Azure específico.

  > [!WARNING]  
  > Si el grupo de seguridad de red no está configurado para limitar el acceso de servicio de Azure de una red virtual a las regiones de punto de conexión, puede acceder a los recursos de servicio de otras regiones, incluso si se aplica la directiva de punto de conexión de servicio.

## <a name="scenarios"></a>Escenarios

- **Redes virtuales emparejadas, conectadas o múltiples**: para filtrar el tráfico en redes virtuales emparejadas, las directivas de punto de conexión deben aplicarse individualmente a estas redes virtuales.
- **Filtrado del tráfico de Internet con dispositivos de red o Azure Firewall**: filtre el tráfico del servicio de Azure con directivas, sobre puntos de conexión, y filtre el resto del tráfico de Internet o de Azure a través de dispositivos o de Azure Firewall. 
- **Filtrado del tráfico en servicios de Azure implementados en redes virtuales**: durante la versión preliminar, las directivas de punto de conexión de servicio no son compatibles con ningún servicio administrado de Azure que se implemente en la red virtual. 
 Para ver servicios específicos, consulte [Limitaciones.](#limitations)
- **Filtrado del tráfico a los servicios de Azure desde el entorno local**: las directivas de punto de conexión de servicio solo se aplican al tráfico de las subredes asociadas a las directivas. Para permitir el acceso a recursos de servicio específicos de Azure desde el entorno local, el tráfico debe filtrarse utilizando aplicaciones de redes virtuales o firewalls.

## <a name="logging-and-troubleshooting"></a>Registro y solución de problemas
No se dispone de un registro centralizado para las directivas de punto de conexión de servicio. Para los registros de diagnóstico de servicio, consulte [Registro de puntos de conexión de servicio](virtual-network-service-endpoints-overview.md#logging-and-troubleshooting).

### <a name="troubleshooting-scenarios"></a>Escenarios de solución de problemas
- Acceso permitido a cuentas de almacenamiento no enumeradas en las directivas de punto de conexión
  - Los grupos de seguridad de red pueden estar permitiendo el acceso a Internet o a cuentas de Azure Storage en otras regiones.
  - Los grupos de seguridad de red deben configurarse para denegar todo el tráfico de salida de Internet y permitir solo el tráfico a regiones específicas de Azure Storage. Para más información, consulte Grupos de seguridad de red.
- Se niega el acceso a las cuentas enumeradas en las directivas de punto de conexión.
  - Los grupos de seguridad de red o el filtrado de firewall podrían estar bloqueando el acceso.
  - Si al quitar o volver a aplicar la directiva se produce una pérdida de conectividad:
    - Valide si el servicio de Azure está configurado para permitir el acceso desde la red virtual, a través de los puntos de conexión, o que la directiva predeterminada para el recurso se establece en *Permitir todo*.
      > [!NOTE]      
      > No es necesario asegurar los recursos de servicio a las redes virtuales para obtener acceso a través de las directivas de punto de conexión. Sin embargo, como procedimiento recomendado de seguridad, recomendamos que los recursos de servicio estén protegidos en redes de confianza, como las redes virtuales de Azure, a través de los puntos de conexión de servicio, y en los entornos locales, mediante un firewall IP.
  
    - Valide que los diagnósticos de servicio muestran el tráfico sobre los puntos de conexión.
    - Compruebe si los registros de flujo de grupo de seguridad de red muestran el acceso y que los registros de almacenamiento muestran el acceso, como se esperaba, a los puntos de conexión de servicio.
    - Póngase en contacto con el equipo de soporte técnico de Azure.
- Se niega el acceso a las cuentas no enumeradas en las directivas de punto de conexión.
  - Los grupos de seguridad de red o el filtrado de firewall podrían estar bloqueando el acceso. Asegúrese de que la etiqueta de servicio *Azure Storage* está permitida para las regiones de punto de conexión. Para las restricciones de directivas, consulte [Limitaciones](#limitations).
  Por ejemplo, a las cuentas de almacenamiento clásicas se les niega el acceso si se aplica una directiva.
  - Valide si el servicio de Azure está configurado para permitir el acceso desde la red virtual, a través de los puntos de conexión, o si la directiva predeterminada para el recurso se establece en *Permitir todo*.

## <a name="provisioning"></a>Aprovisionamiento

Las directivas de punto de conexión de servicio pueden configurarse en subredes por un usuario con acceso de escritura a una red virtual. Obtenga más información sobre los [roles integrados](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de Azure y la asignación de permisos específicos a [roles personalizados](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Las redes virtuales y los recursos de servicio de Azure pueden encontrarse en la misma o en diferentes suscripciones, o en inquilinos de Azure Active Directory. 

## <a name="pricing-and-limits"></a>Precios y límites

No hay ningún cargo adicional para el uso de directivas de punto de conexión de servicio. El modelo de precios vigente para los servicios de Azure (como Azure Storage) se aplica tal cual, sobre los punto de conexión de servicio.

Los límites siguientes se aplican en las directivas de punto de conexión de servicio: 

 |Resource | Límite predeterminado |
 |---------|---------------|
 |ServiceEndpointPoliciesPerSubscription |500 |
 |ServiceEndpintPoliciesPerSubnet|100 |
 |ServiceResourcesPerServiceEndpointPolicyDefinition|200 |

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [configurar las directivas de puntos de conexión de servicio de red virtual](virtual-network-service-endpoint-policies-portal.md).
- Aprenda más sobre los [puntos de conexión de servicio de red virtual](virtual-network-service-endpoints-overview.md).

