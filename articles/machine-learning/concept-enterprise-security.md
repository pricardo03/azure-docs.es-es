---
title: Seguridad de la empresa
titleSuffix: Azure Machine Learning
description: 'Use Azure Machine Learning de forma segura: autenticación, autorización, seguridad de red, cifrado de datos y supervisión.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/17/2019
ms.openlocfilehash: 4b2f9e7f12b468f12fcfbe1b0af5c2918aa6c6ad
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75535520"
---
# <a name="enterprise-security-for-azure-machine-learning"></a>Seguridad de empresa para Azure Machine Learning

En este artículo conocerá las características de seguridad disponibles con Azure Machine Learning.

Cuando se usa un servicio en la nube, se recomienda restringir el acceso solo a los usuarios que lo necesiten. Para empezar, es necesario comprender el modelo de autenticación y autorización que usa el servicio. Es posible que también quiera restringir el acceso a la red o unir de forma segura los recursos de la red local con la nube. También es fundamental el cifrado de datos, tanto si están en reposo como si se mueven entre los servicios. Por último, debe ser capaz de supervisar el servicio y generar un registro de auditoría de todas las actividades.

## <a name="authentication"></a>Authentication

Si Azure Active Directory (Azure AD) se ha configurado para usar la autenticación multifactor, también se admite aquí. Este es el proceso de autenticación:

1. El cliente inicia sesión en Azure AD y recibe un token de Azure Resource Manager.  Los usuarios y las entidades de servicio son totalmente compatibles.
1. El cliente presenta el token a Azure Resource Manager y a todas las instancias de Azure Machine Learning.
1. Machine Learning Service proporciona un token al destino de proceso de usuario (por ejemplo, Proceso de Machine Learning). El destino de proceso de usuario usa este token para volver a llamar a Machine Learning Service una vez completada la ejecución. El ámbito se limita al área de trabajo.

