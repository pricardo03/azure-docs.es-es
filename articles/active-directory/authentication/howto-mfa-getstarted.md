---
title: 'Planear y ejecutar una implementación de Azure Multi-factor Authentication: Azure Active Directory'
description: Planeación de la implementación de Microsoft Azure Multi-factor Authentication
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11846ca2a323da5889f444024767df4803a48a51
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892218"
---
# <a name="planning-a-cloud-based-azure-multi-factor-authentication"></a>Planeamiento de Azure Multi-factor Authentication en la nube

Las personas se conectan a recursos de la organización en escenarios cada vez más complicados. Los usuarios conectarse desde dispositivos de propiedad de la organización, personales y públicos en y fuera de la red corporativa con los smartphones, tabletas, PC y equipos portátiles, a menudo en varias plataformas. En este mundo conectado siempre, multidispositivo y multiplataforma, es más importante que nunca la seguridad de las cuentas de usuario. Ya no son suficientes para garantizar la seguridad de la cuenta de usuario, especialmente cuando los usuarios tienden a reutilice contraseñas en las cuentas de contraseñas, independientemente de su complejidad, que se usa en dispositivos, redes y plataformas. Sofisticadas "phishing" y otra ingeniería social pueden dar lugar a ataques en los nombres de usuario y contraseñas que se va a registrar y vendidos en la web oscura.

[Azure Multi-factor Authentication (MFA)](concept-mfa-howitworks.md) ayuda a proteger el acceso a datos y aplicaciones. Proporciona una capa adicional de seguridad con una segunda forma de autenticación. Las organizaciones pueden usar [acceso condicional](../conditional-access/overview.md) de la solución ajustarse a sus necesidades específicas.

## <a name="prerequisites"></a>Requisitos previos

Antes de iniciar una implementación de Azure Multi-factor Authentication, existen requisitos previos de los elementos que deben tenerse en cuenta.

