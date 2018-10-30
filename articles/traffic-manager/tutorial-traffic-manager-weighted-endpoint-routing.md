---
title: 'Tutorial: Enrutamiento del tráfico a puntos de conexión ponderados mediante Azure Traffic Manager | Microsoft Docs'
description: En este artículo tutorial se describe cómo enrutar el tráfico a puntos de conexión ponderados con Traffic Manager.
services: traffic-manager
author: KumudD
Customer intent: As an IT Admin, I want to distribute traffic based on the weight assigned to a website endpoint so that I can control the user traffic to a given website.
ms.service: traffic-manager
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: kumud
ms.openlocfilehash: 171a9cfe5b5b63efde31c37cd49ef548f1e4ebcc
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649442"
---
# <a name="tutorial-control-traffic-routing-with-weighted-endpoints-using-traffic-manager"></a>Tutorial: Control del enrutamiento del tráfico con puntos de conexión ponderados mediante Traffic Manager 

En este tutorial se describe cómo usar Traffic Manager para controlar el enrutamiento del tráfico de usuario entre los puntos de conexión mediante el método de enrutamiento ponderado. En este método de enrutamiento, asignará pesos a cada punto de conexión de la configuración del perfil de Traffic Manager y el tráfico de usuario se enrutará según el peso asignado a cada punto de conexión. El peso es un entero comprendido entre 1 y 1000. Cuanto mayor sea el valor del peso asignado a un punto de conexión, más alta será su prioridad.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear dos máquinas virtuales que ejecutan un sitio web básico en IIS
> * Crear dos máquinas virtuales de prueba para ver a Traffic Manager en acción
> * Configurar el nombre DNS para las máquinas virtuales que ejecutan IIS
> * Crear un perfil de Traffic Manager
> * Agregar puntos de conexión de máquina virtual en el perfil de Traffic Manager
> * Ver a Traffic Manager en acción

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos
Para ver a Traffic Manager en acción, este tutorial requiere que implemente lo siguiente:
- Dos instancias de sitios web básicos que se ejecuten en diferentes regiones de Azure (**Este de EE. UU.** y **Europa Occidental**).
- Dos máquinas virtuales para probar Traffic Manager (una máquina virtual en **Este de EE. UU.** y la segunda máquina virtual en **Europa Occidental**). Las máquinas virtuales de prueba se usan para ilustrar cómo Traffic Manager enruta el tráfico de usuario a un sitio web que tiene un peso mayor asignado a su punto de conexión.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure 

Inicie sesión en Azure Portal en https://portal.azure.com.

### <a name="create-websites"></a>Creación de sitios web

En esta sección, creará dos instancias de sitio web que proporcionan los puntos de conexión de servicio para el perfil de Traffic Manager en dos regiones de Azure. Para crear los dos sitios web, realice los siguientes pasos:
1. Crear dos máquinas virtuales para ejecutar un sitio web básico: una en **Este de EE. UU.** y otra en **Europa Occidental**.
2. Instale un servidor IIS en cada máquina virtual y actualice la página del sitio web predeterminada que describe el nombre de la máquina virtual a la que un usuario se conecta cuando visita el sitio web.

#### <a name="create-vms-for-running-websites"></a>Creación de máquinas virtuales para ejecutar sitios web
En esta sección, creará dos máquinas virtuales *myIISVMEastUS* y *myIISVMWEurope* en las regiones de Azure **Este de EE. UU.** y **Europa Occidental**.

1. En la esquina superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Compute** > **Windows Server 2016 VM** (VM de Windows Server 2016).
2. Escriba o seleccione la siguiente información para **Aspectos básicos**, acepte los valores predeterminados para el resto de la configuración y luego seleccione **Crear**:

    |Configuración|Valor|
    |---|---|
    |NOMBRE|myIISVMEastUS|
    |Nombre de usuario| Escriba un nombre de usuario de su elección.|
    |Password| Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupos de recursos| Seleccione **Nuevo** y luego escriba *myResourceGroupTM1*.|
    |Ubicación| Seleccione **Este de EE. UU**.|
    |||
