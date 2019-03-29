---
title: ¿Qué son las revisiones de acceso? Azure Active Directory | Microsoft Docs
description: Con las revisiones de acceso de Azure Active Directory, puede controlar el acceso de suscripción y de aplicación de grupo para cumplir con las iniciativas de cumplimiento de su organización, administración de riesgos y gobierno.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 01/18/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1563a023f397999deb5c6abd40843d6a376b0492
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576129"
---
# <a name="what-are-azure-ad-access-reviews"></a>¿Qué acceso de Azure AD de revisiones?

Revisiones de acceso de Azure Active Directory (Azure AD) permiten a las organizaciones a administrar las pertenencias a grupos de forma eficaz, el acceso a aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

Este es un vídeo que proporciona una introducción rápida de las revisiones de acceso:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>¿Por qué son importantes las revisiones de acceso?

Azure AD le permite colaborar internamente dentro de su organización y con usuarios de organizaciones externas, como los asociados. Los usuarios pueden unirse a grupos, invitar a otros usuarios, conectarse a aplicaciones en la nube y trabajar de forma remota desde sus dispositivos de trabajo o personales. La comodidad de aprovechar el potencial del autoservicio ha llevado a una necesidad de mejores funcionalidades de administración del acceso.

- Cuando se unen nuevos empleados, ¿cómo se asegura de que tengan el acceso adecuado para que sean productivos?
- A medida que las personas cambian de equipo o abandonan la empresa, ¿cómo se asegura de que se quite su antiguo acceso, especialmente cuando hay invitados involucrados?
- Los derechos de acceso excesivos pueden provocar malos resultados en las auditorías y riesgos, ya que indican una falta de control sobre el acceso.
- Tendrá que trabajar de manera proactiva junto a los propietarios de los recursos para asegurarse de que revisen periódicamente quién tiene acceso a sus recursos.

## <a name="when-to-use-access-reviews"></a>¿Cuándo usar revisiones de acceso?

- **Demasiados usuarios en roles con privilegios:** Es una buena idea comprobar cuántos usuarios tiene acceso administrativo, ¿cuántos de ellos son los administradores globales, y si hay alguna invitó invitados o socios que no se han quitado después de que se asigna para realizar una tarea administrativa. Puede volver a certificar los usuarios de la asignación de rol de [roles de Azure AD](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) como los administradores globales, o [roles de recursos de Azure](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) como administrador de acceso de usuario en el [con privilegios de Azure AD Identity Management (PIM)](../privileged-identity-management/pim-configure.md) experimentar.
- **Cuando no es factible la automatización:** puede crear las reglas de pertenencia dinámica en grupos de seguridad o grupos de Office 365, pero ¿qué ocurre si los datos de recursos humanos no se encuentran en Azure AD o si los usuarios todavía necesitan acceso después de abandonar el grupo para entrenar a su sustituto? Luego puede crear una revisión en ese grupo para asegurarse de que los usuarios que aún necesiten acceso sigan teniendo acceso.
- **Cuando se usa un grupo para una nueva finalidad:** si tiene un grupo que se va a sincronizar con Azure AD, o si va a habilitar la aplicación Salesforce para todos los usuarios del equipo de Ventas, sería útil solicitar al propietario del grupo que revise la pertenencia al grupo antes de usar el grupo en un contenido de riesgo distinto.
- **Acceso a datos críticos para la empresa:** para determinados recursos y con fines de auditoría, podría exigirse a personas ajenas a TI que cierren sesión periódicamente y justifiquen por qué necesitan acceso.
- **Para mantener la lista de excepciones de la directiva:** En un mundo ideal, todos los usuarios deberían seguir las directivas de acceso para proteger el acceso a los recursos de la organización. A veces, sin embargo, hay casos empresariales en los que hay que hacer excepciones. Como administrador de TI, puede administrar esta tarea, evitar las excepciones de omisiones de la directiva y proporcionar a los auditores prueba de que estas excepciones se revisan normalmente.
- **Pedir a los propietarios de grupos que confirmen que todavía necesitan invitados en sus grupos:** Acceso de los empleados se puede automatizar con algunos local IAM, pero no los usuarios invitados. Si un grupo proporciona a los invitados acceso a contenido empresarial confidencial, es responsabilidad del propietario del grupo confirmar que los invitados todavía tienen una necesidad empresarial legítima de acceso.
- **Repetir las revisiones periódicamente:** puede configurar revisiones periódicas de acceso de usuarios a frecuencias establecidas, como semanal, mensual, trimestral o anualmente, donde los revisores reciban notificaciones al principio de cada revisión. Los revisores pueden aprobar o denegar el acceso con una interfaz sencilla y con la ayuda de recomendaciones inteligentes.

