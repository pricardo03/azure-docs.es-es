---
title: 'Azure AD Connect Health: Alerta sobre que los datos del servicio de mantenimiento no están actualizados | Documentos de Microsoft'
description: Este documento describe la causa de la alerta "Los datos del servicio de mantenimiento no están actualizados" y cómo solucionar el problema.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: SamuelD
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16794dfdcdc6ed9c2effe412237d2681fca4f394
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58803301"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Alerta sobre que los datos del servicio de mantenimiento no están actualizados

## <a name="overview"></a>Información general
Los agentes en las máquinas locales que Azure AD Connect Health supervisa periódicamente cargan los datos al servicio de Azure AD Connect Health. Si el servicio no recibe datos de un agente, la información presentada en el portal estará obsoleta. Para resaltar el problema, el servicio generará una alerta **Los datos del servicio de mantenimiento no están actualizados**. Se genera cuando el servicio no ha recibido datos completados en las últimas dos horas.  

* El **advertencia** alerta de estado se desencadena si el servicio de mantenimiento ha recibido solo **parcial** tipos de datos enviados desde el servidor en las últimas dos horas. Alerta de estado de advertencia no desencadena notificaciones por correo electrónico a destinatarios configurados. 
* El **Error** alerta de estado se desencadena si el servicio de mantenimiento no ha recibido ningún tipo de datos desde el servidor en las últimas dos horas. Los desencadenadores de alerta de estado de error notificaciones por correo electrónico a destinatarios configurados.

El servicio obtiene los datos de los agentes que se ejecutan en las máquinas locales. Según el tipo de servicio en la tabla siguiente se enumera a los agentes que se ejecutan en el equipo, lo que hacen, así como los tipos de datos generados por el servicio. En algunos casos, hay varios servicios implicados en el proceso, por lo que cualquiera de ellos podrían ser el culpable. 

## <a name="understanding-the-alert"></a>Descripción de la alerta
La hoja de detalles de la alerta indica el tiempo cuando la alerta se genera y detectado por última vez. La alerta es generado/volver-evaluated por un proceso en segundo plano que se ejecuta cada dos horas. En el ejemplo siguiente, se produjo la alerta inicial a las 10/03 a las 9:59. Ha ido existe incluso a las 03/12 10:00 A.M. cuando la alerta se evalúen de nuevo.
La hoja también detalla el tiempo de cuándo recibió por última vez un tipo de datos determinado por el servicio de mantenimiento. 
 
 ![Detalles de alerta de Azure AD Connect Health](./media/how-to-connect-health-data-freshness/data-freshness-details.png)
 
A continuación es la asignación de tipos de servicio y tipo de datos necesario correspondiente.

| Tipo de servicio | Agente (nombre del servicio de Windows) | Propósito | Tipo de datos generado  |
| --- | --- | --- | --- |  
| Azure AD Connect (sincronización) | Servicio de análisis de sincronización de Azure AD Connect Health | Recopilar información específica de AAD Connect (los conectores, sincronización reglas etcetera.) | - AadSyncService-SynchronizationRules <br />  - AadSyncService-Connectors <br /> - AadSyncService-GlobalConfigurations  <br />  - AadSyncService-RunProfileResults <br /> - AadSyncService-ServiceConfigurations <br /> - AadSyncService-ServiceStatus   |
|  | Servicio de supervisión de sincronización de Azure AD Connect Health | Recopilar contadores de rendimiento (AAD Connect específico), los seguimientos de ETW, archivos | Contador de rendimiento |
| AD DS | Servicio de análisis de AD DS de Azure AD Connect Health | Realizar pruebas sintéticas, recopilar información de la topología, los metadatos de replicación |  - Adds-TopologyInfo-Json <br /> -Common-TestData-Json (crea los resultados de pruebas)   | 
|  | Servicio de supervisión de AD DS de Azure AD Connect Health | Recopilar archivos de contadores, seguimientos de ETW de rendimiento (específico de ADDS) | : Contador de rendimiento  <br /> -Common-TestData-Json (carga los resultados de pruebas)  |
| AD FS | Servicio de diagnóstico de AD FS de Azure AD Connect Health | Realizar pruebas sintéticas | TestResult (crea los resultados de pruebas) | 
| | Servicio de análisis de AD FS de Azure AD Connect Health  | Recopilar métricas de uso ADFS | Adfs-UsageMetrics |
| | Servicio de supervisión de AD FS de Azure AD Connect Health | Recopilar archivos de contadores, seguimientos de ETW, Perf (específicos de AD FS) | TestResult (carga los resultados de pruebas) |

## <a name="troubleshooting-steps"></a>Pasos para solucionar problemas 

Los pasos necesarios para diagnosticar el problema se indica a continuación. El primero es un conjunto de comprobaciones básicas que son comunes a todos los tipos de servicio. La tabla siguiente que se enumera los pasos específicos para cada tipo de servicio y el tipo de datos. 

> [!IMPORTANT] 
> Esta alerta sigue la [directiva de retención de datos](reference-connect-health-user-privacy.md#data-retention-policy) de Connect Health

* Asegúrese de que se instalan las versiones más recientes de los agentes. Vista [historial de versiones](reference-connect-health-version-history.md). 
* Asegúrese de que los agentes de Azure AD Connect Health servicios son **ejecutando** en el equipo. Por ejemplo, Connect Health para AD FS debe tener tres servicios.
  ![Comprobación de Azure AD Connect Health](./media/how-to-connect-health-agent-install/install5.png)

* Asegúrese de repasar y cumplir la [sección de los requisitos](how-to-connect-health-agent-install.md#requirements).
* Utilice la [herramienta de pruebas de conectividad](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) para detectar problemas de conectividad.
* Si tiene un HTTP Proxy, siga estos [pasos de configuración](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 


## <a name="next-steps"></a>Pasos siguientes
Si cualquiera de los pasos anteriores no identifica un problema, corregirlo y espere resolver la alerta. El proceso de alerta en segundo plano ejecuta cada 2 horas, por lo que tardará hasta 2 horas para resolver la alerta. 

* [Directiva de retención de datos de Azure AD Connect Health](reference-connect-health-user-privacy.md#data-retention-policy)
* [Preguntas más frecuentes de Azure AD Connect Health](reference-connect-health-faq.md)
