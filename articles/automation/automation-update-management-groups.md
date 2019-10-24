---
title: Uso de grupos dinámicos con Azure Update Management
description: En este tema se describe cómo usar grupos dinámicos con Update Management
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 10/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: edc4384be0f1dc73f2e7e098114080d304d92ce8
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377449"
---
# <a name="use-dynamic-groups-with-update-management"></a>Uso de grupos dinámicos con Update Management

Update Management permite tener como destino un grupo dinámico de máquinas virtuales de Azure (o de máquinas virtuales que no sean de Azure) para implementaciones de actualizaciones. Estos grupos se evalúan en tiempo de implementación, por lo que no tiene que editar la implementación para agregar máquinas.

## <a name="azure-machines"></a>Máquinas de Azure

Estos grupos se definen mediante una consulta, cuando se inicia una implementación de actualizaciones, se evalúan los miembros de ese grupo. Los grupos dinámicos no funcionan con máquinas virtuales clásicas. Al definir la consulta, se pueden usar los siguientes elementos juntos para rellenar el grupo dinámico:

* Subscription
* Grupos de recursos
* Ubicaciones
* Etiquetas

![Selección de grupos](./media/automation-update-management/select-groups.png)

Para obtener una vista previa de los resultados de un grupo dinámico, haga clic en el botón **Vista previa**. Esta vista previa muestra la pertenencia al grupo en ese momento; en este ejemplo, estamos buscando máquinas con la etiqueta **Role** es igual a **BackendServer**. Si se ha agregado esta etiqueta a varias máquinas, se agregarán a las implementaciones futuras en ese grupo.

![vista previa de grupos](./media/automation-update-management/preview-groups.png)

## <a name="non-azure-machines"></a>Máquinas que no son de Azure

En el caso de las máquinas que no son de Azure, las búsquedas guardadas, a las que también se hace referencia como grupos de equipos, se usan para crear el grupo dinámico. Para aprender a crear una búsqueda guardada, consulte [Creación de un grupo de equipos](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Una vez creado el grupo puede seleccionarlo en la lista de búsquedas guardadas. Haga clic en **Vista previa** para obtener una vista previa de los equipos de la búsqueda guardada en ese momento.

![Selección de grupos](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>Pasos siguientes

Después de crear un grupo dinámico, puede [crear una implementación de actualizaciones](automation-tutorial-update-management.md).
