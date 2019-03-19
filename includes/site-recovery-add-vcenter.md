---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 926fb3e9a2c09d30da549330842d8b7e185674ae
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57908462"
---
En **Add vCenter** (Agregar vCenter), especifique un nombre descriptivo para el host de vSphere o el servidor vCenter y especifique la dirección IP o el FQDN del servidor. Deje el puerto 443 a menos que los servidores de VMware estén configurados para escuchar las solicitudes en un puerto diferente. Seleccione la cuenta que va a conectar al servidor de VMware vCenter o vSphere ESXi. Haga clic en **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Si va a agregar el servidor VMware vCenter o host de VMware vSphere con una cuenta que no tiene privilegios de administrador en el servidor vCenter o host, asegúrese de que la cuenta tiene habilitados los siguientes privilegios: Centro de datos, almacén de datos, carpeta, Host, red, recurso, Máquina Virtual y conmutador distribuido de vSphere. Además, el servidor VMware vCenter necesita el privilegio Vistas de almacenamiento habilitado.
