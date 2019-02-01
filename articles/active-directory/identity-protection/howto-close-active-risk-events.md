---
title: Procedimiento de cierre de eventos de riesgo activos en Azure Active Directory Identity Protection | Microsoft Docs
description: Obtenga información acerca de las opciones que tiene para cerrar eventos de riesgo activos.
services: active-directory
keywords: azure active directory identity protection, detección de aplicaciones en la nube, administración de aplicaciones, seguridad, riesgo, nivel de riesgo, punto vulnerable, directiva de seguridad
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 222f17541de902ee2180b66563857ccd2193ba54
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178314"
---
# <a name="how-to-close-active-risk-events"></a>Instrucciones: Cierre de eventos de riesgo activos

Con los [eventos de riesgo](../reports-monitoring/concept-risk-events.md), Azure Active Directory detecta indicadores para las cuentas de usuario en peligro potencial. Como administrador, deseará tener todos los eventos de riesgo cerrados para que los usuarios afectados ya no estén en riesgo.

En este artículo se proporciona información general de las opciones adicionales que tiene para cerrar los eventos de riesgo activos.

## <a name="options-to-close-risk-events"></a>Opciones para cerrar los eventos de riesgo 

El estado de un evento de riesgo es **activo** o **cerrado**. Todos los eventos de riesgo activos contribuyen al cálculo de un valor llamado nivel de riesgo del usuario. El nivel de riesgo del usuario es un indicador (bajo, medio, alto) de la probabilidad de que se haya puesto en peligro una cuenta. 

Para cerrar los eventos de riesgo activos, tiene las siguientes opciones:

- Solicitud del restablecimiento de contraseña con una directiva de riesgo de usuarios

- Restablecimiento manual de contraseña
 
- Descarte de todos los eventos de riesgo 

- Cierre manual de los eventos de riesgo individuales



## <a name="require-password-reset-with-a-user-risk-policy"></a>Solicitud del restablecimiento de contraseña con una directiva de riesgo de usuarios

Con la configuración de la [directiva de acceso condicional de riesgo de usuarios](howto-user-risk-policy.md), puede requerir un cambio de contraseña si se ha detectado automáticamente un nivel de riesgo del usuario especificado. 

![Restablecimiento de contraseña](./media/howto-close-active-risk-events/13.png)

Un restablecimiento de contraseña cierra todos los eventos de riesgos activos del usuario relacionado y retorna la identidad a un estado seguro. El uso de una directiva de riesgo del usuario es el método preferido para cerrar los eventos de riesgo activos porque este método es automático. No es necesaria ninguna interacción entre el usuario afectado y el departamento de soporte técnico o un administrador.

Sin embargo, el uso de una directiva de riesgo del usuario no siempre es aplicable. Esto se aplica, por ejemplo, a:

- Usuarios que no se han registrado para la autenticación multifactor (MFA).
- Usuarios con eventos de riesgo activos que se han eliminado.
- Una investigación que revela que el usuario legítimo ha realizado un evento de riesgo notificado.


## <a name="manual-password-reset"></a>Restablecimiento manual de contraseña

Si requerir un restablecimiento de contraseña mediante una directiva de riesgo del usuario no es una opción, puede cerrar todos los eventos de riesgo de un usuario con un restablecimiento de contraseña manual.

![Restablecimiento de contraseña](./media/howto-close-active-risk-events/04.png)


El cuadro de diálogo relacionado proporciona dos métodos diferentes de restablecer una contraseña:

![Restablecimiento de contraseña](./media/howto-close-active-risk-events/05.png)


**Generar una contraseña temporal**: mediante la generación de una contraseña temporal, puede retornar inmediatamente una identidad a un estado seguro. Este método requiere interacción con los usuarios afectados porque tienen que saber cuál es la contraseña temporal. Por ejemplo, puede enviar la nueva contraseña temporal a una dirección de correo electrónico alternativa del usuario o bien al responsable del usuario. Dado que la contraseña es temporal, se le pedirá al usuario que cambie la contraseña en el inicio de sesión siguiente.


**Requerir que el usuario restablezca la contraseña**: requerir que los usuarios restablezcan las contraseñas permite la propia recuperación sin ponerse en contacto con el departamento de soporte técnico o un administrador. Al igual que en el caso de una directiva de riesgo del usuario, este método solo se aplica a los usuarios que están registrados en MFA. Para los usuarios que aún no se han registrado en MFA, esta opción no está disponible.


## <a name="dismiss-all-risk-events"></a>Descarte de todos los eventos de riesgo

Si un restablecimiento de contraseña no es una buena opción, también puede descartar todos los eventos de riesgo. 

![Restablecimiento de contraseña](./media/howto-close-active-risk-events/03.png)

Al hacer clic en **Dismiss all events**(Descartar todos los eventos), se cierran todos los eventos y el usuario afectado deja de correr riesgo. Sin embargo, dado que este método no tiene impacto en la contraseña existente, no retorna la identidad relacionada a un estado seguro. El caso de uso preferido para este método es un usuario eliminado con eventos de riesgo activos. 


## <a name="close-individual-risk-events-manually"></a>Cierre manual de los eventos de riesgo individuales

Puede cerrar manualmente eventos de riesgo individuales. Al cerrar manualmente los eventos de riesgo, puede reducir el nivel de riesgo del usuario. Normalmente, los eventos de riesgo se cierran manualmente en respuesta a una investigación relacionada. Por ejemplo, cuando al hablar con un usuario se muestra que un evento de riesgo activo ya no es necesario. 
 
Al cerrar manualmente los eventos de riesgo, puede elegir realizar cualquiera de las siguientes acciones para cambiar el estado de un evento de riesgo:

![Acciones](./media/howto-close-active-risk-events/06.png)

- **Resolver** : si, después de investigar un evento de riesgo, ha tomado una acción de corrección adecuada fuera de Identity Protection y cree que se debe considerar el evento de riesgo cerrado, marque el evento como resuelto. Los eventos resueltos establecerán el estado del evento de riesgo en Cerrado y ya no contribuirá al riesgo del usuario.

- **Marcar como falso positivo** : en algunos casos, puede investigar un evento de riesgo y detectar que se ha marcado incorrectamente como peligroso. Para ayudar a reducir el número de dichos casos, puede marcar el evento de riesgo como falso positivo. Esto ayudará a los algoritmos de aprendizaje automático a mejorar la clasificación de eventos similares en el futuro. El estado de los eventos con falsos positivos es Cerrado y ya no contribuyen al riesgo del usuario.

- **Omitir** : si no se ha tomado ninguna acción de corrección, pero desea que el evento de riesgo se quite de la lista activa, puede seleccionar Omitir para un evento de riesgo, cuyo estado será Cerrado. Los eventos omitidos no contribuyen al riesgo del usuario. Esta opción solo debe utilizarse en circunstancias inusuales.

- **Reactivar**: los eventos de riesgo que se cierran manualmente (al elegir Resolver, Falso positivo u Omitir) se pueden reactivar, lo que vuelve a establecer el estado del evento en Activo. Los eventos de riesgo reactivados contribuyen al cálculo del nivel de riesgo del usuario. Los eventos de riesgo cerrados mediante una corrección (como el restablecimiento de una contraseña segura) no se pueden reactivar.
  

## <a name="next-steps"></a>Pasos siguientes

Para obtener una visión general de Azure AD Identity Protection, consulte la [Introducción a Azure AD Identity Protection](overview.md).
