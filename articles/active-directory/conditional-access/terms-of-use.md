---
title: Términos de uso - Azure Active Directory | Microsoft Docs
description: Introducción al uso de condiciones de uso para presentar la información antes de obtener acceso a los empleados o los invitados de Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: compliance
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1abae0a454e17e8f633f68bc5853bfb4a4b24d14
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66383175"
---
# <a name="azure-active-directory-terms-of-use"></a>Condiciones de uso de Azure Active Directory

Términos de uso de Azure AD proporciona un método sencillo que las organizaciones pueden usar para presentar información a los usuarios finales. Esta presentación garantiza que los usuarios ven las declinaciones de responsabilidades pertinentes de los requisitos legales o de cumplimiento. En este artículo se describe cómo empezar a trabajar con los términos de uso.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Vídeos de introducción

El vídeo siguiente proporciona una introducción rápida de los términos de uso.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Para otros vídeos, vea:
- [Cómo implementar los términos de uso en Azure Active Directory](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Cómo implementar los términos de uso en Azure Active Directory](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>¿Qué puedo hacer con los términos de uso?

Términos de uso de Azure AD tiene las siguientes funcionalidades:

- Se requieren los empleados o invitados Aceptar los términos de uso antes de obtener acceso.
- Se requieren los empleados o invitados Aceptar los términos de uso en todos los dispositivos antes de obtener acceso.
- Se requieren los empleados o invitados Aceptar los términos de uso según una programación periódica.
- Se requieren los empleados o invitados Aceptar los términos de uso antes de registrar información de seguridad en Azure Multi-factor Authentication (MFA).
- Requerir que los empleados Aceptar los términos de uso antes de registrar información de seguridad en el restablecimiento de contraseña de autoservicio (SSPR) de Azure AD.
- Presentar los términos de uso para todos los usuarios generales de la organización.
- Presentar términos específicos de uso basados en atributos de un usuario (p. ej. médicos por un lado y enfermeras por otro, empleados nacionales por un lado e internacionales por otro, mediante [grupos dinámicos](../users-groups-roles/groups-dynamic-membership.md)).
- Presentar los términos de uso específicos al tener acceso a las aplicaciones empresariales alto impacto, como Salesforce.
- Presentes términos de uso en diferentes idiomas.
- Lista que tiene o no ha aceptado los términos de uso.
- Ayudar a cumplir los reglamentos de privacidad.
- Mostrar un registro de condiciones de actividad de uso de cumplimiento y auditoría.
- Crear y administrar los términos de uso a través [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/agreement) (actualmente en versión preliminar).

## <a name="prerequisites"></a>Requisitos previos

Para usar y configurar las condiciones de uso de Azure AD, debe tener:

- Suscripción a Azure AD Premium P1, P2, EMS E3 o EMS E5.
   - Si no tiene una de estas suscripciones, puede [obtener Azure AD Premium](../fundamentals/active-directory-get-started-premium.md) o [habilitar la evaluación gratuita de Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).
- Una de las siguientes cuentas de administrador para el directorio que quiera configurar:
   - Administrador global
   - Administrador de seguridad
   - Administrador de acceso condicional

## <a name="terms-of-use-document"></a>Documento de términos de uso

Términos de uso de Azure AD usa el formato PDF para presentar el contenido. Este archivo PDF puede tener cualquier contenido, como documentos de contratos existentes, lo que le permite recopilar acuerdos de usuario final durante el inicio de sesión de usuario. Para admitir usuarios en dispositivos móviles, el tamaño de fuente recomendado en el PDF es 24 puntos.

## <a name="add-terms-of-use"></a>Agregar términos de uso

Una vez que haya finalizado los términos de uso de documentos, use el siguiente procedimiento para agregarlo.

1. Inicie sesión en Azure como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

   ![Hoja Términos de uso](./media/terms-of-use/tou-blade.png)

1. Haga clic en **Nuevos términos**.

   ![Agregar condiciones de uso](./media/terms-of-use/new-tou.png)

1. En el **nombre** , escriba un nombre para los términos de uso que se usará en el portal de Azure.
1. En el cuadro **Nombre para mostrar**, escriba un título que vean los usuarios cada vez que inicien sesión.
1. Para **documento condiciones de uso**, vaya a los términos de uso PDF finalizados y selecciónelo.
1. Seleccione el idioma de los términos de uso de documentos. La opción de idioma permite cargar varios términos de uso, cada uno con un idioma diferente. La versión de los términos de uso que verá un usuario final se basará en sus preferencias del explorador.
1. Para requerir que los usuarios finales ver los términos de uso antes de aceptarlos, establezca **requieren que los usuarios que expandan las condiciones de uso** a **en**.
1. Para requerir que los usuarios finales acepten los términos de uso en todos los dispositivos acceden desde, establezca **requieren que los usuarios den su consentimiento en todos los dispositivos** a **en**. Para obtener más información, consulte [condiciones de uso de cada dispositivo](#per-device-terms-of-use).
1. Si desea hacer expirar los términos de uso consentimientos según una programación, establezca **expiran consentimientos** a **en**. Al establecerse en Activado, se muestran dos opciones de configuración de la programación adicionales.

   ![Expirar autorizaciones](./media/terms-of-use/expire-consents.png)

1. Utilice la **a partir de la fecha de expiración** y **frecuencia** configuración para especificar la programación para los términos de uso vencimientos. En la siguiente tabla se muestra el resultado de un par de opciones de configuración de ejemplo:

   | Expirar a partir del | Frecuencia | Resultado |
   | --- | --- | --- |
   | La fecha de hoy  | Programación mensual | A partir de hoy en día, los usuarios deben aceptar los términos de uso y acepten después de cada mes. |
   | Fecha del futuro  | Programación mensual | A partir de hoy en día, los usuarios deben aceptar los términos de uso. Cuando la fecha del futuro llegue, expirarán las autorizaciones y, luego, los usuarios deberán volver a aceptar cada mes.  |

   Por ejemplo, si establece la fecha de inicio de la expiración en **1 de enero** y la frecuencia en **Mensual**, este es el modo en que pueden producirse las expiraciones para dos usuarios:

   | Usuario | Primera fecha de aceptación | Primera fecha de expiración | Segunda fecha de expiración | Tercera fecha de expiración |
   | --- | --- | --- | --- | --- |
   | Alice | 1 ene | 1 feb | 1 mar | 1 abr |
   | Bob | 15 ene | 1 feb | 1 mar | 1 abr |

1. Utilice la **duración antes de (días) requiere la aceptación de re** configuración para especificar el número de días antes de que el usuario debe aceptar los términos de uso. Esto permite a los usuarios seguir su propia programación. Por ejemplo, si establece la duración en **30** días, este es el modo en que pueden producirse las expiraciones para dos usuarios:

   | Usuario | Primera fecha de aceptación | Primera fecha de expiración | Segunda fecha de expiración | Tercera fecha de expiración |
   | --- | --- | --- | --- | --- |
   | Alice | 1 ene | 31 ene | 2 mar | 1 abr |
   | Bob | 15 ene | 14 feb | 16 mar | 15 abr |

   Es posible usar las opciones de configuración **Expirar autorizaciones** y **Duración antes de solicitar que se acepten los términos de nuevo (días)** de forma conjunta, pero normalmente usa una o la otra.

1. En **acceso condicional**, utilice el **aplicar con la plantilla de directiva de acceso condicional** lista para seleccionar la plantilla para aplicar los términos de uso.

   ![Plantillas de acceso condicional](./media/terms-of-use/conditional-access-templates.png)

   | Plantilla | DESCRIPCIÓN |
   | --- | --- |
   | **Acceso a las aplicaciones en la nube para todos los invitados** | Se creará una directiva de acceso condicional para todos los invitados y todas las aplicaciones en la nube. Esta directiva afecta a Azure Portal. Una vez que se crea, es posible que se le pida cerrar e iniciar sesión. |
   | **Acceso a las aplicaciones en la nube para todos los usuarios** | Se creará una directiva de acceso condicional para todos los usuarios y todas las aplicaciones en la nube. Esta directiva afecta a Azure Portal. Una vez que se crea, se le pedirá que cierre e inicie sesión. |
   | **Directiva personalizada** | Seleccione los usuarios, grupos y las aplicaciones que se aplicarán estos términos de uso a. |
   | **Crear directiva de acceso condicional posteriormente** | Estos términos de uso aparecerán en la lista de control de concesiones al crear una directiva de acceso condicional. |

   >[!IMPORTANT]
   >Los controles de la directiva de acceso condicional (incluidos los términos de uso) no admiten el cumplimiento en las cuentas de servicio. Se recomienda excluir todas las cuentas de servicio de la directiva de acceso condicional.

    Las directivas de personalizada de acceso condicional permiten términos de uso a una aplicación específica en la nube o grupo de usuarios pormenorizados. Para más información, consulte [Inicio rápido: Solicitar la aceptación de los términos de uso antes de acceder a aplicaciones en la nube](require-tou.md).

1. Haga clic en **Create**(Crear).

   Si ha seleccionado una plantilla de acceso condicional personalizada, aparece entonces una nueva pantalla que le permite crear la directiva de acceso condicional personalizada.

   ![Directiva personalizada](./media/terms-of-use/custom-policy.png)

   Ahora debería ver los nuevos términos de uso.

   ![Agregar condiciones de uso](./media/terms-of-use/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Ver quién los ha aceptado y rechazado

La hoja Términos de uso muestra un recuento de los usuarios que los han aceptado y rechazado. Estos recuentos y quién aceptado/rechazado se almacenan durante la vida de los términos de uso.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

   ![Hoja Términos de uso](./media/terms-of-use/view-tou.png)

1. Para los términos de uso, haga clic en los números en **aceptado** o **rechazada** para ver el estado actual de usuarios.

   ![Autorizaciones de términos de uso](./media/terms-of-use/accepted-tou.png)

1. Para ver el historial de un usuario individual, haga clic en los puntos suspensivos ( **...** ) y, a continuación, en **Ver historial**.

   ![Menú Ver historial](./media/terms-of-use/view-history-menu.png)

   En el panel Ver historial, puede ver un historial de todas las aceptaciones, rechazos y expiraciones.

   ![Panel Ver historial](./media/terms-of-use/view-history-pane.png)

## <a name="view-azure-ad-audit-logs"></a>Visualización de registros de auditoría de Azure AD

Si desea ver la actividad adicional, condiciones de uso de Azure AD incluye los registros de auditoría. Cada consentimiento de usuario desencadena un evento en los registros de auditoría que se almacena durante **30 días**. Puede ver estos registros en el portal o descargarlos como un archivo .csv.

Para empezar a trabajar con los registros de auditoría de Azure AD, use el procedimiento siguiente:

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).
1. Seleccione una condición de uso.
1. Haga clic en **Ver registros de auditoría**.

   ![Hoja Términos de uso](./media/terms-of-use/audit-tou.png)

1. En la pantalla de registros de auditoría de Azure AD, puede filtrar la información mediante las listas proporcionadas para seleccionar información de registro de auditoría específica.

   También puede hacer clic en **Descargar** para descargar la información en un archivo .csv y usarla localmente.

   ![Registros de auditoría](./media/terms-of-use/audit-logs-tou.png)

   Si hace clic en un registro, aparecerá un panel con detalles de actividad adicionales.

   ![Detalles de actividad](./media/terms-of-use/audit-log-activity-details.png)

## <a name="what-terms-of-use-looks-like-for-users"></a>¿Qué términos de uso es similar a los usuarios

Una vez que se crea y se aplican los términos de uso, los usuarios, que están en el ámbito, verá la siguiente pantalla durante el inicio de sesión.

![Inicio de sesión web de usuario](./media/terms-of-use/user-tou.png)

Los usuarios pueden ver los términos de uso y, si es necesario, utilice los botones para acercar y alejar.

![Ver términos de uso con los botones de zoom](./media/terms-of-use/zoom-buttons.png)

La siguiente pantalla muestra el aspecto de los términos de uso en dispositivos móviles.

![Inicio de sesión móvil de usuario](./media/terms-of-use/mobile-tou.png)

Los usuarios solo deben aceptar los términos de uso de una vez y no podrán ver los términos de uso nuevo en inicios de sesión posteriores.

### <a name="how-users-can-review-their-terms-of-use"></a>Cómo los usuarios pueden revisar los términos de uso

Los usuarios pueden revisar y ver los términos de uso que han aceptado mediante el procedimiento siguiente.

1. Inicie sesión en [https://myapps.microsoft.com](https://myapps.microsoft.com).
1. En la esquina superior derecha, haga clic en su nombre y seleccione **Perfil**.

   ![Perfil](./media/terms-of-use/tou14.png)

1. En la página Perfil, haga clic en **Review terms of use** (Revisar los términos de uso).

   ![Perfil: revisión de los términos de uso](./media/terms-of-use/tou13a.png)

1. Desde ahí puede examinar los términos de uso que ha aceptado.

## <a name="edit-terms-of-use-details"></a>Editar los términos de los detalles de uso

Puede editar algunos detalles de los términos de uso, pero no se puede modificar un documento existente. El siguiente procedimiento describe cómo editar dichos detalles.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).
1. Seleccione los términos de uso que desea editar.
1. Haga clic en **Editar términos**.
1. En el panel de edición de términos de uso, cambie el nombre, muestre el nombre o solicite a los usuarios que expandan los valores.

   Si hay otras opciones que le gustaría cambiar, por ejemplo, el documento PDF, requieren que los usuarios dar su consentimiento en todos los dispositivos, expiran los consentimientos, duración antes de reacceptance o directiva de acceso condicional, debe crear una nueva condición de uso.

   ![Edición de términos de uso](./media/terms-of-use/edit-tou.png)

1. Haga clic en **Guardar** para guardar los cambios.

   Una vez que guarde los cambios, los usuarios no tendrán que volver a aceptar estas ediciones.

## <a name="add-a-terms-of-use-language"></a>Agregar una condición de lenguaje de uso

El siguiente procedimiento describe cómo agregar una condición de lenguaje de uso.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).
1. Seleccione los términos de uso que desea editar.
1. En el panel de detalles, haga clic en la pestaña **Idiomas**.

   ![Agregar condiciones de uso](./media/terms-of-use/languages-tou.png)

1. Haga clic en **Agregar idioma**.
1. En el panel para agregar idioma de los términos de uso, cargue el PDF localizado y seleccione el idioma.

   ![Agregar condiciones de uso](./media/terms-of-use/language-add-tou.png)

1. Haga clic en **Agregar** para agregar el idioma.

## <a name="per-device-terms-of-use"></a>Condiciones de uso de cada dispositivo

El **requieren que los usuarios den su consentimiento en todos los dispositivos** configuración permite obligar a los usuarios finales acepten los términos de uso en todos los dispositivos acceden desde. Al usuario final se le pedirá que se una a su dispositivo en Azure AD. Cuando se une el dispositivo, el identificador de dispositivo se utiliza para exigir los términos de uso en cada dispositivo.

Esta es una lista de las plataformas y el software admitidos.

> [!div class="mx-tableFixed"]
> |  | iOS | Android | Windows 10 | Otros |
> | --- | --- | --- | --- | --- |
> | **Aplicación nativa** | Sí | Sí | Sí |  |
> | **Microsoft Edge** | Sí | Sí | Sí |  |
> | **Internet Explorer** | Sí | Sí | Sí |  |
> | **Chrome (con extensión)** | Sí | Sí | Sí |  |

Condiciones de uso de cada dispositivo tiene las siguientes restricciones:

- Un dispositivo solo puede conectarse a un inquilino.
- Un usuario debe tener permisos para conectarse a su dispositivo.
- No se admite la aplicación de inscripción de Intune.
- No se admiten los usuarios de Azure AD B2B.

Si el dispositivo del usuario no se une, recibirá un mensaje indicándole que debe conectarse a su dispositivo. Su experiencia dependerá de la plataforma y el software.

### <a name="join-a-windows-10-device"></a>Conexión a un dispositivo con Windows 10

Si un usuario usa Windows 10 y Microsoft Edge, recibirá un mensaje similar al siguiente para [conectarse a su dispositivo](../user-help/user-help-join-device-on-network.md#to-join-an-already-configured-windows-10-device).

![Windows 10 y Microsoft Edge: mensaje de conexión a dispositivos](./media/terms-of-use/per-device-win10-edge.png)

Si usa Chrome, se le pedirá que instale la [extensión de cuentas de Windows 10](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="browsers"></a>Exploradores

Si un usuario utiliza un explorador que no se admite, se le pedirá que use otro explorador.

![Explorador no compatible](./media/terms-of-use/per-device-browser-unsupported.png)

## <a name="delete-terms-of-use"></a>Eliminar términos de uso

Puede eliminar los antiguos términos de uso mediante el procedimiento siguiente.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).
1. Seleccione los términos de uso que desea quitar.
1. Haga clic en **Eliminar términos**.
1. En el mensaje que le pregunta si desea continuar, haga clic en **Sí**.

   ![Eliminar términos de uso](./media/terms-of-use/delete-tou.png)

   Ya no debería ver los términos de uso.

## <a name="deleted-users-and-active-terms-of-use"></a>Usuarios eliminados y activos términos de uso

De forma predeterminada, un usuario eliminado está en estado eliminado en Azure AD durante 30 días, tiempo durante el cual un administrador puede restaurarlo si es necesario. Después de 30 días, ese usuario se eliminará definitivamente. Además, mediante el portal de Azure Active Directory, un administrador global puede [eliminar permanentemente un usuario eliminado recientemente](../fundamentals/active-directory-users-restore.md) explícitamente antes de que se alcance ese período de tiempo. Uno de un usuario se ha eliminado permanentemente, posteriores datos sobre ese usuario se quitarán los términos de uso activos. La información de auditoría de los usuarios eliminados se conserva en el registro de auditoría.

## <a name="policy-changes"></a>Cambios de directiva

Las directivas de acceso condicional surten efecto de inmediato. Cuando esto sucede, el administrador empezará a ver "nubes tristes" o "problemas de token de Azure AD". El administrador debe cerrar sesión e iniciarla de nuevo para satisfacer la nueva directiva.

> [!IMPORTANT]
> Los usuarios dentro del ámbito necesitarán cerrar sesión y volver a iniciarla para satisfacer una directiva nueva si:
>
> - se ha habilitado una directiva de acceso condicional en los términos de uso
> - o se crea una segunda condición de uso

## <a name="b2b-guests-preview"></a>Invitados B2B (versión preliminar)

La mayoría de las organizaciones tienen un proceso en lugar de sus empleados para dar su consentimiento a los términos de su organización de instrucciones de uso y la privacidad. Sin embargo, ¿cómo puede forzar las mismas autorizaciones para los invitados negocio a negocio (B2B) de Azure AD cuando se agregan a través de SharePoint o Teams? Con acceso condicional y los términos de uso, puede aplicar una directiva directamente hacia los usuarios invitados de B2B. Durante el flujo de canje de invitación, se presenta al usuario con los términos de uso. Esta compatibilidad se encuentra actualmente en versión preliminar.

Los Términos de uso solo se mostrarán cuando el usuario tenga una cuenta de invitado en Azure AD. SharePoint Online tiene actualmente un [experiencia de destinatario uso compartido externo ad-hoc](/sharepoint/what-s-new-in-sharing-in-targeted-release) para compartir un documento o una carpeta que no requiere que el usuario tenga una cuenta de invitado. En este caso, no se muestra una condición de uso.

![Todos los usuarios invitados](./media/terms-of-use/b2b-guests.png)

## <a name="support-for-cloud-apps-preview"></a>Compatibilidad con aplicaciones en la nube (versión preliminar)

Los Términos de uso se pueden usar para diversas aplicaciones en la nube, como Azure Information Protection y Microsoft Intune. Esta compatibilidad se encuentra actualmente en versión preliminar.

### <a name="azure-information-protection"></a>Azure Information Protection

Puede configurar una directiva de acceso condicional para la aplicación Azure Information Protection y requieren una condición de uso cuando un usuario accede a un documento protegido. Esto desencadenará una condición de uso antes de un usuario tiene acceso a un documento protegido por primera vez.

![Aplicación en la nube de Azure Information Protection](./media/terms-of-use/cloud-app-info-protection.png)

### <a name="microsoft-intune-enrollment"></a>Inscripción en Microsoft Intune

Puede configurar una directiva de acceso condicional para la aplicación de inscripción a Microsoft Intune y requieren los términos de uso antes de la inscripción de un dispositivo en Intune. Para obtener más información, consulte la entrada de blog [Choosing the right Terms solution for your organization](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409) (Elección de la solución de términos adecuada para su organización).

![Aplicación en la nube de Microsoft Intune](./media/terms-of-use/cloud-app-intune.png)

> [!NOTE]
> No se admite la aplicación de inscripción de Intune para [condiciones de uso de cada dispositivo](#per-device-terms-of-use).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P: ¿Cómo se puede ver si un usuario ha aceptado los términos de uso?**<br />
R: En la hoja Términos de uso, haga clic en el número situado bajo **Aceptado**. También puede ver o buscar la actividad de aceptación en los registros de auditoría de Azure AD. Para más información, consulte Ver quién los ha aceptado y rechazado y [Visualización de registros de auditoría de Azure AD](#view-azure-ad-audit-logs).

**P: ¿Cuánto tiempo se almacena la información?**<br />
R: Recuentos de los usuarios en los términos de los informes de uso y quién ha aceptado o rechazado se almacenan durante la vida de los términos de uso. Los registros de auditoría de Azure AD se almacenan durante 30 días.

**P: ¿Por qué veo un número diferente de consentimientos en términos de uso de informes frente a Azure AD los registros de auditoría?**<br />
R: Los términos de los informes de uso se almacena durante la vigencia de que los términos de uso, mientras que la auditoría de Azure AD, los registros se almacenan durante 30 días. Además, los términos de uso informe solo muestra el estado de consentimiento actual de los usuarios. Por ejemplo, si un usuario rechaza y, a continuación, acepta los términos de uso informe solo mostrará ese usuario acepte. Si necesita ver el historial, puede usar los registros de auditoría de Azure AD.

**P: ¿Si editar los detalles de los términos de uso, requieren los usuarios que acepten de nuevo?**<br />
R: No, si un administrador edita los detalles de los términos de uso (nombre, nombre para mostrar, requieren que los usuarios expandir o agregar un idioma), no requiere los usuarios que acepten los términos de nuevo.

**P: ¿Puedo actualizar una existente documento condiciones de uso?**<br />
R: Actualmente, no se puede actualizar una existente documento condiciones de uso. Para cambiar los términos de uso de documentos, tendrá que crear nuevos términos de la instancia de uso.

**P: ¿Si los hipervínculos están en los términos de uso de documentos PDF, los usuarios finales podrán hacer clic en ellos?**<br />
R: El archivo PDF se representa de manera predeterminada como un archivo JPEG, por lo que no se puede hacer clic en los hipervínculos. Los usuarios tiene la opción de seleccionar **¿Tiene problemas con la visualización? Haga clic aquí**, que muestra el PDF de manera nativa, donde sí se admiten los hipervínculos.

**P: ¿Términos de uso admitan varios idiomas?**<br />
R: Sí. Actualmente hay 108 distintos idiomas, un administrador puede configurar para un único términos de uso. Un administrador puede cargar varios documentos PDF y etiquetarlos con un idioma correspondiente (hasta 108). Cuando los usuarios finales inician sesión, nos fijamos en su preferencia de idioma del navegador y mostramos el documento coincidente. Si no hay ninguna coincidencia, mostraremos el documento predeterminado, que es el primer documento que se carga.

**P: ¿Cuándo es los términos de uso desencadenadas?**<br />
R: Los términos de uso se desencadena durante la experiencia de inicio de sesión.

**P: ¿Qué aplicaciones puedo destino los términos de uso?**<br />
R: Puede crear una directiva de acceso condicional en las aplicaciones empresariales que usan autenticación moderna. Para más información, consulte [aplicaciones empresariales](./../manage-apps/view-applications-portal.md).

**P: ¿Puedo agregar varios términos de uso para una aplicación o usuario determinado?**<br />
R: Sí, mediante la creación de varias directivas de acceso condicional cuyo destino sean dichos grupos o aplicaciones. Si un usuario se encuentra en el ámbito de varios términos de uso, aceptar uno de los términos de uso a la vez.

**P: ¿Qué ocurre si un usuario no acepta los términos de uso?**<br />
R: Se bloqueará al usuario y no podrá obtener acceso a la aplicación. El usuario tendría que iniciar sesión de nuevo y aceptar las condiciones con el fin de obtener acceso.

**P: ¿Es posible unaccept términos de uso que aceptó previamente?**<br />
R: También puede [revisión aceptado previamente términos de uso](#how-users-can-review-their-terms-of-use), pero actualmente no hay ninguna manera unaccept.

**P: ¿Qué ocurre si también utilizo los términos y condiciones de Intune?**<br />
R: Si ha configurado ambos términos de uso de Azure AD y [Intune términos y condiciones](/intune/terms-and-conditions-create), le pedirá al usuario que acepte tanto. Para obtener más información, consulte la entrada de blog [Choosing the right Terms solution for your organization](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409) (Elección de la solución de términos adecuada para su organización).

**P: ¿Qué puntos de conexión usa los términos de uso del servicio para la autenticación?**<br />
R: Términos de uso utiliza los siguientes puntos de conexión para la autenticación: https://tokenprovider.termsofuse.identitygovernance.azure.com y https://account.activedirectory.windowsazure.com. Si su organización tiene una lista de permitidos de direcciones URL para la inscripción, deberá agregar estos puntos de conexión a la lista de permitidos, junto con los puntos de conexión de Azure AD para iniciar sesión.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de inicio rápido: Solicitar la aceptación de los términos de uso antes de acceder a aplicaciones en la nube](require-tou.md)
- [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md)
