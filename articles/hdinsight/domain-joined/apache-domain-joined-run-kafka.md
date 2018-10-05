---
title: Configuración de directivas de Kafka en HDInsight con Enterprise Security Package para Azure
description: Obtenga información sobre cómo configurar directivas de Apache Ranger para Kafka en Azure HDInsight con Enterprise Security Package.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: 1a8f04f39568816252175fc9e0893f1ab3e2cdc6
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224824"
---
# <a name="tutorial-configure-kafka-policies-in-hdinsight-with-enterprise-security-package-preview"></a>Tutorial: Configuración de directivas de Kafka en HDInsight con Enterprise Security Package (versión preliminar)

Obtenga información sobre cómo configurar directivas de Apache Ranger para los clústeres Kafka de Enterprise Security Package (ESP). Los clústeres de ESP se conectan a un dominio, lo que permite a los usuarios autenticarse con credenciales de dominio. En este tutorial, creará dos directivas de Ranger para restringir el acceso a los temas `sales*` y `marketingspend`.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear usuarios de dominio
> * Creación de directivas de Ranger
> * Crear temas en un clúster de Kafka
> * Probar directivas de Ranger

## <a name="before-you-begin"></a>Antes de empezar

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/).

* Inicie sesión en el [Azure Portal](https://portal.azure.com/).

* Cree un [clúster de Kafka de HDInsight con Enterprise Security Package](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-run-hive).

## <a name="connect-to-apache-ranger-admin-ui"></a>Conexión a la interfaz de usuario administrador de Apache Ranger

1. En un explorador, conéctese a la interfaz de usuario administrador de Ranger desde la dirección URL `https://<ClusterName>.azurehdinsight.net/Ranger/`. No olvide cambiar `<ClusterName>` por el nombre del clúster de Kafka.

    > [!NOTE] 
    > Las credenciales de Ranger no son las mismas que las credenciales del clúster de Hadoop. Para evitar que los exploradores usen credenciales almacenadas en caché de Hadoop, use una nueva ventana del explorador InPrivate para conectarse a la interfaz de usuario administrador de Ranger.

2. Inicie sesión con sus credenciales de administrador de Azure Active Directory (AD). Las credenciales de administrador de Azure AD no son las mismas que las credenciales del clúster de HDInsight ni las credenciales SSH del nodo de HDInsight Linux.

   ![Interfaz de usuario administrador de Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-login.png)

## <a name="create-domain-users"></a>Crear usuarios de dominio

Visite [Create a HDInsight cluster with Enterprise Security Package](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#create-a-domain-joined-hdinsight-cluster) (Creación de un clúster de HDInsight con Enterprise Security Package) para aprender a crear los usuario del dominio **sales_user** y **marketing_user**. En un escenario de producción, los usuarios del dominio proceden de su inquilino de Active Directory.

## <a name="create-ranger-policy"></a>Creación de directiva de Ranger 

Cree una directiva de Ranger para **sales_user** y **marketing_user**.

1. Abra la **Interfaz de usuario administrador de Ranger**.

2. Haga clic en **\<ClusterName>_kafka** en **Kafka**. Puede aparecer una directiva configurada previamente.

3. Haga clic en **Agregar nueva directiva** y escriba los siguientes valores:

   |**Configuración**  |**Valor sugerido**  |
   |---------|---------|
   |Nombre de la directiva  |  hdi sales* policy   |
   |Tema   |  sales* |
   |Seleccionar usuario  |  sales_user1 |
   |Permisos  | publicar, consumir, crear |

   Los siguientes caracteres comodín se pueden incluir en el nombre del tema:

   * ’*’ indica ninguna o más repeticiones de caracteres.
   * ’?’ indica cualquier carácter individual.

   ![Directiva de creación de la interfaz de usuario administrador de Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy.png)   

   >[!NOTE] 
   >Espere unos instantes para que Ranger se sincronice con Azure AD si un usuario del dominio no se rellena automáticamente en **Seleccionar usuario**.

4. Haga clic en **Agregar** para guardar la directiva.

5. Haga clic en **Agregar nueva directiva** y escriba los siguientes valores:

   |**Configuración**  |**Valor sugerido**  |
   |---------|---------|
   |Nombre de la directiva  |  hdi marketing policy   |
   |Tema   |  marketingspend |
   |Seleccionar usuario  |  marketing_user1 |
   |Permisos  | publicar, consumir, crear |

   ![Directiva de creación de la interfaz de usuario administrador de Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy-2.png)  

6. Haga clic en **Agregar** para guardar la directiva.

## <a name="create-topics-in-a-kafka-cluster-with-esp"></a>Creación de temas en un clúster de Kafka con ESP

Para crear dos temas **salesevents** y **marketingspend**:

1. Use el siguiente comando para abrir una conexión SSH al clúster:

   ```bash
   ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
   ```

   Reemplace `SSHUSER` por el usuario de SSH del clúster y `CLUSTERNAME` por el nombre de su clúster. Si se le solicita, escriba la contraseña de la cuenta de usuario de SSH. Para obtener más información sobre cómo usar `scp` con HDInsight, consulte [Conexión a través de SSH con HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix).

   En un escenario de producción, los usuarios del dominio configurados durante la creación del clúster pueden acceder mediante SSH al clúster.

2. Use los siguientes comandos para guardar el nombre del clúster en una variable e instalar una utilidad de análisis de JSON `jq`. Cuando se le solicite, escriba el nombre del clúster de Kafka.

   ```bash
   sudo apt -y install jq
   read -p 'Enter your Kafka cluster name:' CLUSTERNAME
   ```

3. Use los siguientes comandos para obtener los hosts del agente de Kafka y los hosts de Zookeeper. Cuando se le solicite, escriba la contraseña de la cuenta de administrador del clúster.

   ```bash
   export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
   
   export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
   ```

4. Ejecute los comandos siguientes: 

   ```bash
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create salesevents $KAFKABROKERS
   
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create marketingspend $KAFKABROKERS
   ```

   >[!NOTE] 
   >Solo el propietario del proceso del servicio de Kafka, como raíz, puede escribir en znodes de Zookeeper `/config/topics`. No se aplican las directivas de Ranger cuando un usuario sin privilegios crea un tema. Esto es porque el script `kafka-topics.sh` se comunica directamente con Zookeeper para crear el tema. Las entradas se agregan a los nodos de Zookeeper, mientras que los monitores en el lado del agente supervisan y crean temas según corresponda. La autorización no se puede realizar mediante el complemento de Ranger, y el comando anterior se ejecuta mediante `sudo` a través del agente de Kafka.


## <a name="test-the-ranger-policies"></a>Prueba de las directivas de Ranger

Según las directivas de Ranger configuradas, **sales_user** puede producir o consumir el tema **salesevents**, pero no el tema **marketingspend**. Por el contrario, **marketing_user** puede producir o consumir el tema **marketingspend**, pero no el tema **salesevents**.

1. Abra una nueva conexión SSH al clúster. Use el siguiente comandos para iniciar sesión como **sales_user1**:

   ```bash
   ssh sales_user1@CLUSTERNAME-ssh.azurehdinsight.net
   ```

2. Ejecute el comando siguiente:

   ```bash
   export KAFKA_OPTS="-Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf"
   ```

3. Use los nombres de agente y de Zookeeper de la sección anterior para establecer las siguientes variables de entorno:

   ```bash
   export KAFKABROKERS=<brokerlist>:9092 
   ```

   Ejemplo: `export KAFKABROKERS=wn0-khdicl.contoso.com:9092,wn1-khdicl.contoso.com:9092`

   ```bash
   export KAFKAZKHOSTS=<zklist>:2181
   ```

   Ejemplo: `export KAFKAZKHOSTS=zk1-khdicl.contoso.com:2181,zk2-khdicl.contoso.com:2181`

4. Compruebe que **sales_user1** puede producir al tema **salesevents**.
   
   Ejecute el siguiente comando para iniciar el productor de la consola para el tema **salesevents**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic salesevents --security-protocol SASL_PLAINTEXT
   ```

   A continuación, escriba unos cuantos mensajes en la consola. Presione **Ctrl + C** para salir del productor de la consola.

5. Ejecute el siguiente comando para consumir del tema **salesevents**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic salesevents --security-protocol PLAINTEXTSASL --from-beginning
   ```
 
6. Compruebe que aparezcan los mensajes que escribió en el paso anterior, y que **sales_user1** no puede producir al tema **marketingspend**.

   Desde la misma ventana ssh anterior, ejecute el siguiente comando para producir al tema **marketingspend**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic marketingspend --security-protocol SASL_PLAINTEXT
   ```

   Se produce un error de autorización y se puede omitir. 

7. Tenga en cuenta que **marketing_user1** no puede consumir del tema **salesevents**.

   Repita los pasos 1 a 3 anteriores, pero esta vez como **marketing_user1**.

   Ejecute el siguiente comando para consumir del tema **salesevents**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic marketingspend --security-protocol PLAINTEXTSASL --from-beginning
   ```

   No se pueden ver los mensajes anteriores.

8. Vea los eventos de acceso de auditoría desde la interfaz de usuario de Ranger.

   ![Auditoría de directivas de la interfaz de usuario de Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-audit.png)

## <a name="next-steps"></a>Pasos siguientes

* [Traiga su propia clave a Kafka](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok)
* [Introducción a la seguridad de Hadoop con Enterprise Security Package](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction)
