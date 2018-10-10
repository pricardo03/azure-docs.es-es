---
title: archivo de inclusión
description: archivo de inclusión
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 9/17/2018
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: f0c2d1501b9aa19dec8c4ad157e004a57e0e5070
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006538"
---
| Recurso | Límite predeterminado |
| --- | --- |
| Recursos de Front Door por suscripción | 100 |
| Hosts de front-end que incluyen los dominios personalizados por recurso | 100 |
| Reglas de enrutamiento por recurso | 100 |
| Grupos de back-end por recurso | 50 |
| Back-ends por grupo de back-ends | 100 |
| Patrones de ruta de acceso para coincidir con una regla de enrutamiento | 25 |
| Reglas de firewall de aplicación web personalizadas por directiva | 10 |
| Directiva del firewall de aplicaciones web por recurso | 100 |

### <a name="timeout-values"></a>Valores de tiempo de expiración
#### <a name="client-to-front-door"></a>Cliente para Front Door
- Front Door tiene un tiempo de espera de la conexión TCP de 61 segundos.
#### <a name="front-door-to-application-backend"></a>Front Door al back-end de la aplicación
- Si la respuesta es una respuesta fragmentada, se devolverá una respuesta 200 si se recibe el primer fragmento o cuando se recibe.
- Después de que la petición HTTP se reenvía al back-end, Front Door esperará 30 segundos al primer paquete del back-end, antes de devolver un error 503 al cliente.
- Después de recibir el primer paquete del back-end, Front Door espera 30 segundos (tiempo de espera inactivo) antes de devolver el error 503 al cliente.
- El tiempo de espera de Front Door para la sesión TCP de back-end es de 30 minutos.

### <a name="upload--download-data-limit"></a>Límite de carga y descarga de datos

|  | Con codificación de la transferencia fragmentada | Sin fragmentación de HTTP |
| ---- | ------- | ------- |
| **Descargar** | No hay ningún límite para el tamaño de descarga. | No hay ningún límite para el tamaño de descarga. |
| **Cargar** |  No hay ningún límite siempre y cuando cada carga de CTE sea inferior a 28,6 MB | El tamaño no puede ser superior a 28,6 MB. |
