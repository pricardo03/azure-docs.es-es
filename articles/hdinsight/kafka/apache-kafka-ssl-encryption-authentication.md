---
title: Configurar el cifrado SSL y autenticación de Apache Kafka en HDInsight de Azure
description: Configurar el cifrado SSL para la comunicación entre los clientes de Kafka y agentes de Kafka, así como entre los agentes de Kafka. Configurar la autenticación de clientes SSL.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: hrasheed
ms.openlocfilehash: e526908f5ba9feea53b1c1abebbbfc1bd9a51c54
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147958"
---
# <a name="set-up-secure-sockets-layer-ssl-encryption-and-authentication-for-apache-kafka-in-azure-hdinsight"></a>Configuración de cifrado de capa de Sockets seguros (SSL) y la autenticación de Apache Kafka en HDInsight de Azure

Este artículo muestra cómo configurar el cifrado SSL entre clientes de Apache Kafka y agentes de Apache Kafka. También se muestra cómo configurar la autenticación de clientes (a veces denominados SSL bidireccional).

> [!Important]
> Existen dos clientes que se pueden usar para las aplicaciones de Kafka: un cliente de Java y un cliente de consola. Solo el cliente de Java `ProducerConsumer.java` puede utilizar SSL para producir y consumir. El cliente de productor de consola `console-producer.sh` no funciona con SSL.

## <a name="apache-kafka-broker-setup"></a>Programa de instalación de Apache Kafka Broker

El programa de instalación del agente de Kafka SSL usará cuatro máquinas virtuales del clúster de HDInsight en la siguiente manera:

* nodo principal 0 - la entidad de certificación (CA)
* agentes de nodo de trabajo, 0, 1 y 2:

> [!Note] 
> En esta guía se usarán los certificados autofirmados, pero la solución más segura es usar certificados emitidos por entidades de certificación de confianza.

El resumen del proceso de instalación de agente es como sigue:

1. Los siguientes pasos se repiten en cada uno de los nodos de trabajo de tres:

    1. Generar un certificado.
    1. Crear un solicitud de firma de certificado.
    1. Envíe el certificado de firma de solicitud a la entidad de certificación (CA).
    1. Inicie sesión en la entidad emisora de certificados y firmar la solicitud.
    1. El certificado de firma en el nodo de trabajo de SCP.
    1. El certificado público de la entidad de certificación al nodo de trabajo de SCP.

1. Una vez que tenga todos los certificados, coloque los certificados en el almacén de certificados.
1. Vaya a Ambari y cambiar las configuraciones.

Utilice las siguientes instrucciones detalladas para completar la instalación del agente:

> [!Important]
> En los siguientes fragmentos de código es una abreviatura de uno de los nodos de tres trabajo wnX y debe sustituirse con `wn0`, `wn1` o `wn2` según corresponda. `WorkerNode0_Name` y `HeadNode0_Name` debe sustituirse con los nombres de los respectivos equipos, como `wn0-abcxyz` o `hn0-abcxyz`.

1. Realizar la instalación inicial en un nodo principal 0, lo que, para HDInsight rellenará el rol de la entidad de certificación (CA).

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl

    # Export
    export SRVPASS=MyServerPassword123
    ```

1. Realice la misma configuración inicial en cada uno de los agentes (nodos de trabajo 0, 1 y 2).

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl

    # Export
    export MyServerPassword123=MyServerPassword123
    ```

1. En cada uno de los nodos de trabajo, ejecute los pasos siguientes con el siguiente fragmento de código.
    1. Crear un almacén de claves y rellenarla con un nuevo certificado privado.
    1. Crear una solicitud de firma de certificado.
    1. La solicitud de firma de certificado a la entidad de certificación (headnode0) de SCP

    ```bash
    keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass "MyServerPassword123" -keypass "MyServerPassword123" -dname "CN=FQDN_WORKER_NODE" -storetype pkcs12
    keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass "MyServerPassword123" -keypass "MyServerPassword123"
    scp cert-file sshuser@HeadNode0_Name:~/ssl/wnX-cert-sign-request
    ```

