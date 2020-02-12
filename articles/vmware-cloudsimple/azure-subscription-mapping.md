---
title: Creación de grupos de recursos con asignación de suscripciones de Azure
description: En este artículo se describe cómo crear grupos de recursos para la nube privada de AVS a través de la asignación de suscripciones de Azure.
titleSuffix: Azure VMware Solution by CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ab7549650e4e20d27d3ad11a96d77ba943797f88
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014970"
---
# <a name="create-resource-pools-for-your-avs-private-cloud-with-azure-subscription-mapping"></a>Creación de grupos de recursos para la nube privada de AVS con la asignación de suscripciones de Azure
La asignación de suscripciones de Azure le permite crear grupos de recursos para la nube privada de AVS a partir de los grupos de recursos de vSphere disponibles. En el portal de AVS, puede ver y administrar la suscripción de Azure para las nubes privadas de AVS.

> [!NOTE]
> Cuando se asigna un grupo de recursos, también se asignan los grupos de recursos secundarios. No se puede asignar un grupo de recursos primario si los grupos de recursos secundarios ya están asignados.

1. [Acceda al portal de AVS](access-cloudsimple-portal.md).
2. Abra la página **Recursos** y seleccione **Azure subscriptions mapping** (Asignación de suscripciones de Azure).  
3. Seleccione **Edit Azure subscription mapping** (Editar la asignación de suscripciones de Azure).  
4. Para asignar los grupos de recursos disponibles, selecciónelos a la izquierda y haga clic en la flecha que apunta hacia la derecha. 
5. Para quitar asignaciones, selecciónelas a la derecha y haga clic en la flecha que apunta hacia la izquierda. 

    ![Suscripciones de Azure](media/resources-azure-mapping.png)

6. Haga clic en **OK**.
