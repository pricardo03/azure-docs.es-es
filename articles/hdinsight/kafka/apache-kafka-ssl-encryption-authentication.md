---
title: 'Cifrado y autenticación SSL de Apache Kafka: Azure HDInsight'
description: Configure el cifrado SSL para la comunicación entre los clientes y los agentes de Kafka, así como entre estos últimos. Configure la autenticación SSL de clientes.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: hrasheed
ms.openlocfilehash: 180b7c203755553c343e0f7fc65c93092b330124
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75751314"
---
# <a name="set-up-secure-sockets-layer-ssl-encryption-and-authentication-for-apache-kafka-in-azure-hdinsight"></a>Configuración del cifrado y la autenticación de Capa de sockets seguros (SSL) para Apache Kafka en Azure HDInsight

En este artículo se describe cómo configurar el cifrado SSL entre los clientes y los agentes de Apache Kafka. También se muestra cómo configurar la autenticación de los clientes (lo que a veces se conoce como SSL bidireccional).

> [!Important]
> Existen dos clientes que se pueden usar para las aplicaciones de Kafka: uno de Java y otro de consola. Solo el cliente de Java `ProducerConsumer.java` puede utilizar SSL para producir y consumir. El cliente de productor de consola `console-producer.sh` no funciona con SSL.

## <a name="apache-kafka-broker-setup"></a>Programa de instalación de Apache Kafka Broker

El programa de instalación del agente de Kafka SSL usará cuatro máquinas virtuales del clúster de HDInsight en la siguiente manera:

* Nodo principal 0: entidad de certificación (CA)
* nodo de trabajo 0, 1 y 2: agentes

> [!Note] 
> En esta guía se usarán los certificados autofirmados, pero la solución más segura es usar certificados emitidos por entidades de certificación de confianza.

El resumen del proceso de instalación de agente es como sigue:

1. Los siguientes pasos se repiten en cada uno de los tres nodos de trabajo:

    1. Genere un certificado.
    1. Cree una solicitud de firma de certificado.
    1. Envíe la solicitud de firma del certificado a la entidad de certificación (CA).
    1. Inicie sesión en la entidad de certificación y firme la solicitud.
    1. SCP el certificado firmado de vuelta en el nodo de trabajo.
    1. SCP el certificado público de la entidad de certificación para el nodo de trabajo.

1. Una vez que tenga todos los certificados, colóquelos en el almacén de certificados.
1. Vaya a Ambari y cambie las configuraciones.

Utilice las siguientes instrucciones detalladas para completar la instalación del agente:

> [!Important]
> En los siguientes fragmentos de código, wnX es una abreviatura de uno de los tres nodos de trabajo y debe sustituirse por `wn0`, `wn1` o `wn2`, según corresponda. `WorkerNode0_Name` y `HeadNode0_Name` deben sustituirse por los nombres de las máquinas respectivas.

1. Realice la instalación inicial en un nodo principal 0, lo que, para HDInsight, rellenará el rol de la entidad de certificación (CA).

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl
    ```

1. Realice la misma configuración inicial en cada uno de los agentes (nodos de trabajo 0, 1 y 2).

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl
    ```

1. En cada uno de los nodos de trabajo, ejecute los pasos siguientes con el siguiente fragmento de código.
    1. Cree un almacén de claves y rellénelo con un nuevo certificado privado.
    1. Cree una solicitud de firma de certificados.
    1. SCP la solicitud de firma de certificado a la entidad de certificación (headnode0)

    ```bash
    keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass "MyServerPassword123" -keypass "MyServerPassword123" -dname "CN=FQDN_WORKER_NODE" -storetype pkcs12
    keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass "MyServerPassword123" -keypass "MyServerPassword123"
    scp cert-file sshuser@HeadNode0_Name:~/ssl/wnX-cert-sign-request
    ```

1. En el equipo de la entidad de certificación, ejecute el siguiente comando para crear los archivos ca-cert y ca-key:

    ```bash
    openssl req -new -newkey rsa:4096 -days 365 -x509 -subj "/CN=Kafka-Security-CA" -keyout ca-key -out ca-cert -nodes
    ```

1. Cambie a la máquina de la entidad de certificación y firme todas las solicitudes de firma de certificado recibidas:

    ```bash
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn0-cert-sign-request -out wn0-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn1-cert-sign-request -out wn1-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn2-cert-sign-request -out wn2-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    ```

1. Envíe de vuelta los certificados firmados a los nodos de trabajo de la entidad de certificación (headnode0).

    ```bash
    scp wn0-cert-signed sshuser@WorkerNode0_Name:~/ssl/cert-signed
    scp wn1-cert-signed sshuser@WorkerNode1_Name:~/ssl/cert-signed
    scp wn2-cert-signed sshuser@WorkerNode2_Name:~/ssl/cert-signed
    ```

1. SCP el certificado público de la entidad de certificación para cada nodo de trabajo.

    ```bash
    scp ca-cert sshuser@WorkerNode0_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode1_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode2_Name:~/ssl/ca-cert
    ```

1. En cada nodo de trabajo, agregue el certificado público de la entidad de certificación al almacén de claves y truststore. A continuación, agregue el certificado firmado del nodo de trabajo al almacén de claves.

    ```bash
    keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -import -file cert-signed -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt

    ```

## <a name="update-kafka-configuration-to-use-ssl-and-restart-brokers"></a>Actualización de la configuración de Kafka para usar SSL y reiniciar los agentes

Ya ha configurado cada agente de Kafka con un almacén de claves y truststore, y ha importado los certificados correctos. A continuación, modifique las propiedades de la configuración de Kafka relacionadas mediante Ambari y luego reinicie los agentes de Kafka.