| Escenario | Requisito previo |
| --- | --- |
| **Solo en la nube** entorno de identidad con la autenticación moderna | **No hay tareas de requisitos previos adicionales** |
| **Híbrido** escenarios de identidad | [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) se implementa y las identidades de usuario están sincronizadas o federadas con local Active Directory Domain Services con Azure Active Directory. |
| Las aplicaciones heredadas que se publica para el acceso a la nube localmente | Azure AD [Proxy de aplicación](../manage-apps/application-proxy.md) se implementa. |
| Uso de Azure MFA con la autenticación RADIUS | Un [servidor de directivas de redes (NPS)](howto-mfa-nps-extension.md) se implementa. |
| Los usuarios tienen Microsoft Office 2010 o versiones anterior y Apple Mail de iOS 11 o versiones anteriores | Actualización a [Microsoft Office 2013 o versiones posterior](https://support.microsoft.com/help/4041439/modern-authentication-configuration-requirements-for-transition-from-o) y Apple mail de iOS 12 o posterior. Acceso condicional no es compatible con protocolos de autenticación heredados. |

## <a name="plan-user-rollout"></a>Planear el lanzamiento de usuario

El plan de implementación MFA debe incluir una implementación piloto seguida por las fases de implementación que están dentro de su capacidad de soporte técnico. Comenzar la implementación mediante la aplicación de las directivas de acceso condicional a un grupo reducido de usuarios piloto. Después de evaluar el efecto en los usuarios piloto, proceso que se utiliza y comportamientos de registro, puede agregar más grupos a la directiva o agregar más usuarios a los grupos existentes.

### <a name="user-communications"></a>Comunicaciones de usuario

Es fundamental para informar a los usuarios, en las comunicaciones planeadas, acerca de los próximos cambios, los requisitos del registro de MFA de Azure y las acciones del usuario es necesario. Se recomienda que las comunicaciones se desarrollan junto con los representantes de su organización, por ejemplo, los departamentos de recursos humanos, administración de cambios o las comunicaciones.

Microsoft proporciona [plantillas comunicación](https://aka.ms/mfatemplates) y [documentación de usuario final](../user-help/security-info-setup-signin.md) para ayudar a las comunicaciones de borrador. Puede enviar a los usuarios [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com) para registrar directamente seleccionando la **información de seguridad** vínculos en la página.

## <a name="deployment-considerations"></a>Consideraciones de implementación

Azure Multi-factor Authentication se implementa mediante la aplicación de directivas de acceso condicional. Un [directiva de acceso condicional](../conditional-access/overview.md) puede requerir que los usuarios realicen autenticación multifactor cuando se cumplen ciertos criterios, como:

* Todos los usuarios, un usuario específico, el miembro de un rol, o un grupo asignado
* Aplicación de nube específica que se accede
* Plataforma de dispositivo
* Estado del dispositivo
* Ubicación de red o ubicación geográfica de dirección IP
* Aplicaciones cliente
* Inicio de sesión de riesgo (requiere Identity Protection)
* Dispositivos compatible
* Dispositivo unido a Azure AD híbrido
* Aplicación cliente aprobada

Directivas de acceso condicional forzar el registro, que requieren los usuarios no registrados completar el registro en el primer inicio de sesión, una consideración de seguridad importante.

[Azure AD Identity Protection](../identity-protection/howto-configure-risk-policies.md) contribuye para una directiva de registro y las directivas de detección y corrección automatizada de riesgos en el caso de Azure Multi-factor Authentication. Las directivas se pueden crear para forzar los cambios de contraseña cuando hay una amenaza de identidad en peligro o requerir MFA cuando un inicio de sesión se considera riesgo por los siguientes [eventos](../reports-monitoring/concept-risk-events.md):

* Credenciales con fugas
* Inicios de sesión desde direcciones IP anónimas
* Viaje imposible a ubicaciones inusuales
* Inicios de sesión desde ubicaciones desconocidas
* Inicios de sesión desde dispositivos infectados
* Inicios de sesión desde direcciones IP con actividades sospechosas

Algunos de los eventos de riesgo detectados por Azure Active Directory Identity Protection se producen en tiempo real y algunos requieren un procesamiento sin conexión. Los administradores pueden elegir bloquear los usuarios que exhiben comportamientos de riesgo y corrección manualmente, requieren un cambio de contraseña o exigir una autenticación multifactor como parte de sus directivas de acceso condicional.

## <a name="define-network-locations"></a>Definir las ubicaciones de red

Se recomienda que las organizaciones usar acceso condicional para definir su red mediante [ubicaciones con nombre](../conditional-access/location-condition.md#named-locations). Si su organización usa protección de identidad, considere el uso de directivas basadas en riesgos en lugar de las ubicaciones con nombre.

### <a name="configuring-a-named-location"></a>Configurar una ubicación con nombre

1. Abra **Azure Active Directory** en Azure portal
2. Haga clic en **acceso condicional**
3. Haga clic en **ubicaciones con nombre**
4. Haga clic en **nueva ubicación**
5. En el **nombre** campo, proporcione un nombre descriptivo
6. Seleccione si va a definir la ubicación mediante intervalos de IP o países o regiones
   1. Si usa intervalos de direcciones IP
      1. Decida si desea marcar la ubicación como de confianza. Iniciar sesión desde una ubicación de confianza reduce el riesgo de inicio de sesión del usuario. Sólo Marque esta ubicación como de confianza si conoce los intervalos IP especificados están establecidos y son confiables en su organización.
      2. Especifique los intervalos IP
   2. Si usa los países o regiones
      1. Expanda el menú desplegable y seleccione los países o regiones que desee definir para esta ubicación con nombre.
      2. Decida si desea incluir áreas desconocidas. Las áreas desconocidas son direcciones IP que no se pueden asignar a ningún país o región.
7. Haga clic en **Crear**

## <a name="plan-authentication-methods"></a>Planear métodos de autenticación

Los administradores pueden elegir el [métodos de autenticación](../authentication/concept-authentication-methods.md) que desea que estén disponibles para los usuarios. Es importante permitir que más de un método de autenticación para que los usuarios tienen un método de copia de seguridad disponible en caso de que su método principal no está disponible. Los métodos siguientes están disponibles para los administradores habilitar:

### <a name="notification-through-mobile-app"></a>Notificación a través de aplicación móvil

Se envía una notificación de inserción a la aplicación Microsoft Authenticator en su dispositivo móvil. El usuario ve la notificación y selecciona **aprobar** para completar la comprobación. Notificaciones de inserción a través de una aplicación móvil proporcionan la opción menos intrusiva para los usuarios. También son la opción más confiable y segura porque usan una conexión de datos en lugar de telefonía.

### <a name="verification-code-from-mobile-app"></a>Código de verificación desde aplicación móvil

Una aplicación móvil como la aplicación Microsoft Authenticator genera un nuevo código de verificación de OATH cada 30 segundos. El usuario escribe el código de verificación en la interfaz de inicio de sesión. La opción de aplicación móvil puede utilizarse si el teléfono tiene un datos o una señal celular o no.

### <a name="call-to-phone"></a>Llamada al teléfono

Se realiza una llamada de voz automatizada al usuario. El usuario responde a la llamada y presiona **#** en el teclado del teléfono para aprobar su autenticación. Llamada a teléfono es un método excelente de copia de seguridad para el código de notificación o comprobación de una aplicación móvil.

### <a name="text-message-to-phone"></a>Mensaje de texto al teléfono

Se envía un mensaje de texto que contiene un código de verificación al usuario, el usuario pide que escriba el código de verificación en la interfaz de inicio de sesión.

### <a name="choose-verification-options"></a>Elección de opciones de comprobación

1. Vaya a **Azure Active Directory**, **Usuarios**, **Multi-Factor Authentication**.

   ![Acceso al portal de Multi-factor Authentication desde la hoja de usuarios de Azure AD en Azure Portal](media/howto-mfa-getstarted/users-mfa.png)

1. En la nueva pestaña que se abre, vaya a **valor de configuración del servicio**.
1. En **opciones de comprobación**, active todas las casillas para los métodos disponibles para los usuarios.

   ![Configuración de métodos de verificación en la pestaña de configuración del servicio Multi-Factor Authentication](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

1. Haga clic en **Save**(Guardar).
1. Cierre la pestaña **configuración del servicio**.

## <a name="plan-registration-policy"></a>Plan de la directiva de registro

Los administradores deben determinar cómo registrará los usuarios a sus métodos. Las organizaciones deben [habilitar la nueva experiencia de registro combinado](howto-registration-mfa-sspr-combined.md) para Azure MFA y la contraseña de autoservicio de restablecimiento (SSPR). SSPR permite a los usuarios restablecer su contraseña de forma segura con los mismos métodos que se usan para la autenticación multifactor. Se recomienda que esto combinado registro actualmente en versión preliminar pública, porque es una excelente experiencia para los usuarios, con la capacidad de registrar una vez para ambos servicios. Habilitar los mismos métodos para SSPR y Azure MFA, permitirá a los usuarios a registrarse para utilizar ambas características.

### <a name="registration-with-identity-protection"></a>Registro con Identity Protection

Si su organización usa Azure Active Directory Identity Protection, [configurar la directiva de registro MFA](../identity-protection/howto-mfa-policy.md) para pedir a los usuarios se registren la próxima vez que inicie sesión interactivamente.

### <a name="registration-without-identity-protection"></a>Registro sin protección de identidad

Si su organización no tiene licencias que habilitar la protección de identidad, los usuarios le pedirá que registre la próxima vez que MFA es obligatorio al inicio de sesión. No se pueden registrar usuarios para MFA, aunque utilicen las aplicaciones protegidas con MFA. Es importante obtener todos los usuarios registrados para que los actores no válidos no se puede adivinar la contraseña de un usuario y registrarse para MFA en su nombre, lo que efectivamente tomen el control de la cuenta.

#### <a name="enforcing-registration"></a>Aplicar el registro

Mediante los pasos siguientes una directiva de acceso condicional puede obligar a los usuarios a registrarse para la autenticación multifactor

1. Cree un grupo, agregar todos los usuarios que no está registrados.
2. Mediante el acceso condicional, exigir autenticación multifactor para este grupo para el acceso a todos los recursos.
3. Periódicamente, vuelva a evaluar la pertenencia al grupo y quite los usuarios que se han registrado del grupo.

Puede ayudar a identificar los usuarios de Azure MFA registrados y no registrados con comandos de PowerShell que se basan en el [módulo MSOnline de PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0).

#### <a name="identify-registered-users"></a>Identificar a los usuarios registrados

```PowerShell
Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName | Sort-Object userprincipalname 
```

#### <a name="identify-non-registered-users"></a>Identificar a los usuarios no registrados

```PowerShell
Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName | Sort-Object userprincipalname 
```

## <a name="plan-conditional-access-policies"></a>Planeación de las directivas de acceso condicional

Para planear la estrategia de directiva de acceso condicional, que determinará cuándo se requieren MFA y otros controles, consulte [¿qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md).

Es importante evitar que se bloquee accidentalmente el inquilino de Azure AD. Puede mitigar el impacto de esta una falta involuntaria de acceso administrativo a [crear dos o más cuentas de acceso de emergencia en el inquilino](../users-groups-roles/directory-emergency-access.md) y excluirlas de la directiva de acceso condicional.

### <a name="create-conditional-access-policy"></a>Creación de una directiva de acceso condicional

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador global.
1. Vaya a **Azure Active Directory**, **acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Escriba un nombre descriptivo para la directiva.
1. En **usuarios y grupos**:
   * En la pestaña **Incluir**, seleccione el botón de radio **Todos los usuarios**.
   * En el **excluir** ficha, active la casilla para **usuarios y grupos** y elija las cuentas de acceso de emergencia.
   * Haga clic en **Done**(Listo).
1. En **Aplicaciones en la nube**, seleccione el botón de selección **Todas las aplicaciones en la nube**.
   * OPCIONAL: En la pestaña **Excluir**, elija aplicaciones en la nube para las que su organización no requiere MFA.
   * Haga clic en **Done**(Listo).
1. En la sección **Condiciones**:
   * OPCIONAL: Si ha habilitado Azure Identity Protection, puede optar por integrar la evaluación de riesgos de inicio de sesión en la directiva.
   * OPCIONAL: Si ha configurado ubicaciones de confianza o ubicaciones con nombre, puede especificar que se incluyan o excluyan esas ubicaciones de la directiva.
1. En **Conceder**, asegúrese de que el botón de selección **Conceder acceso** está seleccionado.
    * Active la casilla **Requerir autenticación multifactor**.
    * Haga clic en **Seleccionar**.
1. Omita la sección **Sesión**.
1. Establezca la opción **Habilitar directiva** en **Activada**.
1. Haga clic en **Create**(Crear).

![Creación de una directiva de acceso condicional que habilite MFA para los usuarios de Azure Portal en el grupo piloto](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)

## <a name="plan-integration-with-on-premises-systems"></a>Planear la integración con sistemas locales

Algunas aplicaciones heredadas y locales que no autentican directamente en Azure AD, requieren pasos adicionales para usar MFA, incluidas:

* Heredado aplicaciones locales, lo que deberá usar el proxy de aplicación.
* Aplicaciones de RADIUS, que deberá usar el adaptador de MFA con el servidor NPS local.
* Aplicaciones de AD FS, que deberá usar el adaptador de MFA con AD FS 2016 local.

Las aplicaciones que autentican directamente con Azure AD y que tienen autenticación moderna (WS-Fed, SAML, OAuth, OpenID Connect) pueden hacer uso de directivas de acceso condicional directamente.

### <a name="use-azure-mfa-with-azure-ad-application-proxy"></a>Usar Azure MFA con el Proxy de aplicación de Azure AD

Las aplicaciones que residen en el entorno local se puede publicar en Azure AD del inquilino a través de [Azure AD Application Proxy](../manage-apps/application-proxy.md) y puede aprovechar las ventajas de Azure Multi-factor Authentication si están configurados para usar Azure AD autenticación previa.

Estas aplicaciones están sujetos a directivas de acceso condicional que exigir autenticación multifactor de Azure, al igual que cualquier otro Azure AD aplicación integrada.

Del mismo modo, si se exige la autenticación multifactor de Azure para todos los inicios de sesión, local se protegerán las aplicaciones publicadas con Proxy de aplicación de Azure AD.

### <a name="integrating-azure-multi-factor-authentication-with-network-policy-server"></a>La integración de Microsoft Azure Multi-factor Authentication con el servidor de directivas de red

La extensión de Servidor de directivas de redes (NPS) para Azure MFA agrega funcionalidades de MFA basadas en la nube a la infraestructura de autenticación mediante los servidores existentes. Con la extensión NPS, puede agregar verificación de aplicación de teléfono, mensaje de texto o llamada de teléfono al flujo de autenticación existente. Esta integración tiene las siguientes limitaciones:

* Con el protocolo CHAPv2, se admiten solo las notificaciones de inserción de aplicación de autenticador y llamada de voz.
* No se puede aplicar directivas de acceso condicional.

La extensión de NPS actúa como un adaptador entre RADIUS y en la nube de Azure MFA para proporcionar un segundo factor de autenticación para proteger [VPN](howto-mfa-nps-extension-vpn.md), [las conexiones de puerta de enlace de escritorio remoto](howto-mfa-nps-extension-rdg.md), o en otro capaz de RADIUS aplicaciones. Los usuarios que registrarse para MFA de Azure en este entorno le solicitarán todos los intentos de autenticación, la falta de Media de las directivas de acceso condicional MFA siempre es necesaria.

#### <a name="implementing-your-nps-server"></a>Implementar el servidor NPS

Si tiene una instancia NPS implementa y está en uso, hacen referencia a [integrar la infraestructura existente de NPS con Azure Multi-factor Authentication](howto-mfa-nps-extension.md). Si está configurando el NPS por primera vez, consulte [servidor de directivas de redes (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) para obtener instrucciones. Guía de solución de problemas puede encontrarse en el artículo [resolver mensajes de error de la extensión de NPS para Azure Multi-factor Authentication](howto-mfa-nps-extension-errors.md).

#### <a name="prepare-nps-for-users-that-arent-enrolled-for-mfa"></a>Preparación de NPS para que los usuarios que no estén inscritos en MFA

Elija qué sucede cuando los usuarios que no están inscritos en MFA intentan autenticarse. Use la configuración del registro `REQUIRE_USER_MATCH` en la ruta de acceso del registro `HKLM\Software\Microsoft\AzureMFA` para controlar el comportamiento de la característica. Esta configuración tiene una opción de configuración único.

| Clave | Valor | Valor predeterminado |
| --- | --- | --- |
| `REQUIRE_USER_MATCH` | TRUE / FALSE | No establecido (equivalente a TRUE) |

El propósito de esta configuración es determinar qué hacer cuando un usuario no está inscrito en MFA. Los efectos de modificar esta configuración se muestran en la tabla siguiente.

| Configuración | Estado de MFA de usuario | Efectos |
| --- | --- | --- |
| Clave no existe | No inscrito | Desafío de MFA es incorrecta |
| Valor establecido en True o no establecido | No inscrito | Desafío de MFA es incorrecta |
| Clave establecida en False | No inscrito | Autenticación sin MFA |
| Clave establecida en False o True | Inscrito | Debe autenticarse con MFA |

### <a name="integrate-with-active-directory-federation-services"></a>Integración con servicios de federación de Active Directory

Si su organización está federada con Azure AD, puede usar [Azure Multi-factor Authentication para proteger los recursos de AD FS](multi-factor-authentication-get-started-adfs.md), tanto locales como en la nube. MFA de Azure le permite reducir las contraseñas y proporcionan una manera más segura de autenticar. A partir de Windows Server 2016, ahora puede configurar Azure MFA para la autenticación principal.

A diferencia de con AD FS en Windows Server 2012 R2, el adaptador de AD FS 2016 Azure MFA se integra directamente con Azure AD y no requiere un servidor de Azure MFA en el entorno local. El adaptador de MFA de Azure está integrado en Windows Server 2016, y no es necesario para una instalación adicional.

Al usar Azure MFA con AD FS 2016 y la aplicación de destino está sujeto a la directiva de acceso condicional, hay consideraciones adicionales:

* Acceso condicional está disponible cuando la aplicación es un usuario de confianza a Azure AD federado con AD FS 2016.
* Acceso condicional no está disponible cuando la aplicación es un usuario de confianza para AD FS 2016 y está administrada o federada con AD FS 2016.
* Acceso condicional también no está disponible cuando AD FS 2016 está configurado para usar Azure MFA como método de autenticación principal.

#### <a name="ad-fs-logging"></a>Registro de AD FS

Estándar AD FS 2016 iniciar sesión en el registro de seguridad de Windows y el registro de administración de AD FS, contiene información sobre las solicitudes de autenticación y su éxito o error. Datos de registro de eventos dentro de estos eventos se indican si se usó Azure MFA. Por ejemplo, puede contener un identificador de evento de auditoría de FS AD 1200:

```
<MfaPerformed>true</MfaPerformed>
<MfaMethod>MFA</MfaMethod>
```

#### <a name="renew-and-manage-certificates"></a>Renovar y administrar certificados

En cada servidor de AD FS, en el equipo local My Store, habrá un Azure MFA autofirmado certificado titulada OU = Microsoft AD FS Azure MFA, que contiene la fecha de expiración del certificado. Compruebe que el período de validez de este certificado en cada servidor de AD FS para determinar la fecha de expiración.

Si el período de validez de los certificados se a expirar, [generar y comprobar un nuevo certificado MFA en cada servidor de AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa#configure-the-ad-fs-servers).

Las instrucciones siguientes detallan cómo administrar los certificados de Azure MFA en los servidores de AD FS. Al configurar AD FS con Azure MFA, se generan los certificados a través de la `New-AdfsAzureMfaTenantCertificate` cmdlet de PowerShell son válidos durante 2 años. Renovar e instale los certificados renovados antes de que expire ovoid interrupciones del servicio MFA.

## <a name="implement-your-plan"></a>Implementar el Plan

Ahora que ha planeado la solución, puede implementar siguiendo los pasos siguientes:

1. Cumplir los requisitos previos necesarios
   1. Implementar [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) para los escenarios híbridos
   1. Implementar [Azure AD Application Proxy](../manage-apps/application-proxy.md) para en las aplicaciones locales publicadas para el acceso a la nube
   1. Implementar [NPS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) para cualquier autenticación RADIUS
   1. Asegúrese de que los usuarios han actualizado a las versiones compatibles de Microsoft Office con la autenticación moderna habilitada
1. Configurar elegido [métodos de autenticación](#choose-verification-options)
1. Definir su [ubicaciones de red con nombre](../conditional-access/location-condition.md#named-locations)
1. Seleccionar grupos para comenzar la implementación de MFA.
1. Configurar su [directivas de acceso condicional](#create-conditional-access-policy)
1. Configurar la directiva de registro MFA
   1. [MFA combinado y SSPR](howto-registration-mfa-sspr-combined.md)
   1. Con [protección de identidad](../identity-protection/howto-mfa-policy.md)
1. Enviar las comunicaciones de usuario y hacer que los usuarios inscribir en [https://aka.ms/mfasetup](https://aka.ms/mfasetup)
1. [Realizar un seguimiento de quién está inscrito](#identify-non-registered-users)

## <a name="manage-your-solution"></a>Administración de la solución

Informes para Azure MFA

Azure Multi-factor Authentication proporciona informes a través del portal de Azure:

| Informe | Ubicación | DESCRIPCIÓN |
| --- | --- | --- |
| Alertas de fraude y de uso | Azure AD > Inicios de sesión | Proporciona información sobre el uso general, el resumen del usuario, detalles del usuario; así como un historial de alertas de fraude enviadas durante el intervalo de fechas especificado. |

## <a name="troubleshoot-mfa-issues"></a>Solución de problemas de MFA

Buscar soluciones para problemas comunes con Azure MFA en la [artículo de solución de problemas de Azure Multi-factor Authentication](https://support.microsoft.com/help/2937344/troubleshooting-azure-multi-factor-authentication-issues) en el Microsoft Support Center.

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué son los métodos de autenticación?](concept-authentication-methods.md)
* [Habilitar el registro convergente para el restablecimiento de contraseña de autoservicio de Azure Multi-factor Authentication y Azure AD](concept-registration-mfa-sspr-converged.md)
* ¿Por qué se solicitó o no a un usuario que realizará MFA? Consulte la sección [Informe de inicios de sesión de Azure AD en los informes del documento Azure Multi-factor Authentication](howto-mfa-reporting.md#azure-ad-sign-ins-report).