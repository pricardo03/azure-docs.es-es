---
author: cynthn
ms.author: cynthn
ms.date: 04/30/2019
ms.topic: include
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: b9b2461d888f37b7ae72a3e097d77856255d7e2e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160011"
---
Las imágenes estandarizadas virtual machine (VM) permiten a las organizaciones migrar a la nube y garantizar la coherencia de las implementaciones. Normalmente, las imágenes incluyen configuraciones predefinidas de seguridad y la configuración y el software necesario. La configuración de su propia canalización de procesamiento de imágenes requiere tiempo, infraestructura y el programa de instalación, pero con el generador de imágenes de máquina virtual de Azure, basta con que proporcione una configuración simple que describe la imagen, enviarlo al servicio, y la imagen se crea y distribuye.
 
El generador de imágenes de máquina virtual de Azure (generador de imágenes de Azure) le permite comenzar con un Windows o imagen de Marketplace de Azure basado en Linux, imágenes personalizadas ya existentes o Red Hat Enterprise Linux (RHEL) ISO y empezar a agregar sus propias personalizaciones. Dado que el generador de imágenes se basa [HashiCorp Packer](https://packer.io/), también puede importar los scripts existentes de aprovisionador de shell de Packer. También puede especificar dónde desea que sus imágenes hospedadas en la Galería de imágenes compartidas Azure (virtual-machines-common-shared-image-galleries.md), como una imagen administrada o un disco duro virtual.

> [!IMPORTANT]
> Generador de imágenes de Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="preview-features"></a>Características de la versión de vista previa

Para la versión preliminar, se admiten estas características:

- Creación de imágenes de oro de la línea de base, que incluye las configuraciones corporativas y seguridad mínimos y permitir que los departamentos de TI personalizar aún más para sus necesidades.
- Aplicación de revisiones de imágenes existentes, Image Builder le permitirá revisar continuamente imágenes personalizadas ya existentes.
- Integración con la Galería de imágenes de Azure Shared, le permite distribuir, versión, y escala de imágenes globalmente y le ofrece un sistema de administración de imágenes.
- Integración con la imagen existente crear canalizaciones, simplemente se llame Image Builder desde la canalización o usar generador de Azure DevOps tarea de la imagen de vista previa sencilla.
- Migración de una canalización de personalización de imagen existente a Azure. Use los scripts existentes, comandos y los procesos para personalizar las imágenes.
- Usar el soporte de Red Hat traiga su propia suscripción. Creación de imágenes de Red Hat Enterprise para su uso con las suscripciones de Red Hat aptas, sin usar.
- Creación de imágenes en formato VHD.
 

## <a name="regions"></a>Regiones
El servicio de generador de imágenes de Azure estará disponible en versión preliminar en estas regiones. Las imágenes se pueden distribuir fuera de estas regiones.
- Este de EE. UU
- Este de EE. UU. 2
- Centro occidental de EE.UU.
- Oeste de EE. UU.
- Oeste de EE. UU. 2

## <a name="os-support"></a>Sistemas operativos admitidos
AIB será compatible con imágenes de sistema operativo bases de Azure Marketplace:
- Ubuntu 18.04
- Ubuntu 16.04
- RHEL 7.6
- CentOS 7.6
- Windows 2016
- Windows 2019


## <a name="how-it-works"></a>Cómo funciona


![Dibujo conceptual de generador de imágenes de Azure](./media/virtual-machines-image-builder-overview/image-builder.png)

El generador de imágenes de Azure es un servicio de Azure totalmente administrado que sea accesible por un proveedor de recursos de Azure. El proceso del generador de imágenes de Azure tiene tres partes principales: origen, personalizar y distribuir, se representan en una plantilla. El diagrama siguiente muestra los componentes, con algunas de sus propiedades. 
 


**Proceso del generador de imágenes** 

![Dibujo conceptual del proceso de generador de imágenes de Azure](./media/virtual-machines-image-builder-overview/image-builder-process.png)

1. Cree la plantilla de imagen como un archivo .json. Este archivo .json contiene información sobre el origen de la imagen, las personalizaciones y la distribución. Hay varios ejemplos en los [repositorio de GitHub de generador de imágenes de Azure](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).
1. Enviarla al servicio, se creará un artefacto de la plantilla de imagen en el grupo de recursos que especifique. En segundo plano, Image Builder descargará la imagen de origen o ISO y scripts según sea necesario. Estos se almacenan en un grupo de recursos independiente que se crea automáticamente en su suscripción, en el formato: IT_<DestinationResourceGroup>_<TemplateName>. 
1. Una vez creada la plantilla de imagen, a continuación, puede crear la imagen. En segundo plano Image Builder utiliza los archivos de origen y de la plantilla para crear una máquina virtual, red y almacenamiento en la IT_<DestinationResourceGroup>_<TemplateName> grupo de recursos.
1. Como parte de la creación de imágenes, el generador de imágenes distribuye la imagen de acuerdo con la plantilla, a continuación, elimina los recursos adicionales en el IT_<DestinationResourceGroup>_<TemplateName> grupo de recursos que se creó para el proceso.


## <a name="permissions"></a>Permisos

Para permitir el generador de imágenes de máquina virtual de Azure distribuir las imágenes a las imágenes de administrado o a una galería de imágenes compartidas, deberá proporcionar permisos de 'Colaborador' para el servicio "Generador de imágenes de máquina Virtual de Azure" (Id. de aplicación: cf32a0cc-373c-47c9-9156-0db11f6a6dfc ) en los grupos de recursos. 

Si usa una imagen administrada personalizada existente o una versión de la imagen, el generador de imágenes de Azure necesitará un mínimo de acceso de "Lector" a esos grupos de recursos.

Puede asignar el acceso mediante la CLI de Azure:

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/<distributeResoureGroupName>
```

Si no se encuentra la cuenta de servicio, que puede significar que la suscripción que va a agregar la asignación de roles aún no ha registrado para el proveedor de recursos.


## <a name="costs"></a>Costos
Incurrirá en algunos procesos, redes y los costos de almacenamiento al crear, compilar y almacenar las imágenes con el generador de imágenes de Azure. Estos costos son similares a los costos que conlleva la creación manual de imágenes personalizadas. Para los recursos, se le cobrará tarifas Azure. 

Durante el proceso de creación de imagen, los archivos se descargan y se almacenan en el `IT_<DestinationResourceGroup>_<TemplateName>` grupo de recursos que se incurrirá en costos de una pequeña de almacenamiento. f no desea conservar estos, elimine la plantilla de imagen después de la compilación de imagen.
 
Image Builder crea una máquina virtual con un tamaño de máquina virtual D1v2 y el almacenamiento y redes necesarios para la máquina virtual. Estos recursos estará en vigor para la duración del proceso de compilación y se eliminarán una vez que el generador de imágenes ha terminado de crear la imagen. 
 
Generador de imágenes de Azure va a distribuir la imagen a las regiones elegidas, los cuales pueden incurrir en cargos de salida de red.
 
## <a name="next-steps"></a>Pasos siguientes 
 
Para probar el generador de imágenes de Azure, consulte los artículos para la compilación [Linux](../articles/virtual-machines/linux/image-builder.md) o [Windows](../articles/virtual-machines/windows/image-builder.md) imágenes.
 
 
