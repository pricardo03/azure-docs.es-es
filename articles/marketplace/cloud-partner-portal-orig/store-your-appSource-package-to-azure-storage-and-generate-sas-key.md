---
title: Almacenamiento del paquete de AppSource en Azure Storage y generación de una dirección URL con la clave SAS | Microsoft Docs
description: Detalla los pasos necesarios para cargar y proteger un paquete de AppSource.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 335590cc616035020f66cfc5208e21d4c5128fe6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48808242"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Almacenamiento del paquete de AppSource en Azure Storage y generación de una dirección URL con la clave SAS
=============================================================================

Para mantener los archivos protegidos, todos los asociados deben almacenar su archivo de paquete de AppSource en una cuenta de almacenamiento de blobs de Azure y usar una clave SAS para compartirlo. Recuperaremos el archivo de paquete en su ubicación de almacenamiento de Azure para la certificación y para usarlo para las evaluaciones de AppSource.

Para cargar el paquete en el almacenamiento de blobs, siga estos pasos:

1.  Vaya a <http://azure.microsoft.com> y cree una evaluación gratuita o una cuenta de pago.

2.  Inicie sesión en [Azure Portal](http://portal.azure.com/).

3.  Para crear una nueva cuenta de almacenamiento, haga clic en **+ Nuevo** y vaya a la cuenta **Datos y almacenamiento**.

  ![Hoja Datos y almacenamiento de Microsoft Azure Portal](media/CRMScreenShot7.png)

4. Escriba un **nombre** y un nombre para el **grupo de recursos** y haga clic en el botón **Crear**.

  ![Creación de una cuenta de almacenamiento en Microsoft Azure Portal](media/CRMScreenShot8.png)

5. Desplácese al grupo de recursos recién creado y cree un nuevo contenedor de blobs.

  ![Carga del paquete como blob mediante Microsoft Azure Portal](media/CRMScreenShot9.png)

6.  Si aún no lo ha hecho, descargue e instale el [Explorador de Microsoft Azure Storage](http://storageexplorer.com/).

7.  Abra el Explorador de Storage y use el icono para conectarse a su cuenta de Azure Storage.

8.  Desplácese hasta el contenedor de blobs que creó y haga clic en **Cargar** para agregar el archivo ZIP del paquete.

  ![Carga del paquete mediante el Explorador de Microsoft Azure Storage](media/CRMScreenShot10.png)

9.  Haga clic con el botón derecho en el archivo y seleccione **Obtener firma de acceso compartido**.

  ![Obtención de la Firma de acceso compartido de un archivo de Azure](media/CRMScreenShot11.png)

10.  Modifique el valor de **Tiempo de expiración** para activar SAS durante un mes y, a continuación, haga clic en **Crear**.

  ![Modificación de la fecha de expiración de SAS de un archivo de Azure](media/CRMScreenShot12.png)

11.  Copie el campo de la URL y guárdelo para usarlo más adelante. Deberá escribir esta dirección URL cuando cree la oferta asociada. 

  ![Copia de la dirección URL de SAS de un archivo de Azure](media/CRMScreenShot13.png)

