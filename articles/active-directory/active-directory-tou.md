---
title: Azure Active Directory Terms of Use | Microsoft Docs
description: Azure AD Terms of Use le ofrecen tanto a usted como a su empresa la posibilidad de ofrecer términos de uso a los usuarios de los servicios de Azure AD.
services: active-directory
author: rolyon
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 07/31/2018
ms.author: rolyon
ms.openlocfilehash: edf8eccf8c0a1cba70ac178ac5863a68b26f14a3
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622082"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Característica Condiciones de uso de Azure Active Directory
Azure AD Terms of Use ofrece un método sencillo que pueden usar las organizaciones para presentar información a los usuarios finales. Esta presentación garantiza que los usuarios ven las declinaciones de responsabilidades pertinentes de los requisitos legales o de cumplimiento. En este artículo se describe cómo empezar a trabajar con Azure AD Terms of Use.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-can-i-do-with-terms-of-use"></a>¿Qué puedo hacer con Terms of Use?
Azure AD Terms of Use le permite realizar lo siguiente:
- Exigir que los empleados o invitados acepten los términos de uso antes de obtener acceso.
- Presentar términos de uso generales para todos los usuarios de la organización.
- Presentar términos de uso específicos basados en los atributos de un usuario (por ejemplo, médicos por un lado y enfermeras por otro, empleados nacionales por un lado e internacionales por otro, mediante [grupos dinámicos](users-groups-roles/groups-dynamic-membership.md)).
- Presentar términos de uso específicos al acceder a aplicaciones con una alta repercusión en la empresa, como Salesforce.
- Presentar términos de uso en diferentes idiomas.
- Mostrar quién ha aceptado o no los términos de uso.
- Mostrar un registro de auditoría de la actividad de términos de uso.

## <a name="prerequisites"></a>Requisitos previos
Para usar y configurar Azure AD Terms of Use, debe cumplir los siguientes requisitos:

- Suscripción a Azure AD Premium P1, P2, EMS E3 o EMS E5.
    - Si no tiene una de estas suscripciones, puede [obtener Azure AD Premium](fundamentals/active-directory-get-started-premium.md) o [habilitar la evaluación gratuita de Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).
- Una de las siguientes cuentas de administrador para el directorio que quiera configurar:
    - Administrador global
    - Administrador de seguridad
    - Administrador de acceso condicional

## <a name="terms-of-use-document"></a>Documento de términos de uso

Azure AD Terms of Use emplea el formato PDF para presentar el contenido. Este archivo PDF puede tener cualquier contenido, como documentos de contratos existentes, lo que le permite recopilar acuerdos de usuario final durante el inicio de sesión de usuario. El tamaño de fuente recomendado en el archivo PDF es 24.

## <a name="add-terms-of-use"></a>Agregar términos de uso
Una vez que haya finalizado el documento de términos de uso, use el procedimiento siguiente para agregarlo.

1. Inicie sesión en Azure como administrador global, administrador de seguridad o administrador de acceso condicional.

1. Vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

    ![Hoja Términos de uso](media/active-directory-tou/tou-blade.png)

1. Haga clic en **Nuevos términos**.

    ![Agregar condiciones de uso](media/active-directory-tou/new-tou.png)

1. Escriba el **nombre** de los términos de uso

2. Escriba el **nombre para mostrar**.  Este es el encabezado que ven los usuarios cuando inician sesión.

3. Use **Examinar** para desplazarse hasta el documento PDF finalizado de términos de uso y selecciónelo.

4. **Seleccione** un idioma para los términos de uso.  La opción de idioma permite cargar varios términos de uso, cada uno con un idioma diferente.  La versión de los términos de uso que verá un usuario final se basará en sus preferencias del explorador.

5. En **Require users to expand the Terms of use** (Exigir que los usuarios expandan los términos de uso), seleccione Activado o Desactivado.  Si este valor se establece en Activado, los usuarios finales deberán consultar los términos de uso antes de aceptarlos.

6. En **Acceso condicional**, puede **forzar** los términos de uso cargados mediante la selección de una plantilla en la lista desplegable o una directiva de acceso condicional personalizada.  Las directivas personalizadas de acceso condicional permiten términos de uso pormenorizados, hasta una aplicación de nube o un grupo de usuarios específicos.  Para más información, consulte el artículo sobre [configuración de directivas de acceso condicional](conditional-access/best-practices.md).

    >[!IMPORTANT]
    >Los controles de la directiva de acceso condicional (incluidos los términos de uso) no admiten el cumplimiento en las cuentas de servicio.  Se recomienda excluir todas las cuentas de servicio de la directiva de acceso condicional.

7. Haga clic en **Create**(Crear).

