---
title: Exigencia de directiva de protección de aplicaciones para el acceso a la aplicación en la nube con acceso condicional en Azure Active Directory | Microsoft Docs
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
ms.openlocfilehash: f695d50e251d0104cf9f0d38fe4489a0e66dfe15
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288507"
---
# <a name="how-to-require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>Instrucciones: Requerir la directiva de protección de aplicaciones para el acceso a la aplicación en la nube con acceso condicional (versión preliminar)

Los empleados usan dispositivos móviles para tareas personales y profesionales. Mientras se asegura de que los empleados pueden ser productivos, también puede evitar la pérdida de datos. Con el acceso condicional de Azure Active Directory (Azure AD), puede proteger los datos corporativos restringiendo el acceso a las aplicaciones en la nube para las aplicaciones cliente que tengan una directiva de protección de aplicaciones en primer lugar.

En este tema se explica cómo configurar directivas de acceso condicional que pueden requerir la directiva de protección de aplicaciones antes de acceso a datos.

## <a name="overview"></a>Información general

Con el [acceso condicional de Azure AD](overview.md), puede ajustar el modo en que los usuarios autorizados pueden tener acceso a sus recursos. Por ejemplo, puede limitar el acceso de los dispositivos de confianza a las aplicaciones en la nube.

Puede usar [directivas de protección de aplicaciones de Intune](https://docs.microsoft.com/intune/app-protection-policy) para ayudar a proteger los datos de su compañía. Las directivas de protección de aplicaciones de Intune no requieren una solución de administración de dispositivos móviles (MDM), lo que le permite proteger los datos de su empresa con o sin inscribir los dispositivos en una solución de administración de dispositivos.

Acceso condicional de Azure Active Directory restringe el acceso a las aplicaciones en la nube para las aplicaciones cliente que Intune ha notificado a Azure AD como la recepción de una directiva de protección de aplicaciones. Por ejemplo, puede restringir el acceso a Exchange Online a la aplicación de Outlook que tiene una directiva de protección de aplicaciones de Intune.

En la terminología del acceso condicional, estas aplicaciones cliente se conocen como directiva protegido con un **directiva de protección de aplicaciones**.  

![Acceso condicional](./media/app-protection-based-conditional-access/05.png)

Para obtener una lista de la directiva de aplicaciones de cliente protegido, vea [requisito de directiva de protección de aplicación](technical-reference.md#approved-client-app-requirement).

Puede combinar directivas de acceso condicional basado en la protección de aplicación con otras directivas como [directivas de acceso condicional basado en dispositivos](require-managed-devices.md) para proporcionar flexibilidad en cómo proteger los datos de dispositivos personales y corporativos.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Ventajas de requisito de acceso condicional basado en la protección de la aplicación

Similar a cumplimiento se notifica mediante Intune para iOS y Android para dispositivo administrado, Intune ahora informes a Azure AD si se aplica la directiva de protección de aplicaciones para que el acceso condicional puede usar como una comprobación de acceso. Esta nueva directiva de acceso condicional **directiva de protección de aplicaciones** aumenta la seguridad al proteger contra errores de administrador, como:

- usuarios que no tienen una licencia de Intune
- usuarios que no reciben una directiva de protección de aplicaciones de Intune
- Directiva de Intune app protection aplicaciones que no haya sido configurado para recibir una directiva


## <a name="before-you-begin"></a>Antes de empezar

En este tema se da por supuesto que está familiarizado con:

- El [requisito de directiva de protección de aplicación](technical-reference.md#app-protection-policy-requirement) referencia técnica.

- La referencia técnica del [requisito de las aplicaciones cliente aprobadas](technical-reference.md#approved-client-app-requirement).

- Los conceptos básicos del [Acceso condicional en Azure Active Directory](overview.md).

- La [configuración de la directiva de acceso condicional](app-based-mfa.md).


## <a name="prerequisites"></a>Requisitos previos

Para crear una directiva de acceso condicional basado en la protección de aplicaciones, debe
- Tiene un Enterprise Mobility + Security o una suscripción de Azure Active Directory premium + Intune
- Asegúrese de que los usuarios tienen licencia para EMS o Azure AD + Intune
- Asegúrese de que la aplicación cliente se ha configurado en Intune para recibir una directiva de protección de aplicaciones
- Asegúrese de que los usuarios se configuran en Intune para recibir una directiva de protección de aplicaciones de Intune

## <a name="app-protection-based-policy-for-exchange-online"></a>Directiva de aplicación en función de protección para Exchange Online

Este escenario consta de una directiva de acceso condicional basado en la protección de aplicaciones para tener acceso a Exchange Online.

### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:

- Configura el correo electrónico mediante una aplicación de correo electrónico nativa de iOS o Android para conectarse a Exchange.

- Recibe un correo electrónico que indica que el acceso solo está disponible mediante la aplicación Outlook.

- Descarga la aplicación con el vínculo.

- Abre la aplicación Outlook e inicia sesión con las credenciales de Azure AD

- Se le pide que instale Authenticator (iOS) o el Portal de empresa (Android) para continuar

- Instala la aplicación y puede volver a la aplicación Outlook para continuar.

- Se le solicita que registre un dispositivo.

- Puede recibir una directiva de protección de aplicaciones de Intune

- Puede acceder al correo electrónico.

Todas las directivas de Intune app protection deben estar en la aplicación tenga acceso a los datos corporativos y pueden solicitar al usuario que reinicie la aplicación, use un PIN adicional, etc (si está configurado para la aplicación y plataforma).

### <a name="configuration"></a>Configuración

**Paso 1: configurar una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/01.png)

1. El **nombre** de la directiva de acceso condicional.

2. **Users and groups** (Usuarios y grupos): cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Cloud apps:** (Aplicaciones en la nube) como aplicación en la nube debe seleccionar **Office 365 Exchange Online**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. **Condiciones:** como **condiciones**, debe configurar **plataformas de dispositivo** y **aplicaciones cliente**:

     a. Como **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

    b. Como **Aplicaciones cliente (versión preliminar)**, seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de autenticación moderna**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. Como **controles de acceso**, deberá tener **requieren la directiva de protección de aplicaciones (versión preliminar)** seleccionado.

    ![Acceso condicional](./media/app-protection-based-conditional-access/05.png)
 

**Paso 2: configurar una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. El **nombre** de la directiva de acceso condicional.

2. **Users and groups** (Usuarios y grupos): cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.


3. **Cloud apps:** (Aplicaciones en la nube) como aplicación en la nube debe seleccionar **Office 365 Exchange Online**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. **Condiciones:** como **condiciones**, debe configurar **aplicaciones cliente (versión preliminar)**. 

     a. Como **Aplicaciones cliente (versión preliminar)**, seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de Exchange ActiveSync**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

    b. Como **controles de acceso**, deberá tener **requieren la directiva de protección de aplicaciones (versión preliminar)** seleccionado.

    ![Acceso condicional](./media/app-protection-based-conditional-access/05.png)


**Paso 3: configuración de directiva de protección de aplicaciones de Intune para iOS y las aplicaciones cliente de Android**


![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) para más información.



## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>Directiva de dispositivo compatibles o basado en la protección de aplicaciones para Exchange Online

Este escenario consta de una directiva de acceso condicional de dispositivos compatibles o basado en la protección de aplicaciones para tener acceso a Exchange Online.


### <a name="scenario-playbook"></a>Guía de escenario

En este escenario se supone que:
 
- Algún usuario ya está inscrito (con o sin dispositivos corporativos)

- Los usuarios que no están inscritos ni registrados con Azure AD mediante una aplicación protegida deben registrar un dispositivo para acceder a los recursos

- Los usuarios inscritos mediante la aplicación protegida no tienen que volver a registrar el dispositivo

- Usuario puede recibir una directiva de protección de aplicaciones de Intune si no inscrito

- Usuario puede acceder al correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune si no inscrito

- Usuario puede acceder al correo electrónico con Outlook si el dispositivo está inscrito


### <a name="configuration"></a>Configuración

**Paso 1: configurar una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/62.png)

1. El **nombre** de la directiva de acceso condicional.

2. **Users and groups** (Usuarios y grupos): cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Cloud apps:** (Aplicaciones en la nube) como aplicación en la nube debe seleccionar **Office 365 Exchange Online**. 

     ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. **Condiciones:** como **condiciones**, debe configurar **plataformas de dispositivo** y **aplicaciones cliente**. 
 
     a. Como **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

    b. Como **Aplicaciones cliente (versión preliminar)**, seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de autenticación moderna**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. Como **Controles de acceso**, debe tener seleccionado lo siguiente:

   - **Requerir que el dispositivo esté marcado como compatible**

   - **Requerir directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir uno de los controles seleccionados**   
 
     ![Acceso condicional](./media/app-protection-based-conditional-access/11.png)



**Paso 2: configurar una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. El **nombre** de la directiva de acceso condicional.

2. **Users and groups** (Usuarios y grupos): cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Cloud apps:** (Aplicaciones en la nube) como aplicación en la nube debe seleccionar **Office 365 Exchange Online**. 

    ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. **Condiciones:** como **condiciones**, debe configurar **aplicaciones cliente**. 

    Como **Aplicaciones cliente (versión preliminar)**, seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de Exchange ActiveSync**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

5. Como **Controles de acceso**, debe tener seleccionado lo siguiente:

   - **Requerir que el dispositivo esté marcado como compatible**

   - **Requerir directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir uno de los controles seleccionados**   
    ![Acceso condicional](./media/app-protection-based-conditional-access/11.png)



**Paso 3: configuración de directiva de protección de aplicaciones de Intune para iOS y las aplicaciones cliente de Android**


![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) para más información.





## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>Directiva de dispositivo compatibles y basada en la protección de aplicaciones para Exchange Online

Este escenario consta de una directiva de acceso condicional de dispositivos compatibles y basado en la protección de aplicación para acceder a Exchange Online.


### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:
 
-   Configura el correo electrónico mediante una aplicación de correo electrónico nativa de iOS o Android para conectarse a Exchange.
-   Recibe un correo electrónico que indica que es necesario que el dispositivo esté inscrito para poder acceder.
-   Descarga el portal de la empresa e inicia sesión en este.
-   Comprueba el correo electrónico y se le solicita que use la aplicación Outlook.
-   Descarga la aplicación Outlook.
-   Abre Outlook y escribe las credenciales usadas en la inscripción.
-   Es capaz de recibir una directiva de protección de aplicaciones de Intune
-   Puede tener acceso a correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune

Todas las directivas de Intune app protection se activan antes de acceso a los datos corporativos y pueden solicitar al usuario que reinicie la aplicación, use un PIN adicional, etc (si está configurado para la aplicación y plataforma)


### <a name="configuration"></a>Configuración

**Paso 1: configurar una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/01.png)

1. El **nombre** de la directiva de acceso condicional.

2. **Users and groups** (Usuarios y grupos): cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Cloud apps:** (Aplicaciones en la nube) como aplicación en la nube debe seleccionar **Office 365 Exchange Online**. 

     ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. **Condiciones:** como **condiciones**, debe configurar **plataformas de dispositivo** y **aplicaciones cliente**. 
 
     a. Como **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

    b. Como **Aplicaciones cliente (versión preliminar)**, seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de autenticación moderna**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. Como **Controles de acceso**, debe tener seleccionado lo siguiente:

   - **Requerir que el dispositivo esté marcado como compatible**

   - **Requerir directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir todos los controles seleccionados**   
 
     ![Acceso condicional](./media/app-protection-based-conditional-access/13.png)



**Paso 2: configurar una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/06.png)

