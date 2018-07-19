---
title: Flujos de trabajo de Oozie de Hadoop en clústeres de HDInsight unidos a un dominio
description: Use Oozie de Hadoop en Enterprise Security Package unido a un dominio de HDInsight basado en Linux. Aprenda a definir un flujo de trabajo de Oozie y enviar un trabajo de Oozie.
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: omidm
ms.openlocfilehash: 928f6adbb348683a110f7da9b20efaae998290ca
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972220"
---
#<a name="run-apache-oozie-in-domain-joined-hdinsight-hadoop-clusters"></a>Ejecución de Apache Oozie en clústeres de HDInsight Hadoop unidos a un dominio
Oozie es un sistema de coordinación y flujos de trabajo que administra trabajos de Hadoop. Oozie se integra con la pila de Hadoop y admite los siguientes trabajos:
- Apache MapReduce
- Apache Pig
- Apache Hive
- Apache Sqoop

Oozie también puede usarse para programar trabajos específicos de un sistema, como scripts de shell o programas Java.

##<a name="prerequisites"></a>Requisitos previos:
- Un clúster de HDInsight Hadoop unido a un dominio. Consulte [Configuración de clústeres de HDInsight unidos a un dominio](./apache-domain-joined-configure-using-azure-adds.md)

    > [!NOTE]
    > Para ver instrucciones detalladas para usar Oozie en clústeres no unidos a dominio consulte [esto](../hdinsight-use-oozie-linux-mac.md)

##<a name="connecting-to-domain-joined-cluster"></a>Conexión con un clúster unido a un dominio
Para más información sobre SSH, consulte [Autenticación: HDInsight unido a un dominio](../hdinsight-hadoop-linux-use-ssh-unix.md).
- Conéctese al clúster de HDInsight con SSH:
     ```bash
    ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
    ```
Para verificar que la autenticación Kerberos se realizó correctamente, utilice el comando `klist`. Si no es así, utilice `kinit` para iniciar la autenticación Kerberos.

- Inicie sesión en la puerta de enlace de HDInsight para registrar el token de oAuth necesario para obtener acceso a Azure Data Lake Store (ADLS)
     ```bash
     curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
     ```

    Un código de respuesta de estado _200 Correcto_ indica que el registro se ha realizado con éxito. Compruebe el nombre de usuario y la contraseña si recibe una respuesta de No autorizado (401).

## <a name="define-the-workflow"></a>Definición del flujo de trabajo
Las definiciones de flujo de trabajo de Oozie se escriben en lenguaje de definición de proceso de Hadoop (hPDL), que es un lenguaje de definición de proceso XML. Use los pasos siguientes para definir el flujo de trabajo:

-   Configuración del área de trabajo del usuario de dominio:
 ```bash
hdfs dfs -mkdir /user/<DomainUser>
cd /home/<DomainUserPath>
cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
tar -xvf oozie-examples.tar.gz
hdfs dfs -put examples /user/<DomainUser>/
 ```
Reemplace `DomainUser` con el nombre de usuario del dominio. Reemplace `DomainUserPath` con la ruta de acceso del directorio principal del usuario de dominio. Reemplace `ClusterVersion` la versión de HDP del clúster.

-   Use la instrucción siguiente para crear y editar un archivo nuevo:
 ```bash
nano workflow.xml
 ```

