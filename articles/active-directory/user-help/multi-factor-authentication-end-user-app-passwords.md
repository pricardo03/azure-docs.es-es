---
title: Administración de contraseñas de aplicaciones en Azure Active Directory | Microsoft Docs
description: Esta página ayuda a los usuarios a entender qué son las contraseñas de aplicaciones y para qué se utilizan con respecto la verificación en dos pasos.
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.workload: identity
ms.service: active-directory
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 836f426be950e33031ff74276218d1ba59f1f2f7
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43300920"
---
# <a name="manage-app-passwords-for-two-step-verification"></a>Administración de las contraseñas de aplicaciones para la verificación en dos pasos

Determinadas aplicaciones sin explorador, como Outlook 2010, no admiten la verificación en dos pasos. Esta falta de compatibilidad significa que si usa la verificación en dos pasos, la aplicación no funcionará. Para solucionar este problema, puede crear una contraseña generada automáticamente para usar con cada aplicación sin explorador, independiente de la contraseña normal.

Al utilizar las contraseñas de aplicaciones, es importante recordar:

- Las contraseñas de aplicaciones se generan automáticamente y solo se escriben una vez por cada aplicación.

- Hay un límite de 40 contraseñas por usuario. Si intenta crear una después de ese límite, se le pedirá que elimine una contraseña existente antes de que se le permita crear una nueva.

- Utilice una contraseña de aplicación por dispositivo, no por aplicación. Por ejemplo, cree una única contraseña para todas las aplicaciones del equipo portátil y luego otra contraseña única para todas las aplicaciones del equipo de sobremesa.

    >[!Note]
    >Los clientes de Office 2013 (incluido Outlook) admiten nuevos protocolos de autenticación que se pueden usar con la verificación en dos pasos. Esta compatibilidad significa que después de activar la verificación en dos pasos, ya no necesitará contraseñas de aplicaciones para clientes de Office 2013. Para obtener más información, consulte el artículo [Cómo funciona la autenticación moderna para las aplicaciones de cliente de Office 2013 y Office 2016](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517).

## <a name="where-to-create-and-delete-your-app-passwords"></a>Dónde crear y eliminar las contraseñas de aplicaciones

Recibirá una contraseña de la aplicación durante el registro inicial de verificación en dos pasos. Si necesita más de una contraseña, puede crear contraseñas adicionales, según cómo utilice la verificación en dos pasos:

