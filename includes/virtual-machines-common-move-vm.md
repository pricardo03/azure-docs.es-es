---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/19/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 61990e785741799fcbcd4e6df965953bd9944f4d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473943"
---
## <a name="use-the-azure-portal-to-move-a-vm-to-a-different-subscription"></a>Uso de Azure Portal para mover una VM a otra suscripción
Puede mover una VM y sus recursos asociados a otra suscripción mediante Azure Portal.

1. Abra [Azure Portal](https://portal.azure.com).
2. Haga clic en **Examinar** > **Grupos de recursos** y seleccione el grupo de recursos que contiene la VM que quiere mover.
3. En la parte superior de la página para el grupo de recursos, seleccione **Mover** y, luego, seleccione **Mover a otra suscripción**. Se abre la página **Mover recursos**.
4. Seleccione cada uno de los recursos que quiere mover. En la mayoría de los casos, debe mover todos los recursos relacionados que se muestran.
5. Seleccione la **suscripción** adonde quiere mover la máquina virtual.
6. Seleccione un **grupo de recursos** existente o escriba un nombre para que se cree uno.
7. Cuando haya terminado, seleccione que comprende que se crearán nuevos identificadores de recursos y que los nuevos identificadores se deberán usar con la VM después de que se mueva. Luego seleccione **Aceptar**.

## <a name="use-the-azure-portal-to-move-a-vm-to-another-resource-group"></a>Uso de Azure Portal para mover una VM a otro grupo de recursos
Puede mover una VM y sus recursos asociados a otro grupo de recursos mediante Azure Portal.

1. Abra [Azure Portal](https://portal.azure.com).
2. Haga clic en **Examinar** > **Máquinas virtuales** y seleccione la máquina virtual que desee mover de la lista.
3. En la página de la VM, junto a la etiqueta para el grupo de recursos, seleccione **Cambiar**. Se abre la página **Mover recursos**.
4. Seleccione cada uno de los recursos que quiere mover. En la mayoría de los casos, debe mover todos los recursos relacionados que se muestran.
5. Seleccione un **grupo de recursos** existente o escriba un nombre para que se cree uno.
6. Cuando haya terminado, seleccione que comprende que se crearán nuevos identificadores de recursos y que los nuevos identificadores se deberán usar con la VM después de que se mueva. Luego seleccione **Aceptar**.

