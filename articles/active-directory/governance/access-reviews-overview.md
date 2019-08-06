---
title: ¿Qué son las revisiones de acceso? Azure Active Directory | Microsoft Docs
description: Con las revisiones de acceso de Azure Active Directory, puede controlar la pertenencia a grupos y el acceso a las aplicaciones para cumplir con las iniciativas de cumplimiento, de administración de riesgos y de gobernanza de su organización.
services: active-directory
documentationcenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 07/23/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: da9bc3906e6f39b2d943708eb6a1b930ac8cc5a5
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68401954"
---
# <a name="what-are-azure-ad-access-reviews"></a>¿Qué son las revisiones de acceso de Azure AD?

Las revisiones de acceso de Azure Active Directory (Azure AD) permiten a las organizaciones administrar de forma eficiente la pertenencia a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

Este es un vídeo que proporciona una introducción rápida de las revisiones de acceso:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>¿Por qué son importantes las revisiones de acceso?

Azure AD le permite colaborar internamente dentro de su organización y con usuarios de organizaciones externas, como los asociados. Los usuarios pueden unirse a grupos, invitar a otros usuarios, conectarse a aplicaciones en la nube y trabajar de forma remota desde sus dispositivos de trabajo o personales. La comodidad de aprovechar el potencial del autoservicio ha llevado a una necesidad de mejores funcionalidades de administración del acceso.

- Cuando se unen nuevos empleados, ¿cómo se asegura de que tengan el acceso adecuado para que sean productivos?
- A medida que las personas cambian de equipo o abandonan la empresa, ¿cómo se asegura de que se quite su antiguo acceso, especialmente cuando hay invitados involucrados?
- Los derechos de acceso excesivos pueden provocar malos resultados en las auditorías y riesgos, ya que indican una falta de control sobre el acceso.
- Tendrá que trabajar de manera proactiva junto a los propietarios de los recursos para asegurarse de que revisen periódicamente quién tiene acceso a sus recursos.

## <a name="when-to-use-access-reviews"></a>¿Cuándo usar revisiones de acceso?

- **Demasiados usuarios en roles con privilegios:** es recomendable comprobar cuántos usuarios tienen acceso administrativo, cuántos de ellos son administradores globales, y si hay algún invitado o asociado que no se haya quitado después de que se haya asignado una tarea administrativa. Puede volver a certificar los usuarios con asignación de roles en [roles de Azure AD](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json), por ejemplo, administradores globales, o [roles de recursos de Azure](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json), por ejemplo, administrador de acceso de usuario, en la experiencia [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md).
- **Cuando no es factible la automatización:** puede crear las reglas de pertenencia dinámica en grupos de seguridad o grupos de Office 365, pero ¿qué ocurre si los datos de recursos humanos no se encuentran en Azure AD o si los usuarios todavía necesitan acceso después de abandonar el grupo para entrenar a su sustituto? Luego puede crear una revisión en ese grupo para asegurarse de que los usuarios que aún necesiten acceso sigan teniendo acceso.
- **Cuando se usa un grupo para una nueva finalidad:** si tiene un grupo que se va a sincronizar con Azure AD, o si va a habilitar la aplicación Salesforce para todos los usuarios del equipo de Ventas, sería útil solicitar al propietario del grupo que revise la pertenencia al grupo antes de usar el grupo en un contenido de riesgo distinto.
- **Acceso a datos críticos para la empresa:** para determinados recursos y con fines de auditoría, podría exigirse a personas ajenas a TI que cierren sesión periódicamente y justifiquen por qué necesitan acceso.
- **Para mantener la lista de excepciones de la directiva:** En un mundo ideal, todos los usuarios deberían seguir las directivas de acceso para proteger el acceso a los recursos de la organización. A veces, sin embargo, hay casos empresariales en los que hay que hacer excepciones. Como administrador de TI, puede administrar esta tarea, evitar las excepciones de omisiones de la directiva y proporcionar a los auditores prueba de que estas excepciones se revisan normalmente.
- **Pedir a los propietarios de grupos que confirmen que todavía necesitan invitados en sus grupos:** el acceso de los empleados se puede automatizar con algún tipo de IAM a nivel local, pero no los usuarios invitados. Si un grupo proporciona a los invitados acceso a contenido empresarial confidencial, es responsabilidad del propietario del grupo confirmar que los invitados todavía tienen una necesidad empresarial legítima de acceso.
- **Repetir las revisiones periódicamente:** puede configurar revisiones periódicas de acceso de usuarios a frecuencias establecidas, como semanal, mensual, trimestral o anualmente, donde los revisores reciban notificaciones al principio de cada revisión. Los revisores pueden aprobar o denegar el acceso con una interfaz sencilla y con la ayuda de recomendaciones inteligentes.

## <a name="where-do-you-create-reviews"></a>¿Donde se crean las revisiones?

