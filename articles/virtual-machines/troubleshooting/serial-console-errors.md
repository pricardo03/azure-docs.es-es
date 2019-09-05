---
title: Errores de la consola serie de Azure | Microsoft Docs
description: Errores comunes en la consola serie de Azure
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/20/2019
ms.author: alsin
ms.openlocfilehash: a997675a2f7a280e8311e2bba7dca21de82a086e
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129472"
---
# <a name="common-errors-within-the-azure-serial-console"></a>Errores comunes en la consola serie de Azure
Existe un conjunto de errores conocidos en la consola serie de Azure. A continuación encontrará una lista de dichos errores y los pasos de mitigación.

## <a name="common-errors"></a>Errores comunes

Error                            |   Mitigación
:---------------------------------|:--------------------------------------------|
No se puede recuperar la configuración de diagnóstico de arranque para *&lt;VMNAME&gt;* . Para usar la consola serie, asegúrese de que el diagnóstico de arranque está habilitado para esta máquina virtual. ![Error de diagnóstico de arranque](./media/virtual-machines-serial-console/virtual-machines-serial-console-boot-diagnostics-error.png) | Asegúrese de que la máquina virtual o el conjunto de escalado de máquinas virtuales tiene el [diagnóstico de arranque](boot-diagnostics.md) habilitado. Si usa la consola serie en una instancia de conjunto de escalado de máquinas virtuales, asegúrese de que la instancia tenga el modelo más reciente.
La máquina virtual se encuentra en un estado desasignado detenido. Inicie la máquina virtual y vuelva a intentar la conexión de la consola serie. ![Error de desasignación](./media/virtual-machines-serial-console/virtual-machines-serial-console-deallocating-error.png) | La máquina virtual o la instancia de conjunto de escalado de máquinas virtuales debe encontrarse en un estado iniciado para tener acceso a la consola serie. Inicie la máquina virtual o la instancia de conjunto de escalado de máquinas virtuales e inténtelo de nuevo.
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnósticos de arranque de la VM. ![Error de firewall de la cuenta de almacenamiento](./media/virtual-machines-serial-console/virtual-machines-serial-console-firewall-error.png)| Asegúrese de que los diagnósticos de arranque no tengan ningún firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnósticos de arranque accesible para que la consola serie funcione.
No tiene los permisos necesarios para usar esta máquina virtual como consola serie. Asegúrese de tener los permisos del rol de Colaborador de la máquina virtual como mínimo.| El acceso a la consola serie requiere tener acceso de nivel de colaborador o superior en la máquina virtual o en el conjunto de escalado de máquinas virtuales. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
No se encontró la cuenta de almacenamiento usada para los diagnósticos de arranque en esta máquina virtual. Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual, que esta cuenta de almacenamiento no se ha eliminado y que tiene acceso a esta cuenta de almacenamiento. | Asegúrese de que no ha eliminado la cuenta de almacenamiento de diagnósticos de arranque para la máquina virtual o el conjunto de escalado de máquinas virtuales.
El aprovisionamiento de esta máquina virtual aún no se ha realizado correctamente. Asegúrese de que la máquina virtual está totalmente implementada y vuelva a intentar la conexión de la consola serie. | Es posible que la máquina virtual o el conjunto de escalado de máquinas virtuales todavía se estén aprovisionando. Espere un poco y vuelva a intentarlo.
No tiene los permisos necesarios para escribir en la cuenta de almacenamiento de diagnósticos de arranque de esta máquina virtual. Asegúrese de tener al menos permisos de colaborador de máquina virtual. | El acceso a la consola serie requiere el acceso de nivel de colaborador en la cuenta de almacenamiento de diagnósticos de arranque. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
No se puede determinar el grupo de recursos para la cuenta de almacenamiento de diagnósticos de arranque *&lt;STORAGEACCOUNTNAME&gt;* . Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual y acceda a esta cuenta de almacenamiento. | El acceso a la consola serie requiere el acceso de nivel de colaborador en la cuenta de almacenamiento de diagnósticos de arranque. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
Socket web está cerrado o no se pudo abrir. | Es posible que tenga que agregar acceso de firewall a `*.console.azure.com`. Un enfoque más detallado, pero más largo, consiste en permitir el acceso de firewall a los [intervalos IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653), que cambian con bastante frecuencia.


## <a name="next-steps"></a>Pasos siguientes
* Más información sobre la [consola serie de Azure para máquinas virtuales Linux](./serial-console-linux.md)
* Más información sobre la [consola serie de Azure para máquinas virtuales Windows](./serial-console-windows.md)