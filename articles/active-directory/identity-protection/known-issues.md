---
title: Preguntas frecuentes y problemas conocidos con la versión actualizada de Identity Protection de Azure Active Directory | Microsoft Docs
description: Preguntas frecuentes y problemas conocidos con la versión actualizada de Identity Protection de Azure Active Directory.
services: active-directory
keywords: azure active directory identity protection, detección de aplicaciones en la nube, administración de aplicaciones, seguridad, riesgo, nivel de riesgo, punto vulnerable, directiva de seguridad
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2019
ms.author: markvi
ms.reviewer: raluthra
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1cc8a9305103a30fc0ea0fc40c23a522e136d80
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807191"
---
# <a name="faqs-and-known-issues-with-identity-protection-refreshed-in-azure-active-directory"></a>Preguntas frecuentes y problemas conocidos con la versión actualizada de Identity Protection de Azure Active Directory


## <a name="dismiss-user-risk-known-issues"></a>Descartar problemas conocidos de riesgo de usuario

**Descartar el riesgo del usuario** en la versión clásica de Identity Protection establece el actor del historial de riesgo del usuario de la versión actualizada de Identity Protection en **Azure AD**.


**Descartar el riesgo del usuario** en la versión actualizada de Identity Protection establece el actor del historial de riesgo del usuario de dicha versión de Identity Protection en **\<Admin’s name with a hyperlink pointing to user’s blade\>** (Nombre del administrador con un hipervínculo que apunta a la hoja del usuario).

Actualmente hay un problema conocido que provoca latencias en el flujo de descarte de riesgo de usuario. Si tiene una "directiva de riesgo de usuario", esta directiva dejará de aplicarse a los usuarios descartados a los pocos minutos de hacer clic en "Descartar el riesgo de usuario". Sin embargo, se han constatado retrasos en la experiencia de usuario al actualizar el "Estado de riesgo" de los usuarios descartados. Como alternativa, actualice la página a nivel del explorador para ver el "Estado de riesgo" más reciente del usuario.


## <a name="risky-users-report-known-issues"></a>Problemas conocidos de informes de usuarios de riesgo

Las consultas en el campo **Nombre de usuario** distinguen mayúsculas de minúsculas, mientras que las consultas en el campo **Nombre** no lo hacen.

Si activa o desactiva **Show dates as (Mostrar fechas como)** se ocultará o mostrará la columna **RISK LAST UPDATED** (ÚLTIMA ACTUALIZACIÓN DE RIESGO). Para volver a agregar la columna, haga clic en **Columnas** en la parte superior de la hoja Usuarios de riesgo.

**Descartar todos los eventos** en la versión clásica de Identity Protection establece el estado de los eventos de riesgo en **Closed (resolved)** [Cerrado (resuelto)].


## <a name="risky-sign-ins-report-known-issues"></a>Problemas conocidos de informe de inicios de sesión

**Resolver** en un evento de riesgo establece el estado en **Users passed MFA driven by risk-based policy** (Los usuarios superaron el proceso de MFA según una directiva basada en riesgo).


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-event"></a>¿Por qué no puedo establecer mis propios niveles de riesgo para cada evento de riesgo?

Los niveles de riesgo de Identity Protection se basan en la precisión de la detección y en la tecnología de aprendizaje automático supervisado. Para personalizar qué usuarios de la experiencia se presentan, el administrador puede incluir o excluir ciertos usuarios o grupos de las directivas Riesgo de usuario y Riesgo de inicio de sesión.


### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>¿Por qué la ubicación de inicio de sesión no coincide con el lugar desde donde el usuario inició sesión realmente?

La asignación de geolocalización de direcciones IP es un desafío que afecta a todo el sector. Si cree que la ubicación indicada en el informe de inicios de sesión no coincide con la ubicación real, póngase en contacto con soporte técnico. 


### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>¿Cómo funcionan los mecanismos de comentarios en Identity Protection?

**Confirmado (en peligro)** (en un inicio de sesión): informa a Azure AD Identity Protection de que el inicio de sesión no lo realizó el propietario de la identidad e indica un peligro.

- Al recibir estos comentarios, pasamos el estado de riesgo del inicio de sesión y del usuario a **Confirmado (en peligro)** y nivel de riesgo **Alto**.

- Además, se proporcionará la información a los sistemas de aprendizaje automático para futuras mejoras en la evaluación de riesgos.

    > [!NOTE]
    > Si el usuario ya se ha corregido, no haga clic en **Confirmar que se encuentra en peligro**, ya que pasará el estado de riesgo del inicio de sesión y del usuario a **Confirmado (en peligro)** y el nivel de riesgo a **Alto**.

**Confirmar que es seguro** (en un inicio de sesión): informa a Azure AD Identity Protection de que el inicio de sesión lo realizó el propietario de la identidad y no indica un peligro.

- Al recibir estos comentarios, pasamos el estado de riesgo del inicio de sesión (y no al usuario) a **Confirmado (en peligro)** y el nivel de riesgo a **-**.

- Además, se proporcionará la información a los sistemas de aprendizaje automático para futuras mejoras en la evaluación de riesgos.

    > [!NOTE]
    > Si cree que el usuario no está en peligro, use **Descartar el riesgo del usuario** en el nivel de usuario en lugar de usar **Confirmado (seguro)** en el nivel de inicio de sesión. **Descartar el riesgo del usuario** en el nivel de usuario cierra el riesgo del usuario, así como todos los inicios de sesión y los eventos de riesgo pasados.



### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-events-are-shown-in-identity-protection"></a>¿Por qué veo un usuario con una puntuación de riesgo baja (o superior) a pesar de no hay inicios de sesión ni eventos de riesgo en Identity Protection?

Dado que el riesgo del usuario es de naturaleza acumulativa y no expira, un usuario puede tener un riesgo de usuario bajo o superior, incluso si no hay eventos de riesgo o inicios de sesión de riesgo que se muestren en Identity Protection. Esto podría suceder si la actividad malintencionada en un usuario tuvo lugar fuera del período de tiempo durante el cual almacenamos los detalles de los inicios de sesión y de los eventos de riesgo. No permitimos que el riesgo del usuario expire porque se sabe que los malos actores permanecen en el entorno de los clientes más de 140 días detrás de una identidad comprometida antes de impulsar su ataque. Los clientes pueden revisar la escala de tiempo de riesgo del usuario, para entender porqué un usuario está en peligro, si se dirigen a: `Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>¿Por qué un inicio de sesión tiene una puntuación alta en "sign-in risk (aggregate)" [Riesgo de inicio de sesión (agregado)] cuando las detecciones asociadas con él son de riesgo medio o bajo?

La puntuación agregada de riesgo alto podría basarse en otras características del inicio de sesión, o en el hecho de que se desencadenó más de una detección para ese inicio de sesión. Y a la inversa, un inicio de sesión puede tener un riesgo de inicio de sesión (agregado) de nivel Medio, incluso si las detecciones asociadas con el inicio de sesión son de riesgo alto. 
