---
title: Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory | Microsoft Docs
description: Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2019
ms.author: joflore
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: acf17971bde840743d17dd0b66078630c2d4e7c1
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518803"
---
# <a name="azure-active-directory-device-management-faq"></a>Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory

###<a name="q-i-registered-the-device-recently-why-cant-i-see-the-device-under-my-user-info-in-the-azure-portal-or-why-is-the-device-owner-marked-as-na-for-hybrid-azure-active-directory-azure-ad-joined-devices"></a>P: He registrado el dispositivo hace poco. ¿Por qué no puedo ver el dispositivo en mi información del usuario de Azure Portal? O bien, ¿por qué es el propietario del dispositivo marcado como no disponible para Azure Active Directory (Azure AD) híbrida de dispositivos Unidos a?

**R:** los dispositivos Windows 10 que están unidos a Azure AD híbrido no aparecen bajo los **dispositivos del USUARIO**.
Use la vista **Todos los dispositivos** de Azure Portal. También puede usar un cmdlet [Get-MsolDevice](https://docs.microsoft.com/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) de PowerShell.

Los únicos que aparecen entre los **dispositivos del USUARIO** son los siguientes:

- Todos los dispositivos personales que no están unidos a Azure AD híbrido. 
- Todos los dispositivos que no tienen Windows 10 o Windows Server 2016.
- Todos los dispositivos que no son de Windows. 

---

### <a name="q-how-do-i-know-what-the-device-registration-state-of-the-client-is"></a>P: ¿Cómo se puede saber qué es el estado de registro de dispositivo del cliente?

**R:** En Azure Portal, vaya a **Todos los dispositivos**. Use el identificador del dispositivo para buscarlo. Compruebe el valor en la columna de tipo de unión. A veces, puede haberse restablecido el dispositivo o su imagen inicial. Por lo tanto, también es esencial comprobar el estado de registro del dispositivo:

- En dispositivos Windows 10 y Windows Server 2016 o posteriores, ejecute `dsregcmd.exe /status`.
- En las versiones anteriores del sistema operativo, ejecute `%programFiles%\Microsoft Workplace Join\autoworkplace.exe`.

---

### <a name="q-i-see-the-device-record-under-the-user-info-in-the-azure-portal-and-i-see-the-state-as-registered-on-the-device-am-i-set-up-correctly-to-use-conditional-access"></a>P: Puedo ver el registro del dispositivo en la información del usuario de Azure Portal. Además, puedo ver el estado del dispositivo en "registrado". ¿Puedo configurar correctamente para usar el acceso condicional?

**R:** El estado de unión del dispositivo, que se muestra en **deviceID**, debe coincidir con el de Azure AD y satisfacer los criterios de evaluación del acceso condicional. Para más información, consulte [Require managed devices for cloud app access with conditional access](../conditional-access/require-managed-devices.md) (Exigir dispositivos administrados para el acceso a aplicaciones en la nube con acceso condicional).

---

### <a name="q-i-deleted-my-device-in-the-azure-portal-or-by-using-windows-powershell-but-the-local-state-on-the-device-says-its-still-registered"></a>P: Eliminé mi dispositivo de Azure Portal o mediante Windows PowerShell, Pero el estado local en el dispositivo indica que todavía está registrado.

**R:** Esta operación es así por diseño. El dispositivo no tiene acceso a los recursos en la nube. 

Si quiere volver a registrar el dispositivo, deberá realizar algunas acciones manualmente en él. 

Para anular el estado de unión de los dispositivos Windows 10 y Windows Server 2016 unidos a un dominio de Active Directory local, realice las siguientes acciones:

1.  Abra el símbolo del sistema como administrador.

2.  Escriba `dsregcmd.exe /debug /leave`.

3.  Cierre la sesión y luego iníciela para desencadenar la tarea programada que registra de nuevo el dispositivo con Azure AD. 

Para las versiones anteriores del sistema operativo Windows que estén unidas a un dominio de Active Directory local, realice las siguientes acciones:

1.  Abra el símbolo del sistema como administrador.
2.  Escriba `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Escriba `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

### <a name="q-why-do-i-see-duplicate-device-entries-in-the-azure-portal"></a>P: ¿Por qué veo entradas duplicadas del dispositivo en el portal de Azure?

**R:**

-   En Windows 10 y Windows Server 2016, si se realizan varios intentos para separar y volver a unir el mismo dispositivo, pueden aparecer entradas duplicadas. 

-   Cada usuario de Windows que usa **Agregar cuenta profesional o educativa** crea un nuevo registro de dispositivo con el mismo nombre de dispositivo.

-   En las versiones anteriores del sistema operativo Windows que estén unidas a un dominio de Azure Directory local, el registro automático crea un nuevo registro del dispositivo con el mismo nombre de dispositivo para cada usuario del dominio que inicie sesión en dicho dispositivo. 

-   En el caso de las máquinas unidas a Azure AD que se han borrado, se han vuelto a instalar y se han vuelto a unir con el mismo nombre, aparece otro registro con el mismo nombre de dispositivo.

---

### <a name="q-does-windows-10-device-registration-in-azure-ad-support-tpms-in-fips-mode"></a>P: ¿El registro de dispositivos Windows 10 en Azure AD admite TPM en el modo FIPS?

**R:** No, actualmente el registro de dispositivos en Windows 10 para todos los Estados de dispositivo - Azure AD híbrido, unión a Azure AD y registrados en Azure AD - no admite TPM en el modo FIPS. Para unir o registrarse en Azure AD, debe estar desactivada para los TPM en dichos dispositivos modo FIPS

---

**P: ¿Por qué un usuario puede tener acceso a recursos desde un dispositivo que deshabilité en Azure Portal?**

**R:** Una revocación tarda hasta una hora en aplicarse.

>[!NOTE] 
>En el caso de los dispositivos inscritos, se recomienda que borre los datos del dispositivo para asegurarse de que los usuarios no pueden acceder a los recursos. Para obtener más información, consulte [¿Qué es la inscripción de dispositivos?](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>P+F de unión a Azure AD

### <a name="q-how-do-i-unjoin-an-azure-ad-joined-device-locally-on-the-device"></a>P: ¿Cómo separar un dispositivo unido a Azure AD localmente en el dispositivo?

**R:** 
- En el caso de los dispositivos unidos a Azure AD híbrido, asegúrese de desactivar el registro automático. De esta forma, la tarea programada no registrará el dispositivo de nuevo. A continuación, abra un símbolo del sistema como administrador y escriba `dsregcmd.exe /debug /leave`. O bien ejecute este comando como un script en varios dispositivos para separarlos de forma masiva.

- Para los dispositivos unidos a Azure AD puro, asegúrese de tener una cuenta de administrador local sin conexión o cree una. No podrá iniciar sesión con credenciales de usuario de Azure AD. A continuación, vaya a **Configuración** > **Cuentas** > **Obtener acceso a trabajo o escuela**. Seleccione su cuenta y luego **Desconectar**. Siga las indicaciones y proporcione las credenciales de administrador local cuando se le solicite. Reinicie el dispositivo para finalizar el proceso de separación.

---

### <a name="q-can-my-users-sign-in-to-azure-ad-joined-devices-that-are-deleted-or-disabled-in-azure-ad"></a>P: ¿Pueden mis usuarios iniciar sesión en dispositivos Unidos a Azure AD que se elimina o deshabilita en Azure AD?

**R:** Sí. Windows tiene una funcionalidad para almacenar en caché un nombre de usuario y contraseña. Esta funcionalidad permite que los usuarios que han iniciado sesión anteriormente accedan rápidamente al escritorio, incluso sin conectividad de red. 

Cuando un dispositivo se elimina o deshabilita en Azure AD, el dispositivo Windows no lo reconoce. Por lo que los usuarios que han iniciado sesión anteriormente siguen teniendo acceso al escritorio con el nombre de usuario y contraseña almacenados en caché. Pero dado que el dispositivo se ha eliminado o deshabilitado, los usuarios no pueden acceder a los recursos protegidos con el acceso condicional basado en el dispositivo. 

Los usuarios que no han iniciado sesión anteriormente no pueden acceder al dispositivo. No se ha habilitado ningún nombre de usuario o contraseña almacenados en caché para ellos. 

---

### <a name="q-can-disabled-or-deleted-users-sign-in-to-azure-ad-joined-devices"></a>P: ¿Pueden deshabilitados o eliminados los usuarios iniciar sesión en dispositivos Unidos a Azure AD?

**R:** Sí, pero solo durante un tiempo limitado. Cuando un usuario se elimina o deshabilita en Azure AD, el dispositivo Windows no lo reconoce inmediatamente. Por lo que los usuarios que han iniciado sesión anteriormente pueden acceder al escritorio con el nombre de usuario y contraseña almacenados en caché. 

Normalmente, el dispositivo reconoce el estado del usuario en menos de cuatro horas. Tras lo cual, Windows bloquea el acceso de esos usuarios al escritorio. Como el usuario está eliminado o deshabilitado en Azure AD, se revocan todos sus tokens, por lo que no puede tener acceso a ninguno de los recursos. 

Los usuarios eliminados o deshabilitados que no han iniciado sesión anteriormente no pueden acceder al dispositivo. No se ha habilitado ningún nombre de usuario o contraseña almacenados en caché para ellos. 

---

### <a name="q-why-do-my-users-have-issues-on-azure-ad-joined-devices-after-changing-their-upn"></a>P: ¿Por qué mis usuarios tienen problemas en los dispositivos Unidos a Azure AD después de cambiar su UPN?

**R:** Actualmente, los cambios de UPN no son totalmente compatibles en dispositivos unidos a Azure AD. Por ello, su autenticación con Azure AD genera un error tras la modificación del UPN. Como resultado, los usuarios experimentarán problemas con el acceso condicional y el inicio de sesión único en sus dispositivos. Por tanto, para poder solucionar este problema, los usuarios deben iniciar sesión en Windows con el icono “Otro usuario” mediante su nuevo UPN. Estamos trabajando actualmente para poder resolver este incidente. Sin embargo, los usuarios que inicien sesión con Windows Hello para empresas no tendrán este problema. 

---

### <a name="q-my-users-cant-search-printers-from-azure-ad-joined-devices-how-can-i-enable-printing-from-those-devices"></a>P: Mis usuarios no pueden buscar impresoras desde dispositivos unidos a Azure AD. ¿Cómo se puede habilitar la impresión desde estos dispositivos?

**R:** Con el fin de implementar impresoras para los dispositivos unidos a Azure AD, consulte [Deploy Windows Server Hybrid Cloud Print with Pre-Authentication](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy) (Implementación de impresión en nube híbrida de Windows Server mediante autenticación previa). Necesita un servidor de Windows Server local para implementar la impresión en nube híbrida. Actualmente, el servicio de impresión basado en la nube no está disponible. 

---

### <a name="q-how-do-i-connect-to-a-remote-azure-ad-joined-device"></a>P: ¿Cómo conecto a una remota de Azure AD dispositivo unido a?

**R:** Consulte [Conectarse a un equipo remoto unido a Azure Active Directory](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc).

---

### <a name="q-why-do-my-users-see-you-cant-get-there-from-here"></a>P: ¿Por qué ve Mis usuarios *no se puede llegar allí desde aquí*?

**R:** ¿Ha configurado determinadas reglas de acceso condicional para requerir un estado de dispositivo específico? Si el dispositivo no cumple los criterios, se bloquea a los usuarios y se les muestra ese mensaje. Examine las reglas de la directiva de acceso condicional. Asegúrese de que el dispositivo cumple los criterios para evitar el mensaje.

---

### <a name="q-why-dont-some-of-my-users-get-azure-multi-factor-authentication-prompts-on-azure-ad-joined-devices"></a>P: ¿Por qué no algunos de Mis usuarios obtención solicitudes de Azure Multi-factor Authentication en dispositivos Unidos a Azure AD?

**R:** Un usuario puede unir o registrar un dispositivo a Azure AD mediante Multi-Factor Authentication. De esta forma, el dispositivo se convierte en un segundo factor de confianza para dicho usuario. Siempre que el mismo usuario inicia sesión en el dispositivo y accede a una aplicación, Azure AD considera al dispositivo como segundo factor. Esta función permite que el usuario acceda sin problemas a las aplicaciones sin indicaciones adicionales de Multi-Factor Authentication. 

Este comportamiento:

- Es aplicable a dispositivos unidos a Azure AD y a dispositivos registrados de Azure AD, pero no a dispositivos unidos a Azure AD híbridos.

- No es aplicable a ningún otro usuario que inicie sesión en el dispositivo. Por lo que todos los demás usuarios que accedan a dicho dispositivo recibirán un desafío de Multi-Factor Authentication. Después de realizarlo, podrán tener acceso a las aplicaciones que requieren de Multi-Factor Authentication.

---

### <a name="q-why-do-i-get-a-username-or-password-is-incorrect-message-for-a-device-i-just-joined-to-azure-ad"></a>P: ¿Por qué obtengo un *nombre de usuario o contraseña son incorrectos* mensaje para un dispositivo acaba de Unidos a Azure AD?

**R:** Las razones comunes para este escenario son las siguientes:

- Las credenciales de usuario ya no son válidas.

- El equipo no puede comunicarse con Azure Active Directory. Compruebe si existen errores de conectividad de red.

- Los inicios de sesión federados requieren que el servidor de federación admita puntos de conexión WS-Trust que estén habilitados y accesibles. 

- Habilitó la autenticación de paso a través. Por tanto, debe cambiar su contraseña temporal al iniciar sesión.

---

### <a name="q-why-do-i-see-the-oops-an-error-occurred-dialog-when-i-try-to-azure-ad-join-my-pc"></a>P: ¿Por qué veo el *¡vaya!... se produjo un error.* ¿cuadro de diálogo cuando se intenta en Azure AD unir mi equipo?

**R:** Este error se produce cuando configura la inscripción de Azure Active Directory con Intune. Asegúrese de que el usuario que intenta unirse a Azure AD tiene asignada la licencia de Intune correcta. Para obtener más información, consulte [Configuración de la inscripción de dispositivos Windows](https://docs.microsoft.com/intune/windows-enroll).  

---

### <a name="q-why-did-my-attempt-to-azure-ad-join-a-pc-fail-although-i-didnt-get-any-error-information"></a>P: ¿Por qué mi intento de Azure AD unió a un error de PC, aunque no obtengo ninguna información de error?

**R:** Puede deberse a que inició sesión en el dispositivo con la cuenta de administrador integrada local. Cree una cuenta local distinta antes de usar la unión de Azure Active Directory para completar la instalación. 

---

### <a name="qwhat-are-the-ms-organization-p2p-access-certificates-present-on-our-windows-10-devices"></a>P: ¿Cuáles son los certificados de MS organización P2P acceso presentes en nuestros dispositivos Windows 10?

**R:** Azure AD emite los certificados Ms-Organization-P2P-Access para los dispositivos unidos a Azure AD y a Azure AD híbrido. Estos certificados se usan para habilitar la confianza entre los dispositivos del mismo inquilino en escenarios de escritorios remotos. Se emite un certificado al dispositivo y otro al usuario. El certificado del dispositivo está presente en `Local Computer\Personal\Certificates` y es válido durante un día. Este certificado se renueva (mediante la emisión de un nuevo certificado) si el dispositivo está activo en Azure AD. El certificado de usuario está presente en `Current User\Personal\Certificates` y dicho certificado también es válido durante un día, pero se emite a petición cuando un usuario intenta establecer una sesión de escritorio remoto con otro dispositivo unido a Azure AD. No se renueva cada vez que expira. Ambos certificados se emiten con el certificado MS-Organization-P2P-Access presente en `Local Computer\AAD Token Issuer\Certificates`. Azure AD emite este certificado durante el registro de dispositivos. 

---

### <a name="qwhy-do-i-see-multiple-expired-certificates-issued-by-ms-organization-p2p-access-on-our-windows-10-devices-how-can-i-delete-them"></a>¿Q:Why veo varios certificados expirados emitidos por el acceso a MS-organización-P2P en nuestros dispositivos Windows 10? ¿Cómo puedo eliminarlos?

**R:** Hubo un problema identificado en Windows 10, versión 1709 y versiones anteriores en el que los certificados expirados de MS-Organization-P2P-Access aún existían en el equipo debido a problemas de cifrado. Los usuarios se pueden encontrar con problemas de conectividad de red si está usando algún cliente VPN (por ejemplo, Cisco AnyConnect) que no pueda gestionar el gran número de certificados expirados. Este problema se corrigió en Windows 10, versión 1803 para que se eliminen automáticamente estos certificados MS-Organization-P2P-Access expirados. Puede solucionar este problema al actualizar los dispositivos a Windows 10, 1803. Si no puede actualizarlos, puede eliminar dichos certificados sin ningún impacto negativo.  

---


## <a name="hybrid-azure-ad-join-faq"></a>P+F de unión a Azure AD híbrido

### <a name="q-where-can-i-find-troubleshooting-information-to-diagnose-hybrid-azure-ad-join-failures"></a>P: ¿Dónde puedo encontrar solución de problemas de información para diagnosticar errores de combinación de Azure AD híbrido?

**R:** Para consultar información sobre solución de problemas, vea los artículos:

- [Solución de problemas de dispositivos híbridos de Windows 10 y Windows Server 2016 unidos a Azure Active Directory](troubleshoot-hybrid-join-windows-current.md)

- [Solución de problemas de dispositivos híbridos de nivel inferior unidos a Azure Active Directory](troubleshoot-hybrid-join-windows-legacy.md)
 
### <a name="q-why-do-i-see-a-duplicate-azure-ad-registered-record-for-my-windows-10-hybrid-azure-ad-joined-device-in-the-azure-ad-devices-list"></a>P: ¿Por qué veo un Azure AD duplicado registro registrado para mi Azure AD híbrido de Windows 10 Unidos a un dispositivo en la lista de dispositivos de Azure AD?

**R:** Cuando los usuarios agregan sus cuentas a las aplicaciones en un dispositivo unido a un dominio, se les puede preguntar si quieren **agregarla a Windows**. Si indican que **Sí** en la ventana, el dispositivo se registra con Azure AD. El tipo de confianza se marca como registrado en Azure AD. Después de que habilite la unión de Azure AD híbrido en una organización, el dispositivo también se une con Azure AD híbrido. De esta forma, se muestran dos estados para el mismo dispositivo. 

La unión de Azure AD híbrido tiene prioridad sobre el estado registrado en Azure AD. Por tanto, el dispositivo se considera con unión a Azure AD híbrido para cualquier autenticación y evaluación de acceso condicional. Puede eliminar sin problemas el registro de dispositivos registrados en Azure AD desde el portal de Azure AD. Aprenda a [evitar o borrar los dos estados en el equipo de Windows 10](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan#review-things-you-should-know). 


---

### <a name="q-why-do-my-users-have-issues-on-windows-10-hybrid-azure-ad-joined-devices-after-changing-their-upn"></a>P: ¿Por qué mis usuarios tienen problemas en los dispositivos de Windows 10 híbridos Unidos a Azure AD después de cambiar su UPN?

**R:** Actualmente, los cambios de UPN no son totalmente compatibles en dispositivos híbridos unidos a Azure AD. Aunque los usuarios pueden iniciar sesión en el dispositivo y acceder a sus aplicaciones de forma local, se produce un error en la autenticación con Azure AD después de cambiar un UPN. Como resultado, los usuarios experimentarán problemas con el acceso condicional y el inicio de sesión único en sus dispositivos. Para solucionar el problema, el usuario debe desunir el dispositivo de Azure AD (ejecutar "dsregcmd /leave" con privilegios elevados) y volver a realizar la unión (se produce automáticamente). Estamos trabajando actualmente para poder resolver este incidente. Sin embargo, los usuarios que inicien sesión con Windows Hello para empresas no tendrán este problema. 

---

### <a name="q-do-windows-10-hybrid-azure-ad-joined-devices-require-line-of-sight-to-the-domain-controller-to-get-access-to-cloud-resources"></a>P: ¿Dispositivos Unidos a Azure AD de Windows 10 híbridos requieren línea de visión al controlador de dominio para obtener acceso a recursos en la nube?

**R:** Por lo general no, excepto cuando se cambia la contraseña del usuario. Después de completar la unión a Azure AD híbrido en Windows 10 y de que el usuario ha iniciado sesión al menos una vez, el dispositivo no requiere línea de visión al controlador de dominio para acceder a recursos en la nube. Windows 10 puede obtener inicio de sesión único a aplicaciones de Azure AD desde cualquier lugar con conexión a internet, excepto cuando se cambia una contraseña. Los usuarios que inicie sesión con Windows Hello para empresas continuar recibiendo solo inicie sesión en aplicaciones de Azure AD incluso después de cambiar una contraseña, aunque no tengan la línea de visión a su controlador de dominio. 

---

### <a name="q-what-happens-if-a-user-changes-their-password-and-tries-to-login-to-their-windows-10-hybrid-azure-ad-joined-device-outside-the-corporate-network"></a>P: ¿Qué ocurre si un usuario cambia su contraseña e intenta iniciar sesión en su Azure AD híbrido de Windows 10 Unidos a un dispositivo fuera de la red corporativa?

**R:** Si se cambia una contraseña fuera de la red corporativa (por ejemplo, mediante el uso de SSPR de Azure AD), se producirá un error en el inicio de sesión de usuario con la nueva contraseña. Para dispositivos Unidos a Azure AD híbridos, en el entorno local de Active Directory es la autoridad principal. Cuando un dispositivo no tiene línea de visión al controlador de dominio, es no se puede validar la nueva contraseña. Por lo tanto, el usuario necesita para establecer conexión con el controlador de dominio (ya sea a través de VPN o que están en la red corporativa) antes de que se pueden iniciar sesión el dispositivo con su nueva contraseña. En caso contrario, sólo pueden firmar con su antigua contraseña debido a la capacidad de inicio de sesión almacenado en caché en Windows. Sin embargo, la contraseña antigua se invalida durante las solicitudes de token de Azure AD y por lo tanto, impide el inicio de sesión único en y se produce un error de las directivas de acceso condicional basado en dispositivos. Este problema no se produce si usa Windows Hello para empresas. 

---


## <a name="azure-ad-register-faq"></a>P+F de registro de Azure AD

### <a name="q-can-i-register-android-or-ios-byod-devices"></a>P: ¿Puedo registrar los dispositivos Android o iOS BYOD?

**R:** Sí, pero solo con el servicio de registro de dispositivos de Azure y para clientes híbridos. No es compatible con el servicio de registro de dispositivos local en los servicios de federación de Active Directory (AD FS).

### <a name="q-how-can-i-register-a-macos-device"></a>P: ¿Cómo puedo registrar un dispositivo macOS?

**R:** Siga estos pasos.

1.  [Cree una directiva de cumplimiento](https://docs.microsoft.com/intune/compliance-policy-create-mac-os).
2.  [Defina una directiva de acceso condicional para dispositivos de Mac OS](../active-directory-conditional-access-azure-portal.md). 

**Comentarios:**

- Los usuarios incluidos en la directiva de acceso condicional necesitan una [versión admitida de Office para macOS](../conditional-access/technical-reference.md#client-apps-condition) para acceder a los recursos. 

- Durante el primer intento de acceso, se pide a los usuarios que inscriban el dispositivo mediante el portal de la empresa.

