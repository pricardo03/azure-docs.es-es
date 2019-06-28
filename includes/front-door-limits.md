---
title: archivo de inclusión
description: archivo de inclusión
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 05/09/2019
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: e1f5a1c8229544d97d9ff64748390f0d5237ab97
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66238315"
---
| Recurso | Límite predeterminado/máximo |
| --- | --- |
| Recursos de Azure Front Door Service por suscripción | 100 |
| Hosts de front-end, que incluye los dominios personalizados por recurso | 100 |
| Reglas de enrutamiento por recurso | 100 |
| Grupos de servidores back-end por recurso | 50 |
| Servidores back-end por grupo de servidores back-end | 100 |
| Patrones de ruta de acceso para coincidir con una regla de enrutamiento | 25 |
| Reglas de firewall de aplicación web personalizadas por directiva | 10 |
| Directiva del firewall de aplicaciones web por recurso | 100 |

### <a name="timeout-values"></a>Valores de tiempo de expiración
#### <a name="client-to-front-door"></a>Cliente para Front Door
- Front Door tiene un tiempo de espera de la conexión TCP de 61 segundos.

#### <a name="front-door-to-application-back-end"></a>Front Door al back-end de la aplicación
- Si la respuesta es una respuesta fragmentada, se devuelve una respuesta 200 si se recibe el primer fragmento, o cuando se recibe.
- Después de que la solicitud HTTP se reenvía al back-end, Front Door espera 30 segundos al primer paquete del back-end. A continuación, se devuelve un error 503 al cliente.
- Una vez recibido el primer paquete del back-end, Front Door espera 30 segundos en un tiempo de expiración de inactividad. A continuación, se devuelve un error 503 al cliente.
- El tiempo de expiración de la sesión TCP desde Front Door hasta el back-end es de 30 minutos.

### <a name="upload-and-download-data-limit"></a>Límite de carga y descarga de datos

|  | Con codificación de transferencia fragmentada (CTE) | Sin fragmentación de HTTP |
| ---- | ------- | ------- |
| **Descargar** | No hay ningún límite para el tamaño de descarga. | No hay ningún límite para el tamaño de descarga. |
| **Cargar** |  No hay ningún límite siempre y cuando cada carga de CTE sea inferior a 2 GB. | El tamaño no puede ser superior a 2 GB. |
