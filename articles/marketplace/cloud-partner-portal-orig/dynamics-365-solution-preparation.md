---
title: Preparación de la solución de Dynamics 365 | Microsoft Docs
description: Marco para empaquetar, instalar y desinstalar componentes
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricarod.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 2ea62a826b9f99e264cb1b2242f8d8108453d8e7
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54448778"
---
# <a name="dynamics-365-solution-preparation"></a>Preparación de la solución de Dynamics 365

El sistema de soluciones de Dynamics 365 es un marco para empaquetar, instalar y desinstalar los componentes que proporcionan una funcionalidad empresarial específica. ISV y otros asociados de Microsoft Dynamics 365 usan la soluciones para distribuir las extensiones que crean.

Si ya es ISV de Dynamics 365 (xRM), lo más probable es que ya haya creado una solución administrada y tenga un archivo solution.zip. En la solución, asegúrese de que los campos "Nombre para mostrar" y "Descripción" reflejen lo que quiere que vean los clientes. Estos se muestran en el Centro de administración de CRM Online.

![CRMScreenShot1](media/CRMScreenShot1.png)

_**Nota:** En el siguiente ejemplo de paquete, supondremos que el nombre de la solución es "SampleSolution.zip"_

Si se ha hecho ISV recientemente, puede obtener más información sobre cómo crear una solución aquí: [https://msdn.microsoft.com/library/gg334530.aspx](https://msdn.microsoft.com/library/gg334530.aspx)

Si la solución requiere datos complementarios:

* Cree datos de ejemplo en el entorno de prueba.
* Utilice la herramienta de migración de la configuración para crear un esquema con las reglas de comparación de los datos.
* Guarde el esquema de configuración con sus archivos del proyecto. Lo necesitará más adelante si actualiza los datos de configuración.
* Exporte los datos de configuración. Asegúrese de asignar un nombre al archivo de exportación que sea descriptivo para su exportación.
