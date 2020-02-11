---
title: 'Inicio rápido: Creación de un servicio Private Link mediante Azure Portal'
titlesuffix: Azure Private Link
description: Obtenga información sobre cómo crear un punto de conexión de Private Link mediante Azure Portal en este inicio rápido.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 02/03/2020
ms.author: allensu
ms.openlocfilehash: 4dd27d3fb4321c2eecf222136376bfd4b49790bb
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026208"
---
# <a name="quickstart-create-a-private-link-service-using-the-azure-portal"></a>Inicio rápido: Creación de un servicio Private Link mediante Azure Portal

El servicio Azure Private Link es la referencia a su propio servicio que usa la tecnología de Azure Private Link. El servicio o recurso que está funcionando detrás de Azure Load Balancer se puede habilitar para el acceso a Private Link. Los consumidores del servicio pueden tener acceso al servicio de forma privada desde sus propias redes virtuales. En este inicio rápido va a aprender a crear un servicio Private Link con Azure Portal.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.


## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-an-internal-load-balancer"></a>Creación de equilibrador de carga interno

En primer lugar, cree una red virtual y, a continuación, un equilibrador de carga interno para usarlo con el servicio Azure Private Link.

### <a name="create-the-virtual-network"></a>Crear la red virtual

En esta sección, va a crear una red virtual y una subred para hospedar el equilibrador de carga que se usa para acceder al servicio Private Link.


1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Redes** > **Red virtual**.
   
1. En el panel **Crear red virtual**, escriba o seleccione estos valores:
   
   - **Name**: escriba **MyVNet**.
   - **ResourceGroup**: seleccione **Crear nuevo**, a continuación, escriba **MyResourceGroupLB** y seleccione **Aceptar**. 
   - **Subred** > **Nombre**: escriba **MyBackendSubnet**.
   
1. Seleccione **Crear**.

   ![Creación de una red virtual](../load-balancer/media/tutorial-load-balancer-basic-internal-portal/2-load-balancer-virtual-network.png)

### <a name="create-a-standard-load-balancer"></a>Creación de un equilibrador de carga estándar

Cree un equilibrador de carga interno estándar mediante el portal. El nombre y dirección IP que cree se configuran automáticamente como front-end del equilibrador de carga.

1. En el lado superior izquierdo del portal, seleccione **Crear un recurso** > **Redes** > **Load Balancer**.
   
2. En la pestaña **Datos básicos** de la página **Crear equilibrador de carga**, escriba o seleccione la siguiente información, acepte los valores predeterminados del resto de la configuración y, luego, seleccione **Revisar y crear**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Subscription               | Seleccione su suscripción.    |    
    | Resource group         | Seleccione *MyResourceGroupLB* en el cuadro desplegable.|
    | Nombre                   | *myLoadBalancer*                                   |
    | Region         | Seleccione **Este de EE. UU. 2**.                                        |
    | Tipo          | seleccione **Interno**.                                        |
    | SKU           | Seleccione **Estándar**.                          |
    | Virtual network           | Seleccione *MyVNet*.                          |    
    | Asignación de dirección IP              | Seleccione **Estática**.   |
    | Dirección IP privada|escriba una dirección que esté en el espacio de direcciones de la red virtual y subred, por ejemplo *10.3.0.7*.  |

3. En la pestaña **Revisar y crear**, haga clic en **Crear**. 
   

### <a name="create-standard-load-balancer-resources"></a>Creación de recursos del equilibrador de carga estándar

En esta sección, va a configurar el equilibrador de carga para un grupo de direcciones de back-end y un sondeo de mantenimiento, y a especificar las reglas del equilibrador de carga.

#### <a name="create-a-backend-pool"></a>Creación de un grupo de back-end

Para distribuir el tráfico a los recursos, un grupo de direcciones de back-end contiene las direcciones IP de las tarjetas de interfaz de red (NIC) virtuales conectadas a Load Balancer. Cree el grupo de direcciones de back-end *myBackendPool* para incluir recursos para el tráfico de equilibrio de carga.

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.
2. En **Configuración**, seleccione **Grupos de back-end** y, a continuación, seleccione **Agregar**.
3. En la página **Agregar un grupo back-end**, en nombre, escriba *myBackEndPool*, como el nombre del grupo de back-end y, a continuación, seleccione **Aceptar**.



#### <a name="create-a-health-probe"></a>Creación de un sondeo de estado

Para permitir que el equilibrador de carga supervise el estado del recurso, utilice un sondeo de mantenimiento. El sondeo de estado agrega o quita de forma dinámica los recursos de la rotación del equilibrador de carga en función de su respuesta a las comprobaciones de estado. 

**Para crear un sondeo de mantenimiento con el fin de supervisar el estado de los recursos:**

1. Seleccione **Todos los recursos** en el menú de la izquierda y, a continuación, en la lista de recursos seleccione **MyLoadBalancer**.
   
1. En **Configuración**, seleccione **Sondeos de mantenimiento** y, a continuación, seleccione **Agregar**.
   
