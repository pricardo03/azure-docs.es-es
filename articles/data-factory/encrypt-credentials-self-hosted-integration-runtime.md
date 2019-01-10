---
title: Cifrado de credenciales en Azure Data Factory | Microsoft Docs
description: Aprenda a cifrar y almacenar credenciales para sus almacenes de datos locales en una máquina con Integration Runtime autohospedado.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 8e8a4cabd948783278981c61fa718e51b679ad72
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014172"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Cifrado de credenciales de almacenes de datos locales en Azure Data Factory
Puede cifrar y almacenar las credenciales de los almacenes de datos locales (servicios vinculados con información confidencial) en una máquina con Integration Runtime autohospedado. 

Se pasa un archivo de definición JSON con credenciales al cmdlet <br/>[**New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential**](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0) para generar un archivo de definición JSON de salida con las credenciales cifradas. A continuación, se usa la definición de JSON actualizada para crear los servicios vinculados.

## <a name="author-sql-server-linked-service"></a>Creación del servicio vinculado de SQL Server
Cree un archivo JSON denominado **SqlServerLinkedService.json** en cualquier carpeta con el siguiente contenido:  

Reemplace `<servername>`, `<databasename>`, `<username>` y `<password>` por los valores de SQL Server antes de guardar el archivo. Y, reemplace `<integration runtime name>` por el nombre de su instancia de Integration Runtime. 

```json
{
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
            }
        },
        "connectVia": {
            "type": "integrationRuntimeReference",
            "referenceName": "<integration runtime name>"
        },
        "name": "SqlServerLinkedService"
    }
}
```

## <a name="encrypt-credentials"></a>Cifrado de las credenciales
Para cifrar los datos confidenciales de la carga de JSON en una instancia de Integration Runtime autohospedado, ejecute **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** y pase dicha carga. Este cmdlet garantiza que las credenciales se cifran utilizando DPAPI y se almacenan en el nodo de Integration Runtime autohospedado de manera local. La carga de salida se puede redirigir a otro archivo JSON (en este caso, "encryptedLinkedService.json"), que contiene las credenciales cifradas.

```powershell
New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>Uso de JSON con credenciales cifradas
Ahora, use el archivo JSON de salida del comando anterior que contiene la credencial cifrada para configurar **SqlServerLinkedService**.

```powershell
Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las consideraciones de seguridad en el movimiento de datos, consulte [Consideraciones sobre la seguridad del movimiento de datos](data-movement-security-considerations.md).

