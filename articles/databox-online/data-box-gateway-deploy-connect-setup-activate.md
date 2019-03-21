---
title: Conexión, configuración y activación de Azure Data Box Gateway en Azure Portal | Microsoft Docs
description: Este tercer tutorial para implementar Data Box Gateway proporciona instrucciones para conectar, configurar y activar el dispositivo virtual.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 26b560434e6305689781b8c39c7cf814af9bf8aa
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58112303"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-gateway-preview"></a>Tutorial: Conexión, configuración y activación de Azure Data Box Gateway (versión preliminar) 

## <a name="introduction"></a>Introducción

Este tutorial describe cómo conectar, configurar y activar el dispositivo Data Box Gateway mediante la interfaz de usuario web local. 

El proceso de instalación y activación puede tardar unos 10 minutos en completarse. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Realizar la conexión con el dispositivo virtual
> * Configurar y activar el dispositivo virtual

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.


> [!IMPORTANT]
> - Data Box Gateway está en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 


## <a name="prerequisites"></a>Requisitos previos

Antes de instalar y configurar Data Box Gateway, asegúrese de que:

* Ha aprovisionado un dispositivo virtual y ha obtenido una dirección URL conectada al mismo según se detalla en [Aprovisionamiento de Data Box Gateway en Hyper-V](data-box-gateway-deploy-provision-hyperv.md) o en [Aprovisionamiento de Data Box Gateway en VMware](data-box-gateway-deploy-provision-vmware.md).
* Tiene la clave de activación del servicio Data Box Gateway que creó para administrar los dispositivos Data Box Gateway. Para más información, consulte [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md).


## <a name="connect-to-the-local-web-ui-setup"></a>Conexión a la configuración de la interfaz de usuario web local 

