---
title: Seguridad de la empresa
titleSuffix: Azure Machine Learning service
description: 'Usar servicio Azure Machine Learning de forma segura: autenticación, autorización, seguridad de red, el cifrado de datos y supervisión.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 03/10/2019
ms.openlocfilehash: b950e7d38235d089c6236c76136d8ec2fc7a1f74
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2019
ms.locfileid: "57731335"
---
# <a name="enterprise-security-for-azure-machine-learning-service"></a>Seguridad de la empresa para el servicio Azure Machine Learning

En este artículo, aprenderá sobre las características de seguridad disponibles con el servicio de aprendizaje automático de Azure.

Cuando se usa un servicio en la nube, es una práctica recomendada para restringir el acceso únicamente a los usuarios que lo necesitan. Esto inicia al comprender el modelo de autenticación y autorización utilizado por el servicio. Es posible que también desee restringir el acceso a la red o unirse de forma segura los recursos de la red local con los de la nube. También es fundamental, tanto en reposo y mientras se mueven los datos entre los servicios de cifrado de datos. Por último, deberá ser capaz de supervisar el servicio y generar un registro de auditoría de todas las actividades.

## <a name="authentication"></a>Authentication
Se admite la autenticación multifactor si Azure Active Directory (Azure AD) está configurado para el mismo.
* Cliente inicia sesión en Azure AD y obtiene token de Azure Resource Manager.  Los usuarios y entidades de servicio son totalmente compatibles.
* Cliente presenta el token a Azure Resource Manager y todos los servicios de Azure Machine Learning
* Servicio Azure Machine Learning proporciona un token de Azure Machine Learning para el proceso de usuario. Por ejemplo, Machine Learning Compute. Este token se usa por el usuario de Azure Machine Learning de proceso volver a llamar al servicio de Azure Machine Learning (limita el ámbito al área de trabajo) después de la ejecución completada.

![Captura de pantalla muestra cómo funciona la autenticación en el servicio de Azure Machine Learning](./media/enterprise-readiness/authentication.png)

### <a name="authentication-keys-for-web-service-deployment"></a>Claves de autenticación de la implementación del servicio Web

Al habilitar la autenticación para una implementación, se crean automáticamente las claves de autenticación.

* La autenticación se habilita de manera predeterminada cuando se implementa en Azure Kubernetes Service.
* La autenticación se deshabilita de manera predeterminada cuando se implementa en instancias de Azure Container.

Para controlar la autenticación, use el parámetro `auth_enabled` cuando cree o actualice una implementación.

Si la autenticación está habilitada, puede usar el método `get_keys` para recuperar una clave de autenticación primaria y secundaria:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Si necesita volver a generar una clave, use [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)


## <a name="authorization"></a>Autorización

Puede crear varias áreas de trabajo, y cada área de trabajo se puede compartir entre varias personas. Al compartir un área de trabajo, puede controlar el acceso a la misma si asigna los siguientes roles a los usuarios:
* Propietario
* Colaborador
* Lector
    
En la tabla siguiente se enumera algunas de las principales operaciones de servicio de Azure Machine Learning y las funciones que pueden realizar en ellos:

| Servicio de Azure Machine Learning operación | Propietario | Colaborador | Lector |
| ---- |:----:|:----:|:----:|
| Crear área de trabajo | ✓ | ✓ | |
| Compartir el área de trabajo | ✓ | |  |
| Crear proceso | ✓ | ✓ | |
| Asociar proceso | ✓ | ✓ | |
| Asociar almacenes de datos | ✓ | ✓ | |
| Ejecutar un experimento | ✓ | ✓ | |
| Métricas de ejecuciones de vista | ✓ | ✓ | ✓ |
| Registro del modelo | ✓ | ✓ | |
| Crear imagen | ✓ | ✓ | |
| Implementar el servicio web | ✓ | ✓ | |
| Modelos de vista/images | ✓ | ✓ | ✓ |
| Llame al servicio web | ✓ | ✓ | ✓ |

