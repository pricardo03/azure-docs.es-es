---
title: Requerir la directiva de protección de aplicaciones para el acceso a la aplicación en la nube con acceso condicional en Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo requerir la directiva de protección de aplicaciones para el acceso a la aplicación en la nube con acceso condicional en Azure Active Directory.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 4/4/2019
ms.author: joflore
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2250449c0ef342332945b80cb10cb9a02885b259
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59496937"
---
# <a name="require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>Requerir la directiva de protección de aplicaciones para el acceso a la aplicación en la nube con acceso condicional (versión preliminar)

Los empleados usan dispositivos móviles para tareas personales y profesionales. Mientras se asegura de que los empleados pueden ser productivos, también puede evitar la pérdida de datos. Con el acceso condicional de Azure Active Directory (Azure AD), puede proteger los datos corporativos restringiendo el acceso a las aplicaciones en la nube. Usar primero las aplicaciones cliente con una directiva de protección de aplicaciones.

En este artículo se explica cómo configurar directivas de acceso condicional que pueden requerir una directiva de protección de aplicaciones antes de que se concede acceso a datos.

## <a name="overview"></a>Información general

Con el [acceso condicional de Azure AD](overview.md), puede ajustar el modo en que los usuarios autorizados pueden tener acceso a sus recursos. Por ejemplo, puede limitar el acceso de los dispositivos de confianza a las aplicaciones en la nube.

