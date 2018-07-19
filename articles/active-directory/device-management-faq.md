---
title: Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory | Microsoft Docs
description: Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 864f790db48d3d4542ed56a4c7272a198df5bd56
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901143"
---
# <a name="azure-active-directory-device-management-faq"></a>Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory



**P: ¿Cómo puedo registrar un dispositivo macOS?**

**R:** Para registrar el dispositivo Mac OS:

1.  [Cree una directiva de cumplimiento](https://docs.microsoft.com/intune/compliance-policy-create-mac-os).
2.  [Defina una directiva de acceso condicional para dispositivos de Mac OS](active-directory-conditional-access-azure-portal.md). 

**Comentarios:**

- Los usuarios que se incluyen en la directiva de acceso condicional necesitan un [versión admitida de Office para Mac OS](active-directory-conditional-access-technical-reference.md#client-apps-condition) para acceder a los recursos. 

- Durante el primer intento de acceso, se pide a los usuarios que inscriban el dispositivo mediante el portal de empresa.

---

**P: He registrado el dispositivo hace poco. ¿Por qué no puedo ver el dispositivo en la información del usuario en Azure Portal?**

**R:** los dispositivos Windows 10 que están unidos a Azure AD híbrido no aparecen bajo los dispositivos del USUARIO.
Debe usar la vista Todos los dispositivos de Azure Portal. También puede usar el cmdlet [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) de PowerShell.

Los únicos que aparecen entre los dispositivos del USUARIO son los siguientes:

- Todos los dispositivos personales que no están unidos a Azure AD híbrido. 
- Todos los dispositivos que no tienen Windows 10 o Windows Server 2016
- Todos los dispositivos que no son de Windows 

--- 

**P: ¿Cómo puedo saber cuál es el estado de registro del dispositivo del cliente?**

**R:** Puede usar Azure Portal, ir a Todos los dispositivos y buscar el dispositivo mediante el identificador de dispositivo. Compruebe el valor en la columna de tipo de unión.

Si desea comprobar el estado del registro del dispositivo local desde un dispositivo registrado:

- En dispositivos Windows 10 y Windows Server 2016 o posteriores, ejecute dsregcmd.exe /status.
- En las versiones anteriores del sistema operativo, ejecute "%programFiles%\Microsoft Workplace Join\autoworkplace.exe"

---

**P: He realizado la eliminación en Azure Portal o mediante Windows PowerShell, pero el estado local en el dispositivo indica que todavía está registrado.**

**R:** Se debe al diseño. El dispositivo no puede acceder a los recursos en la nube. 

Si desea volver a registrar el dispositivo, deberá realizar algunas acciones manualmente. 

Para anular el estado de unión de los dispositivos Windows 10 y Windows Server 2016 unidos a un dominio de AD local:

1.  Abra el símbolo del sistema como administrador.

2.  Escriba `dsregcmd.exe /debug /leave`

3.  Cierre la sesión y luego iníciela para desencadenar la tarea programada que registra de nuevo el dispositivo con Azure AD. 

En las versiones anteriores del sistema operativo Windows que están unidas a un dominio de AD local:

1.  Abra el símbolo del sistema como administrador.
2.  Escriba `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Escriba `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---
**P: ¿Cómo se separa un dispositivo unido a Azure AD localmente en el dispositivo?**

**R:** 
- Para dispositivos unidos a Azure AD híbrido, asegúrese de desactivar el registro automático para que la tarea programada no registre el dispositivo nuevamente. A continuación, abra un símbolo del sistema como administrador y escriba `dsregcmd.exe /debug /leave`. O bien, se puede ejecutar este comando como un script en varios dispositivos para realizar la separación de forma masiva.

- Para dispositivos unidos a Azure AD puros, asegúrese de tener una cuenta de administrador local sin conexión o cree una, ya que no podrá iniciar sesión con credenciales de usuario de Azure AD. A continuación, vaya a **Configuración** > **Cuentas** > **Obtener acceso a trabajo o escuela**. Seleccione su cuenta y haga clic en **Desconectar**. Siga las indicaciones y proporcione las credenciales de administrador local cuando se le solicite. Reinicie el dispositivo para completar el proceso de separación.

---

**P: Mis usuarios no pueden buscar impresoras desde dispositivos unidos a Azure AD. ¿Cómo se puede habilitar la impresión desde dispositivos unidos a Azure AD?**

**R:** Para implementar impresoras para dispositivos unidos a Azure AD, consulte el artículo sobre [la impresión en nube híbrida](https://docs.microsoft.com/en-us/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). Necesitará un servidor de Windows Server local para implementar la impresión en nube híbrida. Actualmente, el servicio de impresión basado en la nube no está disponible. 

---

**P: ¿Por qué veo entradas duplicadas del dispositivo en Azure Portal?**

**R:**

-   En Windows 10 y Windows Server 2016, si se realizan varios intentos para desunir y volver a unir el mismo dispositivo, pueden aparecer entradas duplicadas. 

-   Si ha usado Agregar cuenta profesional o educativa, cada usuario de Windows que usa Agregar cuenta profesional o educativa creará un nuevo registro de dispositivo con el mismo nombre de dispositivo.

-   En las versiones anteriores del sistema operativo Windows que estén unidas a un dominio de AD local con el registro automático, se creará un nuevo registro del dispositivo con el mismo nombre de dispositivo para cada usuario del dominio registrado en el dispositivo. 

-   En el caso de las máquinas unidas a Azure AD que se han borrado, se han vuelto a instalar y se han vuelto a unir con el mismo nombre, aparecerá otro registro con el mismo nombre de dispositivo.

---

**P: ¿Por qué puede un usuario tener acceso a recursos desde un dispositivo que he deshabilitado en Azure Portal?**

**R:** Una revocación puede tardar hasta una hora en aplicarse.

>[!Note] 
>En el caso de los dispositivos inscritos, se recomienda borrar el dispositivo para asegurarse de que los usuarios no pueden acceder a los recursos. Para más información, vea [Inscribir dispositivos para administrarlos en Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**P: ¿Por qué mis usuarios ven un mensaje que indica que no pueden acceder desde aquí?**

**R:** Si ha configurado determinadas reglas de acceso condicional para requerir un estado de dispositivo específico y el dispositivo no cumple los criterios, a los usuarios se les bloquea y ven este mensaje. Evalúe las reglas de la directiva de acceso condicional y asegúrese de que el dispositivo es capaz de cumplir los criterios para evitar ese mensaje.

---

**P: ¿Por qué algunos de mis usuarios no reciben los mensajes de MFA en los dispositivos unidos a Azure AD?**

**R:** Si el usuario se une o registra un dispositivo con Azure AD usando la autenticación multifactor, el dispositivo mismo se convertirá en un segundo factor de confianza para ese usuario en particular. Posteriormente, cada vez que el mismo usuario inicia sesión en el dispositivo y accede a una aplicación, Azure AD considera el dispositivo como un segundo factor y permite que el usuario acceda a sus aplicaciones sin problemas, sin necesidad de que MFA le pida que lo haga. Este comportamiento no es aplicable a ningún otro usuario que inicie sesión en ese dispositivo, por lo que a todos los demás usuarios que accedan a ese dispositivo se les solicitará un desafío de MFA antes de acceder a aplicaciones que requieran la autenticación multifactor.

---

**P: Puedo ver el registro del dispositivo en la información del USUARIO en Azure Portal y puedo ver el estado como registrado en el dispositivo. ¿He establecido la configuración correctamente para utilizar el acceso condicional?**

**R:** El estado de unión del dispositivo, especificado en deviceID, debe coincidir con el de Azure AD y satisfacer los criterios de evaluación del acceso condicional. Para más información, vea [Introducción al Registro de dispositivos de Azure Active Directory](active-directory-device-registration.md).

---

**P: ¿Por qué me aparece el mensaje “El nombre de usuario o la contraseña no son correctos” para un dispositivo que ya he unido a Azure AD?**

**R:** Las razones comunes para este escenario son:

- Las credenciales de usuario ya no son válidas.

- El equipo no puede comunicarse con Azure Active Directory. Compruebe si existen errores de conectividad de red.

- No se han cumplido los requisitos previos para Azure AD Join. Asegúrese de que haya seguido los pasos para la [Ampliación de las capacidades de nube a dispositivos de Windows 10 a través de Azure Active Directory Join](active-directory-azureadjoin-overview.md).  

- Los inicios de sesión federados requieren que el servidor de federación admita un punto de conexión activo de WS-Trust. 

- Tiene habilitada la autenticación de paso a través y el usuario tiene una contraseña temporal que debe cambiarse al iniciar sesión.

---

**P: ¿Por qué me aparece un cuadro de diálogo que indica que se ha producido un error cuando intento unir mi equipo a Azure AD?**

**R:** Es el resultado de configurar la inscripción de Azure Active Directory con Intune. Asegúrese de que el usuario que intenta realizar la unión a Azure AD tiene asignada la licencia de Intune correcta. Para más información, vea [Configurar la administración de dispositivos Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**P: ¿Por qué se produce un error al intentar unir mi equipo a pesar de que no he obtenido información sobre el error?**

**R:** Puede deberse a que el usuario ha iniciado sesión en el dispositivo con la cuenta de administrador integrada local. Cree una cuenta local distinta antes de usar Azure Active Directory Join para completar la instalación. 

---

**P: ¿Dónde puedo encontrar instrucciones para la configuración de registro automático de dispositivos?**

**R:** Para ver instrucciones detalladas, consulte [Configuración del registro automático de dispositivos unidos a un dominio de Windows con Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

---

**P: ¿Dónde puedo encontrar información para solucionar problemas con el registro automático de dispositivos?**

**R:** Para consultar información sobre solución de problemas, vea:

- [Solución de problemas de registro automático de equipos unidos a un dominio en Azure AD: Windows 10 y Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Solución de problemas de registro automático de equipos unidos a un dominio en Azure AD para clientes de nivel inferior de Windows](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 

---

