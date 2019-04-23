---
title: Azure Data Box para solucionar problemas mediante la interfaz REST | Microsoft Docs
description: Describe cómo solucionar problemas que se ven en Azure Data Box se copia de datos a través de la interfaz REST.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: ee2820d78e95924e09a0219753f87d6910c0e736
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014639"
---
# <a name="troubleshoot-issues-related-to-azure-data-box-blob-storage"></a>Solucionar problemas relacionados con el cuadro de datos de Azure Blob storage

Esta información de detalles del artículo sobre cómo solucionar problemas, puede que vea cuando se usa el almacenamiento de blobs de cuadro de datos a través de la interfaz REST en el cuadro de datos para copiar los datos. Superficie de estos problemas cuando se usa almacenamiento de blobs de cuadro de datos con otras aplicaciones o bibliotecas de cliente como explorador de Azure Storage, AzCopy o Azure Storage para Python.

## <a name="errors-seen-in-azure-storage-explorer"></a>Errores detectados en el Explorador de Storage de Azure

Esta sección detallan algunos de los problemas que surgen cuando mediante el Explorador de Storage de Azure con almacenamiento de blobs de cuadro de datos.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|No se puede recuperar los recursos secundarios. El valor de uno de los encabezados HTTP no está en el formato correcto.|Desde el **editar** menú, seleccione **API de Azure Stack de destino**. <br>Reinicie el Explorador de Azure Storage.|
|`getaddrinfo ENOTFOUND <accountname>.blob.<serialnumber>.microsoftdatabox.com` |Compruebe que el nombre del extremo `<accountname>.blob.<serialnumber>.microsoftdatabox.com` se agrega al archivo de hosts en esta ruta de acceso: <li>`C:\Windows\System32\drivers\etc\hosts` en Windows, o </li><li> `/etc/hosts` en Linux.</li>|
|No se puede recuperar los recursos secundarios. <br>Detalles: certificado autofirmado |Importar el certificado SSL para el dispositivo en el Explorador de Azure Storage: <li>Descargue el certificado desde el portal de Azure. Para obtener más información, vaya a [descargar el certificado](data-box-deploy-copy-data-via-rest.md#download-certificate).</li><li>Desde el **editar** menú, seleccione **certificados SSL** y, a continuación, seleccione **importar certificados**.</li>|

## <a name="errors-seen-in-azcopy-for-windows"></a>Errores detectados en AzCopy para Windows

Esta sección detallan algunos de los problemas que surgen cuando el uso de AzCopy para Windows con almacenamiento de blobs de cuadro de datos.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|Comando de AzCopy parece que se bloquea durante un minuto antes de mostrar este error: <br>Error al enumerar directorios https://... No se pudo resolver el nombre remoto `<accountname>.blob.<serialnumber>.microsoftdatabox.com`|Compruebe que el nombre del extremo `<accountname>.blob.<serialnumber>.microsoftdatabox.com` se agrega al archivo hosts en: `C:\Windows\System32\drivers\etc\hosts`.|
|Comando de AzCopy parece que se bloquea durante un minuto antes de mostrar este error: <br>Error al analizar la ubicación de origen. Se cerró la conexión subyacente: No se pudo establecer la relación de confianza para el canal seguro SSL/TLS.|Importar el certificado SSL para el dispositivo en el almacén de certificados del sistema. Para obtener más información, vaya a [descargar el certificado](data-box-deploy-copy-data-via-rest.md#download-certificate).|


## <a name="errors-seen-in-azcopy-for-linux"></a>Errores detectados en AzCopy para Linux

Esta sección detallan algunos de los problemas que surgen cuando usa AzCopy para Linux con almacenamiento de blobs de cuadro de datos.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|Comando de AzCopy parece que se bloquea durante 20 minutos antes de mostrar este error: <br>Error al analizar la ubicación de origen `https://<accountname>.blob.<serialnumber>.microsoftdatabox.com/<cntnr>`. No existe tal dispositivo o dirección|Compruebe que el nombre del extremo `<accountname>.blob.<serialnumber>.microsoftdatabox.com` se agrega al archivo hosts en: `/etc/hosts`.|
|Comando de AzCopy parece que se bloquea durante 20 minutos antes de mostrar este error: <br>Ubicación de origen de análisis de errores... No se pudo establecer la conexión SSL.|Importar el certificado SSL para el dispositivo en el almacén de certificados del sistema. Para obtener más información, vaya a [descargar el certificado](data-box-deploy-copy-data-via-rest.md#download-certificate).|

## <a name="errors-seen-in-azure-storage-library-for-python"></a>Errores detectados en la biblioteca de Azure Storage para Python

En esta sección se detallan algunos de los principales problemas que se enfrentan durante la implementación de Data Box Disk cuando se usa un cliente Linux para la copia de datos.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|El valor de uno de los encabezados HTTP no está en el formato correcto. |No se admite la versión instalada de la biblioteca de Microsoft Azure Storage para Python en el cuadro de datos. Consulte los requisitos de almacenamiento de blobs de cuadro de datos de Azure para las versiones compatibles.|
|… [SSL: CERTIFICATE_VERIFY_FAILED]...|Antes de ejecutar Python, establezca la variable de entorno REQUESTS_CA_BUNDLE a la ruta de acceso del archivo de certificado SSL con codificación Base64 (vea cómo [descargar el certificado]()). <br>Por ejemplo: <br>`export REQUESTS_CA_BUNDLE=/tmp/mycert.cer` <br>`python` <br>O bien, agregue el certificado al almacén de certificados del sistema y, a continuación, se establece esta variable de entorno en la ruta de acceso de dicho almacén. <br> Por ejemplo, en Ubuntu: <br>`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt` <br>`python`|


## <a name="common-errors"></a>Errores comunes

Estos errores no son específicos para cualquier aplicación.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|La conexión agota el tiempo de espera. |Inicie sesión en el dispositivo Data Box y compruebe que está desbloqueada. Cada vez que se reinicie el dispositivo, permanece bloqueada hasta que alguien inicia sesión.|

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre la [los requisitos del sistema de almacenamiento Blob de datos cuadro](data-box-system-requirements-rest.md).
