---
title: 'Tutorial: Habilitación de un dominio personalizado con SSL en un sitio web estático mediante Azure CDN (Azure Storage)'
description: Aprenda a configurar un dominio personalizado para el hospedaje de sitios web estáticos.
services: storage
author: normesta
ms.service: storage
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: normesta
ms.reviewer: seguler
ms.custom: seodec18
ms.openlocfilehash: 4b074c9dee93ba44659b0321ae3eee7fbea1c61f
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145053"
---
# <a name="tutorial-use-azure-cdn-to-enable-a-custom-domain-with-ssl-for-a-static-website"></a>Tutorial: Uso de Azure CDN para habilitar un dominio personalizado con SSL para un sitio web estático

Este tutorial es la segunda parte de una serie. En él, aprenderá a habilitar un punto de conexión de dominio personalizado con SSL para el sitio web estático. 

En este tutorial se muestra cómo usar [Azure CDN](../../cdn/cdn-overview.md) para configurar el punto de conexión de dominio personalizado para el sitio web estático. Con Azure CDN, puede aprovisionar certificados SSL personalizados, utilizar un dominio personalizado y configurar reglas de reescritura personalizadas al mismo tiempo. La configuración de Azure CDN da como resultado cargos adicionales, pero proporciona latencias bajas y coherentes a su sitio web desde cualquier parte del mundo. Azure CDN también proporciona el cifrado SSL con su propio certificado. Para más información sobre los precios de Azure CDN, consulte [Precios de Azure CDN](https://azure.microsoft.com/pricing/details/cdn/).

En la segunda parte de la serie, se aprende a:

> [!div class="checklist"]
> * Crear un punto de conexión de red CDN en el punto de conexión del sitio web estático
> * Habilitar el dominio personalizado y SSL

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar este tutorial, complete la parte primera, [Tutorial: Hospedaje de un sitio web estático en Blob Storage](storage-blob-static-website-host.md). 

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/) para empezar a trabajar.

## <a name="create-a-cdn-endpoint-on-the-static-website-endpoint"></a>Crear un punto de conexión de red CDN en el punto de conexión del sitio web estático

1. Abra [Azure Portal](https://portal.azure.com/) en el explorador web. 
1. Busque la cuenta de almacenamiento y muestre la información general de la cuenta.
1. Seleccione **Azure CDN** en el menú **Blob Service** para configurar Azure CDN.
1. En la sección **Nuevo punto de conexión**, rellene los campos para crear un nuevo punto de conexión de red CDN.
1. Escriba un nombre para el punto de conexión, como *mystaticwebsiteCDN*.
1. Escriba el dominio del sitio web como el nombre de host para el punto de conexión de red CDN.
1. En el nombre de host de origen, escriba el punto de conexión del sitio web estático. Para buscar el punto de conexión del sitio web estático, vaya a la sección **Sitio web estático** de la cuenta de almacenamiento y copie el punto de conexión. 
1. Para probar el punto de conexión de red CDN, vaya a *mywebsitecdn.azureedge.net* en el explorador.

## <a name="enable-custom-domain-and-ssl"></a>Habilitar el dominio personalizado y SSL

1. Cree un registro CNAME con su proveedor de nombre de dominio para redirigir el dominio personalizado al punto de conexión de red CDN. El registro CNAME para el subdominio *www* debe ser similar al siguiente:

    ![Especificación de un registro CNAME para el subdominio www](media/storage-blob-static-website-custom-domain/subdomain-cname-record.png)

1. En Azure Portal, haga clic en el punto de conexión que se acaba de crear para configurar el dominio personalizado y el certificado SSL.
1. Seleccione **Agregar un dominio personalizado** y escriba el nombre de dominio; después, haga clic en **Agregar**.
1. Seleccione la asignación de dominio personalizado que se acaba de crear para aprovisionar un certificado SSL.
1. Establezca **Personalizar HTTPS de dominio** en **Activar**. Seleccione **Administrado de CDN** para que Azure CDN administre el certificado SSL. Haga clic en **Save**(Guardar).
1. Pruebe el sitio web mediante el acceso a la dirección URL del sitio web.

## <a name="next-steps"></a>Pasos siguientes

En la segunda parte de este tutorial, ha aprendido a configurar un dominio personalizado con SSL en Azure CDN para el sitio web estático.

Siga este vínculo para obtener más información sobre el hospedaje del sitio web estático en Azure Storage.

> [!div class="nextstepaction"]
> [Más información sobre los sitios web estáticos](storage-blob-static-website.md)
