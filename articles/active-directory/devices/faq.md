---
title: Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory | Microsoft Docs
description: Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: c923023cec03e36b1795619bc9da09aee8def629
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55700400"
---
# <a name="azure-active-directory-device-management-faq"></a>Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory

**P: He registrado el dispositivo hace poco. ¿Por qué no puedo ver el dispositivo en mi información del usuario de Azure Portal? O bien ¿por qué el propietario del dispositivo está marcado como N/D para los dispositivos unidos a Azure Active Directory (Azure AD) híbrido?**

**R:** los dispositivos Windows 10 que están unidos a Azure AD híbrido no aparecen bajo los **dispositivos del USUARIO**.
Use la vista **Todos los dispositivos** de Azure Portal. También puede usar un cmdlet [Get-MsolDevice](https://docs.microsoft.com/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) de PowerShell.

Los únicos que aparecen entre los **dispositivos del USUARIO** son los siguientes:

- Todos los dispositivos personales que no están unidos a Azure AD híbrido. 
- Todos los dispositivos que no tienen Windows 10 o Windows Server 2016.
- Todos los dispositivos que no son de Windows. 

--- 

**P: ¿Cómo puedo saber cuál es el estado de registro del dispositivo del cliente?**

**R:** En Azure Portal, vaya a **Todos los dispositivos**. Use el identificador del dispositivo para buscarlo. Compruebe el valor en la columna de tipo de unión. A veces, puede haberse restablecido el dispositivo o su imagen inicial. Por lo tanto, también es esencial comprobar el estado de registro del dispositivo:

- En dispositivos Windows 10 y Windows Server 2016 o posteriores, ejecute `dsregcmd.exe /status`.
- En las versiones anteriores del sistema operativo, ejecute `%programFiles%\Microsoft Workplace Join\autoworkplace.exe`.

---

**P: Puedo ver el registro del dispositivo en la información del usuario de Azure Portal. Además, puedo ver el estado del dispositivo en "registrado". ¿He establecido la configuración correctamente para utilizar el acceso condicional?**

**R:** El estado de unión del dispositivo, que se muestra en **deviceID**, debe coincidir con el de Azure AD y satisfacer los criterios de evaluación del acceso condicional. Para más información, consulte [Require managed devices for cloud app access with conditional access](../conditional-access/require-managed-devices.md) (Exigir dispositivos administrados para el acceso a aplicaciones en la nube con acceso condicional).

---

**P: Eliminé mi dispositivo de Azure Portal o mediante Windows PowerShell, pero el estado local del dispositivo indica que todavía está registrado.**

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

**P: ¿Por qué veo entradas duplicadas del dispositivo en Azure Portal?**

**R:**

-   En Windows 10 y Windows Server 2016, si se realizan varios intentos para separar y volver a unir el mismo dispositivo, pueden aparecer entradas duplicadas. 

-   Cada usuario de Windows que usa **Agregar cuenta profesional o educativa** crea un nuevo registro de dispositivo con el mismo nombre de dispositivo.

-   En las versiones anteriores del sistema operativo Windows que estén unidas a un dominio de Azure Directory local, el registro automático crea un nuevo registro del dispositivo con el mismo nombre de dispositivo para cada usuario del dominio que inicie sesión en dicho dispositivo. 

-   En el caso de las máquinas unidas a Azure AD que se han borrado, se han vuelto a instalar y se han vuelto a unir con el mismo nombre, aparece otro registro con el mismo nombre de dispositivo.

---

**P: ¿Por qué un usuario puede tener acceso a recursos desde un dispositivo que deshabilité en Azure Portal?**

**R:** Una revocación tarda hasta una hora en aplicarse.

>[!NOTE] 
>En el caso de los dispositivos inscritos, se recomienda que borre los datos del dispositivo para asegurarse de que los usuarios no pueden acceder a los recursos. Para obtener más información, consulte [¿Qué es la inscripción de dispositivos?](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>P+F de unión a Azure AD

**P: ¿Cómo se separa un dispositivo unido a Azure AD localmente en el dispositivo?**

**R:** 
- En el caso de los dispositivos unidos a Azure AD híbrido, asegúrese de desactivar el registro automático. De esta forma, la tarea programada no registrará el dispositivo de nuevo. A continuación, abra un símbolo del sistema como administrador y escriba `dsregcmd.exe /debug /leave`. O bien ejecute este comando como un script en varios dispositivos para separarlos de forma masiva.

- Para los dispositivos unidos a Azure AD puro, asegúrese de tener una cuenta de administrador local sin conexión o cree una. No podrá iniciar sesión con credenciales de usuario de Azure AD. A continuación, vaya a **Configuración** > **Cuentas** > **Obtener acceso a trabajo o escuela**. Seleccione su cuenta y luego **Desconectar**. Siga las indicaciones y proporcione las credenciales de administrador local cuando se le solicite. Reinicie el dispositivo para finalizar el proceso de separación.

---

**P: ¿Mis usuarios pueden iniciar sesión en dispositivos unidos a Azure AD que están eliminados o deshabilitados en Azure AD?**

**R:** Sí. Windows tiene una funcionalidad para almacenar en caché un nombre de usuario y contraseña. Esta funcionalidad permite que los usuarios que han iniciado sesión anteriormente accedan rápidamente al escritorio, incluso sin conectividad de red. 

Cuando un dispositivo se elimina o deshabilita en Azure AD, el dispositivo Windows no lo reconoce. Por lo que los usuarios que han iniciado sesión anteriormente siguen teniendo acceso al escritorio con el nombre de usuario y contraseña almacenados en caché. Pero dado que el dispositivo se ha eliminado o deshabilitado, los usuarios no pueden acceder a los recursos protegidos con el acceso condicional basado en el dispositivo. 

Los usuarios que no han iniciado sesión anteriormente no pueden acceder al dispositivo. No se ha habilitado ningún nombre de usuario o contraseña almacenados en caché para ellos. 

---

**P: ¿Los usuarios deshabilitados o eliminados pueden iniciar sesión en dispositivos unidos a Azure AD?**

**R:** Sí, pero solo durante un tiempo limitado. Cuando un usuario se elimina o deshabilita en Azure AD, el dispositivo Windows no lo reconoce inmediatamente. Por lo que los usuarios que han iniciado sesión anteriormente pueden acceder al escritorio con el nombre de usuario y contraseña almacenados en caché. 

Normalmente, el dispositivo reconoce el estado del usuario en menos de cuatro horas. Tras lo cual, Windows bloquea el acceso de esos usuarios al escritorio. Como el usuario está eliminado o deshabilitado en Azure AD, se revocan todos sus tokens, por lo que no puede tener acceso a ninguno de los recursos. 

Los usuarios eliminados o deshabilitados que no han iniciado sesión anteriormente no pueden acceder al dispositivo. No se ha habilitado ningún nombre de usuario o contraseña almacenados en caché para ellos. 

---

**P: ¿Por qué mis usuarios tienen problemas en los dispositivos unidos a Azure AD después de cambiar su UPN?**

**R:** Actualmente, los cambios de UPN no son totalmente compatibles en dispositivos unidos a Azure AD. Por ello, su autenticación con Azure AD genera un error tras la modificación del UPN. Como resultado, los usuarios experimentarán problemas con el acceso condicional y el inicio de sesión único en sus dispositivos. Por tanto, para poder solucionar este problema, los usuarios deben iniciar sesión en Windows con el icono “Otro usuario” mediante su nuevo UPN. Estamos trabajando actualmente para poder resolver este incidente. Sin embargo, los usuarios que inicien sesión con Windows Hello para empresas no tendrán este problema. 

---

**P: Mis usuarios no pueden buscar impresoras desde dispositivos unidos a Azure AD. ¿Cómo se puede habilitar la impresión desde esos dispositivos?**

**R:** Con el fin de implementar impresoras para los dispositivos unidos a Azure AD, consulte [Deploy Windows Server Hybrid Cloud Print with Pre-Authentication](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy) (Implementación de impresión en nube híbrida de Windows Server mediante autenticación previa). Necesita un servidor de Windows Server local para implementar la impresión en nube híbrida. Actualmente, el servicio de impresión basado en la nube no está disponible. 

---

**P: ¿Cómo me conecto a un dispositivo remoto unido a Azure AD?**

**R:** Consulte [Conectarse a un equipo remoto unido a Azure Active Directory](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc).

---

**P: ¿Por qué mis usuarios ven el mensaje *No puedes acceder desde aquí*?**

**R:** ¿Ha configurado determinadas reglas de acceso condicional para requerir un estado de dispositivo específico? Si el dispositivo no cumple los criterios, se bloquea a los usuarios y se les muestra ese mensaje. Examine las reglas de la directiva de acceso condicional. Asegúrese de que el dispositivo cumple los criterios para evitar el mensaje.

---

**P: ¿Por qué algunos de mis usuarios no reciben las indicaciones de Azure Multi-factor Authentication en dispositivos unidos a Azure AD?**

**R:** Un usuario puede unir o registrar un dispositivo a Azure AD mediante Multi-Factor Authentication. De esta forma, el dispositivo se convierte en un segundo factor de confianza para dicho usuario. Siempre que el mismo usuario inicia sesión en el dispositivo y accede a una aplicación, Azure AD considera al dispositivo como segundo factor. Esta función permite que el usuario acceda sin problemas a las aplicaciones sin indicaciones adicionales de Multi-Factor Authentication. 

Este comportamiento no es aplicable a ningún otro usuario que inicie sesión en el dispositivo. Por lo que todos los demás usuarios que accedan a dicho dispositivo recibirán un desafío de Multi-Factor Authentication. Después de realizarlo, podrán tener acceso a las aplicaciones que requieren de Multi-Factor Authentication.

---

**P: ¿Por qué me aparece el mensaje *El nombre de usuario o la contraseña no son correctos* para un dispositivo que recién uní a Azure AD?**

**R:** Las razones comunes para este escenario son las siguientes:

- Las credenciales de usuario ya no son válidas.

- El equipo no puede comunicarse con Azure Active Directory. Compruebe si existen errores de conectividad de red.

- Los inicios de sesión federados requieren que el servidor de federación admita puntos de conexión WS-Trust que estén habilitados y accesibles. 

- Habilitó la autenticación de paso a través. Por tanto, debe cambiar su contraseña temporal al iniciar sesión.

---

**P: ¿Por qué me aparece un cuadro de diálogo que indica que *se ha producido un error* cuando intento unir mi equipo a Azure AD?**

**R:** Este error se produce cuando configura la inscripción de Azure Active Directory con Intune. Asegúrese de que el usuario que intenta unirse a Azure AD tiene asignada la licencia de Intune correcta. Para obtener más información, consulte [Configuración de la inscripción de dispositivos Windows](https://docs.microsoft.com/intune/windows-enroll).  

---

**P: ¿Por qué se produce un error al intentar unir mi equipo a Azure AD a pesar de que no he obtenido información sobre dicho error?**

**R:** Puede deberse a que inició sesión en el dispositivo con la cuenta de administrador integrada local. Cree una cuenta local distinta antes de usar la unión de Azure Active Directory para completar la instalación. 

---

**P: ¿Cuáles son los certificados MS-Organization-P2P-Access presentes en nuestros dispositivos Windows 10?**

**R:** Azure AD emite los certificados Ms-Organization-P2P-Access para los dispositivos unidos a Azure AD y a Azure AD híbrido. Estos certificados se usan para habilitar la confianza entre los dispositivos del mismo inquilino en escenarios de escritorios remotos. Se emite un certificado al dispositivo y otro al usuario. El certificado del dispositivo está presente en `Local Computer\Personal\Certificates` y es válido durante un día. Este certificado se renueva (mediante la emisión de un nuevo certificado) si el dispositivo está activo en Azure AD. El certificado de usuario está presente en `Current User\Personal\Certificates` y dicho certificado también es válido durante un día, pero se emite a petición cuando un usuario intenta establecer una sesión de escritorio remoto con otro dispositivo unido a Azure AD. No se renueva cada vez que expira. Ambos certificados se emiten con el certificado MS-Organization-P2P-Access presente en `Local Computer\AAD Token Issuer\Certificates`. Azure AD emite este certificado durante el registro de dispositivos. 

---

**P: ¿Por qué se muestran varios certificados expirados que emitió MS-Organization-P2P-Access en nuestros dispositivos Windows 10? ¿Cómo puedo eliminarlos?**

**R:** Hubo un problema identificado en Windows 10, versión 1709 y versiones anteriores en el que los certificados expirados de MS-Organization-P2P-Access aún existían en el equipo debido a problemas de cifrado. Los usuarios se pueden encontrar con problemas de conectividad de red si está usando algún cliente VPN (por ejemplo, Cisco AnyConnect) que no pueda gestionar el gran número de certificados expirados. Este problema se corrigió en Windows 10, versión 1803 para que se eliminen automáticamente estos certificados MS-Organization-P2P-Access expirados. Puede solucionar este problema al actualizar los dispositivos a Windows 10, 1803. Si no puede actualizarlos, puede eliminar dichos certificados sin ningún impacto negativo.  

---


## <a name="hybrid-azure-ad-join-faq"></a>P+F de unión a Azure AD híbrido

**P: ¿Dónde puedo encontrar información de solución de problemas para diagnosticar errores de unión a Azure AD híbrido?**

**R:** Para consultar información sobre solución de problemas, vea los artículos:

- [Solución de problemas de dispositivos híbridos de Windows 10 y Windows Server 2016 unidos a Azure Active Directory](troubleshoot-hybrid-join-windows-current.md)

- [Solución de problemas de dispositivos híbridos de nivel inferior unidos a Azure Active Directory](troubleshoot-hybrid-join-windows-legacy.md)
 
**P: ¿Por qué veo un registro registrado duplicado de Azure AD para mi dispositivo híbrido unido a Azure AD de Windows 10 en la lista de dispositivos de Azure AD?**

**R:** Cuando los usuarios agregan sus cuentas a las aplicaciones en un dispositivo unido a un dominio, se les puede preguntar si quieren **agregarla a Windows**. Si indican que **Sí** en la ventana, el dispositivo se registra con Azure AD. El tipo de confianza se marca como registrado en Azure AD. Después de que habilite la unión de Azure AD híbrido en una organización, el dispositivo también se une con Azure AD híbrido. De esta forma, se muestran dos estados para el mismo dispositivo. 

La unión de Azure AD híbrido tiene prioridad sobre el estado registrado en Azure AD. Por tanto, el dispositivo se considera con unión a Azure AD híbrido para cualquier autenticación y evaluación de acceso condicional. Puede eliminar sin problemas el registro de dispositivos registrados en Azure AD desde el portal de Azure AD. Aprenda a [evitar o borrar los dos estados en el equipo de Windows 10](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan#review-things-you-should-know). 


---

**P: ¿Por qué mis usuarios tienen problemas en los dispositivos híbridos unidos a Azure AD en Windows 10 después de cambiar su UPN?**

**R:** Actualmente, los cambios de UPN no son totalmente compatibles en dispositivos híbridos unidos a Azure AD. Aunque los usuarios pueden iniciar sesión en el dispositivo y acceder a sus aplicaciones de forma local, se produce un error en la autenticación con Azure AD después de cambiar un UPN. Como resultado, los usuarios experimentarán problemas con el acceso condicional y el inicio de sesión único en sus dispositivos. Para solucionar el problema, el usuario debe desunir el dispositivo de Azure AD (ejecutar "dsregcmd /leave" con privilegios elevados) y volver a realizar la unión (se produce automáticamente). Estamos trabajando actualmente para poder resolver este incidente. Sin embargo, los usuarios que inicien sesión con Windows Hello para empresas no tendrán este problema. 

---

**P: ¿Requieren los dispositivos unidos a Azure AD híbrido en Windows 10 línea de visión al controlador de dominio para acceder a recursos en la nube?**

**R:**  No. Después de completar la unión a Azure AD híbrido en Windows 10 y de que el usuario ha iniciado sesión al menos una vez, el dispositivo no requiere línea de visión al controlador de dominio para acceder a recursos en la nube. Windows 10 puede obtener inicio de sesión único a aplicaciones de Azure AD desde cualquier lugar con conexión a internet, excepto cuando se cambia una contraseña. Si se cambia una contraseña fuera de la red corporativa (por ejemplo, mediante SSPR de Azure AD), el usuario debe tener línea de visión al controlador de dominio antes de que se pueda iniciar sesión en el dispositivo con la nueva contraseña. En caso contrario, solo puede iniciar sesión con la contraseña anterior, que Azure AD invalida e impide el inicio de sesión único. Sin embargo, este problema no se produce cuando se usa Windows Hello para empresas. Los usuarios que inician sesión con Windows Hello para empresas continúan recibiendo el inicio de sesión único en las aplicaciones de Azure AD después de un cambio de contraseña, aunque no tengan línea de visión a su controlador de dominio. 

---


## <a name="azure-ad-register-faq"></a>P+F de registro de Azure AD

**P: ¿Puedo registrar los dispositivos BYOD de Android o iOS?**

**R:** Sí, pero solo con el servicio de registro de dispositivos de Azure y para clientes híbridos. No es compatible con el servicio de registro de dispositivos local en los servicios de federación de Active Directory (AD FS).

**P: ¿Cómo puedo registrar un dispositivo macOS?**

**R:** Siga estos pasos.

1.  [Cree una directiva de cumplimiento](https://docs.microsoft.com/intune/compliance-policy-create-mac-os).
2.  [Defina una directiva de acceso condicional para dispositivos de Mac OS](../active-directory-conditional-access-azure-portal.md). 

**Comentarios:**

- Los usuarios incluidos en la directiva de acceso condicional necesitan una [versión admitida de Office para macOS](../conditional-access/technical-reference.md#client-apps-condition) para acceder a los recursos. 

- Durante el primer intento de acceso, se pide a los usuarios que inscriban el dispositivo mediante el portal de la empresa.

