---
title: 'Administración de la configuración de la verificación en dos pasos: Azure Active Directory | Microsoft Docs'
description: Administre cómo usar Azure Multi-Factor Authentication, incluida la modificación de la información de contacto o la configuración de los dispositivos.
services: active-directory
keywords: cliente de multi-factor authentication, problema de autenticación, identificador de correlación
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 433c2d712ca4867a5ec59f86c333511070b6d507
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665056"
---
# <a name="manage-your-settings-for-two-step-verification"></a>Administración de la configuración de la comprobación en dos pasos
Este artículo ofrece respuestas a preguntas acerca de cómo actualizar la configuración de la verificación en dos pasos o la autenticación multifactor. Si tiene problemas para iniciar sesión en su cuenta, consulte [Problemas con la comprobación en dos pasos](multi-factor-authentication-end-user-troubleshoot.md) para obtener ayuda para la solución de problemas.

## <a name="where-to-find-the-settings-page"></a>Dónde encontrar la página de configuración
Según cómo su compañía configure Azure Multi-Factor Authentication, existen algunos lugares donde puede cambiar los ajustes, como su número de teléfono.

Si su compañía admite el envío de una dirección URL específica o pasos para administrar la verificación en dos pasos, siga las instrucciones. De lo contrario, las instrucciones siguientes deben funcionar para los demás. Si se siguen estos pasos, pero no ve las mismas opciones, significa que su trabajo o escuela personalizó su propio portal. Consulte al administrador para obtener el vínculo a su portal de Azure Multi-Factor Authentication.

**Vaya a la página Comprobación de seguridad adicional**

- Vaya a https://aka.ms/MFASetup.

    ![Página de proofup](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

Si hacer clic en ese vínculo no funciona, también puede seguir los pasos a continuación para ir a la página **Comprobación de seguridad adicional**:

1. Inicio de sesión en [https://myapps.microsoft.com](https://myapps.microsoft.com)  

2. Seleccione el nombre de cuenta en la parte superior derecha y, después, seleccione **Perfil**.

3. Seleccione **Comprobación de seguridad adicional**.  

    ![MyApps](./media/multi-factor-authentication-end-user-manage-settings/myapps1.png)

4. La página de comprobación de seguridad adicional se carga con la configuración.

    ![Página de proofup](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Deseo cambiar mi número de teléfono o agregar un número secundario
Es importante configurar un número de teléfono de autenticación secundario.  Dado que el número de teléfono principal y la aplicación móvil probablemente están en el mismo teléfono, el número de teléfono secundario es la forma más rápida, podrá regresar a tu cuenta si se pierde o lo roban el teléfono.

> [!NOTE]
> Si no dispone de acceso a su número de teléfono principal y necesita ayuda para encontrar su cuenta, consulte el artículo [Problemas con la verificación en dos pasos](multi-factor-authentication-end-user-troubleshoot.md) para obtener más ayuda.  

**Para cambiar el número de teléfono principal:**  

1. En la página **Comprobación de seguridad adicional**, seleccione el cuadro de texto con el número de teléfono actual y modifíquelo con el nuevo número de teléfono.  
2. Seleccione **Guardar**.  
3. Si este es el número de teléfono que se utiliza para la opción de comprobación preferida, tiene que comprobar el nuevo número para poder guardarlo.  

**Para agregar un número de teléfono secundario:**  

1. En la página de comprobación de seguridad adicional, active la casilla junto al **teléfono de autenticación alternativo.**  
2. En el cuadro de texto, escriba el número de teléfono secundario.  
3. Seleccione **Guardar** y los cambios habrán finalizado.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>Requisito de nueva verificación en dos pasos en un dispositivo marcado como de confianza

Dependiendo de la configuración de la organización, es posible que tenga una casilla que indica "Don't ask again for **X** days" (No volver a preguntar en X días) al realizar la verificación en dos pasos en el explorador. Si marca este cuadro y pierde el dispositivo o piensa que se ha comprometido la seguridad de su cuenta, debe restaurar la verificación en dos pasos en todos los dispositivos.

1. En la página Comprobación de seguridad adicional, seleccione **Restaurar Multi-Factor Authentication en dispositivos en los que se confió anteriormente**.
2. La próxima vez que inicie sesión en cualquier dispositivo, se le pedirá que realice la verificación en dos pasos.

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>¿Cómo puedo quitar Microsoft Authenticator del dispositivo anterior y moverlo a uno nuevo?
Cuando desinstala la aplicación del dispositivo o restablece el dispositivo, no se quita la activación en el back-end. Para más información, vea [Microsoft Authenticator](user-help-auth-app-download-install.md).

## <a name="next-steps"></a>Pasos siguientes
* Obtener sugerencias para solucionar problemas y necesita ayuda [Problemas con la comprobación en dos pasos](multi-factor-authentication-end-user-troubleshoot.md)
* Configure las [contraseñas de aplicación](multi-factor-authentication-end-user-app-passwords.md) para las aplicaciones que no admitan la verificación en dos pasos.
