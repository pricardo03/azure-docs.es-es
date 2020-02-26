---
title: 'Generación y transferencia de claves protegidas con HSM para Azure Key Vault: Azure Key Vault | Microsoft Docs'
description: Use este artículo para ayudarle a planear, generar y, a continuación, transfiera sus propias claves protegidas con HSM para utilizar con Azure Key Vault. También se denomina BYOK o "traiga su propia clave".
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 2f5269587d222be9a1628b72c1f3f0dc1b105f3c
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461750"
---
# <a name="import-hsm-protected-keys-to-key-vault-preview"></a>Importación de claves protegidas con HSM en Key Vault (versión preliminar)

> [!NOTE]
> Esta característica está en versión preliminar y solo está disponible en las regiones **Este de EE. UU. 2 EUAP** y **Centro de EE. UU. EUAP**. 

Para obtener una mayor seguridad, cuando utilice Azure Key Vault, puede importar o generar claves en módulos de seguridad de hardware (HSM) que no se salen nunca del límite de los HSM. Con frecuencia este escenario se conoce también como *Aportar tu propia clave*, o BYOK. En Azure Key Vault se usa la familia nCipher nShield de HSM (validado con FIPS 140-2 de nivel 2) para proteger las claves.

La información de este tema le ayudará a planear, generar y transferir sus propias claves protegidas con HSM para utilizarlas en Azure Key Vault.

> [!NOTE]
> Esta funcionalidad no está disponible para Azure China 21Vianet. 
> 
> Este método de importación solo está disponible para [HSM compatibles](#supported-hsms). 

Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](key-vault-overview.md)  Para ver un tutorial introductorio que incluya la creación de un almacén de claves para claves protegidas con HSM, consulte [¿Qué es Azure Key Vault?](key-vault-overview.md).

## <a name="overview"></a>Información general

* Genere una clave (a la que se hace referencia como clave de intercambio de claves o KEK) en el almacén de claves. Esta debe ser una clave RSA-HSM con "import" como única operación de clave. Solo la SKU Premium de Key Vault admite claves RSA-HSM.
* Descargue la clave pública de KEK como un archivo .pem.
* Transfiera la clave pública de KEK a la estación de trabajo sin conexión que está conectada al HSM local.
* En la estación de trabajo sin conexión, use la herramienta BYOK que proporciona su proveedor de HSM para crear un archivo BYOK. 
* La clave de destino se cifra con una clave de intercambio de claves (KEK), que permanece cifrada hasta que se transfiere a los HSM de Azure Key Vault. Solo la versión cifrada de la clave sale del HSM local.
* La clave de intercambio de claves se genera dentro de los HSM de Azure Key Vault y no es exportable. Los HSM exigen que no pueda haber una versión sin cifrar de la KEK fuera de los HSM de Key Vault.
* La KEK debe estar en el mismo almacén de claves al que se va a importar la clave de destino.
* Cuando el archivo BYOK se carga en Key Vault, los HSM de Key Vault usan la clave privada de KEK para descifrar el material de la clave de destino e importarlo como una clave de HSM. Esta operación se produce en su totalidad en los HSM de Key Vault y la clave de destino permanece siempre en el límite de la protección del HSM.

## <a name="prerequisites"></a>Prerrequisitos

En la tabla siguiente puede ver una lista de los requisitos previos del método Aportar tu propia clave (BYOK) para Azure Key Vault.

