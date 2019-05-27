---
title: ¿Qué es la puntuación segura de identidad? - Azure Active Directory
description: ¿Cómo puede usar la puntuación segura de identidad para mejorar la postura de seguridad de su directorio
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: tilarso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cdff2305914ca6e4144f7784d1a60026a1d27c0
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65988672"
---
# <a name="what-is-the-identity-secure-score-in-azure-active-directory"></a>¿Qué es la puntuación segura de identidad en Azure Active Directory?

¿Es seguro un inquilino de Azure AD? Si no sabe cómo responder a esta pregunta, en este artículo se explica cómo la puntuación segura de identidad le ayuda a supervisar y mejorar su postura de seguridad de identidad.

## <a name="what-is-an-identity-secure-score"></a>¿Qué es una puntuación segura de identidad?

La puntuación segura de identidad es el número entre 1 y 223 que funciona como un indicador de grado de realización son con las prácticas recomendadas para la seguridad de Microsoft. Cada acción de mejora de la puntuación segura de identidad se adapta a su configuración específica.  

![Puntuación segura](./media/identity-secure-score/identity-secure-score-overview.png)

La puntuación le ayuda a:

- Medir de forma objetiva su nivel de seguridad de la identidad
- Planear la realización de mejoras en la seguridad de la identidad
- Ver si las mejoras han logrado sus objetivos

Puede acceder tanto a la puntuación como a la información relacionada en el panel de la puntuación segura de identidad. En dicho panel, encontrará:

- La puntuación segura de identidad
- Un gráfico de comparación que muestra cómo se compara la puntuación segura de identidad a otros inquilinos en el mismo sector y tamaño similar
- Un gráfico de tendencias que muestra cómo ha cambiado su puntuación segura de identidad con el tiempo
- Una lista de posibles mejoras

Si realiza las acciones de mejora, puede:

- Mejorar su postura de seguridad y su puntuación
- Aprovechar las características disponibles para su organización como parte de sus inversiones en identidad

## <a name="how-do-i-get-my-secure-score"></a>¿Cómo se obtiene la puntuación segura?

La puntuación segura de identidad está disponible en todas las ediciones de Azure AD. Para acceder a su puntuación, vaya a la [panel de información general de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/IdentitySecureScore).

## <a name="how-does-it-work"></a>¿Cómo funciona?

Cada 48 horas, Azure examina la configuración de seguridad y compara los valores con los procedimientos recomendados. En función del resultado de esta evaluación, se calcula una nueva puntuación para su directorio. Es posible que la configuración de seguridad no está completamente alineada con la Guía de procedimientos recomendados y las acciones de mejora solo parcialmente se cumplen. En estos casos, se solo se entregarán una parte de la puntuación máxima disponible para el control.

Cada recomendación se mide según la configuración de Azure AD. Si usa productos de terceros para habilitar una práctica recomendada, puede indicar esta configuración en la configuración de una acción de mejora. También tiene la opción para establecer las recomendaciones se omite si no se aplican a su entorno. Las recomendaciones ignoradas no contribuyen al cálculo de la puntuación.

![Pasar por alto o marcará la acción tal como se explicó por terceros](./media/identity-secure-score/identity-secure-score-ignore-or-third-party-reccomendations.png)

## <a name="how-does-it-help-me"></a>¿Cómo me ayuda?

La puntuación segura le ayuda a:

- Medir de forma objetiva su nivel de seguridad de la identidad
- Planear la realización de mejoras en la seguridad de la identidad
- Ver si las mejoras han logrado sus objetivos

## <a name="what-you-should-know"></a>Qué debería saber

### <a name="who-can-use-the-identity-secure-score"></a>¿Quién puede usar la puntuación segura de identidad?

La puntuación segura de identidad pueden usarla los roles siguientes:

- Administrador global
- Administrador de seguridad
- Lectores de seguridad

### <a name="how-are-controls-scored"></a>¿Cómo se puntúan controles?

