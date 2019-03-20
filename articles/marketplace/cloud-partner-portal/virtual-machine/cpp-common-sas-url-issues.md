---
title: Problemas de dirección URL de SAS comunes y soluciones para Azure Marketplace | Microsoft Docs
description: Se enumeran los problemas comunes relacionados con el uso de URI de firma de acceso compartido y las posibles soluciones.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/27/2018
ms.author: pbutlerm
ms.openlocfilehash: cdee17185b7051220f66ede3b9da50a333409e6d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58119282"
---
# <a name="common-sas-url-issues-and-fixes"></a>Problemas y soluciones comunes de dirección URL de SAS

En la tabla siguiente se enumeran algunos de los problemas comunes que aparecen al trabajar con firmas de acceso compartido (que se usan para identificar y compartir los discos duros virtuales cargados para la solución), junto con las resoluciones sugeridas.

| **Problema** | **Mensaje de error** | **Revisión** | 
| --------- | ------------------- | ------- | 
| &emsp;  *Error al copiar imágenes* |  |  |
| "?" no se encuentra en la dirección URL de SAS | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Actualice la dirección URL de SAS mediante las herramientas recomendadas. |
| Los parámetros "st" y "se" no están en la dirección URL de SAS | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Actualice la dirección URL de SAS con los valores **Fecha de inicio** y **Fecha de finalización** adecuados. | 
| "sp=rl" no aparece la dirección URL de SAS | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | Actualice la dirección URL de SAS con los permisos establecidos como `Read` y `List`. | 
| La dirección URL de SAS tiene espacios en blanco en el nombre del disco duro virtual | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Actualice la dirección URL de SAS para quitar los espacios en blanco. |
| Error de autorización de dirección URL de SAS | `Failure: Copying Images. Not able to download blob due to authorization error` | Revise y corrija el formato del URI de SAS. Vuelva a generarlo si es necesario. |
| Los parámetros "st" y "se" de la dirección URL de SAS no tienen una especificación de fecha y hora completa | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | Los parámetros **Fecha de inicio** y **Fecha de finalización** de la dirección URL de SAS (las subcadenas `st` y ` se`) deben tener un formato de fecha y hora completo, como `11-02-2017T00:00:00Z`. Las versiones abreviadas no son válidas. (Algunos comandos de la CLI de Azure pueden generar valores abreviados de forma predeterminada). | 
|  |  |  |

Para obtener más información, consulte [Uso de firmas de acceso compartido (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
