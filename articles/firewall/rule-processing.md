---
title: Lógica de procesamiento de reglas de Azure Firewall
description: Información acerca de la lógica de procesamiento de reglas de Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/27/2018
ms.author: victorh
ms.openlocfilehash: 12d86793c0d75413559aad77c558c4adb7ac91af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60193652"
---
# <a name="azure-firewall-rule-processing-logic"></a>Lógica de procesamiento de reglas de Azure Firewall
Azure Firewall tiene reglas NAT, reglas de red y reglas de aplicaciones. Las reglas se procesan en función del tipo de regla.


## <a name="network-rules-and-applications-rules"></a>Reglas de red y reglas de aplicaciones 
En primer lugar se aplican las reglas de red y después las reglas de aplicaciones. Las reglas están terminando. Por tanto, si se encuentra alguna coincidencia en las reglas de red, las reglas de aplicación no se procesan.  Si no coincide ninguna regla de red, y si el protocolo del paquete es HTTP/HTTPS, las reglas de aplicación lo evalúan posteriormente. Si sigue sin haber coincidencias, el paquete se evalúa con la colección de reglas de infraestructura. Si sigue sin haber coincidencias, el paquete se deniega de forma predeterminada.

## <a name="nat-rules"></a>Reglas NAT
Conectividad entrante puede habilitarse mediante la configuración de traducción de direcciones de red de destino (DNAT) como se describe en [Tutorial: Filtrar el tráfico entrante con DNAT de Firewall de Azure mediante Azure portal](tutorial-firewall-dnat.md). Las reglas de DNAT son las primeras que se aplican. Si se encuentra alguna coincidencia, se agrega una regla de red correspondiente implícita para permitir el tráfico traducido. Para invalidar este comportamiento, agregue explícitamente una colección de reglas de red con reglas de denegación que coinciden con el tráfico traducido. No se aplican reglas de aplicación a estas conexiones.


## <a name="next-steps"></a>Pasos siguientes

- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md).