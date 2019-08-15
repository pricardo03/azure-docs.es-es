---
title: Habilitar MFA para todos los administradores de Azure
description: Instrucciones para habilitar el administrador global
ms.service: security
ms.subservice: security-fundamentals
author: barclayn
manager: barbkess
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: e30ce71a66dd5cb6c810111d359660d875ae97a8
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927916"
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Aplicación de Multi-Factor Authentication (MFA) para los administradores de suscripciones

Al crear los administradores, incluida la cuenta de administrador global, es fundamental usar métodos de autenticación muy seguros.

Puede realizar la administración diaria mediante la asignación de roles de administrador específicos, como administrador de Exchange o administrador de contraseñas, para cuentas de usuario del personal de TI según sea necesario.
Además, habilitar [Azure Multi-factor Authentication (MFA)](../../active-directory/authentication/multi-factor-authentication.md) para los administradores agrega una segunda capa de seguridad a los inicios de sesión de usuario y las transacciones. Azure MFA también ayuda al equipo de TI a reducir la probabilidad de que se acceda a los datos de la organización mediante credenciales en peligro.

Por ejemplo:  Suponga que aplica Azure MFA para los usuarios y lo configura para usar una llamada de teléfono o un mensaje de texto como confirmación. Si las credenciales del usuario se ven comprometidas, el atacante no podrá acceder a los recursos dado que no tendrá acceso al teléfono del usuario. Las organizaciones que no agregan capas de protección de la identidad adicionales son más susceptibles a ataques de robo de credenciales, lo que puede llevar a poner en peligro la seguridad de los datos.

Una alternativa para las organizaciones que quieren mantener todo el control local de la autenticación es usar el [Servidor Microsoft Azure Multi-Factor Authentication ](../../active-directory/authentication/howto-mfaserver-deploy.md), también denominado MFA local. Mediante este método, podrá seguir aplicando Multi-Factor Authentication y mantener el servidor MFA local.

Para comprobar quién tiene privilegios de administrador en su organización, puede usar el siguiente comando de PowerShell de Microsoft Azure AD V2:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>Habilitación de MFA

Revise el funcionamiento de [MFA](../../active-directory/authentication/howto-mfa-mfasettings.md) antes de continuar.

Siempre y cuando los usuarios tengan licencias que incluyen Azure Multi-Factor Authentication, no hay nada que se pueda hacer para activar Azure MFA. Puede iniciar la solicitud de la verificación en dos pasos en cada usuario individual. Las licencias que habilitan Azure MFA son:

- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Activación de la verificación en dos pasos para los usuarios

Utilice uno de los procedimientos enumerados en el artículo [Exigencia de verificación en dos pasos para un usuario o grupo](../../active-directory/authentication/howto-mfa-userstates.md) para empezar a usar Azure MFA. Puede elegir exigir la verificación en dos pasos para todos los inicios de sesión o crear directivas de acceso condicional para exigir la verificación en dos pasos únicamente cuando lo necesite.

