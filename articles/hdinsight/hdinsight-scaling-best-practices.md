---
title: Escalabilidad de los tamaños de clúster en Azure HDInsight
description: Escale un clúster de HDInsight de Azure con total flexibilidad para que coincida con la carga de trabajo.
author: ashishthaps
ms.author: ashish
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: 622261d0f7e602635aa6a638357278a9c63a6ecd
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990510"
---
# <a name="scale-hdinsight-clusters"></a>Escalabilidad de clústeres de HDInsight

HDInsight proporciona elasticidad, lo que ofrece la opción de escalar y reducir verticalmente el número de nodos de trabajo de los clústeres. Esta flexibilidad, podrá reducir un clúster después de horas o los fines de semana y expandirlo durante períodos de máxima demanda de negocio.

Si tiene el procesamiento por lotes periódicos, el clúster de HDInsight se puede escalar verticalmente unos minutos antes de esa operación, por lo que el clúster tiene suficiente memoria y potencia de CPU.  Después, una vez realizado el procesamiento y de que el uso vuelva a bajar, puede reducir verticalmente el clúster de HDInsight a menos nodos de trabajo.

Puede escalar un clúster manualmente mediante uno de los métodos descritos a continuación o usar [escalado automático](hdinsight-autoscale-clusters.md) opciones para que el sistema automáticamente escalar vertical y horizontalmente en respuesta a la CPU, memoria y otras métricas.

## <a name="utilities-to-scale-clusters"></a>Utilidades para escalar clústeres

Microsoft proporciona las utilidades siguientes para escalar clústeres:

