---
title: En este artículo, aprenderá a actualizar ofertas y planes de Azure Stack | Microsoft Docs
description: En este artículo se describe cómo ver y modificar ofertas y planes de Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
ms.date: 07/30/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a35ba993e6fd1162fa4a18bc0d6bc9351fe7dfa2
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358297"
---
# <a name="azure-stack-add-on-plans"></a>Planes complementarios de Azure Stack

Como operador de Azure Stack, puede crear planes complementarios para modificar un [*plan base*](azure-stack-create-plan.md) cuando desee ofrecer servicios adicionales o ampliar las cuotas de *equipo*, *almacenamiento*  o *red* más allá de la oferta inicial de los planes base. Los planes complementarios modifican el plan base y son extensiones opcionales a las que los usuarios pueden elegir suscribirse. 

Hay ocasiones en las que combinar todo en un único plan es la solución óptima. En otras ocasiones puede interesarle más tener un plan base y ofrecer los servicios adicionales utilizando planes complementarios. Por ejemplo, podría decidir ofrecer servicios IaaS como parte de un plan base, con todos los servicios PaaS tratados como planes complementarios.

Otra razón para usar los planes complementarios es ayudar a los usuarios a que tengan conciencia del uso que hacen de los recursos. Para ello, puede empezar con un plan base que incluye cuotas relativamente pequeñas (dependiendo de los servicios requeridos). Después, a medida que los usuarios vayan utilizando la capacidad disponible, se les avisaría de que han consumido la asignación de recursos en función a su plan asignado. A partir de ese momento, los usuarios pueden seleccionar un plan complementario que proporcione los recursos adicionales.

> [!NOTE]
> Cuando no desee usar un plan complementario para ampliar una cuota, también puede elegir a [editar la configuración original de la cuota](azure-stack-quota-types.md#to-edit-a-quota). 

Cuando un usuario agrega un plan complementario a una suscripción de oferta existente, los recursos adicionales pueden tardar hasta una hora en aparecer. 

## <a name="create-an-add-on-plan"></a>Crear un plan complementario
Los planes complementarios se crean al modificar una oferta existente:

1. Inicie sesión en el portal de administrador de Azure Stack como administrador de nube.
2. Siga los mismos pasos utilizados para [crear un plan base](azure-stack-create-plan.md) para crear un nuevo plan que ofrezca servicios que no se ofrecían anteriormente. En este ejemplo, los servicios de Key Vault (Microsoft.KeyVault) se incluirán en el nuevo plan.
3. En el portal de administrador, haga clic en **Ofertas** y, a continuación, seleccione la oferta que desea actualizar con un plan complementario.

   ![](media/create-add-on-plan/1.PNG)

4.  Desplácese hasta el final de las propiedades de la oferta y seleccione **Planes complementarios**. Haga clic en **Agregar**.
   
    ![](media/create-add-on-plan/2.PNG)

5. Seleccione el plan que se va a agregar. En este ejemplo, el plan se denomina **Key vault plan** (Plan de almacén de claves). Después de seleccionar el plan, haga clic en **Seleccionar** para agregar el plan a la oferta. Debería recibir una notificación de que el plan se agregó correctamente a la oferta.
   
    ![](media/create-add-on-plan/3.PNG)

6. Revise la lista de planes complementarios incluidos con la oferta para comprobar que el nuevo plan complementario aparezca en la lista.
   
    ![](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Creación de una oferta](azure-stack-create-offer.md)