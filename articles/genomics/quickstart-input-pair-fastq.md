---
title: Enviar un flujo de trabajo con entradas de archivo FASTQ - Microsoft Genomics
titleSuffix: Azure
description: En este artículo se da por supuesto que tiene instalado el cliente de msgen y ha ejecutado correctamente los datos de ejemplo a través del servicio.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: conceptual
ms.date: 12/07/2017
ms.openlocfilehash: 2662a8f52c58a39916e5789fa9ed7fadd91216c0
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57438812"
---
# <a name="submit-a-workflow-using-fastq-file-inputs-in-microsoft-genomics"></a>Inicio rápido: Envío de un flujo de trabajo con entradas de archivo FASTQ en Microsoft Genomics

En este artículo se muestra cómo enviar un flujo de trabajo para el servicio Microsoft Genomics, si los archivos de entrada son un único par de archivos FASTQ. En este tema se da por supuesto que ya ha instalado y ejecutado el cliente `msgen` y está familiarizado con el uso de Azure Storage. Si ha enviado correctamente un flujo de trabajo con los datos de ejemplo proporcionado, está listo para continuar con este artículo. 

## <a name="set-up-upload-your-fastq-files-to-azure-storage"></a>Configuración: Cargar los archivos FASTQ en Azure storage
Supongamos que tiene dos archivos, *reads_1.fq.gz* y *reads_2.fq.gz* y que los ha cargado en la cuenta de almacenamiento *myaccount* de Azure como **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/inputs/reads_1<span></span>.fq<span></span>.gz<span></span>** y **https://<span></span>myaccount.blob.core.<span></span>windows<span></span>.net/<span></span>inputs/<span></span>reads_2.fq<span></span>.gz<span></span>**. Tiene la dirección URL de la API y la clave de acceso. Desea los resultados en **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>**.


## <a name="submit-your-job-to-the-msgen-client"></a>Envío del trabajo al cliente `msgen` 

Este es el conjunto mínimo de argumentos que debe proporcionar al cliente `msgen`; los saltos de línea se han agregado para mayor claridad:

Para Windows:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads_1.fq.gz ^
  --input-blob-name-2 reads_2.fq.gz ^
  --output-storage-account-name myaccount ^
  --output-storage-account-key <storage access key to "myaccount"> ^
  --output-storage-account-container outputs
```

Para Unix:

```
msgen submit \
  --api-url-base <Genomics API URL> \
  --access-key <Genomics access key> \
  --process-args R=b37m1 \
  --input-storage-account-name myaccount \
  --input-storage-account-key <storage access key to "myaccount"> \
  --input-storage-account-container inputs \
  --input-blob-name-1 reads_1.fq.gz \
  --input-blob-name-2 reads_2.fq.gz \
  --output-storage-account-name myaccount \
  --output-storage-account-key <storage access key to "myaccount"> \
  --output-storage-account-container outputs
```


Si prefiere usar un archivo de configuración, esto es lo que podría contener:

```
api_url_base:                     <Genomics API URL>
access_key:                       <Genomics access key>
process_args:                     R=b37m1
input_storage_account_name:       myaccount
input_storage_account_key:        <storage access key to "myaccount">
input_storage_account_container:  inputs
input_blob_name_1:                reads_1.fq.gz
input_blob_name_2:                reads_2.fq.gz
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

Envíe el archivo `config.txt` con esta invocación: `msgen submit -f config.txt`

## <a name="next-steps"></a>Pasos siguientes
En este artículo, se van a cargar un par de archivos FASTQ en Azure Storage y se va a enviar un flujo de trabajo al servicio Microsoft Genomics mediante el cliente de Python `msgen`. Para obtener más información sobre el envío del flujo de trabajo y otros comandos que puede usar con el servicio Microsoft Genomics, consulte nuestra [preguntas más frecuentes sobre](frequently-asked-questions-genomics.md). 
