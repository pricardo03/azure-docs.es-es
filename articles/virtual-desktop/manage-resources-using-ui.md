---
title: 'Implementación de una herramienta de administración: Azure'
description: Cómo instalar una herramienta de interfaz de usuario para administrar los recursos de la versión preliminar de Windows Virtual Desktop.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/04/2019
ms.author: v-chjenk
ms.openlocfilehash: 275fec5fb696a7e1352bbddccd288863e984b796
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304543"
---
# <a name="tutorial-deploy-a-management-tool"></a>Tutorial: Implementación de una herramienta de administración

La herramienta de administración proporciona una interfaz de usuario para administrar recursos de la versión preliminar de Microsoft Virtual Desktop. En este tutorial, obtendrá información sobre cómo implementar y conectarse a la herramienta de administración.

>[!NOTE]
>Estas instrucciones son específicas de una configuración de Windows Virtual Desktop, versión preliminar, concreta que se puede usar con los procesos existentes de su organización.

## <a name="important-considerations"></a>Consideraciones importantes

Puesto que la aplicación requiere de su consentimiento para interactuar con Windows Virtual Desktop, esta herramienta no es compatible con escenarios de negocio a negocio (B2B). La suscripción de cada inquilino de Azure Active Directory (AAD) necesitará su propia implementación independiente de la herramienta de administración.

Esta herramienta de administración es un ejemplo. Microsoft proporcionará actualizaciones de calidad y seguridad importantes. El [código fuente está disponible en GitHub](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy). Se aconseja que los clientes y partners personalicen la herramienta para que se ajuste a sus necesidades empresariales.

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>¿Qué se necesita para ejecutar la plantilla de Azure Resource Manager?

Antes de implementar la plantilla de Azure Resource Manager, necesitará un usuario de Azure Active Directory para implementar la interfaz de usuario de administración. Dicho usuario debe:

- Deshabilitar la autenticación multifactor (MFA) de Azure.
- Tener los permisos para crear recursos en la suscripción de Azure.
- Tener los permisos para crear una aplicación de Azure AD. Siga estos pasos para comprobar si el usuario tiene los [permisos necesarios](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions).

Después de implementar la plantilla de Azure Resource Manager, debe iniciar la interfaz de usuario de administración que va a validar. Dicho usuario debe:
- Tener una asignación de roles para ver o editar el inquilino de Windows Virtual Desktop.

## <a name="run-the-azure-resource-manager-template-to-provision-the-management-ui"></a>Ejecutar la plantilla de Azure Resource Manager para aprovisionar la interfaz de usuario de administración.