Si los roles integrados no son suficientes para satisfacer sus necesidades, también puede crear roles personalizados. Tenga en cuenta que los roles personalizados solo que se admite para las operaciones en el área de trabajo y Machine Learning Compute. Los roles personalizados pueden tener leer, escribir o eliminar los permisos en el área de trabajo y el recurso de proceso en esa área de trabajo. Puede realizar el rol disponible en un nivel de área de trabajo específica, un nivel de grupo de recursos específico o un nivel de suscripción específica. Para obtener más información, consulte [administrar usuarios y roles en un área de trabajo de Azure Machine Learning](how-to-assign-roles.md)

### <a name="securing-compute-and-data"></a>Protección de datos y procesos
Los propietarios y colaboradores pueden usar todos los almacenes de datos y destinos del proceso que están conectados al área de trabajo.  
Cada área de trabajo también tiene una identidad de administrada de asignado por el sistema asociado (con el mismo nombre que el área de trabajo) con los siguientes permisos en los recursos conectados utilizados en el área de trabajo:

Para obtener más información sobre las identidades administradas, vea [administra las identidades de los recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

| Recurso | Permisos |
| ----- | ----- |
| Área de trabajo | Colaborador | 
| Cuenta de almacenamiento | Colaborador de datos de Storage Blob | 
| Key Vault | Acceso a todos los certificados de claves, secretos, | 
| Azure Container Registry | Colaborador | 
| Grupo de recursos que contiene el área de trabajo | Colaborador | 
| Grupo de recursos que contiene el almacén de claves (si es diferente a la que contiene el área de trabajo) | Colaborador | 

Se recomienda que los administradores no revocan el acceso de la identidad administrada a los recursos mencionados anteriormente. Acceso se puede restaurar con la operación de resincronización de claves.

Servicio Azure Machine Learning crea una aplicación adicional (nombre empieza con aml-) con el acceso de nivel de colaborador en su suscripción para cada región del área de trabajo. Para p. ej. Si tiene un área de trabajo en el este de Estados Unidos y otra área de trabajo en Europa del Norte en la misma suscripción verá 2 dichas aplicaciones. Esto es necesario para que los recursos de proceso de servicio puede ayudar a administrar Azure Machine Learning.


## <a name="network-security"></a>Seguridad de las redes

Azure Machine Learning Service depende de otros servicios de Azure para los recursos de proceso. Los recursos de proceso (destinos de proceso) se usan para entrenar e implementar modelos. Estos destinos de proceso pueden crearse dentro de una red virtual. Por ejemplo, puede usar la instancia de Microsoft Data Science Virtual Machine para entrenar un modelo y, luego, implementarlo en Azure Kubernetes Service (AKS).  

Para obtener más información, consulte [cómo ejecutar experimentos e inferencia en una red virtual](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Cifrado de datos

### <a name="encryption-at-rest"></a>Cifrado en reposo
#### <a name="azure-blob-storage"></a>Azure Blob Storage
Servicio Azure Machine Learning almacena instantáneas, salidas y registros en la cuenta de almacenamiento de blobs de Azure que está vinculada al área de trabajo del servicio de Azure Machine Learning y vive en la suscripción del usuario. Todos los datos almacenados en Azure Blob Storage se cifran en reposo con claves Microsoft-Managed.

Para obtener más información sobre cómo llevar sus propias claves para los datos almacenados en Azure Blob Storage, consulte [Storage Service Encryption mediante claves administradas por el cliente en Azure Key Vault](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Los datos de entrenamiento normalmente también se almacenan en Azure Blob storage para que sea accesible para el proceso de entrenamiento. Este almacenamiento no está administrado por Azure Machine Learning pero montado para calcular como un sistema de archivos remoto.

#### <a name="cosmos-db"></a>Cosmos DB
Servicio Azure Machine Learning almacena las métricas y los metadatos a Cosmos DB con la que se encuentra en una suscripción a Microsoft administrada por servicio de Azure Machine Learning. Todos los datos almacenados en Cosmos DB se cifran en reposo con claves administradas de Microsoft.

#### <a name="azure-container-registry-acr"></a>Azure Container Registry (ACR)
Todas las imágenes de contenedor en el registro (ACR) se cifran en reposo. Azure cifra una imagen antes de almacenarlos y descifra en marcha cuando el servicio Azure Machine Learning extrae la imagen automáticamente.

#### <a name="machine-learning-compute"></a>Proceso de Machine Learning
El disco del sistema operativo para cada nodo de ejecución se almacena en Azure Storage se cifra mediante claves administradas de Microsoft en las cuentas de almacenamiento del servicio de Azure Machine Learning. Este proceso es efímera y clústeres se reducen normalmente cuando no hay ninguna serie en cola. La máquina virtual subyacente se anuló el aprovisionamiento y elimina el disco del sistema operativo. No se admite el cifrado de disco de Azure para el disco del sistema operativo.
Cada máquina virtual también tiene un disco local temporal para las operaciones del sistema operativo. Este disco también se puede usar opcionalmente para almacenar los datos de entrenamiento. Este disco no está cifrado. Para obtener más información sobre cómo funciona el cifrado en reposo en Azure, consulte [cifrado en reposo de datos de Azure](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest). 

### <a name="encryption-in-transit"></a>Cifrado en tránsito
Se admiten tanto las comunicaciones internas entre varios microservicios de Azure Machine Learning y comunicación externa de una llamada al punto de conexión de puntuación mediante SSL. Todos los accesos de Azure Storage también es a través de un canal seguro. Para obtener más información, consulte [servicios web de Azure Machine Learning segura mediante SSL](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Usar Azure Key Vault
Instancia de Key Vault asociada con el área de trabajo se usa por el servicio de Azure Machine Learning para almacenar las credenciales de varias clases:
* La cadena de conexión de la cuenta de almacenamiento asociada
* Contraseñas para las instancias de Azure Container Repository
* Cadenas de conexión a datos almacenes. 

Las contraseñas SSH y las claves para los destinos como HDInsight de HDI y máquinas virtuales de proceso se almacenan en un almacén de claves independiente que está asociado con la suscripción a Microsoft. Servicio Azure Machine Learning almacenar las contraseñas o claves proporcionan por el usuario en su lugar genera, autoriza y almacena sus propias claves de SSH para conectarse a la máquina virtual/HDInsight para ejecutar los experimentos. Cada área de trabajo tiene una asociada asignado por el sistema administrado identidad (con el mismo nombre que el área de trabajo) que tiene acceso a todas las claves, secretos y certificados en el almacén de claves.

 
## <a name="monitoring"></a>Supervisión

Los usuarios pueden ver el registro de actividad en el área de trabajo para ver varias operaciones realizadas en el área de trabajo y obtener la información básica como el nombre de la operación, el evento iniciado por, etcetera de marca de tiempo.

Captura de pantalla siguiente muestra el registro de actividad para un área de trabajo:

![Registro de actividad de captura de pantalla que muestra en un área de trabajo](./media/enterprise-readiness/workspace-activity-log.png)


Detalles de la solicitud de puntuación se almacenan en el AppInsights, que se crea en la suscripción del usuario al crear el área de trabajo. Esto incluye campos como HTTPMethod UserAgent, ComputeType, RequestUrl, StatusCode, RequestId, duración, etcetera.


## <a name="data-flow-diagram"></a>Diagrama de flujo de datos

### <a name="create-workspace"></a>Crear área de trabajo
El siguiente diagrama muestra el flujo de trabajo de creación del área de trabajo.
Usuario inicia sesión en Azure AD desde cualquiera de los clientes del servicio de Azure Machine Learning compatibles (portal de Azure CLI, SDK de Python) y solicita el token de Azure Resource Manager adecuado.  Usuario, a continuación, llama a Azure Resource Manager para crear el área de trabajo.  Proveedor de recursos para aprovisionar el área de trabajo de servicio de Azure Resource Manager contactos Azure Machine Learning.  Recursos adicionales se crean en la suscripción del cliente durante la creación del área de trabajo:
* Almacén de claves (para almacenar secretos)
* Una cuenta de almacenamiento de Azure (incluido Blob & recurso compartido de archivos)
* Azure Container Registry (para almacenar imágenes de docker para inferencia y experimentación)
* Application Insights (para almacenar datos de telemetría)

Otros procesos que se adjunta a un área de trabajo (servicio de Kubernetes de Azure, etc. de la máquina virtual) también se pueden aprovisionar por los clientes según sea necesario. 

![Captura de pantalla muestra crea flujo de trabajo del área de trabajo](./media/enterprise-readiness/create-workspace.png)

### <a name="save-source-code-training-scripts"></a>Guarde el código fuente (scripts de entrenamiento)
El siguiente diagrama muestra el flujo de trabajo de instantáneas del código.
Asociado a un Azure Machine Learning área de trabajo de servicio son directorios (experimentos), que contiene el código fuente (scripts de entrenamiento).  Estos se almacenan en el equipo del cliente local y en la nube (en el almacenamiento de blobs de Azure en la suscripción del cliente). Estas instantáneas del código se utilizan para la ejecución o inspección de auditoría históricos.

![Captura de pantalla muestra crea flujo de trabajo del área de trabajo](./media/enterprise-readiness/code-snapshot.png)

### <a name="training"></a>Cursos
El siguiente diagrama muestra el flujo de trabajo de aprendizaje.
* Servicio Azure Machine Learning se denomina con el identificador de la instantánea para la instantánea de código que guardó anteriormente
* Crea el servicio de Azure Machine Learning ejecuta ID (opcional) y el token de servicio de Azure Machine Learning, que se utiliza posteriormente en los destinos de proceso, como Machine Learning Compute o de máquinas virtuales para responder a los servicios de Azure Machine Learning
* Puede elegir un proceso administrado (p. ej. Machine Learning Compute) o el proceso no administrado (p. ej. Máquina virtual) para ejecutar los trabajos de entrenamiento. Se explica el flujo de datos para ambos escenarios siguientes:
* (Máquina virtual/HDInsight/Local; que se accede mediante credenciales SSH en Key Vault en la suscripción de Microsoft) Servicio Azure Machine Learning ejecuta código de administración de destino de proceso que:
    1.  Prepara el entorno (tenga en cuenta: Docker es una opción para la máquina virtual o Local. Consulte los pasos para que Machine Learning Compute siguientes entender cómo ejecución experimento en funcionamiento de contenedor de docker)
    2.  Descarga del código
    3.  Configura las variables de entorno y configuraciones
    4.  Ejecuta el script de usuario (instantánea de código que se mencionó anteriormente)
* (Machine Learning Compute: tener acceso utilizando la identidad del área de trabajo administrado) Tenga en cuenta que dado que el proceso de Machine Learning es un proceso administrado, es decir, está administrado por Microsoft, como resultado, se ejecuta bajo la suscripción de Microsoft.
    1.  Construcción de Docker remoto se inicia si es necesario
    2.  Escribe el código de administración al usuario del recurso compartido de archivos de Azure
    3.  Contenedor se inicia con la inicial del comando es decir, código de administración en el paso anterior


#### <a name="querying-runs--metrics"></a>Métricas y ejecuciones de consultas
Este paso se muestra en el flujo de escrituras de proceso de entrenamiento de donde la *ejecutar métricas* al servicio de Azure Machine Learning desde donde se almacena en Cosmos DB. Los clientes pueden llamar a servicio de Azure Machine Learning que extraer las métricas de Cosmos DB y devolverlo al cliente a su vez.

![Captura de pantalla muestra crea flujo de trabajo del área de trabajo](./media/enterprise-readiness/training-and-metrics.png)

### <a name="creating-web-services"></a>Creación de servicios web
El siguiente diagrama muestra el flujo de trabajo de inferencia en el que el modelo se implementa como un servicio web.
Consulte los detalles a continuación:
* Usuario registra un modelo con un cliente como el SDK de Azure ML
* Usuario crea la imagen mediante el modelo, el archivo de puntuación y otras dependencias de modelo
* Se crea la imagen de Docker y se almacena en ACR
* Servicio Web se implementa en el destino de proceso (ACI o AKS) con la imagen creada anteriormente
* Detalles de la solicitud de puntuación se almacenan en el AppInsights, que se encuentra en la suscripción del usuario
* Telemetría también se inserta en la suscripción de Microsoft Azure o

![Captura de pantalla muestra crea flujo de trabajo del área de trabajo](./media/enterprise-readiness/inferencing.png)

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
