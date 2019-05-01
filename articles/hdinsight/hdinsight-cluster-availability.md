---
title: Cómo supervisar la disponibilidad del clúster con los registros de Ambari y Azure Monitor
description: Aprenda a usar Ambari y los registros de Azure Monitor para supervisar la disponibilidad y estado del clúster.
keywords: supervisión de ambari, monitor, log analytics, alerta, disponibilidad, estado
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: tyfox
ms.openlocfilehash: 459de569916af14b0efea0ff08b92e5c93ed2369
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718898"
---
# <a name="how-to-monitor-cluster-availability-with-ambari-and-azure-monitor-logs"></a>Cómo supervisar la disponibilidad del clúster con los registros de Ambari y Azure Monitor

Los clústeres de HDInsight incluyen Apache Ambari, que proporciona información de estado en un solo vistazo y alertas predefinidas, así como integración de registros de Azure Monitor, que proporciona consultables métricas y registros, así como alertas configurables.

Este documento muestra cómo usar estas herramientas para supervisar el clúster y le guía a través de algunos ejemplos para configurar una alerta de Ambari, tarifa de disponibilidad del nodo de supervisión y creación de una alerta de Azure Monitor que se desencadena cuando no se recibió ningún latido desde uno o varios nodos en cinco horas.

## <a name="ambari"></a>Ambari

### <a name="dashboard"></a>panel

