---
title: ¿Qué es Azure Data Lake Storage Gen1? | Microsoft Docs
description: Introducción a Data Lake Storage Gen1 (anteriormente conocido como Azure Data Lake Store) y el valor que proporciona a través de otros almacenes de datos
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: twooley
ms.openlocfilehash: 518c129aedf3161ab761d09139e0c4d988dd2cbc
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681835"
---
# <a name="what-is-azure-data-lake-storage-gen1"></a>¿Qué es Azure Data Lake Storage Gen1?

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Storage Gen1 es un repositorio a gran escala de nivel empresarial para cargas de trabajo de análisis de macrodatos. Azure Data Lake permite capturar datos de cualquier tamaño, tipo y velocidad de ingesta en un único lugar para realizar análisis exploratorios y operativos.

Se puede acceder a Data Lake Storage Gen1 desde Hadoop (disponible con el clúster de HDInsight) mediante las API REST compatibles con WebHDFS. Se ha diseñado para habilitar el análisis de los datos almacenados y está optimizado para rendimiento en escenarios de análisis de datos. Data Lake Storage Gen1 incluye todas las funcionalidades de nivel empresarial: seguridad, facilidad de uso, escalabilidad, confiabilidad y disponibilidad.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

## <a name="key-capabilities"></a>Principales capacidades

Entre las capacidades clave de Data Lake Storage Gen1, se incluyen las siguientes.

### <a name="built-for-hadoop"></a>Creado para Hadoop

Data Lake Storage Gen1 es un sistema de archivos de Apache Hadoop que es compatible con sistema de archivos de distribuido de Hadoop (HDFS) y funciona con el ecosistema de Hadoop. Las aplicaciones o los servicios de HDInsight existentes que usen la API de WebHDFS se pueden integrar fácilmente con Data Lake Storage Gen1. Además, Data Lake Storage Gen1 expone una interfaz de REST compatible con WebHDFS para aplicaciones.

Puede analizar fácilmente los datos almacenados en Data Lake Storage Gen1 mediante marcos analíticos de Hadoop como MapReduce o Hive. Puede aprovisionar clústeres de HDInsight de Azure y configurarlos para tener acceso directamente a los datos almacenados en Data Lake Storage Gen1.

### <a name="unlimited-storage-petabyte-files"></a>Almacenamiento ilimitado, archivos de petabytes de tamaño

Data Lake Storage Gen1 proporciona almacenamiento ilimitado y puede almacenar una variedad de datos para el análisis. No impone ningún límite en tamaños de cuentas, el tamaño de archivo o la cantidad de datos que pueden almacenarse en un lago de datos. Los archivos individuales pueden oscilar entre kilobytes y petabytes de tamaño. Datos se almacenan de forma duradera mediante la realización de varias copias. No hay ningún límite en la cantidad de tiempo para el que se pueden almacenar los datos en data lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Rendimiento optimizado para el análisis de macrodatos

Data Lake Storage Gen1 se ha creado para ejecutar sistemas de análisis a gran escala que requieren un procesamiento masivo para consultar y analizar grandes cantidades de datos. Data Lake distribuye partes de un archivo entre varios servidores de almacenamiento individuales. Esto mejora el rendimiento de lectura cuando se lee el archivo en paralelo para realizar análisis de datos.

### <a name="enterprise-ready-highly-available-and-secure"></a>Preparado para la empresa: durable y con una elevada disponibilidad

Data Lake Storage Gen1 proporciona la confiabilidad y disponibilidad estándar del sector. Los recursos de datos se almacenan de forma duradera realizando copias redundantes para protegerse ante los errores inesperados.

