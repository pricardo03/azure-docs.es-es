---
title: 'Acceso condicional: información de seguridad combinada (Azure Active Directory)'
description: Cree una directiva de acceso condicional personalizada para exigir una ubicación de confianza para el registro de la información de seguridad.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 866b682c9e1ebcb1b3458d26c638237905d06b05
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74889741"
---
# <a name="conditional-access-require-trusted-location-for-mfa-registration"></a>Acceso condicional: Requerir ubicaciones de confianza para el registro de autenticación multifactor

Proteger cuándo y cómo se registran los usuarios para Azure Multi-Factor Authentication y el restablecimiento de contraseñas de autoservicio ya es posible con las acciones del usuario en la directiva de acceso condicional. Esta característica de versión preliminar está disponible para organizaciones que han habilitado la [vista previa del registro combinado](../authentication/concept-registration-mfa-sspr-combined.md). Esta funcionalidad se puede habilitar en las organizaciones que quieren usar condiciones como la ubicación de red de confianza para restringir el acceso al registro en Azure Multi-Factor Authentication y SSPR. Para obtener más información sobre la creación de ubicaciones de confianza en el acceso condicional, consulte el artículo [What is the location condition in Azure Active Directory Conditional Access?](../conditional-access/location-condition.md#named-locations) (¿Qué es la condición de ubicación del acceso condicional de Azure Active Directory?)

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Crear una directiva para requerir el registro desde una ubicación de confianza

La siguiente directiva se aplica a todos los usuarios seleccionados, que intentan registrarse con la experiencia de registro combinado, y bloquea su acceso a menos que se conecten desde una ubicación marcada como red de confianza.

1. En **Azure Portal**, vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. En Nombre, escriba un nombre para la directiva. Por ejemplo, **Registro de información de seguridad combinada en redes de confianza**.
1. En **Asignaciones**, haga clic en **Usuarios y grupos** y seleccione los usuarios y grupos a los que quiera aplicar esta directiva.

   > [!WARNING]
   > Los usuarios deben estar habilitados para la [versión preliminar del registro combinado](../authentication/howto-registration-mfa-sspr-combined.md).

1. En **Aplicaciones en la nube o acciones**, seleccione **Acciones del usuario** y active la casilla **Registrar la información de seguridad (versión preliminar)** .
1. En **Condiciones** > **Ubicaciones**.
   1. Configure **Sí**.
   1. Incluya **Cualquier ubicación**.
   1. Excluya **Todas las ubicaciones de confianza**.
   1. Haga clic en **Listo** en la hoja de ubicaciones.
   1. Haga clic en **Listo** en la hoja de condiciones.
1. En **Controles de acceso** > **Conceder**.
   1. Haga clic en **Bloquear acceso**.
   1. Después, haga clic en **Seleccionar**.
1. Establezca **Habilitar directiva** en **Activado**.
1. A continuación, haga clic en **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

[Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional](troubleshoot-conditional-access-what-if.md)