Antes de empezar, asegúrese de que las aplicaciones cliente y servidor tienen su consentimiento al visitar la [página de consentimiento de Windows Virtual Desktop](https://rdweb.wvd.microsoft.com) de la instancia de Azure Active Directory (AAD) representada.

Siga las instrucciones para implementar la plantilla de Azure Resource Manager:

1. Vaya a la [página RDS-Templates de Azure de GitHub](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy).
2. Implemente la plantilla en Azure.
    - Si va a implementarla en una suscripción Enterprise, desplácese hacia abajo y seleccione **Implementar en Azure**. Consulte la [Guía sobre los parámetros de la plantilla](#guidance-for-template-parameters).
    - Si va a realizar la implementación en una suscripción de proveedor de soluciones en la nube, siga estas instrucciones para implementar en Azure:
        1. Desplácese hacia abajo, haga clic con el botón derecho en **Implementar en Azure** y, después, seleccione **Copy Link Location** (Copiar ubicación del vínculo).
        2. Abra un editor de texto como el Bloc de notas y pegue el vínculo ahí.
        3. Justo después de <https://portal.azure.com/> y antes de la almohadilla (#), escriba una arroba (@) seguida del nombre de dominio del inquilino. Este es un ejemplo del formato: <https://portal.azure.com/@Contoso.onmicrosoft.com#create/>.
        4. Inicie sesión en el Azure Portal como un usuario con permisos de administrador o colaborador en la suscripción del proveedor de soluciones en la nube.
        5. Pegue el vínculo que ha copiado en el editor de texto en la barra de direcciones.

### <a name="guidance-for-template-parameters"></a>Guía sobre los parámetros de la plantilla
Aquí le mostramos cómo especificar parámetros de configuración para la herramienta:

- La dirección URL de agente de Escritorio remoto:  <https://rdbroker.wvd.microsoft.com/>
- La dirección URL de recursos:  <https://mrs-prod.ame.gbl/mrs-RDInfra-prod>
- Use sus credenciales de AAD con la opción de MFA deshabilitada para iniciar sesión en Azure. Consulte [Qué se necesita para ejecutar la plantilla de Azure Resource Manager](#what-you-need-to-run-the-azure-resource-manager-template).
- Use un nombre único para la aplicación que se registrará en Azure Active Directory para la herramienta de administración. Por ejemplo, Apr3UX.

## <a name="provide-consent-for-the-management-tool"></a>Consentimiento para la herramienta de administración

Después de completar la plantilla de Azure Resource Manager de GitHub, encontrará un grupo de recursos que contiene dos instancias de App Services junto con un plan de App Service en Azure Portal.

Antes de iniciar sesión y usar la herramienta de administración, deberá dar su consentimiento para la nueva aplicación de Azure Active Directory que está asociada con la herramienta de administración. Al dar su consentimiento, permitirá que la herramienta de administración realice llamadas de administración a Windows Virtual Desktop en nombre del usuario que ha iniciado sesión en la herramienta.

![Captura de pantalla que muestra los permisos proporcionados al dar su consentimiento a la herramienta de administración de la interfaz de usuario.](media/management-ui-delegated-permissions.png)

Para determinar qué usuario puede usarse para iniciar sesión en la herramienta, vaya a la [página de configuración de usuario de Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) y tome nota del valor de **Los usuarios pueden permitir que las aplicaciones accedan a los datos de la compañía en su nombre**.

![Captura de pantalla que muestra si los usuarios pueden otorgar consentimiento a las aplicaciones solo para su usuario.](media/management-ui-user-consent-allowed.png)

- Si el valor está establecido en **Sí**, puede iniciar sesión con cualquier cuenta de usuario en Azure Active Directory y dar su consentimiento solo para ese usuario. Sin embargo, si más adelante inicia sesión en la herramienta de administración con un usuario diferente, debe otorgar nuevamente el mismo consentimiento.
- Si el valor está establecido en **No**, debe iniciar sesión como administrador global en Azure Active Directory y proporcionar el consentimiento del administrador para todos los usuarios en el directorio. Ningún otro usuario se encontrará con una petición de consentimiento.


Una vez que decida qué usuario usará para dar su consentimiento, siga estas instrucciones para otorgar consentimiento a la herramienta:

1. Vaya a los recursos de Azure, seleccione el recurso de Azure App Services con el nombre proporcionado en la plantilla (por ejemplo, Apr3UX) y vaya a la dirección URL asociada a él. Por ejemplo, <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Inicie sesión con la cuenta de usuario de Azure Active Directory adecuada.
3. Si se ha autenticado con un administrador global, ahora puede marcar la casilla de verificación para otorgar su **Consentimiento en nombre de la organización**. Seleccione **Aceptar** para dar su consentimiento.
   
   ![Captura de pantalla que muestra la página completa de consentimiento que verá el usuario o administrador.](media/management-ui-consent-page.png)

Ahora se abrirá la herramienta de administración.

## <a name="use-the-management-tool"></a>Uso de la herramienta de administración

Después de proporcionar su consentimiento para la organización o para un usuario específico, puede tener acceso a la herramienta de administración en cualquier momento.

Siga estas instrucciones para iniciar la herramienta:

1. Seleccione el recurso de Azure App Services con el nombre proporcionado en la plantilla (por ejemplo, Apr3UX) y vaya a la dirección URL asociada a él. Por ejemplo, <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Inicie sesión con sus credenciales de Windows Virtual Desktop.
3. Cuando se le pida que elija un grupo de inquilinos, seleccione **Default Tenant Group** (grupo de inquilinos predeterminado) en la lista desplegable.

> [!NOTE]
> Si tiene un grupo de inquilinos personalizado, escriba el nombre manualmente en lugar de elegirlo en la lista desplegable.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a implementar y conectarse a la herramienta de administración, puede obtener más información sobre cómo usar Azure Service Health para supervisar problemas de servicio y avisos de estado.

> [!div class="nextstepaction"]
> [Tutorial de configuración de alertas de servicio](./set-up-service-alerts.md)
