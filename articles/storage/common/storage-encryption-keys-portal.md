---
title: Configuración de las claves administradas del cliente para el cifrado de Azure Storage desde Azure Portal
description: Obtenga información sobre cómo usar Azure Portal para configurar las claves administradas del cliente para el cifrado de Azure Storage. Las claves administradas del cliente le permiten crear, girar, deshabilitar y revocar los controles de acceso.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/15/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 74ca6f15baeeb9fe8adad4bda80e313a4b4cf03a
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376238"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-the-azure-portal"></a>Configuración de las claves administradas del cliente para el cifrado de Azure Storage desde Azure Portal

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

En este artículo se muestra cómo configurar un almacén de claves con claves administradas del cliente mediante [Azure Portal](https://portal.azure.com/). Para obtener información sobre cómo crear un almacén de claves mediante Azure Portal, consulte [Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante Azure Portal](../../key-vault/quick-create-portal.md). 

> [!IMPORTANT]
> El uso de claves administradas del cliente con el cifrado de Azure Storage requiere el establecimiento de dos propiedades en el almacén de claves, **Eliminación temporal** y **Do Not Purge** (No purgar). Estas propiedades no están habilitadas de forma predeterminada. Para habilitarlas, use PowerShell o la CLI de Azure.
> Solo se admiten claves RSA y el tamaño de clave 2048.

## <a name="enable-customer-managed-keys"></a>Habilitar claves administradas del cliente

Para habilitar claves administradas del cliente en Azure Portal, siga estos pasos:

1. Vaya a la cuenta de almacenamiento.
1. En la hoja **Configuración** de la cuenta de almacenamiento, haga clic en **Cifrado**. Seleccione la opción **Use su propia clave**, tal como se muestra en la imagen siguiente.

    ![Captura de pantalla del portal que muestra la opción de cifrado](./media/storage-encryption-keys-portal/ssecmk1.png)

## <a name="specify-a-key"></a>Especificar una clave

Después de habilitar las claves administradas del cliente, tendrá la oportunidad de especificar una clave para asociarla con la cuenta de almacenamiento.

### <a name="specify-a-key-as-a-uri"></a>Especificación de una clave como URI

Para especificar una clave como URI, siga estos pasos:

1. Para buscar el URI de la clave en Azure Portal, vaya al almacén de claves y seleccione la opción de configuración **Claves**. Seleccione la clave que desee y luego haga clic en ella para ver su configuración. Copie el valor del campo **Identificador de clave**, que proporciona el URI.

    ![Captura de pantalla en que se muestra el URI de la clave del almacén de claves](media/storage-encryption-keys-portal/key-uri-portal.png)

1. En las opciones de configuración de **cifrado** de la cuenta de almacenamiento, elija la opción **Introducir URI de la clave**.
1. En el campo **Key URI** (URI de la clave), especifique el URI.

   ![Captura de pantalla en que se muestra cómo introducir el URI de la clave](./media/storage-encryption-keys-portal/ssecmk2.png)

### <a name="specify-a-key-from-a-key-vault"></a>Especificación de una clave a partir de un almacén de claves

Para especificar una clave a partir de un almacén de claves, asegúrese de tener un almacén de claves que contenga una clave. Para especificar una clave a partir de un almacén de claves, siga estos pasos:

1. Elija la opción **Select from Key Vault** (Seleccionar desde almacén de claves).
2. Elija el almacén de claves que contiene la clave que desea usar.
3. Elija la clave en el almacén de claves.

   ![Captura de pantalla en que se muestra la opción de clave administrada del cliente](./media/storage-encryption-keys-portal/ssecmk3.png)

## <a name="update-the-key-version"></a>Actualizar la versión de la clave

Al crear una nueva versión de una clave, tendrá que actualizar la cuenta de almacenamiento para que utilice la versión nueva. Siga estos pasos:

1. Vaya a la cuenta de almacenamiento y muestre las opciones de configuración de **cifrado**.
1. Especifique el URI para la nueva versión de la clave. Como alternativa, puede volver a seleccionar el almacén de claves y la clave para actualizar la versión.

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](storage-service-encryption.md)
- [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
