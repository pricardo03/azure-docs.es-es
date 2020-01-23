---
title: Cifrado de datos para Azure Database for MySQL mediante el portal
description: Obtenga información sobre cómo configurar y administrar el cifrado de datos con la clave administrada por el cliente para Azure Database for MySQL mediante Azure Portal.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 5bd100a5858660d80f3704844ed974f2bc9257b4
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028947"
---
# <a name="data-encryption-for-azure-database-for-mysql-server-using-azure-portal"></a>Cifrado de datos para un servidor de Azure Database for MySQL mediante Azure Portal

En este artículo, obtendrá información sobre cómo configurar y administrar para usar Azure Portal para configurar el cifrado de datos con la clave administrada por el cliente para Azure Database for MySQL.

## <a name="prerequisites-for-cli"></a>Requisitos previos para la CLI

* Debe tener una suscripción de Azure y ser un administrador en esa suscripción.
* Cree una instancia de Azure Key Vault y una clave para usar para TDE.
* El almacén de claves debe tener la siguiente propiedad que se usará como una clave administrada por el cliente:
  * [eliminación temporal](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -test -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Protegido contra purgas](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* La clave debe tener los siguientes atributos que se usarán para clave administrada por el cliente.
  * Sin fecha de expiración
  * No deshabilitado
  * Poder realizar las operaciones _get_, _wrap key_, _unwrap key_

## <a name="setting-the-right-permissions-for-key-operations"></a>Establecimiento de los permisos adecuados para las operaciones de clave

1. En el Azure Key Vault, seleccione las **Directivas de acceso** y después **Agregar directiva de acceso**:

   ![Introducción a la directiva de acceso](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. En **Permisos de clave** seleccione **Get**, **Wrap**, **Unwrap** y **Principal**, que es el nombre del servidor MySQL. Si no se encuentra la entidad de seguridad del servidor en la lista de entidades de seguridad existentes, deberá registrarla configurando el cifrado de datos por primera vez, lo que producirá un error.

   ![Introducción a la directiva de acceso](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. **Guarde** la configuración.

## <a name="setting-data-encryption-for-azure-database-for-mysql"></a>Configuración del cifrado de datos para Azure Database for MySQL

1. En **Azure Database for MySQL**, seleccione el **Cifrado de datos** para establecer la configuración de la clave administrada por el cliente.

   ![Configuración del cifrado de datos](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Puede seleccionar **Key Vault** y un par de **claves** o pasar un **Identificador de claves**.

   ![Configuración de Key Vault](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. **Guarde** la configuración.

4. Para asegurarse de que todos los archivos (incluidos los **temporales**) están cifrados de forma completa, **deberá** **reiniciar** el servidor.

## <a name="restoring-or-creating-replica-of-the-server-which-has-data-encryption-enabled"></a>Restauración o creación de una réplica del servidor con el cifrado de datos habilitado

Una vez que se cifra Azure Database for MySQL con la clave administrada por el cliente almacenada en Key Vault, cualquier copia recién creada del servidor, ya sea mediante una operación de restauración geográfica o local o mediante una operación de réplica (local o entre regiones). De modo que, para un servidor MySQL cifrado, puede seguir los siguientes pasos para crear un servidor restaurado cifrado.

1. En el servidor, seleccione **Información general** y, después, seleccione **Restaurar**.

   ![Iniciar-restaurar](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Para un servidor habilitado para la replicación, en el encabezado **Configuración**, seleccione **Replicación**, como se muestra aquí:

   ![Iniciar-réplica](media/concepts-data-access-and-security-data-encryption/mysql-replica.png)

2. Una vez completada la operación de restauración, el nuevo servidor creado cuenta está cifrado con la clave usada para cifrar el servidor principal. Sin embargo, las características y opciones del servidor están deshabilitadas y el servidor está marcado con el estado **Inaccesible**. Este comportamiento está diseñado para evitar la manipulación de datos, ya que aún no se ha concedido permiso a la identidad del nuevo servidor para obtener acceso a Key Vault.

   ![Marcar servidor como inaccesible](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Para corregir el estado inaccesible, debe volver a validar la clave en el servidor restaurado. Seleccione el panel **Cifrado de datos** y, a continuación, el botón **Clave para volver a validar**.

   > [!NOTE]
   > Se producirá un error en el primer intento de volver a validar porque la entidad de servicio del nuevo servidor debe tener acceso al almacén de claves. Para generar la entidad de servicio, seleccione **Volver a validar la clave**. Se producirá un error, pero se generará la entidad de servicio. A partir de ese momento, consulte los pasos [de la sección 2](#setting-the-right-permissions-for-key-operations) anterior.

   ![volver a validar el servidor](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Tendrá que conceder a Key Vault acceso al nuevo servidor.

4. Después de registrar la entidad de servicio, tendrá que volver a validar la clave y el servidor reanudará su funcionalidad normal.

   ![Servidor normal restaurado](media/concepts-data-access-and-security-data-encryption/restore-successful.png)

## <a name="next-steps"></a>Pasos siguientes

 Para obtener más información sobre el cifrado de datos, consulte [Qué es el cifrado de datos de Azure](concepts-data-encryption-mysql.md).
