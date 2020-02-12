---
title: Alertas de seguridad de entornos en Azure DevTest Labs
description: En este artículo se muestra cómo ver las alertas de seguridad de un entorno en DevTest Labs y tomar las medidas adecuadas.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2020
ms.author: spelluru
ms.openlocfilehash: 4ca17bece33107de756eb221e14eaab851660a99
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76992023"
---
# <a name="security-alerts-for-environments-in-azure-devtest-labs"></a>Alertas de seguridad de entornos en Azure DevTest Labs
En este artículo se muestra cómo ver alertas de seguridad de entornos en Azure DevTest Labs. 

## <a name="prerequisites"></a>Prerequisites
Actualmente, solo puede ver alertas de seguridad de los entornos implementados en el laboratorio. Para probar o usar esta característica, implemente un entorno en el laboratorio. 

## <a name="view-security-alerts-for-an-environment"></a>Visualización de alertas de seguridad de un entorno

1. En la página principal del laboratorio, seleccione **Security alerts** (Alertas de seguridad) en el menú de la izquierda. Verá el número de alertas de seguridad (alta, media y baja).

    ![Alertas de seguridad: introducción](./media/environment-security-alerts/security-alerts-overview-page.png)
2. Haga clic con el botón derecho en los tres puntos (...) de la última columna y seleccione **View security alerts** (Ver alertas de seguridad). 

    ![Ver alertas de seguridad](./media/environment-security-alerts/view-security-alerts-menu.png)
3. Puede ver más detalles sobre las alertas y las recomendaciones de Advisor. 

    ![Ver alertas de seguridad](./media/environment-security-alerts/advisor-recommendations.png)


## <a name="next-steps"></a>Pasos siguientes
Para más información sobre los entornos, consulte los siguientes artículos:

- [Creación de entornos de varias máquinas virtuales y recursos de PaaS con plantillas de Azure Resource Manager](devtest-lab-create-environment-from-arm.md)
- [Configuración y uso de entornos públicos](devtest-lab-configure-use-public-environments.md)
