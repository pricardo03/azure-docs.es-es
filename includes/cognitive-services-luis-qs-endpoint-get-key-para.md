---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: ef8dae8219eaf1a85a5c112705517b992e25a50f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124047"
---
El acceso al punto de conexión de predicción se proporciona con una clave de punto de conexión. A los efectos de este tutorial de inicio rápido, use la clave de inicio gratuito asociada con su cuenta de LUIS. 
 
1. Inicie sesión con su cuenta de LUIS. 

    [![Captura de pantalla de la lista de aplicaciones de Language Understanding (LUIS)](media/cognitive-services-luis/app-list.png "Captura de pantalla de la lista de aplicaciones de Language Understanding (LUIS)")](media/cognitive-services-luis/app-list.png)

2. Seleccione su nombre en el menú superior derecho y seleccione **Configuración**.

    ![Acceso al menú de configuración de usuario de LUIS](media/cognitive-services-luis/get-user-settings-in-luis.png)

3. Copie el valor de **Authoring key** (Clave de creación). La usará más adelante en esta guía de inicio rápido. 

    [![Captura de pantalla de la configuración de usuario de Language Understanding (LUIS)](media/cognitive-services-luis/get-user-authoring-key.png "Captura de pantalla de la configuración de usuario de Language Understanding (LUIS)")](media/cognitive-services-luis/get-user-authoring-key.png)

    La clave de creación permite solicitudes ilimitadas gratuitas a la API de creación y hasta 1000 consultas a la API del punto de conexión de predicción por mes para todas las aplicaciones de LUIS. <!--Once the prediction endpoint quota from the authoring key is used for the month, you need to create a **Language Understanding** key from the Azure portal. The key created in the portal is known as the endpoint key. The endpoint key is used _only_ for prediction endpoint queries.-->
