---
title: 'Azure Monitor para VM (versión preliminar): preguntas más frecuentes | Microsoft Docs'
description: Azure Monitor para VM (versión preliminar) es una solución de Azure que combina la supervisión del estado y el rendimiento del sistema operativo de máquina virtual de Azure. Detecta automáticamente los componentes de la aplicación y las dependencias con otros recursos, y asigna la comunicación entre ellos. En este artículo se responde a las preguntas más habituales.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/08/2018
ms.author: magoedte
ms.openlocfilehash: a97a7be0eaa8438a4df27b610106ec6ab9f60d30
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184399"
---
# <a name="azure-monitor-for-vms-preview-faq"></a>Preguntas más frecuentes de Azure Monitor para VM (versión preliminar)
En este artículo se responden las preguntas más frecuentes sobre Azure Monitor para VM. Si tiene alguna otra pregunta sobre la solución, vaya al [foro de discusión de Azure](https://feedback.azure.com/forums/34192--general-feedback) y publíquela. Si hay preguntas que se formulan con frecuencia, se agregan a este artículo para que se puedan encontrar de forma rápida y sencilla.

## <a name="can-i-deploy-vms-to-an-existing-workspace"></a>¿Puedo implementar máquinas virtuales en un área de trabajo existente?
Si las máquinas virtuales ya están conectadas a un área de trabajo de Log Analytics, puede seguir usando esa área de trabajo cuando las implemente en Azure Monitor para VM. El área de trabajo debe existir en una de las regiones admitidas que aparecen en la sección "Requisitos previos" de [Implementación de Azure Monitor para VM (versión preliminar)](vminsights-onboard.md#prerequisites).

Durante la implementación, vamos a configurar los contadores de rendimiento para el área de trabajo. Esto hace que las máquinas virtuales que notifican datos al área de trabajo empiecen a recopilar la información para mostrarla y analizarla en Azure Monitor para VM. Como resultado, verá los datos de rendimiento de todas las máquinas virtuales conectadas al área de trabajo que seleccionó. Las características de mantenimiento y de asignación solo se habilitan para las máquinas virtuales que se han especificado para la implementación.

Para obtener más información acerca de qué contadores rendimiento se habilitan, consulte [Implementación de Azure Monitor para VM (versión preliminar)](vminsights-onboard.md).

## <a name="can-i-deploy-vms-to-a-new-workspace"></a>¿Puedo implementar máquinas virtuales en un área de trabajo? 
Si las máquinas virtuales no están conectadas actualmente a un área de trabajo de Log Analytics existente, deberá crear una para almacenar los datos. Puede crear una automáticamente si configura una sola máquina virtual de Azure Monitor para VM en Azure Portal.

Si decide usar el método basado en script, consulte [Implementación de Azure Monitor para VM (versión preliminar)](vminsights-onboard.md). 

## <a name="what-can-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>¿Qué puedo hacer si mi máquina virtual ya está generando informes para un área de trabajo?
Si ya está recopilando datos de las máquinas virtuales, es posible que ya las haya configurado para que generen informes de datos para un área de trabajo de Log Analytics existente. Siempre y cuando el área de trabajo se encuentre en una de nuestras regiones admitidas, podrá habilitar Azure Monitor para VM en esa área de trabajo preexistente. Estamos trabajando para admitir más regiones.

>[!NOTE]
>Configuramos los contadores de rendimiento para el área de trabajo que afecta a todas las máquinas virtuales que se comunican con el área de trabajo, independientemente de si ha elegido implementarlas en Azure Monitor para VM. Para obtener más información acerca de cómo se configuran los contadores de rendimiento para el área de trabajo, consulte la sección "Configuración de contadores de rendimiento" de [Orígenes de datos de rendimiento de Windows y Linux en Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md). Para obtener información acerca de los contadores configurados para Azure Monitor para VM, consulte [Implementación de Azure Monitor para VM (versión preliminar)](vminsights-onboard.md). 

## <a name="why-did-my-vm-deployment-fail"></a>¿Por qué no se pudo realizar la implementación de la máquina virtual?
Al implementar una máquina virtual de Azure desde Azure Portal, ocurre lo siguiente:

* Se crea un área de trabajo de Log Analytics predeterminada si se ha seleccionado la opción.
* Los contadores de rendimiento se configuran para el área de trabajo seleccionada. Si se produce un error en este paso, algunas de las tablas y gráficos de rendimiento no muestran datos para la máquina virtual que se implementó. Para solucionar este problema, ejecute el script de PowerShell que se documenta en la sección "Habilitar con PowerShell" de [Implementación de Azure Monitor para VM (versión preliminar)](vminsights-onboard.md#enable-with-powershell).
* Se instala el agente de Log Analytics en las máquinas virtuales de Azure con una extensión de máquina virtual, si es necesario. 
* En Azure Monitor para VM, Dependency Agent para Service Map se instala con una extensión, si es necesario. 
* De ser necesario, se configuran los componentes de Azure Monitor que admiten la característica de estado y la máquina virtual se configura para notificar los datos de estado.

Durante la implementación, se comprueba el estado de cada uno de los pasos anteriores y se devuelve un estado de notificación en el portal. La configuración del área de trabajo y la instalación del agente normalmente tarda de 5 a 10 minutos. La visualización de los datos de supervisión y estado en Azure Portal tarda otros 5 o 10 minutos. 

Si ha iniciado la implementación y puede ver los mensajes que indican que la máquina virtual debe implementarse, espere hasta 30 minutos para que esta complete el proceso. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>No veo algunos o ninguno de los datos en el gráfico de rendimiento de la máquina virtual
Si los datos de rendimiento no se muestran en la tabla del disco o en los gráficos de rendimiento, es posible que los contadores de rendimiento no estén configurados en el área de trabajo. Para solucionar este problema, ejecute el script de PowerShell que se documenta en la sección "Habilitar con PowerShell" de [Implementación de Azure Monitor para VM (versión preliminar)](vminsights-onboard.md#enable-with-powershell).

## <a name="how-is-the-azure-monitor-for-vms-map-feature-different-from-service-map"></a>¿En qué se diferencian la característica Mapa de Azure Monitor para VM y Service Map?
La característica Mapa de Azure Monitor para VM está basada en Service Map, pero se diferencia en los siguientes aspectos:

* Se puede acceder a la vista Mapa desde el panel Máquina virtual y desde Azure Monitor para VM en Azure Monitor.
* Ahora se puede hacer clic sobre las conexiones en el mapa y, en el panel lateral, se muestran los datos de métricas de la conexión.
* Se usa una API nueva para crear los mapas, lo que ofrece una mejor compatibilidad con mapas más complejos.
* Ahora se incluyen máquinas virtuales supervisadas en el nodo del grupo del cliente y el gráfico de anillos muestra la proporción de máquinas virtuales no supervisadas frente a las supervisadas. También puede filtrar la lista de máquinas cuando el grupo está expandido.
* Ahora se incluyen máquinas virtuales supervisadas en los nodos del grupo de puertos de servidor y el gráfico de anillos muestra la proporción de máquinas virtuales no supervisadas frente a las supervisadas. También puede filtrar la lista de máquinas cuando el grupo está expandido.
* El estilo del mapa se actualizó para que sea más coherente con el Mapa de aplicación de Azure Application Insights.
* Los paneles laterales se han actualizado, pero todavía no tienen el conjunto completo de aplicaciones integradas que se admitían en Service Map: Update Management, Change Tracking, Security y Service Desk. 
* Se ha actualizado la opción para elegir grupos y máquinas para el mapa. Ahora admite suscripciones, grupos de recursos, conjuntos de escalado de máquinas virtuales de Azure y servicios en la nube.
* No puede crear nuevos grupos de máquinas de Service Map en la característica Mapa de Azure Monitor para VM. 

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>¿Por qué mi gráficos de rendimiento muestran líneas punteadas?

Los gráficos de rendimiento muestran líneas de puntos en lugar de líneas sólidas por dos motivos:
* Puede haber una discontinuidad en la recopilación de datos. 

* La configuración predeterminada para el muestreo de datos es cada 60 segundos. Si elige un intervalo de tiempo reducido para el gráfico y la frecuencia de muestreo es menor que el tamaño de depósito utilizado en el gráfico, puede que vea las líneas de puntos. Supongamos que ha elegido una frecuencia de muestreo de 10 minutos y cada depósito en el gráfico es de 5 minutos. En este caso, elegir un intervalo de tiempo más amplio para la visualización debe hacer que las líneas del gráfico sean sólidas en lugar de punteadas.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>¿Los grupos son compatibles con Azure Monitor para máquinas virtuales?
Sí, una vez que se instala Dependency Agent recopilamos información de las máquinas virtuales para mostrar los grupos por suscripción, grupo de recursos, conjunto de escalado de máquinas virtuales y servicios en la nube. Si ha usado Service Map y ha creado grupos de máquinas, también se muestran. Los grupos de equipos también aparecerán en el filtro de grupos si los ha creado para el área de trabajo que ve. 

## <a name="how-can-i-display-the-details-about-whats-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>¿Cómo puedo mostrar los detalles de lo que está aumentando la línea del percentil 95 en los gráficos de rendimiento agregado?
De forma predeterminada, la lista se ordena para mostrar las máquinas virtuales que tienen el valor más alto para el percentil 95 de la métrica seleccionada. Una excepción es el gráfico de la **memoria disponible**, que muestra las máquinas con el valor del quinto percentil más bajo. Seleccione el gráfico para abrir la vista **Top N List** (Lista de N más altos) con la métrica adecuada seleccionada.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-various-virtual-networks-and-subnets"></a>¿Cómo administra la característica Mapa las direcciones IP duplicadas entre distintas redes virtuales y subredes?
Si va a duplicar los intervalos de direcciones IP con máquinas virtuales o conjuntos de escalado de máquinas virtuales de Azure entre subredes y redes virtuales, puede hacer que la característica Mapa de Azure Monitor para VM muestre información incorrecta. Conocemos el problema y estamos investigando opciones para mejorar la experiencia.

## <a name="does-the-map-feature-support-ipv6"></a>¿La característica Mapa es compatible con IPv6?
Por el momento, la característica Mapa solo es compatible con IPv4 y estamos investigando la compatibilidad con IPv6. También es compatible con IPv4 que se tuneliza dentro de IPv6.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-why-is-the-map-difficult-to-view"></a>Cuando cargo un mapa para un grupo de recursos o algún otro grupo grande, ¿por qué me resulta difícil verlo?
Aunque hemos realizado mejoras en la característica Mapa para tratar con configuraciones grandes y complejas, somos conscientes de que un mapa puede tener una gran cantidad de nodos, conexiones y nodos que funcionen como un clúster. Nos comprometemos a seguir mejorando la compatibilidad para aumentar la escalabilidad.  

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-from-the-network-chart-on-the-azure-vm-overview-page"></a>¿Por qué el gráfico de red de la pestaña Rendimiento es distinta del gráfico de red de la página Información general de la máquina virtual de Azure?

La página Información general de una máquina virtual de Azure muestra gráficos basados en la medición de actividad de la máquina virtual invitada que realiza el host. En el gráfico de red de la página Información general de la máquina virtual de Azure, solo se muestra el tráfico de red que se facturará. Esta pantalla no incluye el tráfico entre redes virtuales. Los datos y gráficos que se muestran en Azure Monitor para VM se basan en los datos de la máquina virtual invitada, y el gráfico de red muestra todo el tráfico TCP/IP entrante y saliente de esa máquina virtual, incluido el que fluye entre redes virtuales.

## <a name="what-are-the-limitations-of-the-log-analytics-free-pricing-plan"></a>¿Qué limitaciones hay en el plan de tarifa gratis de Log Analytics?
Si ha configurado Azure Monitor con un área de trabajo de Log Analytics mediante el plan de tarifa *gratis*, la característica Mapa de Azure Monitor para VM solo permitirá la conexión de cinco máquinas al área de trabajo. 

Por ejemplo, supongamos que tiene cinco máquinas virtuales gratuitas conectadas a un área de trabajo gratuita. Si desconecta una máquina virtual y, a continuación, vuelve a conectar otra nueva, la nueva máquina virtual no se supervisa ni se refleja en la página Mapa. En este escenario, cuando abra la nueva máquina virtual, se le pedirá que use la opción **Probar ahora** y seleccione la opción **Insights (versión preliminar)** en el panel izquierdo, incluso después de que se haya instalado en la máquina virtual. Sin embargo, no se le piden opciones, como ocurriría normalmente si la máquina virtual no se hubiese implementado en Azure Monitor para VM. 

## <a name="next-steps"></a>Pasos siguientes
Para conocer los requisitos y los métodos para habilitar la supervisión de máquinas virtuales, consulte [Implementación de Azure Monitor para VM (versión preliminar)](vminsights-onboard.md).
