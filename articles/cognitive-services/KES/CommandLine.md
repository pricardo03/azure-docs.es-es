---
title: Interfaz de la línea de comandos de Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Use la interfaz de la línea de comandos para compilar archivos de índice y de gramática a partir de datos estructurados y, después, impleméntelos como servicios web.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: conceptual
ms.date: 03/24/2016
ms.author: paulhsu
ms.openlocfilehash: 7ec2282317019275b15a8e506753408c75a68561
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127172"
---
# <a name="command-line-interface"></a>Interfaz de línea de comandos

La interfaz de la línea de comandos de Knowledge Exploration Service (KES) proporciona la capacidad de compilar archivos de índice y de gramática a partir de datos estructurados e implementarlos como servicios web.  Usa la sintaxis general: `kes.exe <command> <required_args> [<optional_args>]`.  Puede ejecutar `kes.exe` sin argumentos para que se muestre una lista de comandos o `kes.exe <command>` para mostrar una lista de los argumentos disponibles para el comando especificado.  A continuación se muestra una lista de los comandos disponibles:

* build_index
* build_grammar
* host_service
* deploy_service
* describe_index
* describe_grammar

<a name="build_index-command"></a>

## <a name="buildindex-command"></a>Comando build_index

El comando **build_index** compila un archivo de índice binario a partir de un archivo de definición de esquema y un archivo de datos de objetos que se tienen que indexar.  El archivo de índice resultante se puede usar para evaluar expresiones de consulta estructuradas o para generar interpretaciones de consultas en lenguaje natural junto con un archivo de gramática compilado.

`kes.exe build_index <schemaFile> <dataFile> <indexFile> [options]`

| Parámetro      | DESCRIPCIÓN               |
|----------------|---------------------------|
| `<schemaFile>` | Ruta de acceso de esquema de entrada |
| `<dataFile>`   | Ruta de acceso de datos de entrada   |
| `<indexFile>`  | Ruta de acceso de índice de salida |
| `--description <description>` | Cadena de descripción |
| `--remote <vmSize>`           | Tamaño de máquina virtual para compilación remota |

Estos archivos se pueden especificar mediante rutas de acceso a archivos locales o rutas de dirección URL a blobs de Azure.  El archivo de esquema describe la estructura de los objetos que se están indexando, así como las operaciones que se deben admitir (consulte [Formato de esquema](SchemaFormat.md)).  El archivo de datos enumera los objetos y valores de atributo que se deben indexar (consulte [Formato de datos](DataFormat.md)).  Si la compilación se realiza correctamente, el archivo de índice de salida contiene una representación comprimida de los datos de entrada que admiten las operaciones deseadas.  

Se puede especificar opcionalmente una cadena de descripción para identificar posteriormente un índice binario mediante el comando **describe_index**.  

