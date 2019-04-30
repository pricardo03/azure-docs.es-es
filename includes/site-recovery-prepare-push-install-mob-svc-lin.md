---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 5ba55e339db4c33d1b0d759e4682481e20318938
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122849"
---
### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Preparación de una instalación de inserción en un servidor Linux

1. Asegúrese de que hay conectividad de red entre la el equipo con Linux y el servidor de procesos.
1. Cree una cuenta que el servidor de procesos pueda utilizar para acceder al equipo. La cuenta debe ser un usuario **raíz** en el servidor Linux de origen. Esta cuenta solo se usa para la instalación de inserción y para las actualizaciones.
1. Compruebe que el archivo /etc/hosts del servidor Linux de origen tiene entradas que asignan el nombre de host local a las direcciones IP asociadas con todos los adaptadores de red.
1. Instale los paquetes openssh, openssh-server y openssl más recientes en el equipo que desea replicar.
1. Asegúrese de que Secure Shell (SSH) está habilitado y se ejecuta en el puerto 22.
1. Habilitación de la autenticación de la contraseña y el subsistema SFTP en el archivo sshd_config. Siga estos pasos:

     a. Inicie sesión como usuario **raíz**.

    b. En el archivo **/etc/ssh/sshd_config**, encuentre la línea que comienza por **PasswordAuthentication**.

    c. Quite la marca de comentario de la línea y cambie el valor **yes**.

    d. Busque la línea que comienza por **Subsystem** y quite la marca de comentario.

      ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

    e. Reinicie el servicio **sshd**.

1. Agregue la cuenta que creó en CSPSConfigtool. Siga estos pasos:

     a. Inicie sesión en el servidor de configuración.

    b. Abra **cspsconfigtool.exe**. Esta herramienta está disponible como acceso directo en el escritorio y se encuentra en la carpeta %ProgramData%\home\svsystems\bin.

    c. En la pestaña **Administrar cuentas**, seleccione en **Agregar cuenta**.

    d. Agregue la cuenta que creó.

    d. Especifique las credenciales que utiliza al habilitar la replicación en un equipo.
