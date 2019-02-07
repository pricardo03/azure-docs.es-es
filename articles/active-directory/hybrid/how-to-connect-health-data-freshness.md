---
title: 'Azure AD Connect Health: Alerta sobre que los datos del servicio de mantenimiento no están actualizados | Documentos de Microsoft'
description: Este documento describe la causa de la alerta "Los datos del servicio de mantenimiento no están actualizados" y cómo solucionar el problema.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 3f97e9696a5138d9102037103a45c86988a7506c
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55489282"
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

### <a name="connect-health-for-sync"></a>Connect Health para sincronización

| Elementos de datos | Pasos para solucionar problemas |
| --- | --- | 
| PerfCounter | - [Conectividad saliente al punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [La inspección de SSL para el tráfico saliente se filtra o se deshabilita](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Puertos del firewall en el servidor que ejecuta el agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Permitir los sitios web designados si la seguridad mejorada de IE está habilitada](https://technet.microsoft.com/windows/ms537180(v=vs.60)) |
| AadSyncService-SynchronizationRules, <br /> AadSyncService-Connectors, <br /> AadSyncService-GlobalConfigurations, <br /> AadSyncService-RunProfileResults, <br /> AadSyncService-ServiceConfigurations, <br /> AadSyncService-ServiceStatus | - Conectividad saliente basada en direcciones IP; vea los [intervalos de IP de Azure](https://www.microsoft.com/download/details.aspx?id=41653). <br /> - [Conectividad saliente al punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Puertos del firewall en el servidor que ejecuta el agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>Connect Health para ADFS

Realice pasos adicionales para validar en AD FS y siga el flujo de trabajo de la [Ayuda de AD FS](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282).

| Elementos de datos | Pasos para solucionar problemas |
| --- | --- | 
| PerfCounter, TestResult | - [Conectividad saliente al punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [La inspección de SSL para el tráfico saliente se filtra o se deshabilita](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Puertos del firewall en el servidor que ejecuta el agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Permitir los sitios web designados si la seguridad mejorada de IE está habilitada](https://technet.microsoft.com/windows/ms537180(v=vs.60)) |
|  Adfs-UsageMetrics | Conectividad saliente basada en direcciones IP; vea los [intervalos de IP de Azure](https://www.microsoft.com/download/details.aspx?id=41653). | 

### <a name="connect-health-for-adds"></a>Connect Health para ADDS

| Elementos de datos | Pasos para solucionar problemas |
| --- | --- | 
| PerfCounter, Adds-TopologyInfo-Json, Common-TestData-Json | - [Conectividad saliente al punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> - [La inspección de SSL para el tráfico saliente se filtra o se deshabilita](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Puertos del firewall en el servidor que ejecuta el agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Permitir los sitios web designados si la seguridad mejorada de IE está habilitada](https://technet.microsoft.com/windows/ms537180(v=vs.60)) <br />  - Conectividad saliente basada en direcciones IP; vea los [intervalos de IP de Azure](https://www.microsoft.com/download/details.aspx?id=41653).  |


## <a name="next-steps"></a>Pasos siguientes
* [Preguntas más frecuentes de Azure AD Connect Health](reference-connect-health-faq.md)
