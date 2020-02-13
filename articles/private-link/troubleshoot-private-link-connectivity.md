---
title: Solución de problemas de conectividad de Azure Private Link
description: Instrucciones paso a paso para diagnosticar la conectividad de Private Link
services: private-link
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
ms.openlocfilehash: 0d26ad6802e551523875dcad13066fdbdbf39ada
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77191053"
---
# <a name="troubleshoot-private-link-service-connectivity-problems"></a>Solución de problemas de conectividad del servicio Private Link

En esta guía se proporcionan instrucciones paso a paso para validar y diagnosticar la conectividad de su configuración del servicio Private Link. 

Azure Private Link le permite acceder a los servicios PaaS de Azure (por ejemplo, Azure Storage, Azure Cosmos DB y SQL Database) y a los servicios de asociados o clientes hospedados de Azure mediante un punto de conexión privado de la red virtual. El tráfico entre la red virtual y el servicio atraviesa la red troncal de Microsoft, eliminando la exposición a la red pública de Internet. También puede crear su propio servicio Private Link en la red virtual y enviarlo de forma privada a los clientes. 

Puede habilitar el servicio que se ejecuta de manera subyacente en la instancia de Standard Load Balancer para el acceso de Private Link. Los consumidores del servicio pueden crear un punto de conexión privado dentro de su red virtual y asignarlo a este servicio para tener acceso a él de forma privada.

Estos son los escenarios de conectividad que están disponibles con el servicio Private Link:
- red virtual de la misma región 
- redes virtuales emparejadas regionalmente
- redes virtuales emparejadas globalmente
- cliente local a través de VPN o circuitos de ExpressRoute

## <a name="deployment-troubleshooting"></a>Solución de problemas de implementación

Revise la información sobre [cómo deshabilitar las directivas de red en el servicio Private Link](https://docs.microsoft.com/azure/private-link/disable-private-link-service-network-policy) para obtener casos de solución de problemas en los que no puede seleccionar la dirección IP de origen de la subred que prefiere para su servicio Private Link.

Asegúrese de que el valor **privateLinkServiceNetworkPolicies** está deshabilitado para la subred de la que está seleccionando la dirección IP de origen.

## <a name="diagnosing-connectivity-problems"></a>Diagnóstico de problemas de conectividad

Si tiene problemas de conectividad con la configuración de Private Link, siga los pasos que se indican a continuación para asegurarse de que todas las configuraciones habituales son las esperadas.

1. Revisión de la configuración del servicio Private Link examinando el recurso 

    a) Vaya al **Centro de Private Link**.

      ![Centro de Private Link](./media/private-link-tsg/private-link-center.png)

    b) Seleccione **Servicios Private Link** en el panel de navegación izquierdo.

      ![Servicios Private Link](./media/private-link-tsg/private-link-service.png)

    c) Filtre y seleccione el servicio Private Link que quiera diagnosticar.

    d) Revise las conexiones del punto de conexión privado.
     - Asegúrese de que el punto de conexión privado desde el que está buscando la conectividad se muestra en la lista con el estado de conexión **Aprobado**. 
     - Si está **Pendiente**, selecciónelo y apruébelo. 

       ![Conexiones de punto de conexión privado](./media/private-link-tsg/pls-private-endpoint-connections.png)

     - Navegue hasta el punto de conexión privado desde el que se está conectando. Para ello, haga clic en el nombre. Asegúrese de que en el estado de la conexión se muestra el estado **Aprobado**.

       ![Información general de la conexión del punto de conexión privado](./media/private-link-tsg/pls-private-endpoint-overview.png)

     - Una vez que se aprueban ambos lados, vuelva a intentar la conectividad.

    e) Revise la opción **Alias** en la pestaña Información general y el valor de **id. de recurso** en la pestaña Propiedades. 
     - Asegúrese de que **el alias o el id. de recurso** coincida con el **alias o el id. de recurso** que usa para crear un punto de conexión privado a este servicio. 

       ![Verificación del alias](./media/private-link-tsg/pls-overview-pane-alias.png)

       ![Verificación del id. de recurso](./media/private-link-tsg/pls-properties-pane-resourceid.png)

    f) Revise la información de visibilidad (sección Información general).
     - Asegúrese de que la suscripción se encuentra en el ámbito **Visibilidad**.

       ![Verificación de la visibilidad](./media/private-link-tsg/pls-overview-pane-visibility.png)

    g) Revise la información de Load Balancer (Información general).
     - Puede navegar hasta Load Balancer haciendo clic en el vínculo Load Balancer.

       ![Verificación de Load Balancer](./media/private-link-tsg/pls-overview-pane-ilb.png)

     - Asegúrese de que la configuración de Load Balancer esté configurada según sus expectativas.
       - Revise la configuración IP de front-end.
       - Revise los grupos de back-end.
       - Revise las reglas de equilibrio de carga.

       ![Verificación de las propiedades de Load Balancer](./media/private-link-tsg/pls-ilb-properties.png)

     - Asegúrese de que Load Balancer funciona según la configuración anterior.
       - Seleccione una máquina virtual en cualquier subred que no sea la subred en la que está disponible el grupo de back-end de Load Balancer.
       - Intente acceder al front-end de Load Balancer desde la máquina virtual anterior.
       - Si la conexión llega al grupo de back-end según las reglas de equilibrio de carga, significa que Load Balancer está operativo.
       - También puede revisar la métrica de Load Balancer a través de Azure Monitor para ver si los datos se transmiten a través de Load Balancer.

2. Uso de [**Azure Monitor**](https://docs.microsoft.com/azure/azure-monitor/overview) para revisar el flujo de los datos

    a) En el recurso del servicio Private Link, seleccione **Métricas**.
     - Seleccione **Bytes de entrada** o **Bytes de salida**.
     - Revise que se transmiten datos al intentar conectarse al servicio Private Link. Espere un retraso de aproximadamente 10 minutos.

       ![Verificación de las métricas del servicio Private Link](./media/private-link-tsg/pls-metrics.png)

3. Póngase en contacto con el equipo de [soporte técnico de Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) si el problema sigue sin resolverse y el problema de conectividad persiste. 

## <a name="next-steps"></a>Pasos siguientes

 * [Creación de un servicio Private Link (CLI)](https://docs.microsoft.com/azure/private-link/create-private-link-service-cli)

 * [Guía de solución de problemas de puntos de conexión privados](troubleshoot-private-endpoint-connectivity.md)
