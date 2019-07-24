---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 9a8d925e5d72500817028b913a80255a4339255c
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148463"
---
| Recurso | Límite predeterminado/máximo | Nota: |
| --- | --- | --- |
| Azure Application Gateway |1000 por suscripción | |
| Configuración de direcciones IP de front-end |2 |1 pública y 1 privada |
| Puertos de front-end |100<sup>1</sup> | |
| Grupos de direcciones de back-end |100<sup>1</sup> | |
| Servidores back-end por grupo |1,200 | |
| Agentes de escucha HTTP |100<sup>1</sup> | |
| Reglas de equilibrio de carga HTTP |100<sup>1</sup> | |
| Configuración de HTTP de back-end |100<sup>1</sup> | |
| Instancias por puerta de enlace |32 | |
| Certificados SSL |100<sup>1</sup> |1 por agentes de escucha HTTP |
| Tamaño máximo de certificados SSL |V1 SKU: 10 KB<br>V2 SKU: 16 KB| |
| Certificados de autenticación |100 | |
| Certificados raíz de confianza |100 | |
| Tiempo de espera de solicitud mínimo |1 segundo | |
| Tiempo de espera de solicitud máximo |24 horas | |
| Número de sitios |100<sup>1</sup> |1 por agentes de escucha HTTP |
| Asignaciones de URL por agente de escucha |1 | |
| Número máximo de reglas basadas en rutas por mapa de direcciones URL|100||
| Configuraciones de redirección |100<sup>1</sup>| |
| Conexiones simultáneas de WebSocket |Puertas de enlace medianas 20k<br> Puertas de enlace grandes 50k| |
| Longitud máxima de dirección URL|8\.000||
| Tamaño máximo de carga de archivos (estándar) |2 GB | |
| Tamaño máximo de carga de archivos WAF |Puertas de enlace WAF medianas (100 MB)<br>Puertas de enlace WAF grandes (500 MB)| |
| Límite de tamaño de cuerpo de WAF (sin archivos)|128 KB||
|Máximo de reglas personalizadas de WAF|100||

<sup>1</sup> En el caso de las SKU habilitadas para WAF, se recomienda que limite el número de recursos a 40 para un rendimiento óptimo.
