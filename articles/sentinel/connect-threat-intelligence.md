---
title: Recopilar datos de inteligencia de amenazas en versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de inteligencia de amenazas a Centinela de Azure.
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 56412543-5664-44c1-b026-2dbaf78a9a50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 2c5a7dc08886e21ef8e287540d9139ec555b11a2
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242299"
---
# <a name="collect-data-from-threat-intelligence-providers"></a>Recopilar datos de proveedores de inteligencia de amenazas 

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Después de transmitir los datos en Azure Sentinel, se puede enriquecer, con la fuente de inteligencia de amenazas que usan en toda la organización. 

Para permitirle cross Compruebe las reglas y alertas con inteligencia de amenazas es true, por ejemplo, si recibe una alerta de una dirección IP específica, la integración del proveedor de inteligencia de amenazas podrán informarle si recientemente encontró esa dirección IP malintencionada , Permite la integración con azure centinela [proveedores de inteligencia de amenazas](https://aka.ms/graphsecuritytips). 

Puede transmitir los registros de proveedores de inteligencia de amenazas en Azure Sentinel con un solo clic. Esta conexión le permite incorporar los indicadores que contiene varios tipos de objetos observables como dirección IP, dominio, dirección URL y hash de archivo para buscar y crear personalizado le alerta de reglas en Azure Sentinel.  

## <a name="prerequisites"></a>Requisitos previos  

- Usuario con permisos de administrador de seguridad o de administrador global 

- Aplicación de inteligencia de amenazas integrada con Microsoft Intelligent Security Graph 

## <a name="connect-to-threat-intelligence"></a>Conectarse a la inteligencia sobre amenazas 

1. Si ya usa un proveedor de inteligencia de amenazas, asegúrese de examinar la aplicación de la sugerencia y conceder permiso para enviar los indicadores a Microsoft y especifique el servicio como Centinela de Azure.  

2. En Azure Sentinel, seleccione **la recopilación de datos** y, a continuación, haga clic en el **inteligencia sobre amenazas** icono.

3. Haga clic en **Conectar**. 

4. Para usar el esquema correspondiente en Log Analytics para las fuentes de inteligencia de amenazas, busque **ThreatIntelligenceIndicator**. 

 
## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido cómo conectar el proveedor de inteligencia de amenazas a Centinela de Azure. Para obtener más información acerca de Centinela de Azure, consulte los siguientes artículos.

- Para empezar a trabajar con Azure Sentinel, necesita una suscripción a Microsoft Azure. Si no tiene una suscripción, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/).
- Obtenga información sobre cómo [incorporar los datos a Azure Sentinel](quickstart-onboard.md), y [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
