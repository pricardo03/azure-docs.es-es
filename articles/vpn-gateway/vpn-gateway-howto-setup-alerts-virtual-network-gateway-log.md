---
title: Configurar alertas de eventos de registro de diagnóstico de Azure VPN Gateway
description: Pasos para configurar alertas en eventos de registro de diagnóstico de puerta de enlace de VPN
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: 3880c847c54136dfd3ba1ecfe0178565091e229f
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510214"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>Configurar alertas de eventos de registro de diagnóstico de puerta de enlace de VPN

En este artículo le ayuda a configurar alertas basadas en eventos de registro de diagnóstico de Azure VPN Gateway.

## <a name="setup"></a>Configuración de alertas

Los pasos del ejemplo siguiente creará una alerta para un evento de desconexión que implica un túnel VPN de sitio a sitio:


1. En Azure portal, busque **Log Analytics** en **todos los servicios** y seleccione **las áreas de trabajo de Log Analytics**.

   ![Selecciones para ir a las áreas de trabajo de Log Analytics](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "Create")

2. Seleccione **crear** en el **Log Analytics** página.

   ![Página de análisis de registros con el botón crear](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "seleccione")

3. Seleccione **crear nuevo** y rellene los detalles.

   ![Detalles de la creación de un área de trabajo de Log Analytics](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "seleccione")

4. Busque la puerta de enlace VPN en el **Monitor** > **configuración de diagnóstico** hoja.

   ![Selecciones para encontrar la puerta de enlace VPN en la configuración de diagnóstico](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "seleccione")

5. Para activar los diagnósticos, haga doble clic en la puerta de enlace y, a continuación, seleccione **Activar diagnósticos**.

   ![Selecciones para activar los diagnósticos](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "seleccione")

6. Rellene los detalles y asegúrese de que **enviar a Log Analytics** y **TunnelDiagnosticLog** están seleccionadas. Elija el área de trabajo de Log Analytics que creó en el paso 3.

   ![Selecciona las casillas de verificación](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "seleccione")

7. Vaya a la información general para el recurso de puerta de enlace de red virtual y seleccione **alertas** desde el **supervisión** ficha. A continuación, cree una nueva regla de alerta o editar una regla de alerta existente.

   ![Selecciones para crear una nueva regla de alerta](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "seleccione")

   ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Select")
8. Seleccione el área de trabajo de Log Analytics y el recurso.

   ![Selecciones para el área de trabajo y recursos](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "seleccione")

9. Seleccione **búsqueda de registros personalizada** como la lógica de señal en **Agregar condición**.

   ![Selecciones para una búsqueda de registros personalizado](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "seleccione")

10. Escriba la consulta siguiente en el cuadro de texto **Consulta de búsqueda**. Reemplace los valores de <> según corresponda.

     `AzureDiagnostics |
     where Category  == "TunnelDiagnosticLog" and ResourceId == toupper("<RESOURCEID OF GATEWAY>") and TimeGenerated > ago(5m) and
     remoteIP_s == "<REMOTE IP OF TUNNEL>" and status_s == "Disconnected"`

    Establezca el valor de umbral en 0 y seleccione **realiza**.

    ![Escriba una consulta y seleccione un umbral](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "seleccione")

11. En el **crear regla** página, seleccione **crear nuevo** bajo el **grupos de acciones** sección. Rellene los detalles y seleccione **Aceptar**.

    ![Los detalles de un nuevo grupo de acciones](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "seleccione")

12. En el **crear regla** página, rellene los detalles de **personalizar acciones** y asegúrese de que el nombre correcto aparece en el **nombre del grupo de acción** sección. Seleccione **crear regla de alertas** para crear la regla.

    ![Selecciones para crear una regla](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "seleccione")

## <a name="next-steps"></a>Pasos siguientes

Para configurar alertas en métricas de túnel, consulte [configurar alertas de métricas de puerta de enlace VPN](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
