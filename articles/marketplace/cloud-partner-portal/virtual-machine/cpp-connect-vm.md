---
title: Conectarse a la máquina virtual basada en Microsoft Azure | Microsoft Docs
description: Se explica cómo conectarse a la nueva máquina virtual creada en Azure.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: fd68846b9144c3efcc71dec369d64119427758a3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60744559"
---
# <a name="connect-to-your-azure-based-virtual-machine"></a>Conectarse a la máquina virtual basada en Azure

En este artículo se explica cómo conectarse a las máquinas virtuales (VM) creadas en Azure y cómo iniciar sesión en ellas.  Cuando se haya conectado correctamente, puede trabajar con la máquina virtual como si hubiera iniciado sesión localmente en su servidor host. 

## <a name="connect-to-a-windows-based-vm"></a>Conectarse a una máquina virtual basada en Windows

Para conectarse a la máquina virtual basada en Windows hospedada en Azure, se usa el cliente de escritorio remoto.  La mayoría de las versiones de Windows incluyen compatibilidad de forma nativa con el Protocolo de escritorio remoto (RDP).  En el caso de las demás máquinas, puede encontrar más información sobre los clientes en [Clientes de escritorio remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).  

El siguiente artículo detalla cómo usar la compatibilidad integrada de Windows RDP para conectarse a la máquina virtual: [Cómo conectarse e iniciar sesión en una máquina virtual de Azure ejecutando Windows](../../../virtual-machines/windows/connect-logon.md).  

>[!TIP]
> Es posible que obtenga advertencias de seguridad durante el proceso, por ejemplo que el archivo .rdp procede de un publicador desconocido o que no se pueden verificar sus credenciales de usuario.  Es seguro hacer caso omiso de estas advertencias.


## <a name="connect-to-a-linux-based-vm"></a>Conectarse a una máquina virtual basada en Linux

Para conectarse a la máquina virtual basada en Linux, necesita un cliente de protocolo Secure Shell (SSH).  Esta discusión usa el terminal SSH [PuTTY](https://www.ssh.com/ssh/putty/) libre.

1. En la hoja **Máquinas virtuales** de [Azure Portal](https://ms.portal.azure.com), seleccione la máquina virtual a la que se quiere conectar.  
2. **Inicie** la máquina virtual si aún no se está ejecutando.
3. Haga clic en el nombre de la máquina virtual para abrir su página **Información general**.
4. Anote la dirección IP pública y el nombre DNS de la máquina virtual.  (Si estos valores no están establecidos, debe [Crear una interfaz de red](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface))

   ![Configuración de Información general de máquina virtual](./media/publishvm_019.png)
 
5. Abra la aplicación PuTTY.  
6. En el cuadro de diálogo Configuración de PuTTY, escriba la dirección IP o el nombre DNS de la máquina virtual. 

   ![Configuración de terminal PuTTY](./media/publishvm_020.png)
 
7. Haga clic en **Abrir** para abrir un terminal PuTTY.  
8. Cuando se le pida, escriba el nombre y la contraseña de la cuenta de máquina virtual Linux. 

   Si tiene problemas de conexión, consulte la documentación del cliente SSH, por ejemplo [capítulo 10: Mensajes de error comunes](https://www.ssh.com/ssh/putty/putty-manuals/0.68/Chapter10.html#errors).

Para obtener más información, por ejemplo cómo agregar un escritorio a una máquina virtual Linux aprovisionada, vea [Instalación y configuración del escritorio remoto para conectarse a una máquina virtual Linux en Azure](../../../virtual-machines/linux/use-remote-desktop.md).


## <a name="stop-unused-vms"></a>Detener máquinas virtuales sin usar
Azure factura por el hospedaje de máquinas virtuales cuando hay una en ejecución *o inactiva*.  Por lo tanto, es recomendable detener las máquinas virtuales que no se están usando.  Por ejemplo, las máquinas virtuales de prueba, de copia de seguridad o retiradas son candidatas para la detención. Para detener una máquina virtual, realice los pasos siguientes:

1. En la hoja **Máquinas virtuales**, seleccione la máquina virtual que quiere detener. 
2. En la barra de herramientas de la parte superior de la página, haga clic en el botón **Detener**.

   ![Detención de una máquina virtual](./media/publishvm_018.png)

Azure detiene rápidamente la máquina virtual en un proceso denominado *desasignación*, que no solo apaga el sistema operativo en la máquina virtual, sino que también libera los recursos de hardware y de red aprovisionados previamente para ella.

Si quiere volver a activar más adelante una máquina virtual detenida, selecciónela y haga clic en el botón **Iniciar**.


## <a name="next-steps"></a>Pasos siguientes

Cuando se haya conectado de forma remota, estará listo para [configurar la máquina virtual](./cpp-configure-vm.md).
