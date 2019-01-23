---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 66dea07a1ff725c6707b19bc6ebdc5563f1b158b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54211910"
---
| Recurso | Límite predeterminado | Nota: |
| --- | --- | --- |
| Application Gateway |1000 por suscripción | |
| Configuración de direcciones IP de front-end |2 |1 pública y 1 privada |
| Puertos de front-end |100<sup>1</sup> | |
| Grupos de direcciones de back-end |100<sup>1</sup> | |
| Servidores back-end por grupo |1200 | |
| Agentes de escucha HTTP |100<sup>1</sup> | |
| Reglas de equilibrio de carga HTTP |100<sup>1</sup> | |
| Configuración de HTTP de back-end |100<sup>1</sup> | |
| Instancias por puerta de enlace |32 | |
| Certificados SSL |100<sup>1</sup> |1 por agentes de escucha HTTP |
| Certificados de autenticación |100 | |
| Certificados raíz de confianza |100 | |
| Tiempo de espera de solicitud mín. |1 segundo | |
| Tiempo de espera de solicitud máx. |24 horas | |
| Número de sitios |100<sup>1</sup> |1 por agentes de escucha HTTP |
| Asignaciones de URL por agente de escucha |1 | |
| Número máximo de reglas basadas en rutas por mapa de direcciones URL|100||
| Configuraciones de redirección |100<sup>1</sup>| |
| Conexiones simultáneas de WebSocket |5000| |
| Longitud máxima de dirección URL|8000||
| Tamaño máximo de carga de archivos estándar |2 GB | |
| Tamaño máximo de carga de archivos WAF |Puertas de enlace WAF medianas: 100 MB<br>Puertas de enlace WAF grandes: 500 MB| |
| Límite de tamaño de cuerpo de WAF (sin archivos)|128 KB||

<sup>1</sup> En el caso de las SKU habilitadas para WAF, se recomienda que limite el número de recursos a 40 para un rendimiento óptimo.
