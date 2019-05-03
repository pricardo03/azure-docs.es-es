---
title: Procedimiento de conmutación por error HANA en un sitio ante desastres para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Cómo realizar la conmutación por error a un sitio de recuperación ante desastres para SAP HANA en Azure (instancias grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f7d4f6216b4a57796ab5c0296713316dd97c47a8
ms.sourcegitcommit: 60606c5e9a20b2906f6b6e3a3ddbcb6c826962d6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2019
ms.locfileid: "64987886"
---
# <a name="disaster-recovery-failover-procedure"></a>Procedimiento de conmutación por error de recuperación ante desastres


>[!IMPORTANT]
>En este artículo no es un reemplazo para la documentación de administración de SAP HANA o notas de SAP. Esperamos que tienen un conocimiento sólido de y experiencia en administración de SAP HANA y las operaciones, especialmente para la copia de seguridad, restauración, alta disponibilidad y recuperación ante desastres (DR). En este artículo, se muestran capturas de pantalla de SAP HANA Studio. El contenido, la estructura y la naturaleza de las pantallas de las herramientas de administración de SAP y de las herramientas en sí pueden cambiar de una versión a otra de SAP HANA.

Hay dos casos a tener en cuenta al conmutar por error a un sitio de recuperación ante desastres:

- Necesita que la base de datos de SAP HANA vuelva al estado más reciente de los datos. En este caso, hay un script de autoservicio con la que puede realizar la conmutación por error sin necesidad de ponerse en contacto con Microsoft. Para la conmutación por recuperación, deberá trabajar con Microsoft.
- Desea restaurar una instantánea de almacenamiento que no sea la última instantánea replicada. En ese caso, tendrá que colaborar con Microsoft. 

>[!NOTE]
>Los pasos siguientes deben realizarse en la unidad de instancia grande de HANA, que representa la unidad de recuperación ante desastres. 
 
Para restaurar a las instantáneas de almacenamiento replicado más recientes, siga los pasos descritos en "Full de realizar conmutación por error de recuperación ante desastres - azure_hana_dr_failover" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

Si desea tener varias instancias de SAP HANA conmutación por error, ejecute el comando azure_hana_dr_failover varias veces. Cuando se solicite, escriba al SID de SAP HANA que desea conmutar por error y restaurar. 


También puede probar la conmutación por error de recuperación ante desastres sin que afecte a la relación de replicación real. Para llevar a cabo una conmutación por error de prueba, siga los pasos descritos en "Realizar una prueba de conmutación por error de recuperación ante desastres: azure_hana_test_dr_failover" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

>[!IMPORTANT]
>Hacer *no* ejecutar las transacciones de producción en la instancia que creó en el sitio de recuperación ante desastres a través del proceso de **probar una conmutación por error**. El comando azure_hana_test_dr_failover crea un conjunto de volúmenes que no tienen relación con el sitio primario. Como consecuencia, *no* se puede realizar una sincronización al sitio principal. 

Si desea tener varias instancias de SAP HANA para probar, ejecute el script varias veces. Cuando se solicite, escriba al SID de SAP HANA de la instancia que desea probar la conmutación por error. 

>[!NOTE]
>Si necesita conmutar por error en el sitio de recuperación ante desastres para recuperar algunos datos que se eliminaron hace horas y necesita los volúmenes de recuperación ante desastres se configuren a una instantánea anterior, este procedimiento se aplica. 

1. Apagar la instancia de no producción de HANA en la unidad de recuperación ante desastres de instancias grandes de HANA que se está ejecutando. Una instancia de producción de HANA inactiva está preinstalada.
1. Asegúrese de que no hay ningún proceso de SAP HANA en ejecución. Use el comando siguiente para realizar esta comprobación:

      `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`.

      El resultado debería mostrar el proceso **hdbdaemon** en estado detenido y ningún otro proceso de HANA en ejecución o iniciado.
1. Determine en qué nombre de instantánea o identificador de copia de seguridad de SAP HANA desea que se restaure el tenga el sitio de recuperación ante desastres. En casos reales de recuperación ante desastres, esta instantánea suele ser la instantánea más reciente. Si necesita recuperar datos perdidos, elija una instantánea anterior.
1. Póngase en contacto con el soporte técnico de Azure a través de una solicitud de soporte técnico de alta prioridad. Solicitar la restauración de esa instantánea con el nombre y la fecha de la instantánea o el identificador de copia de seguridad de HANA en el sitio de recuperación ante desastres. El valor predeterminado es que las operaciones solo restauran el volumen /hana/data. Si desea tener también los volúmenes/hana/logbackups, debe indicarlo específicamente. *No restaure el volumen /hana/shared.* En su lugar, elegir archivos específicos, como global.ini fuera de la **.snapshot** directorio y sus subdirectorios después de volver a montar/hana/volumen shared para PRD. 

   En lo que respecta a las operaciones, se realizan los pasos siguientes:

    a. Se detiene la replicación de instantáneas del volumen de producción en los volúmenes de recuperación ante desastres. Esto podría haber ocurrido ya si una interrupción en el sitio de producción es el motivo que necesita para realizar una recuperación ante desastres.
   
   b. El nombre de la instantánea de almacenamiento o la instantánea con el identificador de copia de seguridad que eligió se restaura en los volúmenes de recuperación ante desastres.
   
   c. Después de la restauración, los volúmenes de recuperación ante desastres están disponibles para montarlos en las unidades de HANA (instancias grandes) en la región de recuperación ante desastres.
      
1. Monte los volúmenes de recuperación ante desastres en la unidad de HANA (instancias grandes) en el sitio de recuperación ante desastres. 
1. Inicie la instancia de producción de SAP HANA inactiva.
1. Si eligió copiar registros de copia de seguridad del registro de transacciones para reducir el tiempo de RPO, combine las copias de seguridad del registro de transacciones en el directorio montado recién recuperación ante desastres/hana/logbackups. No sobrescriba copias de seguridad existentes. Copie las copias de seguridad más recientes que no se han replicado con la última replicación de una instantánea de almacenamiento.
1. También puede restaurar archivos individuales de las instantáneas que no se han replicado en el volumen /hana/shared/PRD en la región de Azure de recuperación ante desastres.

Los pasos siguientes muestran cómo recuperar la instancia de producción de SAP HANA según la instantánea de almacenamiento restaurada y las copias de seguridad del registro de transacciones que están disponibles.

1. Cambie la ubicación de copia de seguridad a **/hana/logbackups** con SAP HANA Studio.

   ![Cambiar la ubicación de copia de seguridad para recuperación de DR](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. SAP HANA examina las ubicaciones del archivo de copia de seguridad y sugiere la copia de seguridad del registro de transacciones más reciente para restaurar. El examen puede tardar unos minutos hasta que una pantalla similar a aparece lo siguiente:

   ![Lista de copias de seguridad de registro de transacciones para recuperación de DR](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Ajuste algunas de las configuraciones predeterminadas:

      - Desactive la casilla **Use Delta Backups** (Usar copias de seguridad diferenciales).
      - Seleccione **Initialize Log Area** (Inicializar el área de registro).

   ![Establecer la inicialización del área de registro](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. Seleccione **Finalizar**.

   ![Finalizar la restauración de DR](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Debería aparecer una ventana de progreso, como la que se muestra aquí. Recuerde que el ejemplo trata de una restauración de recuperación ante desastres de una configuración de SAP HANA de escalado horizontal de tres nodos.

![Progreso de la restauración](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Si la restauración deja de responder en el **finalizar** y no muestra la pantalla de progreso, confirme que todas las instancias de SAP HANA en los nodos de trabajo se está ejecutando. Si es necesario, inicie manualmente las instancias de SAP HANA.


## <a name="failback-from-a-dr-to-a-production-site"></a>Conmutación por recuperación desde un sitio de recuperación ante desastres a uno de producción
Puede conmutar por recuperación desde DR a un sitio de producción. Echemos un vistazo a un escenario en el que la conmutación por error en el sitio de recuperación ante desastres se debió a problemas en la región de Azure de producción, no a que el usuario necesitara recuperar datos perdidos. 

Ha estado ejecutando la carga de trabajo de producción de SAP durante un tiempo en el sitio de recuperación ante desastres. Una vez que se resuelven los problemas en el sitio de producción, es posible que el usuario quiera realizar la conmutación por recuperación con el sitio de producción. Como no se pueden perder datos, el paso de regreso al sitio de producción implica varios pasos y una cooperación estrecha con SAP HANA en el equipo de operaciones de Azure. Es tarea del usuario desencadenar el equipo de operaciones para iniciar la sincronización de vuelta al sitio de producción una vez que se hayan resuelto los problemas.

Siga estos pasos:

1. SAP HANA en el equipo de operaciones de Azure obtiene el desencadenador para sincronizar los volúmenes de almacenamiento de producción desde los volúmenes de almacenamiento de recuperación ante desastres, que ahora representan el estado de la producción. En este estado, la unidad de instancia grande de HANA en el sitio de producción está apagada.
1. SAP HANA en el equipo de operaciones de Azure supervisa la replicación y se asegura de que se actualiza antes de que le informan.
1. El usuario apaga las aplicaciones que usan la instancia de HANA de producción en el sitio de recuperación ante desastres. Después, realiza una copia de seguridad del registro de transacciones de HANA. A continuación, detenga la instancia de HANA que se está ejecutando en las unidades de instancia grande de HANA en el sitio de recuperación ante desastres.
1. Una vez que se apaga la instancia de HANA que se ejecuta en la unidad de instancia grande de HANA en el sitio de recuperación ante desastres, el equipo de operaciones vuelve a sincroniza manualmente los volúmenes de disco.
1. SAP HANA en el equipo de operaciones de Azure comienza la unidad de instancia grande de HANA en el sitio de producción de nuevo. Entregará al le. Asegúrese de que la instancia de SAP HANA está en un estado de cierre en el momento de inicio de la unidad de instancia grande de HANA.
1. Realice los mismos pasos de restauración de base de datos que indicó cuando anteriormente conmutó por error para el sitio de recuperación ante desastres.

## <a name="monitor-disaster-recovery-replication"></a>Supervisión de la replicación de recuperación ante desastres

Para supervisar el estado de su progreso de la replicación de almacenamiento, ejecute el script `azure_hana_replication_status`. Este comando se debe ejecutar desde una unidad que se ejecuta en la ubicación de recuperación ante desastres funcionando según lo previsto. El comando funciona independientemente de si la replicación está activa. El comando se puede ejecutar para cada unidad de instancia grande de HANA del inquilino en la ubicación de recuperación ante desastres. No se puede usar para obtener detalles sobre el volumen de arranque. 

Para obtener más información sobre el comando y su salida, vea "Obtener estado de replicación de recuperación ante desastres - azure_hana_replication_status" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


## <a name="next-steps"></a>Pasos siguientes
- Consulte [supervisar y solucionar problemas del lado HANA](hana-monitor-troubleshoot.md).