1. En la página **Agregar sondeo de mantenimiento**, escriba o seleccione los siguientes valores:
   
   - **Name**: escriba **MyHealthProbe**.
   - **Protocolo**: En la lista desplegable, seleccione **TCP**. 
   - **Puerto**: escriba **80**. 
   - **Intervalo**: escriba **15**. El valor Intervalo es el número de segundos entre los intentos de sondeo.
   - **Umbral incorrecto**: escriba **2**. Este valor es el número de errores de sondeo consecutivos que tienen que producirse para que se considere que una máquina virtual no funciona correctamente.
   
1. Seleccione **Aceptar**.

#### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga

Una regla del equilibrador de carga define cómo se distribuye el tráfico a los recursos. La regla define la configuración IP de front-end para el tráfico entrante y el grupo de direcciones IP de back-end para recibir el tráfico y los puertos de origen y destino requeridos. 

La regla del equilibrador de carga llamada **MyLoadBalancerRule** escucha en el puerto 80 en el front-end **LoadBalancerFrontEnd**. La regla envía el tráfico de red al grupo de direcciones de back-end **MyBackendPool**, también a través del puerto 80. 

**Para crear la regla de equilibrador de carga:**

1. Seleccione **Todos los recursos** en el menú de la izquierda y, a continuación, en la lista de recursos seleccione **MyLoadBalancer**.
   
1. En **Configuración**, seleccione **Reglas de equilibrio de carga** y, a continuación, seleccione **Agregar**.
   
1. En la página **Agregar regla de equilibrio de carga**, escriba o seleccione los valores siguientes, si todavía no están presentes:
   
   - **Name**: escriba **MyLoadBalancerRule**.
   - **Dirección IP de front-end**: escriba **LoadBalancerFrontEnd** si no está ya presente.
   - **Protocolo**: seleccione **TCP**.
   - **Puerto**: escriba **80**.
   - **Puerto back-end**: escriba **80**.
   - **Grupo de back-end**: seleccione **MyBackendPool**.
   - **Sondeo de mantenimiento**: seleccione **MyHealthProbe**. 
   
1. Seleccione **Aceptar**.
   
## <a name="create-a-private-link-service"></a>Creación de un servicio Private Link

En esta sección, creará un servicio Private Link detrás de un equilibrador de carga estándar.

1. En la parte superior izquierda de la pantalla en Azure Portal, seleccione **Crear un recurso** > **Redes** > **Private Link Center (versión preliminar)** . También puede buscar Private Link mediante una búsqueda en el portal.

2. En **Private Link Center - Overview** (Centro de Private Link: Información general), en la opción**Build a private connection to a service** (Crear una conexión privada a un servicio), seleccione Start (Iniciar).

3. En **Crear un servicio Private Link: Conceptos básicos**, escriba o seleccione esta información:

    | Configuración           | Value                                                                        |
    |-------------------|------------------------------------------------------------------------------|
    | **Detalles del proyecto**:  |                                                                              |
    | Subscription      | Seleccione su suscripción.                                                     |
    | Grupo de recursos    | Seleccione *MyResourceGroupLB*.                                                     |
    | **DETALLES DE INSTANCIA**: |                                                                              |
    | Nombre              | Escriba *myPrivateLinkService*. |
    | Region            | Seleccione *Este de EE. UU. 2*.                                                         |

4. Seleccione **Siguiente: Configuración de salida**

5. En **Crear un servicio Private Link: Configuración de salida**, escriba o seleccione esta información:


    | Configuración                           | Value                                                                           |
    |-----------------------------------|---------------------------------------------------------------------------------|
    | Load Balancer                     | Seleccione *myLoadBalancer*.                                                           |
    | Dirección IP de front-end del equilibrador de carga | Seleccione la dirección IP de front-end de MyLoadBalancer.                                |
    | Red virtual NAT de origen        | Seleccione *myVNET*.                                                                   |
    | Subred NAT de origen                 | Seleccione *myBackendSubnet*.                                                          |
    | Habilitar proxy TCP V2               | Seleccione Sí/NO en función de si la aplicación espera el encabezado proxy TCP V2. |
    | Configuración de dirección IP privada       | Configure el método de asignación y la dirección IP para cada dirección IP de NAT.                  |

6. Seleccione **Siguiente: Seguridad de acceso**

7. En **Crear un servicio Private Link: Seguridad de acceso**, escriba o seleccione esta información:

    | Configuración                                     | Value                                                             |
    |---------------------------------------------|-------------------------------------------------------------------|
    | Visibilidad                                  | Elija *Solo control de acceso basado en rol*.         |
  
8. Seleccione **Siguiente: Etiquetas**, a continuación, **Revisar y crear** o elija la pestaña **Revisar y crear** en la parte superior de la página.

9. Revise la información y seleccione **Crear**.

## <a name="clean-up-resources"></a>Limpieza de recursos 
Cuando haya terminado de usar el servicio Private Link, elimine el grupo de recursos para limpiar los recursos que se usan en este inicio rápido:

1. Escriba *myResourceGroupLB* en el cuadro **Buscar** de la parte superior del portal y seleccione *myResourceGroupLB* en los resultados de búsqueda. 
2. Seleccione **Eliminar grupo de recursos**. 
3. Escriba myResourceGroup en **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un equilibrador de carga interno de Azure y un servicio Private Link. Para más información sobre cómo crear puntos de conexión privados, consulte [Inicio rápido: Creación de un punto de conexión privado mediante Azure Portal](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal).
