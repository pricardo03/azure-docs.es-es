---
title: 'Implementación de autoservicio de restablecimiento de contraseña: Azure Active Directory'
description: Estrategia para la implementación correcta del autoservicio de restablecimiento de contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc2c68c53a7c03d1de08e5cde528f27aa61b0096
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74847276"
---
# <a name="deploy-azure-ad-self-service-password-reset"></a>Implementar el autoservicio de restablecimiento de contraseña de Azure AD

> [!NOTE]
> En esta guía se explica el autoservicio de restablecimiento de contraseña y cómo implementarlo. Si está buscando la herramienta de autoservicio de restablecimiento de contraseña para recuperar la cuenta, vaya a [https://aka.ms/sspr](https://aka.ms/sspr). 

El autoservicio de restablecimiento de contraseña (SSPR) es una característica de Azure Active Directory que permite a los empleados restablecer sus contraseñas sin necesidad de ponerse en contacto con el personal de TI. Los empleados deben registrarse o ser registrados en el autoservicio de restablecimiento de contraseña antes de usar el servicio. Durante el registro, el empleado elige uno o varios métodos de autenticación habilitados por su organización.

SSPR permite el desbloqueo rápido de los empleados para que continúen trabajando con independencia de dónde se encuentren o la hora del día. Al permitir que los usuarios se desbloqueen a sí mismos, su organización puede reducir el tiempo no productivo y los altos costos de soporte técnico para los problemas más comunes relacionados con las contraseñas.

Ayude a los usuarios a registrarse rápidamente mediante la implementación de SSPR junto con otra aplicación o servicio en su organización. Esta acción generará un gran número de inicios de sesión e impulsará el registro.

Antes de implementar SSPR, las organizaciones pueden querer determinar cuántas llamadas al departamento de soporte técnico relacionadas con restablecimientos de contraseñas se hacen durante un período, así como el costo promedio de cada llamada. Pueden usar estos datos después de la implementación para mostrar el valor que SSPR aporta a su organización.  

## <a name="how-sspr-works"></a>Cómo funciona SSPR

1. Cuando un usuario intenta restablecer una contraseña, debe verificar su o sus métodos de autenticación registrados anteriormente para demostrar su identidad.
1. A continuación, el usuario escribe una nueva contraseña.
   1. Para los usuarios solo en la nube, la nueva contraseña se almacena en Azure Active Directory. Para más información, consulte el artículo [Cómo funciona SSPR](concept-sspr-howitworks.md#how-does-the-password-reset-portal-work).
   1. Para los usuarios híbridos, la contraseña se escribe en diferido en la instancia local de Active Directory a través del servicio de Azure AD Connect. Para más información, consulte el artículo [Qué es la escritura diferida de contraseñas](concept-sspr-writeback.md#how-password-writeback-works).

## <a name="licensing-considerations"></a>Consideraciones acerca de las licencias

Las licencias de Azure Active Directory se otorgan por usuario; es decir, cada usuario debe tener una licencia adecuada para las características que usa.

Para más información sobre las licencias, consulte la [página de precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="enable-combined-registration-for-sspr-and-mfa"></a>Habilitar el registro combinado en SSPR y MFA

Microsoft recomienda que las organizaciones habiliten la experiencia de registro combinado para SSPR y autenticación multifactor. Al habilitar esta experiencia de registro combinado, los usuarios solo tienen que seleccionar su información de registro una vez para habilitar ambas características.

![Registro de información de seguridad combinado](./media/howto-sspr-deployment/combined-security-info.png)

La experiencia de registro combinado no requiere que las organizaciones habiliten tanto SSPR como Azure Multi-factor Authentication para usarlos. La experiencia de registro combinado proporciona a las organizaciones una mejor experiencia de usuario en comparación con los componentes individuales tradicionales. Para más información sobre el registro combinado y cómo se habilita, consulte el artículo [Información del registro de seguridad combinado (versión preliminar)](concept-registration-mfa-sspr-combined.md).

## <a name="plan-the-configuration"></a>Planear la configuración

La siguiente configuración es necesaria para habilitar SSPR junto con los valores recomendados.

| Ámbito | Configuración | Valor |
| --- | --- | --- |
| **Propiedades de SSPR** | Se habilitó el restablecimiento de contraseña del autoservicio | Grupo **Seleccionado** para piloto/**Todos** para producción |
| **Métodos de autenticación** | Authentication methods required to register (Métodos de autenticación requeridos para registrarse) | Siempre 1 más de los necesarios para el restablecimiento |
|   | Authentication methods required to reset (Métodos de autenticación requeridos para restablecer) | Uno o dos |
| **Registro** | Exigir a los usuarios que se registren al iniciar sesión | Sí |
|   | Número de días antes de que se solicite a los usuarios que vuelvan a confirmar su información de autenticación | De 90 a 180 días |
| **Notifications** | ¿Quiere notificar a los usuarios los restablecimientos de contraseña? | Sí |
|   | ¿Quiere notificar a todos los administradores cuando otros administradores restablezcan su contraseña? | Sí |
| **Personalización** | Personalizar el vínculo del departamento de soporte técnico | Sí |
|   | Dirección URL o correo electrónico del departamento de soporte técnico personalizados | Dirección de correo electrónico o sitio de soporte técnico |
| **Integración local** | Escritura diferida de contraseñas al Active Directory local | Sí |
|   | Allow users to unlock account without resetting password (Permitir a los usuarios desbloquear las cuentas sin restablecer la contraseña) | Sí |

### <a name="sspr-properties-recommendations"></a>Recomendaciones para las propiedades de SSPR

Al habilitar el autoservicio de restablecimiento de contraseña, elija un grupo de seguridad que se usará durante la prueba piloto.

Cuando planee iniciar el servicio de manera más amplia, se recomienda usar la opción Todos para aplicar SSPR para todos los usuarios de la organización. Si no se establece en Todos, seleccione el grupo de seguridad de Azure AD adecuado o el grupo de AD sincronizado con Azure AD.

### <a name="authentication-methods"></a>Métodos de autenticación

Establezca Set Authentication methods required to register en al menos uno más que el número necesario para el restablecimiento. Permitir varios ofrece a los usuarios flexibilidad cuando necesiten restablecer.

Establezca **Número de métodos requeridos para el restablecimiento** en un nivel apropiado para su organización. Uno requiere la menor fricción, mientras que Dos puede aumentar la posición de seguridad.

Consulte [Qué son los métodos de autenticación](concept-authentication-methods.md) para información detallada sobre qué métodos de autenticación están disponibles para SSPR.

### <a name="registration-settings"></a>Configuración de registro

Establezca **Exigir a los usuarios que se registren al iniciar sesión** en **Sí**. Esta configuración significa que los usuarios se ven obligados a registrarse cuando inician sesión, lo que garantiza que todos los usuarios estén protegidos.

Establezca **Número de días antes de que se solicite a los usuarios que vuelvan a confirmar su información de autenticación** entre **90** y **180** días, a menos que su organización tenga una necesidad de negocio para un plazo de tiempo más corto.

### <a name="notifications-settings"></a>Configuración de notificaciones

Configurar tanto **¿Quiere notificar a los usuarios los restablecimientos de contraseña?** como **¿Quiere notificar a todos los administradores cuando otros administradores restablezcan su contraseña?** en **Sí**. Seleccionar **Sí** en ambos aumenta la seguridad al garantizar que el usuario esté al tanto de que se ha restablecido su contraseña, y que todos los administradores sepan cuando un administrador ha cambiado una contraseña. Si los usuarios o administradores reciben dicha notificación y no han comenzado el cambio, pueden denunciar de inmediato una infracción de seguridad potencial.

### <a name="customization"></a>Personalización

Es fundamental personalizar el **correo electrónico o dirección URL del departamento de soporte técnico** para asegurarse de que los usuarios que tengan problemas puedan obtener ayuda rápidamente. Establezca esta opción en una dirección de correo electrónico o página web de soporte técnico comunes con los que los usuarios estén familiarizados.

### <a name="on-premises-integration"></a>Integración local

Si tiene un entorno híbrido, asegúrese de que **Escritura diferida de contraseñas al Active Directory local** esté establecido en **Sí**. Además, establezca Allow users to unlock account without resetting password en Sí, ya que proporciona más flexibilidad.

### <a name="changingresetting-passwords-of-administrators"></a>Cambiar o restablecer contraseñas de administradores

Las cuentas de administrador son cuentas especiales con permisos elevados. Para protegerlas, rigen las restricciones siguientes al cambiar las contraseñas de los administradores:

- Los administradores empresariales locales o los administradores de dominios no pueden restablecer su contraseña mediante SSPR. Solo pueden cambiar su contraseña en su entorno local. Por lo tanto, se recomiendan no sincronizar las cuentas de administrador de AD local con Azure AD.
- Un administrador no puede usar preguntas y respuestas secretas como método para restablecer la contraseña.

### <a name="environments-with-multiple-identity-management-systems"></a>Entornos con varios sistemas de administración de identidades

Si hay varios sistemas de administración de identidades dentro de un entorno, por ejemplo, administradores de identidad local, como Oracle AM, SiteMinder u otros sistemas, las contraseñas escritas en Active Directory deben sincronizarse con los otros sistemas con una herramienta como el servicio de notificación de cambio de contraseña (PCNS) con Microsoft Identity Manager (MIM). Para información sobre el escenario más complejo, consulte el artículo [Implementación del servicio de notificación de cambio de contraseña de MIM en un controlador de dominio](https://docs.microsoft.com/microsoft-identity-manager/deploying-mim-password-change-notification-service-on-domain-controller).

## <a name="plan-deployment-and-support-for-sspr"></a>Planeación de la implementación y el soporte técnico para SSPR

### <a name="engage-the-right-stakeholders"></a>Interactuar con las partes interesadas adecuadas

Cuando fracasan los proyectos tecnológicos, normalmente se debe a expectativas incorrectas relacionadas con el impacto, los resultados y las responsabilidades. Para evitar estos problemas, asegúrese de involucrar a las partes interesadas correctas y que los roles de las partes interesadas en el proyecto se entiendan bien; para ello, documente las partes interesadas y sus aportes y responsabilidades en el proyecto.

### <a name="communications-plan"></a>Plan de comunicaciones

La comunicación es fundamental para el éxito de cualquier servicio nuevo. Comunique de manera proactiva a los usuarios de qué manera usar el servicio y qué pueden hacer para obtener ayuda si algo no funciona según lo previsto. Revise los [materiales de lanzamiento del autoservicio de restablecimiento de contraseña en el Centro de descargas de Microsoft](https://www.microsoft.com/download/details.aspx?id=56768) para obtener ideas sobre cómo planear su estrategia de comunicación con los usuarios finales.

### <a name="testing-plan"></a>Plan de prueba

Para asegurarse de que la implementación funcione según lo previsto, debe planear un conjunto de casos de prueba que usará para validar la implementación. En la tabla siguiente se incluyen algunos escenarios de prueba útiles que puede usar para documentar los resultados esperados por la organización según las directivas.

| Oportunidad de negocio | Resultado esperado |
| --- | --- |
| Se puede acceder al portal de SSPR desde la red corporativa | Determinado por la organización |
| Se puede acceder al portal de SSPR desde fuera de la red corporativa | Determinado por la organización |
| Restablecer la contraseña del usuario desde el explorador cuando el usuario no está habilitado para el restablecimiento de contraseña | El usuario no puede acceder al flujo de restablecimiento de contraseña |
| Restablecer la contraseña del usuario desde el explorador cuando el usuario no se ha registrado para el restablecimiento de contraseña | El usuario no puede acceder al flujo de restablecimiento de contraseña |
| El usuario inicia sesión cuando se impone el registro de restablecimiento de contraseña | Se solicita al usuario que registre información de seguridad |
| El usuario inicia sesión cuando se ha completado el registro de restablecimiento de contraseña | No se solicita al usuario que registre información de seguridad |
| Se puede acceder al portal de SSPR cuando el usuario no tiene una licencia | Es accesible |
| Restablecer la contraseña de usuario desde la pantalla de bloqueo de un dispositivo Windows 10 unido a Azure AD o Azure AD híbrido después de que el usuario se haya registrado | El usuario puede restablecer la contraseña |
| Los datos de uso y de registro de SSPR están disponibles para los administradores casi en tiempo real | Están disponibles a través de los registros de auditoría |

### <a name="support-plan"></a>Plan de soporte técnico

Aunque SSPR no suele crear problemas de usuario, es importante contar con personal de soporte técnico preparado para afrontar los problemas que puedan surgir.

Si bien un administrador puede cambiar o restablecer la contraseña para los usuarios finales a través del portal de Azure AD, es mejor ayudar a resolver el problema a través de un proceso de soporte de autoservicio.

En la sección de la guía operativa de este documento, cree una lista de casos de soporte técnico y sus causas probables, y crear a una guía para su resolución.

### <a name="auditing-and-reporting"></a>Informes y auditoría

Tras la implementación, muchas organizaciones quieren saber cómo se usa (y si se usa) realmente el autoservicio de restablecimiento de contraseña (SSPR). La característica de informes que proporciona Azure Active Directory (Azure AD) le ayuda a responder preguntas mediante informes creados previamente.

Los registros de auditoría para el registro y el restablecimiento de contraseña están disponibles durante 30 días. Por lo tanto, si la auditoría de seguridad dentro de una empresa requiere una retención más prolongada, los registros deben exportarse y consumirse en una herramienta SIEM, como [Azure Sentinel](../../sentinel/connect-azure-active-directory.md), Splunk o ArcSight.

En una tabla, como la siguiente, documente el cronograma de copias de seguridad, el sistema y las partes responsables. Puede que no necesite separar las copias de seguridad para auditoría e informes, pero debe tener una copia de seguridad independiente desde la cual pueda recuperarse de un problema.

|   | Frecuencia de descarga | Sistema de destino | Parte responsable |
| --- | --- | --- | --- |
| Copia de seguridad de auditoría |   |   |   |
| Copia de seguridad de informes |   |   |   |
| Copia de seguridad de recuperación ante desastres |   |   |   |

## <a name="implementation"></a>Implementación

La implementación se produce en tres fases:

- Configuración de usuarios y licencias
- Configuración de SSPR de Azure AD para el registro y autoservicio
- Configuración de Azure AD Connect para escritura diferida de contraseñas

### <a name="communicate-the-change"></a>Comunicar el cambio

Comience la implementación del plan de comunicaciones que ha desarrollado en la fase de planeación.

### <a name="ensure-groups-are-created-and-populated"></a>Asegurarse de que los grupos se creen y rellenen

Consulte la sección de planeación de métodos de autenticación de contraseña y asegúrese de que los grupos para la implementación piloto o de producción están disponibles y de que se hayan agregado a los grupos todos los usuarios adecuados.

### <a name="apply-licenses"></a>Aplicar las licencias

Los grupos que va a implementar deben tener asignadas licencias de Azure AD Premium. Puede asignar licencias directamente al grupo, o puede usar directivas de licencia existentes, por ejemplo, a través de PowerShell o licencias basadas en grupos.

Puede encontrar información acerca de cómo asignar licencias a grupos de usuarios en el artículo, [Asignación de licencias a usuarios según su pertenencia a un grupo en Azure Active Directory](../users-groups-roles/licensing-groups-assign.md).

### <a name="configure-sspr"></a>Configurar SSPR

#### <a name="enable-groups-for-sspr"></a>Habilitar los grupos para SSPR

1. Acceda a Azure Portal con una cuenta de administrador.
1. Seleccione Todos los servicios y, en el cuadro Filtro, escriba Azure Active Directory y, luego, seleccione Azure Active Directory.
1. En la hoja Active Directory, seleccione Restablecimiento de contraseña.
1. En el panel Propiedades, seleccione Seleccionado. Si quiere habilitar a todos los usuarios, seleccione Todos.
1. En la hoja Directiva de restablecimiento de contraseña predeterminada, escriba el nombre del primer grupo, selecciónelo y, luego, haga clic en Seleccionar en la parte inferior de la pantalla y, luego, seleccione Guardar en la parte superior de la pantalla.
1. Repita este proceso para cada grupo.

#### <a name="configure-the-authentication-methods"></a>Configurar los métodos de autenticación

Para la planeación, consulte la sección de planeación de métodos de autenticación de contraseña de este documento.

1. Seleccione Registro, en Exigir a los usuarios que se registren al iniciar sesión, seleccione Sí, luego, establezca el número de días antes de la expiración y, luego, seleccione Guardar.
1. Seleccione Notificación, configure el valor según su plan y, luego, seleccione Guardar.
1. Seleccione Personalización, configure el valor según su plan y, luego, seleccione Guardar.
1. Seleccione Integración local, configure el valor según su plan y, luego, seleccione Guardar.

### <a name="enable-sspr-in-windows"></a>Habilitar SSPR en Windows

Los dispositivos Windows 10 que ejecutan la versión 1803 o posterior y que están unidos a Azure AD o unidos a Azure AD híbrido pueden restablecer sus contraseñas en la pantalla de inicio de sesión de Windows. Puede encontrar información y pasos para configurar esta funcionalidad en el artículo [Restablecimiento de la contraseña de Azure AD desde la pantalla de inicio de sesión](tutorial-sspr-windows.md).

### <a name="configure-password-writeback"></a>Configurar la escritura diferida de contraseñas

Los pasos para configurar la escritura diferida de contraseñas para su organización se encuentran en el artículo [Procedimiento: Configurar la escritura diferida de contraseñas](howto-sspr-writeback.md).

## <a name="manage-sspr"></a>Administrar SSPR

Roles necesarios para administrar las características asociadas con el autoservicio de restablecimiento de contraseña.

| Rol de negocio | Rol de Azure AD (si es necesario) |
| :---: | :---: |
| Departamento de soporte técnico de nivel 1 | Administrador de contraseñas |
| Departamento de soporte técnico de nivel 2 | Administrador de usuarios |
| Administrador de SSPR | Administrador global |

### <a name="support-scenarios"></a>Escenarios de soporte técnico

Para allanar el camino del equipo de soporte técnico, puede crear una página de P+F en función de las preguntas que reciba de los usuarios. En la tabla siguiente encontrará escenarios comunes de soporte técnico.

| Escenarios | DESCRIPCIÓN |
| --- | --- |
| El usuario no tiene disponible ningún método de autenticación registrado. | Un usuario intenta restablecer la contraseña, pero no tiene disponible ninguno de los métodos de autenticación que ha registrado (ejemplo: ha dejado el teléfono móvil en casa y no puede acceder al correo electrónico). |
| El usuario no recibe mensajes de texto ni llamadas en su teléfono móvil o laboral. | Un usuario intenta verificar su identidad mediante mensaje de texto o llamada, pero no recibe un mensaje de texto ni llamada. |
| El usuario no puede acceder al portal de restablecimiento de contraseñas. | Un usuario quiere restablecer su contraseña, pero no está habilitado para restablecer la contraseña y, por tanto, no puede acceder a la página para actualizar las contraseñas. |
| El usuario no puede establecer una contraseña nueva. | Un usuario finaliza la verificación durante el flujo de restablecimiento de contraseña, pero no puede establecer una contraseña nueva. |
| El usuario no ve un vínculo de restablecimiento de contraseña en un dispositivo Windows 10. | Un usuario intenta restablecer la contraseña desde la pantalla de bloqueo de Windows 10, pero el dispositivo no está unido a Azure AD o no está habilitada la directiva de dispositivos de Intune. |

También puede querer incluir información como la siguiente para solucionar otros problemas.

- Qué grupos están habilitados para SSPR.
- Qué métodos de autenticación se han configurado.
- Las directivas de acceso de la red corporativa o relacionadas con esta.
- Pasos para la solución de problemas en escenarios comunes.

También puede consultar la documentación en línea sobre cómo solucionar problemas del autoservicio de restablecimiento de contraseña para conocer los pasos generales de solución de problemas para los escenarios más comunes de SSPR.

## <a name="next-steps"></a>Pasos siguientes

- [Implementar la protección de contraseña de Azure AD](concept-password-ban-bad.md)

- [Implementar el bloqueo inteligente de Azure AD](howto-password-smart-lockout.md)
