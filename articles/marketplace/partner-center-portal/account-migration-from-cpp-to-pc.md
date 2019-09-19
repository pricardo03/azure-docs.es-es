---
title: 'Migración de cuentas de Cloud Partner Portal al centro de partners: Marketplace comercial para Azure'
description: Cómo migrar su cuenta de CPP al centro de partners. - Marketplace comercial para Azure
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 8c7680896507cd3738fa3bce0d30a516d08509c4
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383286"
---
# <a name="account-migration-from-cloud-partner-portal-to-partner-center"></a>Migración de cuentas de Cloud Partner Portal al centro de partners

Al migrar las ofertas desde Cloud Partner Portal (CPP) hasta el Centro de partners (PC), quedan bloqueadas de la edición en CPP. En este momento, la configuración de la cuenta debe migrarse al centro de partners. Tanto la configuración de la cuenta como las ofertas se pueden administrar en el centro de partners.

## <a name="account-migration-process"></a>Proceso de migración de cuentas

Al migrar ofertas desde CPP, la cuenta se configura para el proceso. 
 
Si es un usuario con el rol de propietario en CPP para una cuenta determinada, se muestra un banner en amarillo en la página Perfil de publicación. Se le pedirá que mueva la configuración de su cuenta al Centro de partners. 

Haga clic en el banner para iniciar el proceso de migración de la cuenta. Deberá escribir los siguientes elementos:

### <a name="work-email-address"></a>Dirección de correo electrónico del trabajo

La mayoría de las veces se inicia sesión con la dirección de correo electrónico que se usa para iniciar sesión en CPP. En algunos casos, se debe usar una dirección de correo electrónico diferente:

* Cuenta de Microsoft: si la cuenta de CPP es una cuenta Microsoft, deberá especificar una dirección de correo electrónico del trabajo válida asociada al inquilino para el que está registrado el identificador de MPN.

* El inquilino no coincide: si su dirección de correo electrónico del trabajo no pertenece al inquilino asociado con el identificador de Microsoft Partner Network de la cuenta de CPP, se producirá un error. Para saltarse este error, escriba una dirección de correo electrónico asociada con el inquilino. En un mensaje de error se proporcionará el nombre del inquilino.

### <a name="sign-up-for-microsoft-partner-network-program"></a>Registro en el programa Microsoft Partner Network

En caso de que la cuenta de CPP no tenga un identificador de Microsoft Partner Network o si lo tiene, pero no es válido, debe registrarse en el programa Microsoft Partner Network como parte del proceso de activación.

## <a name="after-account-migration-is-complete"></a>Una vez finalizada la migración de la cuenta

La migración solo se puede realizar una vez por cuenta. Cuando un asociado determinado haya completado la migración de la cuenta, todos los propietarios verán este comportamiento en la página Perfil de publicación:

1. Verá la página Configuración de partners en Microsoft Partner Network, donde ahora puede administrar la configuración de la cuenta. 
2. Se mostrará un banner amarillo en la página del perfil del publicador en CPP a los usuarios que tengan el rol de propietario; en él se les pide que administren la configuración de su cuenta en el Centro de partners.
3. La página de configuración de la cuenta de CPP pasa a modo de solo lectura.

## <a name="move-dynamics-365-based-solutions-to-partner-center"></a>Traslado de soluciones basadas en Dynamics 365 al Centro de partners

Si ha creado soluciones de Dynamics 365 for Customer Engagement o Dynamics 365 for Finance and Operations en el portal de One Commercial Partner GTM, **estas soluciones se administrarán ahora en el Centro de partners**.

**Si no ha movido las soluciones antes del 31 de agosto de 2019**, complete los pasos siguientes tan pronto como sea posible. Hasta que lo haga:
- Los ISV no tendrán acceso a las ventajas de marketing
- La prioridad de la venta conjunta perderá su estado
- Los que requieran Cloud Embed estarán fuera de cumplimiento después del 15 de octubre de 2019

> [!NOTE]
> Si la cuenta de pertenencia de MPN se creó originalmente en Partner Membership Center (PMC), inicie sesión en el [Centro de partners](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile) para confirmar que la cuenta se ha migrado antes de completar los pasos siguientes. Si ve una pantalla de perfil con el identificador de MPN, significa que ya está listo para continuar. Si no es así, debe iniciar la migración de la cuenta siguiendo las indicaciones de [Partner Membership Center](https://partners.microsoft.com/partnerprogram/Welcome.aspx). Consulte[Soporte técnico](https://partner.microsoft.com/support?issueid=100-0077) si necesita ayuda con este paso.

1. Vaya a [Introducción al portal de Marketplace comercial en el Centro de partners](https://partner.microsoft.com/dashboard/commercial-marketplace/overview). Si ve "Marketplace comercial" en el panel de navegación izquierdo, significa que está inscrito y debe continuar con el paso siguiente. Si no es así, [inscríbase ahora en el Marketplace comercial](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/azureisv).
2. Confirme que sus ofertas están en AppSource. Para ello, [busque sus ofertas](https://appsource.microsoft.com/). Si las ofertas ya están en AppSource, continúe con el paso siguiente. En el caso de las ofertas que no estén en AppSource, cree una [nueva oferta de Dynamics 365 for Customer Engagement](create-new-customer-engagement-offer.md) o una [nueva oferta de Dynamics 365 for Operations](create-new-operations-offer.md).
3. Compruebe su inscripción en el programa Connect para ISV de Microsoft Business Applications:
  
   * En la página de [acuerdos](https://partner.microsoft.com/dashboard/account/agreements) del Centro de partners, asegúrese de que ha aceptado **anexo de ISV de Business Applications** para inscribirse en el programa.
   * En la página [Configuración de la cuenta](https://partner.microsoft.com/dashboard/account/v3/accountsettings/billingprofile), proporcione la información de facturación.

4. Envíe cada oferta nueva y existente de certificación, incluso si las ofertas se han certificado previamente. Si es apto, puede solicitar participar en el nivel premium durante este proceso. Si su oferta se ha certificado anteriormente, **debe completar la certificación de la aplicación antes del 15 de octubre de 2019.** La certificación o la recertificación requerirán que la aplicación sea compatible con la versión más reciente de nuestra plataforma Business Applications.
5. Vaya al [portal de One Commercial Partner GTM](https://msgtm.azurewebsites.net/en-US/Profile/SignIn) y agregue la dirección URL de la lista de AppSource en la sección de vínculos de Marketplace. Si necesita ayuda con este paso, envíenos un correo electrónico a cosell@microsoft.com.

## <a name="next-steps"></a>Pasos siguientes

- [Administración de la cuenta de Marketplace comercial en el Centro de partners](./manage-account.md) 
