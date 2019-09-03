---
title: 'Guía de inicio rápido: Configuración de un firewall para un servidor de Analysis Services en Azure | Microsoft Docs'
description: Obtenga información acerca de cómo configurar un firewall para una instancia de servidor de Analysis Services en Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 07/02/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 52ccaa17b9994751bb9dfd54848e20aef90146c3
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69905877"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Inicio rápido: Configuración del firewall del servidor: Portal

Esta guía de inicio rápido le ayudará a configurar un firewall para el servidor de Azure Analysis Services. La habilitación de un firewall y la configuración de los intervalos de direcciones IP solo para los equipos que tienen acceso al servidor son una parte importante de la protección del servidor y los datos.

## <a name="prerequisites"></a>Requisitos previos

- Un servidor de Analysis Services en la suscripción. Para más información, vea esta [Inicio rápido: Creación de un servidor (Portal)](analysis-services-create-server.md) o [Inicio rápido: Creación de un servidor (PowerShell)](analysis-services-create-powershell.md)
- Uno o más intervalos de direcciones IP para los equipos cliente (si es necesario).
- Tenga en cuenta que el escenario de importación de Power BI Premium no se admite actualmente.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal 

[Iniciar sesión en el portal](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Configuración de un firewall

1. Haga clic en el servidor para abrir la página Información general. 
2. En **Configuración** > **Firewall** > **Habilitar firewall**, haga clic en **Activar**.
3. Para permitir el acceso de DirectQuery desde el servicio Power BI, en **Permitir el acceso desde Power BI**, haga clic en **Activar**.  
4. (Opcional) Especifique uno o más intervalos de direcciones IP. Escriba un nombre y una dirección IP inicial y final para cada intervalo. El nombre de la regla de firewall debe estar limitado a 128 caracteres y solo puede contener caracteres en mayúsculas, caracteres en minúsculas, números, caracteres de subrayado y guiones. No se permiten espacios en blanco ni otros caracteres especiales.
5. Haga clic en **Save**(Guardar).

     ![Configuración de firewall](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine intervalos de direcciones IP o deshabilite el firewall.

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha aprendido a configurar un firewall para el servidor. Ahora que tiene el servidor y está protegido con un firewall, puede agregar un modelo de datos de ejemplo básico desde el portal. Tener un modelo de ejemplo es útil para aprender cómo configurar los roles de la base de datos del modelo y probar las conexiones de cliente. Para más información, puede seguir el tutorial para agregar un modelo de ejemplo.

> [!div class="nextstepaction"]
> [Tutorial: Incorporación de un modelo de ejemplo al servidor](analysis-services-create-sample-model.md)
