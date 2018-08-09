---
title: Registro convergente para SSPR y MFA de Azure AD
description: Registro de autoservicio de restablecimiento de contraseña y Multi-Factor Authentication de Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: cdd100d113c3fbeda8ac840d479b065d648ac3ff
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415658"
---
# <a name="converged-registration-for-self-service-password-reset-and-azure-multi-factor-authentication"></a>Registro convergente para autoservicio de restablecimiento de contraseña y Azure Multi-Factor Authentication

Hasta ahora, los usuarios debían registrar métodos de autenticación para Azure Multi-factor Authentication (MFA) y autoservicio de restablecimiento de contraseña (SSPR) en dos portales diferentes. Muchos usuarios se sentían confundidos por el hecho de usar métodos similares para Azure MFA y SSPR y no registrarse en ambos portales. Esta disparidad llevaba a algunos usuarios a no poder usar Azure MFA o SSPR cuando era necesario y, como resultado de ello, a llamadas al departamento de soporte técnico y el posible descontento del usuario. Ahora, los usuarios pueden registrase una vez y obtener las ventajas de Azure MFA y SSPR, lo que elimina la necesidad de registrar sus métodos de autenticación para estas características dos veces.  

Antes de habilitar esta nueva experiencia para su organización, se recomienda que revise este artículo, así como nuestra [documentación de usuario final](https://aka.ms/securityinfoguide) para comprender el efecto que tendrá la experiencia en los usuarios. Para entrenar y preparar a los usuarios para la nueva experiencia y garantizar un lanzamiento satisfactorio, puede usar la [documentación de usuario final](https://aka.ms/securityinfoguide).

|     |
| --- |
| El registro convergente para autoservicio de restablecimiento de contraseña y Azure Multi-Factor Authentication es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Para permitir que los usuarios registren métodos de autenticación para Azure Multi-Factor Authentication y autoservicio de restablecimiento de contraseña en una única experiencia, realice los pasos siguientes:

1. Inicie sesión en Azure Portal como administrador global o administrador de usuarios.
2. Vaya a **Azure Active Directory**, **Configuración de usuario**, **Administrar la configuración de las características en versión preliminar del panel de acceso**.
3. En **Users can use preview features for registering and managing security info** (Los usuarios pueden usar las características en versión preliminar para registrar y administrar la información de seguridad), puede elegir habilitarlas para grupos de usuarios **Seleccionados** o para **Todos** los usuarios.

Los usuarios ahora pueden ir a [ https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) para registrar sus métodos de autenticación para SSPR y MFA. Para más información sobre lo que verán los usuarios en esta nueva experiencia, consulte nuestra [documentación de usuario final](https://aka.ms/securityinfoguide).  

> [!NOTE]
> Después de habilitar esta experiencia, los usuarios que registran o confirman su número de teléfono o aplicación móvil a través de la nueva experiencia tendrán la posibilidad de usarlos para MFA y SSPR, si estos métodos están habilitados en las directivas de MFA y SSPR. Si luego deshabilita esta experiencia, los usuarios que naveguen a la página de registro de SSPR anterior en aka.ms/ssprsetup deberán realizar MFA para poder acceder a la página.  

## <a name="how-it-works"></a>Cómo funciona

Si un usuario ha registrado anteriormente métodos de autenticación mediante experiencias de registro de SSPR y MFA independientes, no tendrá que volver a registrar esa información. Sin embargo, si la configuración exige que los usuarios se registren para MFA o SSPR, puede que se les indique que revisen su información de seguridad al iniciar sesión.

Si un usuario ha registrado un método que puede usarse para MFA, se le pedirá que realice MFA para poder acceder a la nueva experiencia.

Si ha forzado el registro para MFA o SSPR y aún no se ha registrado un usuario, estos usuarios deberán registrarse cuando inicien sesión.

Los usuarios a los que se les pide que se registren al iniciar sesión verán la siguiente experiencia:

![Registro convergido. Configurar los métodos con un nuevo usuario](./media/concept-registration-mfa-sspr-converged/concept-registration-add-methods.png)

> [!NOTE]
> Esta experiencia solo se muestra cuando se le pide a un usuario que se registre al iniciar sesión. Los usuarios que van directamente a la nueva experiencia en aka.ms/setupsecurityinfo verán una versión diferente de la experiencia, que se describe más adelante en este artículo.

Los métodos de autenticación mostrados cambiarán en función de los métodos habilitados en las directivas de MFA o SSPR. Se le pedirá al usuario que registre el número mínimo de métodos de autenticación necesarios para cumplir la directiva de MFA, la directiva de SSPR o ambas. Si hay flexibilidad en los métodos de autenticación que el usuario puede registrar, se puede seleccionar **Elegir la información de seguridad** para elegir otros métodos de autenticación.  

A diferencia de la experiencia anterior de registro de MFA, no se les pedirá a los usuarios que registren una contraseña de aplicación al recorrer la nueva experiencia de registro. En su lugar, deben seguir los pasos indicados en nuestro tutorial de contraseñas de aplicación para registrar las contraseñas de aplicación en la nueva experiencia.  

Una vez que un usuario completa el registro, se establecerá automáticamente su método de MFA predeterminado. Si el usuario ha registrado una aplicación autenticadora, el método predeterminado se establecerá en esa aplicación. Si el usuario no ha registrado una aplicación autenticadora y solo ha registrado su número de teléfono, el método predeterminado se establecerá en llamada de teléfono. Para cambiar sus valores predeterminados, los usuarios pueden ir a [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) y seleccionar **Cambiar valor predeterminado**.  

Si no se fuerza el registro, los usuarios pueden administrar sus propios métodos de autenticación en [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo). Si un usuario ha registrado previamente un método que puede usarse para realizar MFA, se le pedirá que realice MFA para poder acceder a la página.  

![Registro convergido. Editar métodos como un usuario registrado](./media/concept-registration-mfa-sspr-converged/concept-registration-edit-methods.png)

En esta página, los usuarios verán los métodos de autenticación previamente registrados y los métodos de autenticación registrados para ellos, como el teléfono del trabajo. Los usuarios también pueden agregar, editar o eliminar sus métodos de autenticación (excepto el teléfono del trabajo).  

Los registros de auditoría de esta nueva experiencia se encuentran en la categoría Métodos de autenticación del registro de auditoría.  

## <a name="known-issues"></a>Problemas conocidos

**El método MFA predeterminado se establece en llamada de teléfono cuando el usuario registra el teléfono mediante un mensaje de texto**
   * Algunos usuarios pueden advertir que su método de MFA predeterminado está establecido en llamada de teléfono después de registrar su número de teléfono mediante un mensaje de texto. Para resolver este problema, los usuarios pueden cambiar su método predeterminado mediante estas instrucciones. 

**El usuario no puede acceder a la nueva experiencia de registro después de que el administrador deshabilita su método predeterminado**
   * Es posible que algunos usuarios no puedan acceder a la nueva experiencia de registro si su administrador ha deshabilitado el método de MFA predeterminado anteriormente registrado. Este es un escenario de ejemplo: 
      1. Un usuario ha registrado anteriormente su número de teléfono y ha establecido su método predeterminado en llamada de teléfono.
      2. El administrador deshabilita la llamada de teléfono como método de MFA del inquilino.
      3. Se pide al usuario que se registre durante el inicio de sesión porque debe registrar un método adicional para cumplir la directiva de SSPR del inquilino.
      4. El usuario intenta registrarse, pero no puede acceder a la página porque no tiene un método predeterminado establecido y está atrapado en un bucle.

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a implementar el autoservicio de restablecimiento de contraseña de Azure AD](howto-sspr-deployment.md)

[Aprenda a exigir la autenticación multifactor al iniciar sesión](howto-mfa-getstarted.md)

[Documentación de configuración del método de autenticación de usuario final](https://aka.ms/securityinfoguide)