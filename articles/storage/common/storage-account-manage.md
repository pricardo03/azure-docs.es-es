---
title: 'Administración de la configuración de cuentas de almacenamiento en Azure Portal: Azure Storage | Microsoft Docs'
description: Aprenda a administrar la configuración de cuentas de almacenamiento en Azure Portal, como la configuración del control de acceso, la regeneración de claves de acceso de cuenta, el cambio del nivel de acceso o la modificación del tipo de replicación que usa la cuenta. Aprenda también a eliminar una cuenta de almacenamiento en el portal.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: tamram
ms.openlocfilehash: 3c01cc870b20c8256b215eb700548e6cd69ad0d5
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748988"
---
# <a name="manage-storage-account-settings-in-the-azure-portal"></a>Administración de la configuración de cuentas de almacenamiento en Azure Portal

Existen diversas opciones de configuración de la cuenta de almacenamiento en [Azure Portal](https://portal.azure.com). En este artículo se describen algunas de estas opciones y cómo usarlas.

## <a name="access-control"></a>Control de acceso

Azure Storage admite la autorización con Azure Active Directory para Blob Storage y Queue Storage mediante el control de acceso basado en rol (RBAC). Para más información sobre la autenticación con Azure AD, consulte [Autorización del acceso a los blobs y las colas de Azure con Azure Active Directory](storage-auth-aad.md).

La configuración del **control de acceso** en Azure Portal ofrece una manera sencilla de asignar roles de RBAC a usuarios, grupos, entidades de servicio e identidades administradas. Para más información sobre la asignación de roles de RBAC, consulte [Manage access rights to blob and queue data with RBAC](storage-auth-aad-rbac.md) (Administración de derechos de acceso a los datos de blobs y colas con RBAC).

## <a name="tags"></a>Etiquetas

Azure Storage admite etiquetas de Azure Resource Manager para organizar los recursos de Azure con una taxonomía personalizada. Puede aplicar etiquetas a las cuentas de almacenamiento para agruparlas dentro de su suscripción de una manera lógica.

En las cuentas de almacenamiento, el nombre de etiqueta se limita a 128 caracteres y el valor de la etiqueta a 256.

Para más información, consulte [Uso de etiquetas para organizar los recursos de Azure](../../azure-resource-manager/resource-group-using-tags.md).

## <a name="access-keys"></a>Claves de acceso

Cuando se crea una cuenta de almacenamiento, Azure genera dos claves de acceso de cuenta de almacenamiento de 512 bits. Estas claves se pueden usar para autorizar el acceso a su cuenta de almacenamiento mediante una clave compartida. Las claves se pueden rotar y regenerar sin interrupción de sus aplicaciones, y Microsoft recomienda que lo haga con regularidad.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

[!INCLUDE [storage-recommend-azure-ad-include](../../../includes/storage-recommend-azure-ad-include.md)]

### <a name="view-account-keys-and-connection-string"></a>Visualización de claves de cuenta y cadena de conexión

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

### <a name="regenerate-access-keys"></a>Regenerar las claves de acceso

Microsoft recomienda volver a generar las claves de acceso periódicamente para ayudar a proteger la cuenta de almacenamiento. Para poder rotar las claves, se asignan dos claves de acceso. Al rotar las claves, garantiza que su aplicación mantiene el acceso a Azure Storage a lo largo del proceso. 

> [!WARNING]
> La regeneración de las claves de acceso puede afectar a aplicaciones o servicios de Azure que dependen de la clave de cuenta de almacenamiento. Los clientes que usan la clave de cuenta para acceder a la cuenta de almacenamiento se deben actualizar para usar la nueva clave, lo que incluye los servicios multimedia, las aplicaciones de nube, escritorio y móviles y las aplicaciones de interfaz gráfica de usuario de Azure Storage, como el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

Para rotar las claves de la cuenta de almacenamiento, siga este proceso:

1. Actualice las cadenas de conexión en el código de aplicación para usar la clave secundaria.
2. Vuelva a generar la clave de acceso primaria para su cuenta de almacenamiento. En la hoja **Claves de acceso** de Azure Portal, haga clic en **Regenerar clave1** y luego haga clic en **Sí** para confirmar que quiere generar una nueva clave.
3. Actualice las cadenas de conexión en su código para hacer referencia a la nueva clave de acceso primaria.
4. Vuelva a generar la clave de acceso secundaria de la misma forma.

## <a name="account-configuration"></a>Configuración de la cuenta

Después de crear una cuenta de almacenamiento, puede modificar su configuración. Por ejemplo, puede cambiar cómo se replican los datos o cambiar el nivel de acceso de la cuenta de frecuente a esporádico. En [Azure Portal](https://portal.azure.com), vaya a su cuenta de almacenamiento y, luego, busque y haga clic en **Configuración** en **Configuración** para ver o cambiar la configuración de la cuenta.

Cambiar la configuración de la cuenta de almacenamiento puede dar lugar a costos adicionales. Para más información, consulte la página [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="delete-a-storage-account"></a>Eliminar una cuenta de almacenamiento

Para quitar una cuenta de almacenamiento que ha dejado de usarse, navegue a la cuenta de almacenamiento en el [Portal de Azure](https://portal.azure.com)y haga clic en **Eliminar**. Si se elimina la cuenta de almacenamiento, se elimina toda la cuenta, incluidos todos los datos de la cuenta.

> [!WARNING]
> No es posible restaurar una cuenta de almacenamiento eliminada ni recuperar el contenido que contenía antes de la eliminación. Asegúrese de hacer una copia de seguridad de cualquier contenido que desee guardar antes de eliminar la cuenta. Esto también es verdad para los recursos de la cuenta: cuando se elimina un blob, tabla, cola o archivo, este se eliminará definitivamente.
> 

Si intenta eliminar una cuenta de almacenamiento asociada a una máquina virtual de Azure, puede obtener un error sobre que la cuenta de almacenamiento aún está usándose. Para obtener ayuda para solucionar este error, consulte [Solución de errores al eliminar las cuentas de almacenamiento](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a las cuentas de Azure Storage](storage-account-overview.md)
- [Cree una cuenta de almacenamiento](storage-quickstart-create-account.md)
