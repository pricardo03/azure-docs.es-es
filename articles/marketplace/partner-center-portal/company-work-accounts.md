---
title: Cuentas profesionales y el Centro de partners
description: Cómo comprobar si su empresa tiene una cuenta profesional configurada con Microsoft, crear una nueva cuenta profesional o configurar varias cuentas profesionales para usar con el Centro de partners.
author: mattwojo
manager: evansma
ms.author: parthp
ms.service: marketplace
ms.topic: how-to
ms.date: 05/30/2019
ms.openlocfilehash: df8ab370e8874e07f8985ecfb3a772848a2e2b21
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806287"
---
# <a name="company-work-accounts-and-partner-center"></a>Cuentas profesionales y el Centro de partners

El Centro de partners usa cuentas profesionales de empresa, también conocidas como inquilinos de Azure Active Directory (AD) para administrar el acceso de cuenta para varios usuarios, controlar permisos, hospedar grupos y aplicaciones y mantener los datos de perfil. Al vincular el dominio de cuenta de correo electrónico profesional de su empresa a su cuenta de Centro de partners, el personal de su empresa pueden iniciar sesión en el Centro de partners para administrar las ofertas de Marketplace mediante los nombres de usuario y contraseñas de sus propias cuentas profesionales.

## <a name="check-whether-your-company-already-has-a-work-account"></a>Comprobación de si su empresa ya tiene una cuenta profesional

Si su empresa se ha suscrito a un servicio en la nube de Microsoft, como Azure, Microsoft Intune u Office 365, ya tiene un dominio de cuenta de correo electrónico profesional (también denominada inquilino de Azure Active Directory) que se puede usar con el Centro de partners.

Siga estos pasos para comprobar:
1. Inicie sesión en Azure Portal en https://portal.azure.com.
2. Seleccione **Azure Active Directory** en el menú de navegación a la izquierda y, a continuación, seleccione **Nombres de dominio personalizados**.
3. Si ya tiene una cuenta profesional, su nombre de dominio se mostrará en la lista.

Si su empresa no tiene aún una cuenta profesional, se creará una automáticamente durante el proceso de inscripción en el Centro de partners.

## <a name="set-up-multiple-work-accounts"></a>Configuración de varias cuentas profesionales

Antes de decidirse a usar una cuenta profesional existente, considere cuántos usuarios en la cuenta profesional necesitará acceso al Centro de partners. Si tiene usuarios en la cuenta profesional que no necesitan tener acceso al Centro de partners, puede considerar la creación de varias cuentas profesionales, para que solo aquellos usuarios que necesitan acceso al Centro de partners estén representados en una cuenta determinada.

## <a name="create-a-new-work-account"></a>Creación de una cuenta profesional

Para crear una nueva cuenta profesional para su empresa, siga los pasos a continuación. Es posible que tenga que solicitar asistencia de la persona que tenga permisos administrativos en la cuenta de Microsoft Azure de su compañía.

1. Inicie sesión en el [Portal de Microsoft Azure](https://portal.azure.com).
2. En la barra de navegación de la izquierda, seleccione **Azure Active Directory** -> **Usuarios**.
3. Seleccione **Nuevo usuario** y cree una nueva cuenta profesional de Azure escribiendo un nombre y una dirección de correo electrónico. Asegúrese de que el **Rol del directorio** está establecido en **Usuario**, seleccione la casilla **Mostrar contraseña** en la parte inferior para ver la contraseña generada automáticamente y tome nota de la misma.
4. Seleccione **Crear** para guardar el nuevo usuario.

La dirección de correo electrónico para la cuenta de usuario tiene que ser un nombre de dominio comprobado en el directorio. Puede ver una lista de todos los dominios comprobados en el directorio seleccionando **Azure Active Directory** -> **Nombres de dominio personalizados** en el menú de navegación de la izquierda.

Para más información sobre cómo agregar dominios personalizados en Azure Active Directory, consulte [Incorporación o asociación de un dominio en Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain).

## <a name="troubleshoot-work-email-sign-in"></a>Solución de problemas de inicio de sesión de correo electrónico profesional

Si tiene dificultades para iniciar sesión en su cuenta profesional (también conocida como inquilino de Azure AD), busque el escenario en el diagrama siguiente que mejor coincida con su situación y siga los pasos recomendados.

![Diagrama de solución de problemas de inicio de sesión de cuenta profesional](./media/onboarding-aad-flow.png)

## <a name="next-steps"></a>Pasos siguientes

- [Administración de la cuenta de Marketplace comercial en el Centro de partners](./manage-account.md) 
