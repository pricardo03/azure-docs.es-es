---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 592e1973344b231693077f8286a41dfd67a8d188
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66689099"
---
| Recurso | Límite predeterminado y máximo | Nota: |
| --- | --- | --- |
| Azure Application Gateway |1.000 por suscripción | |
| Configuraciones de IP front-end |2 |1 pública y 1 privada |
| Puertos de front-end |100<sup>1</sup> | |
| Grupos de direcciones de back-end |100<sup>1</sup> | |
| Servidores back-end por grupo |1,200 | |
| Agentes de escucha HTTP |100<sup>1</sup> | |
| Reglas de equilibrio de carga HTTP |100<sup>1</sup> | |
| Configuración de HTTP de back-end |100<sup>1</sup> | |
| Instancias por puerta de enlace |32 | |
| Certificados SSL |100<sup>1</sup> |1 por agentes de escucha HTTP |
| Tamaño máximo de certificados SSL |V1 SKU - 10 KB<br>V2 SKU - 25 KB| |
| Certificados de autenticación |100 | |
| Certificados raíz de confianza |100 | |
| Tiempo de espera mínimo |1 segundo | |
| Tiempo de espera de solicitud máximo |24 horas | |
| Número de sitios |100<sup>1</sup> |1 por agentes de escucha HTTP |
| Asignaciones de URL por agente de escucha |1 | |
| Número máximo de reglas basadas en rutas por mapa de direcciones URL|100||
| Configuraciones de redirección |100<sup>1</sup>| |
| Conexiones simultáneas de WebSocket |Puertas de enlace medianas 20k<br> Puertas de enlace grandes 50k| |
| Longitud máxima de dirección URL|8.000||
| Tamaño de carga máximo del archivo, estándar |2 GB | |
| Tamaño máximo de carga de archivos WAF |Puertas de enlace de WAF medianos, 100 MB<br>Puertas de enlace grandes WAF, 500 MB| |
| Límite de tamaño de cuerpo de WAF, sin los archivos|128 KB||
|Reglas personalizadas de WAF máximas|100||

<sup>1</sup> en el caso de las SKU de WAF habilitado, se recomienda que limite el número de recursos a 40 para un rendimiento óptimo.
