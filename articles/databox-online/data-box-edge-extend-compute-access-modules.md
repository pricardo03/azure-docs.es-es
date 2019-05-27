---
title: Administrar procesos de red en el borde del cuadro de datos de Azure a los módulos de acceso | Microsoft Docs
description: Describe cómo extender la red de proceso en el borde del cuadro de datos para tener acceso a los módulos a través de una dirección IP externa.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/17/2019
ms.author: alkohli
ms.openlocfilehash: 907647725dd6795b3b6482476de7442fbbf66114
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65917241"
---
# <a name="enable-compute-network-on-your-azure-data-box-edge"></a>Habilitar el proceso de red en el borde del cuadro de datos de Azure

En este artículo se describe cómo los módulos que se ejecutan en el borde del cuadro de datos de Azure pueden acceder a los procesos de red habilitada en el dispositivo.

Para configurar la red, deberá realizar los pasos siguientes:

- Habilitar una interfaz de red en el dispositivo de borde del cuadro de datos para el proceso
- Agregar un módulo a la red de acceso de proceso en el borde del cuadro de datos
- Compruebe que el módulo puede tener acceso a la interfaz de red habilitada

En este tutorial, usará un módulo de aplicación de servidor Web para demostrar el escenario.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, necesitará:

- Un dispositivo de borde del cuadro de datos con el programa de instalación de dispositivo completada.
- Ha completado **configuración del proceso** paso tal como se indicó el [Tutorial: Transformación de datos con el borde del cuadro de datos de Azure](data-box-edge-deploy-configure-compute-advanced.md#configure-compute) en el dispositivo. El dispositivo debe tener un recurso de IoT Hub asociado, un dispositivo de IoT y un dispositivo IoT Edge.

## <a name="enable-network-interface-for-compute"></a>Habilitar la interfaz de red para el proceso

Para obtener acceso a los módulos que se ejecutan en el dispositivo a través de una red externa, deberá asignar una dirección IP a una interfaz de red en el dispositivo. Esta configuración del proceso se puede administrar desde la interfaz de usuario web local.

Realice los pasos siguientes en la interfaz de usuario web local para configurar el proceso.

1. En la interfaz de usuario web local, vaya a **Configuración > Configuración del proceso**.  

2. **Habilitar** la interfaz de red que desea usar para conectarse a un módulo de proceso que se va a ejecutar en el dispositivo.

    - Si usa direcciones IP estáticas, escriba una dirección IP para la interfaz de red.
    - Si usa DHCP, las direcciones IP se asignan automáticamente. Este ejemplo usa DHCP.

    ![Habilitar la configuración de proceso 1](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-1.png)

3. Seleccione **Aplicar** para aplicar la configuración. Anote la dirección IP asignada a la interfaz de red, si usa DHCP.

    ![Habilitación de la configuración del proceso](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-2.png)

## <a name="add-webserver-app-module"></a>Agregar módulo de aplicación de servidor Web

Siga estos pasos para agregar un módulo de aplicación de servidor Web en el dispositivo de borde del cuadro de datos.

1. Vaya a los recursos de IoT Hub asociado con el dispositivo de borde del cuadro de datos y, a continuación, seleccione **dispositivo de IoT Edge**.
2. Seleccione el dispositivo de IoT Edge asociado con el dispositivo de borde del cuadro de datos. En el **detalles del dispositivo**, seleccione **establecer módulos**. En **agregar módulos**, seleccione **+ agregar** y, a continuación, seleccione **módulo de IoT Edge**.
3. En el **módulos personalizados de IoT Edge** hoja:

    1. Especifique un **nombre** para el módulo de aplicación de servidor Web que desea implementar.
    2. Proporcione un **URI de la imagen** para la imagen del módulo. Se recupera un módulo que coincida con las etiquetas y el nombre proporcionado. En este caso, `nginx:stable` extraerá una imagen de nginx estable (etiquetada como estables) del público [repositorio de Docker](https://hub.docker.com/_/nginx/).
    3. En el **opciones de creación del contenedor**, pegue el código de ejemplo siguiente:  

        ```
        {
            "HostConfig": {
                "PortBindings": {
                    "80/tcp": [
                        {
                            "HostPort": "8080"
                        }
                    ]
                }
            }
        }
        ```

        Esta configuración le permite tener acceso al módulo mediante la dirección IP de red de proceso a través de *http* en el puerto TCP 8080 (con el puerto de servidor Web predeterminado que se va a 80).

        ![Especificar información de puerto en la hoja de módulo personalizado de IoT Edge](media/data-box-edge-extend-compute-access-modules/module-information.png)

    4. Seleccione **Guardar**.

## <a name="verify-module-access"></a>Comprobar el acceso del módulo

1. Compruebe que el módulo se implementa correctamente y se está ejecutando. En el **detalles del dispositivo** página, en el **módulos** ficha, el estado de tiempo de ejecución del módulo debe ser **ejecutando**.  
2. Conectar con el módulo de aplicación de servidor web. Abra una ventana del explorador y escriba:

    `http://<compute-network-IP-address>:8080`

    Debería ver que se está ejecutando la aplicación de servidor Web.

    ![Compruebe la conexión al módulo a través del puerto especificado](media/data-box-edge-extend-compute-access-modules/verify-connect-module-1.png)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar usuarios desde Azure Portal](data-box-edge-manage-users.md).