- Cuando se abra el editor nano, escriba el siguiente XML como contenido del archivo:
 ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <workflow-app xmlns="uri:oozie:workflow:0.4" name="map-reduce-wf">
       <credentials>
          <credential name="metastore_token" type="hcat">
             <property>
                <name>hcat.metastore.uri</name>
                <value>thrift://hn0-<clustername>.<Domain>.com:9083</value>
             </property>
             <property>
                <name>hcat.metastore.principal</name>
                <value>hive/_HOST@<Domain>.COM</value>
             </property>
          </credential>
          <credential name="hs2-creds" type="hive2">
             <property>
                <name>hive2.server.principal</name>
                <value>${jdbcPrincipal}</value>
             </property>
             <property>
                <name>hive2.jdbc.url</name>
                <value>${jdbcURL}</value>
             </property>
          </credential>
       </credentials>
       <start to="mr-test" />
       <action name="mr-test">
          <map-reduce>
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <prepare>
                <delete path="${nameNode}/user/${wf:user()}/examples/output-data/mrresult" />
             </prepare>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
                <property>
                   <name>mapred.mapper.class</name>
                   <value>org.apache.oozie.example.SampleMapper</value>
                </property>
                <property>
                   <name>mapred.reducer.class</name>
                   <value>org.apache.oozie.example.SampleReducer</value>
                </property>
                <property>
                   <name>mapred.map.tasks</name>
                   <value>1</value>
                </property>
                <property>
                   <name>mapred.input.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/input-data/text</value>
                </property>
                <property>
                   <name>mapred.output.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/output-data/mrresult</value>
                </property>
             </configuration>
          </map-reduce>
          <ok to="myHive2" />
          <error to="fail" />
       </action>
       <action name="myHive2" cred="hs2-creds">
          <hive2 xmlns="uri:oozie:hive2-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <jdbc-url>${jdbcURL}</jdbc-url>
             <script>${hiveScript2}</script>
             <param>hiveOutputDirectory2=${hiveOutputDirectory2}</param>
          </hive2>
          <ok to="myHive" />
          <error to="fail" />
       </action>
       <action name="myHive" cred="metastore_token">
          <hive xmlns="uri:oozie:hive-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
             </configuration>
             <script>${hiveScript1}</script>
             <param>hiveOutputDirectory1=${hiveOutputDirectory1}</param>
          </hive>
          <ok to="end" />
          <error to="fail" />
       </action>
       <kill name="fail">
          <message>Oozie job failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
       </kill>
       <end name="end" />
    </workflow-app>
 ```
Reemplace `clustername` por el nombre del clúster. 

Para guardar el archivo, seleccione Ctrl+X, escriba `Y` y, a continuación, seleccione **Entrar**.

El flujo de trabajo se divide en dos partes:
*   Sección Credential: esta sección toma las credenciales que se usarán para autenticar las acciones de Oozie.

    En este ejemplo, se usa la autenticación para las acciones de Hive. Para más información, consulte [esto]( https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html).

    El servicio de credenciales permite a las acciones de Oozie suplantar al usuario para obtener acceso a los servicios de Hadoop.

*   Sección Action: aquí tenemos tres acciones: map-reduce, hive server 2 y hive server 1.

    Map-reduce ejecuta un ejemplo de paquete de Oozie para asignación-reducción que genera el recuento de palabras agregado.

    Las acciones hive server 2 y hive server 1 ejecutan una consulta simple en la tabla hivesample proporcionada con HDInsight.

    Las acciones hive usan las credenciales definidas en la sección de credenciales para la autenticación mediante la palabra clave `cred` en el elemento action

- Use el comando siguiente para copiar el archivo `workflow.xml` en `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`:
     ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
     ```

    Reemplace `domainuser` con el nombre de usuario para el dominio.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Definición del archivo de propiedades para el trabajo de Oozie

1.  Use la instrucción siguiente para crear y editar un archivo nuevo para las propiedades del trabajo:
     ```bash
    nano job.properties
     ```

2.   Cuando se abra el editor nano, use el siguiente XML como contenido del archivo:

    ```bash
        nameNode=adl://home
        jobTracker=headnodehost:8050
        queueName=default
        examplesRoot=examples
        oozie.wf.application.path=${nameNode}/user/[domainuser]/examples/apps/map-reduce/workflow.xml
        hiveScript1=${nameNode}/user/${user.name}/countrowshive1.hql
        hiveScript2=${nameNode}/user/${user.name}/countrowshive2.hql
        oozie.use.system.libpath=true
        user.name=[domainuser]
        jdbcPrincipal=hive/hn0-<ClusterShortName>.<Domain>.com@<Domain>.COM
        jdbcURL=[jdbcurlvalue]
        hiveOutputDirectory1=${nameNode}/user/${user.name}/hiveresult1
        hiveOutputDirectory2=${nameNode}/user/${user.name}/hiveresult2
    ```
    

   * Reemplace `domainuser` con el nombre de usuario para el dominio.
   * Reemplace `ClusterShortName` con el nombre corto para el clúster. Si clustername es https://sechadoopcontoso.azurehdisnight.net, el `clustershortname` son las seis primeras letras del clúster: sechad
   * Reemplace `jdbcurlvalue` con la dirección url JDBC desde la configuración de Hive. Por ejemplo, jdbc:hive2://headnodehost:10001/;transportMode=http .
    
   * Para guardar el archivo, seleccione Ctrl+X, escriba `Y` y, a continuación, seleccione **Entrar**.

   * Este archivo de propiedades tiene que estar presente localmente al ejecutar trabajos de Oozie