Realice los pasos siguientes para completar la modificación de la configuración:

1. Inicie sesión en Azure Portal y seleccione el clúster de Apache Kafka de Azure HDInsight.
1. Vaya a la interfaz de usuario de Ambari; para ello, haga clic en **Ambari home** (Inicio de Ambari) en **Cluster dashboards** (Paneles del clúster).
1. En **Kafka Broker** (Agente de Kafka), establezca la propiedad **listeners** en `PLAINTEXT://localhost:9092,SSL://localhost:9093`.
1. En **Advanced kafka-broker** (Agente de Kafka avanzado), establezca la propiedad **security.inter.broker.protocol** en `SSL`

    ![Edición de las propiedades de configuración de SSL de Kafka en Ambari](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari.png)

1. En **Custom kafka-broker** (Agente de Kafka personalizado), establezca la propiedad **ssl.client.auth** en `required`. Este paso solo es necesario si va a configurar la autenticación y el cifrado.

    ![Edición de las propiedades de configuración de SSL de Kafka en Ambari](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari2.png)

1. En **Advanced kafka-env** (Entorno avanzado de Kafka) agregue las líneas siguientes al final de la propiedad **kafka-env template**.

    ```config
    # Needed to configure IP address advertising
    ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks
    ssl.keystore.password=MyServerPassword123
    ssl.key.password=MyServerPassword123
    ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks
    ssl.truststore.password=MyServerPassword123
    ```

    ![Edición de la propiedad kafka-env template en Ambari](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-kafka-env.png)

1. Reinicie todos los agentes de Kafka.
1. Inicie el cliente de administración con las opciones de productor y consumidor para comprobar que tanto los productores como los consumidores están en funcionamiento en el puerto 9093.

## <a name="client-setup-with-authentication"></a>Configuración del cliente (con autenticación)

> [!Note]
> Los pasos siguientes solo son necesarios si va a configurar el cifrado **y** la autenticación SSL. Si simplemente va a configurar el cifrado, continúe con [Configuración del cliente sin autenticación](apache-kafka-ssl-encryption-authentication.md#client-setup-without-authentication)

Realice los pasos siguientes para finalizar la configuración del cliente:

1. Inicie sesión en la máquina cliente (nodo principal en espera).
1. Cree un almacén de claves Java y obtenga un certificado firmado para el agente. A continuación, copie el certificado en la máquina virtual donde se ejecuta la entidad de certificación.
1. Cambie a la máquina de la entidad de certificación (nodo principal activo) para firmar el certificado de cliente.
1. Vaya a la máquina cliente (nodo principal en espera) y desplácese hasta la carpeta `~/ssl`. Copie el certificado firmado en la máquina cliente.

```bash
cd ssl

# Create a java keystore and get a signed certificate for the broker. Then copy the certificate to the VM where the CA is running.

keytool -genkey -keystore kafka.client.keystore.jks -validity 365 -storepass "MyClientPassword123" -keypass "MyClientPassword123" -dname "CN=mylaptop1" -alias my-local-pc1 -storetype pkcs12

keytool -keystore kafka.client.keystore.jks -certreq -file client-cert-sign-request -alias my-local-pc1 -storepass "MyClientPassword123" -keypass "MyClientPassword123"

# Copy the cert to the CA
scp client-cert-sign-request3 sshuser@HeadNode0_Name:~/tmp1/client-cert-sign-request

# Switch to the CA machine (active head node) to sign the client certificate.
cd ssl
openssl x509 -req -CA ca-cert -CAkey ca-key -in /tmp1/client-cert-sign-request -out /tmp1/client-cert-signed -days 365 -CAcreateserial -passin pass:MyServerPassword123

# Return to the client machine (standby head node), navigate to ~/ssl folder and copy signed cert from the CA (active head node) to client machine
scp -i ~/kafka-security.pem sshuser@HeadNode0_Name:/tmp1/client-cert-signed

# Import CA cert to trust store
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt

# Import CA cert to key store
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt

# Import signed client (cert client-cert-signed1) to keystore
keytool -keystore kafka.client.keystore.jks -import -file client-cert-signed -alias my-local-pc1 -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
```

Para finalizar, consulte el archivo `client-ssl-auth.properties` con el comando `cat client-ssl-auth.properties`. Debe tener las siguientes líneas:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=MyClientPassword123
ssl.keystore.location=/home/sshuser/ssl/kafka.client.keystore.jks
ssl.keystore.password=MyClientPassword123
ssl.key.password=MyClientPassword123
```

## <a name="client-setup-without-authentication"></a>Configuración del cliente (sin autenticación)

Si no necesita realizar la autenticación, los pasos para configurar solamente el cifrado SSL son:

1. Inicie sesión en la máquina cliente (hn1) y desplácese hasta la carpeta `~/ssl`.
1. Copie el certificado firmado en la máquina cliente de la máquina de CA (wn0).
1. Importar el certificado de CA al almacén de confianza
1. Importar el certificado de CA al almacén de claves

Estos pasos se muestran en el siguiente fragmento de código.

```bash
cd ssl

# Copy signed cert to client machine
scp -i ~/kafka-security.pem sshuser@wn0-umakaf:/home/sshuser/ssl/ca-cert .

# Import CA cert to truststore
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt

# Import CA cert to keystore
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file cert-signed -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
```

Para finalizar, consulte el archivo `client-ssl-auth.properties` con el comando `cat client-ssl-auth.properties`. Debe tener las siguientes líneas:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=MyClientPassword123
```

## <a name="next-steps"></a>Pasos siguientes

* [Qué es Apache Kafka en HDInsight](apache-kafka-introduction.md)
