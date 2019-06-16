---
title: 'LogDownloader: Custom Decision Service'
titlesuffix: Azure Cognitive Services
description: Descargue los archivos de registro que genera Azure Custom Decision Service.
services: cognitive-services
author: marco-rossi29
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: marossi
ms.openlocfilehash: 8a8f669c33f40fb80dc826ec04203880dee74d82
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60829997"
---
# <a name="logdownloader"></a>LogDownloader

Descargue los archivos de registro generados por Azure Custom Decision Service y genere los archivos *.gz* que se utilizan en Experimentación.

## <a name="prerequisites"></a>Requisitos previos

- Python 3: instalado y en su ruta de acceso. Se recomienda la versión de 64 bits para controlar archivos grandes.
- El repositorio *Microsoft/mwt-ds*: [Clonación del repositorio](https://github.com/Microsoft/mwt-ds).
- El paquete *azure-storage-blob*: para obtener los detalles de la instalación, vaya a la [biblioteca de Microsoft Azure Storage para Python](https://github.com/Azure/azure-storage-python#option-1-via-pypi).
- Escriba la cadena de conexión de almacenamiento de Azure en *mwt-ds/DataScience/ds.config*: siga la plantilla *my_app_id: my_connectionString*. Puede especificar varios `app_id`. Al ejecutar `LogDownloader.py`, si la entrada `app_id` no se encuentra en `ds.config`, `LogDownloader.py` usa la cadena de conexión `$Default`.

## <a name="usage"></a>Uso

Vaya a `mwt-ds/DataScience` y ejecute `LogDownloader.py` con los argumentos necesarios, tal como se detalla en el siguiente código:

```cmd
python LogDownloader.py [-h] -a APP_ID -l LOG_DIR [-s START_DATE]
                        [-e END_DATE] [-o OVERWRITE_MODE] [--dry_run]
                        [--create_gzip] [--delta_mod_t DELTA_MOD_T]
                        [--verbose] [-v VERSION]
```

### <a name="parameters"></a>Parámetros

| Entrada | DESCRIPCIÓN | Valor predeterminado |
| --- | --- | --- |
| `-h`, `--help` | Muestra el mensaje de ayuda y sale. | |
| `-a APP_ID`, `--app_id APP_ID` | Id. de la aplicación (es decir, nombre del contenedor de blobs de Azure Storage). | Obligatorio |
| `-l LOG_DIR`, `--log_dir LOG_DIR` | Directorio base para la descarga de datos (se crea una subcarpeta).  | Obligatorio |
| `-s START_DATE`, `--start_date START_DATE` | Fecha inicial de la descarga (incluida), en formato *AAAA-MM-DD*. | `None` |
| `-e END_DATE`, `--end_date END_DATE` | Fecha final de la descarga (incluida), en formato *AAAA-MM-DD*. | `None` |
| `-o OVERWRITE_MODE`, `--overwrite_mode OVERWRITE_MODE` | Modo de sobrescritura que se usará. | |
| | `0`: no sobrescribir nunca; preguntar al usuario si se utilizan blobs actualmente. | Valor predeterminado |
| | `1`: preguntar al usuario cómo se debe proceder cuando los archivos tengan tamaños diferentes o cuando se estén usando blobs. | |
| | `2`: sobrescribir siempre; descargar los blobs utilizados actualmente. | |
| | `3`: no sobrescribir nunca y anexar si el tamaño es mayor, sin preguntar; descargar los blobs utilizados actualmente. | |
| | `4`: no sobrescribir nunca y anexar si el tamaño es mayor, sin preguntar; omitir los blobs utilizados actualmente. | |
| `--dry_run` | Imprimir los blobs que se habrían descargado, sin realizar la descarga. | `False` |
| `--create_gzip` | Crear un archivo *.gzip* para Vowpal Wabbit. | `False` |
| `--delta_mod_t DELTA_MOD_T` | Período de tiempo, en segundos, para detectar si un archivo está actualmente en uso. | `3600` s (`1` hora) |
| `--verbose` | Imprimir más detalles. | `False` |
| `-v VERSION`, `--version VERSION` | Versión del descargador de registro que se usará. | |
| | `1`: para los registros sin procesar (solo para compatibilidad con versiones anteriores). | En desuso |
| | `2`: para los registros procesados. | Valor predeterminado |

### <a name="examples"></a>Ejemplos

Para un simulacro de descarga de todos los datos del contenedor de blobs de Azure Storage, utilice el código siguiente:
```cmd
python LogDownloader.py -a your_app_id -l d:\data --dry_run
```

Para descargar solo los registros creados desde el 1 de enero de 2018 con `overwrite_mode=4`, utilice el código siguiente:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4
```

Para crear un archivo *.gzip* que combine todos los archivos descargados, utilice el código siguiente:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4 --create_gzip
```
