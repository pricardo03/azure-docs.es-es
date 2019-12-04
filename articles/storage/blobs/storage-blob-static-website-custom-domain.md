---
title: 'Tutorial: Habilitación de un dominio personalizado y SSL para sitios web estáticos en Azure'
description: Aprenda a configurar un dominio personalizado para el hospedaje de sitios web estáticos.
author: normesta
ms.service: storage
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: normesta
ms.reviewer: dineshm
ms.openlocfilehash: 893ac53dc9f0b6b162c5ec22e478cd15706e50fb
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327493"
---
# <a name="tutorial-enable-custom-domain--ssl-for-a-static-website-in-azure"></a>Tutorial: Habilitación de un dominio personalizado y SSL para sitios web estáticos en Azure

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

1. Busque la cuenta de almacenamiento en Azure Portal y muestre la información general de la cuenta.
1. Seleccione **Azure CDN** en el menú **Blob Service** para configurar Azure CDN.
1. En la sección **Perfil de CDN**, especifique un perfil de CDN nuevo o uno existente. Para más información, consulte [Inicio rápido: Creación de un perfil y un punto de conexión de Azure CDN](../../cdn/cdn-create-new-endpoint.md).
1. Especifique un plan de tarifa para el punto de conexión de CDN. En este tutorial se usa el plan de tarifa **Akamai estándar**, ya que se propaga rápidamente, normalmente en pocos minutos. Otros planes de tarifa pueden tardar más tiempo en propagarse, pero es posible que ofrezcan otras ventajas. Para más información, consulte [Comparación de las características de los productos de Azure CDN](../../cdn/cdn-features.md).
1. En el campo **Nombre del punto de conexión de CDN**, especifique el nombre del punto de conexión de CDN. El punto de conexión de CDN debe ser único en Azure.
1. Escriba el punto de conexión del sitio web estático en el campo **Nombre de host de origen**. Para buscar el punto de conexión del sitio web estático, vaya al valor **Sitio web estático** de la cuenta de almacenamiento. Copie el punto de conexión principal y péguelo en la configuración de CDN, pero quite el identificador de protocolo (*p. ej.* , HTTPS).

    La siguiente imagen muestra una configuración de punto de conexión de ejemplo:

    ![Captura de pantalla que muestra la configuración de un punto de conexión de CDN de ejemplo](media/storage-blob-static-website-custom-domain/add-cdn-endpoint.png)

1. Cree el punto de conexión de CDN y espere hasta que se propague.
1. Para comprobar que el punto de conexión de CDN está configurado correctamente, haga clic en él para ir a su configuración. Desde la información general de la red CDN de la cuenta de almacenamiento, busque el nombre de host del punto de conexión y vaya al punto de conexión, como se muestra en la siguiente imagen. El formato de punto de conexión de CDN será similar a `https://staticwebsitesamples.azureedge.net`.

    ![Captura de pantalla que muestra la información general del punto de conexión de CDN](media/storage-blob-static-website-custom-domain/verify-cdn-endpoint.png)

    Una vez que se ha completado la propagación del punto de conexión de CDN, si se desplaza hasta dicho punto de conexión, se mostrará el contenido del archivo index.html que ha cargado previamente en su sitio web estático.

1. Para revisar la configuración del origen del punto de conexión de CDN, vaya a **Origen** en la sección **Configuración** del punto de conexión de CDN. Verá que el valor del campo **Tipo de origen** es *Origen personalizado* y que el campo **Nombre de host de origen** muestra el punto de conexión del sitio web estático.

    ![Captura de pantalla muestra la configuración del origen del punto de conexión de CDN](media/storage-blob-static-website-custom-domain/verify-cdn-origin.png)

## <a name="enable-custom-domain-and-ssl"></a>Habilitar el dominio personalizado y SSL

1. Cree un registro CNAME con su proveedor de nombre de dominio para redirigir el dominio personalizado al punto de conexión de red CDN. El registro CNAME para el subdominio *www* debe ser similar al siguiente:

    ![Especificación de un registro CNAME para el subdominio www](media/storage-blob-static-website-custom-domain/subdomain-cname-record.png)

1. En Azure Portal, muestre la configuración del punto de conexión de CDN. Vaya a **Dominios personalizados** en **Configuración** para configurar el dominio personalizado y el certificado SSL.
1. Seleccione **Agregar un dominio personalizado** y escriba el nombre de dominio; después, haga clic en **Agregar**.
1. Seleccione la nueva asignación de dominio personalizado para aprovisionar un certificado SSL.
1. En **Personalizar HTTPS de dominio**, seleccione **Activar** y haga clic en **Guardar**. La configuración de un dominio personalizado puede tardar varias horas. El portal muestra el progreso, como se muestra en la siguiente imagen.

    ![Captura de pantalla que muestra el progreso de la configuración de un dominio personalizado](media/storage-blob-static-website-custom-domain/configure-custom-domain-https.png)

1. Pruebe la asignación de su sitio web estático a su dominio personalizado mediante el acceso a la dirección URL del dominio personalizado.

Para más información acerca de cómo habilitar HTTPS para dominios personalizados, consulte [Tutorial: Configuración de HTTPS en un dominio personalizado de Azure CDN](../../cdn/cdn-custom-ssl.md).

## <a name="next-steps"></a>Pasos siguientes

En la segunda parte de este tutorial, ha aprendido a configurar un dominio personalizado con SSL en Azure CDN para el sitio web estático.

Para más información acerca de cómo configurar y usar Azure CDN, consulte [¿Qué es Azure CDN?](../../cdn/cdn-overview.md)