---
title: Implementar estaciones de trabajo - Azure Active Directory administrada de Azure
description: Aprenda a implementar estaciones de trabajo administradas por Azure seguras para reducir el riesgo de infracción debido a una configuración incorrecta o poner en peligro
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 242926c0821e4951d2a2bd2f858f63691baf1017
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307233"
---
# <a name="deploy-a-secure-workstation"></a>Implementar una estación de trabajo segura

Ahora que comprende [por qué es importante la protección del acceso de la estación de trabajo?](concept-azure-managed-workstation.md) es el momento de comenzar el proceso de implementación mediante las herramientas disponibles. Esta guía utilizará los perfiles definidos para crear una estación de trabajo es más seguro desde el principio.

![Implementación de una estación de trabajo segura](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Antes de implementar la solución, se debe seleccionar el perfil que va a usar. Es importante tener en cuenta que puede aplicar cualquiera de los perfiles seleccionados y mover a otra, se asigna el perfil de Intune según sus necesidades. Varios perfiles se pueden usar simultáneamente en una implementación y asignar utilizando las asignaciones de la etiqueta o un grupo.

| Perfil | Bajo | Mejorada | Alto | Especializada | Protegido | Aislado |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Usuario de Azure AD | Sí | Sí | Sí | Sí | Sí | Sí |
| Administrados con Intune | Sí | Sí | Sí | Sí | Sí | Sí |
| Dispositivos registrados en Azure AD | Sí |  |  |  |  | |   |
| Dispositivo unido Azure AD |   | Sí | Sí | Sí | Sí | Sí |
| Línea de base de seguridad de Intune aplicado |   | Sí <br> (Mejorada) | Sí <br> (HighSecurity) | Sí <br> (NCSC) | Sí <br> (Protegido) |  N/D |
| El hardware cumpla segura Windows 10 y estándar |   | Sí | Sí | Sí | Sí | Sí |
| Protección contra amenazas Microsoft Defender habilitado |   | Sí  | Sí | Sí | Sí | Sí |
| Eliminación de derechos de administrador |   |   | Sí  | Sí | Sí | Sí |
| Implementación mediante Microsoft Autopilot |   |   | Sí  | Sí | Sí | Sí |
| Aplicaciones instaladas solo mediante Intune |   |   |   | Sí | Sí |Sí |
| Restringido a solo la lista aprobada de direcciones URL |   |   |   | Sí | Sí |Sí |
| Internet (entrante y saliente bloqueado) |   |   |   |  |  |Sí |

## <a name="license-requirements"></a>Requisitos de licencia

Los conceptos tratados en esta guía supone que Microsoft 365 Enterprise E5 o una SKU equivalente. Algunas de las recomendaciones de esta guía se pueden implementar con las SKU inferior. Puede encontrar información adicional en [licencias de Microsoft 365 Enterprise](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Es posible que desee configurar [licencias basadas en grupo](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) para los usuarios automatizar el aprovisionamiento de licencias.

## <a name="azure-active-directory-configuration"></a>Configuración de Azure Active Directory

Configuración de su directorio de Azure Active Directory (Azure AD), que se va a administrar los usuarios, grupos y dispositivos para las estaciones de trabajo de administrador requiere que habilite servicios de identidad y las características con un [cuenta de administrador](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

Cuando se crea la cuenta de administrador de la estación de trabajo protegida, expone la cuenta de la estación de trabajo actual. Se recomienda que realizar esta configuración inicial y toda la configuración global desde un dispositivo de prueba de errores conocido. Puede considerar la guía para [impedir las infecciones de malware](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection) para reducir el riesgo de exposición en primer lugar la experiencia de primera vez frente a ataques.

Las organizaciones requieren la autenticación multifactor, al menos para los administradores. Consulte [implementar MFA en la nube](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted) para obtener instrucciones de implementación.

### <a name="azure-ad-users-and-groups"></a>Grupos y usuarios de azure AD

Desde el portal de Azure, vaya a **Azure Active Directory** > **usuarios** > **nuevo usuario**. [Crear el usuario de la estación de trabajo seguro](https://docs.microsoft.com/Intune/quickstart-create-user), ¿quién será el administrador del dispositivo.

* **Nombre** -seguras del Administrador de la estación de trabajo
* **Nombre de usuario** - secure-ws-admin@identityitpro.com
* **Rol del directorio** - **administrador limitado** y seleccione la siguiente función administrativa
   * **Administrador de Intune**
* **Creación**

Creamos dos grupos de uno de los usuarios de las estaciones de trabajo y otro para las estaciones de trabajo a sí mismos. Desde el portal de Azure, vaya a **Azure Active Directory** > **grupos** > **nuevo grupo**

Primer grupo de usuarios de la estación de trabajo. Es posible que desee configurar [licencias basadas en grupo](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) para los usuarios de este grupo para automatizar el aprovisionamiento de licencias a los usuarios.

* **Tipo de grupo** -seguridad
* **Nombre del grupo** -Secure estación de trabajo de usuarios
* **Tipo de pertenencia** - asignado
* Agregar su usuario de administrador de la estación de trabajo segura para el grupo
   * secure-ws-admin@identityitpro.com
* Puede agregar otros usuarios que administrará las estaciones de trabajo seguros para el grupo
* **Creación**

Segundo grupo de dispositivos de la estación de trabajo.

* **Tipo de grupo** -seguridad
* **Nombre del grupo** -proteger estaciones de trabajo
* **Tipo de pertenencia** - asignado
* **Creación**

### <a name="azure-ad-device-configuration"></a>Configuración de dispositivo de Azure AD

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Especifique quién puede unir dispositivos a Azure AD

Configurar los dispositivos de configuración en Active Directory para permitir que el grupo de seguridad administrativa unir dispositivos a su dominio. Para configurar esta opción desde el portal de Azure, vaya a **Azure Active Directory** > **dispositivos** > **configuración del dispositivo**. Elija **seleccionados** en **a los usuarios pueden inscribir dispositivos en Azure AD** y seleccione el grupo "Usuarios de estación de trabajo seguro".

#### <a name="removal-of-local-admin-rights"></a>Eliminación de derechos de administrador local

Como parte del lanzamiento de los usuarios de estaciones de trabajo de la VIP, DevOps y estaciones de trabajo seguras no tendrán derechos de administrador en sus equipos. Para configurar esta opción desde el portal de Azure, vaya a **Azure Active Directory** > **dispositivos** > **configuración del dispositivo**. Seleccione **ninguno** en **dispositivos Unidos a administradores locales adicionales en Azure AD**.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Requerir autenticación multifactor para unir dispositivos

Para reforzar aún más el proceso de unión de dispositivos a Azure AD, vaya a **Azure Active Directory** > **dispositivos** > **configuración del dispositivo** Elija **Sí** en **requerir Multi-factor Auth para conectar dispositivos** , a continuación, elija **guardar**.

#### <a name="configure-mdm"></a>Configurar MDM

Desde el portal de Azure, vaya a **Azure Active Directory** > **movilidad (MDM y MAM)**  > **Microsoft Intune**. Cambiar la configuración de **el ámbito de usuario MDM** a **todas** y elija **guardar** como permitiremos que cualquier dispositivo que se administren mediante Intune en este escenario. Se puede encontrar más información en el artículo [Intune Quickstart: Configurar la inscripción automática para dispositivos Windows 10](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment). Las directivas de configuración y el cumplimiento de Intune se creará en un paso posterior.

#### <a name="azure-ad-conditional-access"></a>Acceso condicional de Azure AD

Acceso condicional de Azure AD puede ayudar a mantener estas tareas administrativas con privilegios en dispositivos compatibles. Los usuarios que hemos definido como miembros de la **Secure estación de trabajo de usuarios** grupo se les pedirá que realicen autenticación multifactor al iniciar sesión en aplicaciones en la nube. Agregaremos siga las instrucciones de prácticas recomendadas y excluir las cuentas de acceso de emergencia de la directiva. Puede encontrar información adicional en el artículo [administrar cuentas de acceso de emergencia en Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)

Para configurar el acceso condicional de Azure portal, vaya a **Azure Active Directory** > **acceso condicional** > **nueva directiva**.

* **Nombre** -directiva de solicitud de dispositivo segura
* Assignments
   * **Usuarios y grupos**
      * Incluir - **usuarios y grupos** : seleccione el **Secure estación de trabajo de usuarios** grupo que creó anteriormente
      * Excluir - **usuarios y grupos** -seleccionar cuentas de acceso de emergencia de su organización
   * **Aplicaciones en la nube**
      * Incluir - **todas las aplicaciones en la nube**
* Controles de acceso
   * **GRANT** : seleccione **conceder acceso** botón de radio
      * **Requerir autenticación multifactor**
      * **Requerir que el dispositivo esté marcado como compatible**
      * Para varios controles - **requieren todos los controles seleccionados**
* Habilitar directiva - **en**

Las organizaciones, opcionalmente, pueden crear directivas a países de bloque donde los usuarios no podrían acceder a recursos de la empresa. Se puede encontrar más información acerca de las directivas de acceso condicional basado en la ubicación de IP en el artículo [¿qué es la condición de ubicación en el acceso condicional de Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition)

## <a name="intune-configuration"></a>Configuración de Intune

### <a name="configure-enrollment-status"></a>Configurar el estado de inscripción

Como se describe en la administración de la cadena de suministro, lo que garantiza la estación de trabajo seguro es un dispositivo limpio de confianza se recomienda que al comprar nuevos dispositivos, que los dispositivos se ajuste de fábrica [Windows 10 Pro en modo S](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode), lo que limita la exposición y las vulnerabilidades durante la administración de la cadena de suministro. Una vez que se recibe un dispositivo de su proveedor, se quitará el dispositivo desde el modo de S mediante Autopilot. La guía siguiente proporciona detalles sobre cómo aplicar el proceso de transformación.

Queremos asegurarnos de que los dispositivos estén configurados por completo antes de su uso. Intune proporciona un medio para **bloquea el uso de dispositivos hasta que se instalan todas las aplicaciones y perfiles**. 

1. Desde el **portal Azure** desplácese hasta:
1. **Microsoft Intune** > **inscripción de dispositivos** > **inscripción Windows** > **página de estado de inscripción(versiónpreliminar)**  >  **Predeterminado** > **configuración**.
1. Establecer **mostrar el progreso de instalación de perfil de aplicación** a **Sí**.
1. Establecer **bloquea el uso de dispositivos hasta que se instalan todas las aplicaciones y perfiles** a **Sí**.

### <a name="create-an-autopilot-deployment-profile"></a>Crear un perfil de Autopilot deployment

Después de crear un grupo de dispositivos, debe crear un perfil de implementación para que pueda configurar los dispositivos Autopilot.

1. En Intune en Azure portal, seleccione **inscripción de dispositivos** > **inscripción Windows** > **perfiles de implementación**  >   **Crear perfil**.
1. Para nombre, escriba **proteger el perfil de implementación de la estación de trabajo**. Para obtener la descripción, escriba **implementación de estaciones de trabajo seguras**.
1. Conjunto de convertir todos los dispositivos en Autopilot en Sí. Esta configuración garantiza que todos los dispositivos en la lista de Regístrate con los servicios de implementación de Autopilot.  Permitir 48 horas para que se procesa el registro.
1. Para el modo de implementación, elija **implementación automática (versión preliminar)** . Los dispositivos con este perfil se asocian con el usuario inscribir el dispositivo. Se necesitan credenciales de usuario para inscribir el dispositivo.
1. En la unión a Azure AD como el cuadro, **Unidos a Azure AD** debe elegido y atenuada.
1. Seleccione la configuración de-rápida (OOBE), configure la siguiente opción y dejar otros con el valor predeterminado y, a continuación, seleccione **Aceptar**:
   1. Tipo de cuenta de usuario: **Estándar**
1. Seleccione **Crear** para crear el perfil. El perfil de Autopilot deployment ahora está disponible para asignar a los dispositivos.
1. Elija **asignaciones** > **asignar a** > **grupos seleccionados**
   1. **Seleccionar grupos para incluir** -Secure estación de trabajo de usuarios

### <a name="configure-windows-update"></a>Configuración de Windows Update

Una de las cosas más importantes que se puede hacer una organización es mantener actualizado Windows 10. Para mantener Windows 10 en un estado seguro, implementamos un anillo de actualización para administrar la velocidad a la que se aplican las actualizaciones a estaciones de trabajo. Esta configuración puede encontrarse en el **portal Azure** > **Microsoft Intune** > **las actualizaciones de Software**  >  **Anillos de actualización de Windows 10**.

Vamos a **crear** ha cambiado un anillo de actualización nueva con la siguiente configuración de los valores predeterminados.

* Nombre: **Azure administra las actualizaciones de la estación de trabajo**
* Mantenimiento de canal - **Windows Insider: rápida**
* Aplazamiento de actualizaciones de calidad (días) - **3**
* Período de aplazamiento de actualizaciones de características (días) - **3**
* Comportamiento de actualización automática - **instalar automáticamente y reiniciar sin control de usuario final**
* Bloquea al usuario pausar las actualizaciones de Windows - **bloque**
* Solicitar aprobación del usuario reinicie fuera de horas de trabajo - **necesarios**
* Permitir al usuario que reinicie (reiniciar comprometido) - **necesarios**
   * Realizar la transición a los usuarios comprometido reinicio después de un reinicio automático de (días) - **3**
   * Aviso de reinicio comprometidos aplazamiento (días) - **3**
   * Establecer fecha límite para pendiente se reinicia (días) - **3**

Haga clic en **crear** , a continuación, en el **asignaciones** Agregar pestaña el **proteger estaciones de trabajo** grupo como un grupo incluido.

Encontrará información adicional acerca de las directivas de Windows Update en [directiva CSP - Update](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update)

### <a name="windows-defender-atp-intune-integration"></a>Integración de Intune de Windows Defender ATP

ATP de Windows Defender y Microsoft Intune funcionan conjuntamente para ayudar a evitar infracciones de seguridad y ayudar a limitar el impacto de las infracciones. Las capacidades proporcionará la detección en tiempo real. ATP ofrecerá también nuestro auditoría extensiva de implementación y el registro de los dispositivos de punto de conexión.

Para configurar la integración de Windows Defender ATP Intune en Azure portal, vaya a **Microsoft Intune** > **cumplimiento del dispositivo** > **ATP de Windows Defender** .

1. En el paso 1 en **configurar ATP de Windows Defender**, haga clic en **conectar ATP de Windows Defender a Microsoft Intune en el centro de seguridad de Windows Defender**.
1. En el centro de seguridad de Windows Defender:
   1. Seleccione **configuración** > **características avanzadas**
   1. Para **conexión de Microsoft Intune**, elija **en**
   1. Seleccione **guardar preferencias**
1. Después de establecer una conexión, vuelva a Intune y haga clic en **actualizar** en la parte superior.
1. Establecer **Connect Windows 10.0.15063 de versión de los dispositivos y anterior a Windows Defender ATP** a **en**.
1. **Guardar**

Puede encontrar información adicional en el artículo [protección contra amenazas avanzada de Windows Defender](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="completing-hardening-of-the-workstation-profile"></a>Completar la protección para el perfil de la estación de trabajo

Para completar correctamente la protección de la solución, descargue y ejecute el script en función de la deseada **nivel del perfil** desde el gráfico siguiente.

| Perfil | Ubicación de descarga | Nombre de archivo |
| --- | --- | --- |
| Seguridad baja | N/D |  N/D |
| Mayor seguridad | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Alta seguridad  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Especializada | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline.ps1 |
| Cumplimiento de normas especializadas * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Protegido | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

Cumplimiento de normas especializadas * es un script que aplica la configuración especializada proporcionada en el SecurityBaseline Windows10 NCSC.

Una vez que la secuencia de comandos se ejecuta correctamente, se pueden realizar actualizaciones a las directivas y perfiles en Microsoft Intune. Las secuencias de comandos para los perfiles mejorado y seguro crean directivas y perfiles, pero deben asignar la directiva a su **proteger estaciones de trabajo** grupo.

* Perfiles de configuración de dispositivo de Intune creados por las secuencias de comandos pueden encontrarse en el **portal Azure** > **Microsoft Intune** > **Configuracióndeldispositivo**  >  **Perfiles**.
* Las directivas de cumplimiento de dispositivos Intune creadas por los scripts pueden encontrarse en el **portal Azure** > **Microsoft Intune** > **cumplimiento del dispositivo**  >  **Directivas**.

Para revisar los cambios también se pueden exportar los perfiles y aplicar los cambios al archivo de exportación, como se describe en el SECCON documentación basada en y adicional de protección es necesario.

Ejecutando los datos de Intune exportar script `DeviceConfiguration_Export.ps1` desde el [DeviceConfiguration GiuHub repositorio](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) proporcionará la exportación actual de todos los perfiles de Intune existentes.

## <a name="additional-configurations-and-hardening-to-consider"></a>Consolidación a tener en cuenta y configuraciones adicionales

Las instrucciones proporcionadas habilitó una estación de trabajo protegida, controles adicionales también deben considerarse, como obtener acceso los exploradores alternativos, salidos HTTP permitidas y bloqueadas de sitios Web y la capacidad de ejecutar el script de PowerShell.

### <a name="restrictive-inbound-and-outbound-rules-in-firewall-configuration-service-provider-csp"></a>Reglas restrictivas de entrada y salidas en el proveedor de servicios de configuración de firewall (CSP)

Administración adicional de reglas tanto entrantes y salientes puede actualizarse para reflejar los puntos de conexión permitidos y bloqueados. Puesto que seguimos reforzar la estación de trabajo segura, mueva la restricción a una instrucción deny de todos los entrante y saliente como valor predeterminado y agregar sitios permitidos para la salida reflejar los sitios web comunes y de confianza. La información de configuración adicionales para el proveedor de servicios de configuración de Firewall puede encontrarse en el artículo [Firewall CSP](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp).

De forma predeterminada restringida recomendaciones son:

* Denegar todo el tráfico entrante
* Denegar todos los salientes

El proyecto Spamhaus mantiene una lista buenos que las organizaciones pueden usar como punto de partida para bloquear sitios conocidos como [la lista de bloque de dominio (doble)](https://www.spamhaus.org/dbl/).

### <a name="managing-local-applications"></a>Administración de aplicaciones locales

Quitar aplicaciones locales, incluida la eliminación de aplicaciones de productividad se moverá la estación de trabajo segura a un estado verdaderamente reforzado. En nuestro ejemplo, agregaremos Chrome como explorador predeterminado y restringir la capacidad de modificar el incluidos complementos del explorador mediante Intune.

#### <a name="deploy-applications-using-intune"></a>Implementar aplicaciones con Intune

En algunas situaciones, se necesitan las aplicaciones, como el explorador Google Chrome en la estación de trabajo protegida. El ejemplo siguiente proporciona instrucciones para instalar Chrome en dispositivos en el grupo de seguridad **proteger estaciones de trabajo** creado anteriormente.

1. Descargue el instalador sin conexión [agrupación de cromo de 64 bits de Windows](https://cloud.google.com/chrome-enterprise/browser/download/)
1. Extraiga los archivos y anote la ubicación de la `GoogleChromeStandaloneEnterprise64.msi` debe instalar mediante Intune
1. En el **portal Azure** busque **Microsoft Intune** > **las aplicaciones cliente** > **aplicaciones**  >  **Agregar**
1. En **tipo de aplicación**, elija **Line-of-business**
1. En **archivo de paquete de aplicación**, seleccione el `GoogleChromeStandaloneEnterprise64.msi` desde la ubicación extraída y haga clic en **Aceptar**
1. En **información de la aplicación**, proporcione una descripción y el publicador y elija **Aceptar**
1. Haga clic en **Agregar**.
1. En el **asignaciones** seleccione **disponibles para los dispositivos inscritos** en **tipo de asignación**
1. En **grupos incluidos**, agregue el **proteger estaciones de trabajo** grupo que creó anteriormente
1. Haga clic en **Aceptar** , a continuación, **guardar**

Encontrará instrucciones adicionales sobre la configuración de Chrome en su artículo de soporte técnico [administrar explorador Chrome con Microsoft Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Configurar el portal de empresa para aplicaciones personalizadas

En un modo seguro, la instalación de aplicaciones se restringirá al portal de empresa de Intune. Sin embargo, la instalación del portal requiere acceso a Microsoft Store. En nuestra solución segura, se hará que el portal disponible para todos los dispositivos con un modo sin conexión del portal de empresa.

Instalar una Intune administra la copia de la [Portal de empresa](https://docs.microsoft.com/Intune/store-apps-company-portal-app) permitirá que la capacidad de las herramientas adicionales de inserción a petición a los usuarios de las estaciones de trabajo protegidas.

Algunas organizaciones pueden que deba instalar las aplicaciones de Windows 32 bits o de las aplicaciones que requieren otros preparados para implementar. Para estas aplicaciones, el [herramienta de preparación de Microsoft win32 contenido](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) proporcionará una lista para su uso `.intunewin` archivo de formato para la instalación.

### <a name="setting-up-custom-settings-using-powershell"></a>Configuración personalizada con PowerShell

También usamos un ejemplo del uso de PowerShell para proporcionar extensibilidad en la administración del host. El script configurará un fondo predeterminado en el host. Esta funcionalidad también está disponible en los perfiles y solo sirve para ilustrar la capacidad.

En la solución es posible que sea necesario para configurar algunos controles personalizados y la configuración en estaciones de trabajo seguras. En nuestro ejemplo, cambiamos el fondo de la estación de trabajo mediante Powershell para poder identificar fácilmente el dispositivo como una estación de trabajo segura para su uso. Mientras que en nuestro ejemplo utiliza PowerShell para completar esta tarea, también se puede completar en el portal de Azure.

Nuestro ejemplo usará la siguiente [imagen gratuita genérica en segundo plano](https://i.imgur.com/OAJ28zO.png) y [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) desde el Microsoft Scripting Center para permitir que Windows cargar el fondo en el inicio.

1. Descargar el script en un dispositivo local
1. Actualizar el customerXXXX y la ubicación de descarga del fondo de la que desea para utilizar en la secuencia de comandos para reflejar el archivo en segundo plano y carpeta que le gustaría que use la implementación. En nuestro ejemplo, reemplazamos customerXXXX en el fondo.  
1. Vaya a la **portal Azure** > **Microsoft Intune** > **configuración del dispositivo** > **PowerShell las secuencias de comandos** > **agregar**
1. Proporcione un **nombre** para la secuencia de comandos y especifique el **ubicación del Script** donde lo descargó
1. Seleccione **Configurar**.
   1. Establecer **ejecutar este script mediante el inicio de sesión en las credenciales**a **sí**
   1. Haga clic en **Aceptar**
1. Haga clic en **Crear**
1. Seleccione **asignaciones** > **seleccionar grupos**
   1. Agregar el grupo de seguridad **proteger estaciones de trabajo** creado anteriormente y haga clic en **guardar**

### <a name="using-the-preview-mdm-security-baseline-for-october-2018"></a>Uso de la versión preliminar: Línea de base de seguridad MDM para octubre de 2018

Microsoft Intune ha introducido la característica de administración de línea de base de seguridad proporciona a los administradores una manera sencilla de exigir una postura de seguridad de línea base común. La línea de base proporciona un medio similar para conseguir una forma bloqueada hacia abajo de la estación de trabajo de perfil mejorado.

Para la estación de trabajo segura, implementación que esta línea base no se utiliza como lo estará en conflicto con la implementación de una configuración segura.

|   |   |   |
| :---: | :---: | :---: |
| Por encima de bloqueo | Instalación de dispositivos | Servicios de Escritorio remoto |
| En tiempo de ejecución de la aplicación | Bloqueo del dispositivo | Administración remota |
| Administración de aplicaciones | Servicio de registro de eventos | Llamada a procedimiento remoto |
| Reproducción automática | Experiencia | Search |
| BitLocker | Protección contra vulnerabilidades de seguridad | Pantalla inteligente |
| Browser | Explorador de archivos | Requisitos del sistema|
| Conectividad | Internet Explorer | Wi-Fi |
| Delegación de credenciales | Opciones de seguridad de directivas locales | Administrador de conexiones de Windows |
| Credenciales de la interfaz de usuario | Guía de seguridad de MS | Windows Defender|
| Protección de datos | MSS heredado | El área de trabajo de Windows Ink |
| Device Guard | Potencia | Windows PowerShell |

Para obtener más información sobre esta característica de vista previa puede encontrarse en el artículo [configuración de línea de base de seguridad de Windows Intune](https://docs.microsoft.com/Intune/security-baseline-settings-windows).

## <a name="enroll-and-validate-your-first-device"></a>Inscribir y validar el primer dispositivo

1. Para inscribir el dispositivo, necesita la siguiente información:
   * **Número de serie** : se encuentra en el chasis del dispositivo
   * **Id. de producto de Windows** : se encuentra en **sistema** > **sobre** desde el menú de configuración de Windows.
   * Ejecutando [Get WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) proporcionará un archivo de hash CSV para la inscripción de dispositivos con toda la información necesaria. 
      * Ejecute `Get-WindowsAutoPilotInfo – outputfile device1.csv` para enviar la información como un archivo CSV que se puede importar en Intune.

   > [!NOTE]
   > El script se requieren derechos elevados y se ejecutarán como remoto firmado. Puede usar el siguiente comando para permitir que el script se ejecute correctamente. `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

   *  Puede recopilar esta información al iniciar sesión en una versión de Windows 10 1809 o dispositivo superior para recopilar la información o su distribuidor de hardware puede proporcionar esta información al ordenar los nuevos dispositivos.
1. Recopilar la información necesaria y volver a la **portal Azure**. Vaya a **Microsoft Intune** > **inscripción de dispositivos** > **inscripción Windows** > **dispositivos: Administrar dispositivos Windows Autopilot**, seleccione **importación**y elija el archivo CSV que creó o se han proporcionado.
1. Agregar el dispositivo inscrito ahora al grupo de seguridad **proteger estaciones de trabajo** creado anteriormente.
1. Desde el dispositivo Windows 10 que desea configurar, vaya a **Windows configuración** > **actualización y seguridad** > **recuperación**. Elija **comenzar** en **restablecer este PC** y siga las indicaciones para restablecer y volver a configurar el dispositivo con las directivas de cumplimiento de normas y el perfil configuradas.

Una vez configurado el dispositivo, complete una revisión y compruebe la configuración. Confirme que el primer dispositivo está configurado correctamente antes de continuar con la implementación.

## <a name="assignment-and-monitoring"></a>Asignación y la supervisión

Asignación de dispositivos y usuarios requerirán la asignación de la [seleccionado perfiles](https://docs.microsoft.com/intune/device-profile-assign) para la seguridad de grupo y todos los usuarios nuevos que se concederá permiso al servicio deberán agregarse al grupo de seguridad.

Supervisión de los perfiles que puede hacerse mediante la supervisión [perfiles de Microsoft Intune](https://docs.microsoft.com/intune/device-profile-monitor).

## <a name="next-steps"></a>Pasos siguientes

[Microsoft Intune](https://docs.microsoft.com/intune/index) documentación

[Azure AD](https://docs.microsoft.com/azure/active-directory/index) documentación