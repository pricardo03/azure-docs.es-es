---
title: Introducción a Terraform en Azure.
description: Empiece a trabajar con Terraform en Azure mediante la implementación de Azure Cosmos DB y Azure Container Instances.
services: terraform
author: neilpeterson
ms.service: azure
ms.topic: quickstart
ms.date: 09/20/2019
ms.author: nepeters
ms.openlocfilehash: c53f3a31b46f00d3207cd8f47dcfbfa131c03666
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71173517"
---
# <a name="create-a-terraform-configuration-for-azure"></a>Creación de una configuración de Terraform para Azure

En este ejemplo, ganará experiencia a la hora de crear una configuración de Terraform y de implementar esta configuración en Azure. Cuando haya terminado, habrá implementado una instancia de Azure Cosmos DB, una de Azure Container Instances y una aplicación que funciona con estos dos recursos. En este documento se da por supuesto que se ha completado todo el trabajo en Azure Cloud Shell, que contiene las herramientas de Terraform preinstaladas. Si desea trabajar con el ejemplo en su propio sistema, se puede instalar Terraform mediante las instrucciones que se encuentran [aquí](../virtual-machines/linux/terraform-install-configure.md).

## <a name="create-first-configuration"></a>Creación de la primera configuración

En esta sección, creará la configuración de una instancia de Azure Cosmos DB.

Seleccione **Probar ahora** para abrir Azure Cloud Shell. Una vez abierto, escriba `code .` para abrir el editor de código de Cloud Shell.

```bash
code .
```

Copie y pegue la siguiente configuración de Terraform.

Esta configuración modela un grupo de recursos de Azure, un entero aleatorio y una instancia de Azure Cosmos DB. El entero aleatorio se utiliza en el nombre de la instancia de Cosmos DB. Se configuran también varios valores de Cosmos DB. Para obtener una lista completa de las configuraciones, consulte la [referencia sobre Cosmos DB Terraform](https://www.terraform.io/docs/providers/azurerm/r/cosmosdb_account.html).

Guarde el archivo como `main.tf` cuando haya terminado. Esta operación puede realizarse mediante el botón de puntos suspensivos de la parte superior derecha del editor de código.

```hcl
resource "azurerm_resource_group" "vote-resource-group" {
  name     = "vote-resource-group"
  location = "westus"
}

resource "random_integer" "ri" {
  min = 10000
  max = 99999
}

resource "azurerm_cosmosdb_account" "vote-cosmos-db" {
  name                = "tfex-cosmos-db-${random_integer.ri.result}"
  location            = "${azurerm_resource_group.vote-resource-group.location}"
  resource_group_name = "${azurerm_resource_group.vote-resource-group.name}"
  offer_type          = "Standard"
  kind                = "GlobalDocumentDB"

  consistency_policy {
    consistency_level       = "BoundedStaleness"
    max_interval_in_seconds = 10
    max_staleness_prefix    = 200
  }

  geo_location {
    location          = "westus"
    failover_priority = 0
  }
}
```

El comando [terraform init](https://www.terraform.io/docs/commands/init.html) inicializa el directorio de trabajo. Ejecute `terraform init` en el terminal de Cloud Shell para preparar la implementación de la nueva configuración.

```bash
terraform init
```

El comando [terraform plan](https://www.terraform.io/docs/commands/plan.html) se puede usar para comprobar que la configuración tiene el formato adecuado y para visualizar qué recursos se crearán, actualizarán o destruirán. Los resultados se pueden almacenar en un archivo y usar posteriormente para aplicar la configuración.

Ejecute `terraform plan` para probar la nueva configuración de Terraform.

```bash
terraform plan --out plan.out
```

Aplique la configuración mediante el comando [terraform apply](https://www.terraform.io/docs/commands/apply.html) y especifique el nombre del archivo del plan. Este comando implementa los recursos en su suscripción de Azure.

```bash
terraform apply plan.out
```

Una vez terminado esto, puede ver que se ha creado el grupo de recursos y que se ha colocado una instancia de Azure Cosmos DB en él.

## <a name="update-configuration"></a>actualizar la configuración

Actualice la configuración para incluir una instancia de Azure Container Instances. El contenedor ejecuta una aplicación que lee y escribe datos en Cosmos DB.

Copie la siguiente configuración en la parte inferior del archivo `main.tf`. Guarde el archivo cuando haya terminado.

Se establecen dos variables de entorno, `COSMOS_DB_ENDPOINT` y `COSMOS_DB_MASTERKEY`. Estas variables contienen la ubicación y la clave para acceder a la base de datos. Los valores de estas variables se obtienen de la instancia de la base de datos que se creó en el último paso. Este proceso se denomina interpolación. Para más información acerca de la interpolación de Terraform, consulte [Sintaxis de interpolación](https://www.terraform.io/docs/configuration/interpolation.html).


La configuración también incluye un bloque de salida, que devuelve el nombre de dominio completo (FQDN) de la instancia del contenedor.

```hcl
resource "azurerm_container_group" "vote-aci" {
  name                = "vote-aci"
  location            = "${azurerm_resource_group.vote-resource-group.location}"
  resource_group_name = "${azurerm_resource_group.vote-resource-group.name}"
  ip_address_type     = "public"
  dns_name_label      = "vote-aci"
  os_type             = "linux"

  container {
    name   = "vote-aci"
    image  = "microsoft/azure-vote-front:cosmosdb"
    cpu    = "0.5"
    memory = "1.5"
    ports {
      port     = 80
      protocol = "TCP"
    }

    secure_environment_variables = {
      "COSMOS_DB_ENDPOINT"  = "${azurerm_cosmosdb_account.vote-cosmos-db.endpoint}"
      "COSMOS_DB_MASTERKEY" = "${azurerm_cosmosdb_account.vote-cosmos-db.primary_master_key}"
      "TITLE"               = "Azure Voting App"
      "VOTE1VALUE"          = "Cats"
      "VOTE2VALUE"          = "Dogs"
    }
  }
}

output "dns" {
  value = "${azurerm_container_group.vote-aci.fqdn}"
}
```

Ejecute `terraform plan` para crear el plan actualizado y visualizar los cambios que se van a realizar. En este momento puede ver que se ha agregado un recurso de Azure Container Instances a la configuración.

```bash
terraform plan --out plan.out
```

Por último, ejecute `terraform apply` para aplicar la configuración.

```bash
terraform apply plan.out
```

Una vez completado, tome nota del nombre de dominio completo de la instancia de contenedor.

## <a name="test-application"></a>Prueba de la aplicación

Vaya al nombre de dominio completo de la instancia de contenedor. Si todo se ha configurado correctamente, debería ver la siguiente aplicación.

![Aplicación Azure Vote](media/terraform-quickstart/azure-vote.jpg)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado, se pueden eliminar los recursos y el grupo de recursos mediante el comando [terraform destroy](https://www.terraform.io/docs/commands/destroy.html).

```bash
terraform destroy -auto-approve
```

## <a name="next-steps"></a>Pasos siguientes

En este ejemplo, ha creado, implementado y eliminado una configuración de Terraform. Para más información sobre el uso de Terraform en Azure, consulte la documentación del proveedor de Terraform para Azure.

> [!div class="nextstepaction"]
> [Proveedor de Terraform para Azure](https://www.terraform.io/docs/providers/azurerm/)
