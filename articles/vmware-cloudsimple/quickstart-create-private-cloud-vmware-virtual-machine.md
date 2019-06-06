---
title: 'Inicio rápido: crear una máquina virtual de VMware en una nube privada'
description: Describe cómo crear y una VM de VMware en la nube privada CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/03/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5837fd615b8dbf32ff289a2e0d09da9db51a908d
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481510"
---
# <a name="create-vmware-virtual-machines-on-your-private-cloud"></a>Crear máquinas virtuales de VMware en su propia nube privada

Para crear máquinas virtuales en su propia nube privada, comience por el acceso al portal de CloudSimple desde el portal de Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Acceso al portal de CloudSimple

1. Seleccione **Todos los servicios**.
2. Busque **CloudSimple servicios**.
3. Seleccione el servicio CloudSimple en el que desea crear su propia nube privada.
4. Desde el **Introducción** página, haga clic en **ir al portal CloudSimple** para abrir una nueva pestaña del explorador para CloudSimple portal.  Si se le solicite, inicie sesión con su Azure inicie sesión en las credenciales.  

    ![Iniciar CloudSimple portal](media/launch-cloudsimple-portal.png)

## <a name="launch-vcenter-web-ui"></a>Iniciar la interfaz de usuario web vCenter

Ahora puede iniciar vCenter para configurar las máquinas virtuales y las directivas.

Para obtener acceso a vCenter, iniciar desde el portal de CloudSimple. En la página principal, en **tareas comunes de**, haga clic en **iniciar cliente vSphere**.  Seleccione la nube privada y, a continuación, haga clic en **iniciar cliente vSphere** en la nube privada.

   ![Inicie el cliente de vSphere](media/launch-vcenter-from-cloudsimple-portal.png)

## <a name="upload-an-iso-or-vsphere-template"></a>Cargar una plantilla de vSphere o ISO

> [!WARNING]
> Para cargas ISO, use el cliente de vSphere HTML5.  Con Flash cliente puede producir un error.

1. Obtener la plantilla de vSphere o ISO que desea cargar en vCenter para crear una máquina virtual y lo tiene disponible en el sistema local.

2. En vCenter, haga clic en el **disco** icono y seleccione **vsanDatastore**. Haga clic en **archivos** y, a continuación, haga clic en **nueva carpeta**.

    ![vCenter ISO](media/vcenter-create-folder.png)

3. Cree una carpeta para almacenar los archivos ISO.

4. Vaya a la nueva carpeta creada y haga clic en **cargar archivos**. Siga la pantalla instrucciones para cargar la imagen ISO.

## <a name="create-a-virtual-machine-in-vcenter"></a>Crear una máquina Virtual en vCenter

1. En vCenter, haga clic en el **Hosts y clústeres** icono.

2. Haga clic en **carga de trabajo** y seleccione **nueva máquina Virtual**.
    
    ![Crear máquina virtual](media/create-vcenter-virtual-machine-01.png)

3. Seleccione **crear nueva máquina virtual** y haga clic en **siguiente**.

    ![Asistente para nueva máquina virtual](media/create-vcenter-virtual-machine-02.png)

4. Nombre de la máquina, seleccione la **la máquina virtual de carga de trabajo** carpeta y haga clic en **siguiente**.

    ![Seleccione el nombre y una carpeta](media/create-vcenter-virtual-machine-03.png)

5. Seleccione el **carga de trabajo** recursos de proceso y haga clic en **siguiente**.

    ![Seleccionar recurso de proceso](media/create-vcenter-virtual-machine-04.png)

6. Seleccione **vsanDatastore** y haga clic en **siguiente**.

    ![Selección de Storage](media/create-vcenter-virtual-machine-05.png)

7. Mantenga la selección de compatibilidad de ESXi 6.5 predeterminada y haga clic en **siguiente**.

    ![Active la compatibilidad](media/create-vcenter-virtual-machine-06.png)

8. Seleccione el sistema operativo invitado de la imagen ISO de la máquina virtual y haga clic en **siguiente**.

    ![Personalizar el sistema operativo invitado](media/create-vcenter-virtual-machine-07.png)

