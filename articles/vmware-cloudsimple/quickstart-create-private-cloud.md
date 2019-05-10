---
title: Solución de VMware de Azure por CloudSimple Quickstart - crear una nube privada
description: Obtenga información sobre cómo crear y configurar una nube privada con la solución de VMware de Azure por CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e1fc7809ad94d589483b87c638d027a39098164e
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209540"
---
# <a name="quickstart---configure-a-private-cloud-environment"></a>Inicio rápido: configurar un entorno de nube privada

En este artículo, obtenga información sobre cómo crear una nube privada CloudSimple y configurar el entorno de nube privada.

## <a name="sign-in-to-azure"></a>Iniciar sesión en Azure
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-private-cloud"></a>Creación de una nube privada

1. Seleccione **Todos los servicios**.
2. Busque **CloudSimple servicios**.
3. Seleccione el servicio CloudSimple en el que desea crear su propia nube privada.
4. En la introducción, haga clic en **crear una nube privada** para abrir una nueva pestaña del explorador para CloudSimple portal.  Si se le solicite, inicie sesión con su Azure inicie sesión en las credenciales.  

    ![Creación de la nube privada de Azure](media/create-private-cloud-from-azure.png)

5. En el portal de CloudSimple, proporcione un nombre para su propia nube privada
6. Seleccione el **ubicación** de su nube privada
7. Seleccione el **tipo de nodo** adquirió en Azure.  Puede elegir el [opción CS28 o CS36](cloudsimple-node.md#vmware-solution-by-cloudsimple-nodes-sku). Esta última opción incluye la capacidad de proceso y memoria máxima.
8. Especifique el **número de nodos**.  Los nodos como mínimo tres son necesarios para crear una nube privada

    ![Creación de la nube privada: información básica](media/create-private-cloud-basic-info.png)

9. Haga clic en **Siguiente: Opciones avanzadas**.
10. Escriba el intervalo CIDR de subredes de vSphere/vSAN. Asegúrese de que el intervalo CIDR no se superponga con ninguno de sus instalaciones o a otras subredes de Azure.

    ![Creación de la nube privada: opciones avanzadas](media/create-private-cloud-advanced-options.png)

11. Seleccione **Siguiente: Revisar y crear**.
12. Revise la configuración. Si necesita cambiar la configuración, haga clic en **anterior**.
13. Haga clic en **Create**(Crear).

Se iniciará la nube privada de proceso de aprovisionamiento.  Puede tardar hasta dos horas para la nube privada que se aprovisione.

## <a name="launch-cloudsimple-portal"></a>Iniciar CloudSimple portal

Puede tener acceso a CloudSimple portal de Azure portal.  Se iniciará el portal CloudSimple con su Azure inicie sesión en las credenciales mediante el inicio de sesión único (SSO).  Acceso al portal CloudSimple precisa que autorice el **CloudSimple servicio autorización** aplicación.  Para obtener más información sobre la concesión de permisos, consulte [dar su consentimiento a la aplicación de autorización de servicio CloudSimple](https://docs.azure.cloudsimple.com/access-cloudsimple-portal/#consent-to-cloudsimple-service-authorization-application)

1. Seleccione **Todos los servicios**.
2. Busque **CloudSimple servicios**.
3. Seleccione el servicio CloudSimple en el que desea crear su propia nube privada.
4. En la introducción, haga clic en **ir al portal CloudSimple** para abrir una nueva pestaña del explorador para CloudSimple portal.  Si se le solicite, inicie sesión con su Azure inicie sesión en las credenciales.  

    ![Iniciar CloudSimple Portal](media/launch-cloudsimple-portal.png)

## <a name="create-point-to-site-vpn"></a>Crear VPN de punto a sitio

Una conexión VPN de punto a sitio es la manera más sencilla de conectarse a su propia nube privada desde el equipo. Usar conexión de VPN de punto a sitio si se conecta a la nube privada de forma remota.  Para obtener acceso rápido a su propia nube privada, siga estos pasos.  Acceso a la región CloudSimple desde su red local puede realizarse mediante [VPN de sitio a sitio](https://docs.azure.cloudsimple.com/vpn-gateway/) o [Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection/).

### <a name="create-gateway"></a>Crear puerta de enlace

1. Iniciar CloudSimple portal y seleccione **red**.
2. Seleccione **puerta de enlace VPN**.
3. Haga clic en **nueva puerta de enlace VPN**.

    ![Creación de una puerta de enlace de VPN](media/create-vpn-gateway.png)

4. Para **configuración de puerta de enlace**, especifique la siguiente configuración y haga clic en **siguiente**.

    * Seleccione **Point-to-Site VPN** como el tipo de puerta de enlace.
    * Escriba un nombre para identificar la puerta de enlace.
    * Seleccione la ubicación de Azure donde se implementa el servicio CloudSimple.
    * Especifique la subred de cliente para la puerta de enlace de punto a sitio.  Direcciones DHCP se proporcionará en esta subred cuando se conecta.

5. Para **conexión/usuario**, especifique la siguiente configuración y haga clic en **siguiente**.

    * Para permitir automáticamente que todos los usuarios actuales y futuros tener acceso a la nube privada a través de esta puerta de enlace de punto a sitio, seleccione **agregar automáticamente todos los usuarios**. Cuando se selecciona esta opción, se seleccionan automáticamente todos los usuarios de la lista de usuarios. Puede invalidar la opción automática anulando la selección de usuarios individuales en la lista.
    * Para seleccionar solo los usuarios individuales, haga clic en las casillas de verificación en la lista de usuarios.

6. La sección de VLAN y subredes permite especificar la administración y usuario VLAN y subredes para la puerta de enlace y conexiones.

    * El **agregar automáticamente** opciones establecen la directiva global para esta puerta de enlace. La configuración se aplica a la puerta de enlace actual. Se puede invalidar la configuración en el **seleccione** área.
    * Seleccione **agregar administración de VLAN y subredes de las nubes privadas**. 
    * Para agregar las VLAN y subredes todo definido por el usuario, haga clic en **agregar las VLAN y subredes definido por el usuario**. 
    * El **seleccione** configuración invalidará la configuración global en **agregar automáticamente**. 

7. Haga clic en **siguiente** para revisar la configuración. Haga clic en los iconos de edición para realizar cambios.
8. Haga clic en **crear** para crear la puerta de enlace VPN.

### <a name="connect-to-cloudsimple-using-point-to-site-vpn"></a>Conectarse a CloudSimple mediante VPN de punto a sitio

Se necesita el cliente VPN para conectarse a CloudSimple desde su equipo.  Descargar [OpenVPN cliente](https://openvpn.net/community-downloads/) para Windows o [viscosidad](https://www.sparklabs.com/viscosity/download/) para Mac OS y OS X.

1. Iniciar CloudSimple portal y seleccione **red**.
2. Seleccione **puerta de enlace VPN**.
3. En la lista de puertas de enlace VPN, haga clic en la puerta de enlace VPN de punto a sitio.
4. Seleccione **Usuarios**.
5. Haga clic en **descargar mi configuración de VPN**

    ![Descargar configuración VPN](media/download-p2s-vpn-configuration.png)

6. Importar la configuración en el cliente VPN

    * Instrucciones para [importar configuración de cliente de Windows](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program)
    * Instrucciones para [importar configuración en Mac OS o OS X](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection)

7. Conectarse a CloudSimple

## <a name="create-a-vlan-for-your-workload-vms"></a>Creación de una VLAN para la carga de trabajo de máquinas virtuales

Después de crear una nube privada, cree una VLAN donde va a implementar sus máquinas virtuales de carga de trabajo y aplicación.

1. En el CloudSimple Portal, seleccione **red**.
2. Haga clic en **VLAN y subredes**.
3. Haga clic en **crear subred/VLAN**

    ![Crear subred/VLAN](media/create-new-vlan-subnet.png)

4. Seleccione el **nube privada** para la nueva subred/VLAN.
5. Seleccione un identificador de VLAN en la lista.  
6. Escriba un nombre de subred para identificar la subred.
7. Especifique el intervalo de CIDR de subred y la máscara.  Este intervalo no debe solaparse con las subredes existentes.
8. Haga clic en **Enviar**.

    ![Crear detalles de la subred/VLAN](media/create-new-vlan-subnet-details.png)

Se creará la subred/VLAN.  Ahora puede usar este identificador de VLAN para crear un grupo de puertos distribuidas en el nube privada de vCenter. 

## <a name="connect-your-environment-to-an-azure-virtual-network"></a>Conectar el entorno a una red virtual de Azure

CloudSimple proporciona un circuito ExpressRoute para la nube privada. Puede conectar su red virtual en Azure al circuito ExpressRoute. Para obtener detalles completos acerca de cómo configurar la conexión, siga los pasos de [conexión de red Virtual de Azure mediante ExpressRoute](https://docs.azure.cloudsimple.com/cloudsimple-azure-network-connection/)

## <a name="sign-in-to-vcenter"></a>Inicie sesión en vCenter

Se puede ahora iniciar sesión vCenter configurar las máquinas virtuales y las directivas.

1. Para obtener acceso a vCenter, iniciar desde el portal de CloudSimple. En la página principal, en **tareas comunes de**, haga clic en **iniciar cliente vSphere**.  Seleccione la nube privada y, a continuación, haga clic en **iniciar cliente vSphere** en la nube privada.

    ![Inicie el cliente de vSphere](media/launch-vcenter-from-cloudsimple-portal.png)

2. Seleccione el cliente de vSphere preferido para tener acceso a vCenter e inicie sesión con su nombre de usuario y contraseña.  Los valores predeterminados son:
    * Nombre de usuario: **CloudOwner@cloudsimple.local**
    * Contraseña: **CloudSimple123!**  

Las pantallas de vCenter en los procedimientos siguientes se encuentran en el cliente de vSphere (HTML5).

## <a name="change-your-vcenter-password"></a>Cambiar la contraseña de vCenter

CloudSimple recomienda cambiar la contraseña la primera vez que inicie sesión en vCenter.  
La contraseña que estableció debe cumplir los siguientes requisitos:

* Duración máxima: Debe cambiar la contraseña cada 365 días
* Restringe la reutilización: Los usuarios no pueden reutilizar cualquiera de las últimas cinco contraseñas
* Duración: 8 - 20 caracteres
* Caracteres especiales: Al menos un carácter especial
* Caracteres alfabéticos: Al menos un carácter en minúscula, a-z, A-z y al menos un carácter en mayúscula
* Números: Al menos un carácter numérico, 0-9
* Máximo de caracteres adyacente idéntico: Tres

    Ejemplo: CC o CCC es aceptable como parte de la contraseña, pero no es CCCC.

Si establece una contraseña que no cumple los requisitos:

* Si usa el cliente Flash vSphere, notifica un error
* Si usa al cliente de HTML5, no notifica un error. El cliente no acepta el cambio y la contraseña antigua continúa funcionando.

## <a name="change-nsx-administrator-password"></a>Cambiar contraseña de administrador NSX

El Administrador de NSX se implementa con una contraseña predeterminada.  Se recomienda que cambiar la contraseña después de crear su propia nube privada.

   * Nombre de usuario: **admin**
   * Contraseña: **CloudSimple123!**

Puede encontrar el nombre de dominio completo (FQDN) y la dirección IP del Administrador de NSX en CloudSimple portal.

1. Iniciar CloudSimple portal y seleccione **recursos**.
2. Haga clic en la nube privada, lo que desee usar.
3. Seleccione **vSphere red de administración**
4. Use la dirección IP o FQDN de **NSX Manager** y conectarse con un explorador web. 

    ![Busque NSX Manager FQDN](media/private-cloud-nsx-manager-fqdn.png)

Para cambiar la contraseña, siga las instrucciones de [instalación NSX Manager](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.2/com.vmware.nsxt.install.doc/GUID-A65FE3DD-C4F1-47EC-B952-DEDF1A3DD0CF.html).

## <a name="create-a-port-group"></a>Crear un grupo de puertos

Para crear un grupo de puertos distribuido de vSphere:

1. Siga las instrucciones de "Agregar un grupo de puertos distribuida," en el [vSphere Networking Guide](https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-networking-guide.pdf).
2. Al configurar el grupo de puertos distribuida, proporcione el identificador de VLAN que creó en [crear una VLAN para las máquinas virtuales de carga de trabajo](#create-a-vlan-for-your-workload-vms).

## <a name="next-steps"></a>Pasos siguientes

* [Usar máquinas virtuales de VMware en Azure](https://docs.azure.cloudsimple.com/quickstart-create-vmware-virtual-machine)
* [Usar máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* [Conectarse a la red local mediante Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection/)
* [Configurar VPN de sitio a sitio desde un entorno local](https://docs.azure.cloudsimple.com/vpn-gateway/)
