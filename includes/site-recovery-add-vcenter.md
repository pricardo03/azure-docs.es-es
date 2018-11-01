---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 8f7eae06947a40117f3952a0a5624c22df750b34
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164883"
---
* En **Add vCenter** (Agregar vCenter), especifique un nombre descriptivo para el host de vSphere o el servidor vCenter y especifique la dirección IP o el FQDN del servidor. Deje el puerto 443 a menos que los servidores de VMware estén configurados para escuchar las solicitudes en un puerto diferente. Seleccione la cuenta que va a conectar al servidor de VMware vCenter o vSphere ESXi. Haga clic en **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Si agrega el servidor VMware vCenter o el host VMware vSphere con una cuenta que no tiene privilegios de administrador en el servidor vCenter o el servidor host, asegúrese de que la cuenta tiene habilitados los siguientes privilegios: Centro de datos, Almacén de datos, Carpeta, Host, Red, Recurso, Máquina virtual y Conmutador distribuido de vSphere. Además, el servidor VMware vCenter necesita el privilegio Vistas de almacenamiento habilitado.