1. Cambie a la máquina de la entidad emisora de certificados y firmar el certificado recibido solicitudes de firma todos:

    ```bash
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn0-cert-sign-request -out wn0-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn1-cert-sign-request -out wn1-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn2-cert-sign-request -out wn2-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    ```

1. Devolver los certificados autofirmados a los nodos de trabajo de la entidad de certificación (headnode0).

    ```bash
    scp wn0-cert-signed sshuser@WorkerNode0_Name:~/ssl/cert-signed
    scp wn1-cert-signed sshuser@WorkerNode1_Name:~/ssl/cert-signed
    scp wn2-cert-signed sshuser@WorkerNode2_Name:~/ssl/cert-signed
    ```

1. Enviar el certificado público de la entidad de certificación a cada nodo de trabajo.

    ```bash
    scp ca-cert sshuser@WorkerNode0_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode1_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode2_Name:~/ssl/ca-cert
    ```

1. En cada nodo de trabajo, agregue el certificado público de entidades emisoras de certificados al almacén de claves y truststore. A continuación, agregue el certificado autofirmado del nodo de trabajo para el almacén de claves

    ```bash
    keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -import -file cert-signed -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt

    ```

## <a name="update-kafka-configuration-to-use-ssl-and-restart-brokers"></a>Actualización de la configuración de Kafka para usar SSL y reiniciar los agentes

Ahora ha configurado cada agente de Kafka con un almacén de claves y truststore e importado los certificados correctos. A continuación, modifique las propiedades de la configuración de Kafka relacionadas mediante Ambari y luego reinicie los agentes de Kafka.

Realice los pasos siguientes para completar la modificación de la configuración:

1. Inicie sesión en Azure Portal y seleccione el clúster de Apache Kafka de Azure HDInsight.
1. Vaya a la interfaz de usuario de Ambari; para ello, haga clic en **Ambari home** (Inicio de Ambari) en **Cluster dashboards** (Paneles del clúster).
1. En **Kafka Broker** (Agente de Kafka), establezca la propiedad **listeners** en `PLAINTEXT://localhost:9092,SSL://localhost:9093`.
1. En **Advanced kafka-broker** (Agente de Kafka avanzado), establezca la propiedad **security.inter.broker.protocol** en `SSL`

    ![Edición de las propiedades de configuración de SSL de Kafka en Ambari](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari.png)

1. En **Custom kafka-broker** (Agente de Kafka personalizado), establezca la propiedad **ssl.client.auth** en `required`. Este paso sólo es necesario si está configurando la autenticación y cifrado.

    ![Edición de las propiedades de configuración de SSL de Kafka en Ambari](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari2.png)

1. Agregue propiedades de configuración al archivo `server.properties` de Kafka para anunciar direcciones IP en lugar del nombre de dominio completo (FQDN).

    ```bash
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092,SSL://$IP_ADDRESS:9093" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.key.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

1. Para comprobar que los cambios anteriores se han realizado correctamente, tiene la opción de comprobar que las líneas siguientes están presentes en el archivo `server.properties` de Kafka.

    ```bash
    advertised.listeners=PLAINTEXT://10.0.0.11:9092,SSL://10.0.0.11:9093
    ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks
    ssl.keystore.password=<server_password>
    ssl.key.password=<server_password>
    ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks
    ssl.truststore.password=<server_password>
    ```

1. Reinicie todos los agentes de Kafka.

## <a name="client-setup-with-authentication"></a>Configuración del cliente (con autenticación)

> [!Note]
> Los pasos siguientes solo son necesarios si va a configurar el cifrado **y** la autenticación SSL. Si simplemente va a configurar el cifrado, continúe con [Configuración del cliente sin autenticación](apache-kafka-ssl-encryption-authentication.md#client-setup-without-authentication)

Complete los pasos siguientes para finalizar la instalación del cliente:

1. Inicie sesión en el equipo cliente (hn1).
1. Exporte la contraseña del cliente. Reemplace `<client_password>` por la contraseña de administrador real en la máquina cliente de Kafka.
1. Cree un almacén de claves Java y obtenga un certificado firmado para el agente. A continuación, copie el certificado en la máquina virtual donde se ejecuta la entidad de certificación.
1. Cambie a la máquina de la entidad de certificación (hn0) para firmar el certificado de cliente.
1. Vaya a la máquina cliente (hn1) y desplácese hasta la carpeta `~/ssl`. Copie el certificado firmado en la máquina cliente.

```bash
export CLIPASS=<client_password>
cd ssl

