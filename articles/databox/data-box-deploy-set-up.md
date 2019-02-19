---
title: Configuración de Azure Data Box | Microsoft Docs
description: Aprenda a cablear y conectar Azure Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 02/08/2019
ms.author: alkohli
ms.openlocfilehash: e7c58978c7d6542887854c5a1996b15252fdb025
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55982410"
---
# <a name="tutorial-cable-and-connect-to-your-azure-data-box"></a>Tutorial: Cableado y conexión de un dispositivo Azure Data Box

En este tutorial se describe cómo cablear, conectar y encender un dispositivo Azure Data Box.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Cableado del dispositivo Data Box
> * Conexión al dispositivo Data Box

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que:

1. Ha completado el [Tutorial: Realización de pedidos de Azure Data Box](data-box-deploy-ordered.md).
2. Ha recibido su dispositivo Data Box y el estado del pedido en el portal se actualiza a **Delivered** (Entregado). 
    - Hay una etiqueta de envío en el bolsillo transparente adherida al dispositivo bajo la etiqueta actual. Manténgala en un lugar seguro, ya que la usará para el envío de devolución.
    - Algunas regiones de Europa pueden recibir el dispositivo empaquetado en una caja. Asegúrese de desempaquetar el dispositivo y guarde la caja para posibles devoluciones.
3. Ha revisado la [directrices de seguridad de Data Box](data-box-safety.md).
4. Ha recibido un cable con toma de tierra para usar con el dispositivo de almacenamiento de 100 TB.
5. Tiene un equipo host con los datos que desea copiar en su dispositivo Data Box. El equipo host debe:
    - Ejecutar un [sistema operativo admitido](data-box-system-requirements.md).
    - Estar conectado a una red de alta velocidad. Es muy recomendable tener una conexión de 10 GbE como mínimo. Si no hay disponible una conexión de 10 GbE, se puede usar un vínculo de datos de 1 GbE, pero las velocidades de copia resultarán afectadas. 
6. Debe tener acceso a una superficie plana en la que puede colocar su dispositivo Data Box. Si quiere colocar el dispositivo en un bastidor estándar, necesitará una ranura de 7U en el bastidor del centro de datos. Puede colocar el dispositivo en posición horizontal o vertical en el bastidor.
7. Cuenta con los cables siguientes para conectar su dispositivo Data Box al equipo host.
    - Uno o varios cables de fibra óptica SFP+ o de cobre 10 GbE SFP+ Twinax (se usan con las interfaces de red DATA 1, DATA 2). Data Box cuenta con los adaptadores Mellanox ConnectX®-3 Pro EN de puerto dual 10GBASE-T con interfaz de red PCI Express 3.0, por lo que deben funcionar los cables que son compatibles con esta interfaz. Por ejemplo, se ha utilizado un cable CISCO SFP-H10GB-CU3M 10GBASE-CU TWINMAX SFP +3M para pruebas internas.
    - Un cable de red RJ-45 CAT 6 (se usa con la interfaz de red MGMT)
    - Un cable de red RJ-45 CAT 6A o RJ-45 CAT 6 (se usa con la interfaz de red DATA 3 configurada como 10 Gbps o 1 Gbps, respectivamente)

## <a name="cable-your-device"></a>Cableado del dispositivo

Realice los pasos siguientes para cablear el dispositivo.

