---
title: ¿Qué es Azure Active Directory Identity Protection (actualizado)? | Microsoft Docs
description: ¿Qué es Azure Active Directory Identity Protection (actualizado)?
services: active-directory
keywords: azure active directory identity protection, detección de aplicaciones en la nube, administración de aplicaciones, seguridad, riesgo, nivel de riesgo, punto vulnerable, directiva de seguridad
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 37c63e32f1ee9c404e8b84a6eb17bc6eec30a761
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956935"
---
# <a name="what-is-azure-active-directory-identity-protection-refreshed"></a>¿Qué es Azure Active Directory Identity Protection (actualizado)?

Se ha actualizado la experiencia de Identity Protection para proteger mejor las identidades de la organización. Esta experiencia actualizada proporciona lo siguiente:

- Una experiencia rediseñada de administración que se centra en los riesgos más relevantes para el usuario: riesgo de inicio de sesión y riesgo de usuario.

- Una experiencia eficaz de investigaciones con soporte técnico para el filtrado, la ordenación y las descargas inteligentes.

- Cálculo del riesgo de usuario mejorado para ayudarle a priorizar sus esfuerzos para los usuarios que tienen más probabilidades de estar en peligro.

- Nuevo soporte técnico de API para habilitar el acceso mediante programación a los datos de riesgo.

- Proceso simplificado de comentarios de administración que permite la protección inmediata de los usuarios.

- Nuevo aprendizaje automático supervisado para mejorar la precisión de las evaluaciones de riesgos.



Hoy en día, la seguridad es una preocupación importante para las organizaciones. La mayoría de las infracciones de seguridad tienen lugar cuando los atacantes obtienen acceso a un entorno mediante el robo de identidad de un usuario. Con los años, los atacantes son cada vez más eficaces al aprovechar las brechas de otros fabricantes y el uso de ataques de suplantación de identidad sofisticados. Una vez que los atacantes obtienen acceso, aunque sea a una cuenta de usuario con pocos privilegios, es relativamente sencillo que obtengan acceso a recursos importantes de la empresa mediante el desplazamiento lateral. 

Para responder a estas amenazas, Azure AD Identity Protection le permite: 

- Evitar de forma proactiva que se haga mal uso de las identidades que están en peligro. 

- Mitigar automáticamente los riesgos cuando se detecta actividad sospechosa. 

- Investigar los usuarios e inicios de sesión de riesgo para abordar las posibles vulnerabilidades.  

- Recibir una alerta cuando el riesgo de un usuario alcanza un umbral especificado. 

 

Azure AD Identity Protection es una característica de Azure Active Directory Premium P2 que le permite configurar directivas para responder automáticamente cuando se pone en peligro la identidad de un usuario o cuando alguien que no sea el propietario de la cuenta intente iniciar sesión con su identidad. Estas directivas, además de otros controles de acceso condicional proporcionados por Azure AD, pueden bloquear el acceso automáticamente o iniciar acciones de mitigación, como el restablecimiento de contraseñas o el cumplimiento de la autenticación multifactor. Además, Identity Protection proporciona funcionalidades de supervisión e informes para obtener información más detallada sobre el riesgo y los posibles peligros de la organización. 

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWsS6Q]


## <a name="risk-events"></a>Eventos de riesgo

Azure AD Identity Protection detecta los siguientes eventos de riesgo: 

 

| Tipo de evento de riesgo | DESCRIPCIÓN | Tipo de detección |
| ---             | ---         | ---            |
| Viaje atípico | Iniciar sesión desde una ubicación atípica, en función de los recientes inicios de sesión del usuario. | Sin conexión |
| Dirección IP anónima | Iniciar sesión desde una dirección IP anónima (por ejemplo: el explorador Tor o redes VPN anonimizadoras). | Tiempo real |
| Propiedades de inicio de sesión desconocidas | Iniciar sesión con propiedades que no hemos observado recientemente en el usuario en cuestión. | Tiempo real |
| Dirección IP vinculada al malware | Iniciar sesión desde una dirección IP vinculada al malware. | Sin conexión |
| Filtración de credenciales | Este evento de riesgo indica que se han filtrado las credenciales válidas del usuario. | Sin conexión |





## <a name="types-of-risk"></a>Tipos de riesgo 

La protección de identidades se basa en dos tipos de riesgo:

- Riesgo de inicio de sesión

- Riesgo de usuario

### <a name="sign-in-risk"></a>Riesgo de inicio de sesión

Un inicio de sesión de riesgo representa la probabilidad de que el propietario de la identidad no haya autorizado una solicitud de autenticación determinada.

Hay dos evaluaciones de riesgo de inicio de sesión: 

