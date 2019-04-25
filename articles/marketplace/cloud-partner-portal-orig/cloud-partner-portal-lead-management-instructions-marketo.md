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
ms.openlocfilehash: 482d7a9662a79503bb2b197d5a6c63c9fa3c1c96
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60398524"
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

1. Copie los valores que se muestran en Código para insertar a fin de que pueda configurar el **identificador de servidor**, el **identificador de Munchkin** y el **identificador de formulario** en los campos de Marketo en Cloud Partner Portal.

Use el ejemplo siguiente como guía para obtener los identificadores que necesita desde el ejemplo de código para insertar de Marketo.

- Identificador de servidor = **ys12**
- Identificador de Munchkin = **123-PQR-789**
- Identificador de formulario = **1179**\
