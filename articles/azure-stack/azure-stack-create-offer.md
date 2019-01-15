---
title: Creación de una oferta en Azure Stack | Microsoft Docs
description: Como administrador de la nube, aprenda a crear una oferta para los usuarios de Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 079f45e37bef24ac974a0e2df7b1e81f1002cac0
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159084"
---
# <a name="create-an-offer-in-azure-stack"></a>Creación de una oferta en Azure Stack

Las [ofertas](azure-stack-key-features.md) son grupos de uno o varios planes que los proveedores presentan a los usuarios para que estos los compren o se suscriban a ellos. Este artículo describe cómo crear una oferta que incluye el [plan que ha creado](azure-stack-create-plan.md). Esta oferta da a los suscriptores la posibilidad de configurar máquinas virtuales.

1. Inicie sesión en el [portal de administrador de Azure Stack](https://adminportal.local.azurestack.external) y seleccione **+ Crear un recurso**, **Ofertas + Planes de inquilinos** y, finalmente, **Oferta**.

   ![Creación de una oferta](media/azure-stack-create-offer/image01.png)
  
2. En **Nueva oferta**, escriba un **Nombre para mostrar** y un **Nombre de recurso** y, después, en **Grupo de recursos** seleccione **Crear nuevo** o **Usar existente**. El nombre para mostrar es el nombre descriptivo de la oferta. Este nombre descriptivo es la única información acerca de la oferta que verán los usuarios cuando se suscriban a ella. Utilice un nombre intuitivo que ayude al usuario a entender lo que incluye la oferta. Solo el administrador puede ver el nombre del recurso. Es el nombre que usan los administradores para trabajar con la oferta como un recurso de Azure Resource Manager.

   ![Nueva oferta](media/azure-stack-create-offer/image01a.png)
  
3. Seleccione **Planes base** para abrir el **plan**. Seleccione los planes que desea incluir en la oferta y, después, elija **Seleccionar**. Para crear la oferta, seleccione **Crear**.

   ![Seleccionar plan](media/azure-stack-create-offer/image02.png)
  
4. Después de crear la oferta puede cambiar su estado. Las ofertas deben hacerse **públicas** para que los usuarios tengan la vista completa de ellas cuando se suscriban. Las ofertas pueden ser:

   - **Públicas:** son visibles para los usuarios.
   - **Privado**: solo las ven los administradores de la nube. Esta configuración es útil durante la elaboración del plan o la oferta, o si el administrador de la nube desea [crear cada una de las suscripciones para los usuarios](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Retiradas**: cerradas a nuevos suscriptores. El administrador de la nube puede retirar las ofertas para impedir futuras suscripciones, pero sin que esto afecte a los suscriptores actuales.

   > [!TIP]  
   > Los cambios en la oferta no son inmediatamente visibles para el usuario. Para ver los cambios, es posible que los usuarios tengan que cerrar sesión y volver a iniciarla en el portal de usuarios para ver la nueva oferta.

   En la pantalla de información general de la oferta, seleccione **Estado de accesibilidad**. Elija el estado que desea utilizar (por ejemplo, **Público**) y, después, seleccione **Guardar**.

     ![Elegir el estado](media/azure-stack-create-offer/change-stage-1807.png)

     Como alternativa, seleccione **Cambiar estado** y, a continuación, elija un estado.

    ![Seleccionar Estado de accesibilidad](media/azure-stack-create-offer/change-stage-select-1807.png)

   > [!NOTE]
   > También puede usar PowerShell para crear ofertas, planes y cuotas predeterminados. Para más información, consulte [Azure Stack PowerShell Module 1.4.0](/powershell/azure/azure-stack/overview?view=azurestackps-1.4.0).

## <a name="next-steps"></a>Pasos siguientes

- [Creación de suscripciones](azure-stack-subscribe-plan-provision-vm.md)
- [Aprovisionamiento de una máquina virtual](azure-stack-provision-vm.md)
