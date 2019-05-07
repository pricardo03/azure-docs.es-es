---
title: Configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure desde el portal de Azure
description: Obtenga información sobre cómo usar el portal de Azure para configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure. Claves administradas por el cliente permiten crear, girar, deshabilitar y revocar el acceso a los controles.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: baabc5a8e1d063cb51a3edea3a7218591e85aa1a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154163"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-the-azure-portal"></a>Configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure desde el portal de Azure

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

En este artículo se muestra cómo configurar un almacén de claves con claves administradas por el cliente mediante el [portal Azure](https://portal.azure.com/). Para obtener información sobre cómo crear un almacén de claves mediante Azure portal, consulte [inicio rápido: Establecer y recuperar un secreto de Azure Key Vault mediante el portal de Azure](../../key-vault/quick-create-portal.md). 


> [!IMPORTANT]
> Usar claves administradas por el cliente con cifrado de almacenamiento de Azure requiere que el almacén de claves tienen dos propiedades obligatorias configuradas, **eliminación temporal** y **no purgar**. Estas propiedades están habilitadas de forma predeterminada cuando se crea un nuevo almacén de claves en el portal de Azure. Sin embargo, si tiene que habilitar estas propiedades en un almacén de claves existente, debe usar PowerShell o CLI de Azure.

## <a name="enable-customer-managed-keys"></a>Habilitar claves administradas por el cliente

Para habilitar claves administradas por el cliente en el portal de Azure, siga estos pasos:

1. Vaya a la cuenta de almacenamiento.
1. En la hoja **Configuración** de la cuenta de almacenamiento, haga clic en **Cifrado**. Seleccione la opción **Use su propia clave**, tal como se muestra en la imagen siguiente.

    ![Opción de cifrado de portal de la captura de pantalla que muestra](./media/storage-encryption-keys-portal/ssecmk1.png)

## <a name="specify-a-key"></a>Especifique una clave

Después de habilitar claves administradas por el cliente, tendrá la oportunidad de especificar una clave para asociar la cuenta de almacenamiento.

### <a name="specify-a-key-as-a-uri"></a>Especificación de una clave como URI

Para especificar una clave como un URI, siga estos pasos:

1. Para buscar el URI de la clave en Azure portal, vaya al almacén de claves y seleccione el **claves** configuración. Seleccione la clave que desee y luego haga clic en la clave para ver su configuración. Copie el valor de la **identificador de clave** campo, que proporciona el URI.

    ![URI de clave de almacén de claves de captura de pantalla que muestra](media/storage-encryption-keys-portal/key-uri-portal.png)

1. En el **cifrado** configuración de su cuenta de almacenamiento, elija el **tecla ENTRAR URI** opción.
1. En el campo **Key URI** (URI de la clave), especifique el URI.

   ![Captura de pantalla que muestra cómo escribir el URI de la clave](./media/storage-encryption-keys-portal/ssecmk2.png)

### <a name="specify-a-key-from-a-key-vault"></a>Especificación de una clave a partir de un almacén de claves

Para especificar una clave de un almacén de claves, asegúrese de que tiene un almacén de claves que contiene una clave. Para especificar una clave de un almacén de claves, siga estos pasos:

1. Elija la opción **Select from Key Vault** (Seleccionar desde almacén de claves).
2. Elija el almacén de claves que contiene la clave que desea usar.
3. Elija la clave en el almacén de claves.

   ![Captura de pantalla muestra la opción de clave administrada por el cliente](./media/storage-encryption-keys-portal/ssecmk3.png)

## <a name="update-the-key-version"></a>Actualizar la versión de la clave

Cuando se crea una nueva versión de una clave, deberá actualizar la cuenta de almacenamiento para usar la nueva versión. Siga estos pasos:

1. Vaya a la cuenta de almacenamiento y mostrar la **cifrado** configuración.
1. Especifique el URI para la nueva versión. Como alternativa, puede seleccionar el almacén de claves y la clave de nuevo para actualizar la versión.

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](storage-service-encryption.md)
- [¿Qué es Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