8. Si ha seleccionado una plantilla de acceso condicional personalizada, aparece entonces una nueva pantalla que le permite personalizar la directiva de acceso condicional.

    Ahora debería ver los nuevos términos de uso.

    ![Agregar condiciones de uso](media/active-directory-tou/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Ver quién los ha aceptado y rechazado
La hoja Términos de uso muestra un recuento de los usuarios que los han aceptado y rechazado. Estos recuentos, y quién aceptado o rechazado estos términos, se almacenan mientras están vigentes los términos de uso.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

    ![Evento de auditoría](media/active-directory-tou/view-tou.png)

1. Haga clic en los números en **Aceptado** o **Rechazado** para ver el estado actual de los usuarios.

    ![Evento de auditoría](media/active-directory-tou/accepted-tou.png)

## <a name="view-azure-ad-audit-logs"></a>Visualización de registros de auditoría de Azure AD
Si quiere ver la actividad adicional, Azure AD Terms of Use incluye registros de auditoría. Cada consentimiento de usuario desencadena un evento en los registros de auditoría que se almacena durante 30 días. Puede ver estos registros en el portal o descargarlos como un archivo .csv.

Para empezar a trabajar con los registros de auditoría de Azure AD, use el procedimiento siguiente:

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

1. Haga clic en **Ver registros de auditoría**.

    ![Evento de auditoría](media/active-directory-tou/audit-tou.png)

1. En la pantalla de registros de auditoría de Azure AD, puede filtrar la información mediante las listas desplegables proporcionadas para seleccionar información de registro de auditoría específica.

    ![Evento de auditoría](media/active-directory-tou/audit-logs-tou.png)

1. También puede hacer clic en **Descargar** para descargar la información en un archivo .csv y usarla localmente.

## <a name="what-terms-of-use-looks-like-for-users"></a>Cómo aparecen los términos de uso para los usuarios
Una vez que se crean y aplican los términos de uso, los usuarios, que están dentro del ámbito, verán la siguiente pantalla durante el inicio de sesión.

![Evento de auditoría](media/active-directory-tou/user-tou.png)

En la pantalla siguiente se muestra cómo aparecen los términos de uso en los dispositivos móviles.

![Evento de auditoría](media/active-directory-tou/mobile-tou.png)

### <a name="how-users-can-review-their-terms-of-use"></a>Cómo los usuarios pueden revisar los términos de uso
Los usuarios pueden revisar y ver los términos de uso que han aceptado mediante el procedimiento siguiente.

1. Inicie sesión en [https://myapps.microsoft.com](https://myapps.microsoft.com).

1. En la esquina superior derecha, haga clic en su nombre y seleccione **Perfil** en la lista desplegable.

    ![Perfil](media/active-directory-tou/tou14.png)

1. En la página Perfil, haga clic en **Review terms of use** (Revisar los términos de uso).

    ![Evento de auditoría](media/active-directory-tou/tou13a.png)

1. Desde ahí puede examinar los términos de uso que ha aceptado. 

## <a name="edit-terms-of-use-details"></a>Edición de los detalles de los términos de uso
Algunos de los detalles de los términos de uso se pueden editar, pero no se puede modificar un documento existente. El siguiente procedimiento describe cómo editar dichos detalles.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

1. Seleccione los términos de uso que desea editar.

1. Haga clic en **Editar términos**.

1. En el panel de edición de términos de uso, cambie el nombre, muestre el nombre o solicite a los usuarios que expandan los valores.

    ![Agregar condiciones de uso](media/active-directory-tou/edit-tou.png)

1. Haga clic en **Guardar** para guardar los cambios.

    Una vez que guarde los cambios, los usuarios tendrán que volver a aceptar los nuevos términos.

## <a name="add-a-terms-of-use-language"></a>Agregar un idioma a los términos de uso
El siguiente procedimiento describe cómo agregar un idioma a los términos de uso.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

1. Seleccione los términos de uso que desea editar.

1. En el panel de detalles, haga clic en la pestaña **Idiomas**.

    ![Agregar condiciones de uso](media/active-directory-tou/languages-tou.png)

1. Haga clic en **Agregar idioma**.

1. En el panel para agregar idioma de los términos de uso, cargue el PDF localizado y seleccione el idioma.

    ![Agregar condiciones de uso](media/active-directory-tou/language-add-tou.png)

1. Haga clic en **Agregar** para agregar el idioma.

## <a name="delete-terms-of-use"></a>Eliminar los términos de uso
Puede eliminar los antiguos términos de uso mediante el procedimiento siguiente:

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

1. Seleccione los términos de uso que quiere quitar.

1. Haga clic en **Eliminar términos**.

1. En el mensaje que le pregunta si desea continuar, haga clic en **Sí**.

    ![Agregar condiciones de uso](media/active-directory-tou/delete-tou.png)

    Ahora no debería ver los términos de uso.

## <a name="deleted-users-and-active-terms-of-use"></a>Usuarios eliminados y términos de uso activos
De forma predeterminada, un usuario eliminado está en estado eliminado en Azure AD durante 30 días, tiempo durante el cual un administrador puede restaurarlo si es necesario.  Después de 30 días, ese usuario se eliminará definitivamente.  Además, mediante el portal de Azure Active Directory, un administrador global puede [eliminar permanentemente un usuario eliminado recientemente](fundamentals/active-directory-users-restore.md) de manera explícita antes de que se alcance ese período de tiempo.  Una vez que un usuario se ha eliminado permanentemente, los sucesivos datos sobre ese usuario se eliminarán de los términos de uso activos.  La información de auditoría de los usuarios eliminados se conserva en el registro de auditoría.

## <a name="policy-changes"></a>Cambios de directiva
Las directivas de acceso condicional surten efecto de inmediato. Cuando esto sucede, el administrador empezará a ver "nubes tristes" o "problemas de token de Azure AD". El administrador debe cerrar sesión e iniciarla de nuevo para satisfacer la nueva directiva.

>[!IMPORTANT]
> Los usuarios dentro del ámbito necesitarán cerrar sesión y volver a iniciarla para satisfacer una directiva nueva si:
> - se ha habilitado una directiva de acceso condicional en los términos de uso,
> - o bien se crea una segunda condición de uso.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P: ¿Cómo puedo ver si un usuario ha aceptado los términos de uso y cuándo lo ha hecho?**</br>
R: en la hoja Términos de uso, haga clic en el número situado bajo **Aceptado**. También puede ver o buscar la actividad de aceptación en los registros de auditoría de Azure AD. Para más información, consulte [Ver quién los ha aceptado y rechazado](#view-who-has-accepted-and-declined) y [Visualización de registros de auditoría de Azure AD](#view-azure-ad-audit-logs).
 
**P: ¿Cuánto tiempo se almacena la información?**</br>
R: Los recuentos de usuarios en el informe Términos de uso y quién los ha aceptado o rechazado se almacenan mientras están vigentes los términos de uso. Los registros de auditoría de Azure AD se almacenan durante 30 días.

**P: ¿Por qué veo un número diferente de consentimientos en el informe Términos de uso en comparación con los registros de auditoría de Azure AD?**</br>
R: El informe Términos de uso se almacena durante toda la vigencia de esos términos de uso, mientras que los registros de auditoría de Azure AD se almacenan durante 30 días. Además, el informe Términos de uso solo muestra el estado de consentimiento actual de los usuarios. Por ejemplo, si un usuario manifiesta su rechazo y luego su aceptación, el informe Términos de uso solo mostrará la aceptación de ese usuario. Si necesita ver el historial, puede usar los registros de auditoría de Azure AD.

**P: Si edito los detalles de los términos de uso, ¿los usuarios tienen que volver a aceptarlos?**</br>
R: Sí, si un administrador edita los detalles de los términos de uso, los usuarios tienen que volver a aceptar los nuevos términos.

**P: ¿Se puede actualizar un documento de términos de uso existente?**</br>
R: Actualmente no se puede actualizar un documento de términos de uso existente. Para cambiarlo tendrá que crear una nueva instancia de los términos de uso.

**P: Si hay hipervínculos en el documento PDF de Términos de uso, ¿los usuarios finales podrán hacer clic en ellos?**</br>
R: El archivo PDF se representa de manera predeterminada como un archivo JPEG, por lo que no se puede hacer clic en los hipervínculos. Los usuarios tiene la opción de seleccionar **¿Tiene problemas con la visualización? Haga clic aquí**, que muestra el PDF de manera nativa, donde sí se admiten los hipervínculos.

**P: ¿Pueden los términos de uso admitir varios idiomas?**</br>
R: Sí.  Actualmente, un administrador puede configurar hasta 18 idiomas diferentes para unos solos términos de uso. 

**P: ¿Cuándo se desencadenan los términos de uso?**</br>
R: Los términos de uso se desencadenan durante la experiencia de inicio de sesión.

**P: ¿Qué aplicaciones puedo usar como destino de los términos de uso?**</br>
R: Puede crear una directiva de acceso condicional en las aplicaciones empresariales que usan autenticación moderna.  Para más información, consulte [aplicaciones empresariales](./manage-apps/view-applications-portal.md).

**P: ¿Puedo agregar varios términos de uso a un usuario o aplicación determinados?**</br>
R: Sí, mediante la creación de varias directivas de acceso condicional cuyo destino sean dichos grupos o aplicaciones. Si un usuario se encuentra en el ámbito de varios términos de uso, aceptará primero unos y después otros.
 
**P: ¿Qué ocurre si un usuario no acepta los términos de uso?**</br>
R: El usuario será bloqueado y no podrá obtener acceso a la aplicación. El usuario tendría que iniciar sesión de nuevo y aceptar las condiciones con el fin de obtener acceso.
 
**P: ¿Es posible rechazar los Términos de uso que se han aceptado previamente?**</br>
R: Puede [revisar los Términos de uso aceptados previamente](#how-users-can-review-their-terms-of-use) pero actualmente no hay ninguna manera de poderlos rechazar.

## <a name="next-steps"></a>Pasos siguientes

- [Procedimientos recomendados para el acceso condicional en Azure Active Directory](conditional-access/best-practices.md)