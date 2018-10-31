---
title: 'Azure Monitor para máquinas virtuales: preguntas más frecuentes | Microsoft Docs'
description: Azure Monitor para máquinas virtuales es una solución de Azure que combina la supervisión de rendimiento y de mantenimiento del sistema operativo de la VM de Azure, así como la detección automática de las dependencias y los componentes de aplicación con otros recursos, y asigna la comunicación entre ellos. En este artículo se responden preguntas comunes.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2018
ms.author: magoedte
ms.openlocfilehash: 282620342d2348868ceab5257de7415a9cb2147c
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49388370"
---
# <a name="azure-monitor-for-vms-frequently-asked-questions"></a>Azure Monitor para máquinas virtuales: preguntas más frecuentes
En este artículo de preguntas frecuentes de Microsoft, se presenta una lista con las preguntas frecuentes sobre Azure Monitor para máquinas virtuales en Microsoft Azure. Si tiene alguna otra pregunta sobre esta solución, vaya al [foro de discusión](https://feedback.azure.com/forums/34192--general-feedback) y publíquela. Si una pregunta es frecuente, se agrega a este artículo para que se pueda encontrar de forma rápida y sencilla.

## <a name="can-i-onboard-to-an-existing-workspace"></a>¿Puede incorporarse a un área de trabajo existente?
Si las máquinas virtuales ya están conectadas a un área de trabajo de Log Analytics, puede seguir usando esa área de trabajo cuando se incorpore a Azure Monitor para máquinas virtuales, siempre que se encuentre en una de las regiones compatibles enumeradas [aquí](monitoring-vminsights-onboard.md#prerequisites).

Durante la incorporación, configuramos los contadores de rendimiento para el área de trabajo que harán que todas las máquinas virtuales que estén generando informes para el área de trabajo comiencen a recopilar esta información para su presentación y análisis en Azure Monitor para máquinas virtuales.  Como resultado, verá los datos de rendimiento de todas las máquinas virtuales conectadas al área de trabajo que seleccionó.  Solo se habilitan las características de mantenimiento y de asignación para las máquinas virtuales que ha especificado para la incorporación.

Para obtener más información sobre qué contadores de rendimiento están habilitados, consulte nuestro artículo sobre [incorporación](monitoring-vminsights-onboard.md).

## <a name="can-i-onboard-to-a-new-workspace"></a>¿Puede incorporarse a una nueva área de trabajo? 
Si las máquinas virtuales no están conectadas actualmente a un área de trabajo de Log Analytics existente, deberá crear un área de trabajo para almacenar los datos.  Un área de trabajo predeterminada se crea automáticamente si configura una sola máquina virtual de Azure para Azure Monitor para máquinas virtuales a través de Azure Portal.

Si decide usar el método basado en scripts, estos pasos se tratan en el artículo sobre [incorporación](monitoring-vminsights-onboard.md). 

## <a name="what-do-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>¿Qué puedo hacer si mi máquina virtual ya está generando informes para un área de trabajo?
Si ya está recopilando datos de las máquinas virtuales, es posible que ya las haya configurado para que generen ubfirnes de datos a un área de trabajo de Log Analytics existente.  Siempre y cuando el área de trabajo se encuentre en una de nuestras regiones admitidas, podrá habilitar Azure Monitor para máquinas virtuales en esa área de trabajo preexistente.  Si el área de trabajo que ya está usando no está en una de nuestras regiones admitidas, no podrá incorporar Azure Monitor para máquinas virtuales en este momento.  Estamos trabajando para admitir más regiones.

>[!NOTE]
>Configuramos los contadores de rendimiento para el área de trabajo que afecta a todas las máquinas virtuales que se comunican con el área de trabajo, independientemente de si ha elegido incorporarlas a Azure Monitor para máquinas virtuales. Para obtener más detalles sobre cómo se configuran los contadores de rendimiento para el área de trabajo, consulte nuestra [documentación](../log-analytics/log-analytics-data-sources-performance-counters.md). Para obtener información acerca de los contadores configurados para Azure Monitor para máquinas virtuales, consulte nuestra [documentación de incorporación](monitoring-vminsights-onboard.md#performance-counters-enabled).  

## <a name="why-did-my-vm-fail-to-onboard"></a>¿Por qué no se pudo incorporar mi máquina virtual?
Al incorporar una máquina virtual de Azure desde Azure Portal, se producen los pasos siguientes:

* Se crea un área de trabajo de Log Analytics predeterminada si se ha seleccionado la opción.
* Los contadores de rendimiento se configuran para el área de trabajo seleccionada. Si se produce un error en este paso, verá que algunas de las tablas y gráficos de rendimiento no muestran datos para la máquina virtual que incorporó. Puede solucionar este problema si ejecuta el script de PowerShell documentado [aquí](monitoring-vminsights-onboard.md#enable-with-powershell).
* Se instala el agente de Log Analytics en las máquinas virtuales de Azure mediante la extensión de máquina virtual si se determina que es necesario.  
* Se instala el agente de la dependencia de asignación de Azure Monitor para máquinas virtuales mediante una extensión si se determina que es necesario.  
* De ser necesario, se configuran los componentes de Azure Monitor que admiten la característica de estado y la máquina virtual se configura para notificar sobre los datos de estado.

Durante el proceso de incorporación, comprobamos el estado de cada uno de los pasos anteriores para devolver un estado de notificación en el portal.  La configuración del área de trabajo y la instalación del agente normalmente tarda de 5 a 10 minutos.  La visualización de los datos de supervisión y estado en el portal tarda otros 5 o 10 minutos.  

Si ha iniciado la incorporación y puede ver los mensajes que indican que la máquina virtual debe incorporarse, espere hasta 30 minutos para que la máquina virtual pueda completar el proceso. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>No veo algunos o ninguno de los datos en el gráfico de rendimiento de la máquina virtual
Si no ve los datos de rendimiento en la tabla del disco o en algunos de los gráficos de rendimiento, es posible que los contadores de rendimiento en el área de trabajo no estén configurados. Para solucionar este problema, ejecute el siguiente [script de PowerShell](monitoring-vminsights-onboard.md#enable-with-powershell).

## <a name="how-is-azure-monitor-for-vms-map-feature-different-from-service-map"></a>¿En qué se diferencian la característica de asignación de Azure Monitor para máquinas virtuales y Service Map?
La característica de asignación de Azure Monitor para máquinas virtuales está basada en Service Map, pero se diferencia en los siguientes aspectos:

* Se puede acceder a la vista de asignación desde la hoja de máquina virtual y desde Azure Monitor para máquinas virtuales en Azure Monitor.
* Ahora se puede hacer clic sobre las conexiones en la asignación y, en el panel lateral, muestran una vista de los datos de métricas de la conexión seleccionada.
* Hay una nueva API que se usa para crear las asignaciones, lo que ofrece una mejor compatibilidad con asignaciones más complejas.
* Ahora se incluyen máquinas virtuales supervisadas en el nodo de grupo del cliente y el gráfico de anillos muestra la proporción de máquinas virtuales no supervisadas frente a las supervisadas en el grupo.  También puede usarse para filtrar la lista de máquinas cuando el grupo está expandido.
* Ahora se incluyen las máquinas virtuales supervisadas en los nodos de grupo de los puertos del servidor, y el gráfico de anillos muestra la proporción de máquinas supervisadas frente a las no supervisadas en el grupo.  También puede usarse para filtrar la lista de máquinas cuando el grupo está expandido.
* El estilo de la asignación se actualizó para que sea más coherente con el mapa de aplicación de Application Insights.
* Los paneles laterales se han actualizado, pero aún no tienen el conjunto completo de integración que era compatible con Service Map: Update Management, Change Tracking, seguridad y Service Desk. 
* La opción para elegir los grupos y máquinas que se asignarán se ha actualizado y ahora es compatible con las suscripciones, grupos de recursos, conjuntos de escalado de máquinas virtuales de Azure y servicios en la nube.
* No puede crear grupos de máquinas de Service Map en la característica de asignación de Azure Monitor para máquinas virtuales.  

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>¿Por qué mi gráficos de rendimiento muestran líneas punteadas?

Esto puede ocurrir por varios motivos.  En los casos donde hay una discontinuidad en la recopilación de datos, las líneas se muestran punteadas.  Si ha modificado la frecuencia de muestreo de datos para los contadores de rendimiento habilitados (el valor predeterminado es recopilar datos cada 60 segundos), podrá ver líneas punteadas en el gráfico si elige un intervalo de tiempo reducido para el gráfico y su frecuencia de muestreo es menor que el tamaño de depósito utilizado en el gráfico (por ejemplo, la frecuencia de muestreo es cada 10 minutos y cada depósito en el gráfico es de 5 minutos).  En este caso, elegir un intervalo de tiempo más amplio para la visualización debe hacer que las líneas del gráfico sean sólidas en lugar de punteadas.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>¿Los grupos son compatibles con Azure Monitor para máquinas virtuales?
Sí, una vez que se instala Dependency Agent recopilamos información de las máquinas virtuales para mostrar los grupos por suscripción, grupo de recursos,conjunto de escalado de máquinas virtuales y servicios en la nube.  Si ha usado Service Map y ha creado grupos de máquinas, también se muestran.  Los grupos de equipos también aparecerán en el filtro de grupos si los ha creado para el área de trabajo que ve. 

## <a name="how-do-i-see-the-details-for-what-is-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>¿Cómo puedo ver los detalles de lo que está aumentando la línea del percentil 95 en los gráficos de rendimiento agregado?
De forma predeterminada, la lista está ordenada para mostrar las máquinas virtuales con el valor de percentil 95 más alto de la métrica seleccionada, con la excepción del gráfico de memoria disponible, que muestra las máquinas con el valor más bajo de percentil 5.  Al hacer clic en el gráfico, se abrirá la vista **Top N List** (Lista de N más altos) con la métrica adecuada seleccionada.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-different-vnets-and-subnets"></a>¿Cómo administra la característica de asignación las direcciones IP duplicadas entre distintas redes virtuales y subredes?
Si va a duplicar los intervalos de IP con máquinas virtuales o conjuntos de escalado de máquinas virtuales de Azure entre subredes y redes virtuales, puede hacer que la asignación de Azure Monitor para máquinas virtuales muestre información incorrecta. Se trata de un problema conocido y estamos investigando opciones para mejorar esta experiencia.

## <a name="does-map-feature-support-ipv6"></a>¿La característica de asignación es compatible con IPv6?
Por el momento, la característica de asignación solo es compatible con IPv4 y estamos investigando la compatibilidad con IPv6. También es compatible con IPv4 que se tuneliza dentro de IPv6.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-the-map-is-difficult-to-view"></a>Cuando cargo una asignación para un grupo de recursos o algún otro grupo grande me resulta difícil verla.
Aunque hemos realizado mejoras a la asignación para que controle configuraciones grandes y complejas, somos conscientes de que una asignación puede tener una gran cantidad de nodos, conexiones y nodos que funcionen como un clúster.  Nos comprometemos a seguir mejorando la compatibilidad para aumentar la escalabilidad.   

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-than-the-network-chart-on-the-azure-vm-overview-page"></a>¿Por qué el gráfico de red de la pestaña Rendimiento es distinta al gráfico de red de la página Información general de la máquina virtual de Azure?

La página de información general de una máquina virtual de Azure muestra gráficos basados en la medición de actividad de la máquina virtual invitada que realiza el host.  En el gráfico de red de la información general de la máquina virtual de Azure, solo se muestra el tráfico de red que se facturará.  Esto no incluye el tráfico entre redes virtuales.  Los datos y gráficos que se muestran en Azure Monitor para máquinas virtuales se basan en los datos de la máquina virtual invitada, y el gráfico de red muestra todo el tráfico TCP/IP entrante y saliente de esa máquina virtual, incluido el que fluye entre redes virtuales.

## <a name="next-steps"></a>Pasos siguientes
Consulte [Incorporación de Azure Monitor para VM](monitoring-vminsights-onboard.md) para conocer los requisitos y los métodos necesarios para habilitar la supervisión de máquinas virtuales.
