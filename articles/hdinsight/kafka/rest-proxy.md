---
title: 'Proxy de REST de Apache Kafka: Azure HDInsight'
description: Obtenga información sobre cómo realizar operaciones de Apache Kafka mediante un proxy de REST de Kafka en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: bc6859d29a574cea0d97989977ba9a333b20f6c4
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77157147"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>Interacción con clústeres de Apache Kafka en Azure HDInsight mediante un proxy de REST

El proxy de REST de Kafka le permite interactuar con el clúster de Kafka mediante una API REST a través de HTTP. Esto significa que los clientes de Kafka pueden estar fuera de la red virtual. Además, los clientes pueden realizar llamadas HTTP sencillas para enviar y recibir mensajes en el clúster de Kafka, en lugar de depender de las bibliotecas de Kafka.  

## <a name="background"></a>Información previa

### <a name="architecture"></a>Architecture

Sin un proxy de REST, los clientes de Kafka deben estar en la misma red virtual que el clúster de Kafka o en una red virtual emparejada. El proxy de REST permite conectar a los productores o consumidores de datos ubicados en cualquier lugar. La implementación del proxy de REST crea un nuevo punto de conexión público para el clúster, que puede encontrar en la configuración del portal.

![Arquitectura del proxy de REST de Kafka](./media/rest-proxy/rest-proxy-architecture.png)

Para obtener la especificación completa de las operaciones que admite la API, consulte [API de proxy de REST de Apache Kafka](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy).

### <a name="security"></a>Seguridad

El acceso al proxy de REST de Kafka se administra con grupos de seguridad de Azure Active Directory. Para obtener más información, consulte [Administración del acceso a recursos y aplicaciones con grupos en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups).

Al crear el clúster de Kafka con el proxy de REST habilitado, proporcionará el grupo de seguridad de AAD que debe tener acceso al punto de conexión de REST. El propietario del grupo debe registrar en este grupo los clientes de Kafka (aplicaciones) que necesitan acceso al proxy de REST. El propietario del grupo puede hacerlo a través del portal o mediante PowerShell.

Antes de realizar solicitudes al punto de conexión del proxy de REST, la aplicación cliente debe obtener un token de OAuth para verificar la pertenencia del grupo de seguridad adecuado. Para obtener más información sobre cómo funcionan los tokens de OAuth, consulte [Autorización del acceso a aplicaciones web de Azure Active Directory mediante el flujo de concesión de código OAuth 2.0](../../active-directory/azuread-dev/v1-protocols-oauth-code.md). Para obtener un ejemplo de captura de un token de OAuth en Python, consulte el apartado [Ejemplo de aplicación cliente](#client-application-sample).

Una vez que la aplicación cliente tenga el token de OAuth, debe pasar ese token en la solicitud HTTP realizada al proxy de REST.

## <a name="prerequisites"></a>Prerrequisitos

1. Registrar una aplicación con Azure AD. Las aplicaciones cliente que escriba para interactuar con el proxy de REST de Kafka usarán el identificador y el secreto de la aplicación para autenticarse en Azure.
1. Cree un grupo de seguridad de Azure AD y agregue la aplicación que ha registrado con Azure AD al grupo de seguridad. Este grupo de seguridad se usará para controlar qué aplicaciones pueden interactuar con el proxy de REST. Para obtener más información sobre cómo crear grupos de Azure AD, consulte [Creación de un grupo básico e incorporación de miembros con Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>Creación de un clúster de Kafka con el proxy de REST habilitado

1. Durante el flujo de trabajo de creación del clúster de Kafka, en la pestaña "Seguridad y redes", active la opción "Enable Kafka REST proxy" ("Habilitar proxy de REST de Kafka").

     ![Habilitación del proxy de REST de Kafka y selección del grupo de seguridad](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest.png)

1. Haga clic en **Seleccionar grupo de seguridad**. En la lista de grupos de seguridad, seleccione el que quiere que tenga acceso al proxy de REST. Puede usar el cuadro de búsqueda para buscar el grupo de seguridad adecuado. Haga clic en el botón **Seleccionar** de la parte inferior.

     ![Habilitación del proxy de REST de Kafka y selección del grupo de seguridad](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest2.png)

1. Complete los pasos restantes para crear el clúster como se describe en [Creación de un clúster de Apache Kafka en Azure HDInsight mediante Azure Portal](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started).

1. Una vez creado el clúster, vaya a las propiedades del clúster para registrar la URL del proxy de REST de Kafka.

     ![ver la URL de proxy de REST de Kafka](./media/rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>Nombre de la aplicación cliente

Puede usar el código de Python siguiente para interactuar con el proxy de REST en el clúster de Kafka. Este código hace lo siguiente:

1. Obtiene un token de OAuth de Azure AD.
1. Crea el tema especificado.
1. Envía mensajes a ese tema.
1. Consume mensajes de ese tema.

Para obtener más información sobre cómo obtener tokens de OAuth en Python, consulte [Clase AuthenticationContext de Python](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python). Es posible que vea un retraso mientras los temas que no se crean o eliminan a través del proxy de REST de Kafka se reflejan ahí. Este se debe a la actualización de la memoria caché.

```python
#Required python packages
#pip3 install adal
#pip install msrestazure

import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
import requests

#--------------------------Configure these properties-------------------------------#
# Tenant ID for your Azure Subscription
tenant_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Application Id
client_id = 'XYZABCDE-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Credentials
client_secret = 'password'
# kafka rest proxy -endpoint
kafkarest_endpoint = "https://<clustername>-kafkarest.azurehdinsight.net"
#--------------------------Configure these properties-------------------------------#

#getting token
login_endpoint = AZURE_PUBLIC_CLOUD.endpoints.active_directory
resource = "https://hib.azurehdinsight.net"
context = adal.AuthenticationContext(login_endpoint + '/' + tenant_id)

token = context.acquire_token_with_client_credentials(
    resource,
    client_id,
    client_secret)

accessToken = 'Bearer ' + token['accessToken']

print(accessToken)

# relative url
getstatus = "/v1/metadata/topics"
request_url = kafkarest_endpoint + getstatus

# sending get request and saving the response as response object
response = requests.get(request_url, headers={'Authorization': accessToken})
print(response.content)
```

Para ejecutar el ejemplo de código, siga estos pasos:

1. Guarde el código de ejemplo en una máquina con Python instalado.
1. Instale las dependencias de Python necesarias ejecutando `pip3 install adal` y `pip install msrestazure`.
1. Modifique el código y actualice las siguientes propiedades para su entorno:
    1.  *Identificador de inquilino*: inquilino de Azure donde se encuentra la suscripción.
    1.  *Identificador de cliente*: identificador de la aplicación que registró en el grupo de seguridad.
    1.  *Secreto de cliente*: secreto de la aplicación que registró en el grupo de seguridad.
    1.  *Kafkarest_endpoint*: obtenga este valor en la pestaña "propiedades"de la información general del clúster, tal como se describe en la [sección de implementación](#create-a-kafka-cluster-with-rest-proxy-enabled). Debería tener el siguiente formato: `https://<clustername>-kafkarest.azurehdinsight.net`.
3. Desde la línea de comandos, ejecute el archivo Python mediante la ejecución de `python <filename.py>`.

## <a name="next-steps"></a>Pasos siguientes

* [Documentos de referencia de la API de proxy de REST de Kafka](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
* [Tutorial: Uso de Producer API y Consumer API de Apache Kafka](apache-kafka-producer-consumer-api.md)