De forma predeterminada, el índice se compila en la máquina local.  Fuera del entorno de Azure, las compilaciones locales se limitan a archivos de datos que contienen hasta 10 000 objetos.  Si se especifica el identificador --remote, el índice se compilará en una máquina virtual de Azure creada temporalmente del tamaño especificado.  Esto permite que se compilen eficazmente índices grandes mediante máquinas virtuales de Azure con más memoria.  Para evitar la paginación que ralentiza el proceso de compilación, se recomienda usar una máquina virtual con el triple de memoria RAM del que necesita el tamaño de archivo de datos de entrada.  Para obtener una lista de tamaños de máquina virtual disponibles, consulte [Tamaños de máquinas virtuales](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

> [!TIP] 
> Para compilaciones más rápidas, ordene previamente los objetos del archivo de datos por probabilidad decreciente.

<a name="build_grammar-command"></a>

## <a name="buildgrammar-command"></a>Comando build_grammar

El comando **build_grammar** compila una gramática especificada en XML en un archivo de gramática binario.  El archivo de gramática resultante se puede utilizar junto con un archivo de índice para generar interpretaciones de consultas en lenguaje natural.

`kes.exe build_grammar <xmlFile> <grammarFile>`

| Parámetro       | DESCRIPCIÓN               |
|-----------------|---------------------------|
| `<xmlFile>`     | Ruta de acceso de especificación de gramática XML de entrada |
| `<grammarFile>` | Ruta de acceso de gramática compilada de salida         |

Estos archivos se pueden especificar mediante rutas de acceso a archivos locales o rutas de dirección URL a blobs de Azure.  La especificación de gramática describe el conjunto de expresiones en lenguaje natural ponderadas y sus interpretaciones semánticas (consulte [Formato de gramática](GrammarFormat.md)).  Cuando la compilación se realiza correctamente, el archivo de gramática de salida contiene una representación binaria de la especificación de gramática que permite una rápida descodificación.

<a name="host_service-command"/>

## <a name="hostservice-command"></a>Comando host_service

El comando **host_service** hospeda una instancia del servicio Knowledge Exploration Service en la máquina local.

`kes.exe host_service <grammarFile> <indexFile> [options]`

| Parámetro       | DESCRIPCIÓN                |
|-----------------|----------------------------|
| `<grammarFile>` | Ruta de acceso de gramática binaria de entrada         |
| `<indexFile>`   | Ruta de acceso de índice binario de entrada           |
| `--port <port>` | Número de puerto local.  Predeterminado: 8000 |

Estos archivos se pueden especificar mediante rutas de acceso a archivos locales o rutas de dirección URL a blobs de Azure.  Un servicio web se hospedará en http://localhost:&lt;port&gt;/.  Consulte [API web](WebAPI.md) para obtener una lista de las operaciones admitidas.

Fuera del entorno de Azure, los servicios que se hospedan localmente se limitan a archivos de índice de hasta 1 MB de tamaño, 10 solicitudes por segundo y 1000 llamadas en total.  Para superar estas limitaciones, ejecute **host_service** dentro de una máquina virtual de Azure o realice la implementación en un servicio en la nube de Azure mediante **deploy_service**.

<a name="deploy_service-command"/>

## <a name="deployservice-command"></a>Comando deploy_service

El comando **deploy_service** implementa una instancia del servicio Knowledge Exploration Service en un servicio en la nube de Azure.

`kes.exe deploy_service <grammarFile> <indexFile> <serviceName> <vmSize>[options]`

| Parámetro       | DESCRIPCIÓN                  |
|-----------------|------------------------------|
| `<grammarFile>` | Ruta de acceso de gramática binaria de entrada           |
| `<indexFile>`   | Ruta de acceso de índice binario de entrada             |
| `<serviceName>` | Nombre del servicio en la nube de destino |
| `<vmSize>`      | Tamaño de la máquina virtual del servicio en la nube     |
| `--slot <slot>` | Ranura de servicio en la nube: "almacenamiento provisional" (valor predeterminado), "producción" |

Estos archivos se pueden especificar mediante rutas de acceso a archivos locales o rutas de dirección URL a blobs de Azure.  El nombre de servicio especifica un servicio en la nube de Azure configurado previamente (consulte [Creación e implementación de un servicio en la nube](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md)).  El comando implementará automáticamente el servicio Knowledge Exploration Service en el servicio en la nube de Azure especificado, mediante máquinas virtuales del tamaño especificado.  Para evitar la paginación que reduce significativamente el rendimiento, se recomienda usar una máquina virtual con 1 GB más de RAM que el tamaño del archivo de índice de entrada.  Para obtener una lista de los tamaños de máquina virtual disponibles, consulte [Tamaños de Cloud Services](../../../articles/cloud-services/cloud-services-sizes-specs.md).

De forma predeterminada, el servicio se implementa en el entorno de ensayo y se puede sobrescribir opcionalmente mediante el parámetro --slot.  Consulte [API web](WebAPI.md) para obtener una lista de las operaciones admitidas.

<a name="describe_index-command"/>

## <a name="describeindex-command"></a>Comando describe_index

El comando **describe_index** genera información sobre un archivo de índice, incluyendo el esquema y la descripción.

`kes.exe describe_index <indexFile>`

| Parámetro     | DESCRIPCIÓN      |
|---------------|------------------|
| `<indexFile>` | Ruta de acceso de índice de entrada |

Este archivo se puede especificar mediante una ruta de acceso a un archivo local o una ruta de acceso de dirección URL a un blob de Azure.  La cadena de descripción de salida se puede especificar mediante el parámetro --description del comando **build_index**.

<a name="describe_grammar-command"/>

## <a name="describegrammar-command"></a>Comando describe_grammar

El comando **describe_grammar** genera la especificación de gramática original utilizada para compilar la gramática binaria.

`kes.exe describe_grammar <grammarFile>`

| Parámetro       | DESCRIPCIÓN      |
|-----------------|------------------|
| `<grammarFile>` | Ruta de acceso de gramática de entrada |

Este archivo se puede especificar mediante una ruta de acceso a un archivo local o una ruta de acceso de dirección URL a un blob de Azure.

