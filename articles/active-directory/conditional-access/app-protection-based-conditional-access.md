---
title: 'Directivas de protección de aplicaciones con acceso condicional: Azure Active Directory'
description: Aprenda a establecer el uso obligatorio de directivas de protección de aplicaciones para el acceso a aplicaciones en la nube con acceso condicional en Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c853ef3a5a40381aba4e1c13eaf9ad7d8653170
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77186632"
---
# <a name="require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>Uso obligatorio de directivas de protección de aplicaciones para el acceso a aplicaciones en la nube con acceso condicional en Azure Active Directory (versión preliminar)

Los empleados usan dispositivos móviles para tareas personales y profesionales. Mientras se asegura de que los empleados pueden ser productivos, también puede evitar la pérdida de datos. Con el acceso condicional de Azure Active Directory (Azure AD), puede restringir el acceso a las aplicaciones en la nube para proteger los datos corporativos. Use primero las aplicaciones cliente con una directiva de protección de aplicaciones.

En este artículo se explica cómo configurar directivas de acceso condicional que pueden requerir una directiva de protección de aplicaciones para que se concede acceso a los datos.

## <a name="overview"></a>Información general

Con el [acceso condicional de Azure AD](overview.md), puede ajustar el modo en que los usuarios autorizados pueden acceder a sus recursos. Por ejemplo, puede limitar el acceso de los dispositivos de confianza a las aplicaciones en la nube.

Puede usar [directivas de protección de aplicaciones de Intune](https://docs.microsoft.com/intune/app-protection-policy) para ayudar a proteger los datos de su compañía. Las directivas de protección de aplicaciones de Intune no requieren una solución de administración de dispositivos móviles (MDM). Puede proteger los datos de la empresa con o sin inscripción de dispositivos en una solución de administración de dispositivos.

El acceso condicional de Azure Active Directory restringe el acceso a las aplicaciones en la nube para las aplicaciones cliente que Intune ha notificado a Azure AD como receptoras de una directiva de protección de aplicaciones. Por ejemplo, puede restringir el acceso de Exchange Online a la aplicación de Outlook que tenga una directiva de protección de aplicaciones de Intune.

En la terminología del acceso condicional, estas aplicaciones cliente se conocen como directivas protegidas con una *directiva de protección de aplicaciones*.  

![Acceso condicional](./media/app-protection-based-conditional-access/05.png)

Para obtener una lista de aplicaciones cliente protegidas por directivas, consulte [Requisito de directiva de protección de aplicaciones](concept-conditional-access-grant.md).

Puede combinar directivas de acceso condicional basado en la protección de aplicaciones con otras directivas, como [directivas de acceso condicional basado en dispositivos](require-managed-devices.md). De este modo, puede proporcionar flexibilidad acerca de cómo proteger los datos de dispositivos personales y corporativos.

> [!NOTE]
> Las directivas de protección de aplicaciones de acceso condicional no se pueden aplicar a usuarios de B2B porque la organización que invita no tiene visibilidad en la organización principal del usuario de B2B.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Ventajas del requisito de acceso condicional basado en la protección de aplicaciones

Similar al cumplimiento notificado por Intune para iOS y Android para un dispositivo administrado, ahora Intune informa a Azure AD si se aplica una directiva de protección de aplicaciones. El acceso condicional puede usar esta directiva como una comprobación de acceso. Esta nueva directiva de acceso condicional, la directiva de protección de aplicaciones, aumenta la seguridad. Protege frente a errores de administración como los siguientes:

- Usuarios que no tienen una licencia de Intune.
- Usuarios que no reciben una directiva de protección de aplicaciones de Intune.
- Las aplicaciones de la directiva de protección de aplicaciones de Intune no están configuradas para recibir una directiva.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se da por hecho que está familiarizado con:

- El [requisito de la directiva de protección de aplicaciones](concept-conditional-access-grant.md).
- El [requisito de aplicación cliente aprobada](concept-conditional-access-grant.md).
- Los conceptos básicos del [acceso condicional en Azure Active Directory](overview.md).
- La [configuración de la directiva de acceso condicional](app-based-mfa.md).

## <a name="prerequisites"></a>Prerrequisitos

Para crear una directiva de acceso condicional basado en la protección de aplicaciones, debe:

- Tener una suscripción a Azure Active Directory Premium o Enterprise Mobility + Security + Intune.
- Asegurarse de que los usuarios tienen licencia para Enterprise Mobility + Security o Azure AD + Intune.
- Asegurarse de que la aplicación cliente está configurada en Intune para recibir una directiva de protección de aplicaciones.
- Asegurarse de que los usuarios están configurados en Intune para recibir una directiva de protección de aplicaciones de Intune.

## <a name="app-protection-based-policy-for-exchange-online"></a>Directiva basada en la protección de aplicaciones para Exchange Online

Este escenario consiste en una directiva de acceso condicional basado en la protección de aplicaciones para acceder a Exchange Online.

### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:

- Configura el correo electrónico mediante una aplicación de correo electrónico nativa de iOS o Android para conectarse a Exchange.
- Recibe un correo electrónico que indica que el acceso solo está disponible mediante la aplicación Outlook.
- Descarga la aplicación con el vínculo.
- Abre la aplicación Outlook e inicia sesión con las credenciales de Azure AD.
- Se le solicita que instale la **aplicación Microsoft Authenticator** o el **Portal de empresa Intune** para continuar.
- Instala la aplicación y vuelve a la aplicación Outlook para continuar.
- Se le solicita que registre un dispositivo.
- Puede recibir una directiva de protección de aplicaciones de Intune.
- Puede tener acceso al correo electrónico.

Todas las directivas de protección de aplicaciones de Intune deben estar en la aplicación para tener acceso a los datos corporativos. Las directivas podrían solicitar al usuario que reinicie la aplicación o que use un PIN adicional. Esto sucede si las directivas se configuran para la aplicación y la plataforma.

### <a name="configuration"></a>Configuración

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional descrita en este paso, configure los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/01.png)