El panel de Ambari puede tener acceso al hacer clic en el **Ambari principal** vincular en el **paneles de clúster** sección de la hoja de información general de HDInsight en Azure portal como se muestra a continuación. Como alternativa, puede obtenerse mediante la especificación de la siguiente dirección URL en un explorador [https://\<clustername\>. azurehdinsight.net](https://clustername.azurehdinsight.net/)

![Vista de portal de recursos de HDInsight](media/hdinsight-cluster-availability/portal-overview.png)

A continuación, se le pedirá un nombre de usuario de inicio de sesión de clúster y una contraseña. Escriba las credenciales eligió al crear el clúster.

Se le llevará al panel de Ambari, que contiene los widgets que muestran una serie de métricas para ofrecerle una vista rápida del estado de su clúster de HDInsight. Estos widgets muestran métricas como el número de nodos (nodos de trabajo) de datos en directo y a JournalNodes (nodo zookeeper), el tiempo de actividad de nodos de nombre (nodos principales), como bien métricas específicas para determinados tipos de clúster, como el tiempo de actividad de ResourceManager de YARN para los clústeres de Spark y Hadoop.

![Panel de Ambari](media/hdinsight-cluster-availability/ambari-dashboard.png)

### <a name="hosts--view-individual-node-status"></a>Hosts: ver el estado de nodo individual

También puede ver información de estado de los nodos individuales. Haga clic en el **Hosts** pestaña para ver una lista de todos los nodos del clúster y ver información básica sobre cada nodo. La marca de verificación verde a la izquierda de cada nombre de nodo indica que todos los componentes están activos en el nodo. Si un componente está inactivo en un nodo, verá un triángulo de la alerta rojo en lugar de la marca de verificación verde.

![Vista de hosts de Ambari](media/hdinsight-cluster-availability/ambari-hosts.png)

A continuación, puede hacer clic en el **nombre** de un nodo para ver métricas más detalladas del host para ese nodo concreto. Esta vista muestra la disponibilidad y estado de cada componente individual.

![Vista de nodo único de hosts de Ambari](media/hdinsight-cluster-availability/ambari-hosts-node.png)

### <a name="ambari-alerts"></a>Alertas de Ambari

Ambari también ofrece varias alertas configurables que pueden proporcionar una notificación de determinados eventos. Cuando se desencadenan alertas, se muestran en la esquina superior izquierda de Ambari en un distintivo de color rojo que contiene el número de alertas. Al hacer clic en esta notificación se muestra una lista de las alertas actuales.

![Recuento de alertas de Ambari](media/hdinsight-cluster-availability/ambari-alerts.png)

Para ver una lista de definiciones de alertas y sus Estados, haga clic en el **alertas** pestaña, como se muestra a continuación.

![Ver las definiciones de alertas de Ambari](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambari proporciona muchas alertas predefinidas relacionadas a la disponibilidad, incluyendo:

| Nombre de la alerta                        | DESCRIPCIÓN                                                                                                                                                                           |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Resumen de estado DataNode           | Este nivel de servicio de alerta se desencadena si hay nodos de datos en mal estado                                                                                                                |
| Estado de disponibilidad de NameNode alto | Si no se está ejecutando el NameNode activo o NameNode en espera, se desencadena esta alerta de nivel de servicio.                                                                              |
| Porcentaje JournalNodes disponibles    | Esta alerta se desencadena si el número de abajo JournalNodes del clúster es mayor que el umbral crítico configurado. Agrega los resultados del proceso de JournalNode comprueba. |
| Porcentaje DataNodes disponibles       | Esta alerta se desencadena si el número de abajo nodos de datos en el clúster es mayor que el umbral crítico configurado. Agrega los resultados del proceso de DataNode comprueba.       |

Una lista completa de Ambari alertas que supervisa la Ayuda de la disponibilidad de un clúster puede encontrarse [aquí](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui),

Para ver los detalles de una alerta o modificar los criterios, haga clic en el **nombre** de la alerta. Tomar **resumen de estado DataNode** como ejemplo. Puede ver una descripción de la alerta, así como los criterios específicos que se desencadenarán una alerta de 'advertencia' o 'critical' y el intervalo de comprobación de los criterios. Para editar la configuración, haga clic en el **editar** situado en la esquina superior derecha del cuadro de configuración.

![Configuración de alertas de Ambari](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

En este caso, puede editar la descripción y, más importante aún, la comprobación de intervalo y los umbrales para alertas de advertencia o críticos.

![Vista de edición de configuración de alertas de Ambari](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

En este ejemplo, puede hacer que 2 nodos de datos en mal estado a desencadenar una alerta crítica y 1 en mal estado DataNode solo se desencadenen una advertencia. Haga clic en **guardar** cuando haya terminado edición.

### <a name="email-notifications"></a>Notificaciones por correo electrónico

Opcionalmente, también puede configurar notificaciones por correo electrónico para alertas de Ambari. Para ello, cuando esté en el **alertas** pestaña, haga clic en el **acciones** botón en la esquina superior izquierda, a continuación, **Administrar notificaciones.**

![Acción de las notificaciones de administración de Ambari](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Se abrirá un cuadro de diálogo para administrar las notificaciones de alerta. Haga clic en el **+** en la parte inferior del cuadro de diálogo y rellene los campos obligatorios para proporcionar Ambari con correo electrónico los detalles del servidor desde el que se va a enviar correos electrónicos.

> [!TIP]
> Configuración de Ambari notificaciones por correo electrónico pueden ser una buena forma de recibir alertas en un solo lugar al administrar muchos de los clústeres de HDInsight.

## <a name="azure-monitor-logs-integration"></a>Integración de registros de Azure Monitor

Monitor de Azure registra datos de habilita generados por varios recursos, como clústeres de HDInsight, que se recopilan y se agregan en un solo lugar para lograr una experiencia unificada de supervisión.

Como requisito previo, necesita un área de trabajo de Log Analytics para almacenar los datos recopilados. Si aún no ha creado uno, puede seguir estas instrucciones: [Crear un área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

### <a name="enable-hdinsight-azure-monitor-logs-integration"></a>Habilitar la integración de registros de HDInsight de Azure Monitor

En la página de recursos del clúster de HDInsight en el portal, haga clic en el **Operations Management Suite** hoja. A continuación, haga clic en **habilitar** y seleccione el área de trabajo de Log Analytics en la lista desplegable.

![Hoja de HDInsight Operations Management Suite](media/hdinsight-cluster-availability/portal-enable-oms.png)

### <a name="query-metrics-and-logs-tables-in-the-logs-blade"></a>Consultar las tablas de métricas y registros en la hoja de registros

Una vez habilitada la integración de registros de Azure Monitor (Esto puede tardar unos minutos), vaya a su **el área de trabajo de Log Analytics** recursos y haga clic en el **registros** hoja

![Hoja de registros de área de trabajo de log Analytics](media/hdinsight-cluster-availability/portal-logs.png)

El **registros** hoja muestra un número de consultas de ejemplo, como:

| Nombre de la consulta                      | DESCRIPCIÓN                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Disponibilidad de los equipos hoy en día    | El número de equipos que envían los registros, cada hora del gráfico                     |
| Latidos de lista                 | Lista de todos los latidos de equipo desde la última hora                           |
| Último latido de cada equipo | Mostrar el último latido enviado por cada equipo.                             |
| Equipos desconectados           | Una lista de todos los equipos conocidos que no ha enviado un latido en las últimas 5 horas |
| Tarifa de disponibilidad               | Calcular la tasa de disponibilidad de cada equipo conectado                |

Por ejemplo, ejecute el **tarifa de disponibilidad** consulta de ejemplo haciendo **ejecutar** en esa consulta, como se muestra en la captura de pantalla anterior. Esto mostrará la tarifa de disponibilidad de cada nodo del clúster como un porcentaje. Si ha habilitado varios clústeres de HDInsight enviar métricas a la misma área de trabajo de Log Analytics, verá la tarifa de disponibilidad para todos los nodos en los clústeres que se muestran.

![Iniciar consulta de ejemplo de análisis del área de trabajo registros hoja 'tarifa de disponibilidad'](media/hdinsight-cluster-availability/portal-availability-rate.png)

> [!NOTE] 
> Tarifa de disponibilidad se mide en un período de 24 horas, por lo que el clúster debe ejecutar durante al menos 24 horas antes de ver las tarifas de disponibilidad precisa.

Puede anclar esta tabla a un panel compartido, haga clic en **Pin** en la esquina superior derecha. Si no tiene ningún panel compartido grabable, puede ver cómo crear una aquí: [Crear y compartir paneles en Azure portal](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-a-dashboard-and-manage-access-control).

### <a name="azure-monitor-alerts"></a>Alertas de Azure Monitor

También puede establecer alertas de Azure Monitor que se desencadenará cuando el valor de una métrica o los resultados de una consulta cumplan ciertas condiciones. Por ejemplo, vamos a crear una alerta para enviar un correo electrónico cuando uno o varios nodos no se ha enviado un latido en 5 horas (es decir, se supone que no esté disponible).

Desde el **registros** hoja, ejecute el **equipos desconectados** consulta de ejemplo haciendo **ejecutar** en esa consulta, como se muestra a continuación.

![Iniciar consulta de ejemplo de análisis del área de trabajo registros hoja "equipos no disponible"](media/hdinsight-cluster-availability/portal-unavailable-computers.png)

Si todos los nodos están disponibles, esta consulta debería devolver 0 resultados por ahora. Haga clic en **nueva regla de alerta** para comenzar a configurar la alerta para esta consulta.

![Regla de alerta nueva área de trabajo log Analytics](media/hdinsight-cluster-availability/portal-logs-new-alert-rule.png)

Hay tres componentes a una alerta: el *recursos* para que se va a crear la regla (el área de Log Analytics en este caso), el *condición* para desencadenar la alerta y el *grupos de acciones*  que determinar lo que ocurrirá cuando se desencadene la alerta.

Haga clic en el **condición título**, tal y como se muestra a continuación, para finalizar la configuración de la lógica de señal.

![Condición de regla de alerta](media/hdinsight-cluster-availability/portal-condition-title.png)

Se abrirá el **configurar lógica de señal** hoja.

Establecer el **lógica de alerta** sección como sigue:

*En función de: Número de resultados, condición: Mayor umbral: 0.*

Puesto que esta consulta devuelve sólo nodos no disponibles como resultado, si el número de resultados es cada vez mayor que 0, se debe activar la alerta.

En el **evaluado según** sección, establezca el **período** y **frecuencia** según con qué frecuencia desea comprobar para los nodos no está disponible.

Tenga en cuenta que con el propósito de esta alerta, desea asegurarse de que **= período de frecuencia.** Puede encontrar más información sobre el período, la frecuencia y otros parámetros de alerta [aquí](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types).

Haga clic en **realiza** cuando haya terminado de configurar la lógica de señal.

![Regla de alerta de configuración de la lógica de señal](media/hdinsight-cluster-availability/portal-configure-signal-logic.png)

Si no dispone de un grupo de acciones existente, haga clic en **crear nuevo** bajo el **grupos de acciones** sección.

![Nuevo grupo de acciones de regla de alerta](media/hdinsight-cluster-availability/portal-create-new-action-group.png)

Se abrirá el **Agregar grupo de acciones** hoja. Elija un **nombre del grupo de acción**, **nombre corto**, **suscripción**, y **grupo de recursos.** En el **acciones** sección, elija un **nombre de acción** y seleccione **correo electrónico o SMS/Push, voz** como el **tipo de acción.**

> [!NOTE]
> Hay muchas otras acciones que puede desencadenar una alerta además una correo electrónico o SMS/Push, voz, como una función de Azure,. LogicApp, Webhook, ITSM Runbook de Automation. [Aprende más.](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information)

Se abrirá el **correo electrónico o SMS/Push, voz** hoja. Elija un **nombre** para el destinatario, **comprobar** el **correo electrónico** cuadro y escriba una dirección de correo electrónico al que desee enviar la alerta. Haga clic en **Aceptar** en el **correo electrónico o SMS/Push, voz** hoja, a continuación, en el **Agregar grupo de acciones** hoja para terminar de configurar el grupo de acciones.

![Regla de alerta Agregar grupo de acciones](media/hdinsight-cluster-availability/portal-add-action-group.png)

Después de cerrarán estos módulos, debería ver el grupo de acciones aparece bajo el **grupos de acciones** sección. Por último, complete el **detalles de alerta** sección escribiendo un **nombre de la regla de alerta** y **descripción** y eligiendo un **gravedad**.
Haga clic en **crear regla de alerta** para finalizar.

![Crear regla de alerta acabado](media/hdinsight-cluster-availability/portal-create-alert-rule-finish.png)

> [!TIP]
> La capacidad de especificar **gravedad** es una herramienta eficaz que puede usarse al crear varias alertas. Por ejemplo, podría crear una alerta para generar una advertencia (gravedad 1) si un único nodo principal se queda inactivo y otra alerta que provoca crítico (gravedad 0) en el improbable caso de que ambos nodos principales dejan de funcionar.

Cuando se cumple la condición de esta alerta, se activará la alerta y recibirá un correo electrónico con los detalles de alerta similar al siguiente:

![Correo electrónico de alerta Monitor Azure](media/hdinsight-cluster-availability/alert-email.png)

También puede ver todas las alertas activadas, agrupadas por gravedad, yendo a la **alertas** hoja en su **el área de trabajo de Log Analytics**.

![Alertas de log Analytics del área de trabajo](media/hdinsight-cluster-availability/portal-alerts.png)

Al hacer clic en una agrupación de gravedad (es decir, **Sev 1,** resaltado anteriormente) mostrará los registros para todas las alertas de ese gravedad activadas como a continuación:

![Alertas de gravedad 1 área de trabajo de análisis de registro](media/hdinsight-cluster-availability/portal-alerts-sev-1.png)

## <a name="next-steps"></a>Pasos siguientes
- [Disponibilidad y confiabilidad de clústeres de Apache Hadoop en HDInsight](hdinsight-high-availability-linux.md)