- **Riesgo de inicio de sesión (en tiempo real)**: el riesgo de inicio de sesión (en tiempo real) se basa en todas las detecciones en tiempo real que se desencadenan durante el procesamiento del inicio de sesión.  

- **Riesgo de inicio de sesión (agregado)**: el riesgo de inicio de sesión (agregado) es el riesgo total de un inicio de sesión. Lo calcula un modelo de Machine Learning que tiene en cuenta los aspectos siguientes:

    - Detecciones en tiempo real (descritas anteriormente)
    
    - Detecciones sin conexión (que se activan después de que haya tenido lugar el inicio de sesión) 
    
    - Todas las demás características del inicio de sesión


### <a name="user-risk"></a>Riesgo de usuario

Un riesgo de usuario representa la probabilidad de que una identidad determinada esté en peligro. 

El riesgo de usuario se calcula teniendo en cuenta todos los riesgos asociados con el usuario:

- Todos los inicios de sesión de riesgo
- Todos los eventos de riesgo que no están vinculados a ningún inicio de sesión
- El riesgo del usuario actual
- Las acciones de descarte o de corrección de riesgos realizadas en el usuario hasta la fecha



## <a name="how-identity-protection-detects-risk"></a>Cómo Identity Protection detecta el riesgo  

Azure AD usa el aprendizaje automático para detectar anomalías y actividades sospechosas mediante señales detectadas en tiempo real durante los inicios de sesión, así como con señales que no son en tiempos real relacionadas con los usuarios y sus actividades de inicio de sesión. Con estos datos, Identity Protection calcula un riesgo de inicio de sesión en tiempo real cada vez que un usuario se autentica; asimismo, determina un nivel de riesgo de usuario general para cada usuario. Identity Protection le permite tomar medidas automáticamente en relación con estas detecciones de riesgo mediante la configuración de directivas de riesgo de usuario y riesgo de inicio de sesión de Identity Protection.  

 

Para entender cómo Identity Protection detecta riesgos, hay dos conceptos importantes: riesgo de usuario y riesgo de inicio de sesión. Un inicio de sesión de riesgo refleja la probabilidad de que el propietario de la identidad no haya autorizado una solicitud de autenticación determinada. Hay dos tipos de inicios de sesión de riesgo: total y en tiempo real. El riesgo de inicio de sesión en tiempo real se detecta en el momento del intento de inicio de sesión determinado (por ejemplo, inicios de sesión desde direcciones IP anónimas). El riesgo de inicio de sesión total es el agregado de los riesgos de inicio de sesión en tiempo real detectados y los eventos de riesgo en tiempo real posteriores asociados con los inicios de sesión del usuario (por ejemplo, viaje imposible). El riesgo del usuario refleja la probabilidad global de que un actor perjudicial haya puesto en peligro una identidad determinada. El riesgo de usuario contiene todas las actividades de riesgo de un usuario determinado, incluidas las siguientes:

- Riesgo de inicio de sesión en tiempo real
- Riesgo de inicio de sesión posterior
- Detecciones de usuario de riesgo   

 

 
 ![Flujo](./media/overview-v2/01.png)
 

 

En el gráfico anterior se resumen el flujo de línea de base para la detección de riesgo de Identity Protection y la respuesta para cualquier inicio de sesión determinado.  

 

 

 

## <a name="common-scenarios"></a>Escenarios comunes 

Veamos el ejemplo de un empleado de Contoso. 

1. Un empleado intenta iniciar sesión en Exchange Online desde el explorador Tor. En el momento de inicio de sesión, Azure AD detecta eventos de riesgo en tiempo real. 

2. Azure AD detecta que el empleado inicia sesión desde una dirección IP anónima, desencadenar un nivel de riesgo de inicio de sesión medio. 

3. El empleado se enfrenta un símbolo del sistema MFA, porque el Administrador de TI de Contoso configura la directiva de acceso condicional de inicio de sesión de riesgo de Identity Protection. La directiva requiere MFA para un riesgo de inicio de sesión medio o superior. 

4. El empleado pasa el aviso de MFA y tiene acceso a Exchange Online, y no se puede cambiar su nivel de riesgo del usuario. 

¿Qué ha ocurrido en segundo plano? El intento de inicio de sesión desde el explorador Tor desencadena un riesgo de inicio de sesión en tiempo real en Azure AD para la dirección IP anónima. Cuando Azure AD procesa la solicitud, aplica la directiva de inicio de sesión de riesgo en Identity Protection puede configurada porque el nivel del empleado riesgo de inicio de sesión cumple el umbral (medio). Puesto que el empleado tenía registró anteriormente para MFA, fueron capaces de responder y pase el desafío de MFA. Su capacidad para pasar correctamente el desafío de MFA señala a Azure AD que se encontraban es probable que el propietario legítimo de identidad y no aumentar su nivel de riesgo del usuario. 


