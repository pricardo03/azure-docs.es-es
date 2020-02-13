---
title: Controles de acceso en el acceso condicional de Azure Active Directory
description: Aprenda cómo funcionan los controles de acceso en el acceso condicional de Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 12/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ad8894078a15bf37a5383cdff3721f4bf7be910
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77186227"
---
# <a name="what-are-access-controls-in-azure-active-directory-conditional-access"></a>¿Qué son los controles de acceso en el acceso condicional de Azure Active Directory?

Con el [acceso condicional de Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), puede controlar el modo en que los usuarios autorizados acceden a las aplicaciones en la nube. En una directiva de acceso condicional, se define la respuesta ("haga esto") al motivo para desencadenarla ("cuando esto suceda").

![Control](./media/controls/10.png)

En el contexto del acceso condicional:

- "**Cuando esto suceda**" son las **condiciones** y
- "**Entonces haga esto**" son los **controles de acceso**.

La combinación de una declaración de condición con los controles representa una directiva de acceso condicional.

![Control](./media/controls/61.png)

Cada control es un requisito que la persona o el sistema que inicia sesión deben cumplir, o una restricción en lo que el usuario puede hacer tras iniciar sesión.

Existen dos tipos de controles:

- **Controles de concesión**: para obtener acceso
- **Controles de sesión**: para restringir el acceso dentro de una sesión

En este tema se explican los distintos controles disponibles en el acceso condicional de Azure AD. 

## <a name="grant-controls"></a>Controles de concesión

Con los controles de concesión, puede bloquear el acceso por completo o permitirlo con requisitos adicionales seleccionando los controles que desee. Con varios controles, puede requerir:

- Que todos los controles se cumplan (*Y*)
- Que un control seleccionado se cumpla (*O*)

![Control](./media/controls/18.png)

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Puede utilizar este control para requerir la autenticación multifactor para tener acceso a la aplicación en la nube especificada. Este control es compatible con los siguientes proveedores multifactor:

- Azure Multi-Factor Authentication
- Un proveedor de autenticación multifactor de terceros, en combinación con Servicios de federación de Active Directory (AD FS).

La autenticación multifactor ayuda a proteger los recursos del acceso por parte de un usuario no autorizado que podría haber conseguido las credenciales principales de un usuario válido.

### <a name="compliant-device"></a>Dispositivos compatible

Puede configurar directivas de acceso condicional basadas en el dispositivo. El objetivo de una directiva de acceso condicional basada en el dispositivo es conceder acceso a las aplicaciones en la nube seleccionadas solo desde [dispositivos administrados](require-managed-devices.md). Requerir que un dispositivo esté marcado como compatible es una opción para limitar el acceso a los dispositivos administrados. Un dispositivo se puede marcar como conforme con Intune (para cualquier sistema operativo del dispositivo) o por el sistema MDM de terceros para dispositivos Windows 10. No se pueden utilizar sistemas MDM de terceros con otros sistemas operativos que no sean Windows 10. 

El dispositivo tiene que estar registrado en Azure AD antes de que se pueda marcar como compatible. Para registrar un dispositivo, tiene tres opciones: 

- Dispositivos registrados en Azure AD
- Dispositivos unidos a Azure AD  
- Dispositivos híbridos unidos a Azure AD

Estas tres opciones se describen en el artículo [¿Qué es la identidad de dispositivos?](../devices/overview.md)

Para más información, consulte [Uso obligatorio de dispositivos administrados para el acceso a aplicaciones en la nube mediante el acceso condicional](require-managed-devices.md).

### <a name="hybrid-azure-ad-joined-device"></a>Dispositivo unido a Azure AD híbrido

El uso obligatorio de un dispositivo unido a un dominio de Azure AD híbrido es otra opción que puede usar para configurar directivas de acceso condicional basado en el dispositivo. Este requisito hace referencia a equipos de escritorio, equipos portátiles y tabletas de empresa con Windows que están unidos a una instancia local de Active Directory. Si se selecciona esta opción, la directiva de acceso condicional concede acceso a los intentos de acceso realizados con dispositivos unidos a su instancia de Active Directory local y a su instancia de Azure Active Directory. Los dispositivos Mac no admiten la unión con Azure AD híbrido.