9. Seleccione el disco duro y las opciones de red. Unidad de CD/DVD nuevo, seleccione **archivo Datastore ISO**.  Si desea permitir el tráfico desde la dirección IP pública a esta máquina virtual, seleccione la red como **vm-1**.

    ![Seleccione la personalización de hardware](media/create-vcenter-virtual-machine-08.png)

10. Abre una ventana de selección. Seleccione el archivo que ha cargado previamente en la carpeta imágenes ISO y plantillas y haga clic en **Aceptar**.

    ![Seleccionar ISO](media/create-vcenter-virtual-machine-10.png)

11. Revise la configuración y haga clic en **Aceptar** para crear la máquina virtual.

    ![Revisar opciones](media/create-vcenter-virtual-machine-11.png)

La máquina virtual se agrega ahora a los recursos de proceso de carga de trabajo y está lista para su uso. 

![Nueva máquina virtual en vCenter](media/create-vcenter-virtual-machine-12.png)

La configuración básica está completa ahora. Puede empezar a usar su propia nube privada similar a cómo se podría usar la infraestructura de máquina virtual en el entorno local.

En las secciones siguientes contienen información opcional sobre cómo configurar DNS y DHCP servidores para la nube privada de las cargas de trabajo y modificar la configuración de red predeterminada.

## <a name="add-users-and-identity-sources-to-vcenter-optional"></a>Agregar usuarios y los orígenes de identidad a vCenter (opcional)

