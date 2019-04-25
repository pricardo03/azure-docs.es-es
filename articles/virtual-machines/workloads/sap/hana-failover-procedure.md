---
title: Procedimiento de conmutación por error de HANA en el sitio de recuperación ante desastres para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Cómo realizar la conmutación por error en el sitio de recuperación ante desastres para SAP HANA en Azure (instancias grandes)
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
ms.date: 09/10/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca4d5912d75dd7b33737f61737a209284b7a5a47
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60338553"
---
# <a name="disaster-recovery-failover-procedure"></a>Procedimiento de conmutación por error de recuperación ante desastres


>[!IMPORTANT]
>Esta documentación no sustituye a la documentación de administración de SAP HANA ni a las notas de SAP. Se espera que el lector tenga unos sólidos conocimientos de la administración y operaciones de SAP HANA, sobre todo en los temas relativos a la copia de seguridad, restauración, alta disponibilidad y recuperación ante desastres. En esta documentación, se muestran capturas de pantalla de SAP HANA Studio. El contenido, la estructura y la naturaleza de las pantallas de las herramientas de administración de SAP y de las herramientas en sí pueden cambiar de una versión a otra de SAP HANA.

Hay dos casos que se deben tener en cuenta cuando se realiza la conmutación por error al sitio de recuperación ante desastres:

- Necesita que la base de datos de SAP HANA vuelva al estado más reciente de los datos. En este caso, hay un script de autoservicio con el que se puede realizar la conmutación por error sin necesidad de ponerse en contacto con Microsoft. Sin embargo, para la conmutación por recuperación, es preciso colaborar con Microsoft.
- Si desea realizar la restauración en una instantánea de almacenamiento que no sea la última instantánea replicada. En ese caso, tendrá que colaborar con Microsoft. 

>[!NOTE]
>Los siguientes pasos se deben ejecutar en la unidad de HANA (instancia grande), que representa la unidad de recuperación ante desastres. 
 
Para restaurar a las instantáneas de almacenamiento replicado más recientes, siga estos pasos: 

1. Apague la instancia ajena a producción de HANA en la unidad de recuperación ante desastres de HANA (instancias grandes) que está ejecutando. Eso se debe a que hay instalada previamente una instancia de producción de HANA inactiva.
1. Asegúrese de que no hay ningún proceso de SAP HANA en ejecución. Utilice el siguiente comando para esta comprobación: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. La salida debería mostrarle el proceso **hdbdaemon** en estado detenido y ningún otro proceso de HANA en ejecución o iniciado.
1. En la unidad de HANA (instancias grandes) correspondiente al sitio de recuperación ante desastres, ejecute el script *azure_hana_dr_failover.pl*. El script solicita la restauración de un SID de SAP HANA. Cuando se le solicite, escriba un SID (o el único) de SAP HANA que se haya replicado y que se mantiene en el archivo *HANABackupCustomerDetails.txt* de la unidad de HANA (instancias grandes) en el sitio de recuperación ante desastres. 

      Si desea conmutar por error varias instancias de SAP HANA, debe ejecutar el script varias veces. Cuando se solicite, escriba el SID de SAP HANA que desea conmutar por error y restaurar. Al finalizar, el script muestra una lista de puntos de montaje de los volúmenes que se agregan a la unidad de HANA (instancias grandes). Esta lista incluye también los volúmenes de recuperación ante desastres restaurados.

1. Monte los volúmenes de recuperación ante desastres restaurados mediante comandos del sistema operativo Linux en la unidad de HANA (instancias grandes) del sitio de recuperación ante desastres. 
1. Inicie la instancia de producción de SAP HANA inactiva.
1. Si eligió copiar registros de la copia de seguridad del registro de transacciones para reducir el tiempo de RPO, debe combinar dichas copias de seguridad del registro de transacciones en el directorio /hana/logbackups de recuperación ante desastres que se acaba de montar. No sobrescriba copias de seguridad existentes. Copie las copias de seguridad más recientes que no se hayan replicado con la última replicación de una instantánea de almacenamiento.
1. También puede restaurar archivos individuales de las instantáneas que se han replicado en el volumen /hana/shared/PRD en la región de Azure de recuperación ante desastres. 

También puede probar la conmutación por error de recuperación ante desastres sin que afecte a la relación de replicación real. Para llevar a cabo una conmutación por error de prueba, siga los pasos anteriores 1 y 2 y, después, vaya al paso 3.

>[!IMPORTANT]
>*No* ejecute transacciones de producción en la instancia que ha creado en el sitio de recuperación ante desastres a través del proceso de **prueba de una conmutación por error** con el script que se ha introducido en el paso 3. Ese comando crea un conjunto de volúmenes que no tienen relación con el sitio principal. Como consecuencia, *no* se puede realizar una sincronización al sitio principal. 

Paso 3 para la prueba de conmutación por error:

En la unidad de HANA (instancias grandes) correspondiente al sitio de recuperación ante desastres, ejecute el script **azure_hana_test_dr_failover.pl**. Este script *no* detiene la relación de replicación entre el sitio principal y el de recuperación ante desastres. En su lugar, está clonando los volúmenes de almacenamiento de recuperación ante desastres. Cuando el proceso de clonación se realice correctamente, los volúmenes clonados se restauran al estado de la instantánea más reciente y, a continuación, se monta en la unidad de recuperación ante desastres. El script solicita la restauración de un SID de SAP HANA. Escriba un SID de SAP HANA que se haya replicado y que se mantenga en el archivo *HANABackupCustomerDetails.txt* de la unidad de instancia grande de HANA en el sitio de recuperación ante desastres. 

Si desea poder probar varias instancias de SAP HANA, debe ejecutar el script varias veces. Cuando se solicite, escriba en el SID de SAP HANA de la instancia cuya conmutación por error desea probar. Al finalizar, el script muestra una lista de los puntos de montaje de los volúmenes que se agregan a la unidad de HANA (instancias grandes). Esta lista incluye también los volúmenes de recuperación ante desastres clonados.

Diríjase al paso 4.

   >[!NOTE]
   >Este procedimiento es válido si necesita conmutar por error al sitio de recuperación ante desastres para recuperar algunos datos que se eliminaron hace horas y necesita que los volúmenes de recuperación ante desastres se configuren a una instantánea anterior. 

1. Apague la instancia ajena a producción de HANA en la unidad de recuperación ante desastres de HANA (instancias grandes) que está ejecutando. Eso se debe a que hay instalada previamente una instancia de producción de HANA inactiva.
1. Asegúrese de que no hay ningún proceso de SAP HANA en ejecución. Utilice el siguiente comando para esta comprobación: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. El resultado debería mostrar el proceso **hdbdaemon** en estado detenido y ningún otro proceso de HANA en ejecución o iniciado.
1. Determine en qué nombre de instantánea o identificador de copia de seguridad de SAP HANA desea que se restaure el tenga el sitio de recuperación ante desastres. En casos reales de recuperación ante desastres, esta instantánea suele ser la instantánea más reciente. Si necesita recuperar datos perdidos, elija una instantánea anterior.
1. Póngase en contacto con el soporte técnico de Azure a través de una solicitud de soporte técnico de alta prioridad. Solicite la restauración de dicha instantánea (con el nombre y la fecha de la misma) o el identificador de copia de seguridad de HANA en el sitio de recuperación ante desastres. El valor predeterminado es que las operaciones solo restauran el volumen /hana/data. Si también desea tener los volúmenes /hana/logbackups, debe indicarlo específicamente. *No restaure el volumen /hana/shared.* En su lugar, debe elegir archivos específicos, como global.ini del directorio **.snapshot** y sus subdirectorios después de volver a montar el volumen /hana/shared para PRD. 

   En lo que respecta a las operaciones, se realizan los pasos siguientes:

    a. Se detiene la replicación de instantáneas del volumen de producción en los volúmenes de recuperación ante desastres. Esto podría haber ocurrido ya si una interrupción en el sitio de producción es el motivo que necesita para realizar una recuperación ante desastres.
   
   b. El nombre de la instantánea de almacenamiento o la instantánea con el identificador de copia de seguridad que eligió se restaura en los volúmenes de recuperación ante desastres.
   
   c. Después de la restauración, los volúmenes de recuperación ante desastres están disponibles para montarlos en las unidades de HANA (instancias grandes) en la región de recuperación ante desastres.
      
1. Monte los volúmenes de recuperación ante desastres en la unidad de HANA (instancias grandes) en el sitio de recuperación ante desastres. 
1. Inicie la instancia de producción de SAP HANA inactiva.
1. Si eligió copiar registros de la copia de seguridad del registro de transacciones para reducir el tiempo de RPO, debe combinar dichas copias de seguridad del registro de transacciones en el directorio /hana/logbackups de recuperación ante desastres que se acaba de montar. No sobrescriba copias de seguridad existentes. Copie las copias de seguridad más recientes que no se hayan replicado con la última replicación de una instantánea de almacenamiento.
1. También puede restaurar archivos individuales de las instantáneas que se han replicado en el volumen /hana/shared/PRD en la región de Azure de recuperación ante desastres.

La secuencia de pasos siguiente implica la recuperación de la instancia de producción de SAP HANA según la instantánea de almacenamiento restaurada y las copias de seguridad del registro de transacciones que están disponibles:

1. Cambie la ubicación de copia de seguridad a **/hana/logbackups** con SAP HANA Studio.
   ![Cambiar la ubicación de copia de seguridad para la recuperación de DR](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. SAP HANA examina las ubicaciones del archivo de copia de seguridad y sugiere la copia de seguridad del registro de transacciones más reciente para restaurar. El examen puede tardar unos minutos hasta que una pantalla similar a aparece lo siguiente: ![Lista de copias de seguridad de registro de transacciones para recuperación de DR](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Ajuste algunas de las configuraciones predeterminadas:

      - Desactive la casilla **Use Delta Backups** (Usar copias de seguridad diferenciales).
      - Seleccione **Initialize Log Area** (Inicializar el área de registro).

   ![Establecer la inicialización del área de registro](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. Seleccione **Finalizar**.

   ![Finalizar la restauración de DR](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Debería aparecer una ventana de progreso, como la que se muestra aquí. Recuerde que el ejemplo trata de una restauración de recuperación ante desastres de una configuración de SAP HANA de escalado horizontal de tres nodos.

![Progreso de la restauración](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Si parece que la restauración deja de responder en el **finalizar** y no muestra la pantalla de progreso, confirme que todas las instancias de SAP HANA en los nodos de trabajo se está ejecutando. Si es necesario, inicie manualmente las instancias de SAP HANA.


## <a name="failback-from-a-dr-to-a-production-site"></a>Conmutación por recuperación desde un sitio de recuperación ante desastres a uno de producción
Puede conmutar por recuperación desde DR a un sitio de producción. Echemos un vistazo a un escenario en el que la conmutación por error en el sitio de recuperación ante desastres se debió a problemas en la región de Azure de producción, no a que el usuario necesitara recuperar datos perdidos. La carga de trabajo de producción de SAP lleva un tiempo ejecutándose en el sitio de recuperación ante desastres. Una vez que se resuelven los problemas en el sitio de producción, es posible que el usuario quiera realizar la conmutación por recuperación con el sitio de producción. Como no se pueden perder datos, el paso de regreso al sitio de producción implica varios pasos y una cooperación estrecha con SAP HANA en el equipo de operaciones de Azure. Es tarea del usuario desencadenar el equipo de operaciones para iniciar la sincronización de vuelta al sitio de producción una vez que se hayan resuelto los problemas.

Esta es la secuencia de pasos a seguir:

1. SAP HANA en el equipo de operaciones de Azure obtiene el desencadenador para sincronizar los volúmenes de almacenamiento de producción desde los volúmenes de almacenamiento de recuperación ante desastres, que ahora representan el estado de la producción. En este estado, la unidad de instancia grande de HANA en el sitio de producción está apagada.
1. SAP HANA en el equipo de operaciones de Azure supervisa la replicación y garantiza que se actualiza antes de informar al usuario.
1. El usuario apaga las aplicaciones que usan la instancia de HANA de producción en el sitio de recuperación ante desastres. Después, realiza una copia de seguridad del registro de transacciones de HANA. Luego, detiene la instancia de HANA que se ejecuta en las unidades de HANA (instancias grandes) en el sitio de recuperación ante desastres.
1. Una vez que se apaga la instancia de HANA que se ejecuta en la unidad de HANA (instancias grandes) del sitio de recuperación ante desastres, el equipo de operaciones vuelve a sincronizar manualmente los volúmenes de disco.
1. SAP HANA en el equipo de operaciones de Azure inicia de nuevo la unidad de instancia grande de HANA en el sitio de producción y se la entrega al usuario. Asegúrese de que la instancia de SAP HANA esté apagada en el momento en que se inicia la unidad HANA (instancias grandes).
1. El usuario realiza los mismos pasos de la restauración de la base de datos que siguió cuando se realizó la conmutación por error al sitio de recuperación ante desastres anteriormente.

## <a name="monitor-disaster-recovery-replication"></a>Supervisión de la replicación de recuperación ante desastres

Puede supervisar el estado del progreso de la replicación de almacenamiento ejecutando el script `azure_hana_replication_status.pl`. Para que funcione como cabría esperar, este script se debe ejecutar desde una unidad que ejecute en la ubicación de recuperación ante desastres. El script funciona, independientemente de si la replicación está activa o no. El script se puede ejecutar para cada unidad de instancia grande de HANA del inquilino en la ubicación de recuperación ante desastres. No se puede usar para obtener detalles sobre el volumen de arranque.

Llame al script con este comando:
```
./azure_hana_replication_status.pl
```

La salida se desglosa por volumen en las secciones siguientes:  

- Estado del vínculo
- Actividad de replicación actual
- Instantánea replicada más reciente 
- Tamaño de la instantánea más reciente
- Tiempo de retardo actual entre instantáneas (entre la última replicación de instantánea completada y el instante actual)

El estado del vínculo se muestra como **Activo**, salvo que el vínculo entre las ubicaciones esté inactivo o que haya un evento de conmutación por error en curso. La actividad de replicación controla si algún dato se está replicando actualmente o si está inactivo, o si hay alguna otra actividad actualmente en el vínculo. La última instantánea replicada solo debe aparecer como `snapmirror…`. Luego se muestra el tamaño de la última instantánea. Por último, se muestra el tiempo de retardo. El tiempo de retardo representa el tiempo que transcurre desde la replicación programada hasta que finaliza la replicación. Un tiempo de retardo puede ser más de una hora para la replicación de datos, en especial si se trata de la replicación inicial, incluso si se inició la replicación. El tiempo de retardo sigue aumentando hasta que finalice la replicación en curso.

Este es un ejemplo de la salida:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```

**Pasos siguientes**
- Consulte [Supervisión y solución de problemas en el lado de HANA](hana-monitor-troubleshoot.md).
