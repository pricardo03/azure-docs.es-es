---
title: 'Tutorial: Equilibrio de carga del tráfico de Internet a las máquinas virtuales: Azure Portal'
titlesuffix: Azure Load Balancer
description: Este tutorial muestra cómo crear y administrar una instancia de Standard Load Balancer mediante Azure Portal.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create and Standard Load balancer so that I can load balance internet traffic to VMs and add and remove VMs from the load-balanced set.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2019
ms.author: kumud
ms.custom: seodec18
ms.openlocfilehash: fa40f4f666444209f70d3f49b7947450af01ec36
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2019
ms.locfileid: "56983293"
---
# <a name="tutorial-load-balance-internet-traffic-to-vms-using-the-azure-portal"></a>Tutorial: Equilibrio de carga del tráfico de Internet en máquinas virtuales mediante Azure Portal

El equilibrio de carga proporciona un mayor nivel de disponibilidad y escala, ya que distribuye las solicitudes entrantes entre varias máquinas virtuales. En este tutorial, conocerá los distintos componentes de Azure Standard Load Balancer que distribuyen el tráfico de Internet a máquinas virtuales y proporcionan una alta disponibilidad. Aprenderá a:


> [!div class="checklist"]
> * Creación de un equilibrador de carga de Azure
> * Crear máquinas virtuales e instalar el servidor IIS
> * Creación de recursos del equilibrador de carga
> * Ver un equilibrador de carga en acción
> * Agregar y quitar las máquinas virtuales de un equilibrador de carga

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en Azure Portal en [http://portal.azure.com](http://portal.azure.com).

## <a name="create-a-standard-load-balancer"></a>Crear un equilibrador de carga estándar

En esta sección, se crea un equilibrador de carga público que ayuda a equilibrar la carga de las máquinas virtuales. La versión Estándar de Load Balancer solo admite direcciones IP públicas estándar. Cuando se crea una instancia de Standard Load Balancer, también se debe crear una nueva dirección IP pública estándar que se configura como front-end (denominado *LoadBalancerFrontend* de forma predeterminada) para dicha instancia. 

1. En la parte superior izquierda de la pantalla, haga clic en **Crear un recurso** > **Redes** > **Azure Load Balancer**.
2. En la pestaña **Datos básicos** de la página **Crear equilibrador de carga**, escriba o seleccione la siguiente información, acepte los valores predeterminados del resto de la configuración y, luego, seleccione **Revisar y crear**:

    | Configuración                 | Valor                                              |
    | ---                     | ---                                                |
    | Subscription               | Seleccione su suscripción.    |    
    | Grupos de recursos         | Seleccione **Crear nuevo** y escriba *MyResourceGroupSLB* en el cuadro de texto.|
    | NOMBRE                   | *myLoadBalancer*                                   |
    | Region         | Seleccione **Europa Occidental**.                                        |
    | Type          | Seleccione **Público**.                                        |
    | SKU           | Seleccione **Estándar**.                          |
    | Dirección IP pública | Seleccione **Crear nuevo**. |
    | Nombre de la dirección IP pública              | Escriba *myPublicIP* en el cuadro de texto.   |
    |Zona de disponibilidad| Seleccione **Redundancia de zona**.    |
3. En la pestaña **Revisar y crear**, haga clic en **Crear**.   

  
## <a name="create-backend-servers"></a>Creación de servidores back-end

En esta sección, se crea una red virtual, luego tres máquinas virtuales para el grupo de back-end de la instancia de Load Balancer y, finalmente, se instala IIS en las máquinas virtuales para ayudar a probar el equilibrador de carga.

### <a name="create-a-virtual-network"></a>Creación de una red virtual
1. En la parte superior izquierda de Azure Portal, haga clic en **Crear un recurso** > **Redes** > **Red virtual** y especifique estos valores para la red virtual:
    |Configuración|Valor|
    |---|---|
    |NOMBRE|Escriba *myVNet*.|
    |Subscription| Seleccione su suscripción.|
    |Grupos de recursos| Seleccione **Usar existente** y después seleccione *myResourceGroupSLB*.|
    |Nombre de subred| Escriba *myBackendSubnet*.|
    
2. Haga clic en **Crear** para crear la red virtual.

### <a name="create-virtual-machines"></a>Creación de máquinas virtuales

1. En la parte superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Proceso** > **Windows Server 2016 Datacenter** y especifique estos valores para la máquina virtual:
    1. Escriba *myVM1* como nombre de la máquina virtual.        
    2. En **Grupo de recursos**, seleccione **Usar existente** y, después, seleccione *myResourceGroupSLB*.
2. Haga clic en **OK**.
3. Seleccione **DS1_V2** como tamaño de la máquina virtual y haga clic en **Seleccionar**.
4. Especifique estos valores para la configuración de la máquina virtual:
    1. Asegúrese de que *myVNet* está seleccionada como red virtual y que se selecciona *myBackendSubnet* como la subred.
    2. Como **Dirección IP pública**, en el panel **Crear dirección IP pública**, seleccione **Estándar** y, finalmente, seleccione **Aceptar**.
    3. Como **Grupo de seguridad de red**, seleccione **Avanzado**, y, a continuación, haga lo siguiente:
        1. Seleccione *Grupo de seguridad de red (firewall) y en la página **Elegir grupo de seguridad de red**, seleccione **Crear nuevo**. 
        2. En la página **Elegir grupo de seguridad de red**, en **Nombre**, escriba *myNetworkSecurityGroup* como nombre del grupo de seguridad de red y, finalmente, seleccione **Aceptar**.
5. Haga clic en **Deshabilitado** para deshabilitar los diagnósticos de arranque.
6. Haga clic en **Aceptar**, revise la configuración en la página de resumen y haga clic en **Crear**.
7. Cree dos máquinas virtuales más llamadas *VM2* y *VM3*, y elija *myVnet* como la red virtual, *myBackendSubnet* como su subred y *myNetworkSecurityGroup* como su grupo de seguridad de red mediante los pasos 1 a 6. 

### <a name="create-network-security-group-rule"></a>Creación de una regla de grupo de seguridad de red

En esta sección, va a crear una regla de grupo de seguridad de red para permitir conexiones entrantes que usen HTTP.

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, en la lista de recursos haga clic en **myNetworkSecurityGroup**, que se encuentra en el grupo de recursos **myResourceGroupSLB**.
2. En **Configuración**, haga clic en **Reglas de seguridad de entrada** y, después, en **Agregar**.
3. Especifique estos valores para la regla de seguridad de entrada llamada *myHTTPRule* para permitir que las conexiones HTTP entrantes usen el puerto 80:
    - *Etiqueta de servicio*: en **Origen**.
    - *Internet*: en **Etiqueta de servicio de origen**
    - *80*: en **Intervalos de puerto de destino**
    - *TCP*: en **Protocolo**
    - *Permitir*: en **Acción**
    - *100* en **Prioridad**
    - *myHTTPRule* como nombre
    - *Permitir HTTP*: como descripción
4. Seleccione **Agregar**.

### <a name="install-iis-on-vms"></a>Instalación de IIS en las máquinas virtuales

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, en la lista de recursos, haga clic en **myVM1**, que se encuentra en el grupo de recursos *myResourceGroupSLB*.
2. En la página **Información general**, haga clic en **Conectar** a RDP en la máquina virtual.
3. En la ventana emergente **Conectarse a una máquina virtual**, seleccione **Descargar archivo RDP** y, a continuación, abra el archivo RDP descargado.
4. En la ventana **Conexión a Escritorio remoto**, haga clic en **Conectar**.
5. Inicie sesión en la máquina virtual con las credenciales que proporcionó durante la creación de esta máquina virtual. Esto inicia una sesión de escritorio remoto con la máquina virtual *myVM1*.
6. En el escritorio del servidor, vaya a **Herramientas administrativas de Windows**>**Windows PowerShell**.
7. En la ventana de PowerShell, ejecute los comandos siguientes para instalar el servidor IIS, eliminar el archivo iisstart.htm predeterminado y agregar uno nuevo que muestre el nombre de la máquina virtual:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. Cierre la sesión de RDP con *myVM1*.
7. Repita los pasos 1 a 6 para instalar IIS y el archivo iisstart.htm actualizado en *myVM2* y *myVM3*.

## <a name="create-load-balancer-resources"></a>Creación de recursos del equilibrador de carga

En esta sección se configura el equilibrador de carga para un grupo de direcciones de back-end y un sondeo de mantenimiento, y se especifica una regla del equilibrador de carga.

### <a name="create-a-backend-address-pool"></a>Creación de un grupo de direcciones de back-end

Para distribuir el tráfico a las máquinas virtuales, un grupo de direcciones de back-end contiene las direcciones IP de las tarjetas de interfaz de red (NIC) virtuales conectadas al equilibrador de carga. Cree el grupo de direcciones de back-end *myBackendPool* para incluir *VM1* y *VM2*.

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, haga clic en **myLoadBalancer** en la lista de recursos.
2. En **Configuración**, haga clic en **Grupos de back-end** y luego en **Agregar**.
3. En la página **Agregar grupo back-end**, realice lo siguiente:
   - En el nombre, escriba *myBackEndPool* como nombre del grupo de servidores back-end.
   - Como **Red virtual**, seleccione *myVNet*.
   - Agregue *myVM1*, *myVM2* y *myVM3* en **Máquina virtual** junto con sus direcciones IP correspondientes y, a continuación, seleccione **Agregar**.
4. Compruebe que la configuración del grupo de back-end del equilibrador de carga muestra todas las máquinas virtuales: *myVM1*, *myVM2* y *myVM3* y, a continuación, haga clic en **Aceptar**.

### <a name="create-a-health-probe"></a>Creación de un sondeo de estado

Para permitir que el equilibrador de carga supervise el estado de la aplicación, utilice un sondeo de estado. El sondeo de estado agrega o quita de forma dinámica las máquinas virtuales de la rotación del equilibrador de carga en base a su respuesta a las comprobaciones de estado. Cree un sondeo de estado, *myHealthProbe*, para supervisar el estado de las máquinas virtuales.

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, haga clic en **myLoadBalancer** en la lista de recursos.
2. En **Configuración**, haga clic en **Sondeos de estado** y luego en **Agregar**.
3. Use estos valores para crear el sondeo de estado:
    - *myHealthProbe*: como nombre del sondeo de estado.
    - **HTTP**: en tipo de protocolo.
    - *80*: en número de puerto.
    - *15*: como número de **Intervalo**, en segundos, entre los intentos de sondeo.
    - *2*: como número de **Umbral incorrecto** o errores de sondeo consecutivos que deben producirse para que una máquina virtual se considere que no funciona de manera incorrecta.
4. Haga clic en **Aceptar**.

   ![Incorporación de un sondeo](./media/load-balancer-standard-public-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga

Las reglas de equilibrador de carga se utilizan para definir cómo se distribuye el tráfico a las máquinas virtuales. Defina la configuración de la IP de front-end para el tráfico entrante y el grupo de IP de back-end para el tráfico entrante, junto con los puertos de origen y destino requeridos. Cree una regla de Load Balancer *myLoadBalancerRuleWeb* para escuchar el puerto 80 en el front-end *FrontendLoadBalancer* y enviar tráfico de red con equilibrio de carga al conjunto de direcciones back-end, *myBackEndPool*, que también usan el puerto 80. 

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, haga clic en **myLoadBalancer** en la lista de recursos.
2. En **Configuración**, haga clic en **Reglas de equilibrio de carga** y luego en **Agregar**.
3. Use estos valores para configurar la regla de equilibrio de carga:
    - *myHTTPRule*: como nombre de la regla de equilibrio de carga.
    - **TCP**: en tipo de protocolo.
    - *80*: en número de puerto.
    - *80*: como puerto de back-end.
    - *myBackendPool*: como nombre del grupo back-end.
    - *myHealthProbe*: como nombre del sondeo de estado.
4. Haga clic en **OK**.

## <a name="test-the-load-balancer"></a>Prueba del equilibrador de carga
1. Busque la dirección IP pública de Load Balancer en la pantalla **Información general**. Haga clic en **Todos los recursos** y, después, en **myPublicIP**.

2. Copie la dirección IP pública y péguela en la barra de direcciones del explorador. La página predeterminada del servidor web de IIS se muestra en el explorador.

      ![Servidor web de IIS](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Para ver cómo el equilibrador de carga distribuye el tráfico entre las tres máquinas virtuales que ejecutan la aplicación, puede realizar una actualización forzada del explorador web.

## <a name="remove-or-add-vms-from-the-backend-pool"></a>Eliminación o incorporación de máquinas virtuales del grupo de back-end
Puede que tenga que realizar labores de mantenimiento de las máquinas virtuales que ejecutan la aplicación, como la instalación de actualizaciones del sistema operativo. Para gestionar un aumento de tráfico a la aplicación, tiene que agregar más máquinas virtuales. Esta sección le muestra cómo quitar o agregar una máquina virtual desde el equilibrador de carga.

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, haga clic en **myLoadBalancer** en la lista de recursos.
2. En **Configuración**, haga clic en **Grupos de back-end** y, a continuación, en la lista del grupo de back-end, haga clic en **myBackendPool**.
3. En la página **myBackendPool**, en **Configuraciones IP de red de destino**, para quitar *VM1* del back-end haga clic en el icono de eliminación junto a **Máquina Virtual: myVM1**

Una vez quitada *myVM1* del grupo de direcciones de back-end, puede realizar cualquier tarea de mantenimiento en *myVM1* como, por ejemplo, la instalación de actualizaciones de software. En ausencia de *VM1**, la carga se equilibra ahora entre *myVM2* y *myVM3*. 

Para agregar *myVM1* de nuevo en el grupo de back-end, siga el procedimiento descrito en la sección *Incorporación de máquinas virtuales al grupo de back-end* de este artículo.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, el equilibrador de carga y todos los recursos relacionados. Para ello, seleccione el grupo de recursos que contiene el equilibrador de carga y haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un equilibrador de carga estándar, le ha asociado máquinas virtuales, ha configurado la regla de tráfico del equilibrador de carga y el sondeo de mantenimiento y, después, ha probado el equilibrador de carga. Además, ha quitado una máquina virtual del conjunto de carga equilibrada y la ha agregado de vuelta al grupo de direcciones back-end. Para más información acerca de Azure Load Balancer, diríjase a los tutoriales correspondientes.

> [!div class="nextstepaction"]
> [Tutoriales de Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