CloudSimple asigna una cuenta de usuario de vCenter predeterminado con el nombre de usuario **cloudowner@cloudsimple.local** . Ninguna configuración adicional de cuenta es necesaria para que pueda comenzar.  CloudSimple normalmente asigna a los administradores de los privilegios que necesitan para realizar las operaciones normales.  Configure la instancia de Active Directory o Azure AD como un [origen de identidades adicional](https://docs.azure.cloudsimple.com/set-vcenter-identity/) en su propia nube privada.

## <a name="create-a-dns-and-dhcp-server-optional"></a>Crear un servidor DHCP y DNS (opcional)

Las aplicaciones y cargas de trabajo que se ejecutan en un entorno de nube privada requieren la resolución de nombres y los servicios DHCP para la búsqueda y la asignación de direcciones IP. Se requiere una infraestructura DHCP y DNS adecuada para proporcionar estos servicios. Puede configurar una máquina virtual en vCenter para proporcionar estos servicios en su entorno de nube privada.

### <a name="prerequisites"></a>Requisitos previos

* Un grupo de puertos distribuidos con VLAN configurado

* Ruta a un entorno local o servidores DNS basados en Internet a

* Plantilla de máquina virtual o ISO para crear una máquina virtual

Los vínculos siguientes proporcionan instrucciones sobre cómo configurar los servidores DHCP y DNS en Windows y Linux.

### <a name="linux-based-dns-server-setup"></a>Configuración del servidor DNS basado en Linux

Linux ofrece diversos paquetes para la configuración de servidores DNS.  Este es un vínculo a instrucciones sobre cómo configurar un servidor BIND DNS de código abierto.

[Configuración de ejemplo](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-centos-7)

### <a name="windows-based-setup"></a>Instalación basada en Windows

Estos artículos de Microsoft describe cómo configurar un servidor de Windows como un servidor DNS, como un servidor DHCP.
<br>
[Windows Server como servidor DNS](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

[Windows Server como servidor DHCP](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)

## <a name="customize-networking-configuration-optional"></a>Personalizar la configuración de red (opcional)

Las páginas de la red en el portal de CloudSimple le permiten especificar la configuración para las tablas de firewall y las direcciones IP públicas para las máquinas virtuales.

### <a name="allocate-public-ips"></a>Asignar direcciones IP públicas

1. Vaya a **red > dirección IP pública** en el portal de CloudSimple.
2. Haga clic en **asignar dirección IP pública**.
3. Escriba un nombre para identificar la entrada de dirección IP.
4. Seleccione la ubicación de su nube privada.
5. Utilice el control deslizante para cambiar el tiempo de espera de inactividad si lo desea.
6. Escriba la dirección IP local para el que desea asignar una dirección IP pública.
7. Si lo desea, escriba un nombre DNS asociado.
8. Haga clic en **Done**(Listo).

    ![Dirección IP pública](media/quick-create-pc-public-ip.png)

Comienza la tarea de asignar la dirección IP pública. Puede comprobar el estado de la tarea en el **actividad > tareas** página. Cuando se completa la asignación, se muestra la nueva entrada en la página de direcciones IP públicas.

La máquina virtual a la que se debe asignar esta dirección IP debe configurarse con la dirección local especificada anteriormente. El procedimiento para configurar una dirección IP es específico para el sistema operativo de máquina virtual. Consulte la documentación de su sistema operativo de máquina virtual para el procedimiento correcto.

#### <a name="example"></a>Ejemplo

Por ejemplo, estos son los detalles para Ubuntu 16.04.

Agregue el método estático para la configuración de familia de direcciones inet en el archivo ```/etc/network/interfaces```. Cambie los valores de dirección, la máscara de red y la puerta de enlace. En este ejemplo, estamos usando la interfaz eth0, dirección IP interna 192.168.24.10, dirección de puerta de enlace 192.168.24.1 y máscara de red 255.255.255.0. 

Edite el archivo ```interfaces``` .

```
sudo vi /etc/network/interfaces
```

Actualice la siguiente sección en ```interfaces``` archivo.

```
auto eth0
iface eth0 inet static
address 192.168.24.10
netmask 255.255.255.0
gateway 192.168.24.1
dns-nameservers 8.8.8.8
dns-domain acme.com
dns-search acme.com
```

Deshabilitar la interfaz.

```
sudo ifdown eth0
```

Habilitar la interfaz de nuevo.

```
sudo ifup eth0
```

De forma predeterminada, todo el tráfico entrante desde Internet es **denegado**. Si desea abrir cualquier otro puerto, cree un [tabla firewall](https://docs.azure.cloudsimple.com/firewall/).

Después de configurar una dirección IP interna como la dirección IP estática, compruebe que puede acceder a Internet desde la máquina virtual.

```
ping 8.8.8.8
```

Compruebe que puede acceder a la máquina virtual desde Internet mediante la dirección IP pública.

Asegúrese de que las reglas de firewall (iptable) en la máquina virtual no están bloqueando el puerto 80 entrante.

```
netstat -an | grep 80
```

Iniciar un servidor http que escucha en el puerto 80.
       
```
python2.7 -m SimpleHTTPServer 80
```

o

```
python3 -m http.server 80
```

Inicie un explorador en el escritorio y hacer que apunte al puerto 80 para la dirección IP pública examinar los archivos en la máquina virtual. 

### <a name="default-cloudsimple-firewall-rules-for-public-ip"></a>Reglas de firewall CloudSimple predeterminado para la dirección IP pública

* Tráfico VPN: Se permite todo el tráfico entre (desde/a) la VPN y todas las redes de la carga de trabajo y red de administración.
* Tráfico interno de la nube privada: Se permite todo el tráfico este-oeste entre (desde/a) las redes de la carga de trabajo y la red de administración (mostrado arriba).
* Tráfico de Internet:
    * Se deniega todo el tráfico entrante desde Internet a redes de la carga de trabajo y la red de administración.
    * Se permite todo el tráfico saliente a Internet de las redes de la carga de trabajo o la red de administración.

También puede modificar la forma en que se protege el tráfico, mediante la característica de reglas de Firewall. Para obtener más información, consulte [configurar tablas de firewall y reglas](https://docs.azure.cloudsimple.com/firewall/).

## <a name="install-solutions-optional"></a>Instalar las soluciones (opcionales)
Puede instalar las soluciones en la nube privada CloudSimple para aprovechar al máximo su propia nube privada en el entorno de vCenter. Puede configurar copia de seguridad, recuperación ante desastres, replicación y otras funciones para proteger las máquinas virtuales. Algunos ejemplos son el Administrador de VMware Site Recovery (VMware SRM) y Veeam Backup & Replication.

Para instalar una solución, debe solicitar privilegios adicionales durante un período limitado. Consulte [escalar privilegios](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges).

## <a name="next-steps"></a>Pasos siguientes

* [Usar máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* [Conectarse a la red local mediante Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection)
* [Configurar puertas de enlace VPN en red CloudSimple](https://docs.azure.cloudsimple.com/vpn-gateway)
