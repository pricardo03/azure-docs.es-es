---
title: Hospedaje de sitios web estáticos en Azure Storage (versión preliminar) | Microsoft Docs
description: Azure Storage ahora ofrece hospedaje de sitios web estáticos (versión preliminar), lo que proporciona una solución rentable y escalable para hospedar aplicaciones web modernas.
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: article
ms.date: 08/17/18
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 65a1cd85baf18ac1f0d193e7e6d6c3139919fb59
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617404"
---
# <a name="static-website-hosting-in-azure-storage-preview"></a>Hospedaje de sitios web estáticos en Azure Storage (versión preliminar)
Azure Storage ahora ofrece hospedaje de sitios web estáticos (versión preliminar), lo que permite implementar aplicaciones web modernas rentables y escalables en Azure. En un sitio web estático, las páginas web cuentan con contenido estático y JavaScript u otro código del lado del cliente. En cambio, los sitios web dinámicos dependen de código del lado del servidor y se pueden hospedar mediante [Azure Web Apps](/azure/app-service/app-service-web-overview).

Dado que las implementaciones se desplazan hacia modelos flexibles y rentables, la capacidad para entregar contenido web sin necesidad de administrar servidores es fundamental. La introducción del hospedaje de sitios web estáticos en Azure Storage permite esto y se habilitan funcionalidades de back-end avanzadas con arquitecturas sin servidores que aprovechan [Azure Functions](/azure/azure-functions/functions-overview) y otros servicios de PaaS.

## <a name="how-does-it-work"></a>¿Cómo funciona?
Cuando se habilitan sitios web estáticos en la cuenta de almacenamiento, se crea un nuevo punto de conexión de servicio web del formulario `<account-name>.<zone-name>.web.core.windows.net`.

El punto de conexión de servicio web siempre permite el acceso de lectura anónimo, devuelve páginas HTML con formato en respuesta a errores de servicio y solo permite las operaciones de lectura de objetos. El punto de conexión de servicio web devuelve el documento de índice en el directorio solicitado tanto para la raíz como para todos los subdirectorios. Cuando el servicio de almacenamiento devuelve un error 404, el punto de conexión web devuelve un documento de errores personalizados si está configurado.

El contenido del sitio web estático está hospedado en un contenedor especial llamado "$web". Como parte del proceso de habilitación, "$web" se crea automáticamente si es que aún no existe. Se puede acceder al contenido de "$web" en la raíz de la cuenta mediante el punto de conexión web. Por ejemplo, `https://contoso.z4.web.core.windows.net/` devuelve el documento de índice que se ha configurado para el sitio web, si existe un documento con ese nombre en el directorio raíz de $web.

Al cargar el contenido al sitio web, use el punto de conexión de almacenamiento de blobs. Para cargar un blob denominado "image.jpg" al que se puede acceder en la raíz de la cuenta, use la siguiente dirección URL `https://contoso.blob.core.windows.net/$web/image.jpg`. La imagen cargada se puede visualizar en un explorador web en el punto de conexión web correspondiente `https://contoso.z4.web.core.windows.net/image.jpg`.


## <a name="custom-domain-names"></a>Nombres de dominio personalizados
Puede usar un dominio personalizado para hospedar el contenido web. Para ello, siga la guía de [Configuración de un nombre de dominio personalizado para una cuenta de Azure Storage](storage-custom-domain-name.md). Para obtener acceso a su sitio web hospedado en un nombre de dominio personalizado a través de HTTPS, consulte [Uso de Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md). Apunte CDN al punto de conexión web en lugar del punto de conexión del blob y recuerde que la configuración de CDN no es inmediata, por lo que quizá deba esperar unos minutos para ver el contenido.

## <a name="pricing-and-billing"></a>Precios y facturación
El hospedaje de sitios web estáticos se proporciona sin costo adicional alguno. Para obtener más información acerca de los precios de Azure Blob Storage, consulte la [página de precios de Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Guía de inicio rápido
### <a name="azure-portal"></a>Azure Portal
Si aún no lo ha hecho, [cree una cuenta de almacenamiento GPv2](../common/storage-quickstart-create-account.md). Para empezar a hospedar la aplicación web, puede configurar la característica con Azure Portal; haga clic en "Sitio web estático (versión preliminar)" en "Configuración" en la barra de navegación izquierda. Haga clic en "Habilitado" y escriba el nombre del documento de índice y (opcionalmente) la ruta de acceso del documento de error personalizado.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Cargue los recursos web en el contenedor "$web" que se creó como parte de la habilitación del sitio web estático. Puede hacerlo directamente en Azure Portal, o puede beneficiarse del [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) para cargar las estructuras de todo el directorio. Asegúrese de incluir un documento de índice con el nombre que configuró. En este ejemplo, el nombre del documento es "index.html".

> [!NOTE]
> El nombre del documento distingue mayúsculas de minúsculas y, por tanto, debe coincidir exactamente con el nombre del archivo de almacenamiento.

Por último, desplácese hasta el punto de conexión web para probar su sitio web.

### <a name="azure-cli"></a>Azure CLI
Instale la extensión de la versión preliminar de almacenamiento:

```azurecli-interactive
az extension add --name storage-preview
```
Habilite la característica:

```azurecli-interactive
az storage blob service-properties update --account-name <account-name> --static-website --404-document <error-doc-name> --index-document <index-doc-name>
```
Consulte la dirección URL del punto de conexión web:

```azurecli-interactive
az storage account show -n <account-name> -g <resource-group> --query "primaryEndpoints.web" --output tsv
```

Cargue objetos en el contenedor $web:

```azurecli-interactive
az storage blob upload-batch -s deploy -d $web --account-name <account-name>
```

## <a name="faq"></a>Preguntas más frecuentes
**¿Están disponibles los sitios web estáticos para todos los tipos de cuenta de almacenamiento?**  
No, el hospedaje de sitios web estáticos solo está disponible en las cuentas de almacenamiento estándar GPv2.

**¿Se admiten las reglas de firewall y VNET de almacenamiento con el nuevo punto de conexión web?**  
Sí, el nuevo punto de conexión web cumple las reglas de firewall y de VNET configuradas para la cuenta de almacenamiento.

**¿Distingue el punto de conexión web entre mayúsculas y minúsculas?**  
Sí, el punto de conexión web distingue entre mayúsculas y minúsculas, al igual que el punto de conexión del blob. 

## <a name="next-steps"></a>Pasos siguientes
* 
  [Uso de Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar un nombre de dominio personalizado para el punto de conexión web o de blob](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure Web Apps](/azure/app-service/app-service-web-overview)
* [Crear la primera aplicación web sin servidor](https://aka.ms/static-serverless-webapp)
