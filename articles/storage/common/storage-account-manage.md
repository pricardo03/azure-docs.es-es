---
title: 'Administración de la configuración de cuentas de almacenamiento en Azure Portal: Azure Storage | Microsoft Docs'
description: Aprenda a administrar la configuración de cuentas de almacenamiento en Azure Portal, como la configuración del control de acceso, la regeneración de claves de acceso de cuenta, el cambio del nivel de acceso o la modificación del tipo de replicación que usa la cuenta. Aprenda también a eliminar una cuenta de almacenamiento en el portal.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/11/2018
ms.author: tamram
ms.openlocfilehash: d3f6009c77618bc043586771a6f67442533b2a0d
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57444062"
---
# <a name="manage-storage-account-settings-in-the-azure-portal"></a>Administración de la configuración de cuentas de almacenamiento en Azure Portal

Existen diversas opciones de configuración de la cuenta de almacenamiento en [Azure Portal](https://portal.azure.com). En este artículo se describen algunas de estas opciones y cómo usarlas.

## <a name="access-control"></a>Control de acceso

Azure Storage admite la autenticación con Azure Active Directory para Blob Storage y Queue Storage (versión preliminar) mediante el control de acceso basado en rol (RBAC). Para más información sobre la autenticación con Azure AD, consulte [Autenticación del acceso a Azure Storage con Azure Active Directory (versión preliminar)](storage-auth-aad.md).

La configuración del **control de acceso** en Azure Portal ofrece una manera sencilla de asignar roles de RBAC a usuarios, grupos, entidades de servicio e identidades administradas. Para más información sobre la asignación de roles de RBAC, consulte [Manage access rights to blob and queue data with RBAC (Preview)](storage-auth-aad-rbac.md) (Administración de derechos de acceso a los datos de blobs y colas con RBAC [versión preliminar]).

> [!NOTE]
> Si autentica a los usuarios o a las aplicaciones que usan las credenciales de Azure AD, mejorará la seguridad y le será más fácil usar esta opción en lugar de otros medios de autorización. Aunque puede seguir utilizando la autorización con clave compartida con las aplicaciones, el uso de Azure AD evita la necesidad de almacenar su clave de acceso de cuenta con el código. Asimismo, aún puede usar firmas de acceso compartido (SAS) para conceder acceso pormenorizado a los recursos de su cuenta de almacenamiento, pero Azure AD ofrece funcionalidades similares sin necesidad de administrar tokens de SAS ni de preocuparse sobre cómo revocar una SAS en peligro. 

## <a name="tags"></a>Etiquetas

Azure Storage admite etiquetas de Azure Resource Manager para organizar los recursos de Azure con una taxonomía personalizada. Puede aplicar etiquetas a las cuentas de almacenamiento para agruparlas dentro de su suscripción de una manera lógica. 

En las cuentas de almacenamiento, el nombre de etiqueta se limita a 128 caracteres y el valor de la etiqueta a 256.

Para más información, consulte [Uso de etiquetas para organizar los recursos de Azure](../../azure-resource-manager/resource-group-using-tags.md).

## <a name="access-keys"></a>Claves de acceso

Cuando se crea una cuenta de almacenamiento, Azure genera dos claves de acceso de cuenta de almacenamiento de 512 bits. Estas claves se pueden usar para autorizar el acceso a su cuenta de almacenamiento mediante una clave compartida. Las claves se pueden rotar y regenerar sin interrupción de sus aplicaciones, y Microsoft recomienda que lo haga con regularidad.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="view-and-copy-access-keys"></a>Visualización y copia de las claves de acceso

Para ver las credenciales de la cuenta de almacenamiento:

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Busque su cuenta de almacenamiento.
3. En la sección **Configuración** de la información general de la cuenta de almacenamiento, seleccione **Claves de acceso**. Aparecen las claves de acceso de la cuenta, así como la cadena de conexión completa de cada clave.
4. Busque el valor de **Clave** en **key1** y haga clic en el botón **Copiar** para copiar la clave de la cuenta.
5. Como alternativa, puede copiar la cadena de conexión completa. Busque el valor de **Cadena de conexión** en **key1**y haga clic en el botón **Copiar** para copiar la cadena de conexión.

    ![Captura de pantalla muestra cómo ver las claves de acceso en el portal de Azure](media/storage-manage-account/portal-connection-string.png)

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