1. El **nombre** de la directiva de acceso condicional.

2. **Users and groups** (Usuarios y grupos): cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Cloud apps:** (Aplicaciones en la nube) como aplicación en la nube debe seleccionar **Office 365 Exchange Online**. 

    ![Acceso condicional](./media/app-protection-based-conditional-access/07.png)

4. **Condiciones:** como **condiciones**, debe configurar **aplicaciones cliente (versión preliminar)**. 

    Como **Aplicaciones cliente (versión preliminar)**, seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de Exchange ActiveSync**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/92.png)

5. Como **Controles de acceso**, debe tener seleccionado lo siguiente:

   - **Requerir que el dispositivo esté marcado como compatible**

   - **Requerir aplicación cliente aprobada (versión preliminar)**

   - **Requerir todos los controles seleccionados**   
 
     ![Acceso condicional](./media/app-protection-based-conditional-access/13.png)




**Paso 3: configuración de directiva de protección de aplicaciones de Intune para iOS y las aplicaciones cliente de Android**


![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) para más información.


## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>Directiva de aplicación basado en la aplicación o protección para Exchange Online y SharePoint Online

Este escenario consta de una directiva de aplicaciones aprobadas o basado en la protección de aplicaciones para tener acceso a Exchange Online y SharePoint Online.


### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:

- Configura las aplicaciones cliente que son en la lista de aplicaciones que admiten el requisito de directiva de protección de aplicación o el requisito de aplicaciones aprobadas.  
- Las aplicaciones de cliente de usuario usa que cumplan los requisitos de directiva de protección de aplicación pueden recibir una directiva de protección de aplicaciones de Intune
- Usuario utiliza aplicaciones de cliente que cumplan los requisitos de directiva de aplicaciones aprobadas que admite la directiva de protección de aplicaciones de Intune
- Se abre la aplicación para tener acceso a correo electrónico o documentos
- Abre la aplicación Outlook e inicia sesión con las credenciales de Azure AD
- Se le pedirá que instale Authenticator (iOS) o el Portal de empresa (Android) para continuar (si no instalado)
- Instala la aplicación y puede volver a la aplicación Outlook para continuar.
- Se le solicita que registre un dispositivo.
- Puede recibir una directiva de protección de aplicaciones de Intune
- Puede tener acceso a correo electrónico con Outlook y una directiva de protección de aplicaciones de Intune
- Es capaz de acceder a sitios o documentos con una aplicación, no en el requisito de directiva de protección de aplicación pero enumeradas en el requisito de aplicación aprobada.

Las directivas de protección de aplicaciones de Intune son necesarios para el acceso a los datos corporativos y pueden solicitar al usuario que reinicie la aplicación, use un PIN adicional, etc (si está configurado para la aplicación y plataforma)

