---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/29/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 291ec651061b7a8a3ea3c0645a6bd6581d529ef6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245002"
---
## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure 

Inicie sesión en Azure Portal en https://portal.azure.com.

> [!NOTE]
> Si ha registrado para usar Shared galerías de imágenes durante la versión preliminar, deberá volver a registrar el `Microsoft.Compute` proveedor. Abra [Cloud Shell](https://shell.azure.com/bash) y escriba: `az provider register -n Microsoft.Compute`

## <a name="create-an-image-gallery"></a>Creación de una galería de imágenes

Una galería de imágenes es el recurso principal que se usa para habilitar el uso compartido de imágenes. Los caracteres permitidos para el nombre de la Galería son letras mayúsculas o minúsculas, números y puntos. El nombre de la Galería no puede contener guiones.  Los nombres de las galerías deben ser únicos dentro de su suscripción. 

En el ejemplo siguiente se crea una galería denominada *myGallery* en el grupo de recursos *myGalleryRG*.

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
1. Utilice el tipo de **Galería de imágenes compartidas** en el cuadro de búsqueda y seleccione **Galería de imágenes compartidas** en los resultados.
1. En el **Galería de imágenes compartidas** página, haga clic en **crear**.
1. Seleccione la suscripción correcta.
1. En **grupo de recursos**, seleccione **crear nuevo** y tipo *myGalleryRG* para el nombre.
1. En **nombre**, tipo *myGallery* para el nombre de la galería.
1. Deje el valor predeterminado para **región**.
1. Puede escribir una descripción breve de la galería, como *Mi galería de imágenes para las pruebas.* y, a continuación, haga clic en **revisión + crear**.
1. Después de validación es correcta, seleccione **crear**.
1. Cuando finalice la implementación, seleccione **ir al recurso**.
   
## <a name="create-an-image-definition"></a>Creación de la definición de una imagen 

Las definiciones de la imagen creación una agrupación lógica de imágenes. Se utilizan para administrar la información sobre las versiones de la imagen que se crean dentro de ellos. Los nombres de definición de imagen pueden estar formados por períodos, dígitos, puntos, guiones y las letras mayúsculas o minúsculas. Para obtener más información acerca de los valores que se puede especificar para una definición de la imagen, consulte [definiciones de la imagen](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Cree la definición de la imagen de galería dentro de la galería. En este ejemplo, la image de galería se denomina *myImageDefinition*.

1. En la página de la nueva galería de imágenes, seleccione **agregar una nueva definición de la imagen** desde la parte superior de la página. 
1. Para **nombre de definición de la imagen**, tipo *myImageDefinition*.
1. Para **del sistema operativo**, seleccione la opción correcta en función de la imagen de origen.
1. Para **Publisher**, tipo *myPublisher*. 
1. Para **ofrecen**, tipo *myOffer*.
1. Para **SKU**, tipo *mySKU*.
1. Cuando termine, seleccione **revisión + crear**.
1. Después de la definición de la imagen supera la validación, seleccione **crear**.
1. Cuando finalice la implementación, seleccione **ir al recurso**.


## <a name="create-an-image-version"></a>Creación de la versión de una imagen

Crear una versión de la imagen de una imagen administrada. En este ejemplo, la versión de imagen es *1.0.0* y se replica en los centros de datos *Centro-oeste de EE. UU.* y *Centro-sur de EE. UU.* Al elegir las regiones de destino para la replicación, recuerde que también tiene que incluir el *origen* región como un destino para la replicación.

Los caracteres permitidos para la versión de una imagen son números y puntos. Los números deben estar dentro del rango de un entero de 32 bits. Formato: *MajorVersion*. *MinorVersion*. *Revisión*.

1. En la página de la definición de la imagen, seleccione **agregar versión** desde la parte superior de la página.
1. En **región**, seleccione la región donde se almacena la imagen administrada. Deben crearse en la misma región que la imagen administrada que se crean de las versiones de la imagen.
1. Para **nombre**, tipo *1.0.0*. El nombre de la versión de imagen debe seguir *principales*. *menores*. *revisión* utilizando enteros de formato. 
1. En **imagen de origen**, seleccione la imagen administrada de origen en la lista desplegable.
1. En **excluye de la versión más reciente**, deje el valor predeterminado de *No*.
1. Para **final de ciclo de vida**, seleccione una fecha del calendario que es un par de meses en el futuro.
1. En **replicación**, deje el **recuento de réplica predeterminado** como 1. Debe replicar en la región de origen, por lo que deje la primera réplica como el valor predeterminado y, a continuación, elegir una segunda región de réplica se *East US*.
1. Cuando haya terminado, seleccione **revisión + crear**. Azure validará la configuración.
1. Cuando la versión de la imagen supera la validación, seleccione **crear**.
1. Cuando finalice la implementación, seleccione **ir al recurso**.

Puede tardar un rato para replicar la imagen en todas las regiones de destino.

## <a name="share-the-gallery"></a>Compartir la Galería

Se recomienda que comparten acceso en el nivel de la Galería de imágenes. El siguiente le guiará a través de uso compartido de la galería que acaba de crear.

1. Abra [Azure Portal](https://portal.azure.com).
1. En el menú de la izquierda, seleccione **grupos de recursos**. 
1. En la lista de grupos de recursos, seleccione **myGalleryRG**. Se abrirá la hoja del grupo de recursos.
1. En el menú de la izquierda de la **myGalleryRG** página, seleccione **control de acceso (IAM)** . 
1. En **agregar una asignación de roles**, seleccione **agregar**. El **agregar una asignación de roles** se abrirá el panel. 
1. En **rol**, seleccione **lector**.
1. En **asignar acceso a**, deje el valor predeterminado de **usuario, grupo o entidad de servicio de Azure AD**.
1. En **seleccione**, escriba la dirección de correo electrónico de la persona que desea invitar.
1. Si el usuario está fuera de su organización, verá el mensaje **este usuario se enviará un correo electrónico que les permite colaborar con Microsoft.** Seleccione el usuario con la dirección de correo electrónico y, a continuación, haga clic en **guardar**.

Si el usuario está fuera de su organización, reciben una invitación por correo electrónico para unirse a la organización. El usuario debe aceptar la invitación, a continuación, podrá ver la galería y todas las definiciones de la imagen y las versiones en su lista de recursos.

