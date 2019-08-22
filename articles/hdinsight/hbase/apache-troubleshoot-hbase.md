---
title: Solución de problemas de HBase mediante Azure HDInsight
description: Obtenga respuestas a las preguntas comunes sobre cómo trabajar con HBase y Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive, seodec18
ms.topic: troubleshooting
ms.date: 08/16/2019
ms.openlocfilehash: cf44c27f51bf6312ef546b424646833f69ba0283
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69638429"
---
# <a name="troubleshoot-apache-hbase-by-using-azure-hdinsight"></a>Solución de problemas de Apache HBase mediante Azure HDInsight

Obtenga información sobre los principales problemas y sus soluciones al trabajar con cargas útiles de HBase en Apache Ambari.

## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>¿Cómo se solucionan los problemas de conectividad de JDBC o sqlline con Apache Phoenix?

### <a name="resolution-steps"></a>Pasos de la solución

Para conectar con Apache Phoenix, debe proporcionar la dirección IP del nodo activo de Apache Zookeeper. Asegúrese de que el servicio Zookeeper al que sqlline.py intenta conectarse está en funcionamiento.
1. Inicie sesión en el clúster de HDInsight mediante SSH.
2. Escriba el comando siguiente:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > Puede obtener la dirección IP del nodo de ZooKeeper activo de la interfaz de usuario de Ambari. Vaya a **HBase** > **Quick Links (Vínculos rápidos)**  > **ZK\* (Active)**  > **Zookeeper Info (Información de Zookeeper)** .

3. Si sqlline.py se conecta a Phoenix y no supera el tiempo de espera, ejecute el siguiente comando para validar la disponibilidad y el estado de Phoenix:

   ```apache
           !tables
           !quit
   ```      
4. Si el comando funciona, no hay ningún problema. La dirección IP que proporciona el usuario puede ser incorrecta. Sin embargo, si el comando se detiene durante un período prolongado y luego muestra el siguiente error, vaya al paso 5.

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. Ejecute los siguientes comandos desde el nodo principal (hn0) para diagnosticar la condición de la tabla SYSTEM.CATALOG de Phoenix:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   El comando debe devolver un error similar al siguiente: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. En la interfaz de usuario de Apache Ambari, realice los siguientes pasos para reiniciar el servicio HMaster en todos los nodos de ZooKeeper:

    1. En la sección **Summary** (Resumen) de HBase, vaya a **HBase** > **Active HBase Master**. 
    2. En la sección **Components** (Componentes), reinicie el servicio HBase Master.
    3. Repita estos para todos los servicios **Standby HBase Master** restantes. 

El servicio HBase Master puede tener un máximo de cinco minutos en estabilizar y finalizar el proceso de recuperación. Pocos minutos después, repita los comandos de sqlline.py para confirmar que la tabla SYSTEM.CATALOG está activa y que se puede consultar. 

Cuando la tabla SYSTEM.CATALOG vuelva al estado normal, el problema de la conectividad con Phoenix debería resolverse automáticamente.

## <a name="what-causes-a-restart-failure-on-a-region-server"></a>¿Qué provoca un error de reinicio en un servidor de regiones?

### <a name="issue"></a>Problema

Si se siguen los procedimientos recomendados, se puede evitar un error de reinicio en un servidor de regiones. Se recomienda pausar la actividad de las cargas de trabajo pesadas cuando se planea reiniciar los servidores de regiones de HBase. Si una aplicación continúa conectándose a servidores de regiones mientras el cierre esté en curso, la operación de reinicio del servidor de regiones se ralentizará durante varios minutos. También es aconsejable vaciar primero todas las tablas. Para una referencia sobre cómo vaciar tablas, consulte [HDInsight HBase: How to improve the Apache HBase cluster restart time by flushing tables](https://web.archive.org/web/20190112153155/https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/) (HDInsight HBase: cómo mejorar el tiempo de reinicio de clúster de Apache HBase vaciando tablas).

Si inicia la operación de reinicio en los servidores de regiones de HBase desde la interfaz de usuario de Apache Ambari, verá inmediatamente que los servidores de regiones han dejado de funcionar, pero no se reinician de inmediato. 

Esto es lo que sucede en segundo plano: 

1. El agente de Ambari enviará una solicitud de detención al servidor de regiones.
2. El agente de Ambari espera 30 segundos a que el servidor de regiones se cierre correctamente. 
3. Si su aplicación sigue conectándose con el servidor de regiones, el servidor no apagará de inmediato. El tiempo de expiración de 30 segundos expira antes de que produzca el apagado. 
4. Después de 30 segundos, el agente de Ambari envía un comando force-kill (`kill -9`) al servidor de regiones. Esto se puede ver en el registro del agente de ambari (en el directorio /var/log/ del nodo de trabajo correspondiente):

   ```apache
           2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
           est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
           f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
           -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
           2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
           /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
           PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
           nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
           2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
           2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
   ```
   Debido al abrupto apagado, puede que el puerto asociado con el proceso no se libere, aunque se detenga el proceso del servidor de regiones. Esta situación puede provocar una excepción AddressBindException cuando se inicia el servidor de regiones, como se muestra en los registros siguientes. Puede comprobarlo en el archivo region-server.log del directorio /var/log/hbase en los nodos de trabajo en los que el servidor de regiones no se inicia. 

   ```apache

   2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
   java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
   at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
   at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
        
   Caused by: java.lang.reflect.InvocationTargetException
   at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
   at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
   at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
   at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
   ... 5 more
        
   Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
   at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
   at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
   at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
   ... 10 more
        
   Caused by: java.net.BindException: Address already in use
   at sun.nio.ch.Net.bind0(Native Method)
   at sun.nio.ch.Net.bind(Net.java:463)
   at sun.nio.ch.Net.bind(Net.java:455)
   at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
   at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
   ... 15 more
   ```

### <a name="resolution-steps"></a>Pasos de la solución

1. Intente reducir la carga en los servidores de regiones de HBase antes de iniciar un reinicio. 
2. Como alternativa (si el paso 1 no sirve de ayuda), pruebe a reiniciar manualmente los servidores de regiones en los nodos de trabajo con los siguientes comandos:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta conecta a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
