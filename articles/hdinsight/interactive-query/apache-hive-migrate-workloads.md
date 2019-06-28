---
title: Migración de cargas de trabajo de Hive de Azure HDInsight 3.6 a HDInsight 4.0
description: Aprenda a migrar las cargas de trabajo de Apache Hive en HDInsight 3.6 a 4.0 de HDInsight.
ms.service: hdinsight
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.topic: howto
ms.date: 04/24/2019
ms.openlocfilehash: b39279e560cb1738ff9b33ec587562efd2ed4e8d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65800949"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Migración de cargas de trabajo de Hive de Azure HDInsight 3.6 a HDInsight 4.0

Este documento le muestra cómo migrar las cargas de trabajo de Apache Hive y LLAP en HDInsight 3.6 a HDInsight 4.0. HDInsight 4.0 proporciona las características más recientes de Hive y LLAP como vistas materializadas y almacenamiento en caché de resultados de consulta. Al migrar las cargas de trabajo a HDInsight 4.0, puede utilizar muchas características más recientes de Hive 3 que no están disponibles en HDInsight 3.6.

En este artículo se tratan los siguientes asuntos:

* Migración de los metadatos de Hive a HDInsight 4.0
* Migración segura de tablas ACID y non-ACID
* Conservación de las directivas de seguridad de Hive en las versiones de HDInsight
* Ejecución de consultas y depuración de HDInsight 3.6 a HDInsight 4.0

## <a name="migrate-apache-hive-metadata-to-hdinsight-40"></a>Migración de los metadatos de Apache Hive a HDInsight 4.0

Una de las ventajas de Hive es la capacidad de exportar los metadatos a una base de datos externa (denominada Hive Metastore). **Hive Metastore** es responsable de almacenar las estadísticas de tabla, incluida la ubicación de almacenamiento de la tabla, nombres de columna e información de índices de tabla. El esquema de base de datos de Hive Metastore difiere entre las versiones de Hive. Realice el procedimiento a continuación para actualizar una instancia de Hive Metastore de HDInsight 3.6 para que sea compatible con HDInsight 4.0.

1. Cree una nueva copia de la instancia externa de Hive Metastore. HDInsight 3.6 y HDInsight 4.0 requieren esquemas diferentes de Metastore y no pueden compartir una misma instancia de Hive Metastore. Consulte [Uso de repositorios de metadatos externos en Azure HDInsight](../hdinsight-use-external-metadata-stores.md) para más información acerca de cómo adjuntar una instancia externa de Metastore a un clúster de HDInsight. 
2. Inicie una acción de script en el clúster de HDI 3.6 con "Nodos principales" como el tipo de nodo para la ejecución. Pegue el siguiente URI en el cuadro de texto marcado "URI de script de Bash": https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/launch-schema-upgrade.sh. En el cuadro de texto marcado como "Argumentos", escriba el nombre de servidor, base de datos, nombre de usuario y contraseña para la instancia **copiada** de Hive Metastore, separado por espacios. No incluya ". database.windows.net" al especificar el nombre del servidor.

> [!Warning]
> La actualización que convierte el esquema de metadatos de HDInsight 3.6 en el esquema de HDInsight 4.0. no se puede revertir.

## <a name="migrate-hive-tables-to-hdinsight-40"></a>Migración de tablas de Hive en HDInsight 4.0