4. Seleccione un tamaño de máquina virtual en **Elegir un tamaño**.
5. Seleccione los valores siguientes en **Configuración** y, después, seleccione **Aceptar**:
    
    |Configuración|Valor|
    |---|---|
    |Virtual network| Seleccione **Red virtual**, en **Crear red virtual**, para **Nombre**, escriba *myVNet1*, para cada subred, escriba *mySubnet*.|
    |Grupo de seguridad de red (NSG)|Seleccione **Básico** y, en la lista desplegable **Seleccionar puertos de entrada públicos**, seleccione **HTTP** y **RDP** |
    |Diagnósticos de arranque|Seleccione **Deshabilitado**.|
    |||
6. En **Crear** de la página **Resumen**, seleccione **Crear** para iniciar la implementación de la máquina virtual.

7. Complete de nuevo los pasos del 1 al 6, con los cambios siguientes:

    |Configuración|Valor|
    |---|---|
    |Grupos de recursos | Seleccione **Nuevo** y, a continuación, escriba *myResourceGroupTM2*|
    |Ubicación|Europa occidental|
    |Nombre de la máquina virtual | myIISVMWEurope|
    |Virtual network | Seleccione **Red virtual**, en **Crear red virtual**, para **Nombre**, escriba *myVNet2*, para cada subred, escriba *mySubnet*.|
    |||
8. Las máquinas virtuales tardan unos minutos en crearse. No siga con los pasos restantes hasta que se creen ambas máquinas virtuales.

