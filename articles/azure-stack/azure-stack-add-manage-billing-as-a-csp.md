---
title: Administración del uso y la facturación para Azure Stack como un proveedor de servicios en la nube | Microsoft Docs
description: Tutorial sobre cómo registrar Azure Stack como proveedor en la nube (CSP) y cómo agregar clientes para la facturación.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: sethm
ms.reviewer: alfredo
ms.openlocfilehash: e65f85d8349c4c8c69a50e93b093c04250e73a4e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52961472"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Administración del uso y la facturación para Azure Stack como un proveedor de servicios en la nube 

*Se aplica a: Sistemas integrados de Azure Stack*

Este artículo le guía por el registro de Azure Stack como proveedor en la nube (CSP) y la adición de clientes.

Como un CSP, trabaja con diversos clientes mediante su Azure Stack. Cada cliente tiene una suscripción de CSP en Azure. Tendrá que dirigir el uso de su Azure Stack a cada suscripción de usuario.

El siguiente diagrama muestra los pasos que necesita para elegir su cuenta de servicios compartidos y registrar la cuenta de Azure con la cuenta de Azure Stack. Cuando haya completado el registro, podrá incorporar los clientes finales.

**Pasos para agregar seguimiento de uso como un CSP**

[ ![Proceso para habilitar el uso y administración como Proveedor de servicios en la nube](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png "Proceso para habilitar el uso y administración como Proveedor de servicios en la nube")](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png#lightbox)

## <a name="create-a-csp-or-apss-subscription"></a>Creación de una suscripción de CSP o APSS

### <a name="cloud-service-provider-subscription-types"></a>Tipos de suscripción del proveedor de servicios en la nube

Tendrá que elegir el tipo de cuenta de servicios compartidos que utiliza para Azure Stack. Los tipos de suscripción que se pueden usar para el registro de una instancia multiinquilino de Azure Stack son los siguientes:

 - Proveedor de servicios en la nube 
 - Suscripción de servicios compartidos de asociados 

#### <a name="azure-partner-shared-services"></a>Servicios compartidos para asociados de Azure

Las suscripciones de los servicios compartidos para asociados de Azure (APSS) son la opción preferida para el registro cuando un distribuidor de CSP o CSP directo opera en Azure Stack.

Las suscripciones de APSS están asociadas a un inquilino de servicios compartidos. Cuando se registra Azure Stack, hay que proporcionar credenciales para una cuenta que es propietaria de la suscripción. La cuenta que usa para registrar Azure Stack puede ser diferente de la cuenta de administrador que usa para la implementación. Además, las dos *no* tienen que pertenecer al mismo dominio. En otras palabras, puede realizar la implementación mediante el inquilino que ya usa. Por ejemplo puede usar ContosoCSP.onmicrosoft.com y, después, registrarse con un inquilino diferente, por ejemplo IURContosoCSP.onmicrosoft.com. Deberá recordar que debe iniciar sesión con ContosoCSP.onmicrosoft.com cuando realice la administración diaria de Azure Stack. Cuando inicia sesión en Azure usando IURContosoCSP.onmicrosoft.com cuando necesita hacer operaciones de registro.

Consulte el siguiente vínculo para obtener una descripción de las suscripciones de APSS, así como instrucciones sobre cómo crear una suscripción en [Adición de servicios compartidos para asociados de Azure](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>Suscripciones de CSP

Las suscripciones de un proveedor de servicio en la nube (CSP) son la opción preferida para el registro cuando un revendedor de CSP o CSP directo opera en Azure Stack.

## <a name="register-azure-stack"></a>Registro de Azure Stack

Use la suscripción de APSS creada con la información de la sección anterior para registrar Azure Stack con Azure. Para obtener más información, consulte [Registro de Azure Stack con una suscripción de Azure](azure-stack-registration.md).

## <a name="add-end-customer"></a>Adición a un cliente final

Para configurar Azure Stack de modo que cuando un nuevo inquilino utilice recursos su uso se notifique a su suscripción de un proveedor de servicios en la nube(CSP), consulte [Adición de inquilinos en Azure Stack para uso y facturación](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Carga de las suscripciones correctas

Azure Stack utiliza una característica denominada registro. Un registro es un objeto almacenado en Azure. El objeto de registro documenta qué suscripciones de Azure hay que usar para cobrar por una instancia determinada de Azure Stack. En esta sección se explica la importancia del registro.

Con el uso del registro de Azure Stack, se puede:
 - Desviar los datos de uso de Azure Stack a Azure Commerce y facture una suscripción a Azure.
 - Notificar el uso de cada cliente en una suscripción diferente con una implementación multiinquilino de Azure Stack. La arquitectura multiinquilino permite que Azure Stack admita distintas organizaciones en la misma instancia de Azure Stack.

Para cada instancia de Azure Stack, hay una suscripción predeterminada y varias suscripciones de inquilinos. La suscripción predeterminada es una suscripción de Azure que se cobra si no hay una suscripción específica de inquilino. Debe ser la primera suscripción registrada. Para que los informes de uso multiinquilinos funcionen, la suscripción debe ser un plan CSP o APSS.

Después, el registro se actualiza con una suscripción de Azure para cada inquilino que vaya a usar Azure Stack. Las suscripciones de inquilinos deben ser de tipo CSP y deben trasferirse al asociado que posee la suscripción predeterminada. En otras palabras, no puede registrar los clientes de otra persona.

Cuando Azure Stack envía información de uso a Azure global, un servicio de Azure consulta el registro y asigna el uso de cada inquilino a la suscripción del inquilino correspondiente. Si no se ha registrado un inquilino, ese uso se transfiere a la suscripción predeterminada para la instancia Azure Stack de la que se originó.

Dado que las suscripciones de inquilinos son suscripciones de CSP, se enviará la factura al asociado de CSP y la información de uso no será visible para el cliente final.

## <a name="next-steps"></a>Pasos siguientes

 - Para más información acerca del programa CSP, consulte [Programa Proveedor de soluciones en la nube](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
 - Para más información sobre cómo recuperar la información de utilización de recursos de Azure Stack, consulte [Uso y facturación en Azure Stack](azure-stack-billing-and-chargeback.md).
