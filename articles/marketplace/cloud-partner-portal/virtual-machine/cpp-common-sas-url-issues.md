---
title: Problemas de dirección URL de SAS comunes y soluciones para Azure Marketplace
description: Se enumeran los problemas comunes relacionados con el uso de URI de firma de acceso compartido y las posibles soluciones.
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
ms.service: marketplace
ms.topic: article
ms.date: 09/27/2018
ms.author: pabutler
ms.openlocfilehash: 4f2770312624e1ca4c939ade458a451eb03f9d20
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938374"
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
| Los parámetros "st" y "se" de la dirección URL de SAS no tienen una especificación de fecha y hora completa | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | Dirección URL de SAS **Start Date** y **fecha de finalización** parámetros (`st` y `se` subcadenas) deben tener el formato datetime completa, como `11-02-2017T00:00:00Z`. Las versiones abreviadas no son válidas. (Algunos comandos de la CLI de Azure pueden generar valores abreviados de forma predeterminada). | 
|  |  |  |

Para obtener más información, consulte [Uso de firmas de acceso compartido (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
