---
title: Cifrado de datos para un servidor único de Azure Database for PostgreSQL mediante Azure Portal
description: Obtenga información sobre cómo configurar y administrar el cifrado de datos para el servidor único de Azure Database for PostgreSQL mediante Azure Portal.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 6028f5e618b4b480a2259241fc2380f0200cebc6
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2020
ms.locfileid: "78898361"
---
# <a name="data-encryption-for-azure-database-for-postgresql-single-server-by-using-the-azure-portal"></a>Cifrado de datos para un servidor único de Azure Database for PostgreSQL mediante Azure Portal

Aprenda a usar Azure Portal para configurar y administrar el cifrado de datos para el servidor único de Azure Database for PostgreSQL.

## <a name="prerequisites-for-azure-cli"></a>Requisitos previos de la CLI de Azure

* Debe tener una suscripción de Azure y ser un administrador en esa suscripción.
* En Azure Key Vault, cree un almacén de claves y una clave que se usará como clave administrada por el cliente.
* Para ello, el almacén de claves debe tener las siguientes propiedades:
  * [Eliminación temporal](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -test -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Protegido contra purgas](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* La clave debe tener los siguientes atributos para que se pueda usar como clave administrada por el cliente:
  * Sin fecha de expiración
  * No deshabilitado
  * Capaz de realizar las operaciones get, wrap key y unwrap key

## <a name="set-the-right-permissions-for-key-operations"></a>Establecer los permisos adecuados para las operaciones de clave

1. En Key Vault, seleccione **Directivas de acceso** > **Agregar directiva de acceso**.

   ![Captura de pantalla de Key Vault, con las opciones Directivas de acceso y Agregar directiva de acceso resaltadas](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. En **Permisos de clave**, seleccione **Get**, **Wrap**, **Unwrap** y **Principal**, que es el nombre del servidor PostgreSQL. Si no se encuentra la entidad de seguridad del servidor en la lista de entidades de seguridad existentes, debe registrarla. Se le pedirá que registre la entidad de seguridad del servidor cuando intente configurar el cifrado de datos por primera vez y se produzca un error.  

   ![Introducción a la directiva de acceso](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. Seleccione **Guardar**.

## <a name="set-data-encryption-for-azure-database-for-postgresql-single-server"></a>Configuración del cifrado de datos para un único servidor de Azure Database for PostgreSQL

1. En Azure Database for PostgreSQL, seleccione **Cifrado de datos** para configurar la clave administrada por el cliente.

   ![Captura de pantalla de Azure Database for PostgreSQL, con Cifrado de datos resaltado](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Puede seleccionar un almacén de claves y un par de claves o escribir un identificador de clave.

   ![Captura de pantalla de Azure Database for PostgreSQL, con las opciones de cifrado de datos resaltadas](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. Seleccione **Guardar**.

4. Para asegurarse de que todos los archivos (incluidos los archivos temporales) están totalmente cifrados, reinicie el servidor.

## <a name="restore-or-create-a-replica-of-the-server"></a>Restauración o creación de una réplica del servidor

Después de cifrar un servidor único de Azure Database for PostgreSQL con la clave administrada de un cliente almacenada en Key Vault, también se cifra cualquier copia recién creada del servidor. Puede crear esta nueva copia mediante una operación de restauración local o geográfica, o por medio de una operación de réplica (local o entre regiones). De modo que, con un servidor PostgreSQL cifrado, puede usar los siguientes pasos para crear un servidor restaurado cifrado.

1. En el servidor, seleccione **Información general** > **Restaurar**.

   ![Captura de pantalla de Azure Database for PostgreSQL, las opciones información general y Restaurar resaltadas](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   O bien, en el caso de un servidor habilitado para la replicación, en el encabezado **Configuración**, seleccione **Replicación**.

   ![Captura de pantalla de Azure Database for PostgreSQL, con la opción Replicación resaltada](media/concepts-data-access-and-security-data-encryption/postgresql-replica.png)

2. Una vez completada la operación de restauración, el nuevo servidor creado se cifra con la clave del servidor principal. Sin embargo, las características y opciones del servidor están deshabilitadas y el servidor está inaccesible. Esto evita la manipulación de los datos, ya que la identidad del nuevo servidor todavía no tiene permiso para acceder al almacén de claves.

   ![Captura de pantalla de Azure Database for PostgreSQL, con estado Inaccesible resaltado](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Para que el servidor sea accesible, vuelva a validar la clave en el servidor restaurado. Seleccione **Cifrado de datos** > **Revalidate key** (Volver a validar la clave).

   > [!NOTE]
   > Se producirá un error en el primer intento de volver a realizar la validación, ya que la entidad de servicio debe tener acceso al almacén de claves. Para generar la entidad de servicio, seleccione **Revalidate key** (Volver a validar la clave). Se muestra un error, pero se genera la entidad de servicio. A partir de ese momento, consulte [estos pasos](#set-the-right-permissions-for-key-operations) anteriormente en este artículo.

   ![Captura de pantalla de Azure Database for PostgreSQL, con el paso de nueva validación resaltado](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Tendrá que conceder al almacén de claves acceso al nuevo servidor.

4. Después de registrar la entidad de servicio, vuelva a validar la clave otra vez; el servidor reanudará su funcionalidad normal.

   ![Captura de pantalla de Azure Database for PostgreSQL, que muestra la funcionalidad restaurada](media/concepts-data-access-and-security-data-encryption/restore-successful.png)

## <a name="using-an-azure-resource-manager-template-to-enable-data-encryption"></a>Uso de una plantilla de Azure Resource Manager para habilitar el cifrado de datos

Además de en Azure Portal, también puede habilitar el cifrado de datos en el servidor único de Azure Database for PostgreSQL mediante plantillas de Azure Resource Manager para los servidores nuevos o ya existentes.

### <a name="for-a-new-server"></a>Para un nuevo servidor

Use una de las plantillas de Azure Resource Manager creadas anteriormente para aprovisionar el servidor con el cifrado de datos habilitado: [Ejemplo con cifrado de datos](https://github.com/Azure/azure-postgresql/tree/master/arm-templates/ExampleWithDataEncryption)

Esta plantilla de Azure Resource Manager crea un servidor único de Azure Database for PostgreSQL y utiliza **KeyVault** y **Key** como parámetros para habilitar el cifrado de datos en el servidor.

### <a name="for-an-existing-server"></a>Para un servidor existente
Además, puede usar plantillas de Azure Resource Manager para habilitar el cifrado de datos en los servidores únicos existentes de Azure Database for PostgreSQL.

* Pase el URI de la clave de Azure Key Vault que copió anteriormente en la propiedad `keyVaultKeyUri` en el objeto properties.

* Use *2020-01-01-preview* como la versión de API.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string"
    },
    "serverName": {
      "type": "string"
    },
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Key vault name where the key to use is stored"
      }
    },
    "keyVaultResourceGroupName": {
      "type": "string",
      "metadata": {
        "description": "Key vault resource group name where it is stored"
      }
    },
    "keyName": {
      "type": "string",
      "metadata": {
        "description": "Key name in the key vault to use as encryption protector"
      }
    },
    "keyVersion": {
      "type": "string",
      "metadata": {
        "description": "Version of the key in the key vault to use as encryption protector"
      }
    }
  },
  "variables": {
    "serverKeyName": "[concat(parameters('keyVaultName'), '_', parameters('keyName'), '_', parameters('keyVersion'))]"
  },
  "resources": [
    {
      "type": "Microsoft.DBforPostgreSQL/servers",
      "apiVersion": "2017-12-01",
      "kind": "",
      "location": "[parameters('location')]",
      "identity": {
        "type": "SystemAssigned"
      },
      "name": "[parameters('serverName')]",
      "properties": {
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-05-01",
      "name": "addAccessPolicy",
      "resourceGroup": "[parameters('keyVaultResourceGroupName')]",
      "dependsOn": [
        "[resourceId('Microsoft.DBforPostgreSQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.KeyVault/vaults/accessPolicies",
              "name": "[concat(parameters('keyVaultName'), '/add')]",
              "apiVersion": "2018-02-14-preview",
              "properties": {
                "accessPolicies": [
                  {
                    "tenantId": "[subscription().tenantId]",
                    "objectId": "[reference(resourceId('Microsoft.DBforPostgreSQL/servers/', parameters('serverName')), '2017-12-01', 'Full').identity.principalId]",
                    "permissions": {
                      "keys": [
                        "get",
                        "wrapKey",
                        "unwrapKey"
                      ]
                    }
                  }
                ]
              }
            }
          ]
        }
      }
    },
    {
      "name": "[concat(parameters('serverName'), '/', variables('serverKeyName'))]",
      "type": "Microsoft.DBforPostgreSQL/servers/keys",
      "apiVersion": "2020-01-01-preview",
      "dependsOn": [
        "addAccessPolicy",
        "[resourceId('Microsoft.DBforPostgreSQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "serverKeyType": "AzureKeyVault",
        "uri": "[concat(reference(resourceId(parameters('keyVaultResourceGroupName'), 'Microsoft.KeyVault/vaults/', parameters('keyVaultName')), '2018-02-14-preview', 'Full').properties.vaultUri, 'keys/', parameters('keyName'), '/', parameters('keyVersion'))]"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

 Para más información sobre el cifrado de datos, consulte [Cifrado de datos de un solo servidor de Azure Database for PostgreSQL con la clave administrada por el cliente](concepts-data-encryption-postgresql.md).
