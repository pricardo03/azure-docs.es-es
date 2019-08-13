---
title: ¿Deben conectarse orígenes de datos a la versión preliminar de Azure Sentinel?| Microsoft Docs
description: Aprenda a conectar orígenes de datos a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a3b63cfa-b5fe-4aff-b105-b22b424c418a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2019
ms.author: rkarlin
ms.openlocfilehash: 4928657aa9052b50faf1f326cc09797c5aaf69bb
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780514"
---
# <a name="connect-data-sources"></a>Conexión con orígenes de datos

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Para incorporar Azure Sentinel, primero debe conectarse a sus orígenes de datos. Azure Sentinel llega con varios conectores para soluciones de Microsoft que están disponibles inmediatamente y proporcionan integración en tiempo real; por ejemplo, para soluciones de Microsoft Threat Protection y orígenes de Microsoft 365, como Office 365, Azure AD, Azure ATP y Microsoft Cloud App Security, entre muchos otros. Además, hay conectores integrados al amplio ecosistema de seguridad para soluciones que no son de Microsoft. También puede usar el formato de evento común, Syslog o las API de REST para conectar los orígenes de datos con Azure Sentinel.  

1. En el menú, seleccione **Data connectors** (Conectores de datos). Esta página permite ver la lista completa de los conectores que Azure Sentinel proporciona, y su estado. Seleccione el conector al que desea conectarse y seleccione **Open connector page** (Abrir la página del conector). 

   ![Recopiladores de datos](./media/collect-data/collect-data-page.png)

1. En la página específica del conector, asegúrese de que cumple todos los requisitos previos y siga las instrucciones para conectar los datos a Azure Sentinel. Los registros pueden tardar algún tiempo en iniciar la sincronización con Azure Sentinel. Después de conectarse, verá un resumen de los datos en el gráfico **Datos recibidos**, y el estado de conectividad de los tipos de datos.

   ![Recopiladores de conexión](./media/collect-data/opened-connector-page.png)
  
1. Haga clic en la pestaña **Siguientes pasos** para obtener una lista del contenido que Azure Sentinel proporciona para el tipo de datos específico.

   ![Recopiladores de datos](./media/collect-data/data-insights.png)
 

## <a name="data-connection-methods"></a>Métodos de conexión de datos

Los siguientes métodos de conexión de datos son compatibles con Azure Sentinel:

- **Servicios Microsoft**:<br> Los servicios Microsoft se conectan de forma nativa, aprovechando la base de Azure de la integración lista para usar. Con solo unos clics, se pueden conectar las soluciones siguientes:
    - [Office 365](connect-office-365.md)
    - [Azure AD audit logs and sign-ins](connect-azure-active-directory.md) (Inicios de sesión y pistas de auditoría de Azure AD)
    - [Azure Activity](connect-azure-activity.md) (Actividad de Azure)
    - [Azure AD Identity Protection](connect-azure-ad-Identity-protection.md)
    - [Azure Security Center](connect-azure-security-center.md)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Azure Advanced Threat Protection](connect-azure-atp.md)
    - [Cloud App Security](connect-cloud-app-security.md)
    - [Eventos de seguridad de Windows](connect-windows-security-events.md) 
    - [Firewall de Windows](connect-windows-firewall.md)

- **Soluciones externas mediante API**: algunos orígenes de datos se conectan mediante las API proporcionadas por el origen de datos conectado. Normalmente, la mayoría de las tecnologías de seguridad proporcionan un conjunto de API a través del cual se pueden recuperar registros de eventos. Las API se conectan a Azure Sentinel y recopilan y envían tipos de datos específicos a Azure Log Analytics. Entre los dispositivos conectados mediante API se incluyen:
    - [Barracuda](connect-barracuda.md)
    - [Symantec](connect-symantec.md)
- **Soluciones externas mediante agente**: Azure Sentinel se puede conectar a todos los demás orígenes de datos que pueden realizar secuencias de registro en tiempo real mediante el protocolo de Syslog, a través de un agente. <br>La mayoría de las aplicaciones usan el protocolo de Syslog para enviar mensajes de eventos que incluyen el propio registro y datos sobre este. El formato de los registros varía, pero la mayoría de los dispositivos admiten el estándar de Common Event Format (CEF). <br>El agente de Azure Sentinel, que se basa en Microsoft Monitoring Agent, convierte los registros con formato CEF en un formato que Log Analytics puede ingerir. Dependiendo del tipo de dispositivo, el agente se instala directamente en el dispositivo o en un servidor Linux dedicado. El agente para Linux recibe eventos del demonio de Syslog a través de UDP; sin embargo,si se espera que una máquina Linux recopile un gran volumen de eventos Syslog, se envían a través de TCP desde el demonio de Syslog al agente y desde allí a Log Analytics.
    - Firewalls, proxies y puntos de conexión:
        - [F5](connect-f5.md)
        - [Check Point](connect-checkpoint.md)
        - [Cisco ASA](connect-cisco.md)
        - [Fortinet](connect-fortinet.md)
        - [Palo Alto](connect-paloalto.md)
        - [Other CEF appliances](connect-common-event-format.md) (Otros dispositivos CEF)
        - [Other Syslog appliances](connect-syslog.md) (Otros dispositivos de Syslog)
    - Soluciones de DLP
    - [Threat intelligence providers](connect-threat-intelligence.md) (Proveedores de información sobre amenazas)
    - [DNS machines](connect-dns.md) (Máquinas DNS): agente instalado directamente en la máquina DNS
    - Servidores Linux
    - Otras nubes
    
## Opciones de conexión del agente<a name="agent-options"></a>

Para conectar su dispositivo externo a Azure Sentinel, el agente debe implementarse en una máquina dedicada (máquina virtual o local) para admitir la comunicación entre el dispositivo y Azure Sentinel. Puede implementar el agente automáticamente o de forma manual. La implementación automática solo está disponible si su máquina dedicada es una nueva máquina virtual que crea en Azure. 


![CEF en Azure](./media/connect-cef/cef-syslog-azure.png)

También puede implementar el agente manualmente en una máquina virtual existente, en una máquina virtual en otra nube o en una máquina local.

![CEF local](./media/connect-cef/cef-syslog-onprem.png)


## <a name="next-steps"></a>Pasos siguientes

- Para empezar a trabajar con Azure Sentinel, necesita una suscripción a Microsoft Azure. Si no tiene una suscripción, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/).
- Aprenda a [incorporar los datos en Azure Sentinel](quickstart-onboard.md), [obtenga visibilidad sobre ellos y aprenda a defenderse de posibles amenazas](quickstart-get-visibility.md).