[![Autenticación en Azure Machine Learning](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication-expanded.png#lightbox)

Vea la guía paso a paso [Configuración de la autenticación](how-to-setup-authentication.md) para obtener ejemplos e instrucciones detalladas sobre cómo configurar la autenticación, incluida la autenticación de la entidad de servicio para flujos de trabajo automatizados.

### <a name="authentication-for-web-service-deployment"></a>Autenticación para la implementación de servicios web

Azure Machine Learning admite dos formas de autenticación de los servicios web: una clave y un token. Cada servicio web solo puede permitir una forma de autenticación a la vez.

|Método de autenticación|Descripción|Azure Container Instances|AKS|
|---|---|---|---|
|Clave|Las claves son estáticas y no es necesario actualizarlas. Las claves se pueden regenerar manualmente.|Deshabilitado de forma predeterminada| Habilitado de forma predeterminada|
|Token|Los tokens expiran después de un período de tiempo especificado y se deben actualizar.| No disponible| Deshabilitado de forma predeterminada |

Vea la [sección de autenticación de servicio web](how-to-setup-authentication.md#web-service-authentication) para obtener ejemplos de código sobre la autenticación en servicios web en Azure Machine Learning.

## <a name="authorization"></a>Authorization

Puede crear varias áreas de trabajo, y cada área de trabajo se puede compartir entre varias personas. Al compartir un área de trabajo, puede controlar el acceso a ella si asigna los siguientes roles a los usuarios:

* Propietario
* Colaborador
* Lector

En la tabla siguiente se muestran algunas de las principales operaciones de Azure Machine Learning y los roles que pueden realizarlas:

| Operación de Azure Machine Learning | Propietario | Colaborador | Lector |
| ---- |:----:|:----:|:----:|
| Creación del espacio de trabajo | ✓ | ✓ | |
| Compartir área de trabajo | ✓ | |  |
| Actualizar el área de trabajo de Enterprise Edition | ✓ | |
| Crear el destino de proceso | ✓ | ✓ | |
| Asociar el destino de proceso | ✓ | ✓ | |
| Asociar almacenes de datos | ✓ | ✓ | |
| Ejecutar experimento | ✓ | ✓ | |
| Ver ejecuciones/métricas | ✓ | ✓ | ✓ |
| Registro del modelo | ✓ | ✓ | |
| Crear una imagen | ✓ | ✓ | |
| Implementar el servicio web | ✓ | ✓ | |
| Ver modelos/imágenes | ✓ | ✓ | ✓ |
| Llamar a un servicio web | ✓ | ✓ | ✓ |

Si los roles integrados no satisfacen sus necesidades, puede crear roles personalizados. Solo se admiten roles personalizados para operaciones en el área de trabajo y en Proceso de Machine Learning. Los roles personalizados pueden tener permisos de lectura, escritura o eliminación en el área de trabajo y en el recurso de proceso de ese área de trabajo. Puede hacer que el rol esté disponible en un nivel de área de trabajo específico, un nivel de grupo de recursos específico o un nivel de suscripción específico. Para más información, consulte [Administración de usuarios y roles en un área de trabajo de Azure Machine Learning](how-to-assign-roles.md).

### <a name="securing-compute-targets-and-data"></a>Protección de los datos y destinos de proceso

Los propietarios y colaboradores pueden usar todos los destinos de proceso y almacenes de datos conectados al área de trabajo.  

Cada área de trabajo también tiene una identidad administrada asignada por el sistema asociada con el mismo nombre que el área de trabajo. La identidad administrada tiene los siguientes permisos en los recursos asociados usados en el área de trabajo.

Para más información sobre las identidades administradas, consulte [Identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

| Resource | Permisos |
| ----- | ----- |
| Área de trabajo | Colaborador |
| Cuenta de almacenamiento | Colaborador de datos de blobs de almacenamiento |
| Almacén de claves | Acceso a todas las claves, secretos, certificados |
| Azure Container Registry | Colaborador |
| El grupo de recursos que contiene el área de trabajo | Colaborador |
| El grupo de recursos que contiene el almacén de claves (si es diferente al que contiene el área de trabajo) | Colaborador |

No se recomienda que los administradores revoquen el acceso de la identidad administrada a los recursos mencionados en la tabla anterior. Puede restaurar el acceso mediante la operación de resincronización de claves.

Azure Machine Learning crea una aplicación adicional (el nombre empieza por `aml-` o `Microsoft-AzureML-Support-App-`) con acceso de nivel de colaborador en la suscripción para cada región del área de trabajo. Por ejemplo, si tiene un área de trabajo en el este de Estados Unidos y otra en el norte de Europa en la misma suscripción, verá dos de estas aplicaciones. Estas aplicaciones permiten que Azure Machine Learning le ayude a administrar los recursos de proceso.

## <a name="network-security"></a>Seguridad de las redes

Azure Machine Learning depende de otros servicios de Azure para los recursos de proceso. Los recursos de proceso (destinos de proceso) se usan para entrenar e implementar modelos. Puede crear estos destinos de proceso en una red virtual. Por ejemplo, puede usar Azure Data Science Virtual Machine para entrenar un modelo y, después, implementarlo en AKS.  

Para más información, consulte [Ejecución de experimentos y realización de inferencias en una red virtual](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Cifrado de datos

### <a name="encryption-at-rest"></a>Cifrado en reposo

#### <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Machine Learning almacena instantáneas, salidas y registros en la cuenta de Azure Blob Storage vinculada al área de trabajo de Azure Machine Learning y la suscripción. Todos los datos almacenados en Azure Blob Storage se cifran en reposo con claves administradas por Microsoft.

Para más información sobre el uso de claves propias para los datos almacenados en Azure Blob Storage, consulte [Cifrado de Azure Storage con claves administradas por el cliente en Azure Key Vault](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Los datos de entrenamiento también se almacenan en Azure Blob Storage, de modo que están accesibles para los destinos de proceso de entrenamiento. Este almacenamiento no está administrado por Azure Machine Learning, sino que está montado en destinos de proceso como un sistema de archivos remoto.

Para información sobre la regeneración de las claves de acceso para las cuentas de Azure Storage que se usan con el área de trabajo, consulte [Regeneración de las claves de acceso de la cuenta de almacenamiento](how-to-change-storage-access-key.md).

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Machine Learning almacena métricas y metadatos en la instancia de Azure Cosmos DB asociada con una suscripción de Microsoft administrada por Azure Machine Learning. Todos los datos almacenados en Azure Cosmos DB se cifran en reposo con claves administradas por Microsoft.

#### <a name="azure-container-registry"></a>Azure Container Registry

Todas las imágenes del contenedor en el registro (Azure Container Registry) se cifran en reposo. Azure cifra automáticamente una imagen antes de almacenarla y la descifra sobre la marcha cuando Azure Machine Learning la extrae.

#### <a name="machine-learning-compute"></a>Proceso de Machine Learning

El disco del sistema operativo de cada nodo de proceso almacenado en Azure Storage se cifra mediante claves administradas por Microsoft en las cuentas de almacenamiento de Azure Machine Learning. Este destino de proceso es efímero y, por lo general, los clústeres se reducen verticalmente cuando no hay ninguna ejecución en cola. La máquina virtual subyacente se desaprovisiona y el disco del sistema operativo se elimina. Azure Disk Encryption no se admite con el disco del sistema operativo.

Cada máquina virtual tiene también un disco local temporal para las operaciones del sistema operativo. Si quiere, puede usar el disco para almacenar temporalmente los datos de entrenamiento. El disco no está cifrado.
Para más información sobre cómo funciona el cifrado en reposo en Azure, consulte [Cifrado en reposo de datos de Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest).

### <a name="encryption-in-transit"></a>Cifrado en tránsito

Puede usar SSL para proteger la comunicación interna entre los microservicios de Azure Machine Learning y para proteger las llamadas externas al punto de conexión de puntuación. Todo el acceso de Azure Storage se realiza también a través de un canal seguro.

Para más información, consulte [Uso de SSL para proteger un servicio web con Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Uso de Azure Key Vault

Azure Machine Learning usa la instancia de Azure Key Vault asociada al área de trabajo para almacenar credenciales de varios tipos:

* La cadena de conexión de cuenta de almacenamiento asociada
* Contraseñas para las instancias de Azure Container Repository
* Cadenas de conexión a almacenes de datos

Las contraseñas y las claves SSH para destinos de proceso, como Azure HDInsight y las máquinas virtuales, se almacenan en un almacén de claves independiente asociado a la suscripción de Microsoft. Azure Machine Learning no almacena las contraseñas ni las claves proporcionadas por los usuarios. En cambio, genera, autoriza y almacena sus propias claves SSH para conectarse a las máquinas virtuales y HDInsight y ejecutar los experimentos.

Cada área de trabajo lleva asociada una identidad administrada asignada por el sistema con el mismo nombre que el área de trabajo. Esta identidad administrada tiene acceso a todas las claves, secretos y certificados del almacén de claves.

## <a name="monitoring"></a>Supervisión

### <a name="metrics"></a>Métricas

Puede usar las métricas de Azure Monitor para ver y supervisar las métricas del área de trabajo de Azure Machine Learning. En [Azure Portal](https://portal.azure.com), seleccione el área de trabajo y, a continuación, **Métricas**:

[![Captura de pantalla que muestra las métricas de ejemplo de un área de trabajo](media/concept-enterprise-security/workspace-metrics.png)](media/concept-enterprise-security/workspace-metrics-expanded.png#lightbox)

Las métricas incluye información sobre ejecuciones, implementaciones y registros.

Para más información, consulte [Métricas en Azure Monitor](/azure/azure-monitor/platform/data-platform-metrics).

### <a name="activity-log"></a>Registro de actividades

Puede examinar el registro de actividad de un área de trabajo para ver diversas operaciones que se realizan en el área de trabajo. El registro incluye información básica como el nombre de la operación, el iniciador del evento y la marca de tiempo.

En esta captura de pantalla se muestra el registro de actividad de un área de trabajo:

[![Captura de pantalla que muestra el registro de actividad de un área de trabajo](media/concept-enterprise-security/workspace-activity-log.png)](media/concept-enterprise-security/workspace-activity-log-expanded.png#lightbox)

Los detalles de la solicitud de puntuación se almacenan en Application Insights. Application Insights se crea en la suscripción cuando se crea un área de trabajo. La información registrada incluye campos como HTTPMethod, UserAgent, ComputeType, RequestUrl, StatusCode, RequestId y Duration.

> [!IMPORTANT]
> Algunas acciones del área de trabajo de Azure Machine Learning no registran información en el registro de actividad. Por ejemplo, el inicio de una ejecución de entrenamiento y el registro de un modelo no se registran.
>
> Algunas de estas acciones aparecen en el área **Actividades** del área de trabajo, pero estas notificaciones no indican quién inició la actividad.

## <a name="data-flow-diagrams"></a>Diagramas de flujo de datos

### <a name="create-workspace"></a>Creación del espacio de trabajo

En el siguiente diagrama se muestra el flujo de trabajo de creación del área de trabajo.

* El usuario inicia sesión en Azure AD desde cualquiera de los clientes de Azure Machine Learning admitidos (la CLI de Azure, el SDK de Python, Azure Portal) y solicita el token de Azure Resource Manager adecuado.
* El usuario llama a Azure Resource Manager para crear el área de trabajo. 
* Azure Resource Manager se pone en contacto con el proveedor de recursos de Azure Machine Learning para aprovisionar el área de trabajo.

Durante la creación del área de trabajo, se crean recursos adicionales en la suscripción del usuario:

* Key Vault (para almacenar secretos)
* Una cuenta de Azure Storage (que incluye blob y recurso compartido de archivos)
* Azure Container Registry (para almacenar imágenes de Docker para inferencia, puntuación y experimentación)
* Application Insights (para almacenar datos de telemetría)

El usuario también puede aprovisionar otros destinos de proceso que estén asociados a un área de trabajo (como Azure Kubernetes Service o máquinas virtuales) según sea necesario.

[![Creación del flujo de trabajo del área de trabajo](media/concept-enterprise-security/create-workspace.png)](media/concept-enterprise-security/create-workspace-expanded.png#lightbox)

### <a name="save-source-code-training-scripts"></a>Almacenamiento del código fuente (scripts de entrenamiento)

En el siguiente diagrama se muestra el flujo de trabajo de la instantánea de código.

Existen directorios asociados con un área de trabajo de Azure Machine Learning (experimentos), que contienen el código fuente (scripts de entrenamiento). Estos scripts se almacenan en la máquina local y en la nube (en el almacenamiento de blobs de Azure de su suscripción). Las instantáneas de código se utilizan para la ejecución o inspección de auditorías históricas.

[![Flujo de trabajo de instantánea de código](media/concept-enterprise-security/code-snapshot.png)](media/concept-enterprise-security/code-snapshot-expanded.png#lightbox)

### <a name="training"></a>Cursos

En el siguiente diagrama se muestra el flujo de trabajo de entrenamiento.

* Para llamar a Azure Machine Learning se usa el identificador de la instantánea de código guardada en la sección anterior.
* Azure Machine Learning crea un identificador de ejecución (opcional) y un token de Machine Learning Service, que más adelante se usan en los destinos de proceso, como Proceso de Machine Learning o las máquinas virtuales, para comunicarse con este servicio.
* Puede elegir un destino de proceso administrado (como Proceso de Machine Learning) o un destino de proceso no administrado (como máquinas virtuales) para ejecutar los trabajos de entrenamiento. Estos son los flujos de datos de ambos escenarios:
   * Máquinas virtuales/HDInsight: se accede a ellos mediante las credenciales de SSH de un almacén de claves de la suscripción de Microsoft. Azure Machine Learning ejecuta código de administración en el destino de proceso que:

   1. Prepara el entorno. (Docker es una opción para máquinas virtuales y equipos locales. Consulte los siguientes pasos de Proceso de Machine Learning para entender cómo funciona la ejecución de experimentos en el contenedor de Docker).
   1. Descarga el código.
   1. Configura las variables de entorno y realiza las configuraciones.
   1. Ejecuta scripts de usuario (la instantánea de código mencionada en la sección anterior).

   * Proceso de Machine Learning, al que se accede mediante una identidad administrada por el área de trabajo.
Dado que Proceso de Machine Learning es un destino de proceso administrado (es decir, está administrado por Microsoft), se ejecuta en la suscripción de Microsoft.

   1. Si es necesario, pone en marcha la construcción de un Docker remoto.
   1. El código de administración se escribe en el recurso compartido de Azure Files del usuario.
   1. El contenedor se inicia con un comando inicial. Es decir, el código de administración que se describe en el paso anterior.

#### <a name="querying-runs-and-metrics"></a>Consulta de ejecuciones y métricas

En el diagrama de flujo siguiente, este paso se produce cuando el destino de proceso de entrenamiento escribe las métricas de ejecución de nuevo en Azure Machine Learning desde el almacenamiento de la base de datos de Cosmos DB. Los clientes pueden llamar a Azure Machine Learning. Machine Learning, a su vez, extrae las métricas de la base de datos de Cosmos DB y las devuelve al cliente.

[![Flujo de trabajo de entrenamiento](media/concept-enterprise-security/training-and-metrics.png)](media/concept-enterprise-security/training-and-metrics-expanded.png#lightbox)

### <a name="creating-web-services"></a>Creación de servicios web

En el siguiente diagrama se muestra el flujo de trabajo de inferencia. La inferencia, o puntuación de modelos, es la fase en la que se usa el modelo implementado para la predicción, muy frecuentemente con datos de producción.

Estos son los detalles:

* El usuario registra un modelo mediante un cliente, como el SDK de Azure Machine Learning.
* El usuario crea una imagen mediante un modelo, un archivo de puntuación y otras dependencias del modelo.
* La imagen de Docker se crea y se almacena en Azure Container Registry.
* El servicio web se implementa en el destino de proceso (Container Instances/AKS) mediante la imagen creada en el paso anterior.
* Los detalles de la solicitud de puntuación se almacenan en Application Insights, que se incluye en la suscripción del usuario.
* También se insertan datos de telemetría en la suscripción de Microsoft o Azure.

[![Flujo de trabajo de inferencia](media/concept-enterprise-security/inferencing.png)](media/concept-enterprise-security/inferencing-expanded.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

* [Protección de los servicios web de Azure Machine Learning con SSL](how-to-secure-web-service.md)
* [Consumir un modelo de Machine Learning implementado como un servicio web](how-to-consume-web-service.md)
* [Cómo ejecutar predicciones por lotes](how-to-run-batch-predictions.md)
* [Supervisión de los modelos de Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Recopilar datos de modelos en producción](how-to-enable-data-collection.md)
* [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Uso de Azure Machine Learning con Azure Virtual Network](how-to-enable-virtual-network.md)
* [Procedimientos recomendados para compilar sistemas de recomendaciones](https://github.com/Microsoft/Recommenders)
* [Compilación de una API de recomendaciones en tiempo real en Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
