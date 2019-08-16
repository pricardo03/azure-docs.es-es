---
title: Encabezados HTTP de depuración para Azure CDN de Microsoft | Microsoft Docs
description: Los encabezados de solicitud de caché de depuración proporciona información adicional sobre la directiva de caché aplicada al recurso solicitado. Estos encabezados son específicos para Azure CDN de Microsoft.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: magattus
ms.openlocfilehash: 297c65c1cd89163b8663819f844dc6c2a83fd1bf
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814077"
---
# <a name="debug-http-header-for-azure-cdn-from-microsoft"></a>Encabezados HTTP de depuración para Azure CDN de Microsoft
El encabezado de respuesta de depuración, `X-Cache`, brinda detalles sobre la capa de la pila de CDN desde donde se sirvió el contenido. Este encabezado es específico para Azure CDN de Microsoft.

### <a name="response-header-format"></a>Formato del encabezado de respuesta

Encabezado | DESCRIPCIÓN
-------|------------
X-Cache: TCP_HIT | Este encabezado se devuelve cuando el contenido se sirve desde la caché perimetral de CDN. 
X-Cache: TCP_REMOTE_HIT | Este encabezado se devuelve cuando el contenido se sirve desde la caché regional de CDN (capa de escudo de origen).
X-Cache: TCP_MISS | Este encabezado se devuelve cuando se produce un error de caché y el contenido se sirve desde el origen. 