1. Abra una ventana del explorador y conéctese a la interfaz de usuario web local. Escriba: 
   
   [https://ip-address-of-network-interface](https://ip-address-of-network-interface)
   
   Use la dirección URL de conexión que anotó en el tutorial anterior. Ve un error que indica que hay un problema con el certificado de seguridad del sitio web. Haga clic en **Continue to this webpage**(Continuar a esta página web). (Estos pasos pueden ser diferentes en función del explorador utilizado).
   
    ![Error durante la conexión](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

2. Inicie sesión en la interfaz de usuario web del dispositivo virtual. La contraseña predeterminada es *Password1*. 
   
    ![Inicie sesión en la interfaz de usuario de web local](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

3. Se le solicitará cambiar la contraseña del administrador de dispositivos. Escriba una nueva contraseña que tenga entre 8 y 16 caracteres. La contraseña debe contener 3 de los siguientes elementos: caracteres en mayúsculas, minúsculas, números y caracteres especiales.

    ![Cambiar contraseña de administrador de dispositivos](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

Ya está en el **Panel** del dispositivo.

## <a name="set-up-and-activate-the-virtual-device"></a>Configuración y activación del dispositivo virtual
 
1. Desde el panel puede ir a las distintas opciones de configuración necesarias para configurar y registrar el dispositivo virtual en el servicio Data Box Gateway. Las opciones **Configuración de red**, **Configuración de proxy web** y **Configuración horaria** son opcionales. Los únicos valores obligatorios son **Nombre del dispositivo** y **Configuración de la nube**.
   
    !["Panel" de la IU web local página](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

2. En la página **Nombre de dispositivo**, configure un nombre descriptivo para el dispositivo. El nombre descriptivo puede tener de 1 a 15 caracteres y puede contener letras, números y guiones.

    ![Página de "Nombre del dispositivo" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

3. (Opcionalmente) establezca la **Configuración de red**. Verá al menos 1 interfaz de red o más, en función de cuántas se configuraron en la máquina virtual subyacente. La página **Configuración de red** de un dispositivo virtual con una interfaz de red habilitada es como la que se muestra a continuación.
    
    ![Página de "Configuración de red" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Al establecer la configuración de red, tenga en cuenta lo siguiente:

   - Si DHCP está habilitado en el entorno, las interfaces de red se configuran automáticamente. Por lo tanto, se asignan automáticamente dirección IP, subred, puerta de enlace y DNS.
   - Si DHCP no está habilitado, puede asignar direcciones IP estáticas si es necesario.
   - Puede configurar la interfaz de red como IPv4.

     >[!NOTE] 
     > Se recomienda no cambiar la dirección IP local de la interfaz de red de estática a DHCP, a menos que se tenga otra dirección IP para conectarse al dispositivo. Si se usara una interfaz de red y cambiara a DHCP, no habría forma de determinar la dirección DHCP. Si desea cambiar a una dirección DHCP, espere a que el dispositivo se haya registrado con el servicio y, después, realice el cambio. Luego, puede ver las direcciones IP de todos los adaptadores de las **propiedades del dispositivo** de su servicio en Azure Portal.

4. Configure el servidor proxy web (de manera opcional). Aunque la configuración del proxy web es opcional, tenga en cuenta que, si usa un proxy web, solo puede configurarlo aquí.
   
   ![Página de "Configuración del proxy Web" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   En la página **Proxy web** :
   
   1. Proporcione la **URL de proxy web** en este formato: *http://&lt;dirección IP o FQDN de host&gt;:Número de puerto*. Tenga en cuenta que no se admiten direcciones URL HTTPS.
   2. Especifique **Autenticación** como **Básica** o **Ninguna**.
   3. Si utiliza autenticación, también debe proporcionar un **Nombre de usuario** y una **Contraseña**.
   4. Haga clic en **Aplicar**. Esto validará y aplicará los parámetros de proxy web configurados.

5. (Opcionalmente) configure las opciones de hora para el dispositivo, como la zona horaria y los servidores NTP principal y secundario. Se requieren servidores NTP, ya que el dispositivo debe sincronizar la hora para que pueda autenticarse con los proveedores de servicios en la nube.
    
    ![Página de "Configuración de tiempo" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    En la página **Time settings** (Configuración de hora):
    
    1. En la lista desplegable, seleccione la **zona horaria** según la ubicación geográfica en la que se va a implementar el dispositivo. La zona horaria predeterminada para el dispositivo es la hora del Pacífico (PST). El dispositivo usará esta zona horaria para todas las operaciones programadas.
    2. Especifique un **servidor NTP principal** para el dispositivo o acepte el valor predeterminado de time.windows.com. Asegúrese de que su red permite que el tráfico NTP pase del centro de datos a Internet.
    3. Opcionalmente, especifique un **servidor NTP secundario** para el dispositivo.
    4. Haga clic en **Apply**. Esto validará y aplicará los parámetros de hora configurados.

6. En la página **Configuración de nube**, active el dispositivo con el servicio Data Box Gateway en Azure Portal.
    
    1. Escriba la **Clave de activación** que obtuvo en [Obtención de la clave de activación](data-box-gateway-deploy-prep.md#get-the-activation-key) de Data Box Gateway.

    2. Haga clic en **Activar**. 
       
         ![Página de "Configuración de la nube" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image10a.png)
    
    3. En primer lugar, se activará el dispositivo. El dispositivo, a continuación, se analiza para todas las actualizaciones críticas y si está disponible, las actualizaciones se aplican automáticamente. Verá una notificación al respecto. 

        El cuadro de diálogo también tiene una clave de recuperación que debe copiar y guardarla en una ubicación segura. Esta clave se usa para recuperar los datos en caso de que el dispositivo no puede arrancar.

        ![Página de "Configuración de la nube" de la interfaz de usuario web local](./media/data-box-gateway-deploy-connect-setup-activate/image12.png)    

    4. Es posible que deba esperar varios minutos después de la actualización se ha completado correctamente. La página se actualiza para indicar que el dispositivo se ha activado correctamente.

        ![Página de "Configuración de la nube" de la interfaz de usuario web local actualizado](./media/data-box-gateway-deploy-connect-setup-activate/image13.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Gateway, como:

> [!div class="checklist"]
> * Realizar la conexión con el dispositivo virtual
> * Configurar y activar el dispositivo virtual


Pase al siguiente tutorial para aprender cómo transferir datos con Data Box Gateway.

> [!div class="nextstepaction"]
> [Transferencia de datos con Data Box Gateway](./data-box-gateway-deploy-add-shares.md).