- **Utiliza la verificación en dos pasos para la cuenta profesional o educativa y el portal Mis aplicaciones.** Cree y elimine las contraseñas de aplicaciones mediante las instrucciones de la sección [Creación y eliminación de contraseñas de aplicaciones en el portal Mis aplicaciones](#create-and-delete-app-passwords-using-the-myapps-portal) de este artículo. Para obtener más información sobre el portal Mis aplicaciones y cómo usarlo, consulte [¿Qué es el portal Mis aplicaciones de Azure Active Directory?](active-directory-saas-access-panel-introduction.md).

- **Utiliza la verificación en dos pasos para la cuenta profesional o educativa y el portal de Office 365.** Cree y elimine las contraseñas de aplicaciones mediante las instrucciones de la sección [Creación y eliminación de contraseñas de aplicaciones en el portal de Office 365](#create-and-delete-app-passwords-using-the-office-365-portal) de este artículo.

- **Utiliza la verificación en dos pasos para la cuenta Microsoft personal.** Cree y elimine las contraseñas de aplicaciones mediante la página [Conceptos básicos sobre seguridad](https://account.microsoft.com/account/) con su cuenta de Microsoft personal. Para más información, consulte el artículo [Contraseñas de aplicaciones y verificación en dos pasos](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification).

## <a name="create-and-delete-app-passwords-using-the-myapps-portal"></a>Creación y eliminación de contraseñas de aplicaciones en el portal Mis aplicaciones
Puede crear y eliminar contraseñas de aplicaciones mediante el portal Mis aplicaciones.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Creación de una contraseña de aplicación mediante el portal de MyApps

1. Inicie sesión en [https://myapps.microsoft.com](https://myapps.microsoft.com).

2. Seleccione su nombre en la parte superior derecha y elija **Perfil**.

3. Seleccione **Comprobación de seguridad adicional**.

   ![Seleccionar comprobación de seguridad adicional: captura de pantalla](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Seleccione **Contraseñas de aplicaciones**.

   ![Seleccionar contraseñas de aplicación: captura de pantalla](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Haga clic en **Create**(Crear).

6. Escriba un nombre para la contraseña de aplicación y seleccione **Siguiente**.

7. Copie la contraseña de aplicación en el Portapapeles y péguela en la aplicación.
   
    ![Crear una contraseña de aplicación](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Eliminación de una contraseña de aplicación mediante el portal de MyApps

1. Haga clic en su perfil y seleccione **Comprobación de seguridad adicional**.

2. Seleccione **Contraseñas de aplicaciones** y después seleccione **Eliminar** junto a la contraseña de la aplicación que desea eliminar.

   ![Eliminar una contraseña de aplicación](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

3. Seleccione **Sí** para confirmar que desea eliminar la contraseña y, a continuación, seleccione **Cerrar**.

## <a name="create-and-delete-app-passwords-using-the-office-365-portal"></a>Creación y eliminación de contraseñas de aplicaciones en el portal de Office 365

Si utiliza la verificación en dos pasos con la cuenta profesional o educativa y sus aplicaciones de Office 365, puede crear y eliminar las contraseñas de aplicaciones mediante el portal de Office 365. Puede tener un máximo de 40 contraseñas de aplicaciones a la vez. Si necesita otra contraseña de aplicación después de ese límite, tendrá que eliminar una de las contraseñas de aplicaciones existentes.

### <a name="to-create-app-passwords-using-the-office-365-portal"></a>Para crear contraseñas de aplicaciones en el portal de Office 365

1. Inicie sesión en su cuenta profesional o educativa.

2. Vaya a https://portal.office.com, seleccione el icono **Configuración** en la parte superior derecha de la página **Portal de Office 365** y, a continuación, expanda **Comprobación de seguridad adicional**.

    ![Portal de Office que muestra la ampliación del área de comprobación de seguridad adicional](media/security-info/security-info-o365password.png)

3. Seleccione el vínculo **Crear y administrar contraseñas de aplicaciones** para abrir la página **Contraseñas de aplicaciones**.

4. Seleccione **Crear**, escriba un nombre descriptivo para la aplicación que necesita la contraseña y, después, seleccione **Siguiente**.

5. Seleccione **Copiar contraseña en el Portapapeles** y luego seleccione **Cerrar**.

6. Utilice la contraseña de la aplicación copiada para iniciar sesión en su aplicación sin explorador. Solo tiene que escribir esta contraseña una vez y será recordada en el futuro.

### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Para eliminar contraseñas de aplicación en el portal de Office 365

1. Inicie sesión en su cuenta profesional o educativa.

2. Vaya a https://portal.office.com, seleccione el icono **Configuración** en la parte superior derecha de la página **Portal de Office 365** y, a continuación, seleccione **Comprobación de seguridad adicional**.

3. Seleccione el vínculo **Crear y administrar contraseñas de aplicaciones** para abrir la página **Contraseñas de aplicaciones**.

4. Seleccione **Eliminar** para la contraseña de la aplicación que desea eliminar, seleccione **Sí** en el cuadro de confirmación y, después, seleccione **Cerrar**.

    La contraseña de aplicación se eliminó correctamente.

5. Siga los pasos de crear una contraseña de aplicación para crear una nueva.

## <a name="if-your-app-passwords-arent-working-properly"></a>Si las contraseñas de aplicaciones no funcionan correctamente

Asegúrese de escribir correctamente la contraseña. Si está seguro de que ha introducido tu contraseña correctamente, vuelva a iniciar sesión y cree una nueva contraseña de la aplicación. Si ninguna de estas opciones soluciona el problema, póngase en contacto con el soporte técnico de la empresa para que pueda eliminar las contraseñas de aplicaciones existentes, permitiéndole crear otras nuevas. 

## <a name="next-steps"></a>Pasos siguientes

- [Administrar la configuración de la verificación en dos pasos](multi-factor-authentication-end-user-manage-settings.md)

- Pruebe la [aplicación Microsoft Authenticator](microsoft-authenticator-app-how-to.md) para verificar los inicios de sesión con las notificaciones de aplicación, en lugar de recibir llamadas o textos.
