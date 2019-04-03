---
title: Introducción a Azure Data Lake Storage Gen1 | Microsoft Docs
description: Descripción de Azure Data Lake Storage Gen 1 (antes conocido como Azure Data Lake Store) y el valor que aporta en comparación con otros almacenes de datos
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 438eab091fac103b66f0789beca0098b87ee44cd
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885662"
---
# <a name="overview-of-azure-data-lake-storage-gen1"></a>Introducción a Azure Data Lake Storage Gen1

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Storage Gen1 es un repositorio a gran escala de nivel empresarial para cargas de trabajo de análisis de macrodatos. Azure Data Lake permite capturar datos de cualquier tamaño, tipo y velocidad de ingesta en un único lugar para realizar análisis exploratorios y operativos.

> [!TIP]
> Use la [ruta de aprendizaje de Data Lake Storage Gen1](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) para empezar a explorar el servicio Data Lake Storage Gen1.
> 
> 

Se puede acceder a Data Lake Storage Gen1 desde Hadoop (disponible con el clúster de HDInsight) mediante las API REST compatibles con WebHDFS. Está diseñado específicamente para habilitar el análisis de los datos almacenados y está optimizado para el rendimiento en escenarios de análisis de datos. De forma inmediata, incluye todas las capacidades de nivel empresarial –seguridad, facilidad de administración, escalabilidad, confiabilidad y disponibilidad– esenciales para los casos de uso empresariales reales.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Entre las capacidades clave de Data Lake Storage Gen1, se incluyen las siguientes.

### <a name="built-for-hadoop"></a>Creado para Hadoop
Data Lake Storage Gen1 es un sistema de archivos de Apache Hadoop compatible con el Sistema de archivos distribuido de Hadoop (HDFS) que funciona con el ecosistema de Hadoop.  Las aplicaciones o los servicios de HDInsight existentes que usen la API de WebHDFS se pueden integrar fácilmente con Data Lake Storage Gen1. Además, Data Lake Storage Gen1 expone una interfaz de REST compatible con WebHDFS para aplicaciones.

Los datos almacenados en Data Lake Storage Gen1 se pueden analizar fácilmente mediante marcos analíticos de Hadoop como MapReduce o Hive. Los clústeres de Microsoft Azure HDInsight se pueden aprovisionar y configurar para que accedan directamente a datos almacenados en Data Lake Storage Gen1.

### <a name="unlimited-storage-petabyte-files"></a>Almacenamiento ilimitado, archivos de petabytes de tamaño
Data Lake Storage Gen1 proporciona almacenamiento ilimitado y es adecuado para almacenar diversos datos para análisis. No se impone ningún límite al tamaño de cuenta, el tamaño de archivo o la cantidad de datos que se pueden almacenar en un Data Lake. El tamaño de los archivos individuales puede oscilar entre kilobytes y petabytes, por lo que es una buena opción para almacenar cualquier tipo de datos. Los datos se almacenan de forma duradera mediante la realización de varias copias y no hay ningún límite al período de tiempo durante el que se pueden almacenar los datos en Data Lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Rendimiento optimizado para el análisis de macrodatos
Data Lake Storage Gen1 se creó para ejecutar sistemas de análisis a gran escala que requieren un procesamiento masivo con el fin de consultar y analizar grandes cantidades de datos. Data Lake distribuye partes de un archivo entre varios servidores de almacenamiento individuales. Esto mejora el rendimiento de lectura cuando se lee el archivo en paralelo para realizar análisis de datos.

### <a name="enterprise-ready-highly-available-and-secure"></a>Preparada para la empresa: Alta disponibilidad y segura
Data Lake Storage Gen1 proporciona la confiabilidad y disponibilidad estándar del sector. Los recursos de datos se almacenan de forma duradera realizando copias redundantes para protegerse ante los errores inesperados. Las empresas pueden usar Data Lake Storage Gen1 en sus soluciones como parte importante de su plataforma de datos existente.

