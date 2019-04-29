---
title: Configuración del cifrado y la autenticación SSL para Apache Kafka en Azure HDInsight
description: Configure el cifrado SSL para la comunicación entre clientes y agentes de Kafka, así como entre estos últimos. Configuración de la autenticación SSL de clientes.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
origin.date: 01/15/2019
ms.date: 04/15/2019
ms.author: v-yiso
ms.openlocfilehash: 9d8d5e57d0dd7d7022e65a061360c8450848fb4b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62114964"
---
# <a name="setup-secure-sockets-layer-ssl-encryption-and-authentication-for-apache-kafka-in-azure-hdinsight"></a>Configuración del cifrado y la autenticación de Capa de sockets seguros (SSL) para Apache Kafka en Azure HDInsight

En este artículo se describe cómo configurar el cifrado SSL entre los clientes y los agentes de Apache Kafka. También se proporcionan los pasos necesarios para configurar la autenticación de clientes (a veces conocido como SSL bidireccional).

## <a name="server-setup"></a>Configuración del servidor

El primer paso es crear un almacén de claves y un almacén de confianza en cada agente de Kafka. Después de crearlos, importe los certificados de entidad de certificación (CA) y del agente a estos almacenes.

> [!Note] 
> En esta guía se usarán los certificados autofirmados, pero la solución más segura es usar certificados emitidos por entidades de certificación de confianza.

Siga este procedimiento para completar la configuración del servidor:

1. Cree una carpeta denominada ssl y exporte la contraseña del servidor como una variable de entorno. En el resto de esta guía, reemplace `<server_password>` por la contraseña de administrador real del servidor.
1. A continuación, cree un almacén de claves Java (kafka.server.keystore.jks) y un certificado de entidad de certificación.
1. Seguidamente, cree una solicitud de firma para obtener el certificado creado en el paso anterior firmado por la entidad de certificación.
1. Ahora, envíe la solicitud de firma a la entidad de certificación y obtenga este certificado firmado. Como estamos usando un certificado autofirmado, el certificado se firmará con nuestra entidad de certificación con el comando `openssl`.
1. Cree un almacén de confianza e importe el certificado de la entidad de certificación.
1. Importe el certificado de entidad de certificación público en el almacén de claves.
1. Importe el certificado firmado en el almacén de claves.

Los comandos para completar estos pasos se muestran en el siguiente fragmento de código.

```bash
export SRVPASS=<server_password>
mkdir ssl
cd ssl

# Create a java keystore (kafka.server.keystore.jks) and a CA certificate.

keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass $SRVPASS -keypass $SRVPASS -dname "CN=wn0-umakaf.xvbseke35rbuddm4fyvhm2vz2h.cx.internal.cloudapp.net" -storetype pkcs12

# Create a signing request to get the certificate created in the previous step signed by the CA.

keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass $SRVPASS -keypass $SRVPASS

# Send the signing request to the CA and get this certificate signed.

openssl x509 -req -CA ca-cert -CAkey ca-key -in cert-file -out cert-signed -days 365 -CAcreateserial -passin pass:$SRVPASS

# Create a trust store and import the certificate of the CA.

keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# Import the public CA certificate into the keystore.

keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# Import the signed certificate into the keystore.

keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# The output should say "Certificate reply was added to keystore"
```

Importar el certificado firmado en el almacén de claves es el último paso necesario para configurar el almacén de confianza y el almacén de claves para un agente de Kafka.

## <a name="update-kafka-configuration-to-use-ssl-and-restart-brokers"></a>Actualización de la configuración de Kafka para usar SSL y reiniciar los agentes

Ya ha configurado cada agente de Kafka con un almacén de claves y un almacén de confianza y ha importado los certificados correctos.  A continuación, modifique las propiedades de la configuración de Kafka relacionadas mediante Ambari y luego reinicie los agentes de Kafka. 

Realice los pasos siguientes para completar la modificación de la configuración:

1. Inicie sesión en Azure Portal y seleccione el clúster de Apache Kafka de Azure HDInsight.
1. Vaya a la interfaz de usuario de Ambari; para ello, haga clic en **Ambari home** (Inicio de Ambari) en **Cluster dashboards** (Paneles del clúster).
1. En **Kafka Broker** (Agente de Kafka), establezca la propiedad **listeners** en `PLAINTEXT://localhost:9092,SSL://localhost:9093`.
1. En **Advanced kafka-broker** (Agente de Kafka avanzado), establezca la propiedad **security.inter.broker.protocol** en `SSL`

    ![Edición de las propiedades de configuración de SSL de Kafka en Ambari](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari.png)

1. En **Custom kafka-broker** (Agente de Kafka personalizado), establezca la propiedad **ssl.client.auth** en `required`. Este paso solo es necesario si va a configurar la autenticación y el cifrado.

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

Realice los pasos siguientes para completar la configuración del cliente:

1. Inicie sesión en el equipo cliente (hn1).
1. Exporte la contraseña del cliente. Reemplace `<client_password>` por la contraseña de administrador real en la máquina cliente de Kafka.
1. Cree un almacén de claves Java y obtenga un certificado firmado para el agente. A continuación, copie el certificado en la máquina virtual donde se ejecuta la entidad de certificación.
1. Cambie a la máquina de la entidad de certificación (wn0) para firmar el certificado de cliente.
1. Vaya a la máquina cliente (hn1) y desplácese hasta la carpeta `~/ssl`. Copie el certificado firmado en la máquina cliente.

```bash
export CLIPASS=<client_password>
cd ssl

# Create a java keystore and get a signed certificate for the broker. Then copy the certificate to the VM where the CA is running.

keytool -genkey -keystore kafka.client.keystore.jks -validity 365 -storepass $CLIPASS -keypass $CLIPASS -dname "CN=mylaptop1" -alias my-local-pc1 -storetype pkcs12

keytool -keystore kafka.client.keystore.jks -certreq -file client-cert-sign-request -alias my-local-pc1 -storepass $CLIPASS -keypass $CLIPASS

# Copy the cert to the vm where the CA is
scp client-cert-sign-request3 sshuser@wn0-umakaf:~/tmp1/client-cert-sign-request

# Switch to the CA machine (wn0) to sign the client certificate.
cd ssl
openssl x509 -req -CA ca-cert -CAkey ca-key -in /tmp1/client-cert-sign-request -out /tmp1/client-cert-signed -days 365 -CAcreateserial -passin pass:<server_password>

# Return to the client machine (hn1), navigate to ~/ssl folder and copy signed cert to client machine
scp -i ~/kafka-security.pem sshuser@wn0-umakaf:/tmp1/client-cert-signed

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

Si no necesita realizar la autenticación, los pasos para configurar solamente el cifrado SSL son:

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