Pueden puntuar los controles de dos maneras. Algunos se puntúan en forma binaria: obtener el 100% de la puntuación si tiene la característica o valor configurado según nuestra recomendación. Otras puntuaciones se calculan como un porcentaje de la configuración del total. Por ejemplo, si la recomendación mejora indica obtendrá 30 puntos si protege todos los usuarios con MFA y solo tiene 5 de 100 usuarios totales protegidos, se podría ofrecerá una calificación parcial alrededor de 2 puntos (5 protegidos / total de 100 * pts máximo 30 = 2 calificación parcial de pts) .

### <a name="what-does-not-scored-mean"></a>¿Qué significa [Not Scored]?

Las acciones con la etiqueta [Not Scored] (Sin puntuación) son las que puede realizar en su organización, pero que no se puntúan porque no están conectadas en la herramienta (aún). Por consiguiente, puede mejorar aún más la seguridad, pero no obtendrá ninguna puntuación por esas acciones ahora mismo.

### <a name="how-often-is-my-score-updated"></a>¿Con qué frecuencia se actualiza la puntuación?

La puntuación se calcula una vez al día (aproximadamente a las 1:00 A.M., hora del Pacífico). Si realiza algún cambio en una acción medida, la puntuación se actualizará automáticamente al día siguiente. Cualquier cambio tarda hasta 48 horas en reflejarse en la puntuación.

### <a name="my-score-changed-how-do-i-figure-out-why"></a>Mi puntuación ha cambiado. ¿Cómo averiguo por qué?

Vaya a la [centro de seguridad de Microsoft 365](https://security.microsoft.com/), donde encontrará su puntuación segura de Microsoft completado. Puede ver fácilmente todos los cambios a la puntuación segura mediante la revisión de los cambios en la pestaña historial detallados.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>¿La puntuación segura de medir mi riesgo de obtener infringido?

En pocas palabras, no. La puntuación segura no expresa una medida absoluta de probabilidad de que van a obtener infringido. Expresa en qué medida ha adoptado características que pueden compensar el riesgo de vulneración de la seguridad. Ningún servicio puede garantizar que no le superarse y la puntuación segura no debe interpretarse como una garantía de ningún modo.

### <a name="how-should-i-interpret-my-score"></a>¿Cómo debo interpretar la puntuación?

Se le dan puntos por configurar las características de seguridad recomendadas o por realizar tareas relacionadas con la seguridad (como la lectura de informes). Se puntúan algunas acciones por la finalización parcial, como por ejemplo, habilitar la autenticación multifactor (MFA) para los usuarios. La puntuación segura directamente es representativo de los servicios de seguridad de Microsoft que usa. Recuerde que la seguridad debe equilibrarse con facilidad de uso. Todos los controles de seguridad tienen un componente de impacto en el usuario. Los controles que tienen un impacto bajo en el usuario deben tener poco o ningún efecto en las operaciones diarias de los usuarios.

Para ver el historial de puntuación, diríjase a la [centro de seguridad de Microsoft 365](https://security.microsoft.com/) y revise su puntuación segura de Microsoft. Puede revisar los cambios de su generales puntuación segura se haga clic en ver el historial. Elija una fecha concreta para ver los controles que se han habilitado para ese día y los puntos que ha logrado por cada uno de ellos.

### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>¿Cómo se relaciona la puntuación segura de identidad con la puntuación segura de Office 365?

El [puntuación segura de Microsoft](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score) contiene cinco categorías de puntuación y control distinto:

- Identidad
- Datos
- Dispositivos
- Infraestructura
- Aplicaciones

La puntuación segura de identidad representa la parte de la identidad de la puntuación segura de Microsoft. Esta superposición significa que las recomendaciones para la identidad segura puntuación y la puntuación de identidad de Microsoft es los mismos.

## <a name="next-steps"></a>Pasos siguientes

[Encontrará más información sobre la puntuación segura de Microsoft](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score)
