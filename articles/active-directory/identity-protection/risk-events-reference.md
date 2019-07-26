---
title: Referencia sobre eventos de riesgo de Azure Active Directory Identity Protection | Microsoft Docs
description: Referencia sobre eventos de riesgo de Azure Active Directory Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
ms.date: 01/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08e27052c3583ddea7a2fb6fe96fa6b48cd6c372
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333881"
---
# <a name="azure-active-directory-identity-protection-risk-events-reference"></a>Referencia sobre eventos de riesgo de Azure Active Directory Identity Protection

La mayoría de las infracciones de seguridad tienen lugar cuando los atacantes obtienen acceso a un entorno mediante el robo de identidad de un usuario. Descubrir las identidades en peligro no es tarea fácil. Azure Active Directory utiliza algoritmos y heurística de aprendizaje automático adaptable para detectar acciones sospechosas que están relacionadas con las cuentas de usuario. Cada acción sospechosa detectada se almacena en un registro llamado evento de riesgo.

## <a name="anonymous-ip-address"></a>Dirección IP anónima

**Tipo de detección**: Tiempo real  
**Nombre anterior**: Inicios de sesión desde una dirección IP anónima

Este tipo de evento de riesgo indica inicios de sesión desde una dirección IP anónima (por ejemplo, el explorador Tor o Anonymizer VPN).
Estas direcciones IP normalmente las usan actores que quieren ocultar su telemetría de inicio de sesión (dirección IP, ubicación, dispositivo, etc.) con fines potencialmente malintencionados.

## <a name="atypical-travel"></a>Viaje atípico

**Tipo de detección**: Sin conexión  
**Nombre anterior**: Viaje imposible a ubicaciones inusuales

Este tipo de evento de riesgo identifica dos inicios de sesión procedentes de ubicaciones geográficamente distantes, donde al menos una de las ubicaciones puede también ser inusual para el usuario, según su comportamiento anterior. Entre otros factores, este algoritmo de aprendizaje automático tiene en cuenta el tiempo entre los dos inicios de sesión y el tiempo que habría necesitado el usuario para viajar de la primera ubicación a la segunda, lo que indica que otro usuario utiliza las mismas credenciales.

Este algoritmo omite "falsos positivos" obvios que contribuyen a una condición de viaje imposible, como las VPN y las ubicaciones que usan con regularidad otros usuarios de la organización. El sistema tiene un período de aprendizaje inicial de 14 días o 10 inicios de sesión, lo que ocurra primero, durante el cual aprende el comportamiento de inicio de sesión del nuevo usuario.

## <a name="leaked-credentials"></a>Filtración de credenciales

**Tipo de detección**: Sin conexión  
**Nombre anterior**: Usuarios con credenciales perdidas

Este tipo de evento de riesgo indica que se han filtrado las credenciales válidas del usuario.
Cuando los cibercriminales llegan a poner en peligro las contraseñas válidas de usuarios legítimos, es frecuente que las compartan. Suelen hacer esto publicándolas en la Web oscura o sitios de pegado, o bien mediante el intercambio o la venta de esas credenciales en el mercado negro. El servicio de filtrado de credenciales de Microsoft adquiere pares de nombre de usuario y contraseña mediante la supervisión de sitios web públicos y de la Web oscura y trabajando en conjunto con:

- Investigadores
- Autoridades judiciales
- Equipos de seguridad de Microsoft
- Otros orígenes de confianza

Cuando el servicio adquiere las credenciales de usuario de la Web oscura, los sitios de pegado o los orígenes anteriores, se comparan con las credenciales válidas actuales de los usuarios de Azure AD para encontrar coincidencias válidas.

## <a name="malware-linked-ip-address"></a>Dirección IP vinculada al malware

**Tipo de detección**: Sin conexión  
**Nombre anterior**: Inicios de sesión desde dispositivos infectados

Este tipo de evento de riesgo indica inicios de sesión desde direcciones IP infectadas con malware, que se sabe que se comunican activamente con un servidor bot. Esto se determina mediante la correlación de direcciones IP de dispositivos de usuarios con direcciones IP que estuvieron en contacto con un servidor bot mientras este último estaba activo.

## <a name="unfamiliar-sign-in-properties"></a>Propiedades de inicio de sesión desconocidas

**Tipo de detección**: Tiempo real  
**Nombre anterior**: Inicios de sesión desde ubicaciones desconocidas

Este tipo de evento de riesgo tiene en cuenta el historial de inicio de sesión anterior (dirección IP, latitud/longitud y ASN) para determinar inicios de sesión anómalos. El sistema almacena información acerca de las ubicaciones anteriores utilizadas por un usuario y considera estas ubicaciones "conocidas". El evento de riesgo se desencadena cuando el inicio de sesión se produce desde una ubicación que no está en la lista de ubicaciones conocidas. Los usuarios recién creados estarán en "modo de aprendizaje" durante un período de tiempo, en el que los eventos de riesgo de las propiedades de inicio de sesión desconocidas estarán desactivados mientras nuestros algoritmos aprenden el comportamiento del usuario. La duración del modo de aprendizaje es dinámica y depende de cuánto tiempo tarde el algoritmo en recopilar suficiente información sobre los patrones de inicio de sesión del usuario. La duración mínima es de cinco días. Un usuario puede volver al modo de aprendizaje tras un largo período de inactividad. El sistema también ignora los inicios de sesión desde dispositivos conocidos y ubicaciones geográficamente cercanas a una ubicación conocida. 

También se ejecuta esta detección para una autenticación básica o para protocolos heredados. Dado que estos protocolos no tienen propiedades modernas como el identificador de cliente, hay una telemetría limitada para reducir los falsos positivos. Se recomienda que los clientes realicen la migración a la autenticación moderna.

## <a name="azure-ad-threat-intelligence"></a>Inteligencia de Azure AD sobre amenazas

**Tipo de detección**: Sin conexión <br>
**Nombre anterior**: Esta detección se mostrará en los informes heredados de Azure AD Identity Protection (Usuarios marcados en riesgo, Eventos de riesgo) como "Usuarios con credenciales filtradas".

Este tipo de evento de riesgo indica una actividad de usuario poco común para el usuario en cuestión o coherente con patrones de ataque conocidos basados en orígenes de información sobre amenazas internas y externas de Microsoft.