# Create a java keystore and get a signed certificate for the broker. Then copy the certificate to the VM where the CA is running.

keytool -genkey -keystore kafka.client.keystore.jks -validity 365 -storepass $CLIPASS -keypass $CLIPASS -dname "CN=mylaptop1" -alias my-local-pc1 -storetype pkcs12

keytool -keystore kafka.client.keystore.jks -certreq -file client-cert-sign-request -alias my-local-pc1 -storepass $CLIPASS -keypass $CLIPASS

# Copy the cert to the CA
scp client-cert-sign-request3 sshuser@HeadNode0_Name:~/tmp1/client-cert-sign-request

# Switch to the CA machine (hn0) to sign the client certificate.
cd ssl
openssl x509 -req -CA ca-cert -CAkey ca-key -in /tmp1/client-cert-sign-request -out /tmp1/client-cert-signed -days 365 -CAcreateserial -passin pass:<server_password>

# Return to the client machine (hn1), navigate to ~/ssl folder and copy signed cert from the CA (hn0) to client machine
scp -i ~/kafka-security.pem sshuser@HeadNode0_Name:/tmp1/client-cert-signed

# Import CA cert to trust store
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import CA cert to key store
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import signed client (cert client-cert-signed1) to keystore
keytool -keystore kafka.client.keystore.jks -import -file client-cert-signed -alias my-local-pc1 -storepass $CLIPASS -keypass $CLIPASS -noprompt
```

Para finalizar, consulte el archivo `client-ssl-auth.properties` con el comando `cat client-ssl-auth.properties`. Debe tener las siguientes líneas:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=<client_password>
ssl.keystore.location=/home/sshuser/ssl/kafka.client.keystore.jks
ssl.keystore.password=<client_password>
ssl.key.password=<client_password>
```

## <a name="client-setup-without-authentication"></a>Configuración del cliente (sin autenticación)

Si no necesita autenticación, los pasos para configurar el cifrado de SSL solo son:

1. Inicie sesión en la máquina cliente (hn1) y desplácese hasta la carpeta `~/ssl`.
1. Exporte la contraseña del cliente. Reemplace `<client_password>` por la contraseña de administrador real en la máquina cliente de Kafka.
1. Copie el certificado firmado en la máquina cliente de la máquina de CA (wn0).
1. Importar el certificado de CA al almacén de confianza
1. Importar el certificado de CA al almacén de claves

Estos pasos se muestran en el siguiente fragmento de código.

```bash
export CLIPASS=<client_password>
cd ssl

# Copy signed cert to client machine
scp -i ~/kafka-security.pem sshuser@wn0-umakaf:/home/sshuser/ssl/ca-cert .

# Import CA cert to truststore
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import CA cert to keystore
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file cert-signed -storepass $CLIPASS -keypass $CLIPASS -noprompt
```

Para finalizar, consulte el archivo `client-ssl-auth.properties` con el comando `cat client-ssl-auth.properties`. Debe tener las siguientes líneas:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=<client_password>
```

## <a name="next-steps"></a>Pasos siguientes

* [Qué es Apache Kafka en HDInsight](apache-kafka-introduction.md)
