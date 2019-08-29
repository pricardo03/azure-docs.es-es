---
title: Creación de grupos de recursos con asignación de suscripciones de Azure
description: En este artículo se describe cómo crear grupos de recursos para la nube privada a través de la asignación de suscripciones de Azure.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3ea102ad8377da70b88a0e59834ebe3a09866632
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69563220"
---
# <a name="create-resource-pools-for-your-private-cloud-with-azure-subscription-mapping"></a>Creación de grupos de recursos para la nube privada con la asignación de suscripciones de Azure
La asignación de suscripciones de Azure le permite crear grupos de recursos para la nube privada a partir de los grupos de recursos de vSphere disponibles. En el portal de CloudSimple, puede ver y administrar la suscripción de Azure para las nubes privadas.

> [!NOTE]
> Cuando se asigna un grupo de recursos, también se asignan los grupos de recursos secundarios. No se puede asignar un grupo de recursos primario si los grupos de recursos secundarios ya están asignados.

1. [Acceda al portal de CloudSimple](access-cloudsimple-portal.md).
2. Abra la página **Recursos** y seleccione **Azure subscriptions mapping** (Asignación de suscripciones de Azure).  
3. Seleccione **Edit Azure subscription mapping** (Editar la asignación de suscripciones de Azure).  
4. Para asignar los grupos de recursos disponibles, selecciónelos a la izquierda y haga clic en la flecha que apunta hacia la derecha. 
5. Para quitar asignaciones, selecciónelas a la derecha y haga clic en la flecha que apunta hacia la izquierda. 

    ![Suscripciones de Azure](media/resources-azure-mapping.png)

6. Haga clic en **OK**.
