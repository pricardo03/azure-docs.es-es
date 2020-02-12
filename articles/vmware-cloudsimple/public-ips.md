---
title: 'Azure VMware Solutions (AVS): asignación de direcciones IP públicas'
description: Describe cómo asignar direcciones IP públicas para las máquinas virtuales en el entorno de nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 87133f5efb9f096d3fdb0956aab1caac58b4bd94
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024303"
---
# <a name="allocate-public-ip-addresses-for-avs-private-cloud-environment"></a>Asignación de direcciones IP públicas para el entorno de nube privada de AVS

Abra la pestaña IP públicas en la página Red para asignar direcciones IP públicas para las máquinas virtuales del entorno de nube privada de AVS.

1. [Acceda al portal de AVS](access-cloudsimple-portal.md) y seleccione **Red** en el menú lateral.
2. Seleccione **Public IPs** (Direcciones IP públicas).
3. Haga clic en **New Public IP** (Nueva IP pública).

    ![Página de direcciones IP públicas](media/public-ips-page.png)

4. Escriba un nombre para identificar la entrada de la dirección IP.
5. Mantenga la ubicación predeterminada.
6. Use el control deslizante para cambiar el tiempo de espera de inactividad si lo desea.
7. Escriba la dirección IP local a la que quiere asignar una dirección IP pública.
8. Escriba un nombre DNS asociado.
9. Haga clic en **Enviar**.

![Asignación de IP públicas](media/network-public-ip-allocate.png)

Comienza la tarea de asignar la dirección IP pública. Puede comprobar el estado de la tarea en la página **Activity > Tasks** (Actividad > Tareas). Cuando se complete la asignación, se muestra la nueva entrada en la página de direcciones IP públicas.
