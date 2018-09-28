---
title: Filtro del tráfico entrante con la DNAT de Azure Firewall mediante Azure Portal
description: En este tutorial, aprenderá a implementar y configurar la DNAT de Azure Firewall mediante Azure Portal.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 766ad04251fbe404d43734115e41e23ae0a4be28
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982059"
---
# <a name="tutorial-filter-inbound-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>Tutorial: Filtro del tráfico entrante con la DNAT de Azure Firewall mediante Azure Portal

La traducción de direcciones de red de destino (DNAT) de Azure Firewall se puede configurar para traducir y filtrar el tráfico que entra a las subredes. Azure Firewall no tiene el concepto de las reglas de entrada y reglas de salida. Hay reglas de aplicación y reglas de red, y se aplican a todo el tráfico que entra en el firewall. En primer lugar se aplican las reglas de red, después las reglas de aplicación y las reglas finalizan.

>[!NOTE]
>La característica DNAT de Firewall solo está disponible actualmente en Azure PowerShell y REST.

Por ejemplo, si alguna regla de red coincide, las reglas de aplicación ya no evaluarán el paquete. Si no coincide ninguna regla de red, y si el protocolo del paquete es HTTP/HTTPS, las reglas de aplicación lo evalúan posteriormente. Si sigue sin haber coincidencias, el paquete se evalúa con la [colección de reglas de infraestructura](infrastructure-fqdns.md). Si sigue sin haber coincidencias, el paquete se deniega de forma predeterminada.

Al configurar DNAT, en la acción de colección de la regla NAT se selecciona **Traducción de direcciones de red de destino (DNAT)**. La dirección IP y el puerto públicos del firewall se traducen en una dirección IP y un puerto privados. Luego las reglas se aplican de la forma habitual, primero las de red y luego las de aplicaciones. Por ejemplo, se puede configurar una regla de red que permita el tráfico del Escritorio remoto en el puerto TCP 3389. Primero se produce la traducción de direcciones y, después, se aplican tanto las reglas de red como las de aplicaciones mediante las direcciones traducidas.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar un entorno de red de prueba
> * Implementar un firewall
> * Crear una ruta predeterminada
> * Configurar una regla de DNAT
> * Configurar una regla de red
> * Probar el firewall

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