|Utilidad | DESCRIPCIÓN|
|---|---|
|[PowerShell Az](https://docs.microsoft.com/powershell/azure)|[Conjunto AzHDInsightClusterSize](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) - ClusterName \<nombre del clúster > - TargetInstanceCount \<NewSize >|
|[PowerShell AzureRM](https://docs.microsoft.com/powershell/azure/azurerm) |[Conjunto AzureRmHDInsightClusterSize](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) - ClusterName \<nombre del clúster > - TargetInstanceCount \<NewSize >|
|[CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)| [cambiar el tamaño de AZ hdinsight](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) : grupo de recursos \<grupo de recursos >--nombre \<nombre del clúster >--recuento de instancias de destino \<NewSize >|
|[CLI de Azure](hdinsight-administer-use-command-line.md)|cambio de tamaño de clúster de hdinsight de Azure \<clusterName > \<recuento de instancias de destino > |
|[Azure Portal](https://portal.azure.com)|Abra el panel del clúster de HDInsight, seleccione **tamaño del clúster** en el menú izquierdo, en el panel de tamaño del clúster, escriba el número de nodos de trabajo y seleccione Guardar.|  

![Escalar clúster](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Con cualquiera de estos métodos, puede escalar o reducir verticalmente el clúster de HDInsight en cuestión de minutos.

> [!IMPORTANT]  
> * La CLI de Azure clásica está en desuso y solo debe usarse con el modelo de implementación clásica. Para todas las demás implementaciones, utilice el [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).  
> * El módulo AzureRM de PowerShell está en desuso.  Use la [módulo Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) siempre que sea posible.

## <a name="impact-of-scaling-operations"></a>Impacto de las operaciones de escalado

Cuando se **agregar** no se verán afectados los nodos del clúster de HDInsight ejecutando (escalado vertical), todos los trabajos pendientes o en ejecución. Los trabajos nuevos pueden enviarse de forma segura mientras el proceso de escalado está en ejecución. Si se produce un error en la operación de escalado por cualquier motivo, se controlará el error para dejar el clúster en un estado funcional.

Si se **quitar** nodos (escala hacia abajo), cualquier will trabajos pendientes o en ejecución producirá un error cuando se complete la operación de escalado. Este error es debido a algunos de los servicios se reinician durante el proceso de escalado. También hay un riesgo de que el clúster puede obtener atascado en modo seguro durante la operación de escalado manual.

## <a name="how-to-safely-scale-down-a-cluster"></a>Cómo escalar con seguridad verticalmente un clúster

### <a name="scale-down-a-cluster-with-running-jobs"></a>Escalar verticalmente un clúster con la ejecución de trabajos

Para evitar que los trabajos en ejecución producirá un error durante una operación de reducción, puede intentar tres cosas:

1. Espere a que los trabajos para completarse antes de reducir verticalmente el clúster.
1. Finalizar manualmente los trabajos.
1. Vuelva a enviar los trabajos una vez haya finalizado la operación de escalado.

Para ver una lista de pendientes y los trabajos en ejecución, puede usar el YARN **UI de ResourceManager**, siga estos pasos:

1. Inicie sesión en el [portal de Azure](https://portal.azure.com).
2. Desde la izquierda, vaya a **todos los servicios** > **Analytics** > **clústeres de HDInsight**y, a continuación, seleccione el clúster.
3. En la vista principal, vaya a **paneles de clúster** > **Ambari principal**. Escriba sus credenciales de clúster.
4. En la UI de Ambari, seleccione **YARN** en la lista de servicios en el menú izquierdo.  
5. En la página YARN, seleccione **vínculos rápidos** y mantenga el mouse sobre el nodo principal activo, a continuación, seleccione **UI de ResourceManager**.

    ![UI de ResourceManager](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

Puede acceder directamente a la UI de ResourceManager con `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`.

Verá una lista de trabajos, junto con su estado actual. En la captura de pantalla, hay un trabajo en ejecución actualmente:

![Aplicaciones de la UI de ResourceManager](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Para terminar manualmente dicha aplicación en ejecución, ejecute el siguiente comando del shell de SSH:

```bash
yarn application -kill <application_id>
```

Por ejemplo:

```bash
yarn application -kill "application_1499348398273_0003"
```

### <a name="getting-stuck-in-safe-mode"></a>Están acumulando en modo seguro

Al escalar verticalmente un clúster, HDInsight utiliza Apache Ambari interfaces de administración antes de retirar los nodos de trabajo adicionales, que replican sus bloques de HDFS en otros nodos de trabajo en línea. Después de eso, HDInsight escala de forma segura el clúster hacia abajo. HDFS entra en modo seguro durante la operación de escalado y se supone que salen de una vez finalizado el escalado. En algunos casos, sin embargo, HDFS se atasca en modo seguro durante una operación de escalado debido a la replicación debajo del bloque de archivos.

De forma predeterminada, HDFS se configura con un `dfs.replication` establecer de 3, que controla el número de copias de cada bloque de archivos está disponible. Cada copia de un bloque de archivos se almacena en otro nodo del clúster.

Cuando HDFS detecta que el número esperado de copias de bloque no está disponible, HDFS entra en modo seguro y genera alertas en Ambari. Si HDFS entra en modo seguro para una operación de escalado, pero, a continuación, no puede salir del modo seguro porque no se detecta el número necesario de nodos para la replicación, el clúster puede bloquearse en modo seguro.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Ejemplos de errores cuando el modo seguro está activado

```
org.apache.hadoop.hdfs.server.namenode.SafeModeException: Cannot create directory /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. Name node is in safe mode.
```

```
org.apache.http.conn.HttpHostConnectException: Connect to hn0-clustername.servername.internal.cloudapp.net:10001 [hn0-clustername.servername. internal.cloudapp.net/1.1.1.1] failed: Connection refused
```

Puede revisar los registros del nodo de nombre en la carpeta `/var/log/hadoop/hdfs/`, cerca del momento en que se escaló el clúster, para ver cuándo entró en modo seguro. Los archivos de registro se denominan `Hadoop-hdfs-namenode-hn0-clustername.*`.

La causa de los errores anteriores es que Hive depende de los archivos temporales de HDFS durante la ejecución de consultas. Cuando HDFS entra en modo seguro, Hive no puede ejecutar las consultas porque no puede escribir en HDFS. Los archivos temporales de HDFS se encuentran en la unidad local montada en las máquinas virtuales de los nodos de trabajo individuales y se replican entre otros nodos de trabajo en tres réplicas, como mínimo.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode"></a>Cómo impedir que HDInsight se están acumulando en modo seguro

Hay varias maneras de evitar que HDInsight se quede en modo seguro:

* Detenga todos los trabajos de Hive antes de reducir verticalmente HDInsight. Como alternativa, programe el proceso de reducción vertical para evitar que entre en conflicto con los trabajos de Hive en ejecución.
* Limpie manualmente los archivos temporales del directorio `tmp` de Hive en HDFS antes de la reducción vertical.
* Solo reduzca verticalmente HDInsight a tres nodos de trabajo como mínimo. Evite reducir a un solo nodo de trabajo.
* Ejecute el comando para salir del modo seguro, si es necesario.

Estas opciones se describen en las secciones siguientes.

#### <a name="stop-all-hive-jobs"></a>Detención de todos los trabajos de Hive

Detenga todos los trabajos de Hive antes de reducir verticalmente a un nodo de trabajo. Si la carga de trabajo está programada, ejecute la reducción vertical una vez realizado el trabajo de Hive.

Detener los trabajos de Hive antes de escalar, ayuda a minimiza el número de archivos temporales en la carpeta tmp (si existe).

#### <a name="manually-clean-up-hives-scratch-files"></a>Limpieza manual de los archivos temporales de Hive

Si Hive ha dejado archivos temporales, puede limpiarlos manualmente antes de la reducción vertical para evitar la activación del modo seguro.

1. Comprobar qué ubicación se usa para los archivos temporales de Hive examinando el `hive.exec.scratchdir` propiedad de configuración. Este parámetro se establece en `/etc/hive/conf/hive-site.xml`:

    ```xml
    <property>
        <name>hive.exec.scratchdir</name>
        <value>hdfs://mycluster/tmp/hive</value>
    </property>
    ```

1. Detenga los servicios de Hive y asegúrese de que se hayan completado todas las consultas y los trabajos.
2. Mostrar el contenido del directorio temporal se encuentra por encima, `hdfs://mycluster/tmp/hive/` para ver si contiene los archivos:

    ```
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```

    Este es un ejemplo de resultado cuando existen archivos:

    ```
    sshuser@hn0-scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Si sabe que Hive ya no necesita estos archivos, puede eliminarlos. Asegúrese de que Hive no tiene ninguna consulta en ejecución; para ello, consulte la página de la interfaz de usuario de ResourceManager en Yarn.

    Línea de comandos de ejemplo para quitar archivos de HDFS:

    ```
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```

#### <a name="scale-hdinsight-to-three-or-more-worker-nodes"></a>Escala HDInsight a tres o más nodos de trabajo

Si los clústeres de quedarse atascados en modo seguro con frecuencia cuando se escala hasta los menos de tres nodos de trabajo y los pasos anteriores no funcionan, a continuación, puede evitar el clúster entran en modo seguro por completo si se mantiene al menos tres nodos de trabajo.

Conserva los tres nodos de trabajo es más costoso que reducir verticalmente a un solo nodo de trabajo, pero impedirá que el clúster están acumulando en modo seguro.

#### <a name="run-the-command-to-leave-safe-mode"></a>Ejecución del comando para salir del modo seguro

La última opción es ejecutar el comando de dejar el modo seguro. Si sabe que es la razón para entrar en modo seguro de HDFS debido a la replicación de exceso de archivos de Hive, puede ejecutar el siguiente comando para salir del modo seguro:


```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

### <a name="scale-down-an-apache-hbase-cluster"></a>Reducir verticalmente un clúster de Apache HBase

Servidores regionales se equilibran automáticamente en unos minutos después de completar una operación de escalado. Para equilibrar manualmente servidores regionales, complete los pasos siguientes:

1. Conéctate al clúster de HDInsight con SSH: Para más información, consulte [Uso SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Inicie el shell de HBase:

    ```bash
    hbase shell
    ```

3. Use el comando siguiente para equilibrar manualmente los servidores regionales:

    ```bash
    balancer
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Escalado automático de clústeres de HDInsight de Azure](hdinsight-autoscale-clusters.md)
* [Introducción a Azure HDInsight](hadoop/apache-hadoop-introduction.md)
* [Escalado de clústeres](hdinsight-administer-use-portal-linux.md#scale-clusters)
