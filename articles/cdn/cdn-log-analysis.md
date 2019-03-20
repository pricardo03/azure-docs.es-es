---
title: Análisis de patrones de uso de Azure CDN | Microsoft Docs
description: En este artículo se describen los diferentes tipos de informes de análisis disponibles para los productos de Azure CDN.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: magattus
ms.openlocfilehash: ef713c954d6eab05259547a277db12a1e9036bcf
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650553"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Análisis de patrones de uso de Azure CDN

Después de habilitar CDN para la aplicación, puede supervisar el uso de la red CDN, comprobar el mantenimiento de su entrega y solucionar posibles problemas. Azure CDN proporciona estas funcionalidades de las siguientes maneras: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Análisis básicos a través de los registros de diagnóstico de Azure

El análisis esencial está disponible para los puntos de conexión de CDN de todos los planes de tarifa. Los registros de diagnósticos de Azure permiten análisis básico se exporten a Azure storage, event hubs o registros de Azure Monitor. Registros de Azure Monitor ofrece una solución con gráficos que son configurables por el usuario y personalizables. Para más información sobre los registros de diagnóstico de Azure, consulte [Registros de diagnóstico de Azure](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Informes principales de Verizon

Como usuario de Azure CDN con un **perfil estándar de Azure CDN de Verizon** o un **perfil premium de Azure CDN de Verizon**, puede ver los informes principales de Verizon en el portal complementario de Verizon. Se puede acceder a los informes principales de Verizon con la opción **Administrar** de Azure Portal y ofrecen una variedad de gráficos y vistas. Para más información, vea [Informes principales de Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Informes personalizados de Verizon

Como usuario de Azure CDN con un **perfil estándar de Azure CDN de Verizon** o un **perfil premium de Azure CDN de Verizon**, puede ver los informes personalizados de Verizon en el portal complementario de Verizon. Se puede acceder a los informes personalizados de Verizon con la opción **Administrar** de Azure Portal. La página de informes personalizados de Verizon muestra el número de aciertos o datos transferidos para cada CName perimetral que pertenece a un perfil de Azure CDN. Los datos se pueden agrupar por código de respuesta HTTP o estado de caché durante cualquier período de tiempo. Para más información, consulte [Informes personalizados de Verizon](cdn-verizon-custom-reports.md).

## <a name="azure-cdn-premium-from-verizon-reports"></a>Informes de Azure CDN Premium de Verizon

Con **Azure CDN premium de Verizon**, también puede tener acceso a los informes siguientes:
   * [Informes de HTTP avanzados](cdn-advanced-http-reports.md)
   * [Estadísticas en tiempo real](cdn-real-time-stats.md)
   * [Rendimiento del nodo perimetral](cdn-edge-performance.md)

