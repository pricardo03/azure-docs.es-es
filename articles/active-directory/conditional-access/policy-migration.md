---
title: ¿Qué es una migración de directivas en el acceso condicional de Azure Active Directory? | Microsoft Docs
description: Aprenda todo lo necesario sobre cómo migrar las directivas clásicas en Azure Portal.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/24/2018
ms.author: joflore
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3246a7efa36e6c6ef0dac93e659a73dac17090a
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58516882"
---
# <a name="what-is-a-policy-migration-in-azure-active-directory-conditional-access"></a>¿Qué es una migración de directivas en el acceso condicional de Azure Active Directory? 


El [acceso condicional](../active-directory-conditional-access-azure-portal.md) es una funcionalidad de Azure Active Directory (Azure AD) que le permite controlar cómo acceden los usuarios autorizados a las aplicaciones en la nube. Aunque el propósito sigue siendo el mismo, la nueva versión de Azure Portal presenta mejoras importantes acerca de cómo funciona el acceso condicional.

Debería considerar la posibilidad de migrar las directivas que no haya creado en Azure portal porque:

- Ahora puede resolver situaciones que antes no podía controlar.

- Puede reducir el número de directivas que tiene que administrar mediante su consolidación.   

- Puede administrar todas las directivas de acceso condicional en una ubicación central.

- Se retirará el Portal de Azure clásico.   

En este artículo se explica lo que necesita saber para migrar las directivas de acceso condicional existentes al marco nuevo.
 
## <a name="classic-policies"></a>Directivas clásicas

