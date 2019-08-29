---
title: 'Acceso condicional: Requerir dispositivos compatibles (Azure Active Directory)'
description: Cree una directiva de acceso condicional personalizada para exigir el cumplimiento de los dispositivos.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: da1a5f54e5e989f661770d518a6753b831b59bd4
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69990514"
---
# <a name="conditional-access-require-compliant-devices"></a>Acceso condicional: Requerir dispositivos compatibles

Las organizaciones que han implementado Microsoft Intune pueden usar la información que devuelven sus dispositivos para identificar aquellos que satisfacen los requisitos de cumplimiento, por ejemplo:

* Exigir un PIN para desbloquearlos
* Exigir el cifrado del dispositivo
* Exigir una versión mínima o máxima del sistema operativo
* Exigir que un dispositivo no se haya liberado ni modificado

Esta información del cumplimiento de las directivas se reenvía a Azure AD donde se pueden tomar decisiones mediante el acceso condicional para conceder o bloquear el acceso a los recursos.

## <a name="create-a-conditional-access-policy"></a>Creación de una directiva de acceso condicional

Los pasos siguientes le ayudarán a crear una directiva de acceso condicional para exigir que los dispositivos que acceden a los recursos se marquen como compatibles con las directivas de cumplimiento de Intune de su organización.

1. Inicie sesión en  **Azure Portal**  como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Todos los usuarios**.
   1. En **Excluir**, seleccione **Usuarios y grupos** y, luego, elija las cuentas de acceso de emergencia de la organización. 
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Todas las aplicaciones en la nube**.
   1. Si debe excluir aplicaciones específicas de la directiva, puede seleccionarlas en la pestaña **Excluir** en **Seleccionar las aplicaciones en la nube excluidas** y elegir **Seleccionar**.
   1. Seleccione **Listo**.
1. En **Controles de acceso** > **Conceder**, seleccione **Requerir que el dispositivo esté marcado como compatible**.
   1. Elija **Seleccionar**.
1. Confirme la configuración y establezca **Habilitar directiva** en **Activado**.
1. Seleccione **Crear** para crear la directiva.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

[Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional](troubleshoot-conditional-access-what-if.md)

[Las directivas de cumplimiento de dispositivos funcionan con Azure AD](https://docs.microsoft.com/intune/device-compliance-get-started#device-compliance-policies-work-with-azure-ad)
