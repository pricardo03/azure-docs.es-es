---
title: Eliminación de recursos de imagen en Azure Container Registry
description: Detalles sobre cómo administrar de forma eficaz el tamaño del registro mediante la eliminación de datos de imagen de contenedor.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 07/27/2018
ms.author: marsma
ms.openlocfilehash: 6ab667a01eddd84d1145868a3ae499e7497035c9
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39267116"
---
# <a name="delete-container-images-in-azure-container-registry"></a>Eliminación de imágenes de contenedor en Azure Container Registry

Para mantener el tamaño del registro de contenedor de Azure, debe eliminar periódicamente los datos de imagen obsoletos. Aunque algunas imágenes de contenedor implementadas en producción pueden requerir un almacenamiento a largo plazo, otras normalmente se pueden eliminar antes. Por ejemplo, en un escenario de compilación y prueba automatizado, el registro se puede rellenar rápidamente con imágenes que es posible que nunca se implementen y se pueden purgar poco después de completar el paso de compilación y prueba.

Dado que puede eliminar datos de la imagen de varias maneras diferentes, es importante comprender cómo afecta cada operación de eliminación al uso del almacenamiento. Este artículo presenta los componentes de un registro de Docker y de las imágenes de contenedor en primer lugar y, a continuación, describe varios métodos para eliminar los datos de la imagen.

## <a name="registry"></a>Registro

Un *registro* de contenedor es un servicio que almacena y distribuye imágenes de contenedor. Docker Hub es un registro de contenedor de Docker público, mientras que Azure Container Registry proporciona registros de contenedor de Docker privados en Azure.

## <a name="repository"></a>Repositorio

Los registros de contenedor administran *repositorios*, colecciones de imágenes de contenedor con el mismo nombre, pero etiquetas diferentes. Por ejemplo, las tres imágenes siguientes están en el repositorio "acr-helloworld":

```
acr-helloworld:latest
acr-helloworld:v1
acr-helloworld:v2
```

