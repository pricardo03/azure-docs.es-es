---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 05/09/2018
ms.date: 06/04/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 9c4910d9ec1fdf651b1bc29955a4e1de2a775f7a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60576159"
---
1. Haga clic en el botón **Conectar** en la página de propiedades de la máquina virtual. 
2. En la página **Conexión a la máquina virtual**, mantenga seleccionadas las opciones adecuadas y haga clic en **Descargar archivo RDP**.
2. Abra el archivo RDP que descargó y haga clic en **Conectar** cuando se le solicite. 
2. Aparece una advertencia que indica que el archivo `.rdp` procede de un editor desconocido. Esto es normal. En la ventana de Escritorio remoto, haga clic en **Conectar** para continuar.

    ![Captura de pantalla de una advertencia sobre un editor desconocido.](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. En la ventana **Seguridad de Windows**, seleccione **Más opciones** y, después, **Usar otra cuenta**. Escriba las credenciales de una cuenta en la máquina virtual y haga clic en **Aceptar**.

     **Cuenta local** : suele ser el nombre de usuario y la contraseña de la cuenta local que especificó al crear la máquina virtual. En este caso, el dominio es el nombre de la máquina virtual y se escribe como *nombreDeVm*&#92;*nombreDeUsuario*.  

    **Máquina virtual unida a dominio**: si la máquina virtual pertenece a un dominio, escriba el nombre de usuario con el formato *Dominio*&#92;*Nombre de usuario*. La cuenta también debe estar en el grupo Administradores o tener privilegios de acceso remoto a la máquina virtual.

    **Controlador de dominio** : si la máquina virtual es un controlador de dominio, escriba el nombre de usuario y la contraseña de una cuenta de administrador de dominio para ese dominio.
4. Haga clic en **Sí** para comprobar la identidad de la máquina virtual y finalizar el inicio de sesión.

   ![Captura de pantalla que muestra un mensaje sobre la comprobación de la identidad de la máquina virtual.](./media/virtual-machines-log-on-win-server/cert-warning.png)
