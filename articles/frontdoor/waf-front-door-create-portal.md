---
title: Crear una directiva de firewall de aplicaciones web para el acceso de principal de Azure mediante el portal de Azure
titlesuffix: Azure web application firewall
description: Obtenga información sobre cómo crear una directiva de firewall (WAF) de aplicaciones web mediante el portal de Azure.
services: frontdoor
documentationcenter: na
author: KumudD
manager: twooley
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/8/2019
ms.author: kumud;tyao
ms.openlocfilehash: 26db3a67c3efbd0ba2a5c58facd0c07175f7ed12
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564152"
---
# <a name="create-a-waf-policy-for-azure-front-door-by-using-the-azure-portal"></a>Crear una directiva de WAF para Azure puerta de entrada mediante el portal de Azure

Este artículo describe cómo crear una directiva de firewall (WAF) de aplicación web básica de Azure y se aplican a un host de front-end en Azure puerta de entrada.

## <a name="prerequisites"></a>Requisitos previos

Para crear un perfil de Front Door siga las instrucciones que se describen en [Inicio rápido: Creación de un perfil de Front Door](quickstart-create-front-door.md). 

## <a name="create-a-waf-policy"></a>Crear una directiva de WAF

En primer lugar, cree una directiva básica de WAF con administrado predeterminado regla establecer (DRS) a través del portal. 

1. En la parte superior izquierda de la pantalla, seleccione **crear un recurso**> busque **WAF**> seleccione **firewall de aplicaciones Web (versión preliminar)** > seleccione  **Crear**.
2. En el **Fundamentos** pestaña de la **crear una directiva de WAF** , escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de configuraciones y, a continuación, seleccione **revisión + crear**:

    | Configuración                 | Valor                                              |
    | ---                     | ---                                                |
    | Subscription            |Seleccione el nombre de la suscripción de la puerta delantera.|
    | Grupos de recursos          |Seleccione el nombre del grupo de recursos de puerta de entrada.|
    | Nombre de la directiva             |Escriba un nombre único para la directiva de WAF.|

   ![Crear una directiva de WAF](./media/waf-front-door-create-portal/basic.png)

3. En el **asociación** pestaña de la **crear una directiva de WAF** página, seleccione **Agregar host de front-end**, escriba la siguiente configuración y, a continuación, seleccione **agregar**:

    | Configuración                 | Valor                                              |
    | ---                     | ---                                                |
    | Puerta de entrada              | Seleccione el nombre de perfil de la puerta de entrada.|
    | Host de front-end           | Seleccione el nombre del host de puerta de entrada y luego seleccione **agregar**.|
    
    > [!NOTE]
    > Si el host de front-end está asociado a una directiva de WAF, se muestra como deshabilitado. Debe quitar primero el host de front-end de la directiva asociada y, a continuación, volver a asociar el host de front-end a una nueva directiva de WAF.
1. Seleccione **revisión + crear**, a continuación, seleccione **crear**.

## <a name="configure-waf-rules-optional"></a>Configurar las reglas de WAF (opcionales)

### <a name="change-mode"></a>Cambiar modo

Cuando se crea una directiva de WAF, por el valor predeterminado WAF directiva está en **detección** modo. En **detección** modo, WAF no bloquea las solicitudes, en su lugar, las solicitudes que coinciden con las reglas de WAF se registran en los registros de WAF.
Para ver el WAF en acción, puede cambiar la configuración del modo de **detección** a **prevención**. En **prevención** modo, se solicita que las reglas de coincidencia que se definen en predeterminado regla establecer (DRS) quedarán bloqueadas y registradas en los registros de WAF.

 ![WAF de cambiar el modo de directiva](./media/waf-front-door-create-portal/policy.png)

### <a name="default-rule-set-drs"></a>Conjunto de reglas predeterminado (DRS)

Administrado por Azure conjunto de reglas predeterminado está habilitado de forma predeterminada. Para deshabilitar una regla individual dentro de un grupo de reglas, expanda las reglas dentro de ese grupo de reglas, seleccione el **casilla** delante del número de regla y seleccione **deshabilitar** en la pestaña anterior. Para cambiar los tipos de acciones para conjunto de reglas individuales dentro de la regla, active la casilla situada delante del número de regla y, a continuación, seleccione el **cambie acción** pestaña anterior.

 ![Cambiar el conjunto de reglas de WAF](./media/waf-front-door-create-portal/managed.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [firewall de aplicaciones web de Azure](waf-overview.md).
- Obtenga más información sobre [Azure puerta](front-door-overview.md).