1. Inspeccione el dispositivo para comprobar si hay alguna evidencia de que haya sido alterado o de que haya sufrido algún otro daño evidente. Si el dispositivo está alterado o presenta daños graves, no continúe. Póngase en contacto inmediatamente con el Soporte técnico de Microsoft para que le ayude a evaluar si el dispositivo está en buenas condiciones y si debe devolverlo para que se lo cambien.
2. Transporte el dispositivo a la ubicación donde desea encenderlo. Coloque el dispositivo en una superficie plana. También se puede colocar el dispositivo en un bastidor estándar.
3. Conecte los cables de alimentación y red. A continuación se muestra el panel posterior de un dispositivo conectado con una configuración común. Dependiendo de su entorno, puede escoger otras [opciones de cableado](data-box-cable-options.md).
    
    ![Panel posterior del dispositivo Data Box con los cables](media/data-box-deploy-set-up/data-box-cabled-dhcp.png)

    1. Conecte el cable de alimentación a la entrada de alimentación etiquetada. El otro extremo del cable de alimentación debe conectarse a una unidad de distribución de energía.
    2. Utilice el cable RJ-45 CAT 6 para conectar el puerto MGMT en un extremo y un equipo portátil en el otro extremo.            
    3. Utilice el cable RJ-45 CAT 6A para conectarse al puerto DATA 3 en un extremo. DATA 3 se configura como 10 GbE si se conecta mediante el cable RJ-45 CAT 6A y como 1 GbE si se conecta mediante el cable RJ-45 CAT 6.
    4. En función de las interfaces de red que desee conectar para la transferencia de datos, utilice hasta dos cables de cobre 10 GbE SFP + Twinax y fibra óptica SFP+ para conectar los puertos DATA 1 y DATA 2, respectivamente. 
    5. El otro extremo de los cables de los puertos de datos se conectan al equipo host mediante un conmutador de 10 GbE.

4. Busque el botón de encendido en el panel frontal del dispositivo. Encienda el dispositivo.

    ![Botón de encendido del dispositivo Data Box](media/data-box-deploy-set-up/data-box-powered-door-open.png)

## <a name="connect-to-your-device"></a>Conexión al dispositivo

Realice los pasos siguientes para configurar el dispositivo mediante la interfaz de usuario web local y la interfaz de usuario del portal.

1. Configure el adaptador Ethernet en portátil que usa para conectarse al dispositivo con la dirección IP estática 192.168.100.5 y la subred 255.255.255.0. 
2. Conéctese al puerto MGMT del dispositivo y acceda a la interfaz de usuario web local en https\://192.168.100.10. Esto puede tardar hasta 5 minutos tras encender el dispositivo.
3. Haga clic en **Details** (Detalles) y haga clic en **Go on to the webpage** (Continuar a la página web).

   ![Conexión a la interfaz de usuario web local](media/data-box-deploy-set-up/data-box-connect-local-web-ui.png) 

4. Verá la página de **inicio de sesión** de la interfaz de usuario web local. Asegúrese de que el número de serie del dispositivo coincide tanto en la interfaz de usuario del portal como en la interfaz de usuario web local. En este momento, el dispositivo está bloqueado.
5. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
6. Descargue las credenciales del dispositivo del portal. Vaya a **General > Device details** (General > Detalles del dispositivo). Copie el valor de **Device password** (Contraseña del dispositivo). La contraseña del dispositivo está asociada a un determinado orden en el portal. 

    ![Obtención de las credenciales del dispositivo](media/data-box-deploy-set-up/data-box-device-credentials.png)
    
    
7. Proporcione la contraseña del dispositivo que obtuvo en Azure Portal en el paso anterior para iniciar sesión en la interfaz de usuario web local del dispositivo. Haga clic en **Iniciar sesión**.
8. En el **Panel**, asegúrese de que las interfaces de red están configuradas. 
    - Si DHCP está habilitado en el entorno, las interfaces de red se configuran automáticamente. 
    - Si DHCP no está habilitado, vaya a **Set network interfaces** (Establecer interfaces de red) y asigne direcciones IP estáticas si es necesario.

    ![Panel del dispositivo](media/data-box-deploy-set-up/data-box-dashboard-1.png)

Una vez configuradas las interfaces de red de datos, también puede utilizar la dirección IP de cualquiera de las interfaces DATA 1 - DATA 3 para acceder a la interfaz de usuario web local en `https://<IP address of a data network interface>`. 

Después de completada la configuración del dispositivo, puede conectarse a los recursos compartidos del dispositivo y copiar los datos desde el equipo en el dispositivo. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Data Box, como:

> [!div class="checklist"]
> * Cableado del dispositivo Data Box
> * Conexión al dispositivo Data Box

Pase al siguiente tutorial para aprender a copiar datos en su dispositivo Data Box.

> [!div class="nextstepaction"]
> [Copia de datos a un dispositivo Azure Data Box](./data-box-deploy-copy-data.md)

