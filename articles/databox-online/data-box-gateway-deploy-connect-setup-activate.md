---
title: Conexión, configuración y activación de Azure Data Box Gateway en Azure Portal | Microsoft Docs
description: Este tercer tutorial para implementar Data Box Gateway proporciona instrucciones para conectar, configurar y activar el dispositivo virtual.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/18/2019
ms.author: alkohli
ms.openlocfilehash: de2ef4908260a62acf28a270dda6ad738a1760b9
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402344"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-gateway"></a>Tutorial: Conectarse, configurar, activar la puerta de enlace del cuadro de datos de Azure

## <a name="introduction"></a>Introducción

Este tutorial describe cómo conectarse a, configurar y activar el dispositivo de puerta de enlace de datos cuadro mediante el uso de la interfaz de usuario web local. 

El proceso de instalación y activación puede tardar unos 10 minutos en completarse. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conectarse a un dispositivo virtual
> * Configuración y activación del dispositivo virtual

## <a name="prerequisites"></a>Requisitos previos

Antes de instalar y configurar Data Box Gateway, asegúrese de que:

* Ha aprovisionado un dispositivo virtual y ha obtenido una dirección URL de conectados a él como se detalla en el [aprovisionar una puerta de enlace de datos de cuadro en Hyper-V](data-box-gateway-deploy-provision-hyperv.md) o [aprovisionar una puerta de enlace de datos de cuadro en VMware](data-box-gateway-deploy-provision-vmware.md).
* Tiene la clave de activación del servicio Data Box Gateway que creó para administrar los dispositivos Data Box Gateway. Para más información, consulte [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md).


## <a name="connect-to-the-local-web-ui-setup"></a>Conexión a la configuración de la interfaz de usuario web local 

