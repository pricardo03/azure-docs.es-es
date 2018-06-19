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
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a84148a3ac31d51ff30cebffab00e5fec8fdaa87
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35237003"
---
# <a name="azure-stack-add-on-plans"></a>Planes complementarios de Azure Stack
Como operador de Azure Stack, va a crear planes que contienen los servicios deseados y las cuotas aplicables para que sus usuarios se suscriban. Estos [*planes base*](azure-stack-create-plan.md) contienen los servicios básicos que se ofrecerán a los usuarios y solo se puede tener un plan base por oferta. Si necesita modificar su oferta, puede utilizar *planes complementarios* que le permitan modificar el plan para ampliar las cuotas de equipo, almacenamiento o red ofrecidas inicialmente con el plan base. 

Aunque combinar todo en un solo plan puede ser adecuado en algunos casos, es posible que desee tener un plan base y ofrecer servicios adicionales utilizando planes complementarios. Por ejemplo, podría decidir ofrecer servicios IaaS como parte de un plan base, con todos los servicios PaaS tratados como planes complementarios. Los planes también se pueden utilizar para controlar el consumo de recursos en su entorno de Azure Stack. Por ejemplo, si desea que los usuarios sean conscientes de su uso de recursos, podría tener un plan base relativamente pequeño (dependiendo de los servicios requeridos) y a medida que los usuarios alcancen la capacidad, se les avisaría de que ya han consumido la asignación de recursos basada en su plan asignado. Desde allí, los usuarios pueden seleccionar un plan complementario disponible para recursos adicionales. 

> [!NOTE]
> Cuando un usuario agrega un plan complementario a una suscripción de oferta existente, los recursos adicionales pueden tardar hasta una hora en aparecer. 

## <a name="create-an-add-on-plan"></a>Crear un plan complementario
Los planes complementarios se crean al modificar una oferta existente:

1. Inicie sesión en el portal de administrador de Azure Stack como administrador de nube.
2. Siga los mismos pasos utilizados para [crear un plan base](azure-stack-create-plan.md) para crear un nuevo plan que ofrezca servicios que no se ofrecían anteriormente. En este ejemplo, los servicios de Key Vault (Microsoft.KeyVault) se incluirán en el nuevo plan.
3. En el portal de administrador, haga clic en **Ofertas** y, a continuación, seleccione la oferta que desea actualizar con un plan complementario.

   ![](media/create-add-on-plan/1.PNG)

4.  Desplácese hasta el final de las propiedades de la oferta y seleccione **Planes complementarios**. Haga clic en **Agregar**.
   
    ![](media/create-add-on-plan/2.PNG)

5. Seleccione el plan que se va a agregar. En este ejemplo, el plan se denomina **Key vault plan** (Plan de almacén de claves) y, después, haga clic en **Seleccionar** para agregar el plan a la oferta. Debería recibir una notificación de que el plan se agregó correctamente a la oferta.
   
    ![](media/create-add-on-plan/3.PNG)

6. Revise la lista de planes complementarios incluidos con la oferta para comprobar que el nuevo plan complementario aparezca en la lista.
   
    ![](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Creación de una oferta](azure-stack-create-offer.md)