---
title: Solución de problemas de conectividad de puntos de conexión privados de Azure
description: Instrucciones paso a paso para diagnosticar la conectividad de puntos de conexión privados
services: private-endpoint
documentationcenter: na
author: rdhillon
manager: narayan
editor: ''
ms.service: private-link
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: rdhillon
ms.openlocfilehash: df4ec6ddbba029eb29d2440717697968f8c79302
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77191059"
---
# <a name="troubleshoot-private-endpoint-connectivity-problems"></a>Solución de problemas de conectividad de puntos de conexión privados

En esta guía se proporcionan instrucciones paso a paso para validar y diagnosticar la configuración de conectividad de puntos de conexión privados. 

Un punto de conexión privado de Azure es una interfaz de red que le conecta de forma privada y segura a un servicio de Private Link. Esta solución le ayuda a proteger sus cargas de trabajo en Azure al proporcionar conectividad privada a los recursos del servicio de Azure desde la red virtual. De este modo, los servicios se incorporan a la red virtual. 

Estos son los escenarios de conectividad que están disponibles con los puntos de conexión privados 
- red virtual de la misma región 
- redes virtuales emparejadas regionalmente
- redes virtuales emparejadas globalmente
- cliente local a través de VPN o circuitos de ExpressRoute

## <a name="diagnosing-connectivity-problems"></a>Diagnóstico de problemas de conectividad 
Siga los pasos que se indican a continuación para asegurarse de que todas las configuraciones habituales son las esperadas a fin de resolver los problemas de conectividad con la configuración del punto de conexión privado.

1. Revisión de la configuración del punto de conexión privado examinando el recurso 

    a) Vaya al **Centro de Private Link**.

      ![Centro de Private Link](./media/private-endpoint-tsg/private-link-center.png)

    b) Seleccione Puntos de conexión privados en el panel de navegación izquierdo.
    
      ![Puntos de conexión privados](./media/private-endpoint-tsg/private-endpoints.png)

    c) Filtre y seleccione el punto de conexión privado que quiera diagnosticar.

    d) Revise la red virtual y la información de DNS.
    
     - Compruebe que el estado de la conexión es **Aprobado**.
     - Asegúrese de que la máquina virtual tenga conectividad con la red virtual que hospeda los puntos de conexión privados.
     - Información de FQDN (copia) y dirección IP privada asignada
    
       ![Configuración de red virtual y DNS](./media/private-endpoint-tsg/vnet-dns-configuration.png)    
    
2. Uso de [**Azure Monitor**](https://docs.microsoft.com/azure/azure-monitor/overview) para revisar el flujo de los datos

    a) En el recurso de punto de conexión privado, seleccione **Supervisión**.
     - Seleccione los datos de entrada o salida y revise si se transmiten al intentar conectarse al punto de conexión privado. Espere un retraso de aproximadamente 10 minutos.
    
       ![Verificación de la telemetría del punto de conexión privado](./media/private-endpoint-tsg/private-endpoint-monitor.png)

3. Uso de la solución de problemas de conexión de máquinas virtuales desde **Network Watcher**

    a) Seleccione la máquina virtual cliente.

    b) Seleccione la pestaña **Conexión saliente** de la sección **Solución de problemas de conexión**.
    
      ![Network Watcher: prueba de las conexiones salientes](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)
    
    c) Seleccione **Use Network Watcher for detail connection tracing** (Usar Network Watcher para el seguimiento de conexión detallado).
    
      ![Network Watcher: solución de problemas de conexión](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d) Seleccione **Test by FQDN** (Probar por FQDN).
     - Pegue el FQDN del recurso de punto de conexión privado.
     - Proporcione un puerto (*normalmente 443 para Azure Storage o COSMOS, 1336 para SQL...* ).

    e) Haga clic en **Probar** y valide los resultados de la prueba.
    
      ![Network Watcher: resultados de la prueba](./media/private-endpoint-tsg/network-watcher-test-results.png)
    
        
4. La resolución de DNS de los resultados de la prueba debe tener la misma dirección IP privada asignada al punto de conexión privado.

    a) Si la configuración de DNS es incorrecta, haga lo siguiente:
     - Con una zona privada: 
       - Asegúrese de que la red virtual de la máquina virtual cliente está asociada a la zona privada.
       - Compruebe que el registro de la zona DNS privado existe. Si no es así, créelo.
    
     - Con un DNS personalizado:
       - Revise la configuración de DNS del cliente y valide que la configuración de DNS sea correcta.
       Consulte el artículo [Información general sobre el punto de conexión privado: configuración de DNS](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#dns-configuration) para obtener instrucciones.

    b) Si se produce un error en la conectividad debido a NSG/UDR:
     - Revise las reglas de salida de NSG y cree reglas de salida adecuadas para permitir el tráfico.
    
       ![Reglas de salida de NSG](./media/private-endpoint-tsg/nsg-outbound-rules.png)

5. Si la conexión tiene resultados validados, el problema de conectividad puede estar relacionado con otros aspectos, como secretos, tokens o contraseñas a nivel de aplicación.
   - En este caso, revise la configuración del recurso de Private Link asociado al punto de conexión privado. Consulte la [guía de solución de problemas de Private Link](troubleshoot-private-link-connectivity.md). 

6. Póngase en contacto con el equipo de [soporte técnico de Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) si el problema sigue sin resolverse y el problema de conectividad persiste. 

## <a name="next-steps"></a>Pasos siguientes

 * [Creación de un punto de conexión privado en la subred actualizada (Azure Portal)](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)

 * [Guía de solución de problemas de Private Link](troubleshoot-private-link-connectivity.md)
