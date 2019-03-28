---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows
author: rothja
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 01020a23b102c896bbeb3d8cf455afabfc164917
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505650"
---
## <a name="prepare-for-akv-integration"></a>Preparación para la integración de AKV
Para usar la Integración de Azure Key Vault para configurar la máquina virtual de SQL Server, hay varios requisitos previos: 

1. [Instalar Azure Powershell](#install)
2. [Crear un directorio de Azure Active Directory](#register)
3. [Creación de un Almacén de claves](#createkeyvault)

Las secciones siguientes describen estos requisitos previos y la información que necesita recopilar para ejecutar más adelante los cmdlets de PowerShell.

[!INCLUDE [updated-for-az](./updated-for-az.md)]

### <a id="install"></a> Instalar Azure PowerShell
Asegúrese de que ha instalado el SDK más reciente de Azure PowerShell. Para obtener más información, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/install-az-ps).

### <a id="register"></a> Registrar una aplicación en Azure Active Directory

En primer lugar, tiene que tener un [directorio de Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) en la suscripción. Entre otras muchas ventajas, esto le permite conceder permiso para Almacén de claves a determinados usuarios y aplicaciones.

A continuación, registre una aplicación con AAD. Esto le dará una cuenta de entidad de servicio que tenga acceso a su almacén de claves, algo que la máquina virtual va a necesitar. En el artículo de Azure Key Vault, puede encontrar estos pasos en el [registrar una aplicación con Azure Active Directory](../articles/key-vault/key-vault-manage-with-cli2.md#registering-an-application-with-azure-active-directory) sección, o bien puede ver los pasos con capturas de pantalla de la **obtener una identidad para la sección de la aplicación**  de [esta entrada de blog](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Antes de completar estos pasos, durante este registro tiene que recopilar la siguiente información que necesitará más adelante cuando habilite la integración de Azure Key Vault en la máquina virtual de SQL.

* Después de agregar la aplicación, busque el **Id. de la aplicación** en la hoja **Aplicación registrada**.
    El identificador de la aplicación se asigna posteriormente al parámetro **$spName** (nombre de entidad de seguridad de servicio) en el script de PowerShell para habilitar la integración de Azure Key Vault.

   ![Identificador de aplicación](./media/virtual-machines-sql-server-akv-prepare/aad-application-id.png)

* Durante estos pasos al crear la clave, copie el secreto de la clave tal y como se muestra en la siguiente captura de pantalla. Este secreto de clave se asigna posteriormente al parámetro **$spSecret** (secreto de entidad de servicio) en el script de PowerShell.

   ![Secreto de AAD](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)

* El identificador de la aplicación y el secreto también se utilizarán para crear una credencial en SQL Server.

* Tiene que autorizar este nuevo identificador de cliente para que tenga los siguientes permisos de acceso: **get**, **wrapKey**, **unwrapKey**. Esto se hace con el cmdlet [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy). Para obtener más información, consulte la [información general de Azure Key Vault](../articles/key-vault/key-vault-overview.md).

### <a id="createkeyvault"></a> Crear un almacén de claves
Para poder usar Azure Key Vault para guardar las claves que se utilizarán para el cifrado en la máquina virtual, tiene que tener acceso a un almacén de claves. Si no ha configurado ya su almacén de claves, cree uno siguiendo los pasos que se mencionan en el artículo [Introducción a Azure Key Vault](../articles/key-vault/key-vault-overview.md). Antes de completar estos pasos, durante esta configuración tiene que recopilar cierta información que necesitará más adelante cuando habilite la integración de Azure Key Vault en la máquina virtual de SQL.

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Cuando llegue al paso Creación de un Almacén de claves, tenga en cuenta la propiedad **vaultUri** devuelta, que es la dirección URL del Almacén de claves. En el ejemplo que se proporciona en ese paso, que se muestra a continuación, el nombre del almacén de claves es ContosoKeyVault, por lo que su dirección URL sería https://contosokeyvault.vault.azure.net/.

La URL de Key Vault se asigna posteriormente al parámetro **$akvURL** en el script de PowerShell para habilitar la integración de Azure Key Vault.

Una vez creado el almacén de claves, se debe agregar una clave para dicho almacén; esta clave se utilizará más adelante cuando se cree una clave asimétrica en SQL Server.
