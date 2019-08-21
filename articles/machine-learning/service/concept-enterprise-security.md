---
title: Seguridad de la empresa
titleSuffix: Azure Machine Learning service
description: 'Use Azure Machine Learning Service de forma segura: autenticación, autorización, seguridad de red, cifrado de datos y supervisión.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/07/2019
ms.openlocfilehash: ebecb69e57c620b2eb84568757c8e3e6f1cb1663
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946408"
---
# <a name="enterprise-security-for-azure-machine-learning-service"></a>Seguridad de empresa para Azure Machine Learning Service

En este artículo conocerá las funciones de seguridad disponibles con el Azure Machine Learning Service.

Cuando se usa un servicio en la nube, se recomienda restringir el acceso únicamente a los usuarios que lo necesitan. Esto comienza por comprender el modelo de autenticación y autorización que usa el servicio. Es posible que también desee restringir el acceso a la red o unir de forma segura los recursos de la red local con la nube. También es fundamental el cifrado de datos, tanto si están en reposo como si se mueven entre los servicios. Por último, debe ser capaz de supervisar el servicio y generar un registro de auditoría de todas las actividades.

## <a name="authentication"></a>Authentication

Si Azure Active Directory (Azure AD) se ha configurado para la autenticación multifactor, también se admite aquí.

* El cliente inicia sesión en Azure AD y obtiene el token de Azure Resource Manager.  Los usuarios y entidades de servicio son totalmente compatibles.
* El cliente presenta el token a Azure Resource Manager y a todos los servicios de Azure Machine Learning Service.
* La instancia de Azure Machine Learning Service proporciona un token de Azure Machine Learning al proceso del usuario. Por ejemplo, Proceso de Machine Learning. Este token se utiliza en el proceso del usuario para volver a llamar a la instancia de Azure Machine Learning Service (limita el ámbito al área de trabajo) una vez que ha finalizado la ejecución.

![La captura de pantalla muestra cómo funciona la autenticación en Azure Machine Learning Service](./media/enterprise-readiness/authentication.png)

### <a name="authentication-for-web-service-deployment"></a>Autenticación para la implementación del servicio web

Azure Machine Learning admite dos formas de autenticación para los servicios web, una clave y un token. Cada servicio web solo puede habilitar una forma de autenticación a la vez.

|Método de autenticación|ACI|AKS|
|---|---|---|
|Clave|Deshabilitado de forma predeterminada| Habilitado de forma predeterminada|
|Se necesita el cifrado de tokens| No disponible| Deshabilitado de forma predeterminada |

#### <a name="authentication-with-keys"></a>Autenticación con claves

Al habilitar la autenticación con claves para una implementación, se crean automáticamente las claves de autenticación.

* La autenticación se habilita de manera predeterminada cuando se implementa en Azure Kubernetes Service.
* La autenticación se deshabilita de manera predeterminada cuando se implementa en instancias de Azure Container.

Para habilitar la autenticación con claves, use el parámetro `auth_enabled` cuando cree o actualice una implementación.

Si la autenticación con claves está habilitada, puede usar el método `get_keys` para recuperar una clave de autenticación primaria y secundaria:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Si necesita regenerar una clave, use [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py).

#### <a name="authentication-with-tokens"></a>Autenticación con tokens

Cuando se habilita la autenticación por tokens para un servicio web, el usuario debe proporcionar un token JSON Web Token de Azure Machine Learning al servicio web para tener acceso a él.

* La autenticación por tokens se deshabilita de manera predeterminada cuando se implementa en Azure Kubernetes Service.
* La autenticación por tokens no se admite cuando se implementa en Azure Container Instances.

Para controlar la autenticación por tokens, use el parámetro `token_auth_enabled` cuando cree o actualice una implementación.

