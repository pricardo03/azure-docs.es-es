---
title: Creación de una revisión de acceso de grupos o aplicaciones en las revisiones de acceso de Azure AD | Microsoft Docs
description: Obtenga información sobre cómo crear una revisión de acceso de los miembros del grupo o el acceso a la aplicación de revisiones de acceso de Azure AD.
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
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: c17078ea14a254f64a41751f2efffc16e2a1e821
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57847389"
---
# <a name="create-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Creación de una revisión de acceso de grupos o aplicaciones en las revisiones de acceso de Azure AD

El acceso a los grupos y las aplicaciones para empleados e invitados cambia a lo largo del tiempo. Para reducir el riesgo derivado de las asignaciones de acceso obsoletas, los administradores pueden usar Azure Active Directory (Azure AD) para crear revisiones de acceso para los miembros de grupo o el acceso de aplicación. Si necesita revisar el acceso de manera rutinaria, también puede crear revisiones de acceso periódicas. Para más información sobre estos escenarios, vea [Administración del acceso de los usuarios con las revisiones de acceso de Azure AD](manage-user-access-with-access-reviews.md) y [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](manage-guest-access-with-access-reviews.md).

Este artículo describe cómo crear una o varias de las revisiones de acceso para los miembros del grupo o el acceso a la aplicación.

## <a name="prerequisites"></a>Requisitos previos

- [Revisiones de acceso habilitadas](access-reviews-overview.md)
- Administrador global o administrador de usuarios

## <a name="create-one-or-more-access-reviews"></a>Crear una o varias revisiones de acceso

