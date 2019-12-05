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
ms.openlocfilehash: 7bd9fe4044dace4061285c016cb08562b556b98e
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483641"
---
# <a name="common-errors-within-the-azure-serial-console"></a>Errores comunes en la consola serie de Azure
Existe un conjunto de errores conocidos en la consola serie de Azure. A continuación encontrará una lista de dichos errores y los pasos de mitigación.

## <a name="common-errors"></a>Errores comunes

Error                             |   Mitigación
:---------------------------------|:--------------------------------------------|
"La consola serie de Azure requiere que se habiliten los diagnósticos de arranque. Haga clic aquí para configurar los diagnósticos de arranque de la máquina virtual". ![Error de diagnóstico de arranque](./media/virtual-machines-serial-console/virtual-machines-serial-console-boot-diagnostics-error.png) | Asegúrese de que la máquina virtual o el conjunto de escalado de máquinas virtuales tiene el [diagnóstico de arranque](boot-diagnostics.md) habilitado. Si usa la consola serie en una instancia de conjunto de escalado de máquinas virtuales, asegúrese de que la instancia tenga el modelo más reciente.
"La consola serie de Azure requiere que haya una máquina virtual en ejecución. Use el botón Inicio anterior para iniciar la máquina virtual". ![Error de desasignación](./media/virtual-machines-serial-console/virtual-machines-serial-console-deallocating-error.png) | La instancia de la máquina virtual o del conjunto de escalado de máquinas virtuales debe encontrarse en un estado iniciado para tener acceso a la consola serie (la VM no debe estar detenida ni desasignada). Compruebe que la instancia de la máquina virtual o del conjunto de escalado de máquinas virtuales está en ejecución e inténtelo de nuevo.
"La consola serie de Azure no está habilitada para esta suscripción, póngase en contacto con el administrador de la suscripción para habilitarla". ![Error de suscripción deshabilitada](./media/virtual-machines-serial-console/virtual-machines-serial-console-subscription-disabled-error.png) | La consola serie de Azure se puede deshabilitar en el nivel de suscripción. Si es administrador de suscripciones, puede [habilitar y deshabilitar la consola serie de Azure](./serial-console-enable-disable.md). Si no es administrador de suscripciones, debe ponerse en contacto con el administrador de suscripciones para conocer los pasos siguientes.
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnósticos de arranque de la VM. ![Error de firewall de la cuenta de almacenamiento](./media/virtual-machines-serial-console/virtual-machines-serial-console-firewall-error.png)| Asegúrese de que los diagnósticos de arranque no tengan ningún firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnósticos de arranque accesible para que la consola serie funcione. Por su diseño, la consola serie no puede operar con firewalls de la cuenta de almacenamiento habilitados en la cuenta de almacenamiento del diagnóstico de arranque.
No tiene los permisos necesarios para usar esta máquina virtual como consola serie. Asegúrese de tener los permisos del rol de Colaborador de la máquina virtual como mínimo.| El acceso a la consola serie requiere tener acceso de nivel de colaborador o superior en la máquina virtual o en el conjunto de escalado de máquinas virtuales. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
No se encontró la cuenta de almacenamiento usada para los diagnósticos de arranque en esta máquina virtual. Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual, que esta cuenta de almacenamiento no se ha eliminado y que tiene acceso a esta cuenta de almacenamiento. | Asegúrese de que no ha eliminado la cuenta de almacenamiento de diagnósticos de arranque para la máquina virtual o el conjunto de escalado de máquinas virtuales.
El aprovisionamiento de esta máquina virtual aún no se ha realizado correctamente. Asegúrese de que la máquina virtual está totalmente implementada y vuelva a intentar la conexión de la consola serie. | Es posible que la máquina virtual o el conjunto de escalado de máquinas virtuales todavía se estén aprovisionando. Espere un poco y vuelva a intentarlo.
No tiene los permisos necesarios para escribir en la cuenta de almacenamiento de diagnósticos de arranque de esta máquina virtual. Asegúrese de tener al menos permisos de colaborador de máquina virtual. | El acceso a la consola serie requiere el acceso de nivel de colaborador en la cuenta de almacenamiento de diagnósticos de arranque. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
No se puede determinar el grupo de recursos para la cuenta de almacenamiento de diagnósticos de arranque *&lt;STORAGEACCOUNTNAME&gt;* . Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual y acceda a esta cuenta de almacenamiento. | El acceso a la consola serie requiere el acceso de nivel de colaborador en la cuenta de almacenamiento de diagnósticos de arranque. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
Socket web está cerrado o no se pudo abrir. | Es posible que tenga que agregar acceso de firewall a `*.console.azure.com`. Un enfoque más detallado, pero más largo, consiste en permitir el acceso de firewall a los [intervalos IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653), que cambian con bastante frecuencia.
La consola serie no funciona con una cuenta de almacenamiento que use Azure Data Lake Storage Gen2 con espacios de nombres jerárquicos. | Se trata de un problema conocido con los espacios de nombres jerárquicos. Para mitigar el problema, asegúrese de que la cuenta de almacenamiento con diagnóstico de arranque de la máquina virtual no se crea mediante Azure Data Lake Storage Gen2. Esta opción solo puede establecerse durante la creación de la cuenta de almacenamiento. Es posible que deba crear cuenta de almacenamiento con diagnóstico de arranque independiente sin Azure Data Lake Storage Gen2 habilitado para mitigar este problema.


## <a name="next-steps"></a>Pasos siguientes
* Más información sobre la [consola serie de Azure para máquinas virtuales Linux](./serial-console-linux.md)
* Más información sobre la [consola serie de Azure para máquinas virtuales Windows](./serial-console-windows.md)