Dependiendo de lo que quiera revisar, creará la revisión de acceso en Revisiones de acceso de Azure AD, aplicaciones de empresa de Azure AD (en versión preliminar) o Azure AD PIM.

| Derechos de acceso de los usuarios | Los revisores pueden ser | Revisión creada en | Experiencia del revisor |
| --- | --- | --- | --- |
| Miembros del grupo de seguridad</br>Miembros del grupo de Office | Revisores especificados</br>Propietarios del grupo</br>Autorrevisión | Revisiones de acceso de Azure AD</br>Grupos de Azure AD | Panel de acceso |
| Asignados a una aplicación conectada | Revisores especificados</br>Autorrevisión | Revisiones de acceso de Azure AD</br>Aplicaciones de empresa de Azure AD (en versión preliminar) | Panel de acceso |
| Rol de Azure AD | Revisores especificados</br>Autorrevisión | [Azure AD PIM](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Portal de Azure |
| Rol de recursos de Azure | Revisores especificados</br>Autorrevisión | [Azure AD PIM](../privileged-identity-management/pim-resource-roles-start-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Portal de Azure |

## <a name="which-users-must-have-licenses"></a>¿Qué usuarios deben tener licencias?

Cada usuario que interactúa con las revisiones de acceso debe tener una licencia de pago de Azure AD Premium P2. Algunos ejemplos son:

- Administradores que crean una revisión de acceso
- Propietarios de grupos que realizan una revisión de acceso
- Usuarios asignados como revisores
- Usuarios que realizan una autorrevisión

También puede solicitar a los usuarios invitados que revisen su propio acceso. Por cada licencia de Azure AD Premium P2 de pago que asigne a uno de los usuarios de la organización, puede usar la colaboración negocio a negocio (B2B) Azure AD para invitar hasta a cinco usuarios, gracias a la concesión de usuarios externos. Los usuarios invitados también pueden usar las características de Azure AD Premium P2. Para más información, consulte [Guía de concesión de licencias de colaboración B2B de Azure Active Directory](../b2b/licensing-guidance.md).

Estos son algunos escenarios de ejemplo que le ayudarán a determinar el número de licencias que debe tener.

| Escenario | Cálculo | Número necesario de licencias |
| --- | --- | --- |
| Un administrador crea una revisión de acceso al grupo A con 500 usuarios.<br/>Asigna a tres propietarios del grupo como revisores. | Un administrador y tres propietarios de grupos | 4 |
| Un administrador crea una revisión de acceso al grupo A con 500 usuarios.<br/>Realiza una autorrevisión. | Un administrador y 500 usuarios como autorrevisores | 501 |
| Un administrador crea una revisión de acceso de un grupo con cinco usuarios y 25 usuarios invitados.<br/>Realiza una autorrevisión. | Un administrador y cinco usuarios como revisores automática<br/>(los usuarios invitados están cubiertos en la proporción requerida de 1:5) | 6 |
| Un administrador crea una revisión de acceso de un grupo con cinco usuarios y 28 usuarios invitados.<br/>Realiza una autorrevisión. | Un administrador y cinco usuarios como autorrevisores más un usuario para cubrir los usuarios invitados en la proporción 1:5 requerida | 7 |

Para más información acerca de cómo asignar licencias a los usuarios, consulte [Asignación o eliminación de licencias mediante el portal de Azure Active Directory](../fundamentals/license-users-groups.md).

## <a name="onboard-access-reviews"></a>Incorporarse a las revisiones de acceso

Para incorporar las revisiones de acceso, siga estos pasos.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) donde quiera usar las revisiones de acceso, como administrador global o administrador de usuarios.

1. En el panel de navegación izquierdo, haga clic en **Azure Active Directory**.

1. En el menú izquierdo, haga clic en **Gobierno de identidades**.

1. Haga clic en **Revisiones de acceso**.
 
    ![Página de inicio de revisiones de acceso](./media/access-reviews-overview/access-reviews-overview-onboard.png)

1. En la página, haga clic en el botón **Incorporar ahora**.
    
      ![Incorporar revisiones de acceso](./media/access-reviews-overview/access-reviews-overview-select-onboard.png)


## <a name="learn-about-access-reviews"></a>Más información sobre las revisiones de acceso

Para obtener más información sobre cómo crear y realizar las revisiones de acceso, vea esta breve demostración:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Si está listo para implementar revisiones de acceso en su organización, siga estos pasos en el vídeo para incorporar y entrenar a los administradores, y crear su primera revisión de acceso.

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="license-requirements"></a>Requisitos de licencia

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una revisión de acceso de grupos o aplicaciones](create-access-review.md)
- [Creación de una revisión de acceso de los usuarios en un rol administrativo de Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Revisión del acceso a grupos o aplicaciones](perform-access-review.md)
- [Completar una revisión de acceso de grupos o aplicaciones](complete-access-review.md)
