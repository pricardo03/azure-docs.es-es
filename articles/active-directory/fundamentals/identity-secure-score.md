---
title: '¿Qué es la puntuación segura de identidad en Azure AD? : versión preliminar | Microsoft Docs'
description: Aprenda a usar la puntuación segura de identidad para mejorar el nivel de seguridad de un inquilino de Azure AD.
services: active-directory
keywords: puntuación segura de identidad, Azure AD, acceso seguro a los recursos de la empresa
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: fundamentals
ms.topic: overview
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: a9bb22cae2e9fcd607a81d442f4fb51f12c43454
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224144"
---
# <a name="what-is-the-identity-secure-score-in-azure-ad---preview"></a>¿Qué es la puntuación segura de identidad en Azure AD? : versión preliminar

¿Es seguro un inquilino de Azure AD? Si no sabe qué responder a esta pregunta, lea este artículo para obtener información acerca de cómo le ayuda la puntuación segura de identidad a supervisar y mejorar el nivel de seguridad de la identidad. 

## <a name="what-is-an-identity-secure-score"></a>¿Qué es una puntuación segura de identidad?

La puntuación segura de identidad es un número entre 1 y 248 que funciona como indicador del grado de cumplimiento de las recomendaciones de los procedimientos recomendados de Microsoft relativos a la seguridad.


![Puntuación segura](./media/identity-secure-score/01.png)



La puntuación le ayuda a:

- Medir de forma objetiva su nivel de seguridad de la identidad

- Planear la realización de mejoras en la seguridad de la identidad

- Ver si las mejoras han logrado sus objetivos 


Puede acceder tanto a la puntuación como a la información relacionada en el panel de la puntuación segura de identidad. En dicho panel, encontrará:

- Su puntuación

    ![Puntuación segura](./media/identity-secure-score/02.png)

- Un gráfico comparativo

    ![Puntuación segura](./media/identity-secure-score/03.png)

- Un gráfico de tendencias

    ![Puntuación segura](./media/identity-secure-score/04.png)

- Una lista de procedimientos recomendados de seguridad la identidad. 

    ![Puntuación segura](./media/identity-secure-score/05.png)


Si realiza las acciones de mejora, puede:

- Mejorar su nivel de seguridad y su puntuación.
 
- Aprovechar las características de identidad de Microsoft. 



## <a name="how-do-i-get-my-secure-score"></a>¿Cómo se obtiene la puntuación segura?

La puntuación segura de identidad está disponible en todas las ediciones de Azure AD.

Para acceder a su puntuación, vaya a la [panel de información general de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/IdentitySecureScore).



## <a name="how-does-it-work"></a>¿Cómo funciona?

Cada 48 horas, Azure examina la configuración de seguridad y compara los valores con los procedimientos recomendados. En función del resultado de esta evaluación, se calcula una nueva puntuación para el inquilino, lo que significa que pueden pasar hasta 48 horas hasta que un cambio realizado en la configuración se refleje en la puntuación. 

Cada recomendación se mide según la configuración de Azure AD. Si usa productos de terceros para habilitar un procedimiento recomendado, puede indicarlo en la configuración de una acción de mejora.

![Puntuación segura](./media/identity-secure-score/07.png)


Además, también tiene la opción de establecer que se ignoren las recomendaciones si no se aplican a su entorno. Las recomendaciones ignoradas no contribuyen al cálculo de la puntuación. 
 
![Puntuación segura](./media/identity-secure-score/06.png)



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

### <a name="what-does-not-scored-mean"></a>¿Qué significa [Not Scored]?

Las acciones con la etiqueta [Not Scored] (Sin puntuación) son las que puede realizar en su organización, pero que no se puntúan porque no están conectadas en la herramienta (aún). Por consiguiente, puede mejorar aún más la seguridad, pero no obtendrá ninguna puntuación por esas acciones ahora mismo.

### <a name="how-often-is-my-score-updated"></a>¿Con qué frecuencia se actualiza la puntuación?

La puntuación se calcula una vez al día (aproximadamente a las 1:00 A.M., hora del Pacífico). Si realiza algún cambio en una acción medida, la puntuación se actualizará automáticamente al día siguiente. Cualquier cambio tarda hasta 48 horas en reflejarse en la puntuación.


### <a name="my-score-changed-how-do-i-figure-out-why"></a>Mi puntuación ha cambiado. ¿Cómo averiguo por qué?

En la página del analizador de puntuación del [portal de puntuación segura](https://securescore.microsoft.com/#!/score), haga clic en un punto de datos de un día determinado y desplácese hacia abajo para ver las acciones completadas e incompletas de ese día para averiguar lo que ha cambiado.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>¿Mide la puntuación segura el riesgo de vulneración de la seguridad?

En pocas palabras, no. La puntuación segura no expresa una medida absoluta de la probabilidad de sufrir una vulneración de la seguridad. Expresa en qué medida ha adoptado características que pueden compensar el riesgo de vulneración de la seguridad. Ningún servicio puede garantizar que no va a sufrir una vulneración de la seguridad y la puntuación segura no se debe interpretar de ningún modo como una garantía.

### <a name="how-should-i-interpret-my-score"></a>¿Cómo debo interpretar la puntuación?

Se le dan puntos por configurar las características de seguridad recomendadas o por realizar tareas relacionadas con la seguridad (como la lectura de informes). Se puntúan algunas acciones por la finalización parcial, como por ejemplo, habilitar la autenticación multifactor (MFA) para los usuarios. Su puntuación segura representa directamente los servicios de seguridad de Microsoft que usa. Recuerde que siempre debe existir un equilibrio entre seguridad y facilidad de uso. Todos los controles de seguridad tienen un componente de impacto en el usuario. Los controles que tienen un impacto bajo en el usuario deben tener poco o ningún efecto en las operaciones diarias de los usuarios.

Para ver el historial de puntuación, vaya a la página del analizador de puntuación del [portal de puntuación segura](https://securescore.microsoft.com/#!/score). Elija una fecha concreta para ver los controles que se han habilitado para ese día y los puntos que ha logrado por cada uno de ellos.


### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>¿Cómo se relaciona la puntuación segura de identidad con la puntuación segura de Office 365? 

La [puntuación segura de Office 365](https://docs.microsoft.com/office365/securitycompliance/office-365-secure-score) está a punto de migrarse a un agregado de cinco puntuaciones distintas:

- Identidad

- Datos

- Dispositivos

- Infraestructura

- Aplicaciones

La puntuación segura de identidad representa la parte de la identidad de la puntuación segura de Office 365, lo que significa que las recomendaciones de la puntuación segura de identidad y las de Office 365 son idénticas. 


## <a name="next-steps"></a>Pasos siguientes

Si desea ver un vídeo acerca de la puntuación segura de Office 365, haga clic [aquí](https://www.youtube.com/watch?v=jzfpDJ9Kg-A).
 
