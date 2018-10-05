---
title: Conexión, configuración y activación de Azure Data Box Gateway en Azure Portal | Microsoft Docs
description: Este tercer tutorial para implementar Data Box Gateway proporciona instrucciones para conectar, configurar y activar el dispositivo virtual.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/27/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: 77ccc6ebbb48ab3d7ce22d6c593bd62aa8dec3ac
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419495"
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

<!--* If this is the second or subsequent virtual device that you are registering with an existing StorSimple Device Manager service, you should have the service data encryption key. This key was generated when the first device was successfully registered with this service. If you have lost this key, see [Get the service data encryption key](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) for your Data Box Gateway.-->

## <a name="connect-to-the-local-web-ui-setup"></a>Conexión a la configuración de la interfaz de usuario web local 

1. Abra una ventana del explorador y conéctese a la interfaz de usuario web local. Escriba: 
   
   [https://ip-address-of-network-interface](https://ip-address-of-network-interface)
   
   Use la dirección URL de conexión que anotó en el tutorial anterior. Ve un error que indica que hay un problema con el certificado de seguridad del sitio web. Haga clic en **Continue to this webpage**(Continuar a esta página web). (Estos pasos pueden ser diferentes en función del explorador utilizado).
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

2. Inicie sesión en la interfaz de usuario web del dispositivo virtual. La contraseña predeterminada es *Password1*. 
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

3. Se le solicitará cambiar la contraseña del administrador de dispositivos. Escriba una nueva contraseña que tenga entre 8 y 16 caracteres. La contraseña debe contener 3 de los siguientes elementos: caracteres en mayúsculas, minúsculas, números y caracteres especiales.

    ![](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

Ya está en el **Panel** del dispositivo.

## <a name="set-up-and-activate-the-virtual-device"></a>Configuración y activación del dispositivo virtual
 
1. Desde el panel puede ir a las distintas opciones de configuración necesarias para configurar y registrar el dispositivo virtual en el servicio Data Box Gateway. Las opciones **Configuración de red**, **Configuración de proxy web** y **Configuración horaria** son opcionales. Los únicos valores obligatorios son **Nombre del dispositivo** y **Configuración de la nube**.
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

2. En la página **Nombre de dispositivo**, configure un nombre descriptivo para el dispositivo. El nombre descriptivo puede tener de 1 a 15 caracteres y puede contener letras, números y guiones.

    ![](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

3. (Opcionalmente) establezca la **Configuración de red**. Verá al menos 1 interfaz de red o más, en función de cuántas se configuraron en la máquina virtual subyacente. La página **Configuración de red** de un dispositivo virtual con una interfaz de red habilitada es como la que se muestra a continuación.
    
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Al establecer la configuración de red, tenga en cuenta lo siguiente:

    - Si DHCP está habilitado en el entorno, las interfaces de red se configuran automáticamente. Por lo tanto, se asignan automáticamente dirección IP, subred, puerta de enlace y DNS.
    - Si DHCP no está habilitado, puede asignar direcciones IP estáticas si es necesario.
    - Puede configurar la interfaz de red como IPv4.
   
4. Configure el servidor proxy web (de manera opcional). Aunque la configuración del proxy web es opcional, tenga en cuenta que, si usa un proxy web, solo puede configurarlo aquí.
   
   ![](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   En la página **Proxy web** :
   
   1. Proporcione la **URL de proxy web** en este formato:  *http://&lt;dirección o nombre de dominio completo&gt;:Número de puerto*. Tenga en cuenta que no se admiten direcciones URL HTTPS.
   2. Especifique **Autenticación** como **Básica** o **Ninguna**.
   3. Si utiliza autenticación, también debe proporcionar un **Nombre de usuario** y una **Contraseña**.
   4. Haga clic en **Aplicar**. Esto validará y aplicará los parámetros de proxy web configurados.

5. (Opcionalmente) configure las opciones de hora para el dispositivo, como la zona horaria y los servidores NTP principal y secundario. Se requieren servidores NTP, ya que el dispositivo debe sincronizar la hora para que pueda autenticarse con los proveedores de servicios en la nube.
    
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    En la página **Time settings** (Configuración de hora):
    
    1. En la lista desplegable, seleccione la **zona horaria** según la ubicación geográfica en la que se va a implementar el dispositivo. La zona horaria predeterminada para el dispositivo es la hora del Pacífico (PST). El dispositivo usará esta zona horaria para todas las operaciones programadas.
    2. Especifique un **servidor NTP principal** para el dispositivo o acepte el valor predeterminado de time.windows.com. Asegúrese de que su red permite que el tráfico NTP pase del centro de datos a Internet.
    3. Opcionalmente, especifique un **servidor NTP secundario** para el dispositivo.
    4. Haga clic en **Apply**. Esto validará y aplicará los parámetros de hora configurados.

6. En la página **Configuración de nube**, active el dispositivo con el servicio Data Box Gateway en Azure Portal.
    
    1. Escriba la **Clave de activación** que obtuvo en [Obtención de la clave de activación](data-box-gateway-deploy-prep.md#get-the-activation-key) de Data Box Gateway.

    2. Haga clic en **Activar**. 
       
         ![](./media/data-box-gateway-deploy-connect-setup-activate/image10.png)
    
    3. Debe esperar un minuto antes de que el dispositivo se active correctamente. Después de la activación, la página se actualiza para indicar que el dispositivo se ha activado correctamente.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Gateway, como:

> [!div class="checklist"]
> * Realizar la conexión con el dispositivo virtual
> * Configurar y activar el dispositivo virtual


Pase al siguiente tutorial para aprender cómo transferir datos con Data Box Gateway.

> [!div class="nextstepaction"]
> [Transferencia de datos con Data Box Gateway](./data-box-gateway-deploy-add-shares.md).