![Crear una VM](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>Instalación de IIS y personalización de la página web predeterminada

En esta sección, instalará el servidor IIS en las dos máquinas virtuales (*myIISVMEastUS*  & *myIISVMWEurope*) y luego actualizará la página del sitio web predeterminado. La página del sitio web personalizado muestra el nombre de la máquina virtual a la que se está conectando cuando visita el sitio web desde un explorador web.

1. Seleccione **Todos los recursos** en el menú de la izquierda y, en la lista de recursos, haga clic en *myIISVMEastUS*, que se encuentra en el grupo de recursos *myResourceGroupTM1*.
2. En la página **Introducción**, haga clic en **Conectar** y luego, en **Conectarse a una máquina virtual**, seleccione **Descargar archivo RDP**. 
3. Abra el archivo .rdp descargado. Cuando se le pida, seleccione **Conectar**. Escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual. Puede que deba seleccionar **More choices** (Más opciones) y, luego, **Use a different account** (Usar una cuenta diferente), para especificar las credenciales que escribió al crear la máquina virtual. 
4. Seleccione **Aceptar**.
5. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe la advertencia, seleccione **Sí** o **Continuar** para continuar con la conexión.
6. En el escritorio del servidor, vaya a **Herramientas administrativas de Windows**>**Administrador del servidor** .
7. Inicie Windows PowerShell en VM1 y use los siguientes comandos para instalar el servidor IIS y actualizar el archivo HTM predeterminado.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![Instalación de IIS y personalización de la página web](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)

8. Cierre la conexión de RDP con *myIISVMEastUS*.
9. Repita los pasos 1-8 mediante la creación de una conexión RDP con la máquina virtual *myIISVMWEurope* dentro del grupo de recursos *myResourceGroupTM2* para instalar IIS y personalizar su página web predeterminada.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Configuración de nombres DNS para las máquinas virtuales que ejecutan IIS

Traffic Manager enruta el tráfico de usuario basándose en el nombre DNS de los puntos de conexión de servicio. En esta sección, configurará los nombres DNS para los servidores IIS (*myIISVMEastUS* y *myIISVMWEurope*).

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, en la lista de recursos, seleccione *myIISVMEastUS*, que se encuentra en el grupo de recursos *myResourceGroupTM1*.
2. En la página **Introducción**, en **Nombre DNS**, seleccione **Configurar**.
3. En la página **Configuración**, en la etiqueta de nombre DNS, agregue un nombre único y luego seleccione **Guardar**.
4. Repita los pasos 1-3, para la máquina virtual denominada *myIISVMWEurope* que se encuentra en el grupo de recursos *myResourceGroupTM1*.

### <a name="create-a-test-vm"></a>Creación de una máquina virtual de prueba

En esta sección, creará una máquina virtual: *mVMEastUS*. Usará esta máquina virtual para probar cómo Traffic Manager enruta el tráfico al punto de conexión del sitio web que tiene el valor de peso más alto.

1. En la esquina superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Compute** > **Windows Server 2016 VM** (VM de Windows Server 2016).
2. Escriba o seleccione la siguiente información para **Aspectos básicos**, acepte los valores predeterminados para el resto de la configuración y luego seleccione **Crear**:

    |Configuración|Valor|
    |---|---|
    |NOMBRE|myVMEastUS|
    |Nombre de usuario| Escriba un nombre de usuario de su elección.|
    |Password| Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupos de recursos| Seleccione **Existente** y, a continuación, seleccione *myResourceGroupTM1*.|
    |||

4. Seleccione un tamaño de máquina virtual en **Elegir un tamaño**.
5. Seleccione los valores siguientes en **Configuración** y, después, seleccione **Aceptar**:
    |Configuración|Valor|
    |---|---|
    |Virtual network| Seleccione **Red virtual**, en **Crear red virtual**, para **Nombre**, escriba *myVNet3*, para cada subred, escriba *mySubnet*.|
    |Grupo de seguridad de red (NSG)|Seleccione **Básico** y, en la lista desplegable **Seleccionar puertos de entrada públicos**, seleccione **HTTP** y **RDP** |
    |Diagnósticos de arranque|Seleccione **Deshabilitado**.|
    |||

6. En **Crear** de la página **Resumen**, seleccione **Crear** para iniciar la implementación de la máquina virtual.
8. La máquina virtual tarda en crearse unos minutos. No siga con los pasos restantes hasta que se cree la máquina virtual.

## <a name="create-a-traffic-manager-profile"></a>Crear un perfil de Traffic Manager
Cree un perfil de Traffic Manager basado en el método de enrutamiento **Ponderado**.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Perfil de Traffic Manager** > **Crear**.
2. En **Crear perfil de Traffic Manager**, escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y, a continuación, seleccione **Crear**:

    | Configuración                 | Valor                                              |
    | ---                     | ---                                                |
    | NOMBRE                   | Este nombre debe ser único en la zona trafficmanager.net y generará el nombre DNS, trafficmanager.net, que se usa para acceder al perfil de Traffic Manager.                                   |
    | Método de enrutamiento          | Seleccione el método de enrutamiento **Ponderado**.                                       |
    | Subscription            | Seleccione su suscripción.                          |
    | Grupos de recursos          | Seleccione **Existente** y, luego, *myResourceGroupTM1*. |
    |        |   |

    ![Crear un perfil de Traffic Manager](./media/tutorial-traffic-manager-weighted-endpoint-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Incorporación de puntos de conexión de Traffic Manager

Agregue las dos máquinas virtuales que ejecutan los servidores IIS (*myIISVMEastUS*  & *myIISVMWEurope*) para enrutar a ellas el tráfico de usuario.

1. En la barra de búsqueda del portal, busque el nombre del perfil de Traffic Manager que creó en la sección anterior y seleccione el perfil en los resultados que aparecen.
2. En **perfil de Traffic Manager**, en la sección **Configuración**, haga clic en **Puntos de conexión** y, a continuación, haga clic en **Agregar**.
3. Escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y luego seleccione **Aceptar**:

    | Configuración                 | Valor                                              |
    | ---                     | ---                                                |
    | Escriba                    | Punto de conexión de Azure                                   |
    | NOMBRE           | myEastUSEndpoint                                        |
    | Tipo de recurso de destino           | Dirección IP pública                          |
    | Recurso de destino          | **Elija una dirección IP pública** para mostrar la lista de recursos con direcciones IP públicas en la misma suscripción. En **Recurso**, seleccione la dirección IP pública denominada *myIISVMEastUS-ip*. Se trata de la dirección IP pública de la máquina virtual del servidor IIS en la región Este de EE. UU.|
    |  Peso      | Escriba **100**.        |
    |        |           |

4. Repita los pasos 2 y 3 para agregar otro punto de conexión denominado *myWestEuropeEndpoint* para la dirección IP pública *myIISVMWEurope-ip* que está asociada la máquina virtual del servidor IIS denominada *myIISVMWEurope* y, en **Peso**, escriba **25**. 
5.  Cuando termine de agregar ambos puntos de conexión, aparecerán en **Perfil de Traffic Manager** junto con el estado de supervisión como **En línea**.

## <a name="test-traffic-manager-profile"></a>Prueba del perfil de Traffic Manager
Para ver a Traffic Manager en acción, complete los pasos siguientes:
1. Determine el nombre DNS del perfil de Traffic Manager.
2. Vea a Traffic Manager en acción.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Determinación del nombre DNS del perfil de Traffic Manager
En este tutorial, por simplicidad, usará el nombre DNS del perfil de Traffic Manager para visitar los sitios web. 

Puede determinar el nombre DNS del perfil de Traffic Manager de la siguiente manera:

1.  En la barra de búsqueda del portal, busque el nombre del **perfil de Traffic Manager** que creó en la sección anterior. Haga clic en el perfil de Traffic Manager en los resultados que aparezcan.
1. Haga clic en **Descripción general**.
2. La hoja **Perfil de Traffic Manager** muestra el nombre DNS del perfil de Traffic Manager que acaba de crear. En las implementaciones de producción, puede configurar un nombre de dominio mnemónico para que apunte al nombre de dominio de Traffic Manager, mediante un registro CNAME de DNS.

   ![Nombre DNS de Traffic Manager](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Ver a Traffic Manager en acción
En esta sección, puede ver a Traffic Manager en acción. 

1. Seleccione **Todos los recursos** en el menú de la izquierda y, en la lista de recursos, haga clic en *myVMEastUS*, que se encuentra en el grupo de recursos *myResourceGroupTM1*.
2. En la página **Introducción**, haga clic en **Conectar** y luego, en **Conectarse a una máquina virtual**, seleccione **Descargar archivo RDP**. 
3. Abra el archivo .rdp descargado. Cuando se le pida, seleccione **Conectar**. Escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual. Puede que deba seleccionar **More choices** (Más opciones) y, luego, **Use a different account** (Usar una cuenta diferente), para especificar las credenciales que escribió al crear la máquina virtual. 
4. Seleccione **Aceptar**.
5. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe la advertencia, seleccione **Sí** o **Continuar** para continuar con la conexión. 
6. En un explorador web, escriba la máquina virtual *myVMEastUS*, escriba el nombre DNS del perfil de Traffic Manager para ver el sitio web. Se le dirigirá al sitio web hospedado en el servidor IIS *myIISVMEastUS* puesto que se le ha asignado un peso más alto de **100** en comparación con el servidor IIS *myIISVMWEurope* (al que se le ha asignado un valor de peso de punto de conexión más bajo **25**).

   ![Prueba del perfil de Traffic Manager](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)
   
## <a name="delete-the-traffic-manager-profile"></a>Eliminar el perfil de Traffic Manager
Cuando ya no los necesite, elimine los grupos de recursos que ha creado en este tutorial. Para ello, seleccione el grupo de recursos (**ResourceGroupTM1** o **ResourceGroupTM2**) y luego seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Enrutamiento del tráfico a puntos de conexión específicos según la ubicación geográfica del usuario](traffic-manager-configure-geographic-routing-method.md)


