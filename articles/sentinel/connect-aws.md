---
title: Conexión de datos de Symantec AWS con Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Symantec AWS con Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2019
ms.author: rkarlin
ms.openlocfilehash: 214269bc5c854aa4d3bfd508b0adb5a53ec096df
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674115"
---
# <a name="connect-azure-sentinel-to-aws"></a>Conexión de Azure Sentinel con AWS

Utilice el conector de AWS para transmitir todos los eventos de AWS CloudTrail a Azure Sentinel. Este proceso de conexión delega el acceso de Azure Sentinel en los registros de recursos de AWS, lo que crea una relación de confianza entre AWS CloudTrail y Azure Sentinel. Para ello, en AWS se crea un rol que concede a Azure Sentinel el permiso necesario para acceder a los registros de AWS.

## <a name="prerequisites"></a>Requisitos previos

Debe tener permiso de escritura en el área de trabajo de Azure Sentinel.

## <a name="connect-aws"></a>Conexión de AWS 
 
1.  En la consola de Amazon Web Services, en **Security, Identity & Compliance** (Seguridad, identidad y cumplimiento), haga clic en **IAM**.

    ![AWS1](./media/connect-aws/aws-1.png)

2.  Haga clic en **Roles** y en **Create role** (Crear rol).

    ![AWS2](./media/connect-aws/aws-2.png)

3.  Seleccione **Another AWS account** (Otra cuenta de AWS). En el campo **Account ID** (Id. de cuenta), especifique el **identificador de la cuenta Microsoft** (**123412341234**) que encontrará en la página del conector de AWS del portal de Azure Sentinel.

    ![AWS3](./media/connect-aws/aws-3.png)

4.  Asegúrese de que la opción **Require External ID** (Requerir id. externo) esté seleccionada y escriba el identificador externo (identificador del área de trabajo), que encontrará en la página del conector de AWS del portal de Azure Sentinel.

    ![AWS4](./media/connect-aws/aws-4.png)

5.  En **Attach permissions policy** (Asociar directiva de permisos), seleccione**AWSCloudTrailReadOnlyAccess**.

    ![AWS5](./media/connect-aws/aws-5.png)

6.  Escriba una etiqueta (opcional).

    ![AWS6](./media/connect-aws/aws-6.png)

7.  Especifique el **nombre del rol** y haga clic en el botón **Create role** (Crear rol).

    ![AWS7](./media/connect-aws/aws-7.png)

8.  En la lista Roles, seleccione el rol que ha creado.

    ![AWS8](./media/connect-aws/aws-8.png)

9.  Copie el **ARN del rol** y péguelo en el campo **Role to add** (Rol que se va a agregar) en el Portal de Azure Sentinel.

    ![AWS9](./media/connect-aws/aws-9.png)

10. Para usar el esquema correspondiente de Log Analytics con los eventos de AWS, busque **AWSCloudTrail**.



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar AWS CloudTrail con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