Los nombres de repositorio también pueden incluir [espacios de nombres](container-registry-best-practices.md#repository-namespaces). Los espacios de nombres le permiten agrupar imágenes con nombres de repositorio delimitados por barras diagonales hacia delante, por ejemplo:

```
marketing/campaign10-18/web:v2
marketing/campaign10-18/api:v3
marketing/campaign10-18/email-sender:v2
product-returns/web-submission:20180604
product-returns/legacy-integrator:20180715
```

## <a name="components-of-an-image"></a>Componentes de una imagen

Una imagen de contenedor dentro de un registro está asociada con una o varias etiquetas, tiene una o más capas y se identifica mediante un manifiesto. Entender cómo se relacionan entre sí estos componentes puede ayudarle a determinar el mejor método para liberar espacio en el registro.

### <a name="tag"></a>Etiqueta

Una *etiqueta* de imagen especifica su versión. A una sola imagen dentro de un repositorio se le pueden asignar una o varias etiquetas y también puede ser "sin etiqueta". Es decir, puede eliminar todas las etiquetas de una imagen, mientras que los datos de la imagen (sus capas) permanecen en el registro.

El repositorio (o repositorio y espacio de nombres) junto con una etiqueta definen el nombre de una imagen. Puede insertar y extraer una imagen especificando su nombre en la operación de inserción o extracción.

En un registro privado como Azure Container Registry, el nombre de la imagen también incluye el nombre completo del host del registro. El host del registro para imágenes en ACR está en el formato *acrname.azurecr.io*. Por ejemplo, el nombre completo de la primera imagen en el espacio de nombres "marketing" de la sección anterior sería:

```
myregistry.azurecr.io/marketing/campaign10-18/web:v2
```

Para obtener una explicación de los procedimientos recomendados para el etiquetado de imágenes, consulte la entrada del blog en MSDN [Etiquetado de Docker: procedimientos recomendados para el control de versiones y el etiquetado de imágenes de docker][tagging-best-practices].

### <a name="layer"></a>Nivel

Las imágenes se componen de una o más *capas*, cada una correspondiente a una línea en el archivo de Docker que define la imagen. Las imágenes de un registro comparten capas comunes para aumentar la eficacia de almacenamiento. Por ejemplo, varias imágenes en distintos repositorios pueden compartir la misma capa base de Alpine Linux, pero solo se almacena en el registro una copia de esa capa.

El uso compartido de las capas optimiza la distribución de la capa en nodos con varias imágenes que comparten capas comunes. Por ejemplo, si una imagen que ya se encuentra en un nodo incluye la capa de Alpine Linux como su base, la extracción posterior de una imagen distinta que haga referencia a la misma capa no transfiere la capa al nodo. En su lugar, hace referencia a la capa que ya existe en el nodo.

### <a name="manifest"></a>Manifest

Cada imagen de contenedor que se inserta en un registro de contenedor está asociada con un *manifiesto*. El manifiesto, generado por el registro cuando se inserta la imagen, identifica de forma única la imagen y especifica sus capas. Puede enumerar los manifiestos de un repositorio con el comando de la CLI de Azure [az acr repository show-manifests][az-acr-repository-show-manifests]:

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>
```

Por ejemplo, para enumerar los hash de manifiesto del repositorio "acr-helloworld":

```console
$ az acr repository show-manifests --name myregistry --repository acr-helloworld
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  },
  {
    "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
    "tags": [
      "v1"
    ],
    "timestamp": "2018-07-11T21:38:35.9170967Z"
  }
]
```

El manifiesto que se trata aquí es diferente del manifiesto de imagen que puede ver en Azure Portal o con el comando [docker manifest inspect][docker-manifest-inspect]. En la sección siguiente, "hash de manifiesto" hace referencia al hash generado por la operación de inserción, no al elemento *config.digest* del manifiesto de la imagen. Puede extraer y eliminar imágenes por el **hash de manifiesto**, no config.digest. La siguiente imagen ilustra los dos tipos de código hash.

![Hash de manifiesto y config.digest en Azure Portal][manifest-digest]

### <a name="manifest-digest"></a>Hash de manifiesto

Los manifiestos se identifican mediante un hash SHA-256 único o *hash de manifiesto*. Cada imagen, etiquetada o no, se identifica por su hash. El valor del hash es único, incluso si los datos de la capa de la imagen son idénticos a los de otra imagen. Este mecanismo es lo que le permite insertar varias veces imágenes etiquetadas de forma idéntica a un registro. Por ejemplo, puede insertar varias veces `myimage:latest` en el registro sin errores porque cada imagen se identifica mediante su hash único.

Puede extraer una imagen de un registro especificando su hash en la operación de extracción. Algunos sistemas se pueden configurar para extraer por hash porque se garantiza la versión de la imagen que se va a extraer, incluso si una imagen etiquetada de forma idéntica se inserta posteriormente en el registro.

Por ejemplo, para extraer una imagen desde el repositorio "acr-helloworld" por hash del manifiesto:

```console
$ docker pull myregistry.azurecr.io/acr-helloworld@sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108
```

> [!IMPORTANT]
> Si inserta repetidamente imágenes modificadas con etiquetas idénticas, puede crear imágenes huérfanas: imágenes que están sin etiquetas, pero consumen espacio en el registro. Las imágenes sin etiquetas no se muestran en la CLI de Azure ni en Azure Portal al enumerar o visualizar las imágenes por etiqueta. Sin embargo, sus capas existen y consumen espacio en el registro. La sección [Eliminación de imágenes sin etiquetas](#delete-untagged-images) de este artículo describe cómo liberar el espacio usado por las imágenes no etiquetadas.

## <a name="delete-image-data"></a>Eliminación de los datos de la imagen

Puede eliminar los datos de la imagen del registro de contenedor de varias maneras:

* Eliminar un [repositorio](#delete-repository): elimina todas las imágenes y todas las capas únicas dentro del repositorio.
* Eliminar por [etiqueta](#delete-by-tag): elimina una imagen, la etiqueta, todas las capas únicas a las que hace referencia la imagen y todas las demás etiquetas asociadas con la imagen.
* Eliminar por [hash de manifiesto](#delete-by-manifest-digest): elimina una imagen, todas las capas únicas a las que hace referencia la imagen y todas las etiquetas asociadas con la imagen.

## <a name="delete-repository"></a>Eliminación de un repositorio

Al eliminar un repositorio se eliminan todas las imágenes del repositorio, incluidas todas las etiquetas, capas únicas y manifiestos. Cuando se elimina un repositorio, se recupera el espacio de almacenamiento utilizado por las imágenes que se encontraban en ese repositorio.

El siguiente comando de la CLI de Azure elimina el repositorio "acr-helloworld" y todas las etiquetas y manifiestos dentro del repositorio. Si no se hace referencia a las capas a las que hacen referencia los manifiestos eliminados desde cualquier otra imagen del registro, también se eliminan los datos de la capa.

```azurecli
 az acr repository delete --name myregistry --repository acr-helloworld