## <a name="creating-custom-hive-scripts-for-the-oozie-job"></a>Creación de scripts personalizados de Hive para el trabajo de Oozie
Los 2 scripts de Hive para hive server 1 y hive server 2 se pueden de la forma siguiente:
-   Archivo de hive server 1:
1.  Use la instrucción siguiente para crear y editar un archivo para la acción hive server 1:
    ```bash
    nano countrowshive1.hql
    ```

2.  Cree el script
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemake from hivesampletable limit 2;
    ```

3.  Guarde el archivo en HDFS
    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

-   Archivo de hive server 2:
1.  Use la instrucción siguiente para crear y editar un archivo para la acción hive server 2:
    ```bash
    nano countrowshive2.hql
    ```

2.  Cree el script
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3.  Guarde el archivo en HDFS
    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submission-of-oozie-jobs"></a>Envío de trabajos de Oozie
El envío de trabajos de Oozie para clústeres unidos a un dominio es similar al envío de trabajos de Oozie en los clústeres no unidos a dominio.

Para más información, consulte cómo [enviar y administrar el trabajo](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-oozie-job-submission"></a>Resultados del envío de trabajos de Oozie
Puesto que los trabajos de Oozie se ejecutan en nombre del usuario, los registro de auditoría Yarn y Ranger muestran los trabajos como ejecutados por el usuario suplantado. La salida de la CLI del trabajo de Oozie es similar a la siguiente:


```bash
    Job ID : 0000015-180626011240801-oozie-oozi-W
    ------------------------------------------------------------------------------------------------
    Workflow Name : map-reduce-wf
    App Path      : adl://home/user/alicetest/examples/apps/map-reduce/wf.xml
    Status        : SUCCEEDED
    Run           : 0
    User          : alicetest
    Group         : -
    Created       : 2018-06-26 19:25 GMT
    Started       : 2018-06-26 19:25 GMT
    Last Modified : 2018-06-26 19:30 GMT
    Ended         : 2018-06-26 19:30 GMT
    CoordAction ID: -
    
    Actions
    ------------------------------------------------------------------------------------------------
    ID                      Status  Ext ID          ExtStatus   ErrCode
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@:start:    OK  -           OK      -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@mr-test    OK  job_1529975666160_0051  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive2    OK  job_1529975666160_0053  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive OK  job_1529975666160_0055  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@end    OK  -           OK      -
    -----------------------------------------------------------------------------------------------
```

*    Los registros de auditoría de Ranger para las acciones hive server 2 muestran a Oozie ejecutando la acción en nombre del usuario. La vista de Ranger y Yarn es visible solamente para el administrador de clúster.

## <a name="configuration-of-user-authorization-in-oozie"></a>Configuración de autorización de usuario en Oozie
Oozie por sí solo tiene una configuración de autorización de usuario que impide que los usuarios detengan o eliminen trabajos de otro usuario. Esto se habilita estableciendo `oozie.service.AuthorizationService.security.enabled` en `true`. 

Encontrará más detalles sobre este punto en la sección de documentación de Oozie [Oozie User Authorization Configuration]( https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html) (Configuración de autorización de usuario de Oozie):

Para los componentes como hive server 1 donde el complemento Ranger no es compatible o no está disponible, solo es posible usar la autorización HDFS general. El tipo de autorización más específica solo está disponible mediante los complementos Ranger.

## <a name="oozie-web-ui"></a>Interfaz de usuario web de Oozie
La interfaz de usuario web de Oozie ofrece una vista basada en web en el estado de los trabajos de Oozie en el clúster. En los clústeres unidos a un dominio tiene que:

1. Agregar un [nodo perimetral](../hdinsight-apps-use-edge-node.md) y habilitar la [autenticación SSH Kerberos](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. Siga los pasos en [Interfaz de usuario web de Oozie](../hdinsight-use-oozie-linux-mac.md) para habilitar la tunelización de SSH al nodo perimetral y tener acceso a la interfaz de usuario web.

## <a name="next-steps"></a>Pasos siguientes
* [Uso de Oozie con Hadoop para definir y ejecutar un flujo de trabajo en Azure HDInsight basado en Linux](../hdinsight-use-oozie-linux-mac.md)
* [Uso del coordinador de Oozie basado en tiempo](../hdinsight-use-oozie-coordinator-time.md)
* [Ejecución de consultas de Hive mediante SSH en clústeres de HDInsight unidos a un dominio](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
