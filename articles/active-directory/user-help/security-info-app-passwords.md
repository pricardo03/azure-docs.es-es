---
title: 'Configuración de contraseñas de aplicación mediante información de seguridad: Azure Active Directory | Microsoft Docs'
description: Configure contraseñas generadas automáticamente (contraseñas de aplicación) para usar con cada aplicación sin navegador, independientes de una contraseña normal, mediante información de seguridad.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 185c6e18a84369702de3bd0c398a9cc0c64b8217
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191387"
---
# <a name="manage-app-passwords-using-security-info-preview"></a>Administración de contraseñas de aplicación mediante información de seguridad (versión preliminar)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Determinadas aplicaciones sin navegador, como Outlook 2010, no admiten la verificación en dos pasos. Esta falta de compatibilidad significa que si usa la verificación en dos pasos, la aplicación no funcionará. Para solucionar este problema, puede crear una contraseña generada automáticamente para usar con cada aplicación sin explorador, independiente de la contraseña normal.

Al utilizar las contraseñas de aplicaciones, es importante recordar:

- Las contraseñas de aplicaciones se generan automáticamente y solo se escriben una vez por cada aplicación.

- Hay un límite de 40 contraseñas por usuario. Si intenta crear una después de ese límite, se le pedirá que elimine una contraseña existente antes de que se le permita crear una nueva.

- Utilice una contraseña de aplicación por dispositivo, no por aplicación. Por ejemplo, cree una única contraseña para todas las aplicaciones del equipo portátil y luego otra contraseña única para todas las aplicaciones del equipo de sobremesa.

    >[!Note]
    >Los clientes de Office 2013 (incluido Outlook) admiten nuevos protocolos de autenticación que se pueden usar con la verificación en dos pasos. Esta compatibilidad significa que después de activar la verificación en dos pasos, ya no necesitará contraseñas de aplicaciones para clientes de Office 2013. Para obtener más información, consulte el artículo [Cómo funciona la autenticación moderna para las aplicaciones de cliente de Office 2013 y Office 2016](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517).

## <a name="create-and-delete-app-passwords-using-security-info"></a>Creación y eliminación de contraseñas de aplicación mediante información de seguridad

Si utiliza la verificación en dos pasos con su cuenta profesional o educativa y su administrador ha activado la experiencia de información de seguridad, puede crear y eliminar las contraseñas de aplicación mediante el portal Mis aplicaciones.

Si el administrador no ha activado la experiencia de información de seguridad, debe seguir las instrucciones y la información de la sección [¿Qué son las contraseñas de aplicación en Azure Multi-Factor Authentication?](multi-factor-authentication-end-user-app-passwords.md).

### <a name="to-create-app-passwords-using-the-my-apps-portal"></a>Para crear contraseñas de aplicación mediante el portal Mis aplicaciones

1. Inicie sesión en su cuenta profesional o educativa.

2. Vaya a myapps.microsoft.com, seleccione su nombre de la esquina superior derecha de la página y luego seleccione **Perfil**.

3. En el área **Administrar cuenta**, seleccione **Editar la información de seguridad**.

    ![Pantalla de perfil con el vínculo Editar información de seguridad resaltado](media/security-info/security-info-profile.png)

4. En la pantalla **Garantizar la seguridad de la cuenta**, seleccione **Agregar información de seguridad**.

    ![Pantalla de información de seguridad con información existente editable](media/security-info/security-info-edit-add-info.png)

5. En la pantalla **Agregar información de seguridad**, seleccione **Contraseña de aplicación**.

6. En la pantalla **Creación de la contraseña de la aplicación**, escriba un nombre para la contraseña de aplicación y luego seleccione **Siguiente**.

    ![Pantalla con el nombre y la contraseña de aplicación](media/security-info/security-info-name-app-password.png)

7. Seleccione **Copiar** para copiar la contraseña en el portapapeles y luego seleccione **Siguiente**.

    ![Pantalla con la contraseña de aplicación para copiar](media/security-info/security-info-create-app-password.png)
    
8. Asegúrese de que la contraseña de aplicación aparece en la pantalla **Garantizar la seguridad de la cuenta**.

    ![Pantalla para garantizar la seguridad de la cuenta, con la contraseña de aplicación](media/security-info/security-info-keep-secure-app-password.png)

### <a name="to-delete-app-passwords-using-the-my-apps-portal"></a>Para eliminar contraseñas de aplicación mediante el portal Mis aplicaciones

1. En la pantalla **Garantizar la seguridad de la cuenta**, seleccione la **X** junto a la contraseña de la aplicación que desea eliminar.

    ![Pantalla para garantizar la seguridad de la cuenta, eliminar contraseña de aplicación](media/security-info/security-info-keep-secure-delete-app-password.png)

2. En la pantalla **Eliminar contraseña de aplicación**, seleccione **Eliminar**.

    ![Pantalla Eliminar contraseña de aplicación](media/security-info/security-info-keep-secure-delete-app-password2.png)

## <a name="next-steps"></a>Pasos siguientes

- Si necesita actualizar la información de seguridad, siga las instrucciones del artículo [Administración de la información de seguridad](security-info-manage-settings.md).

- Para información más general sobre la información de seguridad y qué puede hacer, consulte [Security info overview](user-help-security-info-overview.md) (Introducción a la información de seguridad). 