1. Escriba el nombre de la directiva de acceso condicional.
1. En **Asignaciones**, en **Usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.
1. En **Aplicaciones en la nube**, seleccione **Office 365 Exchange Online**.

   ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

1. En **Condiciones**, configure **Plataformas de dispositivo** y **Aplicaciones cliente (versión preliminar)** :
   1. En **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

   1. En **Aplicaciones cliente (versión preliminar)** , seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de autenticación moderna**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

1. En **Controles de acceso**, seleccione **Requerir directiva de protección de aplicaciones (versión preliminar)** .

   ![Acceso condicional](./media/app-protection-based-conditional-access/05.png)

**Paso 2: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, configure los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. Escriba el nombre de la directiva de acceso condicional.
1. En **Asignaciones**, en **Usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.
1. En **Aplicaciones en la nube**, seleccione **Office 365 Exchange Online**.

   ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

1. En **Condiciones**, configure **Aplicaciones cliente (versión preliminar)** . 

   1. En **Aplicaciones cliente (versión preliminar)** , seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de Exchange ActiveSync**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

   1. En **Controles de acceso**, seleccione **Requerir directiva de protección de aplicaciones (versión preliminar)** .

      ![Acceso condicional](./media/app-protection-based-conditional-access/05.png)

**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**

![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Para más información, consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>Directiva de dispositivos compatibles o basada en la protección de aplicaciones para Exchange Online

Este escenario consiste en una directiva de acceso condicional de dispositivos compatibles o basada en la protección de aplicaciones para tener acceso a Exchange Online.

### <a name="scenario-playbook"></a>Guía de escenario

En este escenario se supone que:
 
- Un usuario ya está inscrito (con o sin dispositivos corporativos).
- Los usuarios que no están inscritos ni registrados con Azure AD mediante una aplicación protegida deben registrar un dispositivo para tener acceso a los recursos.
- Los usuarios inscritos mediante la aplicación protegida no tienen que volver a registrar el dispositivo.
- El usuario puede recibir una directiva de protección de aplicaciones de Intune si no se ha inscrito.
- El usuario puede tener acceso al correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune si no se ha inscrito.
- El usuario puede tener acceso al correo electrónico con Outlook si el dispositivo está inscrito.

### <a name="configuration"></a>Configuración

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional descrita en este paso, configure los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/62.png)

