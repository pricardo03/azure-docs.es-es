---
title: Prueba de módulos de Terraform en Azure mediante Terratest
description: Aprenda a usar Terratest para probar los módulos de Terraform.
services: terraform
ms.service: terraform
keywords: terraform, devops, cuenta de almacenamiento, azure, terratest, prueba unitaria, prueba de integración
author: JunyiYi
manager: jeconnoc
ms.author: junyi
ms.topic: tutorial
ms.date: 10/19/2018
ms.openlocfilehash: 7feee063c7b311934f7d157a9dff62d803a041b0
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638718"
---
# <a name="test-terraform-modules-in-azure-using-terratest"></a>Prueba de módulos de Terraform en Azure mediante Terratest

Los módulos de Terraform se usan para crear componentes reutilizables, que se pueden componer y probar. Incorporan la encapsulación en el mundo de "infraestructura como código".

Al igual que con otros componentes de software, el control de calidad desempeña un papel importante en los módulos de Terraform. Desafortunadamente, hay poca documentación disponible que explique cómo crear pruebas unitarias y pruebas de integración en los módulos de Terraform. Este tutorial presenta una infraestructura de pruebas y los procedimientos recomendados que hemos adoptado al crear nuestros [módulos de Terraform de Azure](https://registry.terraform.io/browse?provider=azurerm).

Después de considerar todas las infraestructuras de pruebas más populares, hemos optado por utilizar [Terratest](https://github.com/gruntwork-io/terratest). Terratest se implementa como una biblioteca de Go. Proporciona una colección de funciones auxiliares y patrones para tareas de prueba de infraestructura comunes, como hacer solicitudes HTTP y SSH para una determinada máquina virtual de prueba. Algunas de las ventajas principales de Terratest son:

- **Proporciona aplicaciones auxiliares prácticas para comprobar la infraestructura.** Esta característica resulta útil cuando desea comprobar la infraestructura real en el entorno real.
- **La estructura de carpetas se organiza claramente.** Los casos de prueba se organizarán claramente y seguirán la [estructura de carpetas de módulo de Terraform estándar](https://www.terraform.io/docs/modules/create.html#standard-module-structure).
- **Todos los casos de prueba están escritos en Go.** Puesto que la mayoría de los desarrolladores de Terraform ya lo son de Go, el uso de Terratest elimina la necesidad de aprender otro lenguaje de programación. Además, las únicas dependencias necesarias para ejecutar los casos de prueba en Terratest son Go y Terraform.
- **Esta infraestructura es muy extensible.** No resulta difícil ampliar las funciones adicionales a partir de Terratest, como características específicas de Azure.

## <a name="prerequisites"></a>Requisitos previos

Esta guía práctica es independiente de la plataforma; se puede ejecutar en Windows, Linux o MacOS. Antes de seguir, instale el software siguiente:

- **El lenguaje de programación Go**: los casos de prueba de Terraform se escriben en [Go](https://golang.org/dl/).
- **dep**: [dep](https://github.com/golang/dep#installation) es una herramienta de administración de dependencias para Go.
- **CLI de Azure**: la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) es una herramienta de línea de comandos para administrar los recursos de Azure. (Terraform admite la autenticación para Azure mediante una entidad de servicio o [la CLI de Azure](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html)).
- **mage**: usaremos el [ejecutable mage](https://github.com/magefile/mage/releases) para aprender cómo simplificar la ejecución de los casos de Terratest. 

## <a name="create-a-static-webpage-module"></a>Creación de un módulo de la página web estática

En este tutorial, creará un módulo de Terraform que aprovisione una página web estática; para ello, cargará un único archivo HTML en un blob de almacenamiento de Azure. Este módulo permitirá a los usuarios de todo el mundo acceder a esta página web con una dirección URL devuelta por el módulo.

> [!NOTE]
> Todos los archivos descritos en esta sección se deben crear en [GOPATH](https://github.com/golang/go/wiki/SettingGOPATH).

En primer lugar, cree una carpeta nueva denominada `staticwebpage` bajo la carpeta `src` de GoPath. La estructura general de carpetas de este tutorial se muestra a continuación. (Los archivos marcados con un asterisco `(*)` son el eje principal de esta sección).

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf      (*)
   ├ 📄 outputs.tf   (*)
   └ 📄 variables.tf (*)
```

El módulo de la página web estática acepta tres entradas, que se declaran en `./variables.tf`:

```hcl
variable "location" {
  description = "The Azure region in which all resources will be created."
}

variable "website_name" {
  description = "The website name which will be used to create a bunch of related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static homepage HTML in your local filesystem."
  default     = "index.html"
}
```

Como se mencionó antes, este módulo también generará una dirección URL que se declara en `./outputs.tf`:

```hcl
output "homepage_url" {
  value = "${azurerm_storage_blob.homepage.url}"
}
```

Esto nos lleva a la lógica principal de este módulo. En total, aprovisionará cuatro recursos:
- Un grupo de recursos cuyo nombre es la entrada `website_name` anexada por `-staging-rg`.
- Una cuenta de almacenamiento cuyo nombre es la entrada `website_name` anexada por `data001`. Pero con el fin de cumplir las limitaciones de nombre de la cuenta de almacenamiento, el módulo quitará todos los caracteres especiales y pondrá en minúsculas todo el nombre.
- Un contenedor de nombre fijo `wwwroot` creado en la cuenta de almacenamiento anterior.
- Un único archivo HTML que se lee la entrada `html_path` y se cargan en `wwwroot/index.html`.

La lógica del módulo de la página web estática se implementa en `./main.tf`:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "${var.website_name}-staging-rg"
  location = "${var.location}"
}

resource "azurerm_storage_account" "main" {
  name                     = "${lower(replace(var.website_name, "/[[:^alnum:]]/", ""))}data001"
  resource_group_name      = "${azurerm_resource_group.main.name}"
  location                 = "${azurerm_resource_group.main.location}"
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "main" {
  name                  = "wwwroot"
  resource_group_name   = "${azurerm_resource_group.main.name}"
  storage_account_name  = "${azurerm_storage_account.main.name}"
  container_access_type = "blob"
}

resource "azurerm_storage_blob" "homepage" {
  name                   = "index.html"
  resource_group_name    = "${azurerm_resource_group.main.name}"
  storage_account_name   = "${azurerm_storage_account.main.name}"
  storage_container_name = "${azurerm_storage_container.main.name}"
  source                 = "${var.html_path}"
  type                   = "block"
  content_type           = "text/html"
}
```

### <a name="unit-test"></a>Prueba unitaria

Tradicionalmente, Terratest es una herramienta diseñada para pruebas de integración, lo que significa que aprovisionarán los recursos reales en un entorno real. A veces, esos trabajos pueden ser excepcionalmente grandes, en especial cuando tiene cientos de recursos que aprovisionar. La lógica de conversión de nomenclatura de las cuentas de almacenamiento descrita en la sección anterior es un buen ejemplo: realmente no es necesario aprovisionar ningún recurso, simplemente deseamos asegurarnos de que es correcta.

Gracias a la flexibilidad de Terratest, esto se consigue fácilmente mediante el uso de pruebas unitarias. Las pruebas unitarias son casos de prueba que se ejecutan localmente (aunque el acceso a internet sigue siendo necesario); para ello, basta con ejecutar los comandos `terraform init` y `terraform plan`, y los casos de prueba unitaria analizarán la salida de `terraform plan` y buscarán los valores de atributo que se van a comparar.

El resto de esta sección describe cómo usar Terratest para implementar una prueba unitaria a fin de asegurarse de que la lógica de conversión de la nomenclatura de cuentas de almacenamiento es correcta. Solo nos interesan los archivos marcados con un asterisco `(*)`.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html                (*)
   │   │       └ 📄 main.tf                   (*)
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go (*)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

En primer lugar, un archivo HTML vacío `./test/fixtures/storage-account-name/empty.html` es simplemente un marcador de posición.

El archivo `./test/fixtures/storage-account-name/main.tf` es el esqueleto del caso de prueba. Acepta una entrada `website_name`, que también es la entrada de las pruebas unitarias. Su lógica se muestra aquí:

```hcl
variable "website_name" {
  description = "The name of your static website."
}

module "staticwebpage" {
  source       = "../../../"
  location     = "West US"
  website_name = "${var.website_name}"
  html_path    = "empty.html"
}
```

Por último, el componente principal es la implementación de las pruebas unitarias: `./test/storage_account_name_unit_test.go`

Si es desarrollador de Go, reconocerá que coincide con la firma de una función de prueba de Go clásica al aceptar un argumento de tipo `*testing.T`.

En el cuerpo de la prueba unitaria, tenemos un total de cinco casos definidos en la variable `testCases` (clave como entrada, mientras que el valor como salida esperada). Para cada caso de prueba unitaria, se ejecutará en primer lugar `terraform init` destinado a la carpeta del accesorio de prueba (`./test/fixtures/storage-account-name/`). 

A continuación, un comando `terraform plan` con la entrada del caso de prueba concreto (eche un vistazo a la definición `website_name` de `tfOptions`) guardará el resultado en `./test/fixtures/storage-account-name/terraform.tfplan` (que no aparece en la estructura general de la carpeta).

A continuación, este archivo de resultados se analizará en una estructura de código legible usando el analizador de planes de Terraform oficial.

Ahora se buscan los atributos que interesan (en este caso, el `name` de la `azurerm_storage_account`) y se comparan con la salida esperada.

```go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    terraformCore "github.com/hashicorp/terraform/terraform"
)

func TestUT_StorageAccountName(t *testing.T) {
    t.Parallel()

    // Test cases for storage account name conversion logic
    testCases := map[string]string{
        "TestWebsiteName": "testwebsitenamedata001",
        "ALLCAPS":         "allcapsdata001",
        "S_p-e(c)i.a_l":   "specialdata001",
        "A1phaNum321":     "a1phanum321data001",
        "E5e-y7h_ng":      "e5ey7hngdata001",
    }

    for input, expected := range testCases {
        // Specify test case folder and "-var" options
        tfOptions := &terraform.Options{
            TerraformDir: "./fixtures/storage-account-name",
            Vars: map[string]interface{}{
                "website_name": input,
            },
        }

        // Terraform init and plan only
        tfPlanOutput := "terraform.tfplan"
        terraform.Init(t, tfOptions)
        terraform.RunTerraformCommand(t, tfOptions, terraform.FormatArgs(tfOptions.Vars, "plan", "-out="+tfPlanOutput)...)

        // Read and parse the plan output
        f, err := os.Open(path.Join(tfOptions.TerraformDir, tfPlanOutput))
        if err != nil {
            t.Fatal(err)
        }
        defer f.Close()
        plan, err := terraformCore.ReadPlan(f)
        if err != nil {
            t.Fatal(err)
        }

        // Validate the test result
        for _, mod := range plan.Diff.Modules {
            if len(mod.Path) == 2 && mod.Path[0] == "root" && mod.Path[1] == "staticwebpage" {
                actual := mod.Resources["azurerm_storage_account.main"].Attributes["name"].New
                if actual != expected {
                    t.Fatalf("Expect %v, but found %v", expected, actual)
                }
            }
        }
    }
}
```

Para ejecutar las pruebas unitarias, deberá completar los pasos siguientes en la línea de comandos.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestUT_StorageAccountName
```

Verá el resultado de la prueba de Go tradicional después de aproximadamente un minuto.

### <a name="integration-test"></a>Prueba de integración

A diferencia de las pruebas unitarias, las pruebas de integración son necesarias para aprovisionar recursos en un entorno real desde la perspectiva integral. Terratest se desempeña bien en dichas tareas. Dado que el procedimiento recomendado del módulo de Terraform también recomienda la carpeta `examples` que contiene algunos ejemplos integrales, ¿por qué no usar simplemente esos ejemplos como pruebas de integración? En esta sección, nos centraremos en tres archivos, cada marcado con un asterisco `(*)`.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html              (*)
   │       └ 📄 main.tf                 (*)
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go (*)
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Comencemos con los ejemplos. Una nueva carpeta de ejemplo denominada `hello-world/` se crea en la carpeta `./examples/`. Aquí ofrecemos una página HTML sencilla para cargarse `./examples/hello-world/index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demostrate Terratest.</p>
</body>
</html>
```

El ejemplo de Terraform `./examples/hello-world/main.tf` es similar al que se muestra en la prueba unitaria, con solo una gran diferencia: también imprime la dirección URL del archivo HTML cargado denominada `homepage`.

```hcl
variable "website_name" {
  description = "The name of your static website."
  default     = "Hello-World"
}

module "staticwebpage" {
  source       = "../../"
  location     = "West US"
  website_name = "${var.website_name}"
}

output "homepage" {
  value = "${module.staticwebpage.homepage_url}"
}
```

La función de prueba de Go clásico y de Terratest vuelve a aparecer en el archivo de prueba de integración `./test/hello_world_example_test.go`.

A diferencia de las pruebas unitarias, las de integración crearán recursos reales en Azure y por eso se debe tener cuidado para evitar conflictos de nombres. (Preste atención especial a algunos nombres únicos globales, como el nombre de la cuenta de almacenamiento). Por lo tanto, es el primer paso de la lógica de prueba generar un `websiteName` aleatorio utilizando la función `UniqueId()` proporcionada por TerraTest. Esta función generará un nombre aleatorio que contiene números, letras mayúsculas o minúsculas. `tfOptions` hace que todos los comandos de Terraform tengan como destino la carpeta `./examples/hello-world/` y también garantiza que `website_name` esté establecido en el texto aleatorio `websiteName`.

A continuación, se ejecutan `terraform init`, `terraform apply` y `terraform output`, uno por uno. Usamos otra función auxiliar `HttpGetWithCustomValidation()` proporcionada por Terratest para asegurarse de que ese HTML se carga en la salida de la dirección URL `homepage` devuelta por `terraform output` al comparar el código de estado HTTP Get con `200` y buscar algunas palabras clave en el contenido del código HTML. Por último, se espera que `terraform destroy` se ejecute al aprovechar la característica de Go `defer`.

```go
package test

import (
    "fmt"
    "strings"
    "testing"

    "github.com/gruntwork-io/terratest/modules/http-helper"
    "github.com/gruntwork-io/terratest/modules/random"
    "github.com/gruntwork-io/terratest/modules/terraform"
)

func TestIT_HelloWorldExample(t *testing.T) {
    t.Parallel()

    // Generate a random website name to prevent naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demostrate Terratest.")
    })
}
```

Para ejecutar las pruebas de integración, deberá completar los pasos siguientes en la línea de comandos.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestIT_HelloWorldExample
```

Verá el resultado de la prueba de Go tradicional después de un minuto, aproximadamente. Por supuesto, también podría ejecutar ambas pruebas de unidad, así como las pruebas de integración si ejecuta:

```shell
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ go test
```

Como puede imaginar, las pruebas de integración tardan mucho más que las pruebas unitarias (dos minutos para un caso de integración mientras que un minuto para cinco casos de unidad). Pero sigue siendo decisión suya elegir cuándo usar las pruebas unitarias y cuándo aprovechar las pruebas de integración. Normalmente, es preferible utilizar pruebas unitarias para una lógica compleja con funciones de Terraform HCL; mientras que se usan las pruebas de integración desde la perspectiva integral de un usuario.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Uso de mage para simplificar la ejecución de los casos Terratest 

Como ha visto, ejecutar casos de prueba en el shell no es una tarea fácil porque tendrá que ir a diferentes directorios y ejecutar distintos comandos. Es por esta razón que se introduce el sistema de compilación en el proyecto. En esta sección, usaremos un mage de sistema de compilación de Go para realizar el trabajo.

Lo único que necesita mage es un `magefile.go` en el directorio raíz del proyecto (marcado con `(+)` en la ilustración siguiente).

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 magefile.go (+)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

A continuación se incluye un ejemplo de `./magefile.go`. En este script de compilación, escrito en Go, hemos implementado cinco pasos de compilación:
- `Clean`: este paso, eliminará todos los archivos temporales o generados durante las ejecuciones de prueba.
- `Format`: este paso ejecutará `terraform fmt` y `go fmt` para dar formato a la base de código.
- `Unit`: este paso ejecutará todas las pruebas unitarias (mediante la convención de los nombres de función `TestUT_*`) bajo la carpeta `./test/`.
- `Integration`: similar a `Unit`, pero en lugar de las pruebas unitarias, ejecuta las de integración (`TestIT_*`).
- `Full`: este paso ejecuta `Clean`, `Format`, `Unit', and `Integration` en secuencia.

```go
// +build mage

// Build script to format and run tests of a Terraform module project.
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// Default target when execute `mage` in shell
var Default = Full

// A build step that runs Clean, Format, Unit and Integration in sequence
func Full() {
    mg.Deps(Unit)
    mg.Deps(Integration)
}

// A build step that runs unit tests
func Unit() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running unit tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestUT_", "-v")
}

// A build step that runs integration tests
func Integration() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running integration tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestIT_", "-v")
}

// A build step that formats both Terraform code and Go code
func Format() error {
    fmt.Println("Formatting...")
    if err := sh.RunV("terraform", "fmt", "."); err != nil {
        return err
    }
    return sh.RunV("go", "fmt", "./test/")
}

// A build step that removes temporary build/test files
func Clean() error {
    fmt.Println("Cleaning...")
    return filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
        if err != nil {
            return err
        }
        if info.IsDir() && info.Name() == "vendor" {
            return filepath.SkipDir
        }
        if info.IsDir() && info.Name() == ".terraform" {
            os.RemoveAll(path)
            fmt.Printf("Removed \"%v\"\n", path)
            return filepath.SkipDir
        }
        if !info.IsDir() && (info.Name() == "terraform.tfstate" ||
            info.Name() == "terraform.tfplan" ||
            info.Name() == "terraform.tfstate.backup") {
            os.Remove(path)
            fmt.Printf("Removed \"%v\"\n", path)
        }
        return nil
    })
}
```

Al igual que los pasos de ejecución anteriores, puede usar los siguientes comandos para ejecutar un conjunto de pruebas completo:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in magefile or test cases
GoPath/src/staticwebpage$ go fmt      # Only requied when you change the magefile
GoPath/src/staticwebpage$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage$ mage
```

