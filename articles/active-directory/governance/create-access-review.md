---
title: 'Creación de una revisión de acceso de grupos o aplicaciones: Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo crear una revisión de acceso de los miembros del grupo o el acceso a la aplicación de revisiones de acceso de Azure Active Directory.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6bafa4614e40bb1796ec90e07ecf5b9286a8acb9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113479"
---
# <a name="create-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Creación de una revisión de acceso de los grupos o las revisiones de acceso de las aplicaciones de Azure AD

El acceso a los grupos y las aplicaciones para empleados e invitados cambia a lo largo del tiempo. Para reducir el riesgo derivado de las asignaciones de acceso obsoletas, los administradores pueden usar Azure Active Directory (Azure AD) para crear revisiones de acceso para los miembros de grupo o el acceso de aplicación. Si necesita revisar el acceso de manera rutinaria, también puede crear revisiones de acceso periódicas. Para más información sobre estos escenarios, vea [Administración del acceso de los usuarios con las revisiones de acceso de Azure AD](manage-user-access-with-access-reviews.md) y [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](manage-guest-access-with-access-reviews.md).

Este artículo describe cómo crear una o varias de las revisiones de acceso para los miembros del grupo o el acceso a la aplicación.

## <a name="prerequisites"></a>Requisitos previos

- Azure AD Premium P2
- [Revisiones de acceso habilitadas](access-reviews-overview.md)
- Administrador global o administrador de usuarios