1. Abra una ventana del explorador y el acceso local del dispositivo en la interfaz de usuario de web:
   
   [https://ip-address-of-network-interface](https://ip-address-of-network-interface)
   
   Use la dirección URL de conexión que anotó en el tutorial anterior. Ve un error o advertencia que indica que hay un problema con el certificado de seguridad del sitio web.

2. Seleccione **Continue to this webpage**(Continuar a esta página web). Estos pasos pueden variar en función del explorador que se use.
   
    ![Mensaje de error del certificado de seguridad del sitio web](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

3. Inicie sesión en la interfaz de usuario web del dispositivo virtual. La contraseña predeterminada es *Password1*. 
   
    ![Inicie sesión en la interfaz de usuario de web local](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

4. En el símbolo del sistema, cambie la contraseña del dispositivo. La nueva contraseña debe contener entre 8 y 16 caracteres. Debe contener 3 de los siguientes: caracteres en mayúsculas, minúsculas, numéricos y especiales.

    ![Cambiar la contraseña del dispositivo](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

Ahora está en el **Panel** del dispositivo.

## <a name="set-up-and-activate-the-virtual-device"></a>Configuración y activación del dispositivo virtual
 
El panel muestra las distintas configuraciones que son necesarios para configurar y registrar el dispositivo virtual con el servicio de puerta de enlace de datos cuadro. Las opciones **Nombre de dispositivo**, **Configuración de red**, **Configuración de proxy web** y **Configuración horaria** son opcionales. Los únicos valores obligatorios son **Configuración de la nube**.
   
!["Panel" de la IU web local página](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

1. En el panel izquierdo, seleccione **nombre de dispositivo**y, a continuación, escriba un nombre descriptivo para el dispositivo. El nombre descriptivo debe contener entre 1 y 15 caracteres y tener letras, números y guiones.

    ![Página de "Nombre del dispositivo" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

2. (Opcional) En el panel izquierdo, seleccione **configuración de red** y, a continuación, configure las opciones. En el dispositivo virtual, verá al menos una interfaz de red y más, en función de cuántos que configuró en la máquina virtual subyacente. La página **Configuración de red** de un dispositivo virtual con una interfaz de red habilitada es como la que se muestra a continuación.
    
    ![Página de "Configuración de red" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Configurar opciones de red, tenga en cuenta:

    - Si DHCP está habilitado en el entorno, las interfaces de red se configuran automáticamente. Se asignan automáticamente una dirección IP, la subred, la puerta de enlace y DNS.
    - Si DHCP no está habilitado, puede asignar direcciones IP estáticas si es necesario.
    - Puede configurar la interfaz de red como IPv4.

     >[!NOTE] 
     > Se recomienda no cambiar la dirección IP local de la interfaz de red de estática a DHCP, a menos que se tenga otra dirección IP para conectarse al dispositivo. Si se usara una interfaz de red y cambiara a DHCP, no habría forma de determinar la dirección DHCP. Si desea cambiar a una dirección DHCP, espere a que el dispositivo se haya registrado con el servicio y, después, realice el cambio. Después, puede ver las direcciones IP de todos los adaptadores de las **propiedades del dispositivo** de su servicio en Azure Portal.

3. (Opcional) Configure el servidor proxy web. Aunque la configuración del proxy web es opcional, si usa a un proxy web, puede configurar solo en esta página.
   
   ![Página de "Configuración del proxy Web" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   En el **proxy Web** página, realice lo siguiente:
   
   1. En el cuadro **URL de proxy web** , escriba la dirección URL en este formato: `http://&lt;host-IP address or FQDN&gt;:Port number`. No se admiten direcciones URL HTTPS.
   2. En **Autenticación**, seleccione **No** o **NTLM**.
   3. Si utiliza la autenticación, escriba un **Username** y **contraseña**.
   4. Para validar y aplicar la configuración de proxy web, seleccione **aplicar**.

4. (Opcional) En el panel izquierdo, seleccione **Configuración horaria** y, después, configure la zona horaria y los servidores NTP principal y secundario del dispositivo. 

    Se requieren servidores NTP, ya que el dispositivo debe sincronizar la hora para que pueda autenticarse con los proveedores de servicios en la nube.
    
    ![Página de "Configuración de tiempo" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    En el **configuración horaria** página, realice lo siguiente:
    
    1. En el **zona horaria** lista desplegable, seleccione la zona horaria que corresponde a la ubicación geográfica en la que se va a implementar el dispositivo.
        La zona horaria predeterminada para el dispositivo es la hora del Pacífico (PST). El dispositivo usará esta zona horaria para todas las operaciones programadas.

    2. Especifique un **servidor NTP principal** para el dispositivo o acepte el valor predeterminado de `time.windows.com`.   
        Asegúrese de que su red permite que el tráfico NTP pase del centro de datos a Internet.

    3. Si lo desea, en el **servidor NTP secundario** , escriba un servidor secundario para el dispositivo.

    4. Seleccione **Aplicar** para validar y aplicar la configuración horaria.

6. En el panel izquierdo, seleccione **configuración de nube**y, a continuación, activar el dispositivo con el servicio de puerta de enlace de datos cuadro en Azure portal.
    
    1. En el **clave de activación** , escriba el **clave de activación** que obtuvo en [obtener la clave de activación](data-box-gateway-deploy-prep.md#get-the-activation-key) para puerta de enlace de datos de cuadro.

    2. Seleccione **Activar**.
       
         ![Página de "Configuración de la nube" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image10a.png)
    
    3. El dispositivo está activado y las actualizaciones críticas, si está disponible, se aplican automáticamente. Verá una notificación al respecto. Supervisar el progreso de actualización a través del portal de Azure.

        ![Página de "Configuración de la nube" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image12.png)
        
        **El cuadro de diálogo también tiene una clave de recuperación que se debe copiar y guardar en una ubicación segura. Esta clave se usa para recuperar los datos en caso de que el dispositivo no puede arrancar.**


    4. Es posible que deba esperar varios minutos completar correctamente la actualización. Después de la actualización está completa, inicie sesión en el dispositivo. El **configuración de nube** página actualiza para indicar que el dispositivo se ha activado correctamente.

        ![Página de "Configuración de la nube" de la interfaz de usuario web local actualizado](./media/data-box-gateway-deploy-connect-setup-activate/image13.png)

La instalación del dispositivo está completa. Ahora puede agregar recursos compartidos en el dispositivo.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Conectarse a un dispositivo virtual
> * Configuración y activación del dispositivo virtual

Para obtener información sobre cómo transferir datos con la puerta de enlace de datos de cuadro, vea:

> [!div class="nextstepaction"]
> [Transferencia de datos con Data Box Gateway](./data-box-gateway-deploy-add-shares.md).
