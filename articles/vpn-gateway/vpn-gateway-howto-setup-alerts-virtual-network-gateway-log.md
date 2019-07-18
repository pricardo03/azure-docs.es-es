---
title: Configuración de alertas de eventos de registro de diagnóstico de Azure VPN Gateway
description: Pasos para configurar alertas en eventos de registro de diagnóstico de VPN Gateway
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: alzam
ms.openlocfilehash: c84d457c51f71bdf315bbbcec674ff1186dd905f
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249012"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>Configuración de alertas de eventos de registro de diagnóstico de VPN Gateway

En este artículo se le ayuda a configurar alertas basadas en eventos de registro de diagnóstico de Azure VPN Gateway con Azure Log Analytics. 

Los registros siguientes están disponibles en Azure:

|***Nombre*** | ***Descripción*** |
|---        | ---               |
|GatewayDiagnosticLog | Contiene los registros de diagnóstico para los eventos de configuración de puerta de enlace, los cambios principales y los eventos de mantenimiento |
|TunnelDiagnosticLog | Contiene los eventos de cambio del estado del túnel. Los eventos de conexión/desconexión del túnel tienen un motivo resumido para el cambio de estado si es aplicable |
|RouteDiagnosticLog | Registra los cambios en rutas estáticas y eventos BGP que se producen en la puerta de enlace |
|IKEDiagnosticLog | Registra mensajes de control de IKE y eventos en la puerta de enlace |
|P2SDiagnosticLog | Registra mensajes de control de punto a sitio y eventos en la puerta de enlace |

## <a name="setup"></a>Configuración de alertas

Los pasos del ejemplo siguiente crearán una alerta para un evento de desconexión que implica un túnel VPN de sitio a sitio:


1. En Azure Portal, seleccione **Log Analytics** en **Todos los servicios** y seleccione **Áreas de trabajo de Log Analytics**.

   ![Selecciones para ir a las áreas de trabajo de Log Analytics](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "Crear")

2. Seleccione **Crear** en la página **Log Analytics**.

   ![Página de Log Analytics con el botón Crear](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "Seleccionar")

3. Seleccione **Crear nuevo** y rellene los detalles.

   ![Detalles para crear un área de trabajo de Log Analytics](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "Seleccionar")

4. Busque la instancia de VPN Gateway en la hoja **Monitor** > **Configuración de diagnóstico**.

   ![Selecciones para buscar la instancia de VPN Gateway en Configuración de diagnóstico](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "Seleccionar")

5. Para activar los diagnósticos, haga doble clic en la puerta de enlace y, después, seleccione **Activar diagnósticos**.

   ![Selecciones para activar los diagnósticos](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "Seleccionar")

6. Rellene los detalles y asegúrese de que **Enviar a Log Analytics** y **TunnelDiagnosticLog** están seleccionados. Elija el área de trabajo de Log Analytics que creó en el paso 3.

   ![Casillas de verificación seleccionadas](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "Seleccionar")

7. Vaya a la introducción del recurso de la puerta de enlace de red virtual y seleccione **Alertas** en la pestaña **Supervisión**. Después, cree una regla de alertas o modifique una existente.

   ![Selecciones para crear una nueva regla de alertas](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Seleccionar")

   ![punto a sitio](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Seleccionar")
8. Seleccione el área de trabajo de Log Analytics y el recurso.

   ![Selecciones para el área de trabajo y el recurso](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "Seleccionar")

9. Seleccione **Búsqueda de registros personalizada** como la lógica de la señal en **Agregar condición**.

   ![Selecciones para una búsqueda de registros personalizada](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "Seleccionar")

10. Escriba la consulta siguiente en el cuadro de texto **Consulta de búsqueda**. Reemplace los valores de <> según corresponda.

    ```
    AzureDiagnostics |
      where Category  == "TunnelDiagnosticLog" and ResourceId == toupper("<RESOURCEID OF GATEWAY>") and TimeGenerated > ago(5m) and
      remoteIP_s == "<REMOTE IP OF TUNNEL>" and status_s == "Disconnected"
    ```

    Establezca el valor del umbral en 0 y seleccione **Listo**.

    ![Escriba una consulta y seleccione un umbral](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "Seleccionar")

11. En la página **Crear regla**, seleccione **Crear nueva** en la sección **GRUPOS DE ACCIONES**. Rellene los detalles y seleccione **Aceptar**.

    ![Detalles para un nuevo grupo de acciones](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "Seleccionar")

12. En la página **Crear regla**, rellene los detalles de **Personalizar acciones** y asegúrese de que aparece el nombre correcto en la sección **NOMBRE DEL GRUPO DE ACCIONES**. Seleccione **Crear regla de alerta** para crear la regla.

    ![Selecciones para la creación de una regla](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "Seleccionar")

## <a name="next-steps"></a>Pasos siguientes

Para configurar alertas en las métricas de túnel, vea [Configuración de alertas en métricas de VPN Gateway](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
