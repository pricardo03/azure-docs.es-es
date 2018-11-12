---
title: Solución de problemas de conmutaciones por error a Azure | Microsoft Docs
description: En este artículo se describe cómo solucionar los problemas comunes que surgen durante la conmutación por error a Azure con Azure Site Recovery.
author: ponatara
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 09/11/2018
ms.author: ponatara
ms.openlocfilehash: 420d061b34734c7b5997f5cdd58fe7faaee9cb82
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236763"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Solución de problemas y errores cuando una máquina virtual se conmuta por error en Azure

Es posible que aparezca uno de los errores siguientes mientras se realiza la conmutación por error de una máquina virtual en Azure. Para solucionar los problemas, use los pasos que se describen para cada condición de error.

## <a name="failover-failed-with-error-id-28031"></a>No se pudo realizar la conmutación por error con el identificador de error 28031

Site Recovery no pudo crear una máquina virtual conmutada por error en Azure. Esto podría deberse a uno de los siguientes motivos:

* No hay una cuota suficiente disponible para crear la máquina virtual: para comprobar la cuota disponible, vaya a Suscripción -> Uso y cuotas. Puede abrir una [solicitud de soporte técnico nueva](https://aka.ms/getazuresupport) para aumentar la cuota.

* Se intenta conmutar por error máquinas virtuales de familias de distinto tamaño en el mismo conjunto de disponibilidad. Asegúrese de elegir una familia del mismo tamaño para todas las máquinas virtuales del mismo conjunto de disponibilidad. Cambie el tamaño en la configuración de Proceso y red de la máquina virtual y reintente la conmutación por error.

* Hay una directiva de la suscripción que impide crear una máquina virtual. Cambie la directiva para permitir la creación de una máquina virtual y reintente la conmutación por error.

## <a name="failover-failed-with-error-id-28092"></a>No se pudo realizar la conmutación por error con el identificador de error 28092

Site Recovery no pudo crear una interfaz de red para la máquina virtual conmutada por error. Asegúrese de tener una cuota suficiente disponible para crear interfaces de red en la suscripción. Para comprobar la cuota disponible, vaya a Suscripción ->Uso y cuotas. Puede abrir una [solicitud de soporte técnico nueva](https://aka.ms/getazuresupport) para aumentar la cuota. Si tiene una cuota suficiente, puede tratarse de un error intermitente. Vuelva a intentar la operación. Si el problema sigue después de varios reintentos, deje un comentario al final de este documento.  

## <a name="failover-failed-with-error-id-70038"></a>No se pudo realizar la conmutación por error con el identificador de error 70038

Site Recovery no pudo crear una máquina virtual clásica conmutada por error en Azure. Esto podría suceder porque:

* No existe uno de los recursos, como una red virtual, que se requieren para crear la máquina virtual. Cree la red virtual según lo indicado en la configuración de Proceso y red de la máquina virtual, o bien modifique la configuración a una red virtual que ya existe y reintente la conmutación por error.

## <a name="unable-to-connectrdpssh---vm-connect-button-grayed-out"></a>No se puede conectar/RDP/botón de conexión SSH con la máquina virtual no disponible

Si el botón **Conectar** de la máquina virtual conmutada por error de Azure no está disponible y no está conectado a Azure a través de una conexión VPN Express Route o de sitio a sitio, entonces:

1. Vaya a la **Máquina virtual** > **Red**, y haga clic en el nombre de la interfaz de red necesaria.  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navegue hasta **Configuraciones IP** y, después, haga clic en el campo de nombre de la configuración IP necesaria. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Para habilitar la dirección IP pública, haga clic en **Habilitar**. ![Habilitar IP](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Haga clic en **Configurar los valores obligatorios** > **Crear uno nuevo**. ![Cree uno nuevo](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Escriba el nombre de la dirección pública, seleccione las opciones predeterminadas para **SKU**y **asignación** y, después, haga clic en **Aceptar**.
6. Ahora, haga clic en **Guardar** para guardar los cambios.
7. Cierre los paneles y navegue a la sección **Introducción** de la máquina virtual para conectar/RDP.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>No se puede conectar/RDP/botón de conexión SSH con la máquina virtual disponible

Si el botón **Conectar** de la máquina virtual conmutada por error de Azure está disponible, compruebe **Diagnósticos de arranque** en su máquina virtual, así como los errores enumerados en [este artículo](../virtual-machines/windows/boot-diagnostics.md).

1. Si la máquina virtual no se ha iniciado, realice la conmutación por error a un punto de recuperación anterior.
2. Si la aplicación dentro de la máquina virtual no aparece, realice la conmutación por error a un punto de recuperación coherente con la aplicación.
3. Si la máquina virtual está unida al dominio, asegúrese de que ese controlador de dominio funciona correctamente. Esto se puede hacer siguiendo estos pasos:

    a. Cree una nueva máquina virtual en la misma red.

    b.  Asegúrese de que es capaz de unirse al mismo dominio en el que se espera que aparezca la máquina virtual que ha conmutado por error.

    c. Si el controlador de dominio **no** funciona correctamente, inicie sesión en la máquina virtual con conmutación por error mediante una cuenta de administrador local.
4. Si está utilizando un servidor DNS personalizado, asegúrese de que es accesible. Esto se puede hacer siguiendo estos pasos:

    a. Cree una nueva máquina virtual en la misma red y

    b. Compruebe si la máquina virtual es capaz de realizar la resolución de nombres con el servidor DNS personalizado.

>[!Note]
>Habilitar cualquier ajuste que no sea Diagnósticos de arranque requiere que el agente de la máquina virtual de Azure esté instalado en la máquina virtual antes de la conmutación por error.

## <a name="unexpected-shutdown-message-event-id-6008"></a>Mensaje de cierre inesperado (Id. de evento 6008)

Al arrancar una máquina tras la conmutación por error, si recibe un mensaje de cierre inesperado sobre la máquina virtual recuperada, esto indica que el estado de cierre no se capturó en el punto de recuperación usado para la conmutación por error. Esto ocurre al recuperarse hasta un punto en el que la máquina virtual no se había cerrado completamente.

Esto no suele ser motivo de preocupación y normalmente puede omitirse en las conmutaciones por error no planeadas. En el caso de una conmutación por error planeada, asegúrese de que la máquina virtual se cierra correctamente antes de la conmutación por error y proporcione tiempo suficiente para que los datos de replicación pendientes locales se envíen a Azure. A continuación, use la opción **Más reciente** de la [pantalla Conmutación por error](site-recovery-failover.md#run-a-failover) para que los datos pendientes de Azure se procesen en un punto de recuperación, el cual se utiliza para la conmutación por error de la máquina virtual.

## <a name="retaining-drive-letter-after-failover"></a>Conservación de la letra de unidad después de la conmutación por error
Para conservar la letra de unidad en las máquinas virtuales después de la conmutación por error, puede establecer la **Directiva SAN** de la máquina virtual local en **OnlineAll**. [Más información](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).

## <a name="next-steps"></a>Pasos siguientes
- Solucionar problemas de [conexión RDP con una máquina virtual Windows](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Solucionar problemas de [conexión SSH a una máquina virtual Linux](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Si necesita más ayuda, publique la consulta en el [foro de Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) o deje un comentario al final de este documento. Tenemos una comunidad activa que debería poder ayudarle.