Además, Data Lake Storage Gen1 también proporciona seguridad de nivel empresarial para los datos almacenados. Para obtener más información, consulte [Securing data in Azure Data Lake Storage Gen1](#DataLakeStoreSecurity) (Protección de los datos almacenados en Azure Data Lake Storage Gen1).

### <a name="all-data"></a>Todos los datos

Data Lake Storage Gen1 puede almacenar los datos en su formato nativo, sin necesidad de transformarlo antes. Asimismo, Data Lake Storage Gen1 no requiere la definición de un esquema antes de que se carguen los datos, sino que deja que cada marco analítico interprete los datos y defina un esquema en el momento del análisis. La capacidad de almacenar los archivos de formatos y tamaños arbitrarios hace posible para Data Lake Storage Gen1 administrar los datos estructurados, semiestructurados y estructurados.

Los contenedores de datos de Data Lake Storage Gen1 son básicamente carpetas y archivos. Operar en los datos almacenados mediante el SDK, el portal de Azure y Azure Powershell. Si coloca los datos en el almacén mediante estas interfaces y los contenedores adecuados, puede almacenar cualquier tipo de datos. Data Lake Storage Gen1 no realiza ningún control especial de datos según el tipo de datos que almacene.

## <a name="DataLakeStoreSecurity"></a>Protección de datos

Listas (ACL) para administrar el acceso de control de usos de Data Lake Storage Gen1 Azure Active Directory (Azure AD) para la autenticación y acceso a los datos.

| Característica | DESCRIPCIÓN |
| --- | --- |
| Authentication |Data Lake Storage Gen1 se integra con Azure AD para la administración de identidades y acceso para todos los datos almacenados en Data Lake Storage Gen1. Debido a la integración, Data Lake Storage Gen1 ventajas de todo Azure AD de características como la autenticación multifactor, acceso condicional, control de acceso basado en roles, la supervisión del uso de aplicaciones, supervisión de seguridad y alertas y así sucesivamente. Igualmente, Data Lake Storage Gen1 es compatible con el protocolo OAuth 2.0 para la autenticación en la interfaz de REST. Consulte [Data Lake Storage Gen1 autenticación](data-lakes-store-authentication-using-azure-active-directory.md).|
| Control de acceso |Data Lake Storage Gen1 proporciona control de acceso gracias a la compatibilidad con los permisos de estilo POSIX que expone el protocolo WebHDFS. Puede permitir que las ACL en la carpeta raíz, en las subcarpetas y archivos individuales. Para obtener más información acerca de cómo funcionan las ACL en el contexto de Data Lake Storage Gen1, consulte [control de acceso en Data Lake Storage Gen1](data-lake-store-access-control.md). |
| Cifrado |Data Lake Storage Gen1 también proporciona cifrado de datos que se almacenan en la cuenta. Puede especificar la configuración de cifrado mientras crea una cuenta de Data Lake Storage Gen1. Puede elegir si cifrar o no los datos. Para obtener más información, consulte el artículo de [Cifrado de datos en Data Lake Storage Gen1](data-lake-store-encryption.md). Para obtener instrucciones sobre cómo proporcionar la configuración relacionada con el cifrado, consulte [empezar a trabajar con Data Lake Storage Gen1 mediante Azure portal](data-lake-store-get-started-portal.md). |

Para obtener instrucciones sobre cómo proteger datos en el Data Lake Storage Gen1, consulte [Securing data in Azure Data Lake Storage Gen1](data-lake-store-secure-data.md) (Proteger los datos en Azure Data Lake Storage Gen1).

## <a name="application-compatibility"></a>Compatibilidad de aplicación

Data Lake Storage Gen1 es compatible con la mayor parte de los componentes de código abierto del ecosistema de Hadoop. También se integra bien con otros servicios de Azure. Para obtener más información acerca de cómo puede usar Data Lake Storage Gen1 con componentes de código abierto y otros servicios de Azure, use los vínculos siguientes:

- Consulte [Aplicaciones y servicios compatibles con Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md) para obtener una lista de aplicaciones de código abierto interoperables con Data Lake Storage Gen1.
- Consulte [la integración con otros servicios de Azure](data-lake-store-integrate-with-other-services.md) para aprender a usar Data Lake Storage Gen1 con otros servicios de Azure para habilitar una gama más amplia de escenarios.
- Consulte [Scenarios for using Data Lake Storage Gen1](data-lake-store-data-scenarios.md) (Escenarios para usar Data Lake Storage Gen1) para obtener información acerca de cómo usar Data Lake Storage Gen1 en escenarios como la ingesta de datos, el procesamiento de datos, la descarga de datos y la visualización de datos.

## <a name="data-lake-storage-gen1-file-system"></a>Sistema de archivos de Data Lake Storage Gen1

Data Lake Storage Gen1 se puede acceder mediante el sistema de archivos, AzureDataLakeFilesystem (adl: / /) en entornos de Hadoop (disponibles con el clúster de HDInsight). Las aplicaciones y servicios que usan adl: / / pueden aprovechar aún más las optimizaciones de rendimiento que no están actualmente disponibles en WebHDFS. Como resultado, proporciona Data Lake Storage Gen1 la flexibilidad necesaria para cualquiera que use del mejor rendimiento con la opción recomendada de usar adl: / / o mantener el código existente debe seguir usando la API de WebHDFS directamente. Azure HDInsight aprovecha completamente AzureDataLakeFilesystem para proporcionar el mejor rendimiento en Data Lake Storage Gen1.

Puede obtener acceso a los datos en Data Lake Storage Gen1 mediante `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Para obtener más información acerca de cómo obtener acceso a los datos en Data Lake Storage Gen1, consulte [ver las propiedades de los datos almacenados](data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Data Lake Storage Gen1 mediante el portal de Azure](data-lake-store-get-started-portal.md)
- [Introducción a Data Lake Storage Gen1 con SDK de .NET](data-lake-store-get-started-net-sdk.md)
- [Use Azure HDInsight with Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md) (Uso de Azure HDInsight con Data Lake Storage Gen1)