Además, Data Lake Storage Gen1 también proporciona seguridad de nivel empresarial para los datos almacenados. Para obtener más información, consulte [Securing data in Azure Data Lake Storage Gen1](#DataLakeStoreSecurity) (Protección de los datos almacenados en Azure Data Lake Storage Gen1).

### <a name="all-data"></a>Todos los datos
Data Lake Storage Gen1 puede almacenar cualquier dato en su formato nativo, tal cual, sin necesidad de transformarlo antes. Asimismo, Data Lake Storage Gen1 no requiere la definición de un esquema antes de que se carguen los datos, sino que deja que cada marco analítico interprete los datos y defina un esquema en el momento del análisis. La capacidad de almacenar archivos de formatos y tamaños arbitrarios hace posible que Data Lake Storage Gen1 administre datos estructurados, semiestructurados y no estructurados.

Los contenedores de datos de Data Lake Storage Gen1 son básicamente carpetas y archivos. Se opera en los datos almacenados mediante los SDK, Azure Portal y Azure Powershell. Siempre que ponga los datos en el almacén mediante estas interfaces y los contenedores adecuados, puede almacenar cualquier tipo de datos. Data Lake Storage Gen1 no realiza ningún control especial de datos según el tipo de datos que almacene.

## <a name="DataLakeStoreSecurity"></a>Protección de datos en Data Lake Storage Gen1
Data Lake Storage Gen1 usa Azure Active Directory para la autenticación y listas de control de acceso (ACL) para administrar el acceso a los datos.

| Característica | DESCRIPCIÓN |
| --- | --- |
| Authentication |Data Lake Storage Gen1 se integra con Azure Active Directory (AAD) para la administración de identidades y accesos para todos los datos almacenados en Data Lake Storage Gen1. Como resultado de la integración, Data Lake Storage Gen1 se beneficia de todas las características de AAD, lo que incluye la autenticación multifactor, el acceso condicional, el control de acceso basado en roles, la supervisión del uso de aplicaciones, la supervisión y las alertas de seguridad, etc. Igualmente, Data Lake Storage Gen1 es compatible con el protocolo OAuth 2.0 para la autenticación en la interfaz de REST. Consulte [autenticación de Data Lake Storage Gen1](data-lakes-store-authentication-using-azure-active-directory.md).|
| Control de acceso |Data Lake Storage Gen1 proporciona control de acceso gracias a la compatibilidad con los permisos de estilo POSIX que expone el protocolo WebHDFS. Las listas de control de acceso se pueden habilitar en la carpeta raíz, en subcarpetas y en archivos individuales. Para obtener más información sobre cómo funcionan las ACL en el contexto de Data Lake Storage Gen1, consulte [Control de acceso en Data Lake Storage Gen1](data-lake-store-access-control.md). |
| Cifrado |Data Lake Storage Gen1 también proporciona el cifrado de los datos que se almacenan en la cuenta. Puede especificar la configuración de cifrado mientras crea una cuenta de Data Lake Storage Gen1. Puede elegir si cifrar o no los datos. Para obtener más información, consulte el artículo de [Cifrado de datos en Data Lake Storage Gen1](data-lake-store-encryption.md). Para obtener instrucciones sobre cómo proporcionar una configuración relacionada con el cifrado, consulte [Introducción a Azure Data Lake Storage Gen1 con Azure Portal](data-lake-store-get-started-portal.md). |

¿Quiere obtener más información acerca cómo proteger los datos en Data Lake Storage Gen1? siga estos vínculos.

* Para obtener instrucciones sobre cómo proteger datos en el Data Lake Storage Gen1, consulte [Securing data in Azure Data Lake Storage Gen1](data-lake-store-secure-data.md) (Proteger los datos en Azure Data Lake Storage Gen1).
* ¿Prefiere vídeos? [Vea este vídeo](https://mix.office.com/watch/1q2mgzh9nn5lx) sobre cómo proteger los datos almacenados en Data Lake Storage Gen1.

## <a name="applications-compatible-with-data-lake-storage-gen1"></a>Aplicaciones compatibles con Data Lake Storage Gen1
Data Lake Storage Gen1 es compatible con la mayor parte de los componentes de código abierto del ecosistema de Hadoop. También se integra perfectamente con otros servicios de Azure. Por ello, Data Lake Storage Gen1 es una opción perfecta para sus necesidades de almacenamiento de datos. Para obtener más información acerca de cómo se puede usar Data Lake Storage Gen1 tanto con componentes de código abierto, así como con otros servicios de Azure siga estos vínculos.

* Consulte [Applications and services compatible with Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md) (Aplicaciones y servicios compatibles con Azure Data Lake Storage Gen1) para obtener una lista de aplicaciones de código abierto interoperables con Data Lake Storage Gen1.
* Consulte la página sobre la [integración con otros servicios de Azure](data-lake-store-integrate-with-other-services.md) para saber cómo usar Data Lake Storage Gen1 con otros servicios de Azure, y así poder obtener una gama más amplia de escenarios.
* Consulte [Scenarios for using Data Lake Storage Gen1](data-lake-store-data-scenarios.md) (Escenarios para usar Data Lake Storage Gen1) para obtener información acerca de cómo usar Data Lake Storage Gen1 en escenarios como la ingesta de datos, el procesamiento de datos, la descarga de datos y la visualización de datos.

## <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>¿Qué es el sistema de archivos de Data Lake Storage Gen1 (adl://)?
En entornos de Hadoop (disponibles con el clúster de HDInsight), se puede acceder a Data Lake Storage Gen1 a través del nuevo sistema de archivos, AzureDataLakeFilesystem (adl://). Las aplicaciones y los servicios que usan adl:// pueden aprovechar aún más la optimización del rendimiento que no está actualmente disponible en WebHDFS. Como resultado, Data Lake Storage Gen1 ofrece la posibilidad de elegir entre disponer del mejor rendimiento con la opción recomendada que usa adl:// o mantener el código existente usando la API de WebHDFS directamente. Azure HDInsight aprovecha completamente AzureDataLakeFilesystem para proporcionar el mejor rendimiento en Data Lake Storage Gen1.

Puede obtener acceso a los datos en Data Lake Storage Gen1 mediante `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Para obtener más información sobre cómo acceder a los datos en Data Lake Storage Gen1, consulte [View properties of the stored data](data-lake-store-get-started-portal.md#properties) (Ver las propiedades de los datos almacenados).

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Data Lake Storage Gen1 mediante el Portal de Azure](data-lake-store-get-started-portal.md)
* [Introducción a Azure Data Lake Storage Gen1 con SDK de .NET](data-lake-store-get-started-net-sdk.md)
* [Uso de Azure HDInsight con Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)