1. Escriba el nombre de la directiva de acceso condicional.
1. En **Asignaciones**, en **Usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.
1. En **Aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

   ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

1. En **Condiciones**, configure **Plataformas de dispositivo** y **Aplicaciones cliente (versión preliminar)** . 
 
   1. En **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

   1. En **Aplicaciones cliente (versión preliminar)** , seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de autenticación moderna**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. En **Controles de acceso**, seleccione las opciones siguientes:
   - **Requerir que el dispositivo esté marcado como compatible**
   - **Requerir directiva de protección de aplicaciones (versión preliminar)**
   - **Requerir uno de los controles seleccionados**   
 
      ![Acceso condicional](./media/app-protection-based-conditional-access/11.png)

**Paso 2: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online con ActiveSync**

Para la directiva de acceso condicional descrita en este paso, configure los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. Escriba el nombre de la directiva de acceso condicional.
1. En **Asignaciones**, en **Usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.
1. En **Aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

   ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

1. En **Condiciones**, configure **Aplicaciones cliente (versión preliminar)** . 

   En **Aplicaciones cliente (versión preliminar)** , seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de Exchange ActiveSync**.

   ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

1. En **Controles de acceso**, seleccione las opciones siguientes:
   - **Requerir que el dispositivo esté marcado como compatible**
   - **Requerir directiva de protección de aplicaciones (versión preliminar)**
   - **Requerir uno de los controles seleccionados**

      ![Acceso condicional](./media/app-protection-based-conditional-access/11.png)

**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**

![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Para más información, consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>Directiva de dispositivos compatibles y basada en la protección de aplicaciones para Exchange Online

Este escenario consiste en una directiva de acceso condicional de dispositivos compatibles y basada en la protección de aplicaciones para tener acceso a Exchange Online.

### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:
 
- Configura el correo electrónico mediante una aplicación de correo electrónico nativa de iOS o Android para conectarse a Exchange.
- Recibe un correo electrónico que indica que es necesario que el dispositivo esté inscrito para poder tener acceso.
- Descarga el Portal de empresa de Intune e inicia sesión en este.
- Comprueba el correo electrónico y se le solicita que use la aplicación Outlook.
- Descarga la aplicación Outlook.
- Abre Outlook y escribe las credenciales usadas en la inscripción.
- Puede recibir una directiva de protección de aplicaciones de Intune.
- Puede tener acceso al correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune.

Todas las directivas de protección de aplicaciones de Intune se activan antes de que se conceda acceso a los datos corporativos. Las directivas podrían solicitar al usuario que reinicie la aplicación o que use un PIN adicional. Esto sucede si las directivas se configuran para la aplicación y la plataforma.

### <a name="configuration"></a>Configuración

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional descrita en este paso, configure los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/01.png)

1. Escriba el nombre de la directiva de acceso condicional.
1. En **Asignaciones**, en **Usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.
1. En **Aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

   ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

1. En **Condiciones**, configure **Plataformas de dispositivo** y **Aplicaciones cliente (versión preliminar)** . 
   1. En **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

   1. En **Aplicaciones cliente (versión preliminar)** , seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de autenticación moderna**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

1. En **Controles de acceso**, seleccione las opciones siguientes:
   - **Requerir que el dispositivo esté marcado como compatible**
   - **Requerir directiva de protección de aplicaciones (versión preliminar)**
   - **Requerir todos los controles seleccionados**   
 
      ![Acceso condicional](./media/app-protection-based-conditional-access/13.png)

**Paso 2: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online con ActiveSync**

Para la directiva de acceso condicional descrita en este paso, configure los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. Escriba el nombre de la directiva de acceso condicional.
1. En **Asignaciones**, en **Usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.
1. En **Aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

   ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