En este tutorial se crearán dos redes virtuales emparejadas:
- **VN-Hub**: el firewall está en esta red virtual.
- **VN-Spoke**: el servidor de carga de trabajo está en esta red virtual.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
1. Inicie sesión en Azure Portal en [http://portal.azure.com](http://portal.azure.com).
1. En la página principal de Azure Portal, haga clic en **Grupos de recursos** y, a continuación, haga clic en **Agregar**.
2. En **Nombre del grupo de recursos**, escriba **RG-DNAT-Test**.
3. En **Suscripción**, seleccione la suscripción.
4. En **Ubicación del grupo de recursos**: seleccione una ubicación. Todos los recursos posteriores que cree deben estar en la misma ubicación.
5. Haga clic en **Create**(Crear).

## <a name="set-up-the-network-environment"></a>Configuración del entorno de red
En primer lugar, cree las redes virtuales y, después, emparéjelas.

### <a name="create-the-hub-vnet"></a>Creación de la red virtual Hub
1. En la página principal de Azure Portal, haga clic en **Todos los servicios**.
2. En **Redes**, haga clic en **Redes virtuales**.
3. Haga clic en **Agregar**.
4. En **Nombre**, escriba **VN-Hub**.
5. En **Espacio de direcciones**, escriba **10.0.0.0/16**.
7. En **Suscripción**, seleccione la suscripción.
8. En **Grupo de recursos**, seleccione **Usar existente** y, después, **RG-DNAT-Test**.
9. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
10. En **Subred**, como **Nombre** escriba **AzureFirewallSubnet**.

     El firewall estará en esta subred y el nombre de la subred **debe** ser AzureFirewallSubnet.
     > [!NOTE]
     > El tamaño mínimo de la subred AzureFirewallSubnet es /25.
11. En **Intervalo de direcciones**, escriba **10.0.1.0/24**.
12. Utilice los otros valores predeterminados y, a continuación, haga clic en **Crear**.

### <a name="create-a-spoke-vnet"></a>Creación de la red virtual Spoke

1. En la página principal de Azure Portal, haga clic en **Todos los servicios**.
2. En **Redes**, haga clic en **Redes virtuales**.
3. Haga clic en **Agregar**.
4. En **Nombre**, escriba **VN-Spoke**.
5. En **Espacio de direcciones**, escriba **192.168.0.0/16**.
7. En **Suscripción**, seleccione la suscripción.
8. En **Grupo de recursos**, seleccione **Usar existente** y, después, **RG-DNAT-Test**.
9. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
10. En **Subred**, en **Nombre** escriba **SN-Workload**.

    El servidor estará en esta subred.
1. En **Intervalo de direcciones**, escriba **192.168.1.0/24**.
2. Utilice los otros valores predeterminados y, a continuación, haga clic en **Crear**.

### <a name="peer-the-vnets"></a>Emparejamiento de las redes virtuales

Ahora empareje las dos redes virtuales.

#### <a name="hub-to-spoke"></a>De centro a radio

1. Haga clic en la red virtual **VN-Hub**.
2. En **Configuración**, haga clic en **Emparejamientos**.
3. Haga clic en **Agregar**.
4. Escriba **Peer-HubSpoke** como nombre.
5. Seleccione **VN-Spoke** como red virtual.
7. Haga clic en **OK**.

#### <a name="spoke-to-hub"></a>De radio al centro

1. Haga clic en la red virtual **VN-Spoke**.
2. En **Configuración**, haga clic en **Emparejamientos**.
3. Haga clic en **Agregar**.
4. Escriba **Peer-SpokeHub** como nombre.
5. Seleccione **VN-Hub** como red virtual.
6. Haga clic en **Permitir tráfico reenviado**.
7. Haga clic en **OK**.

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

Crear una máquina virtual de cargas de trabajo y colóquela en la subred **SN-Workload**.

1. En la página principal de Azure Portal, haga clic en **Todos los servicios**.
2. En **Compute**, haga clic en **Máquinas virtuales**.
3. Haga clic en **Agregar** y haga clic en **Windows Server**, haga clic en **Windows Server 2016 Datacenter** y, finalmente, haga clic en **Crear**.

**Aspectos básicos**

1. En **Nombre**, escriba **Srv-Workload**.
5. Escriba un nombre de usuario y una contraseña.
6. En **Suscripción**, seleccione la suscripción.
7. En **Grupo de recursos**, haga clic en **Usar existente** y, después, seleccione **RG-DNAT-Test**.
8. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
9. Haga clic en **OK**.

**Tamaño**

1. Elija un tamaño apropiado para una máquina virtual de prueba que ejecute Windows Server. Por ejemplo, **B2ms** (8 GB de RAM, 16 GB de almacenamiento).
2. Haga clic en **Seleccionar**.

**Configuración**

1. En **Red**, en **Red virtual**, seleccione **VN-Spoke**.
2. En **Subred**, seleccione **SN-Workload**.
3. Haga clic en **Dirección IP pública** y, después, haga clic en **No**.
4. En **Seleccionar puertos de entrada públicos**, seleccione **No public inbound ports** (No hay puertos de entrada públicos). 
2. Deje los demás valores predeterminados y haga clic en **Aceptar**.

**Resumen**

Revise el resumen y luego haga clic en **Crear**. Esta operación tardará algunos minutos en completarse.

Al finalizar la implementación, anote la dirección IP privada de la máquina virtual, ya que tendrá que usarla más adelante, al configurar el firewall. Haga clic en el nombre de la máquina virtual y, en **Configuración**, haga clic en **Redes** para buscar la dirección IP privada.


## <a name="deploy-the-firewall"></a>Implementación del firewall

1. En la página principal del portal, haga clic en **Crear un recurso**.
2. Haga clic en **Redes** y, después, en **Destacados**, haga clic en **Ver todo**.
3. Haga clic en **Firewall** y, luego, en **Crear**. 
4. En la página **Creación de un firewall**, utilice la tabla siguiente para configurar el firewall:
   
   |Configuración  |Valor  |
   |---------|---------|
   |NOMBRE     |FW-DNAT-test|
   |Subscription     |\<su suscripción\>|
   |Grupos de recursos     |**Usar existente**: RG-DNAT-Test |
   |Ubicación     |Seleccione la misma ubicación que usó anteriormente.|
   |Elegir una red virtual     |**Usar existente**: VN-Hub|
   |Dirección IP pública     |**Cree uno nuevo**. La dirección IP pública tiene que ser del tipo de SKU estándar.|

2. Haga clic en **Revisar + crear**.
3. Revise el resumen y luego haga clic en **Crear** para crear el firewall.

   La implementación tardará varios minutos.
4. Tras finalizar la implementación, vaya al grupo de recursos **RG-DNAT-Test** y haga clic en el firewall **FW-DNAT-test**.
6. Anote la dirección IP privada. Se usará más adelante al crear la ruta predeterminada.


## <a name="create-a-default-route"></a>Crear una ruta predeterminada

En la subred **SN-Workload**, configure la ruta predeterminada de salida para que cruce el firewall.

1. En la página principal de Azure Portal, haga clic en **Todos los servicios**.
2. En **Redes**, haga clic en **Tablas de rutas**.
3. Haga clic en **Agregar**.
4. En **Nombre**, escriba **RT-FWroute**.
5. En **Suscripción**, seleccione la suscripción.
6. En **Grupo de recursos**, seleccione **Usar existente** y, después, **RG-DNAT-Test**.
7. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
8. Haga clic en **Create**(Crear).
9. Haga clic en **Actualizar**y, luego, haga clic en la tabla de rutas **RT-FWroute**.
10. Haga clic en **Subredes** y en **Asociar**.
11. Haga clic en **Red virtual**y seleccione **VN-Spoke**.
12. En **Subred**, haga clic en **SN-Workload**.
13. Haga clic en **OK**.
14. Haga clic en **Rutas** y, después, en **Agregar**.
15. En **Nombre de ruta**, escriba **FW-DG**.
16. En **Prefijo de dirección** escriba **0.0.0.0/0**.
17. En **Tipo del próximo salto**, seleccione **Aplicación virtual**.

    Azure Firewall es realmente un servicio administrado, pero una aplicación virtual funciona en esta situación.
1. En **Dirección del próximo salto**, escriba la dirección IP privada del firewall que anotó anteriormente.
2. Haga clic en **OK**.


## <a name="configure-a-dnat-rule"></a>Configuración de una regla de DNAT

```azurepowershell-interactive
 $rgName  = "RG-DNAT-Test"
 $firewallName = "FW-DNAT-test"
 $publicip = type the Firewall public ip
 $newAddress = type the private IP address for the Srv-Workload virtual machine 
 
# Get Firewall
    $firewall = Get-AzureRmFirewall -ResourceGroupName $rgName -Name $firewallName
  # Create NAT rule
    $natRule = New-AzureRmFirewallNatRule -Name RL-01 -SourceAddress * -DestinationAddress $publicip -DestinationPort 3389 -Protocol TCP -TranslatedAddress $newAddress -TranslatedPort 3389
  # Create NAT rule collection
    $natRuleCollection = New-AzureRmFirewallNatRuleCollection -Name RC-DNAT-01 -Priority 200 -Rule $natRule
  # Add NAT Rule collection to firewall:
    $firewall.AddNatRuleCollection($natRuleCollection)
  # Save:
    $firewall | Set-AzureRmFirewall
```
## <a name="configure-a-network-rule"></a>Configuración de una regla de red

1. Abra **DNAT-RG-Test**y haga clic en el firewall **FW-DNAT-test**.
1. En la página **FW-DNAT-test**, en **Configuración**, haga clic en **Reglas**.
2. Haga clic en **Agregar una colección de reglas de red**.

Use la siguiente tabla para configurar la regla con la siguiente tabla y, después, haga clic en **Agregar**:


|Parámetro  |Valor  |
|---------|---------|
|NOMBRE     |**RC-Net-01**|
|Prioridad     |**200**|
|.     |**Permitir**|

En **Reglas**:

|Parámetro  |Configuración  |
|---------|---------|
|NOMBRE     |**RL-RDP**|
|Protocolo     |**TCP**|
|Direcciones de origen     |*|
|Direcciones de destino     |Dirección IP privada de **Srv-Workload**|
|Puertos de destino|**3389**|


## <a name="test-the-firewall"></a>Prueba del firewall

1. Conecte un Escritorio remoto a la dirección IP pública del firewall. Debe estar conectado a la máquina virtual **Srv-Workload**.
3. Cierre el Escritorio remoto.
4. Cambie la acción de la colección de la regla de red **RC-Net-01** a **Denegar**.
5. Intente volver a conectarse a la dirección IP pública del firewall. Esta vez no debería poder hacerlo por la regla de **denegación**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede conservar los recursos relacionados con el firewall para el siguiente tutorial o, si ya no los necesita, eliminar el grupo de recursos **RG-DNAT-Test** para eliminarlos todos.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió a:

> [!div class="checklist"]
> * Configurar un entorno de red de prueba
> * Implementar un firewall
> * Crear una ruta predeterminada
> * Configurar una regla de DNAT
> * Configurar una regla de red
> * Probar el firewall

A continuación, puede supervisar los registros de Azure Firewall.

> [!div class="nextstepaction"]
> [Tutorial: Supervisión de los registros de Azure Firewall](./tutorial-diagnostics.md)