Reemplace la última línea de comandos con cualquier paso mage, por ejemplo `mage unit` o `mage clean`. Ahora es posible que piense que todavía hay muchas líneas de comandos aquí y es una buena idea incrustar comandos `dep` así como `az login` en el magefile. Pero aquí no se mostrará el código. Un paso adicional para usar mage es que los pasos podrían compartirse con el sistema de paquetes Go. Para que se pudiera simplificar magefiles a través de todos los módulos, bastaría con hacer referencia a una implementación común y declarar dependencias (`mg.Deps()`).

> [!NOTE]
> **Opción: establezca las variables del entorno principal de servicio para ejecutar las pruebas de aceptación**
> 
> En lugar de ejecutar `az login` antes de las pruebas, puede realizar la autenticación de Azure estableciendo las variables de entorno principal de servicio. Terraform publica [una lista de nombres de variable de entorno](https://www.terraform.io/docs/providers/azurerm/index.html#testing). (Solo las primeras cuatro de estas variables de entorno son obligatorias). Terraform también publica detalles de instrucción explicando cómo [obtener el valor de estas variables de entorno.](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Terratest, consulte [su página GitHub](https://github.com/gruntwork-io/terratest). Puede encontrar información útil sobre mage en [su página GitHub](https://github.com/magefile/mage) y [su página principal](https://magefile.org/).
