---
title: 'Tutorial:  Configuración de PHS como copia de seguridad de AD FS en Azure AD Connect | Microsoft Docs'
description: Muestra cómo activar la sincronización de hash de contraseña como copia de seguridad y para AD FS.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 01/30/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2281bc451a5acf9e4e634a124161a3e8b0734deb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58090515"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Tutorial:  Configuración de PHS como copia de seguridad de AD FS en Azure AD Connect

El siguiente tutorial le guiará a través de la configuración de la sincronización de hash de contraseña como copia de seguridad y la conmutación por error para AD FS.  En este documento también se demostrará cómo habilitar la sincronización de hash de contraseña como método de autenticación principal, en caso de que se produzca un error con AD FS o deje de estar disponible.

## <a name="prerequisites"></a>Requisitos previos
Este tutorial se basa en el [Tutorial: Federación de un único entorno de bosques de AD en la nube](tutorial-federation.md) y es un requisito previo para intentar este tutorial.  Si no ha completado este tutorial, hágalo antes de intentar los pasos descritos en este documento.

## <a name="enable-phs-in-azure-ad-connect"></a>Habilitación de PHS en Azure AD Connect
Ahora que tenemos un entorno de Azure AD Connect que utiliza la federación, el primer paso es activar la sincronización de hash de contraseña y permitir que Azure AD Connect sincronice los hash.

Haga lo siguiente:

1.  Haga doble clic en el icono de Azure AD Connect que se creó en el escritorio.
2.  Haga clic en **Configurar**.
3.  En la página Tareas adicionales, seleccione **Personalizar las opciones de sincronización** y haga clic en **Siguiente**.
4.  Escriba el nombre de usuario y la contraseña del administrador global.  Creó esta cuenta [aquí](tutorial-federation.md#create-a-global-administrator-in-azure-ad) en el tutorial anterior.
5.  En la pantalla **Conectar sus directorios**, haga clic en **Siguiente**.
6.  En la pantalla **Filtrado de dominios y unidades organizativas**, haga clic en **Siguiente**.
7.  En la pantalla **Características opcionales**, seleccione **Sincronización de contraseña** y haga clic en **Siguiente**.
![Select](media/tutorial-phs-backup/backup1.png)</br>
8.  En la pantalla **Listo para configurar**, haga clic en **Configurar**.
9.  Cuando haya completado la configuración, haga clic en **Salir**.
10. Eso es todo.  Listo.  Ahora se producirá la sincronización de hash de contraseña, que se podrá usar como copia de seguridad si AD FS no está disponible.

## <a name="switch-to-password-hash-synchronization"></a>Cambio a la sincronización de hash de contraseña
Ahora le mostraremos cómo cambiar a la sincronización de hash de contraseña. Antes de comenzar, considere en qué condiciones debe realizar el cambio. No realice el cambio por motivos temporales, como una interrupción de la red, un problema de AD FS menor u otro que afecte a un subconjunto de usuarios. Si decide realizar el cambio porque tardaría mucho en solucionar el problema, realice lo siguiente:

1. Haga doble clic en el icono de Azure AD Connect que se creó en el escritorio.
2.  Haga clic en **Configurar**.
3.  Seleccione **Cambiar inicio de sesión de usuario** y haga clic en **Siguiente**.
![Cambio](media/tutorial-phs-backup/backup2.png)</br>
4.  Escriba el nombre de usuario y la contraseña del administrador global.  Creó esta cuenta [aquí](tutorial-federation.md#create-a-global-administrator-in-azure-ad) en el tutorial anterior.
5.  En la pantalla **Inicio de sesión de usuario**, seleccione **Sincronización de hash de contraseña** y coloque una marca de verificación en el cuadro **No convertir las cuentas de usuario**.  
6.  Deje seleccionado el valor predeterminado **Habilitar el inicio de sesión único** y haga clic en **Siguiente**.
7.  En la pantalla **Habilitar el inicio de sesión único**, haga clic en **Siguiente**.
8.  En la pantalla **Listo para configurar**, haga clic en **Configurar**.
9.  Cuando haya completado la configuración, haga clic en **salir**.
10. Los usuarios ya pueden usar sus contraseñas para iniciar sesión en Azure y servicios de Azure.

## <a name="test-signing-in-with-one-of-our-users"></a>Prueba del inicio de sesión con uno de nuestros usuarios

1. Vaya a [https://myapps.microsoft.com](https://myapps.microsoft.com).
2. Inicie sesión con la cuenta de usuario que se creó en nuestro nuevo inquilino.  Deberá iniciar sesión mediante el formato siguiente: (user@domain.onmicrosoft.com). Use la misma contraseña que el usuario utiliza para iniciar sesión en el entorno local.</br>
   ![Verify](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="next-steps"></a>Pasos siguientes


- [Hardware y requisitos previos](how-to-connect-install-prerequisites.md) 
- [Configuración rápida](how-to-connect-install-express.md)
- [Sincronización de hash de contraseñas](how-to-connect-password-hash-synchronization.md)|
