---
title: Azure VMware Solution by CloudSimple - Cuota de nodos de CloudSimple
description: Se describen los límites de cuota de los nodos de CloudSimple y cómo solicitar un aumento de la cuota
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e765d7c87f2f941a5e2d558b71c4e5a71d2df9b1
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816677"
---
# <a name="cloudsimple-node-quota-limits"></a>Límites de cuota de los nodos de CloudSimple

Cuando la suscripción está habilitada para el servicio CloudSimple, la cantidad predeterminada de nodos disponibles para su aprovisionamiento es cuatro.  Puede aprovisionar cualquier [tipo de nodo](cloudsimple-node.md) desde Azure Portal.  Para crear una nube privada, se requieren un mínimo de tres nodos de la misma SKU.  Si ha aprovisionado los nodos, es posible que vea un error cuando intente aprovisionar nodos adicionales.

## <a name="quota-increase"></a>Aumento de la cuota

Puede aumentar la cuota de nodos enviando una solicitud de soporte técnico. El equipo de operaciones de servicio evaluará la solicitud y le ayudará a aumentar la cuota de nodos.  Al abrir una incidencia nueva, seleccione las opciones siguientes:

* Tipo de problema: **Técnico**
* Suscripción: **Id. de suscripción**
* Tipo de servicio: **VMware Solution by CloudSimple**
* Tipo de problema: **Cuota de nodos dedicados**
* Subtipo de problema: **Aumentar la cuota de nodos dedicados**
* Asunto: **Aumento de la cuota**

En los detalles de la incidencia de soporte técnico, proporcione el número de nodos necesarios y la SKU de los nodos.

También puede ponerse en contacto con su representante de la cuenta Microsoft en [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com) para aumentar la cuota de nodos de su suscripción.  Tendrá que proporcionar la siguiente información:

* Id. de suscripción
* SKU de los nodos
* Número de nodos adicionales para los que está solicitando el aumento de cuota

## <a name="next-steps"></a>Pasos siguientes

* [Aprovisionamiento de nodos](create-nodes.md)
* [Introducción a los nodos de CloudSimple](cloudsimple-node.md)