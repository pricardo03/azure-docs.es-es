---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 44f9400a1ecebd3c204ec0b891c5d7fe01bdfd25
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51628207"
---
| Recurso | Límite predeterminado | Nota: |
| --- | --- | --- |
| Application Gateway |1000 por suscripción | |
| Configuraciones de direcciones IP de front-end |2 |1 pública y 1 privada |
| Puertos de front-end |40 | |
| Grupo de direcciones de back-end |40 | |
| Servidores back-end por grupo |1200 | |
| Agentes de escucha HTTP |40 | |
| Reglas de equilibrio de carga HTTP |400 |Número de agentes de escucha HTTP * n |
| Configuración de HTTP de back-end |40 | |
| Instancias por puerta de enlace |75 | |
| Certificados SSL |40 |1 por agentes de escucha HTTP |
| Certificados de autenticación |40 | |
| Tiempo de espera de solicitud mín. |1 segundo | |
| Tiempo de espera de solicitud máx. |24 horas | |
| Número de sitios |20 |1 por agentes de escucha HTTP |
| Asignaciones de URL por agente de escucha |1 | |
| Número máximo de reglas basadas en rutas por mapa de direcciones URL|100|
| Configuraciones de redirección |40| |
| Conexiones simultáneas de WebSocket |5000| |
|Longitud máxima de dirección URL|8000|
| Tamaño máximo de carga de archivos estándar |2 GB | |
| Tamaño máximo de carga de archivos WAF |Puertas de enlace WAF medianas: 100 MB<br>Puertas de enlace WAF grandes: 500 MB| |
|Límite de tamaño de cuerpo de WAF (sin archivos)|128 KB|
