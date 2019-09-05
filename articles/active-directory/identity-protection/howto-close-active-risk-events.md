---
title: Cierre de detecciones de riesgo activas en Azure Active Directory Identity Protection | Microsoft Docs
description: Obtenga información sobre las opciones que tiene para cerrar detecciones de riesgo activas.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d24ec94d0381fc2e79fdef97b6d525b7cec33fef
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126472"
---
# <a name="how-to-close-active-risk-detections"></a>Instrucciones: Cierre de detecciones de riesgo activas

Con las [detecciones de riesgo](../reports-monitoring/concept-risk-events.md), Azure Active Directory detecta indicadores para las cuentas de usuario en peligro potencial. Como administrador, querrá tener todas las detecciones de riesgo cerradas para que los usuarios afectados ya no estén en riesgo.

En este artículo se proporciona información general de las opciones adicionales que tiene para cerrar las detecciones de riesgo activas.

## <a name="options-to-close-risk-detections"></a>Opciones para cerrar las detecciones de riesgo 

El estado de una detección de riesgo es **activa** o **cerrada**. Todas las detecciones de riesgo activas contribuyen al cálculo de un valor llamado nivel de riesgo del usuario. El nivel de riesgo del usuario es un indicador (bajo, medio, alto) de la probabilidad de que se haya puesto en peligro una cuenta. 

Para cerrar las detecciones de riesgo activas, tiene las siguientes opciones:

- Solicitud del restablecimiento de contraseña con una directiva de riesgo de usuarios
- Restablecimiento manual de contraseña
- Descarte de todas las detecciones de riesgo 
- Cierre manual de las detecciones de riesgo individuales

## <a name="require-password-reset-with-a-user-risk-policy"></a>Solicitud del restablecimiento de contraseña con una directiva de riesgo de usuarios

Con la configuración de la [directiva de acceso condicional de riesgo de usuarios](howto-user-risk-policy.md), puede requerir un cambio de contraseña si se ha detectado automáticamente un nivel de riesgo del usuario especificado. 

![Restablecimiento de contraseña](./media/howto-close-active-risk-events/13.png)

Un restablecimiento de contraseña cierra todos los eventos de riesgos activos del usuario relacionado y retorna la identidad a un estado seguro. El uso de una directiva de riesgo del usuario es el método preferido para cerrar las detecciones de riesgo activas, ya que este método está automatizado. No es necesaria ninguna interacción entre el usuario afectado y el departamento de soporte técnico o un administrador.

Sin embargo, el uso de una directiva de riesgo del usuario no siempre es aplicable. Esto se aplica, por ejemplo, a:

- Usuarios que no se han registrado para la autenticación multifactor (MFA).
- Usuarios con detecciones de riesgo activas que se han eliminado.
- Una investigación que revela que el usuario legítimo ha realizado una detección de riesgo notificada.

## <a name="manual-password-reset"></a>Restablecimiento manual de contraseña

Si el requisito de un restablecimiento de contraseña mediante una directiva de riesgo del usuario no es una opción, puede cerrar todas las detecciones de riesgo de un usuario con un restablecimiento de contraseña manual.

![Restablecimiento de contraseña](./media/howto-close-active-risk-events/04.png)

El cuadro de diálogo relacionado proporciona dos métodos diferentes de restablecer una contraseña:

![Restablecimiento de contraseña](./media/howto-close-active-risk-events/05.png)

**Generar una contraseña temporal**: mediante la generación de una contraseña temporal, puede retornar inmediatamente una identidad a un estado seguro. Este método requiere interacción con los usuarios afectados porque tienen que saber cuál es la contraseña temporal. Por ejemplo, puede enviar la nueva contraseña temporal a una dirección de correo electrónico alternativa del usuario o bien al responsable del usuario. Dado que la contraseña es temporal, se le pedirá al usuario que cambie la contraseña en el inicio de sesión siguiente.

**Requerir que el usuario restablezca la contraseña**: requerir que los usuarios restablezcan las contraseñas permite la propia recuperación sin ponerse en contacto con el departamento de soporte técnico o un administrador. Al igual que en el caso de una directiva de riesgo del usuario, este método solo se aplica a los usuarios que están registrados en MFA. Para los usuarios que aún no se han registrado en MFA, esta opción no está disponible.

## <a name="dismiss-all-risk-detections"></a>Descarte de todas las detecciones de riesgo

Si un restablecimiento de contraseña no es una buena opción, también puede descartar todas las detecciones de riesgo. 

![Restablecimiento de contraseña](./media/howto-close-active-risk-events/03.png)

Al hacer clic en **Dismiss all events**(Descartar todos los eventos), se cierran todos los eventos y el usuario afectado deja de correr riesgo. Sin embargo, dado que este método no tiene impacto en la contraseña existente, no retorna la identidad relacionada a un estado seguro. El caso de uso preferido para este método es un usuario eliminado con detecciones de riesgo activas. 

## <a name="close-individual-risk-detections-manually"></a>Cierre manual de las detecciones de riesgo individuales

Puede cerrar de forma manual las detecciones de riesgo individuales. Al cerrar manualmente las detecciones de riesgo, puede reducir el nivel de riesgo del usuario. Normalmente, las detecciones de riesgo se cierran manualmente en respuesta a una investigación relacionada. Por ejemplo, cuando al hablar con un usuario se revela que una detección de riesgo activa ya no es necesaria. 
 
Al cerrar manualmente las detecciones de riesgo, puede elegir realizar cualquiera de las siguientes acciones para cambiar el estado de una detección de riesgo:

![Acciones](./media/howto-close-active-risk-events/06.png)

- **Resolver**: si, después de investigar una detección de riesgo, ha tomado una acción de corrección adecuada fuera de Identity Protection y cree que se debe considerar cerrada la detección de riesgo, marque el evento como resuelto. Los eventos resueltos establecerán el estado de la detección de riesgo en Cerrada y ya no contribuirá al riesgo del usuario.
- **Marcar como falso positivo**: en algunos casos, puede investigar una detección de riesgo y descubrir que se ha marcado incorrectamente como un peligrosa. Para ayudar a reducir el número de dichos casos, puede marcar la detección de riesgo como falso positivo. Esto ayudará a los algoritmos de aprendizaje automático a mejorar la clasificación de eventos similares en el futuro. El estado de los eventos con falsos positivos es Cerrado y ya no contribuyen al riesgo del usuario.
- **Omitir**: si no se ha tomado ninguna acción de corrección, pero quiere que la detección de riesgo se quite de la lista activa, puede seleccionar Omitir para una detección de riesgo, cuyo estado será Cerrada. Los eventos omitidos no contribuyen al riesgo del usuario. Esta opción solo debe utilizarse en circunstancias inusuales.
- **Reactivar**: las detecciones de riesgo que se cierran manualmente (al elegir Resolver, Falso positivo u Omitir) se pueden reactivar, lo que vuelve a establecer el estado del evento en Activo. Las detecciones de riesgo reactivadas contribuyen al cálculo del nivel de riesgo del usuario. Las detecciones de riesgo cerradas mediante una corrección (como el restablecimiento de una contraseña segura) no se pueden reactivar.

## <a name="next-steps"></a>Pasos siguientes

Para obtener una visión general de Azure AD Identity Protection, consulte la [Introducción a Azure AD Identity Protection](overview.md).
