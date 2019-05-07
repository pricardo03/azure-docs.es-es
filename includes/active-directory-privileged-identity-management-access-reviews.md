---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 04/29/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: 364d4a11772e6bb72e2e258503f3cce49dc61453
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141844"
---
## <a name="create-one-or-more-access-reviews"></a>Crear una o varias revisiones de acceso

1. Haga clic en **New** para crear una nueva revisión de acceso.

1. Ponga un nombre a la revisión de acceso. Opcionalmente, asigne a la revisión una descripción. El nombre y la descripción se muestran a los revisores.

    ![Creación de una revisión de acceso: nombre y descripción de la revisión](./media/active-directory-privileged-identity-management-access-reviews/name-description.png)

1. Establezca un valor para **Fecha de inicio**. De forma predeterminada, una revisión de acceso ocurre una vez, se inicia a la misma hora en que se crea y finaliza en un mes. Puede cambiar las fechas de inicio y de finalización para hacer que una revisión de acceso se inicie en el futuro, transcurridos tantos días como desee.

    ![Creación de una revisión de acceso: fechas de inicio y de finalización](./media/active-directory-privileged-identity-management-access-reviews/start-end-dates.png)

1. Para realizar la revisión de acceso periódica, cambie el **frecuencia** de **una vez** a **semanal**, **mensual**,  **Trimestral**, **anualmente**, o **semestrales**. Use la **duración** control deslizante o cuadro de texto para definir cuántos días cada revisión de la serie periódicas será abrir para la entrada de revisores. Por ejemplo, la duración máxima que puede establecer para una revisión mensual es 27 días, con el fin de evitar la superposición de revisiones.

1. Use el valor **Fin** para especificar cómo finalizar la serie de revisión de acceso periódica. La serie puede terminar de tres formas: se ejecuta continuamente para iniciar revisiones indefinidamente, hasta una fecha concreta, o hasta que se haya completado un número definido de veces. Otro usuario administrador u otro administrador Global puede detener la serie después de la creación, cambie la fecha en **configuración**, de modo que termina en esa fecha.

1. En el **usuarios** , seleccione uno o varios roles que desea revisar la pertenencia de.

    ![Creación de una revisión de acceso - los usuarios](./media/active-directory-privileged-identity-management-access-reviews/users.png)

    > [!NOTE]
    > Si selecciona más de un rol, se creación varias revisiones de acceso. Por ejemplo, selección de cinco roles creará cinco de las revisiones de acceso independientes.

    Si va a crear una revisión de acceso de roles de Azure AD, a continuación muestra un ejemplo de la lista de miembros de la revisión.

    ![Creación de una revisión de acceso - pertenencia al rol de revisión](./media/active-directory-privileged-identity-management-access-reviews/review-membership.png)

    Si va a crear una revisión de acceso de roles de recursos de Azure, a continuación muestra un ejemplo de la lista de miembros de la revisión.

    ![Creación de una revisión de acceso - pertenencia al rol de revisión](./media/active-directory-privileged-identity-management-access-reviews/review-membership-azure-resource-roles.png)

1. En el **revisores** , seleccione uno o más personas para revisar todos los usuarios. También puede seleccionar que los miembros revisen su propio acceso.

    ![Creación de una revisión de acceso: revisores](./media/active-directory-privileged-identity-management-access-reviews/reviewers.png)

    - **Los usuarios seleccionados** -Use esta opción si no sabe quién tiene acceso. Con esta opción, puede asignar la revisión a un propietario de recursos o al administrador de grupos.
    - **Miembros (Autoasignado)** -Utilice esta opción para que los usuarios revisen sus propias asignaciones de roles.

### <a name="upon-completion-settings"></a>Configuración de finalización

1. Para especificar lo que sucede una vez finalizada una revisión, expanda la sección **Configuración de finalización**.

    ![Configuración de finalización](./media/active-directory-privileged-identity-management-access-reviews/upon-completion-settings.png)

1. Si desea quitar automáticamente el acceso para los usuarios que se han denegado, establezca **Aplicar automáticamente los resultados al recurso** en **Habilitar**. Si desea aplicar manualmente los resultados cuando se complete la revisión, establezca el conmutador en **Deshabilitar**.

1. Use la lista **Si el revisor no responde** para especificar lo que ocurre con los usuarios a los que el revisor no ha revisado dentro del período de revisión. Este valor no afecta a los usuarios que los revisores revisaron manualmente. Si la decisión final del revisor es Denegar, se quitará el acceso del usuario.

    - **Sin cambios**: dejar el acceso del usuario sin cambios
    - **Quitar acceso**: quitar el acceso del usuario
    - **Aprobar acceso**: aprobar el acceso del usuario
    - **Aceptar recomendaciones**: aceptar la recomendación del sistema sobre la denegación o aprobación del acceso continuo del usuario

### <a name="advanced-settings"></a>Configuración avanzada

1. Para especificar configuraciones adicionales, expanda la sección **Configuración avanzada**.

    ![Configuración avanzada](./media/active-directory-privileged-identity-management-access-reviews/advanced-settings.png)

1. Establezca **Mostrar recomendaciones** en **Habilitar** para mostrar las recomendaciones del sistema de los revisores según la información del acceso del usuario.

1. Establezca **Requerir motivo de la aprobación** en **Habilitar** para requerir que el revisor proporcione un motivo para la aprobación.

1. Establezca **Notificaciones de correo** en **Habilitar**para que Azure AD envíe notificaciones de correo electrónico a los revisores cuando se inicia una revisión de acceso y a los administradores cuando se complete.

1. Establezca **Avisos** en **Habilitar** para que Azure AD envíe recordatorios de revisiones de acceso en curso a los revisores que no hayan completado su revisión.