```

## <a name="delete-by-tag"></a>Eliminación por etiqueta

Puede eliminar imágenes individuales de un repositorio especificando el nombre del repositorio y la etiqueta en la operación de eliminación. Cuando se elimina por etiqueta, se recupera el espacio de almacenamiento utilizado por todas las capas únicas de la imagen (las capas no compartidas por cualquier otra imagen del registro).

Para eliminar por etiqueta, use [az acr repository delete][az-acr-repository-delete] y especifique el nombre de la imagen en el parámetro `--image`. Se eliminan todas las capas únicas de la imagen y las demás etiquetas asociadas con la imagen.

Por ejemplo, para eliminar la imagen "acr-helloworld:latest" del registro "myregistry":

```azurecli
$ az acr repository delete --name myregistry --image acr-helloworld:latest
This operation will delete the manifest 'sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108' and all the following images: 'acr-helloworld:latest', 'acr-helloworld:v3'.
Are you sure you want to continue? (y/n): y
```

> [!TIP]
> La eliminación *por etiqueta* no debe confundirse con la eliminación de una etiqueta (quitar etiqueta). Puede eliminar una etiqueta con el comando de la CLI de Azure [az acr repository untag][az-acr-repository-untag]. No se libera espacio al quitar la etiqueta de una imagen porque su [manifiesto](#manifest) y los datos de la capa permanecen en el registro. Solo se elimina la propia referencia de la etiqueta.

## <a name="delete-by-manifest-digest"></a>Eliminación por hash de manifiesto

Un [hash de manifiesto](#manifest-digest) se puede asociar con una, ninguna o varias etiquetas. Cuando se elimina por hash, se eliminan todas las etiquetas a las que hace referencia el manifiesto, así como los datos de capa de todas las capas únicas de la imagen. Los datos de las capas compartidas no se eliminan.

Para eliminar por hash, debe enumerar en primer lugar los hash de manifiesto del repositorio que contiene las imágenes que desea eliminar. Por ejemplo: 

```console
$ az acr repository show-manifests --name myregistry --repository acr-helloworld
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  }
]
```

A continuación, especifique el hash que desea eliminar en el comando [az acr repository delete][az-acr-repository-delete]. El formato del comando es:

```azurecli
az acr repository delete --name <acrName> --image <repositoryName>@<digest>
```

Por ejemplo, para eliminar el último manifiesto enumerado en la salida anterior (con la etiqueta "v2"):

```console
$ az acr repository delete --name myregistry --image acr-helloworld@sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57
This operation will delete the manifest 'sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57' and all the following images: 'acr-helloworld:v2'.
Are you sure you want to continue? (y/n): y
```

La imagen "acr-helloworld:v2" se elimina del registro, así como los datos de la capa únicos para esa imagen. Si un manifiesto está asociado a varias etiquetas, también se eliminan todas las etiquetas asociadas.

## <a name="delete-untagged-images"></a>Eliminación de imágenes sin etiqueta

Como se mencionó en la sección [Hash de manifiesto](#manifest-digest), la inserción de una imagen modificada con una etiqueta existente **quita las etiquetas** de la imagen insertada previamente, lo que resulta en una imagen huérfana (o "pendiente"). El manifiesto de la imagen insertada previamente y los datos de la capa permanecen en el registro. Considere la siguiente secuencia de eventos:

1. Insertar la imagen *acr-helloworld* con la etiqueta **latest**: `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Comprobar los manifiestos del repositorio *acr-helloworld*:

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

