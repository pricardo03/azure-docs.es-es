---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3cfdca99c91dc54a711801d92aa0da91fb9703e4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66115516"
---
> [!IMPORTANT]
> La clave de la cuenta de almacenamiento es similar a la contraseña raíz de la cuenta de almacenamiento. Siempre debe proteger la clave de la cuenta. Evite distribuirla a otros usuarios, codificarla de forma rígida o guardarla en un archivo de texto al que puedan acceder otros usuarios. Vuelva a generar la clave de la cuenta mediante el Azure Portal si cree que puede verse comprometida.
> 
> Los tokens de SAS (Firma de acceso compartido) son fundamentales para proteger, al igual que las claves de acceso de la cuenta. A la vez que proporciona granularidad, SAS concede a los clientes acceso a los recursos de la cuenta de almacenamiento, por lo que no debe compartirse públicamente. Cuando sea necesario compartirla para solucionar problemas, considere la posibilidad de usar una versión censurada de los archivos de registro o eliminar los tokens de SAS (si existen) de dichos archivos, y asegúrese de que las capturas de pantalla tampoco incluyan información de SAS.
> 
> Microsoft recomienda usar la autenticación de Azure Active Directory (Azure AD) para las aplicaciones de Blob Storage y Queue Storage (versión preliminar) cuando sea posible para mejorar la seguridad. Para obtener más información, consulte [Autenticación del acceso a blobs y colas de Azure con Azure Active Directory (versión preliminar)](https://docs.microsoft.com/azure/storage/common/storage-auth-aad).
