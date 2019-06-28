---
title: Conectar datos de inteligencia sobre amenazas a Azure Sentinel, versión preliminar | Microsoft Docs
description: Obtenga información sobre cómo conectar datos de inteligencia sobre amenazas a Azure Sentinel.
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 56412543-5664-44c1-b026-2dbaf78a9a50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 266e487a7c345f75e966afbde567c5bc4683b5c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233748"
---
# <a name="connect-data-from-threat-intelligence-providers"></a>Conectar datos de proveedores de inteligencia sobre amenazas 

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Después de transmitir los datos a Azure Sentinel, pueden enriquecerse con la fuente de inteligencia sobre amenazas que se use en toda la organización. 

Azure Sentinel permite la integración con [proveedores de inteligencia sobre amenazas](https://aka.ms/graphsecuritytips) con el propósito de que se puedan comprobar las reglas y alertas con una inteligencia sobre amenazas genuina, así, por ejemplo, si se recibe una alerta de una dirección IP específica, la integración del proveedor de inteligencia sobre amenazas podrá informarle de si esa dirección IP se ha detectado recientemente como malintencionada. 

Se pueden transmitir registros desde los proveedores de inteligencia sobre amenazas a Azure Sentinel con tan solo un clic. Esta conexión le permite incorporar indicadores que contienen varios tipos de objetos observables (como direcciones IP, dominios, direcciones URL y hash de archivos) para buscar y crear reglas de alertas personalizadas en Azure Sentinel.  
> [!NOTE]
> Para especificar indicadores de amenaza personalizados en Azure Sentinel para usarlos en reglas de alertas, paneles y escenarios de búsqueda, se puede integrar la entidad [tiIndicator de Microsoft Graph Security](https://aka.ms/graphsecuritytiindicators) o usar una [plataforma de inteligencia sobre amenazas integrada de Microsoft Graph Security](https://aka.ms/graphsecuritytips).

## <a name="prerequisites"></a>Requisitos previos  

- Un usuario que sea administrador global o que tenga permisos de administrador de seguridad 

- Aplicación de inteligencia sobre amenazas integrada con Microsoft Intelligent Security Graph 

## <a name="connect-to-threat-intelligence"></a>Conectar con la inteligencia sobre amenazas 

1. Si ya usa un proveedor de inteligencia sobre amenazas, asegúrese de examinar esa aplicación y concederle permiso para enviar indicadores a Microsoft, así como especificar el servicio Azure Sentinel.  

2. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **inteligencia sobre amenazas**.

3. Haga clic en **Conectar**. 

4. Para usar el esquema correspondiente en Log Analytics para encontrar fuentes de inteligencia sobre amenazas, busque **ThreatIntelligenceIndicator**. 

 
## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar su proveedor de inteligencia sobre amenazas a Azure Sentinel. Para obtener más información sobre Azure Sentinel, consulte los siguientes artículos.

- Para empezar a trabajar con Azure Sentinel, necesita una suscripción a Microsoft Azure. Si no tiene una suscripción, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/).
- Aprenda a [incorporar los datos en Azure Sentinel](quickstart-onboard.md), [obtenga visibilidad sobre ellos y aprenda a defenderse de posibles amenazas](quickstart-get-visibility.md).