1. Inicie sesión en Azure Portal y abra la página [Revisiones de acceso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. Haga clic en **Controles**.

1. Haga clic en **Nueva revisión de acceso** para crear una nueva revisión de acceso.

    ![Revisión de acceso: controles](./media/create-access-review/controls.png)

1. Ponga un nombre a la revisión de acceso. Opcionalmente, asigne a la revisión una descripción. El nombre y la descripción se muestran a los revisores.

    ![Creación de una revisión de acceso: nombre y descripción de la revisión](./media/create-access-review/name-description.png)

1. Establezca un valor para **Fecha de inicio**. De forma predeterminada, una revisión de acceso ocurre una vez, se inicia a la misma hora en que se crea y finaliza en un mes. Puede cambiar las fechas de inicio y de finalización para hacer que una revisión de acceso se inicie en el futuro, transcurridos tantos días como desee.

    ![Creación de una revisión de acceso: fechas de inicio y de finalización](./media/create-access-review/start-end-dates.png)

1. Para que la revisión de acceso sea periódica, cambie el valor **Frecuencia** de **Una vez** a **Semanal**, **Mensual**, **Trimestral** o **Anual**, y utilice el cuadro de texto o el control deslizante **Duración** para definir el número de días que cada revisión de la serie periódica estará abierta para las aportaciones de los revisores. Por ejemplo, la duración máxima que puede establecer para una revisión mensual es 27 días, con el fin de evitar la superposición de revisiones.

1. Use el valor **Fin** para especificar cómo finalizar la serie de revisión de acceso periódica. La serie puede terminar de tres formas: se ejecuta continuamente para iniciar revisiones indefinidamente, hasta una fecha concreta, o hasta que se haya completado un número definido de veces. Otro usuario administrador u otro administrador Global puede detener la serie después de la creación, cambie la fecha en **configuración**, de modo que termina en esa fecha.

1. En la sección **Usuarios**, especifique los usuarios a los que aplica esa revisión de acceso. Las revisiones de acceso pueden ser de los miembros de un grupo o de los usuarios que se hayan asignado a una aplicación. Puede limitar aún más el ámbito de la revisión de acceso para revisar solo los usuarios invitados que sean miembros (o se hayan asignado a la aplicación), en lugar de revisar todos los usuarios miembros o que tengan acceso a la aplicación.

    ![Creación de una revisión de acceso: usuarios](./media/create-access-review/users.png)

1. En el **grupos** , seleccione uno o varios grupos que le gustaría ver la pertenencia de.

    > [!NOTE]
    > Si selecciona más de un grupo, se creación varias revisiones de acceso. Por ejemplo, selección de cinco grupos creará cinco de las revisiones de acceso independientes.
    
    ![Creación de una revisión de acceso - Seleccionar grupo](./media/create-access-review/select-group.png)

1. En el **aplicaciones** sección (si seleccionó **asignado a una aplicación** en el paso 8), seleccione las aplicaciones que le gustaría que revisen el acceso a.

    > [!NOTE]
    > Si selecciona más de una aplicación, se creación varias revisiones de acceso. Por ejemplo, la selección de cinco aplicaciones creará cinco revisiones de acceso independientes.
    
    ![Creación de una revisión de acceso - selección de aplicaciones](./media/create-access-review/select-application.png)

1. En la sección **Revisores**, seleccione una o más personas para revisar todos los usuarios del ámbito. También puede seleccionar que los miembros revisen su propio acceso. Si el recurso es un grupo, puede pedir a sus propietarios que hagan la revisión. También puede requerir que los revisores proporcionen un motivo cuando aprueben el acceso.

    ![Creación de una revisión de acceso: revisores](./media/create-access-review/reviewers.png)

1. En la sección **Programas**, seleccione el programa que desea usar. Puede simplificar el seguimiento y la recopilación de revisiones de acceso para distintos fines organizándolos en programas. El **Programa predeterminado** siempre está presente, o puede crear otro. Por ejemplo, puede elegir tener un programa cada para iniciativa de cumplimiento de normas u objetivo empresarial.

    ![Creación de una revisión de acceso: programas](./media/create-access-review/programs.png)

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

## <a name="start-the-access-review"></a>Inicio de la revisión de acceso

Una vez que haya especificado la configuración para una revisión de acceso, haga clic en **Iniciar**.

De forma predeterminada, Azure AD envía un correo electrónico a los revisores poco después de iniciar la revisión. Si decide no hacer que Azure AD envíe el correo electrónico, asegúrese de informar a los revisores de que hay una revisión de acceso esperando para que la lleven a cabo. Puede mostrarles las instrucciones sobre cómo [revisen el acceso a grupos o aplicaciones](perform-access-review.md). Si la revisión es para que los invitados que revisen su propio acceso, muéstreles las instrucciones sobre cómo [revisar el acceso por sí mismo a grupos o aplicaciones](review-your-access.md).

Si alguno de los revisores es un invitado, solo se notificará a los invitados por correo electrónico si ya han aceptado su invitación.

## <a name="manage-the-access-review"></a>Administración de la revisión de acceso

Puede seguir el progreso de las revisiones a medida que los revisores las realizan en el panel de Azure AD, en la sección **Revisiones de acceso**. Los derechos de acceso no se cambian en el directorio hasta que [la revisión finaliza](complete-access-review.md).

Si se trata de una revisión de un solo uso, a continuación, una vez que el período de revisión de acceso o el administrador detenga la revisión de acceso, siga los pasos de [revisión de acceso de grupos o aplicaciones](complete-access-review.md) para ver los resultados y aplicarlos.  

Para administrar una serie de revisiones de acceso, vaya a la revisión de acceso de **Controles** y verá los próximos eventos en Revisión programada; ahí podrá editar la fecha de finalización o agregar o quitar revisores según corresponda. 

Según las selecciones de la Configuración de finalización, la aplicación automática se ejecutará después de la fecha de finalización de la revisión o cuando se detenga manualmente la revisión. El estado de la revisión cambiará de Completado a estados intermedios como Aplicando y, por último, a Aplicado. Debería esperar ver a los usuarios denegados, si es que los hay, eliminados de la pertenencia al grupo o la asignación de aplicaciones en unos minutos.

## <a name="create-reviews-via-apis"></a>Creación de revisiones mediante API

También puede crear las revisiones de acceso mediante API. Qué hacer para administrar las revisiones de acceso de grupos y usuarios de aplicación en Azure Portal también pueden realizarse mediante Microsoft Graph API. Para más información, consulte [Azure AD Access Reviews API reference](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta) (Referencia de API de revisiones de acceso de Azure AD). Para obtener un ejemplo de código, vea [Example of retrieving Azure AD Access Reviews via Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096) (Ejemplo de recuperación de las revisiones de acceso de Azure AD a través de Microsoft Graph).

## <a name="next-steps"></a>Pasos siguientes

- [Revisar el acceso a grupos o aplicaciones](perform-access-review.md)
- [Revisar el acceso por sí mismo a grupos o aplicaciones](review-your-access.md)
- [Revisión de acceso de grupos o aplicaciones](complete-access-review.md)
