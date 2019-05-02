---
title: Inteligencia sobre amenazas y mapa de alertas de seguridad de Azure Security Center | Microsoft Docs
description: Aprenda a usar la funcionalidad de mapa de alertas e inteligencia sobre amenazas de seguridad de Azure Security Center para identificar posibles amenazas en las máquinas virtuales y los equipos.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: f1e238890b777e2816648ec407e2581f636d1c12
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60906197"
---
# <a name="security-alerts-map-and-threat-intelligence"></a>Mapa de alertas de seguridad e información sobre amenazas
En este artículo obtendrá ayuda para usar el mapa de alertas de seguridad y el mapa de inteligencia sobre amenazas basada en eventos de seguridad de Azure Security Center para solucionar problemas relacionados con la seguridad.

## <a name="how-the-security-alerts-map-works"></a>Cómo funciona el mapa de alertas de seguridad
Security Center le proporciona un mapa que le ayuda a identificar las amenazas de seguridad en el entorno. Por ejemplo, pueden identificar si un determinado equipo forma parte de una red de robots (botnet) y de dónde proviene la amenaza. Los equipos pueden convertirse en nodos en una de estas redes cuando los atacantes instalan de forma ilegal malware que interactúa en secreto con comandos y controles que administran la red de robots. 

Para crear este mapa, Security Center usa datos procedentes de varios orígenes dentro de Microsoft y aprovecha estos datos para crear un mapa de posibles amenazas en su entorno. 

Uno de los pasos de un [proceso de respuesta a incidentes de seguridad](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) consiste en identificar la gravedad de los sistemas en peligro. En esta fase debe realizar las siguientes tareas:

- Determinar la naturaleza del ataque
- Determinar el punto de origen del ataque.
- Determinar la intención del ataque ¿Estaba el ataque dirigido a su organización para conseguir información concreta o fue aleatorio?
- Identificar los sistemas que se pusieron en peligro
- Identificar los archivos a los que se ha obtenido acceso y determinar la confidencialidad de esos archivos

Puede usar el mapa de alertas de seguridad de Security Center para ayudarle con estas tareas.

## <a name="access-the-security-alerts-map"></a>Acceso al mapa de alertas de seguridad
Para visualizar las amenazas actuales en su entorno, abra el mapa de alertas de seguridad:

1. Abra el panel **Security Center**.
2. En el panel izquierdo, en **Protección contra amenazas**, haga clic en **Mapa de alertas de seguridad**. Se abre el mapa.
3. Para obtener más información acerca de la alerta y recibir los pasos de corrección, haga clic en el punto de alerta en el mapa y siga las instrucciones. 
 
El mapa de alertas de seguridad se basa en alertas. Estas alertas se basan en las actividades para las que la comunicación de red estaba asociada con una dirección IP que se resolvió correctamente, así la dirección IP sea una dirección IP de riesgo conocida o no (por ejemplo, un criptominero conocido) o una dirección IP que anteriormente no se reconocía como de riesgo. El mapa proporciona alertas a través de cualquier suscripción que haya seleccionado previamente en Azure. 

Las alertas en el mapa se muestran según la ubicación geográfica de donde se detecte su origen, y se codifican por color según la gravedad. 
    ![Información de inteligencia sobre amenazas](./media/security-center-threat-intel/security-center-alert-map.png)

## <a name="viewing-the-event-based-threat-intelligence-dashboard"></a>Visualización del panel de inteligencia de amenazas basadas en eventos
Para ver el mapa de inteligencia de amenazas basadas en eventos de seguridad sin procesar, puede seguir este procedimiento. Este mapa muestra solo los eventos que implican una dirección IP que se considera de riesgo, por ejemplo, una dirección IP de una red de robots (botnet) conocida.

1. Abra el panel **Security Center**.

1. En el panel izquierdo, en **Protección contra amenazas**, haga clic en **Mapa de alertas de seguridad**. Se abre el mapa.
2. En la esquina superior derecha, haga clic en **Ir al mapa de eventos de seguridad**.
3. Seleccione el área de trabajo para la que desea ver el panel.
4. En la parte superior del mapa, seleccione **Ver inteligencia de amenazas clásica**. Se abre el panel **Inteligencia sobre amenazas**.

   > [!NOTE]
   > Si en la columna de la derecha se muestra **UPGRADE PLAN** (ACTUALIZAR PLAN), esta área de trabajo está usando la suscripción gratuita. Para usar esta característica, actualice a la versión Estándar. Si la columna derecha muestra **requiere actualizar**, actualizar [registros de Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) para usar esta característica. Para más información sobre el plan de precios, consulte Precios de Azure Security Center.
   >
5. Si tiene más de un área de trabajo para investigar, priorice la investigación de acuerdo con la columna **Direcciones IP malintencionadas**. Muestra el número actual de direcciones IP malintencionadas en esta área de trabajo. Seleccione el área de trabajo que desea usar, y aparecerá el panel **Información sobre amenazas**.

    ![Información de inteligencia sobre amenazas](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

6. Este panel se divide en cuatro iconos:

     a.  **Tipos de amenaza**. Resume el tipo de amenaza detectada en el área de trabajo seleccionada.

    b.  **País de origen**. Agrega la cantidad de tráfico en función de su ubicación de origen.

    c.  **Ubicación de la amenaza**. Le ayuda a identificar las ubicaciones actuales de todo el mundo que se comunican con su entorno. En el mapa que se muestra, las flechas naranja (entrante) y roja (saliente) identifican las direcciones del tráfico. Al seleccionar una de estas flechas, aparece el tipo de amenaza y la dirección del tráfico.

    d.  **Detalles de la amenaza**. Muestra más detalles sobre la amenaza que seleccionó en el mapa.

Con independencia del icono de opción que seleccione, el panel que aparece se basa en la consulta de búsqueda de registros. La única diferencia es el tipo de consulta y el resultado.

### <a name="threat-types"></a>Tipos de amenaza
Seleccione el icono **Tipos de amenazas** para abrir el panel **Búsqueda de registros**. A la izquierda aparecen las opciones de filtro y a la derecha los resultados de consulta.

![Búsqueda de registros](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

El resultado de la consulta muestra las amenazas por nombre. Puede usar el panel izquierdo para seleccionar el atributo por el que quiere filtrar. Por ejemplo, para ver solo las amenazas que están conectadas actualmente a las máquinas en **SESSIONSTATE**, seleccione **Conectada** > **Aplicar**.

![Estado de sesión](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

En las máquinas virtuales de Azure, solo aparecen en el panel **Inteligencia sobre amenazas** los datos de red que pasan por el agente. Los siguientes tipos de datos también se usan en la inteligencia sobre amenazas:

- Datos CEF (Tipo = CommonSecurityLog)
- WireData (Tipo = WireData)
- Registros de IIS (Tipo = W3CIISLog)
- Firewall de Windows (Tipo = WindowsFirewall)
- Eventos de DNS (Tipo = DnsEvents)


## <a name="see-also"></a>Vea también
En este artículo, ha aprendido a utilizar la inteligencia sobre amenazas de Security Center para ayudarle a identificar cualquier actividad sospechosa. Para más información sobre Security Center, consulte los siguientes artículos:

* [Administración y respuesta a las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) Aprenda a administrar las alertas y a responder a incidentes de seguridad en Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.
* [Comprensión de las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Obtenga información acerca de los distintos tipos de alertas de seguridad.
* [Guía de solución de problemas de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Obtenga información acerca de cómo solucionar problemas comunes en Security Center.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md). Obtenga respuestas a preguntas frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/). Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.
