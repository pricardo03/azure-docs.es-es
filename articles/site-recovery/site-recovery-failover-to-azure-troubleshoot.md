---
title: Solución de problemas de conmutación por error en Azure | Microsoft Docs
description: En este artículo se describe cómo solucionar problemas y errores comunes de la conmutación por error en Azure
services: site-recovery
documentationcenter: ''
author: ponatara
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: b7b5dcd88b6e4e09dd9beb21e83ef405df148115
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443391"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Solución de problemas y errores cuando una máquina virtual se conmuta por error en Azure

Es posible que aparezca uno de los errores siguientes mientras se realiza la conmutación por error de una máquina virtual en Azure. Para solucionar los problemas, use los pasos que se describen para cada condición de error.

## <a name="failover-failed-with-error-id-28031"></a>No se pudo realizar la conmutación por error con el identificador de error 28031

Site Recovery no pudo crear una máquina virtual conmutada por error en Azure. Esto podría deberse a uno de los siguientes motivos:

* No hay una cuota suficiente disponible para crear la máquina virtual: para comprobar la cuota disponible, vaya a Suscripción -> Uso y cuotas. Puede abrir una [solicitud de soporte técnico nueva](http://aka.ms/getazuresupport) para aumentar la cuota.

* Se intenta conmutar por error máquinas virtuales de familias de distinto tamaño en el mismo conjunto de disponibilidad. Asegúrese de elegir una familia del mismo tamaño para todas las máquinas virtuales del mismo conjunto de disponibilidad. Cambie el tamaño en la configuración de Proceso y red de la máquina virtual y reintente la conmutación por error.

* Hay una directiva de la suscripción que impide crear una máquina virtual. Cambie la directiva para permitir la creación de una máquina virtual y reintente la conmutación por error.

## <a name="failover-failed-with-error-id-28092"></a>No se pudo realizar la conmutación por error con el identificador de error 28092

Site Recovery no pudo crear una interfaz de red para la máquina virtual conmutada por error. Asegúrese de tener una cuota suficiente disponible para crear interfaces de red en la suscripción. Para comprobar la cuota disponible, vaya a Suscripción ->Uso y cuotas. Puede abrir una [solicitud de soporte técnico nueva](http://aka.ms/getazuresupport) para aumentar la cuota. Si tiene una cuota suficiente, puede tratarse de un error intermitente. Vuelva a intentar la operación. Si el problema sigue después de varios reintentos, deje un comentario al final de este documento.  

## <a name="failover-failed-with-error-id-70038"></a>No se pudo realizar la conmutación por error con el identificador de error 70038

Site Recovery no pudo crear una máquina virtual clásica conmutada por error en Azure. Esto podría suceder porque:

* No existe uno de los recursos, como una red virtual, que se requieren para crear la máquina virtual. Cree la red virtual según lo indicado en la configuración de Proceso y red de la máquina virtual, o bien modifique la configuración a una red virtual que ya existe y reintente la conmutación por error.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>No se puede conectar/RDP/SSH a la máquina virtual que conmutó por error debido a que el botón Conectar de la máquina virtual no está disponible.

Si el botón Conectar no está disponible y no está conectado a Azure a través de una conexión VPN Express Route o de sitio a sitio, entonces:

1. Vaya a la **Máquina virtual** > **Red**, y haga clic en el nombre de la interfaz de red necesaria.  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
1. Navegue hasta **Configuraciones IP** y, después, haga clic en el campo de nombre de la configuración IP necesaria. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
1. Para habilitar la dirección IP pública, haga clic en **Habilitar**. ![Habilitar IP](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
1. Haga clic en **Configurar los valores obligatorios** > **Crear uno nuevo**. ![Cree uno nuevo](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
1. Escriba el nombre de la dirección pública, seleccione las opciones predeterminadas para **SKU**y **asignación** y, después, haga clic en **Aceptar**.
1. Ahora, haga clic en **Guardar** para guardar los cambios.
1. Cierre los paneles y navegue a la sección **Introducción** de la máquina virtual para conectar/RDP.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-even-though-connect-button-is-available-not-grayed-out-on-the-virtual-machine"></a>No se puede conectar/RDP/SSH a la máquina virtual que conmutó por error aunque el botón Conectar de la máquina virtual está disponible.

Compruebe **Diagnósticos de arranque** en la máquina virtual y compruebe los errores que se muestran en este artículo.

1. Si la máquina virtual no se ha iniciado, realice la conmutación por error a un punto de recuperación anterior.
1. Si la aplicación dentro de la máquina virtual no aparece, realice la conmutación por error a un punto de recuperación coherente con la aplicación.
1. Si la máquina virtual está unida al dominio, asegúrese de que ese controlador de dominio funciona correctamente. Esto se puede hacer siguiendo los siguientes pasos.
    a. Cree una nueva máquina virtual en la misma red.

    b.  Asegúrese de que es capaz de unirse al mismo dominio en el que se espera que aparezca la máquina virtual que ha conmutado por error.

    c. Si el controlador de dominio no funciona correctamente, inicie sesión en la máquina virtual con conmutación por error mediante una cuenta de administrador local.
1. Si está utilizando un servidor DNS personalizado, asegúrese de que es accesible. Esto se puede hacer siguiendo los siguientes pasos.
    a. Cree una nueva máquina virtual en la misma red. b. Compruebe si la máquina virtual es capaz de realizar la resolución de nombres con el servidor DNS personalizado.

>[!Note]
>Habilitar cualquier ajuste que no sea Diagnósticos de arranque requiere que el agente de la máquina virtual de Azure esté instalado en la máquina virtual antes de la conmutación por error.

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda, publique la consulta en el [foro de Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) o deje un comentario al final de este documento. Tenemos una comunidad activa que debería poder ayudarle.
