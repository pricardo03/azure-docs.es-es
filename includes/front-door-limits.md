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
ms.openlocfilehash: deca0034996f6c8ddcac71cd4f191c1a0659b655
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "67333392"
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
| Condiciones de coincidencia del firewall de aplicaciones web por regla personalizada | 10 |
| Intervalos de direcciones IP del firewall de aplicaciones web por condición de coincidencia | 600 |
| Valores de coincidencia de la cadena del firewall de aplicaciones web por condición de coincidencia | 10 |
| Longitud del valor de coincidencia de la cadena del firewall de aplicaciones web | 256 |
| Longitud del nombre del parámetro del cuerpo POST del firewall de aplicaciones web | 256 |
| Longitud del nombre del encabezado HTTP del firewall de aplicaciones web | 256 |
| Longitud del nombre de la cookie del firewall de aplicaciones web | 256 |
| Tamaño del cuerpo de la solicitud HTTP del firewall de aplicaciones web inspeccionado | 128 KB |
| Longitud del cuerpo de respuesta personalizada del firewall de aplicaciones web | 2 KB |

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

### <a name="other-limits"></a>Otros límites
- Tamaño máximo de la dirección URL - 8192 bytes - Especifica la longitud máxima de la dirección URL sin procesar (esquema + nombre de host + puerto + ruta de acceso + cadena de consulta de la dirección URL) - Tamaño máximo de la cadena de consulta - 4096 bytes - Especifica la longitud máxima de la cadena de consulta, en bytes.