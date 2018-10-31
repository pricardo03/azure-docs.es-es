---
title: Conexión, configuración y activación de Azure Data Box Edge en Azure Portal | Microsoft Docs
description: Este tercer tutorial para implementar Data Box Edge proporciona instrucciones para conectar, configurar y activar el dispositivo físico.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 3d6742bec54e612b5dca6d9ef6c4f67a33929448
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166583"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-edge-preview"></a>Tutorial: Conexión, configuración y activación de Azure Data Box Edge (versión preliminar) 

Este tutorial describe cómo conectar, configurar y activar el dispositivo Data Box Edge mediante la interfaz de usuario web local. 

El proceso de instalación y activación puede tardar unos 20 minutos en completarse. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conectarse a un dispositivo físico
> * Configurar y activar el dispositivo virtual

> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 


## <a name="prerequisites"></a>Requisitos previos

Antes de configurar e instalar Data Box Edge, asegúrese de que:

* Ha instalado el dispositivo físico tal como se detalla en [Instalación de Data Box Edge](data-box-edge-deploy-install.md).
* Tiene la clave de activación del servicio Data Box Edge que creó para administrar los dispositivos Data Box Edge. Para más información, consulte [Preparación de la implementación de Azure Data Box Edge](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Conexión a la configuración de la interfaz de usuario web local 

1. Configure el adaptador Ethernet en el equipo que usa para conectarse al dispositivo Edge con la dirección IP estática 192.168.100.5 y la subred 255.255.255.0.
2. Conecte el equipo a PUERTO 1 en el dispositivo. 
3. Abra una ventana del explorador y acceda a la interfaz de usuario web local del dispositivo en https://192.168.100.10. Esta acción puede tardar unos minutos después de activar el dispositivo. 
4. Ve un error o advertencia que indica que hay un problema con el certificado de seguridad del sitio web. Haga clic en **Continue to this webpage**(Continuar a esta página web). (Estos pasos pueden ser diferentes en función del explorador utilizado).
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

2. Inicie sesión en la interfaz de usuario web del dispositivo. La contraseña predeterminada es *Password1*. 
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

3. Se le solicitará cambiar la contraseña del administrador de dispositivos. Escriba una nueva contraseña que tenga entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales.

Ahora está en el **Panel** del dispositivo.

## <a name="set-up-and-activate-the-physical-device"></a>Configuración y activación del dispositivo físico
 
1. Desde el panel, puede ir a las distintas opciones de configuración necesarias para configurar y registrar el dispositivo físico en el servicio Data Box Edge. Las opciones **Nombre de dispositivo**, **Configuración de red**, **Configuración de proxy web** y **Configuración horaria** son opcionales. Los únicos valores obligatorios son **Configuración de la nube**.
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

2. En la página **Nombre de dispositivo**, configure un nombre descriptivo para el dispositivo. El nombre descriptivo puede tener de 1 a 15 caracteres y contener letras, números y guiones.

    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

3. (Opcionalmente) establezca la **Configuración de red**. En el dispositivo físico, verá seis interfaces de red. PUERTO 1 y PUERTO 2 son interfaces de red de 1 Gbps. PUERTO 3, PUERTO 4, PUERTO 5 y PUERTO 6 son todas las interfaces de red de 25 Gbps. PUERTO 1 se configura automáticamente como puerto solo de administración, mientras que PUERTO 2 a PUERTO 6 son todos los puertos de datos. La página **Configuración de red** tal como se muestra a continuación.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Al establecer la configuración de red, tenga en cuenta lo siguiente:

    - Si DHCP está habilitado en el entorno, las interfaces de red se configuran automáticamente. Se asignan automáticamente una dirección IP, la subred, la puerta de enlace y DNS.
    - Si DHCP no está habilitado, puede asignar direcciones IP estáticas si es necesario.
    - Puede configurar la interfaz de red como IPv4.
   
4. Configure el servidor proxy web (de manera opcional). Aunque la configuración del proxy web es opcional, si usa un proxy web, solo puede configurarlo aquí.
   
   ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   En la página **Proxy web** :
   
   1. Proporcione la **dirección URL del proxy web** en este formato: `http://host-IP address or FDQN:Port number`. No se admiten direcciones URL HTTPS.
   2. Especifique **Autenticación** como **Básica** o **Ninguna**.
   3. Si utiliza autenticación, también debe proporcionar un **Nombre de usuario** y una **Contraseña**.
   4. Haga clic en **Aplicar** para validar y aplicar los parámetros de proxy web configurados.

5. (Opcionalmente) configure las opciones de hora para el dispositivo, como la zona horaria y los servidores NTP principal y secundario. Se requieren servidores NTP, ya que el dispositivo debe sincronizar la hora para que pueda autenticarse con los proveedores de servicios en la nube.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)
    
    En la página **Time settings** (Configuración de hora):
    
    1. En la lista desplegable, seleccione la **zona horaria** según la ubicación geográfica en la que se va a implementar el dispositivo. La zona horaria predeterminada para el dispositivo es la hora del Pacífico (PST). El dispositivo usará esta zona horaria para todas las operaciones programadas.
    2. Especifique un **servidor NTP principal** para el dispositivo o acepte el valor predeterminado de time.windows.com. Asegúrese de que su red permite que el tráfico NTP pase del centro de datos a Internet.
    3. Opcionalmente, especifique un **servidor NTP secundario** para el dispositivo.
    4. Haga clic en **Aplicar** para validar y aplicar los parámetros de tiempo configurados.

6. En la página **Configuración de nube**, active el dispositivo con el servicio Data Box Edge en Azure Portal.
    
    1. Escriba la **Clave de activación** que obtuvo en [Obtención de la clave de activación](data-box-edge-deploy-prep.md#get-the-activation-key) de Data Box Edge.

    2. Haga clic en **Aplicar**. 
       
         ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)
    
    3. Una vez que el dispositivo se ha activado correctamente, se le presentan las opciones del modo de conectividad. Estas opciones se configuran si necesita trabajar con el dispositivo en modo sin conexión o parcialmente sin conexión. 

        ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)    

La instalación del dispositivo está completa. Ahora puede agregar recursos compartidos en el dispositivo.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Edge, como:

> [!div class="checklist"]
> * Conectarse a un dispositivo físico
> * Configurar y activar el dispositivo virtual


Siga con el siguiente tutorial para aprender cómo transferir datos con Data Box Edge.

> [!div class="nextstepaction"]
> [Transferencia de datos con Data Box Edge](./data-box-edge-deploy-add-shares.md).