Pero ¿qué ocurre si el empleado no era el uno intentando iniciar sesión? 

1. Con las credenciales del empleado de un individuo malintencionado intenta iniciar sesión en su cuenta de Exchange Online desde el explorador Tor, puesto que intentan ocultar su dirección IP. 

2. Azure AD detecta que el intento de inicio de sesión proviene de una dirección IP anónima, lo que desencadena un nivel de riesgo de inicio de sesión en tiempo real. 

3. El individuo malintencionado encuentra un símbolo del sistema de MFA, ya que el administrador de TI de Contoso configuró la directiva de acceso condicional de riesgo de inicio de sesión de Identity Protection para que requiera MFA con riesgo de inicio de sesión medio o superior. 

4. El actor malintencionado produce un error en el desafío de MFA y no puede obtener acceso a la cuenta de Exchange Online del empleado. 

5. El error MFA símbolo del sistema desencadena un evento de riesgo que se registren, elevar su riesgo de usuario para futuros inicios de sesión. 

Ahora que un individuo malintencionado ha intentado obtener acceso a la cuenta de Sarah, veamos lo que sucede la próxima vez que el empleado intenta iniciar sesión. 

1. El empleado intenta iniciar sesión en Exchange Online desde Outlook. En el momento de inicio de sesión, Azure AD detecta eventos de riesgo en tiempo real, así como los riesgos de usuario anteriores. 

2. Azure AD no detecta ningún riesgo de inicio de sesión en tiempo real, pero detecta un riesgo de usuario alto debido a la actividad de riesgo precedente en los escenarios anteriores.  

3. El empleado se somete a una solicitud de restablecimiento de contraseña, porque Contoso del Administrador de TI configura la directiva de riesgo del usuario de Identity Protection para requerir cambio de contraseña cuando un usuario con alto riesgo inicia sesión. 

4. Puesto que el empleado se registra para SSPR y MFA, restablece correctamente su contraseña. 

5. Al restablecer su contraseña, ya no están en peligro las credenciales del empleado y devuelve su identidad a un estado seguro. 

6. Los eventos de riesgo anteriores del empleado se resuelven y se restablece automáticamente su nivel de riesgo de usuario como respuesta a mitigar el riesgo de credenciales. 

## <a name="how-do-i-configure-identity-protection"></a>Cómo configurar Identity Protection 

Para empezar a trabajar con Identity Protection, primero configure una directiva de riesgo de usuario y una directiva de riesgo de inicio de sesión. Una vez configuradas y aplicadas a un grupo de prueba, puede simular eventos de riesgo para entender cómo responderá Identity Protection en su entorno. Las siguientes guías de inicio rápido proporcionan un tutorial sobre cómo configurar las directivas mencionadas y probarlas en su entorno. 

 

Identity Protection es compatible con 3 roles de Azure AD para equilibrar las actividades de administración en torno a la implementación: 

| Rol | Puede hacer | No puede hacer |
| --- | --- | --- |
| Administrador global | Acceso completo a Identity Protection, incorporar Identity Protection | |
| Administrador de seguridad | Acceso completo a Identity Protection | Incorporar Identity Protection, restablecer contraseñas para un usuario |
| Lector de seguridad | Acceso de solo lectura a Identity Protection | Incorporar Identity Protection, corregir usuarios, configurar directivas, restablecer contraseñas| 

Para obtener más detalles, consulte [Asignación de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)

 
## <a name="licensing"></a>Licencia

>[!NOTE]
> Durante la versión preliminar pública de Identity Protection (actualizado), solo los clientes de Azure AD Premium P2 tendrán acceso a los informes de usuarios de riesgo e inicios de sesión de riesgo.



| Funcionalidad | Azure AD Premium P2 | Azure AD Premium P1 | Azure AD Basic/Free |
| --- | --- | --- | --- |
| Directiva de riesgo de usuario | Sí | No | No |
| Directiva de riesgo de inicio de sesión | Sí | No | No |
| Informe de usuarios de riesgo | Acceso total | Información limitada | Información limitada |
| Informe de inicios de sesión peligrosos | Acceso total | Información limitada | Información limitada |
| Directiva de registro de MFA | Sí | No | No |







## <a name="next-steps"></a>Pasos siguientes 

Para empezar a trabajar con Identity Protection, consulte [Configuración de la directiva de riesgo de inicio de sesión](quickstart-sign-in-risk-policy.md). 






