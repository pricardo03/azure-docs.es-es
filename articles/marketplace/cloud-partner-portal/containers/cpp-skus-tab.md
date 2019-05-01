---
title: SKU de una imagen de contenedores de Azure | Azure Marketplace
description: Configure SKU para un contenedor de Azure.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pabutler
ms.openlocfilehash: 6953329bfabe99fc4bb28f2494cb412ba9cbbba0
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942897"
---
# <a name="container-skus-tab"></a>Pestaña de SKU de contenedor

La pestaña **SKU** de la página **Nueva oferta** permite crear una o varias SKU y asociarlas a la nueva oferta.  Puede usar diferentes SKU para diferenciar una solución por conjuntos de características, modelos de facturación o alguna otra característica.

## <a name="sku-settings"></a>Configuración de SKU

Cuando se empieza a crear una nueva oferta, no hay ninguna SKU asociada a ella. Para crear una nueva SKU, siga estos pasos:

1. En la pestaña SKU, seleccione **Nueva SKU**.

   ![Mensaje Nueva SKU](./media/containers-sku-settings.png)

2. Proporcione la información necesaria de la SKU y el contenedor. Cada SKU corresponde a una imagen de contenedor. Una SKU tiene dos partes:

    -   Metadatos de la SKU
    -   Metadatos del contenedor


### <a name="sku-metadata"></a>Metadatos de la SKU

Los metadatos de SKU contienen información para mostrar en escaparate del listado de contenedores.

![Metadatos de la SKU](./media/containers-sku-details.png)


### <a name="container-metadata"></a>Metadatos del contenedor

Los metadatos de contenedor contienen información de referencia de los detalles del repositorio de imágenes dentro de Azure Container Registry (ACR). Azure Marketplace copia esta imagen en un registro público específico de Marketplace y, a continuación, pasa a estar disponible para los clientes después de la certificación. Todas las solicitudes del usuario de Azure para utilizar una imagen de contenedor de Azure Marketplace se sirven desde el registro público de Marketplace, no desde ACR.

![Metadatos del contenedor](./media/containers-image-repository.png)
    
El **detalles del repositorio de imágenes** en la pantalla anterior captura contiene los siguientes campos.  Los campos obligatorios se indican con un asterisco (*).

-   **Id. de suscripción\***  -Id. de suscripción de Azure donde está presente el ACR.
-   **Nombre del grupo de recursos\***  -el nombre del grupo de recursos de ACR.
-   **Nombre del registro\***  -nombre el ACR.
-   **Nombre del repositorio\***  -el nombre del repositorio. Una vez establecido este nombre, no se puede cambiar este valor. Para evitar conflictos con otras ofertas de su cuenta, use un nombre único.
-   **Nombre de usuario\***  -el nombre de usuario (admin username) asociado a la imagen ACR.
-   **Contraseña\***  -la contraseña asociada con la imagen ACR.

    >[!NOTE]
    >El nombre de usuario y la contraseña son necesarios para asegurar que los asociados tienen acceso al ACR mencionado en el proceso de publicación.


### <a name="image-version"></a>Versión de la imagen

Al publicar una imagen de contenedor, puede proporcionar una o varias etiquetas de imagen, junto con resúmenes de SHA.

**Etiqueta de imagen\* o implícita**
 
- Esta etiqueta o resumen debe incluir una etiqueta `latest` y una etiqueta de versión (que, por ejemplo, empiece por `xx.xx.xx-`, donde xx es un número). Deben ser [etiquetas de manifiesto](https://github.com/estesp/manifest-tool) para varias plataformas. Todas las etiquetas a las que hace referencia una etiqueta de manifiesto también se deben agregar para que podamos cargarlas. 
- Puede agregar varias versiones de contenedor mediante etiquetas. Todas las etiquetas de manifiesto (excepto `latest`) deben comenzar por `X.Y-` o `X.Y.Z-`, donde X, Y, Z son números enteros. <br/> Por ejemplo, si una etiqueta `latest` apunta a `1.0.1-linux-x64`, `1.0.1-linux-arm32` y `1.0.1-windows-arm32`, estas etiquetas se deben agregar aquí.

>[!NOTE]
>No olvide agregar una **etiqueta de prueba** a la imagen para facilitar su identificación durante las pruebas.


## <a name="next-steps"></a>Pasos siguientes

Use la [pestaña Marketplace](./cpp-marketplace-tab.md) para crear una descripción de Marketplace de la oferta. 