Después de completar el conjunto de pasos anterior para migrar Hive Metastore a HDInsight 4.0, las tablas y bases de datos que se registran en la instancia de Metastore serán visibles desde dentro del clúster de HDInsight 4.0 mediante la ejecución de `show tables` o `show databases` desde dentro del clúster. Consulte [Ejecución de consultas entre diferentes versiones de HDInsight](#query-execution-across-hdinsight-versions) para obtener información sobre la ejecución de consultas en los clústeres de HDInsight 4.0.

Los datos reales de las tablas, sin embargo, no son accesibles hasta que el clúster tenga acceso a las cuentas de almacenamiento necesarias. Para asegurarse de que el clúster de HDInsight 4.0 puede acceder a los mismos datos que el antiguo clúster de HDInsight 3.6, complete los pasos siguientes:

1. Determine la cuenta de almacenamiento de Azure de la tabla o base de datos mediante la descripción del formato.
2. Si ya se está ejecutando el clúster de HDInsight 4.0, asocie la cuenta de almacenamiento de Azure al clúster mediante Ambari. Si todavía no ha creado el clúster de HDInsight 4.0, asegúrese de que la cuenta de almacenamiento de Azure se especifica como la principal o como una cuenta de almacenamiento de clúster secundaria. Para más información acerca de cómo agregar cuentas de almacenamiento a clústeres de HDInsight, consulte [Adición de más cuentas de almacenamiento a HDInsight](../hdinsight-hadoop-add-storage.md).

> [!Note]
> Las tablas se tratan de manera diferente en HDInsight 3.6 y HDInsight 4.0. Por este motivo, no pueden compartir las mismas tablas para los clústeres de distintas versiones. Si desea usar HDInsight 3.6 al mismo tiempo que HDInsight 4.0, tiene que tener copias independientes de los datos para cada versión.

La carga de trabajo de Hive puede incluir una mezcla de tablas ACID y non-ACID. Una diferencia clave entre Hive en HDInsight 3.6 (Hive 2) y Hive en HDInsight 4.0 (Hive 3) es el cumplimiento de las propiedades ACID para las tablas. En HDInsight 3.6, para habilitar el cumplimiento de ACID en Hive requiere configuración adicional, mientras que en HDInsight 4.0 las tablas son compatibles con ACID de forma predeterminada. La única acción necesaria antes de efectuar una migración es ejecutar una compactación principal con la tabla ACID en el clúster de 3.6. Desde la vista de Hive o Beeline, ejecute la consulta siguiente:

```bash
alter table myacidtable compact 'major';
```

Esta compactación es necesaria porque las tablas ACID de HDInsight 3.6 y HDInsight 4.0 reconocen los valores delta ACID de forma diferente. La compactación aplica un inicio limpio que garantiza la coherencia. La sección 4 de la [documentación de migración de Hive](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-upgrade-major/content/prepare_hive_for_upgrade.html) contiene una guía para la compactación masiva de tablas ACID de HDInsight 3.6.

Una vez que haya completado los pasos de compactación y migración de Metastore, puede migrar el almacenamiento. Después de completar la migración del almacenamiento de Hive, el almacenamiento de HDInsight 4.0 tendrá las siguientes propiedades:

* Las tablas externas en HDInsight 3.6 serán tablas externas en HDInsight 4.0
* Las tablas administradas no transaccionales en HDInsight 3.6 serán tablas externas en HDInsight 4.0
* Las tablas administradas transaccionales en HDInsight 3.6 serán tablas administradas en HDInsight 4.0

Es posible que tenga que ajustar las propiedades del almacenamiento antes de ejecutar la migración. Por ejemplo, si espera que haya acceso de terceros a alguna tabla (por ejemplo, a un clúster de HDInsight 3.6), esa tabla tiene que ser externa una vez completada la migración. En HDInsight 4.0, todas las tablas administradas son transaccionales. Por lo tanto, solo los clústeres de HDInsight 4.0 pueden acceder a las tablas administradas en HDInsight 4.0.

Una vez establecidas las propiedades de tabla correctamente, ejecute la herramienta de migración de almacenamiento de Hive desde uno de los nodos principales del clúster mediante el shell de SSH:

1. Conéctese a su nodo principal del clúster mediante SSH. Para obtener instrucciones, consulte [Conexión a HDInsight mediante SSH](../hdinsight-hadoop-linux-use-ssh-unix.md)
1. Abra un shell de inicio de sesión como el usuario de Hive ejecutando `sudo su - hive`
1. Determine la versión de la pila de Hortonworks Data Platform ejecutando `ls /usr/hdp`. Esto mostrará una cadena de versión que debe usar en el siguiente comando.
1. Ejecute el comando siguiente en el shell. Reemplace `${{STACK_VERSION}}` por la cadena de versión del paso anterior:

```bash
/usr/hdp/${{STACK_VERSION}}/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true  -m automatic  automatic  --modifyManagedTables --oldWarehouseRoot /apps/hive/warehouse
```

Una vez que la herramienta de migración termina, el almacenamiento de Hive estará listo para HDInsight 4.0. 

> [!Important]
> No se deba acceder a las tablas administradas en 4.0 HDInsight (incluidas las migradas desde 3.6) desde otros servicios o aplicaciones, incluidos los clústeres de HDInsight 3.6.

## <a name="secure-hive-across-hdinsight-versions"></a>Protección de Hive en diferentes versiones de HDInsight

Desde HDInsight 3.6, HDInsight se integra con Azure Active Directory mediante Enterprise Security Package (ESP) de HDInsight. ESP utiliza Kerberos y Apache Ranger para administrar los permisos de recursos específicos dentro del clúster. Las directivas de Ranger implementadas con Hive en HDInsight 3.6 se pueden migrar a HDInsight 4.0 con los pasos siguientes:

1. Vaya al panel de Ranger Service Manager en el clúster de HDInsight 3.6.
2. Vaya a la directiva denominada **HIVE** y expórtela a un archivo json.
3. Asegúrese de que todos los usuarios a los que se hace referencia en el archivo json de directiva exportada existen en el nuevo clúster. Si en el json de directiva aparece una referencia a un usuario que no existe en el nuevo clúster, tendrá que agregar el usuario al nuevo clúster o quitar la referencia de la directiva.
4. Vaya al panel de **Ranger Service Manager** en el clúster de HDInsight 4.0.
5. Vaya a la directiva denominada **HIVE** e importe el archivo json de directiva de ranger del paso 2.

## <a name="query-execution-across-hdinsight-versions"></a>Ejecución de consultas entre diferentes versiones de HDInsight

Hay dos maneras de ejecutar y depurar las consultas de Hive/LLAP dentro de un clúster de HDInsight 3.6. HiveCLI proporciona una experiencia de línea de comandos y la vista de Hive/vista de Tez proporciona un flujo de trabajo basado en una GUI.

En HDInsight 4.0, HiveCLI se ha reemplazado por Beeline. HiveCLI es un cliente de Thrift para Hiveserver 1 y Beeline es un cliente JDBC que proporciona acceso a Hiveserver 2. Beeline también se puede usar para conectarse a cualquier otro punto de conexión de la base de datos compatible con JDBC. Beeline está disponible como componente integrado en HDInsight 4.0 sin necesidad de instalación.

En HDInsight 3.6, el cliente de la interfaz gráfica de usuario para interactuar con el servidor de Hive es la vista de Hive de Ambari. HDInsight 4.0 reemplaza la vista de Hive con Hortonworks Data Analytics Studio (DAS). DAS no se suministra integrado con clústeres de HDInsight y no es un paquete oficialmente compatible. Sin embargo, DAS se puede instalar en el clúster de la forma siguiente:

Inicie una acción de script en el clúster con "Nodos principales" como el tipo de nodo para la ejecución. Pegue el siguiente URI en el cuadro de texto marcadas "URI de script de Bash": https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh

Data Analytics Studios se puede iniciar con la dirección URL: https://<clustername>.azurehdinsight.net/das/



Una vez instalado DAS, si no ve las consultas que haya ejecutado en el visor de consultas, realice los pasos siguientes:

1. Establezca las configuraciones de Hive, Tez y DAS, como se describe en [esta guía para solucionar problemas de instalación de DAS](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html).
2. Asegúrese de que las siguientes configuraciones de directorio de almacenamiento de Azure son blobs en páginas, y que aparecen en `fs.azure.page.blob.dirs`:
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. Reinicie HDFS, Tez, Hive y DAS en ambos nodos principales.

## <a name="next-steps"></a>Pasos siguientes

* [Anuncio de HDInsight 4.0](../hdinsight-version-release.md)
* [Análisis a fondo de HDInsight 4.0](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [Tablas ACID de Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
