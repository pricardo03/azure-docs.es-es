---
title: 'Azure AD Connect Health: Alerta sobre que los datos del servicio de mantenimiento no están actualizados | Documentos de Microsoft'
description: Este documento describe la causa de la alerta "Los datos del servicio de mantenimiento no están actualizados" y cómo solucionar el problema.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: e470a44732b881311eacecfdf2bd2211598d880a
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49984867"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Alerta sobre que los datos del servicio de mantenimiento no están actualizados

## <a name="overview"></a>Información general
<li>Azure AD Connect Health genera una alerta de datos actualizada si no recibe todos los puntos de datos del servidor durante dos horas. El título de la alerta es **Los datos del servicio de mantenimiento no están actualizados**. </li>
<li>La alerta con el estado **Advertencia** se desencadena si Connect Health no recibe elementos de datos parciales enviados desde el servidor durante dos horas. La alerta con el estado de advertencia no desencadena notificaciones de correo electrónico para el administrador de inquilinos. </li>
<li>La alerta con el estado **Error** se desencadena si Connect Health no recibe ningún elemento de datos enviado desde el servidor durante dos horas. La alerta con el estado de error desencadena notificaciones de correo electrónico para el administrador de inquilinos. </li>

>[!IMPORTANT] 
> Esta alerta sigue la [directiva de retención de datos](reference-connect-health-user-privacy.md#data-retention-policy) de Connect Health

## <a name="troubleshooting-steps"></a>Pasos para solucionar problemas 
* Asegúrese de repasar y cumplir la [sección de los requisitos](how-to-connect-health-agent-install.md#requirements).
* Utilice la [herramienta de pruebas de conectividad](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) para detectar problemas de conectividad.
* Si tiene un proxy HTTP, siga [aquí los pasos de configuración](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 

### <a name="connect-health-for-adfs"></a>Connect Health para ADFS
Realice pasos adicionales para validar en AD FS y siga el flujo de trabajo de la [Ayuda de AD FS](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282).

### <a name="data-collection-map-required-steps"></a>Pasos requeridos para la asignación de colecciones de datos
| Nombre de servicio | Elementos de datos | Pasos para solucionar problemas |
| --- | --- | --- | 
| Connect Health para AD FS | PerfCounter, TestResult | - [Conectividad saliente al punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [La inspección de SSL para el tráfico saliente se filtra o se deshabilita](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Puertos del firewall en el servidor que ejecuta el agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Permitir los sitios web designados si la seguridad mejorada de IE está habilitada](https://support.microsoft.com/help/815141/internet-explorer-enhanced-security-configuration-changes-the-browsing) |
|  | Adfs-UsageMetrics | Conectividad saliente basada en direcciones IP; vea los [intervalos de IP de Azure](https://www.microsoft.com/download/details.aspx?id=41653). | 
| Connect Health para sincronización | PerfCounter | - [Conectividad saliente al punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [La inspección de SSL para el tráfico saliente se filtra o se deshabilita](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Puertos del firewall en el servidor que ejecuta el agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Permitir los sitios web designados si la seguridad mejorada de IE está habilitada](https://support.microsoft.com/help/815141/internet-explorer-enhanced-security-configuration-changes-the-browsing) |
|  | AadSyncService-SynchronizationRules, <br /> AadSyncService-Connectors, <br /> AadSyncService-GlobalConfigurations, <br /> AadSyncService-RunProfileResults, <br /> AadSyncService-ServiceConfigurations, <br /> AadSyncService-ServiceStatus | - Conectividad saliente basada en direcciones IP; vea los [intervalos de IP de Azure](https://www.microsoft.com/download/details.aspx?id=41653). <br /> - [Conectividad saliente al punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Puertos del firewall en el servidor que ejecuta el agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 
| Connect Health para ADDS  | PerfCounter, Adds-TopologyInfo-Json, Common-TestData-Json | - [Conectividad saliente al punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> - [La inspección de SSL para el tráfico saliente se filtra o se deshabilita](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Puertos del firewall en el servidor que ejecuta el agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Permitir los sitios web designados si la seguridad mejorada de IE está habilitada](https://support.microsoft.com/help/815141/internet-explorer-enhanced-security-configuration-changes-the-browsing) <br />  - Conectividad saliente basada en direcciones IP; vea los [intervalos de IP de Azure](https://www.microsoft.com/download/details.aspx?id=41653).  |




## <a name="next-steps"></a>Pasos siguientes
* [Preguntas más frecuentes de Azure AD Connect Health](reference-connect-health-faq.md)
