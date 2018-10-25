---
title: Marketo | Microsoft Docs
description: Configure la administración de clientes potenciales para Marketo.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: abb0abb94d3b3e7abc4dce58cdb11fa0c2cedd34
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807192"
---
# <a name="configure-lead-management-in-marketo"></a>Configuración de la administración de clientes potenciales en Marketo

En este artículo se describe cómo configurar Marketo para administrar clientes potenciales de Microsoft.

1. Inicie sesión en Marketo.
2. Seleccione **Design Studio**.
    ![Marketo Design Studio](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo1.png)

3.  Seleccione **Nuevo formulario**.
    ![Nuevo formulario de Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo2.png)

4.  Rellene los campos obligatorios en el nuevo formulario y seleccione **Crear**.
    ![Crear nuevo formulario de Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo3.png)

4.  En Detalles de campo, seleccione **Finalizar**.
    ![Finalizar formulario de Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo4.png)

5.  Confirme y cierre.

6.  En la pestaña MarketplaceLeadBacked, seleccione **Código para insertar**.
    ![Opción de código para insertar de Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo5.png)

7.  Código para insertar de Marketo muestra un código similar al ejemplo siguiente.

`<script src="//app-ys12.marketo.com/js/forms2/js/forms2.min.js"></script>`

    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>

8.  Copie los valores que se muestran en Código para insertar a fin de que pueda configurar el **identificador de servidor**, el **identificador de Munchkin** y el **identificador de formulario** en los campos de Marketo en Cloud Partner Portal.

Use el ejemplo siguiente como guía para obtener los identificadores que necesita desde el ejemplo de código para insertar de Marketo.

- Identificador de servidor = **ys12**
- Identificador de Munchkin = **123-PQR-789**
- Identificador de formulario = **1179**\
