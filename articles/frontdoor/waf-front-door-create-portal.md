---
title: Creación de una directiva de firewall de aplicaciones web para Azure Front Door con Azure Portal
titlesuffix: Azure web application firewall
description: Aprenda a crear una directiva de firewall de aplicaciones web (WAF) mediante Azure Portal.
services: frontdoor
documentationcenter: na
author: KumudD
manager: twooley
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: abaef0fb521d848134885a06591b0656c60c67e6
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846382"
---
# <a name="create-a-waf-policy-for-azure-front-door-by-using-the-azure-portal"></a>Creación de una directiva WAF para Azure Front Door con Azure Portal

Este artículo describe cómo crear una directiva de firewall de aplicaciones web (WAF) de Azure básica y aplicarla a un host de front-end en Azure Front Door.

## <a name="prerequisites"></a>Requisitos previos

Para crear un perfil de Front Door siga las instrucciones que se describen en [Inicio rápido: Creación de un perfil de Front Door](quickstart-create-front-door.md). 

## <a name="create-a-waf-policy"></a>Creación de una directiva WAF

En primer lugar, en el portal, cree una directiva WAF con un conjunto de reglas predeterminado (DRS) administrado. 

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso**> busque **WAF**> seleccione **Firewall de aplicaciones Web (versión preliminar)** > seleccione  **Crear**.
2. En la pestaña **Datos básicos** de la página **Crear una directiva WAF**, escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y, luego, seleccione **Revisar y crear**:

    | Configuración                 | Valor                                              |
    | ---                     | ---                                                |
    | Subscription            |Seleccione el nombre de la suscripción a Front Door.|
    | Resource group          |Seleccione el nombre del grupo de recursos de Front Door.|
    | Nombre de la directiva             |Escriba un nombre único para la directiva WAF.|

   ![Creación de una directiva WAF](./media/waf-front-door-create-portal/basic.png)

3. En la pestaña **Asociación** de la página **Crear una directiva WAF**, seleccione **Agregar un host de front-end**, escriba la siguiente configuración y, a continuación, seleccione **Agregar**:

    | Configuración                 | Valor                                              |
    | ---                     | ---                                                |
    | Front Door              | Seleccione el nombre de su perfil de Front Door.|
    | Host de front-end           | Seleccione el nombre de su host de Front Door y luego seleccione **Agregar**.|
    
    > [!NOTE]
    > Si el host de front-end está asociado a una directiva WAF, se muestra como atenuado. Debe quitar primero el host de front-end de la directiva asociada y, a continuación, volver a asociarlo a una nueva directiva WAF.
1. Seleccione **Revisar y crear** y, luego, **Crear**.

## <a name="configure-waf-rules-optional"></a>Configuración de reglas de WAF (opcional)

### <a name="change-mode"></a>Cambio del modo

Cuando se crea una directiva WAF, de forma predeterminada, esta está en modo **Detección**. En el modo **Detección**, WAF no bloquea las solicitudes, sino que las solicitudes que coinciden con las reglas de WAF se registran en los registros de WAF.
Para ver WAF en acción, puede cambiar la configuración del modo de **Detección** a **Prevención**. En el modo **Prevención**, las solicitudes que coinciden con las reglas que se definen en el conjunto de reglas predeterminado (DRS) se bloquean y se registran en los registros de WAF.

 ![Cambio del modo de directiva WAF](./media/waf-front-door-create-portal/policy.png)

### <a name="custom-rules"></a>Reglas personalizadas

Puede crear una regla personalizada si selecciona **Agregar regla personalizada** bajo la sección **Reglas personalizadas**. De esta forma se inicia la página de configuración de reglas personalizadas. A continuación se presenta un ejemplo de cómo configurar una regla personalizada para bloquear una solicitud si la cadena de consulta contiene **blockme**.

![Cambio del modo de directiva WAF](./media/waf-front-door-create-portal/customquerystring2.png)

### <a name="default-rule-set-drs"></a>Conjunto de reglas predeterminado (DRS)

El conjunto de reglas predeterminado administrado por Azure está habilitado de forma predeterminada. Para deshabilitar una regla individual dentro de un grupo de reglas, expanda las reglas dentro de ese grupo de reglas, active la **casilla** delante del número de regla y seleccione **Deshabilitar** en la pestaña anterior. Para cambiar los tipos de acciones para los conjuntos de reglas individuales dentro del conjunto de reglas, active la casilla situada delante del número de regla y, a continuación, seleccione la pestaña **Cambiar acción** anterior.

 ![Cambio del conjunto de reglas de WAF](./media/waf-front-door-create-portal/managed2.png)

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [firewall de aplicaciones web de Azure](waf-overview.md).
- Obtenga más información acerca de [Azure Front Door](front-door-overview.md).