Para más información, consulte [Configuración de directivas de acceso condicional basadas en dispositivos de Azure Active Directory](require-managed-devices.md).

### <a name="approved-client-app"></a>Aplicaciones cliente aprobadas

Dado que los empleados emplean dispositivos móviles tanto para las tareas de trabajo como para uso personal, debe tener la capacidad de proteger los datos de la empresa a los que se accede con dispositivos, incluso en el caso en el que no estén administrados por el usuario.
Puede usar [directivas de protección de aplicaciones de Intune](https://docs.microsoft.com/intune/app-protection-policy) para ayudar a proteger los datos de su compañía independientes de cualquier solución de administración de dispositivos móviles (MDM).

Con las aplicaciones cliente aprobadas, puede requerir que una aplicación cliente que intente obtener acceso a las aplicaciones en la nube admita [directivas de protección de aplicaciones de Intune](https://docs.microsoft.com/intune/app-protection-policy). Por ejemplo, puede restringir el acceso a Exchange Online para la aplicación Outlook. Una directiva de acceso condicional que requiere aplicaciones cliente aprobadas se conoce también como [directiva de acceso condicional basada en la aplicación](app-based-conditional-access.md). Para una lista de las aplicaciones cliente admitidas, consulte el [requisito de las aplicaciones cliente aprobadas](concept-conditional-access-grant.md#require-approved-client-app).

### <a name="app-protection-policy-preview"></a>Directiva de protección de aplicaciones (versión preliminar)

Dado que los empleados emplean dispositivos móviles tanto para las tareas de trabajo como para uso personal, debe tener la capacidad de proteger los datos de la empresa a los que se accede con dispositivos, incluso en el caso en el que no estén administrados por el usuario.
Puede usar [directivas de protección de aplicaciones de Intune](https://docs.microsoft.com/intune/app-protection-policy) para ayudar a proteger los datos de su compañía independientes de cualquier solución de administración de dispositivos móviles (MDM).

Con la directiva de protección de aplicaciones, se puede limitar el acceso a las aplicaciones cliente que han notificado a Azure AD que han recibido [directivas de protección de aplicaciones de Intune](https://docs.microsoft.com/intune/app-protection-policy). Por ejemplo, puede restringir el acceso de Exchange Online a la aplicación de Outlook que tenga una directiva de protección de aplicaciones de Intune. Una directiva de acceso condicional que requiere una directiva de protección de aplicaciones también se conoce como [directiva de acceso condicional basada en la protección de aplicaciones](concept-conditional-access-session.md#application-enforced-restrictions). 

Para que una aplicación pueda marcarse como protegida por una directiva, el dispositivo debe estar registrado en Azure AD.

Para ver una lista de directiva de aplicaciones cliente protegidas por una directiva admitidas, consulte [Requisito de directiva de protección de aplicaciones](concept-conditional-access-session.md#application-enforced-restrictions).

### <a name="terms-of-use"></a>Términos de uso

Puede requerir a un usuario en el inquilino que consienta las condiciones de uso antes de concederle acceso a un recurso. Como administrador, puede configurar y personalizar las condiciones de uso mediante la carga de un documento PDF. Si un usuario se encuentra en el ámbito de control, el acceso a una aplicación solo se concede si se han aceptado las condiciones de uso.

## <a name="custom-controls-preview"></a>Controles personalizados (versión preliminar)

Los controles personalizados son una funcionalidad de la edición Azure Active Directory Premium P1. Al utilizar controles personalizados, los usuarios se redirigen a un servicio compatible para satisfacer más requisitos fuera de Azure Active Directory. Para satisfacer este control, el explorador de un usuario se redirige al servicio externo, lleva a cabo todas las actividades de autenticación y validación necesarias y luego se vuelve a redirigir a Azure Active Directory. Azure Active Directory comprueba la respuesta y, si el usuario se autenticó o validó correctamente, seguirá en el flujo de acceso condicional.

Estos controles permiten el uso de determinados servicios externos o personalizados como controles de acceso condicional y generalmente amplían las funcionalidades de acceso condicional.

Los proveedores que actualmente ofrecen un servicio compatible incluyen:

- [Duo Security](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [GSMA](https://mobileconnect.io/azure/)
- [Identidad de ping](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- [RSA](https://community.rsa.com/docs/DOC-81278)
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [Thales (Gemalto)](https://resources.eu.safenetid.com/help/AzureMFA/Azure_Help/Index.htm)
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Para más información sobre esos servicios, póngase en contacto con los proveedores directamente.

### <a name="creating-custom-controls"></a>Creación de controles personalizados

Para crear un control personalizado, primero debe ponerse en contacto con el proveedor que desea utilizar. Cada proveedor que no es Microsoft tiene su propio proceso y requisitos para registrarse, suscribirse o pasar a formar parte del servicio e indicar que usted desea integrarse con el acceso condicional. En ese momento, el proveedor le proporcionará un bloque de datos en formato JSON. Estos datos permiten al proveedor y al acceso condicional trabajar conjuntamente para su inquilino, crean el nuevo control y definen cómo el acceso condicional puede indicar si los usuarios han realizado correctamente la comprobación con el proveedor.

No se pueden usar controles personalizados con la automatización de Identity Protection que requiere autenticación multifactor o elevar los roles en Privileged Identity Manager (PIM).

Copie los datos JSON y, a continuación, péguelos en el cuadro de texto relacionado. No realice ningún cambio en JSON a menos que entienda explícitamente dicho cambio. Realizar cualquier cambio podría interrumpir la conexión entre el proveedor y Microsoft y dejarle tanto a usted como a sus usuarios fuera de sus cuentas.

La opción para crear un control personalizado está en la sección **Administrar** de la página **Acceso condicional**.

![Control](./media/controls/82.png)

Al hacer clic en **Nuevo control personalizado** se abre una hoja con un cuadro de texto para los datos JSON del control.  

![Control](./media/controls/81.png)

### <a name="deleting-custom-controls"></a>Eliminación de controles personalizados

Para eliminar un control personalizado, primero debe asegurarse de que no se usa en ninguna directiva de acceso condicional. Cuando haya terminado:

1. Vaya a la lista Controles personalizados.
1. Haga clic en ...  
1. Seleccione **Eliminar**.

### <a name="editing-custom-controls"></a>Edición de controles personalizados

Para editar un control personalizado, debe eliminar el control actual y crear un nuevo control con la información actualizada.

## <a name="session-controls"></a>Controles de sesión

Los controles de sesión permiten limitar la experiencia desde una aplicación en la nube. Los controles de sesión son aplicados por aplicaciones en la nube y se basan en información adicional sobre la sesión proporcionada por Azure AD a la aplicación.

![Control](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Usar restricciones que exige la aplicación

Puede usar este control para requerir que Azure AD transmita la información del dispositivo a la aplicación en la nube. La información del dispositivo permite que las aplicaciones de nube sepan si una conexión se inicia desde un dispositivo compatible o unido al dominio. Este control solo admite Exchange Online y SharePoint Online como aplicaciones de nube seleccionadas. Cuando se selecciona, la aplicación de nube usa la información del dispositivo para proporcionar a los usuarios una experiencia completa o limitada, según el estado del dispositivo.

Para obtener más información, consulte:

- [Habilitación del acceso limitado con SharePoint Online](https://aka.ms/spolimitedaccessdocs)
- [Habilitación del acceso limitado con Exchange Online](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>Pasos siguientes

- Si quiere saber cómo configurar una directiva de acceso condicional, vea [Requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md).
- Si está listo para configurar directivas de acceso condicional para su entorno, vea [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md).
