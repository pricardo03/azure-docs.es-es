---
title: Eventos de riesgo de Azure Active Directory | Microsoft Docs
description: En artículo se explica detalladamente qué son los eventos de riesgo.
services: active-directory
keywords: azure active directory identity protection, seguridad, riesgo, nivel de riesgo, vulnerabilidad, directiva de seguridad
author: priyamohanram
manager: daveba
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ed1b7dfa19644fa558cafd8df5fed72490f24ff
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56195692"
---
# <a name="azure-active-directory-risk-events"></a>Eventos de riesgo de Azure Active Directory

La mayoría de las infracciones de seguridad tienen lugar cuando los atacantes obtienen acceso a un entorno mediante el robo de identidad de un usuario. Descubrir las identidades en peligro no es tarea fácil. Azure Active Directory utiliza algoritmos y heurística de aprendizaje automático adaptable para detectar acciones sospechosas que están relacionadas con las cuentas de usuario. Cada acción sospechosa detectada se almacena en un registro llamado **evento de riesgo**.

Hay dos lugares donde puede revisar los eventos de riesgo informados:

 - **Informes de Azure AD**: los eventos de riesgo forman parte de los informes de seguridad de Azure AD. Para más información, consulte el [informe de seguridad de usuarios en riesgo](concept-user-at-risk.md) y el [informe de seguridad de inicios de sesión riesgosos](concept-risky-sign-ins.md).

 - **Azure AD Identity Protection**: los eventos de riesgo también forman parte de las funcionalidades de informes de [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

Además, puede utilizar la [API de eventos de riesgo Identity Protection](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) para acceder mediante programación a las detecciones de seguridad con Microsoft Graph. Para obtener más información, consulte [Introducción a Azure Active Directory Identity Protection y Microsoft Graph](../identity-protection/graph-get-started.md). 

En la actualidad, Azure Active Directory detecta seis tipos de eventos de riesgo:

- [Usuarios con credenciales perdidas](#leaked-credentials) 
- [Inicios de sesión desde direcciones IP anónimas](#sign-ins-from-anonymous-ip-addresses) 
- [Viaje imposible a ubicaciones inusuales](#impossible-travel-to-atypical-locations) 
- [Inicios de sesión desde dispositivos infectados](#sign-ins-from-infected-devices) 
- [Inicios de sesión desde direcciones IP con actividad sospechosa](#sign-ins-from-ip-addresses-with-suspicious-activity) 
- [Inicios de sesión desde ubicaciones desconocidas](#sign-in-from-unfamiliar-locations) 

![Evento de riesgo](./media/concept-risk-events/91.png)

> [!IMPORTANT]
> En ocasiones, es posible que encuentre un evento de riesgo sin una entrada de inicio de sesión correspondiente en el [informe de inicios de sesión](concept-sign-ins.md). Esto se debe a que Identity Protection evalúa el riesgo para inicios de sesión tanto **interactivos** como **no interactivos**, mientras que el informe de inicios de sesión muestra solo los inicios de sesión interactivos.

La perspectiva que se obtiene de un evento de riesgo detectado está asociada a su suscripción de Azure AD. 

* Con la **edición de Azure AD Premium P2**, obtiene la información más detallada acerca de todas las detecciones subyacentes. 
* Con la **edición de Azure AD Premium P1**, las detecciones que no están cubiertas por su licencia aparecen como el evento de riesgo **Inicio de sesión con riesgo adicional detectado**.

Si bien la detección de eventos de riesgo ya representa un aspecto importante de la protección de las identidades, también tiene la opción de abordarlas manualmente o implementar respuestas automatizadas si configura directivas de acceso condicional. Para obtener más información, consulte [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

## <a name="risk-event-types"></a>Tipo de evento de riesgo

La propiedad **tipo de evento de riesgo** es un identificador de la acción sospechosa para la que se ha creado un registro de evento de riesgo.

Las continuas inversiones de Microsoft en procesos de detección conducirán a:

- Mejoras en la precisión de la detección de los eventos de riesgo ya existentes 
- Nuevos tipos de eventos de riesgo que se agregarán en el futuro

### <a name="leaked-credentials"></a>Credenciales con fugas

Cuando los cibercriminales llegan a poner en peligro las contraseñas válidas de usuarios legítimos, es frecuente que las compartan. Suelen hacer esto publicándolas en la Web oscura o sitios de pegado, o bien mediante el intercambio o la venta de esas credenciales en el mercado negro. El servicio de filtrado de credenciales de Microsoft adquiere pares de nombre de usuario y contraseña mediante la supervisión de sitios web públicos y de la Web oscura y trabajando en conjunto con:

- Investigadores
- Autoridades judiciales
- Equipos de seguridad de Microsoft
- Otros orígenes de confianza 

Cuando el servicio adquiere pares de nombre de usuario y contraseña, se comprueban con respecto a las credenciales actuales válidas de los usuarios de AAD. Cuando se encuentra una coincidencia, significa que la contraseña de un usuario está en peligro y se crea un **evento de riesgo de credenciales filtradas**.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Inicios de sesión desde direcciones IP anónimas

Este tipo de evento de riesgo identifica los usuarios que han iniciado sesión correctamente desde una dirección IP que se ha identificado como una dirección IP de proxy anónima. A menudo, estos servidores proxy los usan los usuarios que desean ocultar la dirección IP del dispositivo y es posible que se usen con fines malintencionados.

### <a name="impossible-travel-to-atypical-locations"></a>Viaje imposible a ubicaciones inusuales

Este tipo de evento de riesgo identifica dos inicios de sesión procedentes de ubicaciones geográficamente distantes, donde al menos una de las ubicaciones puede también ser inusual para el usuario, según su comportamiento anterior. Entre otros factores, este algoritmo de aprendizaje automático tiene en cuenta el tiempo entre los dos inicios de sesión y el tiempo que habría necesitado el usuario para viajar de la primera ubicación a la segunda, lo que indica que otro usuario utiliza las mismas credenciales.

Este algoritmo omite "falsos positivos" obvios que contribuyen a una condición de viaje imposible, como las VPN y las ubicaciones que usan con regularidad otros usuarios de la organización. El sistema tiene un período de aprendizaje inicial de 14 días, durante el cual aprende el comportamiento de inicios de sesión del nuevo usuario. 

### <a name="sign-in-from-unfamiliar-locations"></a>Inicio de sesión desde ubicaciones desconocidas

Este tipo de evento de riesgo tiene en cuenta las ubicaciones de inicio de sesión anteriores (dirección IP, latitud/longitud y ASN) para determinar las ubicaciones nuevas o desconocidas. El sistema almacena información acerca de las ubicaciones anteriores utilizadas por un usuario y considera estas ubicaciones "conocidas". El evento de riesgo se desencadena cuando el inicio de sesión se produce desde una ubicación que no está en la lista de ubicaciones conocidas. El sistema tiene un período de aprendizaje inicial de 30 días, durante el cual no marca ninguna nueva ubicación como ubicación desconocida. El sistema también ignora los inicios de sesión desde dispositivos conocidos y ubicaciones geográficamente cercanas a una ubicación conocida. 

Identity Protection detecta inicios de sesión desde ubicaciones desconocidas también para protocolos heredados/de autenticación básica. Dado que estos protocolos no tienen las modernas características conocidas, como el identificador del cliente, no hay suficiente telemetría para reducir los falsos positivos. Para reducir el número de eventos de riesgo detectados, debe pasarse a la autenticación moderna.   

### <a name="sign-ins-from-infected-devices"></a>Inicios de sesión desde dispositivos infectados

Este tipo de evento de riesgo identifica los inicios de sesión desde dispositivos infectados con malware, que se sabe que se comunican activamente con un servidor bot. Esto se determina mediante la correlación de direcciones IP de dispositivos de usuarios con direcciones IP que estuvieron en contacto con un servidor bot. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Inicios de sesión desde direcciones IP con actividad sospechosa
Este tipo de evento de riesgo identifica las direcciones IP desde las que se ha producido un gran número de intentos fallidos de inicio de sesión, en varias cuentas de usuario, durante un corto período de tiempo. Esto compara los patrones de tráfico de direcciones IP usadas por atacantes y es un claro indicador de que las cuentas están ya o van a estar en peligro. Se trata de un algoritmo de aprendizaje automático que omite falsos positivos obvios, como las direcciones IP que utilizan con regularidad otros usuarios de la organización.  El sistema tiene un período de aprendizaje inicial de 14 días, durante el cual aprende el comportamiento de inicio de sesión de un nuevo usuario y un nuevo inquilino.

## <a name="detection-type"></a>Tipo de detección

La propiedad de tipo de detección es un indicador (**en tiempo real** o **sin conexión**) del período de tiempo de detección de un evento de riesgo. Actualmente, la mayoría de los eventos de riesgo se detectan sin conexión en una operación posterior de procesamiento una vez que ya se ha producido el evento de riesgo.

En la tabla siguiente se muestra la cantidad de tiempo que tarda un tipo de detección en aparecer en un informe relacionado:

| Tipo de detección | Informes de latencia |
| --- | --- |
| Tiempo real | 5 a 10 minutos |
| Sin conexión | 2 a 4 horas |


Para los tipos de evento de riesgo que detecta Azure Active Directory, los tipos de detección son los siguientes:

| Tipo de evento de riesgo | Tipo de detección |
| :-- | --- | 
| [Usuarios con credenciales perdidas](#leaked-credentials) | Sin conexión |
| [Inicios de sesión desde direcciones IP anónimas](#sign-ins-from-anonymous-ip-addresses) | Tiempo real |
| [Viaje imposible a ubicaciones inusuales](#impossible-travel-to-atypical-locations) | Sin conexión |
| [Inicios de sesión desde ubicaciones desconocidas](#sign-in-from-unfamiliar-locations) | Tiempo real |
| [Inicios de sesión desde dispositivos infectados](#sign-ins-from-infected-devices) | Sin conexión |
| [Inicios de sesión desde direcciones IP con actividad sospechosa](#sign-ins-from-ip-addresses-with-suspicious-activity) | Sin conexión|


## <a name="risk-level"></a>Nivel de riesgo

La propiedad de nivel de riesgo de un evento de riesgo es un indicador (**alto**, **medio** o **bajo**) de la gravedad y la confianza de un evento de riesgo. Esta propiedad le ayuda a clasificar por orden de prioridad las acciones que debe realizar. 

La gravedad del evento de riesgo representa la fuerza de la señal como predicción del riesgo de la identidad. La confianza es indicador de la posibilidad de que existan falsos positivos. 

Por ejemplo, 

* **Alta**: evento de riesgo de gravedad alta y de alta confianza. Estos eventos son buenos indicadores de que se ha puesto en peligro la identidad del usuario; las cuentas de usuario afectadas deben corregirse inmediatamente.

* **Media**: evento de riesgo de gravedad alta, pero confianza inferior, o viceversa. Estos eventos son potencialmente peligrosos y las cuentas de usuario afectadas deben corregirse.

* **Baja**: evento de riesgo de baja confianza y gravedad baja. Este evento puede no requerir una acción inmediata, pero cuando se combina con otros eventos de riesgo, puede proporcionar una indicación clara de que la identidad está en peligro.

![Nivel de riesgo](./media/concept-risk-events/01.png)

### <a name="leaked-credentials"></a>Credenciales con fugas

Los eventos de riesgo de credenciales con fugas se clasifican con gravedad **Alta**, ya que proporcionan una indicación clara de que el nombre de usuario y la contraseña están a disposición de un atacante.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Inicios de sesión desde direcciones IP anónimas

El nivel de riesgo de este tipo de evento es **Medio** porque una dirección IP anónima no es una indicación clara de una cuenta en riesgo. Se recomienda ponerse en contacto inmediatamente con el usuario para comprobar si utilizaba direcciones IP anónimas.


### <a name="impossible-travel-to-atypical-locations"></a>Viaje imposible a ubicaciones inusuales

Un viaje imposible suele ser un buen indicador de que un hacker logró iniciar sesión correctamente. Sin embargo, pueden producirse falsos positivos cuando un usuario viaja con un nuevo dispositivo o usa una VPN que normalmente no utilizan otros usuarios de la organización. Otra fuente de falsos positivos son las aplicaciones que pasan incorrectamente direcciones IP del servidor como IP de cliente, lo que puede dar la impresión de que los inicios de sesión tienen lugar desde el centro de datos en el que está hospedado el back-end de esa aplicación (a menudo son centros de datos de Microsoft, por lo que parece que los inicios de sesión tienen lugar en direcciones IP propiedad de Microsoft). Como resultado de estos falsos positivos, el nivel de riesgo de estos eventos de riesgo es **Medio**.

> [!TIP]
> Puede reducir la cantidad de falsos positivos notificados para este tipo de evento de riesgo si configura [ubicaciones con nombre](../active-directory-named-locations.md). 

### <a name="sign-in-from-unfamiliar-locations"></a>Inicio de sesión desde ubicaciones desconocidas

Las ubicaciones desconocidas pueden proporcionar una indicación clara de que un atacante puede usar una identidad robada. Se pueden producir falsos positivos cuando un usuario está viajando o probando un nuevo dispositivo, o bien usando una VPN nueva. Como resultado de estos falsos positivos, el nivel de riesgo para este tipo de evento es **Medio**.

### <a name="sign-ins-from-infected-devices"></a>Inicios de sesión desde dispositivos infectados

Este evento de riesgo identifica las direcciones IP, no los dispositivos de usuarios. Si hay varios dispositivos detrás de una única dirección IP, y solo algunos son controlados por una red de bot, los inicios de sesión desde otros dispositivos pueden desencadenar este evento innecesariamente, por lo que este evento de riesgo se clasifica como **Bajo**.  

Se recomienda ponerse en contacto con el usuario y examinar todos sus dispositivos. También es posible que el dispositivo personal de un usuario esté infectado o que otra persona usara un dispositivo infectado desde la misma dirección IP que el usuario. A menudo, los dispositivos infectados son infectados por malware que todavía no ha sido identificado por el software antivirus, y también pueden indicar cualquier hábito incorrecto del usuario que, posiblemente, hizo que el dispositivo se infectara.

Para obtener más información acerca de cómo tratar infecciones de malware, consulte el [Centro de protección contra malware](https://www.microsoft.com/en-us/security/portal/definitions/adl.aspx/).

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Inicios de sesión desde direcciones IP con actividad sospechosa

Se recomienda ponerse en contacto con el usuario para comprobar si realmente iniciaron sesión desde una dirección IP marcada como sospechosa. El nivel de riesgo de este tipo de evento es "**Medio**" porque varios dispositivos pueden encontrarse detrás de la misma dirección IP, mientras que solo algunos pueden ser responsables de la actividad sospechosa. 


## <a name="next-steps"></a>Pasos siguientes

* [Informe de seguridad sobre usuarios en riesgo](concept-user-at-risk.md)
* [Informe de seguridad sobre inicios de sesión de riesgo](concept-risky-sign-ins.md)
* [Azure AD Identity Protection](../active-directory-identityprotection.md).