Para obtener más información, consulte [qué usuarios deben tener licencias?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="create-one-or-more-access-reviews"></a>Crear una o varias revisiones de acceso

1. Inicie sesión en Azure portal y abra el [página de revisiones de acceso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. En el menú izquierdo, haga clic en **las revisiones de acceso**.

1. Haga clic en **Nueva revisión de acceso** para crear una nueva revisión de acceso.

    ![Revisión de acceso: controles](./media/create-access-review/access-reviews.png)

1. Ponga un nombre a la revisión de acceso. Opcionalmente, asigne a la revisión una descripción. El nombre y la descripción se muestran a los revisores.

    ![Creación de una revisión de acceso: nombre y descripción de la revisión](./media/create-access-review/name-description.png)

1. Establezca un valor para **Fecha de inicio**. De forma predeterminada, una revisión de acceso ocurre una vez, se inicia a la misma hora en que se crea y finaliza en un mes. Puede cambiar las fechas de inicio y de finalización para hacer que una revisión de acceso se inicie en el futuro, transcurridos tantos días como desee.

    ![Creación de una revisión de acceso: fechas de inicio y de finalización](./media/create-access-review/start-end-dates.png)

1. Para realizar la revisión de acceso periódica, cambie el **frecuencia** de **una vez** a **semanal**, **mensual**,  **Trimestral** o **anualmente**. Use la **duración** control deslizante o cuadro de texto para definir cuántos días cada revisión de la serie periódicas será abrir para la entrada de revisores. Por ejemplo, la duración máxima que puede establecer para una revisión mensual es 27 días, con el fin de evitar la superposición de revisiones.

1. Use el valor **Fin** para especificar cómo finalizar la serie de revisión de acceso periódica. La serie puede terminar de tres formas: se ejecuta continuamente para iniciar revisiones indefinidamente, hasta una fecha concreta, o hasta que se haya completado un número definido de veces. Otro usuario administrador u otro administrador Global puede detener la serie después de la creación, cambie la fecha en **configuración**, de modo que termina en esa fecha.

1. En el **usuarios** sección, especifique los usuarios que se aplica la revisión de acceso. Las revisiones de acceso pueden ser de los miembros de un grupo o de los usuarios que se hayan asignado a una aplicación. Puede limitar aún más el ámbito de la revisión de acceso para revisar solo los usuarios invitados que sean miembros (o se hayan asignado a la aplicación), en lugar de revisar todos los usuarios miembros o que tengan acceso a la aplicación.

    ![Creación de una revisión de acceso: usuarios](./media/create-access-review/users.png)

1. En el **grupo** , seleccione uno o varios grupos que le gustaría ver la pertenencia de.

    > [!NOTE]
    > Si selecciona más de un grupo, se creación varias revisiones de acceso. Por ejemplo, selección de cinco grupos creará cinco de las revisiones de acceso independientes.
    
    ![Creación de una revisión de acceso - Seleccionar grupo](./media/create-access-review/select-group.png)

1. En el **aplicaciones** sección (si seleccionó **asignado a una aplicación** en el paso 8), seleccione las aplicaciones que le gustaría que revisen el acceso a.

    > [!NOTE]
    > Si selecciona más de una aplicación, se creación varias revisiones de acceso. Por ejemplo, la selección de cinco aplicaciones creará cinco revisiones de acceso independientes.
    
    ![Creación de una revisión de acceso - selección de aplicaciones](./media/create-access-review/select-application.png)

1. En la sección **Revisores**, seleccione una o más personas para revisar todos los usuarios del ámbito. También puede seleccionar que los miembros revisen su propio acceso. Si el recurso es un grupo, puede pedir a sus propietarios que hagan la revisión. También puede requerir que los revisores proporcionen un motivo cuando aprueben el acceso.

    ![Creación de una revisión de acceso: revisores](./media/create-access-review/reviewers.png)

1. En la sección **Programas**, seleccione el programa que desea usar. El **programa predeterminado** siempre está presente.

    ![Creación de una revisión de acceso: programas](./media/create-access-review/programs.png)

    Puede simplificar el seguimiento y la recopilación de revisiones de acceso para distintos fines organizándolos en programas. Cada revisión de acceso se puede vincular a un programa. Por lo tanto, cuando prepare los informes para un auditor, puede centrarse en las revisiones de acceso en el ámbito de una iniciativa determinada. Programas y los resultados de la revisión de acceso son visibles para los usuarios en el administrador Global, Administrador de usuarios, Administrador de seguridad o rol de lector de seguridad.

    Para ver una lista de programas, las revisiones se dirijan a la página y seleccione **programas**. Si está en un administrador Global o un rol de usuario administrador, puede crear programas adicionales. Por ejemplo, puede elegir tener un programa cada para iniciativa de cumplimiento de normas u objetivo empresarial. Si ya no necesita un programa y no tiene ningún control vinculado a él, puede eliminarlo.

### <a name="upon-completion-settings"></a>Configuración de finalización

1. Para especificar lo que sucede una vez finalizada una revisión, expanda la sección **Configuración de finalización**.

    ![Configuración de finalización](./media/create-access-review/upon-completion-settings.png)

1. Si desea quitar automáticamente el acceso para los usuarios que se han denegado, establezca **Aplicar automáticamente los resultados al recurso** en **Habilitar**. Si desea aplicar manualmente los resultados cuando se complete la revisión, establezca el conmutador en **Deshabilitar**.

1. Use la lista **Si el revisor no responde** para especificar lo que ocurre con los usuarios a los que el revisor no ha revisado dentro del período de revisión. Este valor no afecta a los usuarios que los revisores revisaron manualmente. Si la decisión final del revisor es Denegar, se quitará el acceso del usuario.

    - **Sin cambios**: dejar el acceso del usuario sin cambios
    - **Quitar acceso**: quitar el acceso del usuario
    - **Aprobar acceso**: aprobar el acceso del usuario
    - **Aceptar recomendaciones**: aceptar la recomendación del sistema sobre la denegación o aprobación del acceso continuo del usuario

### <a name="advanced-settings"></a>Configuración avanzada

1. Para especificar configuraciones adicionales, expanda la sección **Configuración avanzada**.

    ![Configuración avanzada](./media/create-access-review/advanced-settings.png)

1. Establezca **Mostrar recomendaciones** en **Habilitar** para mostrar las recomendaciones del sistema de los revisores según la información del acceso del usuario.

1. Establezca **Requerir motivo de la aprobación** en **Habilitar** para requerir que el revisor proporcione un motivo para la aprobación.

1. Establezca **Notificaciones de correo** en **Habilitar**para que Azure AD envíe notificaciones de correo electrónico a los revisores cuando se inicia una revisión de acceso y a los administradores cuando se complete.

1. Establezca **Avisos** en **Habilitar** para que Azure AD envíe recordatorios de revisiones de acceso en curso a los revisores que no hayan completado su revisión.

    De forma predeterminada, Azure AD envía automáticamente un recordatorio hacia la mitad del plazo de finalización a los revisores que no hayan respondido.

## <a name="start-the-access-review"></a>Inicio de la revisión de acceso

Una vez que haya especificado la configuración para una revisión de acceso, haga clic en **Iniciar**. La revisión de acceso aparecerá en la lista con un indicador de su estado.

![Lista de revisiones de acceso](./media/create-access-review/access-reviews-list.png)

De forma predeterminada, Azure AD envía un correo electrónico a los revisores poco después de iniciar la revisión. Si decide no hacer que Azure AD envíe el correo electrónico, asegúrese de informar a los revisores de que hay una revisión de acceso esperando para que la lleven a cabo. Puede mostrarles las instrucciones sobre cómo [revisen el acceso a grupos o aplicaciones](perform-access-review.md). Si la revisión es para que los invitados que revisen su propio acceso, muéstreles las instrucciones sobre cómo [revisar el acceso por sí mismo a grupos o aplicaciones](review-your-access.md).

Si se han asignado los invitados como revisores y no han aceptado la invitación, no recibirán un correo electrónico de las revisiones de acceso porque primero debe aceptar la invitación antes de la revisión.

## <a name="create-reviews-via-apis"></a>Creación de revisiones mediante API

También puede crear las revisiones de acceso mediante API. Qué hacer para administrar las revisiones de acceso de grupos y usuarios de aplicación en Azure Portal también pueden realizarse mediante Microsoft Graph API. Para obtener más información, consulte el [referencia de API de revisiones de acceso de Azure AD](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta). Para obtener un ejemplo de código, vea [revisa el ejemplo de recuperación de acceso de Azure AD a través de Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Pasos siguientes

- [Revisar el acceso a grupos o aplicaciones](perform-access-review.md)
- [Revisar el acceso por sí mismo a grupos o aplicaciones](review-your-access.md)
- [Revisión de acceso de grupos o aplicaciones](complete-access-review.md)
