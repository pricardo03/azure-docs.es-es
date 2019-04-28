---
title: Cómo configurar alertas sobre eventos de registro de diagnóstico de Azure VPN Gateway
description: Pasos para configurar alertas en eventos de registro de diagnóstico de puerta de enlace de VPN
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: 39a6d2e6201dd0635149159cb3727fd3c40f710a
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769742"
---
# <a name="setting-up-alerts-on-vpn-gateway-diagnostic-log-events"></a>Configurar alertas sobre eventos de registro de diagnóstico de puerta de enlace de VPN

En este artículo le ayuda a configurar alertas basadas en eventos de registro de diagnóstico de puerta de enlace de VPN.


## <a name="setup"></a>Configurar alertas de Azure Monitor basadas en eventos de registro de diagnóstico mediante el portal

Los pasos de ejemplo siguientes creará una alerta para un evento de desconexión de túnel VPN de sitio a sitio



1. Busque "Log Analytics" en todos los servicios y elija "Áreas de trabajo de Log Analytics" ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "Create")

2. En la página de Log Analytics, haga clic en "Crear".
![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "Select")

3. Compruebe el área de trabajo "Crear nuevo" y rellene los detalles.
![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "Select")

4. Busque la puerta de enlace VPN en el "Monitor" > hoja "Configuración de diagnóstico" ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "seleccione")

5. Para activar los diagnósticos, haga doble clic en la puerta de enlace y, a continuación, haga clic en "Activar diagnósticos" ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "seleccione")

6. Rellene los detalles y asegúrese de que están activada la opción "Enviar a Log Analytics" y "TunnelDiagnosticLog". Elija el área de trabajo de Log Analytics que se creó en el paso 3.
![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "Select")

7. Vaya a la información general sobre recursos de red virtual de puerta de enlace y seleccione "Alertas" en la ficha Supervisión, a continuación, cree una nueva regla de alerta o editar una regla de alerta existente.
![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Select")

8. Seleccione el área de trabajo de Log Analytics y el recurso.
![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "Select")

9. Seleccione "búsqueda de registros personalizada" como la lógica de señal en "Agregar condición" ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "seleccione")

10. Escriba la siguiente consulta en el cuadro de texto "Consulta de búsqueda" reemplazando los valores de <> según corresponda.

    AzureDiagnostics | donde categoría == "TunnelDiagnosticLog" y ResourceId == toupper ("<RESOURCEID OF GATEWAY>") y TimeGenerated > ago(5m) y remoteIP_s == "<REMOTE IP OF TUNNEL>" y status_s == "Desconectado"

    Establezca el valor de umbral en 0 y haga clic en "Listo"

    ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "Select")

11. En la página "Crear regla", haga clic en "Crear nuevo" en la sección grupos de acciones. Rellene los detalles y haga clic en Aceptar

![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "Select")

12. En la página "Crear regla", rellene los detalles para "Personalizar la acción" y asegúrese de que el nombre del grupo de acción correcto aparece en la sección "Nombre del grupo de acciones". Haga clic en "Crear regla de alerta" para crear la regla.
![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "Select")

## <a name="next-steps"></a>Pasos siguientes

Para configurar alertas en métricas de túnel, consulte [cómo configurar alertas en métricas de puerta de enlace VPN](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