Puede usar [directivas de protección de aplicaciones de Intune](https://docs.microsoft.com/intune/app-protection-policy) para ayudar a proteger los datos de su compañía. Directivas de protección de aplicaciones de Intune no requieren una solución de administración (MDM) de dispositivos móviles. Puede proteger los datos de su empresa con o sin inscripción de dispositivos en una solución de administración de dispositivos.

Acceso condicional de Azure Active Directory restringe el acceso a las aplicaciones en la nube para las aplicaciones cliente que Intune ha notificado a Azure AD como la recepción de una directiva de protección de aplicaciones. Por ejemplo, puede restringir el acceso a Exchange Online a la aplicación de Outlook que tiene una directiva de protección de aplicaciones de Intune.

En la terminología del acceso condicional, estas aplicaciones cliente se conocen como directiva protegido con un *directiva de protección de aplicaciones*.  

![Acceso condicional](./media/app-protection-based-conditional-access/05.png)

Para obtener una lista de aplicaciones protegidas por directiva de cliente, consulte [requisito de directiva de protección de aplicación](technical-reference.md#approved-client-app-requirement).

Puede combinar directivas de acceso condicional basado en la protección de aplicación con otras directivas, como [directivas de acceso condicional basado en dispositivos](require-managed-devices.md). De este modo, puede proporcionar flexibilidad en cómo proteger los datos de dispositivos personales y corporativos.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Ventajas de requisito de acceso condicional basado en la protección de la aplicación

Similar a cumplimiento notificado por Intune para iOS y Android para un dispositivo administrado, ahora se aplica a los informes a Azure AD si una directiva de protección de aplicaciones de Intune. Acceso condicional puede usar esta directiva como una comprobación de acceso. Esta nueva directiva de acceso condicional, la directiva de protección de aplicaciones, aumenta la seguridad. Protege frente a errores de administración, como:

- Usuarios que no tienen una licencia de Intune.
- Usuarios que no reciben una directiva de protección de aplicaciones de Intune.
- Directiva de Intune app protection aplicaciones que no está configurado para recibir una directiva.


## <a name="before-you-begin"></a>Antes de empezar

En este artículo se da por hecho que está familiarizado con:

- El [requisito de directiva de protección de aplicación](technical-reference.md#app-protection-policy-requirement) referencia técnica.
- La referencia técnica del [requisito de las aplicaciones cliente aprobadas](technical-reference.md#approved-client-app-requirement).
- Los conceptos básicos del [Acceso condicional en Azure Active Directory](overview.md).
- La [configuración de la directiva de acceso condicional](app-based-mfa.md).


## <a name="prerequisites"></a>Requisitos previos

Para crear una directiva de acceso condicional basado en la protección de aplicaciones, debe:

- Tener un Enterprise Mobility + Security o una suscripción de Azure Active Directory premium + Intune.
- Asegúrese de que los usuarios tienen licencia para Enterprise Mobility + Security o Azure AD + Intune.
- Asegúrese de que la aplicación cliente está configurada en Intune para recibir una directiva de protección de aplicaciones.
- Asegúrese de que los usuarios se configuran en Intune para recibir una directiva de protección de aplicaciones de Intune.

## <a name="app-protection-based-policy-for-exchange-online"></a>Directiva de aplicación en función de protección para Exchange Online

Este escenario consta de una directiva de acceso condicional basado en la protección de aplicaciones para tener acceso a Exchange Online.

### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:

- Configura el correo electrónico mediante el uso de una aplicación de correo nativo en iOS o Android para conectarse a Exchange.
- Recibe un correo electrónico que indica que el acceso está disponible solo mediante el uso de la aplicación Outlook.
- Descarga la aplicación con el vínculo.
- Abre la aplicación Outlook e inicia sesión con credenciales de Azure AD.
- Se solicita al instalar Microsoft Authenticator para su uso de iOS o Portal de empresa de Intune para Android usan continuar.
- Instala la aplicación y vuelve a la aplicación Outlook para continuar.
- Se le pedirá que registre un dispositivo.
- Puede recibir una directiva de protección de aplicaciones de Intune.
- Puede acceder al correo electrónico.

Todas las directivas de Intune app protection deben estar en la aplicación para tener acceso a los datos corporativos. Las directivas podrían solicitar al usuario que reinicie la aplicación o usar un PIN adicional. Esto sucede si se configuran las directivas de la aplicación y plataforma.

### <a name="configuration"></a>Configuración

**Paso 1: Configurar una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional en este paso, configure los siguientes componentes:

![Acceso condicional](./media/app-protection-based-conditional-access/01.png)

1. Escriba el nombre de la directiva de acceso condicional.

2. En **asignaciones**, en **usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.

3. En **aplicaciones en la nube**, seleccione **Office 365 Exchange Online**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. En **condiciones**, configurar **plataformas de dispositivos** y **aplicaciones cliente (versión preliminar)**:

     a. En **plataformas de dispositivos**, seleccione **Android** y **iOS**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

    b. En **aplicaciones cliente (versión preliminar)**, seleccione **aplicaciones móviles y clientes de escritorio** y **clientes de autenticación moderna**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. En **controles de acceso**, seleccione **requieren la directiva de protección de aplicaciones (versión preliminar)**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/05.png)
 

**Paso 2: Configurar una directiva de acceso condicional de Azure AD para Exchange Online con ActiveSync (EAS)**

Para la directiva de acceso condicional en este paso, configure los siguientes componentes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. Escriba el nombre de la directiva de acceso condicional.

2. En **asignaciones**, en **usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.


3. En **aplicaciones en la nube**, seleccione **Office 365 Exchange Online**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. En **condiciones**, configurar **aplicaciones cliente (versión preliminar)**. 

     a. En **aplicaciones cliente (versión preliminar)**, seleccione **aplicaciones móviles y clientes de escritorio** y **clientes de Exchange ActiveSync**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

    b. En **controles de acceso**, seleccione **requieren la directiva de protección de aplicaciones (versión preliminar)**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/05.png)


**Paso 3: Configurar la directiva de protección de aplicaciones de Intune para iOS y las aplicaciones cliente de Android**


![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Para obtener más información, consulte [proteger aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).



## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>Directiva de dispositivo compatibles o basado en la protección de aplicaciones para Exchange Online

Este escenario consta de una directiva de acceso condicional de dispositivos compatibles o basado en la protección de aplicaciones para tener acceso a Exchange Online.


### <a name="scenario-playbook"></a>Guía de escenario

En este escenario se supone que:
 
- Un usuario ya está inscrito, con o sin dispositivos corporativos.
- Los usuarios que no están inscritos ni registrados con Azure AD mediante el uso de una aplicación protegida deben registrar un dispositivo para tener acceso a los recursos.
- Los usuarios inscritos que usan la aplicación protegida no tienen que volver a registrar el dispositivo.
- El usuario puede recibir una directiva de protección de aplicaciones de Intune si no está inscrito.
- El usuario puede acceder al correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune si no está inscrito.
- El usuario puede tener acceso a correo electrónico con Outlook, si el dispositivo está inscrito.


### <a name="configuration"></a>Configuración

**Paso 1: Configurar una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional en este paso, configure los siguientes componentes:

![Acceso condicional](./media/app-protection-based-conditional-access/62.png)

1. Escriba el nombre de la directiva de acceso condicional.

2. En **asignaciones**, en **usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.

3. En **aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

     ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. En **condiciones**, configurar **plataformas de dispositivos** y **aplicaciones cliente (versión preliminar)**. 
 
     a. En **plataformas de dispositivos**, seleccione **Android** y **iOS**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

    b. En **aplicaciones cliente (versión preliminar)**, seleccione **aplicaciones móviles y clientes de escritorio** y **clientes de autenticación moderna**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. En **controles de acceso**, seleccione las opciones siguientes:

   - **Requerir que el dispositivo esté marcado como compatible**

   - **Requerir la directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir uno de los controles seleccionados**   
 
     ![Acceso condicional](./media/app-protection-based-conditional-access/11.png)



**Paso 2: Configurar una directiva de acceso condicional de Azure AD para Exchange Online con ActiveSync**

Para la directiva de acceso condicional en este paso, configure los siguientes componentes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. Escriba el nombre de la directiva de acceso condicional.

2. En **asignaciones**, en **usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.

3. En **aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

    ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. En **condiciones**, configurar **aplicaciones cliente (versión preliminar)**. 

    En **aplicaciones cliente (versión preliminar)**, seleccione **aplicaciones móviles y clientes de escritorio** y **clientes de Exchange ActiveSync**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

5. En **controles de acceso**, seleccione las opciones siguientes:

   - **Requerir que el dispositivo esté marcado como compatible**

   - **Requerir la directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir uno de los controles seleccionados**

     ![Acceso condicional](./media/app-protection-based-conditional-access/11.png)



**Paso 3: Configurar la directiva de protección de aplicaciones de Intune para iOS y las aplicaciones cliente de Android**


![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Para obtener más información, consulte [proteger aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).





## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>Directiva de dispositivo compatibles y basada en la protección de aplicaciones para Exchange Online

Este escenario consta de una directiva de acceso condicional de dispositivos compatibles y basado en la protección de aplicación para acceder a Exchange Online.


### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:
 
-   Configura el correo electrónico mediante el uso de una aplicación de correo nativo en iOS o Android para conectarse a Exchange.
-   Recibe un correo electrónico que indica que el acceso requiere su dispositivo esté inscrito.
-   Descargas de Portal de empresa de Intune e inicia sesión en el portal.
-   Comprueba el correo electrónico y se le pide que use la aplicación Outlook.
-   Descarga la aplicación Outlook.
-   Se abre la aplicación Outlook y escribe las credenciales usadas en la inscripción.
-   Puede recibir una directiva de protección de aplicaciones de Intune.
-   Puede tener acceso a correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune.

Todas las directivas de Intune app protection se activan antes de que se concede acceso a los datos corporativos. Las directivas podrían solicitar al usuario que reinicie la aplicación o usar un PIN adicional. Esto sucede si se configuran las directivas de la aplicación y plataforma.


### <a name="configuration"></a>Configuración

**Paso 1: Configurar una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional en este paso, configure los siguientes componentes:

![Acceso condicional](./media/app-protection-based-conditional-access/01.png)

1. Escriba el nombre de la directiva de acceso condicional.

2. En **asignaciones**, en **usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.

3. En **aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

     ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. En **condiciones**, configurar **plataformas de dispositivos** y **aplicaciones cliente (versión preliminar)**. 
 
     a. En **plataformas de dispositivos**, seleccione **Android** y **iOS**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

    b. En **aplicaciones cliente (versión preliminar)**, seleccione **aplicaciones móviles y clientes de escritorio** y **clientes de autenticación moderna**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. En **controles de acceso**, seleccione las opciones siguientes:

   - **Requerir que el dispositivo esté marcado como compatible**

   - **Requerir la directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir todos los controles seleccionados**   
 
     ![Acceso condicional](./media/app-protection-based-conditional-access/13.png)



**Paso 2: Configurar una directiva de acceso condicional de Azure AD para Exchange Online con ActiveSync**

Para la directiva de acceso condicional en este paso, configure los siguientes componentes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. Escriba el nombre de la directiva de acceso condicional.

2. En **asignaciones**, en **usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.

3. En **aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

    ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. En **condiciones**, configurar **aplicaciones cliente (versión preliminar)**. 

    En **aplicaciones cliente (versión preliminar)**, seleccione **aplicaciones móviles y clientes de escritorio** y **clientes de Exchange ActiveSync**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

5. En **controles de acceso**, seleccione las opciones siguientes:

   - **Requerir que el dispositivo esté marcado como compatible**

   - **Requerir la directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir todos los controles seleccionados**   
 
     ![Acceso condicional](./media/app-protection-based-conditional-access/13.png)




**Paso 3: Configurar la directiva de protección de aplicaciones de Intune para iOS y las aplicaciones cliente de Android**


![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Para obtener más información, consulte [proteger aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).


## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>Directiva de aplicación basado en la aplicación o protección para Exchange Online y SharePoint Online

Este escenario consta de una directiva de aplicaciones aprobadas o basado en la protección de aplicaciones para tener acceso a Exchange Online y SharePoint Online.


### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:

- Configura las aplicaciones cliente que están en la lista de aplicaciones que admiten el requisito de directiva de protección de aplicación o el requisito de aplicaciones aprobadas.  
- Usa las aplicaciones cliente que cumplen el requisito de directiva de protección de aplicación y pueden recibir una directiva de protección de aplicaciones de Intune.
- Usa las aplicaciones cliente que cumplan los requisitos de directiva de aplicaciones aprobadas que admite la directiva de protección de aplicaciones de Intune.
- Abre la aplicación para tener acceso a correo electrónico o documentos.
- Abre la aplicación Outlook e inicia sesión con credenciales de Azure AD.
- Se solicita al instalar Microsoft Authenticator para su uso de iOS o Portal de empresa de Intune para Android usan si aún no están instalados.
- Instala la aplicación y puede volver a la aplicación Outlook para continuar.
- Se le pedirá que registre un dispositivo.
- Puede recibir una directiva de protección de aplicaciones de Intune.
- Puede tener acceso a correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune.
- Puede tener acceso a documentos con una aplicación, no en el requisito de directiva de protección de aplicaciones y sitios, pero aparece en el requisito de aplicación aprobada.

Las directivas de protección de aplicaciones de Intune son necesarias para acceder a los datos corporativos. Las directivas podrían solicitar al usuario que reinicie la aplicación o usar un PIN adicional. Esto sucede si se configuran las directivas de la aplicación y plataforma.

**Comentarios:**

- Puede usar este escenario, si desea admitir las dos directivas de aplicación basados en la aplicación y protección de acceso condicional.
- En este *o* se evalúa la directiva, las aplicaciones con un requisito de directiva de protección de aplicación para el acceso en primer lugar antes que el requisito de las aplicaciones cliente aprobadas.

### <a name="configuration"></a>Configuración

**Paso 1: Configurar una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional en este paso, configure los siguientes componentes:

![Acceso condicional](./media/app-protection-based-conditional-access/62.png)

1. Escriba el nombre de la directiva de acceso condicional.

2. En **asignaciones**, en **usuarios y grupos**, seleccione al menos un usuario o grupo para cada directiva de acceso condicional.

3. En **aplicaciones en la nube**, seleccione **Office 365 Exchange Online**. 

     ![Acceso condicional](./media/app-protection-based-conditional-access/02.png)

4. En **condiciones**, configurar **plataformas de dispositivos** y **aplicaciones cliente (versión preliminar)**. 
 
     a. En **plataformas de dispositivos**, seleccione **Android** y **iOS**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

    b. En **aplicaciones cliente (versión preliminar)**, seleccione **aplicaciones móviles y clientes de escritorio** y **clientes de autenticación moderna**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. En **controles de acceso**, seleccione las opciones siguientes:

   - **Requerir aplicación cliente aprobada**

   - **Requerir la directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir uno de los controles seleccionados**
 
     ![Acceso condicional](./media/app-protection-based-conditional-access/12.png)


**Paso 2: Configurar la directiva de protección de aplicaciones de Intune para iOS y las aplicaciones cliente de Android**


![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Para obtener más información, consulte [proteger aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).




## <a name="next-steps"></a>Pasos siguientes

- Si quiere saber cómo configurar una directiva de acceso condicional, consulte [Requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md).
- Si está listo para configurar directivas de acceso condicional para su entorno, consulte [procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md). 