---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/09/2018
ms.author: estfan
ms.openlocfilehash: 3fa71085d649ace95aa24ac87c8714a7268f5386
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186908"
---
Para realizar una estimación más precisa de los costos de consumo, piense en el número de mensajes o eventos posibles que podrían llegar cualquier día, en lugar de basar sus cálculos únicamente en el intervalo de sondeo. Cuando un evento o mensaje satisface los criterios del desencadenador, muchos desencadenadores intentan leer inmediatamente todos y cada uno de los otros eventos o mensajes de espera que satisfacen los criterios. Este comportamiento significa que incluso cuando se selecciona un intervalo de sondeo más largo, el desencadenador se activa en función del número de eventos o mensajes de espera que pueden iniciar los flujos de trabajo. Los desencadenadores que siguen este comportamiento son Azure Service Bus y Azure Event Hubs.

Así que, suponga, por ejemplo, que configura un desencadenador que comprueba un punto de conexión cada día. Cuando el desencadenador comprueba el punto de conexión y busca 15 eventos que satisfacen los criterios, se activa y ejecuta el flujo de trabajo correspondiente 15 veces. Logic Apps mide todas las acciones que realizan esos 15 flujos de trabajo, incluidas las solicitudes del desencadenador. Para calcular los costos potenciales, pruebe la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/).