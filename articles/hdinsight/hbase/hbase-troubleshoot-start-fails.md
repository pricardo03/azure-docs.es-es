---
title: Apache HBase Master no se inicia en Azure HDInsight
description: Apache HBase Master (HMaster) no se inicia en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/06/2019
ms.openlocfilehash: 8368ebfca4cdd72c5c455a04e29b6c0cb44938ea
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935455"
---
# <a name="apache-hbase-master-hmaster-fails-to-start-in-azure-hdinsight"></a>Apache HBase Master (HMaster) no se inicia en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="scenario-atomic-renaming-failure"></a>Escenario: Error de cambio de nombre atómico

### <a name="issue"></a>Problema

Archivos inesperados identificados durante el proceso de inicio.

### <a name="cause"></a>Causa

Durante el proceso de inicio, HMaster realiza muchos pasos de inicialización, incluido el movimiento de datos desde la carpeta temporal (.tmp) a la carpeta de datos. HMaster también examina la carpeta de registros de escritura previa (WAL, por sus siglas en inglés) para ver si hay servidores de la región que no responden. En todas estas situaciones, ejecuta un comando `list` básico en estas carpetas. Si en algún momento ve un archivo inesperado en cualquiera de estas carpetas, genera una excepción y, por tanto, no se inicia.

### <a name="resolution"></a>Resolución

En esta situación, compruebe la pila de llamadas para ver qué carpeta podría estar causando el problema (por ejemplo, si es la carpeta de WAL o la carpeta .tmp). A continuación, mediante Cloud Explorer o comandos HDFS, busque el archivo problemático. Suele ser un archivo `*-renamePending.json` (un archivo de diario que se usa para implementar la operación de cambio de nombre atómico en el controlador WASB). Debido a errores en esta implementación, pueden permanecer tales archivos en caso de bloqueo del proceso. Fuerce la eliminación de este archivo mediante Cloud Explorer. Además, podría haber un archivo temporal de naturaleza $ en esta ubicación. El archivo no se puede visualizar mediante Cloud Explorer, solo con el comando `ls` de HDFS. Puede usar el comando `hdfs dfs -rm //\$\$\$.\$\$\$` de HDFS para eliminar este archivo.

Cuando se haya eliminado el archivo problemático, HMaster debería iniciarse inmediatamente.

---

## <a name="scenario-no-server-address-listed"></a>Escenario: No se muestra ninguna dirección de servidor

### <a name="issue"></a>Problema

El registro de HMaster muestra un mensaje de error similar a "No se muestra ninguna dirección de servidor en hbase: meta para la región xxx".

### <a name="cause"></a>Causa

No se pudo inicializar HMaster después de reiniciar HBase.

### <a name="resolution"></a>Resolución

1. Ejecute los siguientes comandos en el shell de HBase (cambie los valores reales según sea aplicable):

    ```
    scan 'hbase:meta'
    delete 'hbase:meta','hbase:backup <region name>','<column name>' 
    ```

1. Elimine la entrada de hbase: espacio de nombres ya que el mismo error puede aparecer al examinar la tabla hbase: espacio de nombres.

1. Reinicie el HMaster activo desde la interfaz de usuario de Ambari para que aparezca en estado de ejecución.

1. Ejecute el siguiente comando en el shell de HBase para activar todas las tablas sin conexión:

    ```
    hbase hbck -ignorePreCheckPermission -fixAssignments
    ```

---

## <a name="scenario-javaioioexception-timedout"></a>Escenario: java.io.IOException: Timedout

### <a name="issue"></a>Problema

HMaster agota el tiempo de espera con una excepción grave, como `java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned`.

### <a name="cause"></a>Causa

El tiempo de espera es un defecto conocido de HMaster. Las tareas de inicio de los clústeres generales pueden tardar mucho tiempo. HMaster se cierra si la tabla de espacios de nombres todavía no está asignada. Las tareas de inicio largas tienen lugar cuando existe una gran cantidad de datos sin vaciar y no basta con un tiempo de espera de cinco minutos.

### <a name="resolution"></a>Resolución

1. Acceda a la interfaz de usuario de Ambari, vaya a HBase -> Configs (Configuraciones) y, en el valor de `hbase-site.xml` personalizado, agregue la siguiente configuración:

    ```
    Key: hbase.master.namespace.init.timeout Value: 2400000  
    ```

1. Reinicie los servicios requeridos (principalmente HMaster y posiblemente otros servicios de HBase).

---

## <a name="scenario-frequent-regionserver-restarts"></a>Escenario: Reinicios frecuentes de regionserver

### <a name="issue"></a>Problema

Los nodos se reinician periódicamente. En los registros de regionserver puede ver entradas parecidas a estas:

```
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
```

### <a name="cause"></a>Causa

Pausa larga de la recolección de elementos no utilizados de Máquina virtual Java en regionserver. La pausa hará que regionserver deje de responder y no pueda enviar latidos a HMaster dentro del tiempo de espera de sesión de Zookeeper de 40 s. HMaster creará que regionserver está inactivo y lo anulará y reiniciará.

### <a name="resolution"></a>Resolución

Cambie el tiempo de espera de sesión de Zookeeper; no solo es necesario cambiar el valor `zookeeper.session.timeout` de hbase-site, sino también el valor de `maxSessionTimeout` de zoo.cfg de Zookeeper.

1. Acceda a la interfaz de usuario de Ambari, vaya a **HBase -> Configs -> Settings** (HBase -> Configuraciones -> Configuración) y, en la sección, Timeouts (Tiempos de espera), cambie el valor del tiempo de espera de la sesión de Zookeeper.

1. Acceda a la interfaz de usuario de Ambari UI, vaya a **Zookeeper -> Configs -> Custom** (Zookeeper -> Configuraciones -> Personalizar) zoo.cfg, y agregue o cambie el siguiente valor. Asegúrese de que el valor sea el mismo que `zookeeper.session.timeout` de HBase.

    ```
    Key: maxSessionTimeout Value: 120000  
    ```

1. Reinicie los servicios necesarios.

---

## <a name="scenario-log-splitting-failure"></a>Escenario: Error de división de registro

### <a name="issue"></a>Problema

HMasters no se pudo mostrar en un clúster de HBase.

### <a name="cause"></a>Causa

Error de configuración de HDFS y HBase en una cuenta de almacenamiento secundaria.

### <a name="resolution"></a>Resolución

Establezca hbase.rootdir: wasb://@.blob.core.windows.net/hbase y reinicie los servicios de Ambari.

---

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta conecta a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
