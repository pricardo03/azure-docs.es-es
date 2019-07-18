---
title: Habilitación de la escritura diferida de contraseñas en Azure AD
description: En este tutorial, habilitará la escritura diferida de contraseñas para que los cambios de contraseña iniciados en la nube vuelvan al entorno local de Azure AD como parte de Azure AD Connect.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1efb67df6c31a3b03fdc45fffc0564fb09e39faf
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67853035"
---
# <a name="tutorial-enabling-password-writeback"></a>Tutorial: Habilitación de la escritura diferida de contraseñas

En este tutorial, habilitará la escritura diferida de contraseñas en un entorno híbrido. La escritura diferida de contraseñas se utiliza para sincronizar los cambios de contraseñas en Azure Active Directory (Azure AD) de vuelta al entorno local de Active Directory Domain Services (AD DS). La escritura diferida de contraseñas se habilita como parte de Azure AD Connect a fin de proporcionar un mecanismo seguro para enviar los cambios de contraseñas de vuelta a un directorio local existente desde Azure AD. Puede encontrar más detalles sobre el funcionamiento interno de la escritura diferida de contraseñas en el artículo [¿Qué es la escritura diferida de contraseñas](concept-sspr-writeback.md).

> [!div class="checklist"]
> * Habilitación de la opción de escritura diferida de contraseñas en Azure AD Connect
> * Habilitación de la opción de la escritura diferida de contraseñas en el autoservicio de restablecimiento de contraseñas (SSPR)

## <a name="prerequisites"></a>Requisitos previos

* Acceso a un inquilino de Azure AD activo con al menos una licencia de prueba asignada.
* Una cuenta con privilegios de administrador global en su inquilino de Azure AD.
* Un servidor existente configurado que ejecute una versión actual de [Azure AD Connect](../hybrid/how-to-connect-install-express.md).
* Se han completado los tutoriales anteriores para el autoservicio de restablecimiento de contraseñas (SSPR).

## <a name="enable-password-writeback-option-in-azure-ad-connect"></a>Habilitación de la opción de escritura diferida de contraseñas en Azure AD Connect

Para habilitar la escritura diferida de contraseñas, primero deberá habilitar la característica desde el servidor en el que ha instalado Azure AD Connect.

1. Para configurar y habilitar la escritura diferida de contraseñas, inicie sesión en su servidor de Azure AD Connect e inicie el asistente de configuración de **Azure AD Connect**.
2. En la página **principal**, seleccione **Configurar**.
3. En la página **Tareas adicionales**, seleccione **Personalizar las opciones de sincronización** y haga clic en **Siguiente**.
4. En la página **Conectar con Azure AD**, escriba una credencial de administrador global y elija **Siguiente**.
5. En las páginas de filtrado **Conectar directorios** y **Dominio/Unidad organizativa**, seleccione **Siguiente**.
6. En la página **Características opcionales**, active la casilla junto a **Escritura diferida de contraseñas** y haga clic en **Siguiente**.
7. En la página **Listo para configurar**, haga clic en **Configurar** y espere a que se complete el proceso.
8. Cuando finalice la configuración, seleccione **Salir**.

## <a name="enable-password-writeback-option-in-sspr"></a>Habilitación de la opción de escritura diferida de contraseñas en SSPR

Habilitar la característica de escritura diferida de contraseñas en Azure AD Connect constituye solo la mitad del proceso. Permitir a SSPR usar la escritura diferida de contraseñas completa el bucle, permitiendo por tanto a los usuarios cambiar o restablezcan su contraseña para hacer que esa contraseña se establezca también en el entorno local.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador global.
2. Vaya a **Azure Active Directory**, haga clic en **Restablecer contraseña** y, a continuación, elija **Integración local**.
3. Establezca la opción **Write back passwords to your on-premises directory** (Escritura diferida de contraseñas en el directorio local) como **Sí**.
4. Establezca la opción **Permitir a los usuarios desbloquear las cuentas sin restablecer la contraseña** en **Sí**.
5. Haga clic en **Guardar**

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha habilitado la escritura diferida de contraseñas para el autoservicio de restablecimiento de contraseñas. Deje abierta la ventana de Azure Portal y siga con el siguiente tutorial para configurar opciones adicionales relacionadas con el autoservicio de restablecimiento de contraseñas antes de implementar la solución en un entorno piloto.

> [!div class="nextstepaction"]
> [Evaluación del riesgo en el inicio de sesión](tutorial-risk-based-sspr-mfa.md)