Si la autenticación por tokens está habilitada, puede usar el método `get_token` para recuperar un token JWT y la hora de expiración de los tokens:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Tendrá que solicitar un nuevo token después de la hora del token `refresh_by`.
>
> Se recomienda crear el área de trabajo de Azure Machine Learning en la misma región que el clúster de Azure Kubernetes Service. Para autenticarse con un token, el servicio web hará una llamada a la región en la que se crea el área de trabajo de Azure Machine Learning. Si la región del área de trabajo no está disponible, no se podrá capturar un token para el servicio web, incluso si el clúster está en una región distinta del área de trabajo. Esto da como resultado que el servicio Autenticación de Azure AD no estará disponible hasta que la región del área de trabajo vuelva a estarlo. Además, cuanto mayor sea la distancia entre la región del clúster y la región del área de trabajo, más tiempo tardará la captura de un token.

## <a name="authorization"></a>Authorization

Puede crear varias áreas de trabajo, y cada área de trabajo se puede compartir entre varias personas. Al compartir un área de trabajo, puede controlar el acceso a la misma si asigna los siguientes roles a los usuarios:

* Propietario
* Colaborador
* Lector

En la tabla siguiente se muestran algunas de las principales operaciones de Azure Machine Learning Service y los roles que pueden realizarlas:

| Operación de Azure Machine Learning Service | Propietario | Colaborador | Lector |
| ---- |:----:|:----:|:----:|
| Crear el área de trabajo | ✓ | ✓ | |
| Compartir el área de trabajo | ✓ | |  |
| Crear el proceso | ✓ | ✓ | |
| Asociar el proceso | ✓ | ✓ | |
| Asociar almacenes de datos | ✓ | ✓ | |
| Ejecutar un experimento | ✓ | ✓ | |
| Ver ejecuciones/métricas | ✓ | ✓ | ✓ |
| Registro del modelo | ✓ | ✓ | |
| Crear una imagen | ✓ | ✓ | |
| Implementar el servicio web | ✓ | ✓ | |
| Ver modelos/imágenes | ✓ | ✓ | ✓ |
| Llamar a un servicio web | ✓ | ✓ | ✓ |

Si los roles integrados no son suficientes para sus necesidades, puede crear roles personalizados. Solo se admiten roles personalizados para operaciones en el área de trabajo y en el Proceso de Machine Learning. Los roles personalizados pueden tener permisos de lectura, escritura o eliminación en el área de trabajo y en el recurso de proceso de esa área de trabajo. Puede hacer que el rol esté disponible en un nivel de área de trabajo específico, un nivel de grupo de recursos específico o un nivel de suscripción específico. Para más información, consulte [Administración de usuarios y roles en un área de trabajo de Azure Machine Learning](how-to-assign-roles.md).

### <a name="securing-compute-and-data"></a>Protección de procesos y datos

Los propietarios y colaboradores pueden usar todos los destinos de proceso y almacenes de datos conectados al área de trabajo.  
Cada área de trabajo tiene también asociada una identidad administrada asignada por el sistema (con el mismo nombre que el área de trabajo) con los siguientes permisos en los recursos asociados utilizados en el área de trabajo:

Para más información sobre las identidades administradas, consulte el artículo sobre [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

| Recurso | Permisos |
| ----- | ----- |
| Área de trabajo | Colaborador |
| Cuenta de almacenamiento | Colaborador de datos de blobs de almacenamiento |
| Key Vault | Acceso a todas las claves, secretos, certificados |
| Azure Container Registry | Colaborador |
| El grupo de recursos que contiene el área de trabajo | Colaborador |
| El grupo de recursos que contiene el almacén de claves (si es diferente al que contiene el área de trabajo) | Colaborador |

Se recomienda que los administradores no revoquen el acceso de la identidad administrada a los recursos mencionados. El acceso se puede restaurar con la operación de resincronización de claves.

Azure Machine Learning Service crea una aplicación adicional (el nombre empieza por `aml-`) en la suscripción con acceso de nivel de colaborador para cada región del área de trabajo. Por ejemplo, si tiene un área de trabajo en el este de Estados Unidos y otra área de trabajo en el norte de Europa, en la misma suscripción verá dos de estas aplicaciones. Esto es necesario para que Azure Machine Learning Service pueda ayudar a administrar los recursos de proceso.

## <a name="network-security"></a>Seguridad de las redes

Azure Machine Learning Service depende de otros servicios de Azure para los recursos de proceso. Los recursos de proceso (destinos de proceso) se usan para entrenar e implementar modelos. Estos destinos de proceso pueden crearse dentro de una red virtual. Por ejemplo, puede usar la instancia de Microsoft Data Science Virtual Machine para entrenar un modelo y, luego, implementarlo en Azure Kubernetes Service (AKS).  

Para más información, consulte [Ejecución de experimentos y realización de inferencias en una red virtual](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Cifrado de datos

### <a name="encryption-at-rest"></a>Cifrado en reposo

#### <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Machine Learning Service almacena instantáneas, salidas y registros en la cuenta de Azure Blob Storage vinculada al área de trabajo de Azure Machine Learning Service y se incluye en la suscripción del usuario. Todos los datos almacenados en Azure Blob Storage se cifran en reposo con claves administradas por Microsoft.

Para más información sobre el uso de claves propias para los datos almacenados en Azure Blob Storage, consulte [Cifrado del servicio Storage mediante claves administradas por el cliente en Azure Key Vault](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Por lo general, los datos de entrenamiento también se almacenan en Azure Blob Storage, de modo que están accesibles para el proceso de entrenamiento. Este almacenamiento no está administrado por Azure Machine Learning, sino que está montado para el proceso como un sistema de archivos remoto.

Para información sobre la regeneración de las claves de acceso para las cuentas de Azure Storage que se usan con el área de trabajo, consulte el artículo [Regeneración de las claves de acceso de la cuenta de almacenamiento](how-to-change-storage-access-key.md).

#### <a name="cosmos-db"></a>Cosmos DB

Azure Machine Learning Service almacena métricas y metadatos en la base de datos de Cosmos DB incluida en una suscripción a Microsoft administrada por este servicio. Todos los datos almacenados en Cosmos DB se cifran en reposo con claves administradas por Microsoft.

#### <a name="azure-container-registry-acr"></a>Azure Container Registry (ACR)

Todas las imágenes del contenedor en el registro (ACR) se cifran en reposo. Azure cifra automáticamente una imagen antes de almacenarla y la descifra sobre la marcha cuando Azure Machine Learning Service la extrae.

#### <a name="machine-learning-compute"></a>Proceso de Machine Learning

El disco del sistema operativo para cada nodo de ejecución almacenado en Azure Storage se cifra mediante claves administradas por Microsoft en las cuentas de almacenamiento de Azure Machine Learning Service. Este destino de proceso es efímero y por lo general los clústeres se reducen verticalmente cuando no hay ninguna ejecución en cola. La máquina virtual subyacente se desaprovisiona y el disco del sistema operativo se elimina. Los discos del sistema operativo no admiten el cifrado de discos de Azure.
Cada máquina virtual tiene también un disco local temporal para las operaciones del sistema operativo. El disco también se puede usar para almacenar los datos de entrenamiento. El disco no se cifra.
Para más información sobre cómo funciona el cifrado en reposo en Azure, consulte [Cifrado en reposo de datos de Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest).

### <a name="encryption-in-transit"></a>Cifrado en tránsito

Tanto las comunicaciones internas entre varios microservicios de Azure Machine Learning como las comunicaciones externas de las llamadas al punto de conexión de puntuación se admiten mediante SSL. Todos los accesos de Azure Storage se realizan también a través de un canal seguro.
Para más información, consulte [Protección de los servicios web de Azure Machine Learning mediante SSL](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Usar Azure Key Vault

Azure Machine Learning Service usa la instancia de Key Vault asociada al área de trabajo para almacenar credenciales de varios tipos:

* La cadena de conexión de cuenta de almacenamiento asociada
* Contraseñas para las instancias de Azure Container Repository
* Cadenas de conexión a almacenes de datos

Las contraseñas y las claves SSH para destinos de proceso como HDInsight y VM de HDI se almacenan en un almacén de claves independiente asociado a la suscripción a Microsoft. Azure Machine Learning Service no almacena las contraseñas ni las claves que proporciona el usuario; en su lugar genera, autoriza y almacena sus propias claves SSH para conectarse a VM/HDInsight y ejecutar los experimentos.
Cada área de trabajo tiene asociada una identidad administrada asignada por el sistema (con el mismo nombre que el área de trabajo) que tiene acceso a todas las claves, secretos y certificados del almacén de claves.

## <a name="monitoring"></a>Supervisión

### <a name="metrics"></a>Métricas

Las métricas de Azure Monitor se pueden usar para ver y supervisar las métricas del área de trabajo de Azure Machine Learning Service. En [Azure Portal](https://portal.azure.com), seleccione el área de trabajo y, a continuación, siga el vínculo __Métricas__.

![Captura de pantalla que muestra las métricas de ejemplo de un área de trabajo](./media/enterprise-readiness/workspace-metrics.png)

Métricas incluye información sobre ejecuciones, implementaciones y registros.

Para más información, consulte [Métricas en Azure Monitor](/azure/azure-monitor/platform/data-platform-metrics).

### <a name="activity-log"></a>Registro de actividades

Puede consultar el registro de actividad en el área de trabajo para ver diferentes operaciones realizadas en ella y obtener información básica, como el nombre de la operación, quién inició el evento, la marca de tiempo, etc.

En la siguiente captura de pantalla se muestra el registro de actividad de un área de trabajo:

![Captura de pantalla que muestra el registro de actividad en un área de trabajo](./media/enterprise-readiness/workspace-activity-log.png)

Los detalles de la solicitud de puntuación se almacenan en Application Insights, que se crea en la suscripción del usuario al crear el área de trabajo. La información registrada incluye campos como HTTPMethod, UserAgent, ComputeType, RequestUrl, StatusCode, RequestId, Duration, etc.

> [!IMPORTANT]
> Algunas acciones dentro del área de trabajo Azure Machine Learning no registran información en el registro de actividad. Por ejemplo, el inicio de una ejecución de entrenamiento o el registro de un modelo.
>
> Algunas de estas acciones aparecen en el área __Actividades__ del área de trabajo, pero no indican quién inició la actividad.

## <a name="data-flow-diagram"></a>Diagrama de flujo de datos

### <a name="create-workspace"></a>Creación del espacio de trabajo

En el siguiente diagrama se muestra el flujo de trabajo de creación del área de trabajo.
El usuario inicia sesión en Azure AD desde cualquiera de los clientes de Azure Machine Learning Service compatibles (la CLI, el SDK de Python, Azure Portal) y solicita el token de Azure Resource Manager adecuado. A continuación, el usuario llama a Azure Resource Manager para crear el área de trabajo.  Azure Resource Manager contacta con el proveedor de recursos de Azure Machine Learning Service para aprovisionar el área de trabajo.  Durante la creación del área de trabajo, se crean recursos adicionales en la suscripción del cliente:

* KeyVault (para almacenar secretos)
* Una cuenta de Azure Storage (que incluye blob y recurso compartido de archivos)
* Docker para WindowsAzure Container Registry (para almacenar imágenes de Docker para la inferencia, puntuación y experimentación)
* Application Insights (para almacenar datos de telemetría)

Según sea necesario, también se pueden aprovisionar otros procesos asociados a un área de trabajo (Azure Kubernetes Service, VM, etc.).

![Captura de pantalla que muestra el flujo de trabajo de creación del área de trabajo](./media/enterprise-readiness/create-workspace.png)

### <a name="save-source-code-training-scripts"></a>Almacenamiento del código fuente (scripts de entrenamiento)

En el siguiente diagrama se muestra el flujo de trabajo de la instantánea de código.
Un área de trabajo de Azure Machine Learning Service tiene asociados directorios (experimentos), que contienen el código fuente (scripts de entrenamiento).  Estos scripts se almacenan en la máquina local del cliente y en la nube (en Azure Blob Storage, bajo la suscripción del cliente). Las instantáneas de código se utilizan para la ejecución o inspección de auditorías históricas.

![Captura de pantalla que muestra el flujo de trabajo de creación del área de trabajo](./media/enterprise-readiness/code-snapshot.png)

### <a name="training"></a>Cursos

En el siguiente diagrama se muestra el flujo de trabajo de entrenamiento.

* Se llama a Azure Machine Learning Service con el identificador de la instantánea de código guardada antes.
* Azure Machine Learning Service crea un identificador de ejecución (opcional) y un token de Azure Machine Learning Service, que más adelante usan los destinos de proceso, como Proceso o VM de Machine Learning, para responder a Azure Machine Learning Service.
* Puede elegir un proceso administrado (por ejemplo, Proceso de Machine Learning) o un proceso no administrado (por ejemplo, VM) para ejecutar los trabajos de entrenamiento. A continuación se explica el flujo de datos para ambos escenarios:
* (VM/HDInsight: se accede a ellos con credenciales SSH en Key Vault en la suscripción a Microsoft) Azure Machine Learning Service ejecuta código de administración en el destino de proceso que:

   1. Prepara el entorno. (Docker también es una opción para VM y local. Consulte los pasos de Proceso de Machine Learning siguientes para entender cómo funciona la ejecución de experimentos en el contenedor de Docker).
   1. Descarga el código.
   1. Define las variables de entorno y las configuraciones.
   1. Ejecuta el script de usuario (la instantánea de código mencionada antes).

* (Proceso de Machine Learning: se accede a él mediante la identidad administrada del área de trabajo) Puesto que Proceso de Machine Learning es un proceso administrado, es decir, está administrado por Microsoft, se ejecuta bajo la suscripción a Microsoft.

   1. Si es necesario, pone en marcha la construcción de un Docker remoto.
   1. Escribe el código de administración en el recurso compartido de archivos de Azure del usuario.
   1. Inicia el contenedor con un comando inicial, es decir, el código de administración como se describe en el paso anterior.

#### <a name="querying-runs-and-metrics"></a>Consulta de ejecuciones y métricas

Este paso se muestra en la parte del flujo donde el proceso de entrenamiento escribe las *métricas de ejecución* en Azure Machine Learning Service, desde donde se almacena en Cosmos DB. Los clientes pueden llamar a Azure Machine Learning Service que, a su vez, extraerá las métricas de Cosmos DB y las devolverá al cliente.

![Captura de pantalla que muestra el flujo de trabajo de creación del área de trabajo](./media/enterprise-readiness/training-and-metrics.png)

### <a name="creating-web-services"></a>Creación de servicios web

En el siguiente diagrama se muestra el flujo de trabajo de inferencia. La inferencia, o modelo de puntuación, es la fase donde se usa el modelo implementado para la predicción, frecuentemente en datos de producción.
Vea los detalles a continuación:

* El usuario registra un modelo con un cliente como el SDK de Azure ML.
* El usuario crea una imagen usando el modelo, el archivo de puntuación y otras dependencias del modelo.
* La imagen de Docker se crea y se almacena en ACR.
* El servicio web se implementa en el destino de proceso (ACI o AKS) con la imagen creada anteriormente.
* Los detalles de la solicitud de puntuación se almacenan en Application Insights, que se incluye en la suscripción del usuario.
* También se insertan datos de telemetría en la suscripción a Microsoft o Azure.

![Captura de pantalla que muestra el flujo de trabajo de creación del área de trabajo](./media/enterprise-readiness/inferencing.png)

## <a name="next-steps"></a>Pasos siguientes

* [Protección de los servicios web de Azure Machine Learning con SSL](how-to-secure-web-service.md)
* [Consumir un modelo de ML que está implementado como un servicio web](how-to-consume-web-service.md)
* [Cómo ejecutar predicciones por lotes](how-to-run-batch-predictions.md)
* [Supervisión de los modelos de Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Recopilar datos de modelos en producción](how-to-enable-data-collection.md)
* [SDK de Azure Machine Learning Service](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Uso de Azure Machine Learning Service con Azure Virtual Network](how-to-enable-virtual-network.md)
* [Procedimientos recomendados para compilar sistemas de recomendaciones](https://github.com/Microsoft/Recommenders)
* [Compilación de una API de recomendaciones en tiempo real en Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
