---
title: Administración de actualizaciones para varias máquinas virtuales de Azure
description: En este artículo se describe cómo administrar actualizaciones para máquinas virtuales de Azure.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 01/10/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3897225ef6ed7fcc0db75e82058e5b5b273ccbd4
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214035"
---
# <a name="manage-updates-for-multiple-machines"></a>Administración de actualizaciones para varias máquinas

Puede usar la solución Update Management para administrar las actualizaciones y las revisiones de las máquinas Windows y Linux. Desde la cuenta de [Azure Automation](automation-offering-get-started.md), puede:

- Incorporar máquinas virtuales
- Evaluar el estado de las actualizaciones disponibles
- Programar la instalación de las actualizaciones necesarias
- Revisar los resultados de la implementación para comprobar que se han aplicado correctamente actualizaciones a todas las máquinas virtuales para las cuales se ha habilitado Update Management

## <a name="prerequisites"></a>Requisitos previos

Para usar Update Management, necesita lo siguiente:

- Una cuenta de ejecución de Azure Automation Para saber cómo crear una, consulte [Introducción a Azure Automation](automation-offering-get-started.md).
- Una máquina virtual o un equipo con uno de los sistemas operativos admitidos instalado.

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

Update Management es compatible con los sistemas operativos siguientes:

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Solo admite evaluaciones de actualización.         |
|Windows Server 2008 R2 SP1 y posterior     |Se requiere Windows PowerShell 4.0 o posterior. ([Descargar WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))</br> Se recomienda Windows PowerShell 5.1 para aumentar la confiabilidad. ([Descargar WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) y 7 (x64)      | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|Red Hat Enterprise (x86/x64) 6 y 7 (x64)     | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|SUSE Linux Enterprise Server 11 (x86/x64) y 12 (x64)     | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|Ubuntu 14.04 LTS, 16.04 y 18.04 LTS (x86/x64)      |Los agentes de Linux deben tener acceso a un repositorio de actualización.         |

> [!NOTE]
> Para evitar que las actualizaciones se apliquen fuera de una ventana de mantenimiento en Ubuntu, vuelva a configurar el paquete de actualizaciones desatendidas para deshabilitar las actualizaciones automáticas. Para más información, consulte el [tema sobre actualizaciones automáticas en la guía de Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Los agentes de Linux deben tener acceso a un repositorio de actualización.

Esta solución no es compatible con un agente de Log Analytics para Linux que esté configurado para informar a varias áreas de trabajo de Azure Log Analytics.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Habilitación de Update Management en máquinas virtuales de Azure

En Azure Portal, abra su cuenta de Automation y seleccione **Update Management**.

Seleccione **Agregar máquina virtual de Azure**.

![Pestaña Agregar máquina virtual de Azure](./media/manage-update-multi/update-onboard-vm.png)

Seleccione la máquina virtual que desea incorporar. 

En **Habilitar la administración de actualizaciones**, seleccione **Habilitar** para incorporar la máquina virtual.

![Cuadro de diálogo Habilitar la administración de actualizaciones](./media/manage-update-multi/update-enable.png)

Una vez que se completa la incorporación, Update Management está habilitado para su máquina virtual.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Habilitación de Update Management en equipos y máquinas virtuales que no son de Azure

Para saber cómo habilitar Update Management en equipos y máquinas virtuales Windows que no sean de Azure, consulte [Conexión de equipos Windows al servicio Azure Log Analytics](../log-analytics/log-analytics-windows-agent.md).

Para saber cómo habilitar Update Management en equipos y máquinas virtuales Linux que no sean de Azure, consulte [Conexión de equipos con Linux a Log Analytics](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Visualización de equipos conectados a la cuenta de Automation

Después de habilitar Update Management en las máquinas, puede ver información sobre ellas si selecciona **Equipos**. Encontrará información sobre el *nombre de la máquina*, el *estado de cumplimiento*, el *entorno*, el *tipo de sistema operativo*, las *actualizaciones críticas y de seguridad instaladas*, *otras actualizaciones instaladas* y la *preparación del agente de actualización* de los equipos.

  ![Pestaña en la que se ven los equipos](./media/manage-update-multi/update-computers-tab.png)

Es posible que los equipos que no se hayan habilitado recientemente para Update Management no se hayan evaluado aún. En este caso, el estado de cumplimiento de esos equipos es **No evaluado**. Esta es una lista de valores posibles de estado de cumplimiento:

- **Compatible**: equipos que no tienen actualizaciones críticas o de seguridad pendientes.

- **No compatible**: equipos que no tienen al menos una actualización crítica o de seguridad pendiente.

- **No evaluado**: los datos de valoración de las actualizaciones no se han recibido del equipo en el período esperado. En equipos Linux, el período de tiempo esperado está en las 3 últimas horas. En equipos Windows, el período de tiempo esperado está en las 12 últimas horas.

Para ver el estado del agente, seleccione el vínculo de la columna **Preparación de actualizaciones del agente**. Al seleccionar esta opción se abre el panel **Hybrid Worker**, que muestra el estado de Hybrid Worker. En la siguiente imagen se muestra un ejemplo de un agente que no se ha conectado a Update Management durante un período de tiempo prolongado:

![Pestaña en la que se ven los equipos](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Visualización de la evaluación de la actualización

Una vez habilitado Update Management, se abre el panel **Update Management**. Puede ver una lista de las actualizaciones que faltan en la pestaña **Actualizaciones que faltan**.

## <a name="collect-data"></a>Recopilación de datos

Los agentes que están instalados en máquinas virtuales y equipos recopilan datos acerca de las actualizaciones. Los agentes envían los datos a Azure Update Management.

### <a name="supported-agents"></a>Agentes admitidos

En la tabla siguiente se describen los orígenes conectados compatibles con esta solución:

| Origen conectado | Compatible | DESCRIPCIÓN |
| --- | --- | --- |
| Agentes de Windows |SÍ |Update Management recopila información sobre las actualizaciones del sistema de los agentes de Windows e inicia la instalación de las actualizaciones necesarias. |
| Agentes de Linux |SÍ |Update Management recopila información sobre las actualizaciones del sistema de los agentes de Linux e inicia la instalación de las actualizaciones necesarias en las distribuciones admitidas. |
| Grupo de administración de Operations Manager |SÍ |Update Management recopila información sobre las actualizaciones del sistema de los agentes de un grupo de administración conectado. |
| Cuenta de Azure Storage |Sin  |Azure Storage no incluye información sobre las actualizaciones del sistema. |

### <a name="collection-frequency"></a>Frecuencia de recopilación

Después de que un equipo completa un examen de cumplimiento de las actualizaciones, el agente reenvía la información de forma masiva a Azure Log Analytics. En un equipo Windows, el examen de cumplimiento se ejecuta cada 12 horas de forma predeterminada.

Además del examen programado, el examen de cumplimiento de las actualizaciones se inicia a los 15 minutos del reinicio de MMA, así como antes y después de la instalación de actualizaciones.

En un equipo Linux, el examen de cumplimiento se realiza cada 3 horas de forma predeterminada. Si se reinicia el agente MMA, se inicia un examen de cumplimiento al cabo de 15 minutos.

Puede que transcurran entre 30 minutos y 6 horas antes de que se muestren los datos actualizados de los equipos administrados.

## <a name="schedule-an-update-deployment"></a>Programación de una implementación de actualizaciones

Para instalar actualizaciones, programe una implementación que esté en consonancia con su ventana de programación y servicio de versiones. Puede elegir los tipos de actualizaciones que quiere incluir en la implementación. Por ejemplo, puede incluir actualizaciones de seguridad o críticas y excluir paquetes acumulativos de actualizaciones.

Para programar una nueva implementación de actualizaciones de una o varias máquinas virtuales, en **Update Management**, seleccione **Programar implementación de actualizaciones**.

En el panel **Nueva implementación de actualización**, especifique la siguiente información:

- **Nombre**: proporcione un nombre único para identificar la implementación de actualizaciones.
- **Sistema operativo**: seleccione **Windows** o **Linux**.
- **Grupos que se deben actualizar (versión preliminar)**: Defina una consulta basada en una combinación de suscripción, grupos de recursos, ubicaciones y etiquetas para crear un grupo dinámico de VM de Azure e incluirlo en la implementación. Para obtener más información, consulte [Dynamic Groups](automation-update-management.md#using-dynamic-groups) (Grupos dinámicos).
- **Máquinas para actualizar**: seleccione una Búsqueda guardada, Grupo importado o Máquinas para elegir las máquinas que desea actualizar. Si elige **Máquinas**, la preparación de la máquina se muestra en la columna **PREPARACIÓN DE ACTUALIZACIONES DEL AGENTE**. Puede ver el estado de mantenimiento de la máquina antes de programar la implementación de actualizaciones. Para obtener información sobre los distintos métodos de creación de grupos de equipos en Log Analytics, consulte [Grupos de equipos en búsquedas de registros en Log Analytics](../azure-monitor/platform/computer-groups.md)

  ![Panel Nueva implementación de actualización](./media/manage-update-multi/update-select-computers.png)

- **Actualizar clasificación**: seleccione los tipos de software que se incluirán en la implementación de actualizaciones. Para ver una descripción de los tipos de clasificación, consulte [Actualización de clasificaciones](automation-update-management.md#update-classifications). Los tipos de clasificación son:
  - Actualizaciones críticas
  - Actualizaciones de seguridad
  - Paquetes acumulativos de actualizaciones
  - Feature Packs
  - Service Packs
  - Actualizaciones de definiciones
  - Herramientas
  - Actualizaciones

- **Actualizaciones para incluir/excluir**: abre la página para **incluir/excluir**. Las actualizaciones que se incluirán o excluirán están en pestañas independientes. Para obtener más información sobre cómo se controla la inclusión, consulte la sección [Inclusion behavior](automation-update-management.md#inclusion-behavior) (Comportamiento de la inclusión).

- **Configuración de la programación**: puede aceptar la fecha y hora predeterminadas, que son 30 minutos después de la hora actual. También puede especificar una hora distinta.

   También puede especificar si la implementación se produce una vez o configurar una programación periódica. Para configurar una programación periódica, en **Periodicidad**, seleccione **Periódica**.

   ![Cuadro de diálogo Configuración de programación](./media/manage-update-multi/update-set-schedule.png)

- **Scripts previos + scripts posteriores**: seleccione los scripts que se ejecutarán antes y después de la implementación. Para obtener más información, consulte [Administración de scripts previos y posteriores](pre-post-scripts.md).
- **Ventana de mantenimiento (minutos)**: especifique el período de tiempo en el que desea que se produzca la implementación de actualizaciones. Esta configuración ayuda a garantizar que los cambios se realizan en las ventanas de servicio definidas.

- **Reboot control**: esta configuración determina cómo se controlan los reinicios para la implementación de actualizaciones.

   |Opción|DESCRIPCIÓN|
   |---|---|
   |Reiniciar si es necesario| **(Valor predeterminado)** En caso necesario, se inicia un reinicio cuando la ventana de mantenimiento lo permite.|
   |Always reboot (Reiniciar siempre)|Se inicia un reinicio, independientemente de si se necesita. |
   |Never reboot (No reiniciar nunca)|Independientemente de que se requiera un reinicio, los reinicios se suprimen.|
   |Only reboot (solo reiniciar), no se instalarán las actualizaciones|Esta opción omite la instalación de actualizaciones y solo inicia un reinicio.|

Cuando haya terminado de configurar la programación, seleccione el botón **Crear** para volver al panel de estado. En la tabla **Programada** se muestra la programación de implementaciones que ha creado.

> [!NOTE]
> Update Management admite la implementación de actualizaciones de origen y la descarga previa de revisiones. Esto requiere cambios en los sistemas a los que se aplican revisiones. Para aprender a configurar estos valores en sus sistemas, consulte [soporte técnico para aplicación de revisiones propias y para descargas previas](automation-update-management.md#firstparty-predownload).

## <a name="view-results-of-an-update-deployment"></a>Visualización de los resultados de una implementación de actualizaciones

Después de que se inicie la implementación programada, puede ver su estado en la pestaña **Implementaciones de actualizaciones** en **Update Management**.

Si la implementación se está ejecutando, su estado es **En curso**. Cuando la implementación finaliza correctamente, el estado cambia a **Correcto**.

Si se produce un error en una o varias actualizaciones de la implementación, el estado es **Error parcial**.

![Estado de la implementación de actualizaciones](./media/manage-update-multi/update-view-results.png)

Para establecer el panel para una implementación de actualizaciones, seleccione la implementación completada.

En el panel **Resultados de actualización** se muestra el número total de actualizaciones y los resultados de la implementación de la máquina virtual. En la tabla de la derecha se muestra un análisis detallado de cada actualización y los resultados de la instalación. Los resultados de la instalación puede ser uno de los siguientes valores:

- **No intentado**: la actualización no se instaló porque no había tiempo disponible suficiente de acuerdo con la ventana de mantenimiento definida.
- **Correcto**: actualización realizada correctamente.
- **Error**: error en la actualización.

Seleccione **Todos los registros** para ver todas las entradas de registro que creó la implementación.

Para ver el flujo de trabajo del runbook que administra la implementación de actualizaciones en la máquina virtual de destino, seleccione el icono de salida.

Seleccione **Errores** para ver información detallada sobre los errores de la implementación.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre Update Management, incluidos registros, salidas y errores, consulte [Solución Update Management en Azure](../operations-management-suite/oms-solution-update-management.md).