1. Modificar el archivo de Docker *acr-helloworld*
1. Insertar la imagen *acr-helloworld* con la etiqueta **latest**: `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Comprobar los manifiestos del repositorio *acr-helloworld*:

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:38:35.9170967Z"
     },
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": null,
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

Como puede ver en la salida del último paso de la secuencia, ahora hay un manifiesto huérfano cuya propiedad `"tags"` es `null`. Este manifiesto sigue existiendo en el registro, junto con los datos de cualquier capa única a la que haga referencia. **Para eliminar estas imágenes huérfanas y sus datos de capa, debe eliminar por hash del manifiesto**.

### <a name="list-untagged-images"></a>Enumeración de imágenes sin etiqueta

Puede enumerar todas las imágenes sin etiqueta del repositorio mediante el siguiente comando de la CLI de Azure. Reemplace `<acrName>` y `<repositoryName>` por los valores adecuados para su entorno.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>  --query "[?tags==null].digest"
```

### <a name="delete-all-untagged-images"></a>Eliminación de todas las imágenes sin etiqueta

Utilice los siguientes scripts de ejemplo con precaución: los datos de las imágenes eliminadas son IRRECUPERABLES.

**CLI de Azure en Bash**

El siguiente script de Bash elimina todas las imágenes no etiquetadas de un repositorio. Requiere la CLI de Azure y **xargs**. De forma predeterminada, el script no realiza ninguna eliminación. Establezca el valor de `ENABLE_DELETE` en `true` para habilitar la eliminación de imágenes.

> [!WARNING]
> Si tiene sistemas que extraen imágenes por hash del manifiesto (en lugar de por el nombre de la imagen), no debe ejecutar este script. La eliminación de imágenes sin etiqueta impedirá que esos sistemas extraigan las imágenes del registro. En lugar de extraer por manifiesto, considere la posibilidad de adoptar un esquema de *etiquetado único*, un [procedimiento recomendado][tagging-best-practices].

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
REGISTRY=myregistry
REPOSITORY=myrepository

# Delete all untagged (orphaned) images
if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY  --query "[?tags==null].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted. Set ENABLE_DELETE=true to enable image deletion."
fi
```

**CLI de Azure en PowerShell**

El siguiente script de PowerShell elimina todas las imágenes no etiquetadas de un repositorio. Requiere PowerShell y la CLI de Azure. De forma predeterminada, el script no realiza ninguna eliminación. Establezca el valor de `$enableDelete` en `$TRUE` para habilitar la eliminación de imágenes.

> [!WARNING]
> Si tiene sistemas que extraen imágenes por hash del manifiesto (en lugar de por el nombre de la imagen), no debe ejecutar este script. La eliminación de imágenes sin etiqueta impedirá que esos sistemas extraigan las imágenes del registro. En lugar de extraer por manifiesto, considere la posibilidad de adoptar un esquema de *etiquetado único*, un [procedimiento recomendado][tagging-best-practices].

```powershell
# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to '$TRUE' to enable image delete
$enableDelete = $FALSE

# Modify for your environment
$registry = "myregistry"
$repository = "myrepository"

if ($enableDelete) {
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags==null].digest" -o tsv `
    | %{ az acr repository delete --name $registry --image $repository@$_ --yes }
} else {
    Write-Host "No data deleted. Set `$enableDelete = `$TRUE to enable image deletion."
}
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el almacenamiento de imágenes en Azure Container Registry, consulte [Almacenamiento de imágenes de contenedor en Azure Container Registry](container-registry-storage.md).

<!-- IMAGES -->
[manifest-digest]: ./media/container-registry-delete/01-manifest-digest.png

<!-- LINKS - External -->
[docker-manifest-inspect]: https://docs.docker.com/edge/engine/reference/commandline/manifest/#manifest-inspect
[portal]: https://portal.azure.com
[tagging-best-practices]: https://blogs.msdn.microsoft.com/stevelasker/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