1. En **Condiciones**, configure **Aplicaciones cliente (versión preliminar)** . 

   En **Aplicaciones cliente (versión preliminar)** , seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de Exchange ActiveSync**.

   ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

1. En **Controles de acceso**, seleccione las opciones siguientes:
   - **Requerir que el dispositivo esté marcado como compatible**
   - **Requerir directiva de protección de aplicaciones (versión preliminar)**
   - **Requerir todos los controles seleccionados**   
 
      ![Acceso condicional](./media/app-protection-based-conditional-access/13.png)

**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**

![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Para más información, consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>Directiva basada en aplicaciones o en la protección aplicaciones para Exchange Online y SharePoint Online

Este escenario consiste en una directiva de aplicaciones aprobadas o basada en la protección de aplicaciones para tener acceso a Exchange Online y SharePoint Online.

### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:

- Configura aplicaciones cliente que están en la lista de aplicaciones que admiten el requisito de directiva de protección de aplicaciones o el requisito de aplicaciones aprobadas.  
- Usa aplicaciones cliente que cumplen el requisito de directiva de protección de aplicaciones y pueden recibir una directiva de protección de aplicaciones de Intune.
- Usa aplicaciones cliente que cumplen el requisito de directiva de aplicaciones aprobadas que admite la directiva de protección de aplicaciones de Intune.
- Abre la aplicación para tener acceso al correo electrónico o a documentos.
- Abre la aplicación Outlook e inicia sesión con las credenciales de Azure AD.
- Se le solicita que instale Microsoft Authenticator para su uso con iOS o el Portal de empresa de Intune para Android si aún no se han instalado.
- Instala la aplicación y puede volver a la aplicación Outlook para continuar.
- Se le solicita que registre un dispositivo.
- Puede recibir una directiva de protección de aplicaciones de Intune.
- Puede tener acceso al correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune.
- Puede tener acceso a sitios y documentos con una aplicación que no se incluye en el requisito de directiva de protección de aplicaciones, pero aparece en el requisito de aplicaciones aprobadas.

Todas las directivas de protección de aplicaciones de Intune son necesarias para que se conceda acceso a los datos corporativos. Las directivas podrían solicitar al usuario que reinicie la aplicación o que use un PIN adicional. Esto sucede si las directivas se configuran para la aplicación y la plataforma.

**Comentarios:**

- Puede usar este escenario si quiere admitir ambas directivas de acceso condicional basadas en aplicaciones y en la protección de aplicaciones.
- En esta directiva *OR*, las aplicaciones con un requisito de directiva de protección de aplicaciones se evalúan para obtener acceso antes del requisito de aplicaciones clientes aprobadas.

### <a name="configuration"></a>Configuración

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online y SharePoint Online**

Para la directiva de acceso condicional descrita en este paso, configure los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/62.png)

1. Escriba el nombre de la directiva de acceso condicional.
1. En **Asignaciones**, en **Usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.
1. En **Aplicaciones en la nube**, seleccione **Office 365 Exchange Online** y **Office 365 SharePoint Online**. 

   ![Acceso condicional](./media/app-protection-based-conditional-access/02.png)

1. En **Condiciones**, configure **Plataformas de dispositivo** y **Aplicaciones cliente (versión preliminar)** . 
   1. En **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

   1. En **Aplicaciones cliente (versión preliminar)** , seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de autenticación moderna**.

      ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

1. En **Controles de acceso**, seleccione las opciones siguientes:
   - **Requerir aplicación cliente aprobada**
   - **Requerir directiva de protección de aplicaciones (versión preliminar)**
   - **Requerir uno de los controles seleccionados**
 
      ![Acceso condicional](./media/app-protection-based-conditional-access/12.png)

**Paso 2: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**

![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Para más información, consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="next-steps"></a>Pasos siguientes

- Si quiere saber cómo configurar una directiva de acceso condicional, vea [Requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md).
- Si está listo para configurar directivas de acceso condicional para su entorno, consulte [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md).