| Requisito | Más información |
| --- | --- |
| Una suscripción a Azure |Para crear una instancia de Azure Key Vault, se necesita una suscripción a Azure: [Suscríbase ahora para disfrutar de una prueba gratis](https://azure.microsoft.com/pricing/free-trial/). |
| Un almacén de claves (SKU Premium) para importar claves protegidas con HSM |Para obtener más información sobre los niveles de servicio y las funcionalidades de Azure Key Vault, consulte el sitio web [Precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) . |
| Un HSM de la lista de HSM compatibles junto con la herramienta BYOK y las instrucciones que proporciona el proveedor de HSM | Debe acceder al módulo de seguridad de hardware y al conocimiento operativo básico de los HSM. Consulte los [HSM compatibles](#supported-hsms). |
| La CLI de Azure, versión 2.1.0 u otra posterior | Para más información, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).|

## <a name="supported-hsms"></a>HSM compatibles

|Nombre del proveedor de HSM|Modelos de HSM compatibles|Detalles adicionales|
|---|---|---|
|Thales|Familia SafeNet Luna HSM 7 con la versión del firmware 7.3 o posterior| [Herramienta BYOK y documentación de SafeNet Luna](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|


> [!NOTE]
> Para importar claves protegidas con HSM de la familia nCipher nShield de HSM [use el procedimiento con BYOK heredado](hsm-protected-keys-legacy.md).


## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Generación y transferencia de una clave a un HSM de Azure Key Vault

Los pasos siguientes se utilizan para generar y transferir una clave a un HSM de Azure Key Vault:

* [Paso 1: Generación de una KEK](#step-1-generate-a-kek)
* [Paso 2: Descarga de la clave pública de KEK](#step-2-download-kek-public-key)
* [Paso 3: Generación y preparación de la clave para la transferencia](#step-3-generate-and-prepare-your-key-for-transfer)
* [Paso 4: Transferencia de la clave a Azure Key Vault](#step-4-transfer-your-key-to-azure-key-vault)

### <a name="step-1-generate-a-kek"></a>Paso 1: Generación de una KEK

La KEK (clave de intercambios de clave) es una clave RSA que se genera en el HSM de Key Vault. Esta clave se utiliza para cifrar la clave que se va a importar (clave de destino).

La KEK debe:
1. Ser una clave **RSA-HSM** (de 2048, 3072 o 4096 bits)
2. Generarse en el mismo almacén de claves al que desea importar la clave de destino
3. Crearse con las operaciones de clave permitidas establecidas para **importar**

Use el comando [az keyvault key create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) para crear KEK con las operaciones de clave establecidas para importar. Anote el identificador de clave, "kid", que devuelve el siguiente comando. Lo necesitará en el [paso 3](#step-3-generate-and-prepare-your-key-for-transfer).


```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

### <a name="step-2-download-kek-public-key"></a>Paso 2: Descarga de la clave pública de KEK

Use el comando [az keyvault key download](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download) para descargar la clave pública de KEK en un archivo .pem. La clave de destino que importa se cifra mediante la clave pública de KEK.

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

Transfiera el archivo KEKforBYOK.publickey.pem a la estación de trabajo sin conexión. Necesitará este archivo en el siguiente paso.

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>Paso 3: Generación y preparación de la clave para la transferencia

Consulte la documentación del proveedor del HSM para descargar e instalar la herramienta BYOK. Siga las instrucciones del proveedor de HSM para generar una clave de destino y crear, a continuación, un paquete de transferencia de claves (un archivo BYOK). La herramienta BYOK utilizará el identificador de clave del [paso 1](#step-1-generate-a-kek) y el archivo KEKforBYOK.publickey.pem que descargó en el [paso 2](#step-2-download-kek-public-key) para generar una clave de destino cifrada en un archivo BYOK.

Transfiera el archivo BYOK a la estación de trabajo conectada.

> [!NOTE] 
> La clave de destino debe ser una clave RSA con un tamaño de 2048, 3072 o 4096 bits. En este momento no se admite la importación de claves de Elliptic Curve.
> <br/><strong>Problema conocido:</strong> Se produce un error al importar la clave de destino RSA 4K desde HSM de SafeNet Luna. Cuando se resuelva el problema, se actualizará el documento.

### <a name="step-4-transfer-your-key-to-azure-key-vault"></a>Paso 4: Transferencia de la clave a Azure Key Vault

En este último paso, transfiera el paquete de transferencia de claves (un archivo BYOK) desde la estación de trabajo desconectada a la estación de trabajo conectada a Internet y, a continuación, use el comando [az keyvault key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) para cargar el archivo BYOK en el HSM de Azure Key Vault para completar la importación de la clave.

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

Si la carga se realiza correctamente, verá que se muestran las propiedades de la clave que acaba de importar.

## <a name="next-steps"></a>Pasos siguientes

Ahora puede usar esta clave protegida con HSM en el almacén de claves. Para más información, vea esta [comparación](https://azure.microsoft.com/pricing/details/key-vault/) de precios y características.
