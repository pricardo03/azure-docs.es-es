---
title: Controles personalizados con acceso condicional de Azure AD
description: Aprenda cómo funcionan los controles personalizados en el acceso condicional de Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 02/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f1df037b66c72177a96f77231cee70782d04992
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77620708"
---
# <a name="custom-controls-preview"></a>Controles personalizados (versión preliminar)

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

## <a name="creating-custom-controls"></a>Creación de controles personalizados

Para crear un control personalizado, primero debe ponerse en contacto con el proveedor que desea utilizar. Cada proveedor que no es Microsoft tiene su propio proceso y requisitos para registrarse, suscribirse o pasar a formar parte del servicio e indicar que usted desea integrarse con el acceso condicional. En ese momento, el proveedor le proporcionará un bloque de datos en formato JSON. Estos datos permiten al proveedor y al acceso condicional trabajar conjuntamente para su inquilino, crean el nuevo control y definen cómo el acceso condicional puede indicar si los usuarios han realizado correctamente la comprobación con el proveedor.

No se pueden usar controles personalizados con la automatización de Identity Protection que requiere autenticación multifactor o elevar los roles en Privileged Identity Manager (PIM).

Copie los datos JSON y, a continuación, péguelos en el cuadro de texto relacionado. No realice ningún cambio en JSON a menos que entienda explícitamente dicho cambio. Realizar cualquier cambio podría interrumpir la conexión entre el proveedor y Microsoft y dejarle tanto a usted como a sus usuarios fuera de sus cuentas.

La opción para crear un control personalizado está en la sección **Administrar** de la página **Acceso condicional**.

![Control](./media/controls/82.png)

Al hacer clic en **Nuevo control personalizado** se abre una hoja con un cuadro de texto para los datos JSON del control.  

![Control](./media/controls/81.png)

## <a name="deleting-custom-controls"></a>Eliminación de controles personalizados

Para eliminar un control personalizado, primero debe asegurarse de que no se usa en ninguna directiva de acceso condicional. Cuando haya terminado:

1. Vaya a la lista Controles personalizados.
1. Haga clic en ...  
1. Seleccione **Eliminar**.

## <a name="editing-custom-controls"></a>Edición de controles personalizados

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
