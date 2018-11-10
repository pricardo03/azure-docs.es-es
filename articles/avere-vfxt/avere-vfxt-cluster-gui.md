---
title: Acceso al panel de control de Avere vFXT - Azure
description: Cómo conectarse al clúster de vFXT y al panel de control basado en explorador de Avere para configurar Avere vFXT
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: c48f0d8f7ad34db585f4deae566641b6453357e8
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670062"
---
# <a name="access-the-vfxt-cluster"></a>Acceso al clúster de vFXT

Para cambiar la configuración y supervisar el clúster de Avere vFXT, use el panel de control de Avere. El panel de control de Avere es una interfaz gráfica basada en explorador para el clúster.

Dado que el clúster de vFXT se encuentra dentro de una red virtual privada, debe crear un túnel SSH o usar otro método para conectarse con la dirección IP de administración del clúster. Hay dos pasos básicos: 

1. Crear una conexión entre su estación de trabajo y la red virtual privada 
1. Usar la dirección IP de administración del clúster para cargar el panel de control en un explorador web 

> [!NOTE] 
> En este artículo se da por supuesto que ha establecido una dirección IP pública en el controlador del clúster o en otra VM dentro de la red virtual del clúster. Si usa una VPN o ExpressRoute para el acceso a la red virtual, vaya a [Conexión al panel de control de Avere](#connect-to-the-avere-control-panel-in-a-browser).

Antes de conectarse, asegúrese de que el par de claves pública y privada SSH que usó al crear el controlador del clúster está instalado en el equipo local. Lea la documentación de las claves SSH para [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) o para [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) si necesita ayuda.  

## <a name="access-with-a-linux-host"></a>Acceso con un host Linux

con esta forma: 

ssh -L *local_port*:*cluster_mgmt_ip*:443 *controller_username*@*controller_public_IP* 

Este comando se conecta a la dirección IP de administración del clúster a través de la dirección IP del controlador del clúster.

Ejemplo:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

La autenticación es automática si usa la clave pública SSH para crear el clúster y la clave correspondiente está instalada en el sistema cliente.


## <a name="access-with-a-windows-host"></a>Acceso con un host Windows

Si usa PuTTY, rellene el campo **hostname** con el nombre de usuario del controlador del clúster y su dirección IP: *su_nombre_de_usuario*@*IP_pública_del_controlador*.

Ejemplo: ``azureuser@203.0.113.51``

En el panel **Configuración**:

1. Expanda **Conexión** > **SSH** a la izquierda. 
1. Haga clic en **Túneles**. 
1. Escriba un puerto de origen, como 8443. 
1. Para el destino, escriba la dirección IP del clúster de vFXT y el puerto 443. 
   Ejemplo: ``203.0.113.51:443``
1. Haga clic en **Agregar**.
1. Haga clic en **Abrir**.

![Captura de pantalla de la aplicación Putty que muestra dónde hacer clic para agregar un túnel](media/avere-vfxt-ptty-numbered.png)

La autenticación es automática si usa la clave pública SSH para crear el clúster y la clave correspondiente está instalada en el sistema cliente.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Conexión al panel de control de Avere en un explorador

Este paso usa un explorador web para conectarse a la utilidad de configuración que se ejecutan en el clúster de vFXT.

Abra el explorador web y vaya a https://127.0.0.1:8443. 

Según el explorador, es posible que tenga que hacer clic en **Advanced** y comprobar que es seguro continuar a la página.

Especifique el nombre de usuario `admin` y la contraseña que proporcionó al crear el clúster.

![Captura de pantalla de la página de inicio de sesión de Avere con el nombre de usuario "admin" y una contraseña](media/avere-vfxt-gui-login.png)

Haga clic en **Login** o presione ENTRAR en el teclado.

## <a name="next-steps"></a>Pasos siguientes

Ahora que puede acceder al clúster, habilite el [soporte](avere-vfxt-enable-support.md).