**Comentarios**

- Puede usar este escenario, si desea admitir las dos directivas de aplicación basados en la aplicación y protección de acceso condicional.

- En este *o* directiva, las aplicaciones con **directiva de protección de aplicaciones** requisito se evalúan para el acceso antes **aplicaciones cliente aprobadas** requisito.

### <a name="configuration"></a>Configuración

**Paso 1: configurar una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-protection-based-conditional-access/62.png)

1. El **nombre** de la directiva de acceso condicional.

2. **Users and groups** (Usuarios y grupos): cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Cloud apps:** (Aplicaciones en la nube) como aplicación en la nube debe seleccionar **Office 365 Exchange Online**. 

     ![Acceso condicional](./media/app-protection-based-conditional-access/02.png)

4. **Condiciones:** como **condiciones**, debe configurar **plataformas de dispositivo** y **aplicaciones cliente**. 
 
     a. Como **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/03.png)

    b. Como **Aplicaciones cliente (versión preliminar)**, seleccione **Aplicaciones móviles y aplicaciones de escritorio** y **Clientes de autenticación moderna**.

    ![Acceso condicional](./media/app-protection-based-conditional-access/91.png)

5. Como **Controles de acceso**, debe tener seleccionado lo siguiente:

   - **Requerir aplicación cliente aprobada**

   - **Requerir directiva de protección de aplicaciones (versión preliminar)**

   - **Requerir uno de los controles seleccionados**   
 
     ![Acceso condicional](./media/app-protection-based-conditional-access/12.png)


**Paso 2: configurar la directiva de protección de aplicaciones de Intune para iOS y las aplicaciones cliente de Android**


![Acceso condicional](./media/app-protection-based-conditional-access/09.png)

Consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) para más información.




## <a name="next-steps"></a>Pasos siguientes

Si quiere saber cómo configurar una directiva de acceso condicional, consulte [Requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md).

Si está listo para configurar directivas de acceso condicional para su entorno, consulte [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md). 