En [Azure Portal](https://portal.azure.com), la página [ Acceso condicional - Directivas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) es el punto de entrada a las directivas de acceso condicional. Sin embargo, en su entorno, también podría tener directivas de acceso condicional que no creara con esta página. Estas directivas se conocen como *directivas clásicas*. Las directivas de clásicas son directivas de acceso condicional que creó con:

- El Portal de Azure clásico
- El Portal de Intune clásico
- El Portal de Intune App Protection


En la página **Acceso condicional**, puede tener acceso a las directivas clásicas si hace clic en [ **Directivas clásicas (versión preliminar)**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) en la sección **Administrar**. 


![Azure Active Directory](./media/policy-migration/71.png)


La vista **Directivas clásicas** le permite:

- Filtrar las directivas clásicas.
 
    ![Azure Active Directory](./media/policy-migration/72.png)

- Deshabilitar las directivas clásicas.

    ![Azure Active Directory](./media/policy-migration/73.png)
   
- Revisar la configuración de las directivas clásicas (y desactivarla).

    ![Azure Active Directory](./media/policy-migration/74.png)


Si deshabilitó una directiva clásica, ya no podrá revertir ese paso. Por ese motivo puede modificar la pertenencia a un grupo en una directiva clásica mediante la vista **Detalles**. 

![Azure Active Directory](./media/policy-migration/75.png)

Si cambia los grupos seleccionados o excluye grupos específicos, puede probar el efecto de una directiva clásica deshabilitada para algunos usuarios de prueba antes de deshabilitar la directiva para todos los usuarios y grupos. 



## <a name="azure-ad-conditional-access-policies"></a>Directivas de acceso condicional de Azure AD

Puede administrar todas las directivas en una ubicación central mediante el acceso condicional en Azure Portal. Dado que la implementación sobre cómo funciona el acceso condicional ha cambiado considerablemente, debe familiarizarse con los conceptos básicos antes de migrar las directivas clásicas.

Consulte:

- [Qué es el acceso condicional en Azure Active Directory](../active-directory-conditional-access-azure-portal.md) para información sobre la terminología y los conceptos básicos.

- [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md) para obtener información sobre la implementación de acceso condicional en su organización.

- [Requerir MGA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md) para familiarizarse con la interfaz de usuario de Azure Portal.


 
## <a name="migration-considerations"></a>Consideraciones sobre la migración

En este artículo, las directivas de acceso condicional de Azure AD también se conocen como *directivas nuevas*.
Las directivas clásicas seguirán funcionando con las directivas nuevas hasta que las deshabilite o las elimine. 

Los aspectos siguientes son importantes en el contexto de una consolidación de directiva:

- Aunque las directivas clásicas están asociadas a una aplicación de nube específica, puede seleccionar tantas como necesite en una directiva nueva.

- Los controles de una directiva clásica y una directiva nueva para una aplicación de nube requieren que se cumplan todos ellos (*AND*). 


- En una directiva nueva puede:
 
    - Combinar varias condiciones, si la situación lo requiere. 

    - Seleccionar varios requisitos de concesión como control de acceso y combinarlos con un valor lógico *OR* (se requiere uno de los controles seleccionados) o con un valor lógico *AND* (se requieren todos los controles seleccionados).

        ![Azure Active Directory](./media/policy-migration/25.png)




### <a name="office-365-exchange-online"></a>Office 365 Exchange Online

Si quiere migrar las directivas clásicas para **Office 365 Exchange Online** que incluyen **Exchange Active Sync** como condición de aplicaciones de cliente, es posible que no pueda consolidarlas en una directiva nueva. 

Esto sucede en caso de que quiera admitir todos los tipos de aplicación de cliente. En una directiva nueva que tiene **Exchange Active Sync** como condición de aplicaciones de cliente no puede seleccionar otras aplicaciones cliente.

![Azure Active Directory](./media/policy-migration/64.png)

Tampoco es posible una consolidación en una directiva nueva si las directivas clásicas contienen varias condiciones. Una directiva nueva que tiene **Exchange Active Sync** como condición de aplicaciones de cliente no admite otras condiciones:   

![Azure Active Directory](./media/policy-migration/08.png)

Si existe una directiva nueva que tiene **Exchange Active Sync** como condición de aplicaciones de cliente configurada, debe asegurarse de que todas las demás condiciones no están configuradas. 

![Azure Active Directory](./media/policy-migration/16.png)
 

Las directivas clásicas [basadas en aplicaciones](technical-reference.md#approved-client-app-requirement) para Office 365 Exchange Online que incluyen **Exchange Active Sync** como condición de aplicaciones de cliente permiten [plataformas de dispositivos](technical-reference.md#device-platform-condition) **admitidas** y **no admitidas**. Aunque no se pueda configurar plataformas de dispositivos individuales en una directiva nueva relacionada, puede limitar la compatibilidad solo para [plataformas de dispositivos compatibles](technical-reference.md#device-platform-condition). 

![Azure Active Directory](./media/policy-migration/65.png)

Puede consolidar varias directivas clásicas que incluya **Exchange Active Sync** como condición de aplicaciones de cliente si tienen:

- **Exchange Active Sync** como única condición 

- Varios requisitos configurados para la concesión de acceso

Una situación habitual es la consolidación de:

- Una directiva clásica basada en dispositivos desde el Portal de Azure clásico 
- Una directiva clásica basada aplicaciones desde el Portal de Intune App Protection 
 
En este caso, puede consolidar las directivas clásicas en una nueva directiva que tenga seleccionados ambos requisitos.

![Azure Active Directory](./media/policy-migration/62.png)



### <a name="device-platforms"></a>Plataformas de dispositivo

Las directivas de clásicas con [controles basados en aplicaciones](technical-reference.md#approved-client-app-requirement) están preconfiguradas con iOS y Android como la [condición de la plataforma del dispositivo](technical-reference.md#device-platform-condition). 

En una directiva nueva, debe seleccionar las [plataformas de dispositivo](technical-reference.md#device-platform-condition) que quiera admitir de forma individual.

![Azure Active Directory](./media/policy-migration/41.png)



 
 


## <a name="next-steps"></a>Pasos siguientes

- Si quiere saber cómo configurar una directiva de acceso condicional, consulte [Requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md).

- Si está listo para configurar directivas de acceso condicional para su entorno, consulte [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md). 