## <a name="where-do-you-create-reviews"></a>¿Donde se crean las revisiones?

Dependiendo de lo que desea revisar, creará la revisión de acceso en Azure AD acceso a PIM de Azure AD, aplicaciones de empresa de Azure AD (en versión preliminar) o las revisiones.

| Derechos de acceso de los usuarios | Los revisores pueden ser | Revisión creada en | Experiencia del revisor |
| --- | --- | --- | --- |
| Miembros del grupo de seguridad</br>Miembros del grupo de Office | Revisores especificados</br>Propietarios del grupo</br>Revisión propia | Revisiones de acceso de Azure AD</br>Grupos de Azure AD | Panel de acceso |
| Asignados a una aplicación conectada | Revisores especificados</br>Revisión propia | Revisiones de acceso de Azure AD</br>Aplicaciones de empresa de Azure AD (en versión preliminar) | Panel de acceso |
| Rol de Azure AD | Revisores especificados</br>Revisión propia | Azure AD PIM | Azure Portal |
| Rol de recursos de Azure | Revisores especificados</br>Revisión propia | Azure AD PIM | Azure Portal |

## <a name="prerequisites"></a>Requisitos previos

Para usar revisiones de acceso, debe tener una de las siguientes licencias:

- Azure AD Premium P2
- Licencia de Enterprise Mobility + Security (EMS) E5

Para más información, consulte [ Suscripción a Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) o [Enterprise Mobility + Security E5 Trial](https://aka.ms/emse5trial) (Prueba de Enterprise Mobility + Security E5).

## <a name="get-started-with-access-reviews"></a>Introducción a las revisiones de acceso

Para obtener más información sobre cómo crear y realizar las revisiones de acceso, vea esta breve demostración:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Si está listo para implementar revisiones de acceso en su organización, siga estos pasos en el vídeo para incorporar y entrenar a los administradores, y crear su primera revisión de acceso.

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="enable-access-reviews"></a>Habilitación de revisiones de acceso

Para habilitar las revisiones de acceso, siga estos pasos.

1. Como administrador Global o administrador de usuario, inicie sesión en el [portal Azure](https://portal.azure.com) donde desea usar el acceso de revisiones.

1. Haga clic en **Todos los servicios** y busque el servicio Revisiones de acceso.

1. Haga clic en **las revisiones de acceso**.

    ![Revisiones de acceso de todos los servicios:](./media/access-reviews-overview/all-services-access-reviews.png)

1. En la lista de navegación, haga clic en **Incorporar** para abrir la página **Incorporarse a las revisiones de acceso**.

    ![Incorporación de revisiones de acceso](./media/access-reviews-overview/onboard-button.png)

1. Haga clic en **Crear** para habilitar las revisiones de acceso en el directorio actual.

    ![Incorporarse a las revisiones de acceso](./media/access-reviews-overview/onboard-access-reviews.png)

    Revisa la próxima vez que inicie acceso, se habilitarán las opciones de revisión de acceso.

    ![Revisiones de acceso habilitadas](./media/access-reviews-overview/access-reviews-enabled.png)

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una revisión de acceso de grupos o aplicaciones](create-access-review.md)
- [Creación de una revisión de acceso de los usuarios en un rol administrativo de Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Revisar el acceso a grupos o aplicaciones](perform-access-review.md)
- [Revisión de acceso de grupos o aplicaciones](complete-access-review.md)
