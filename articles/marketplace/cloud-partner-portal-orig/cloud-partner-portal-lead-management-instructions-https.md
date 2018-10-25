---
title: Punto de conexión HTTPS | Microsoft Docs
description: Configure la administración de clientes potenciales para HTTPS.
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
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: fd13a7281c7e8702fd199364261ebcd458db0555
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807152"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Configuración de la administración de clientes potenciales mediante un punto de conexión HTTPS

Puede usar un punto de conexión HTTPS para administrar clientes potenciales de AppSource y Azure Marketplace que se pueden escribir en un sistema CRM. En este artículo se describe cómo configurar la administración de clientes potenciales mediante el servicio de automatización Microsoft Flow.


## <a name="create-a-flow-using-microsoft-flow"></a>Creación de un flujo mediante Microsoft Flow

1.  Abra la página web de [Flow](https://flow.microsoft.com/). Seleccione **Iniciar sesión** o **Regístrese gratis** para crear una cuenta gratuita de Flow.

2.  Inicie sesión y seleccione **Mis flujos** en la barra de menús.

    ![Mis flujos](./media/cloud-partner-portal-lead-management-instructions-https/image001.png)

3.  Seleccione **Crear desde cero**.

    ![Crear desde cero](./media/cloud-partner-portal-lead-management-instructions-https/image003.png)


4.  Seleccione el conector **Request/Response** y busque el desencadenador de solicitudes. 

    ![Crear desde cero](./media/cloud-partner-portal-lead-management-instructions-https/image005.png)

5. Seleccione el desencadenador **Request**.
    ![Desencadenador de solicitud](./media/cloud-partner-portal-lead-management-instructions-https/image007.png)


6.  Copie el **ejemplo de JSON** que está al final de este artículo en el **esquema de JSON del cuerpo de la solicitud**.

7.  Agregue un paso y elija el sistema CRM que prefiera con la acción para crear un registro. La siguiente captura de pantalla muestra **Dynamics 365: Crear un nuevo registro** como ejemplo.

    ![Crear un nuevo registro](./media/cloud-partner-portal-lead-management-instructions-https/image009.png)

8.  Proporcione las entradas de conexión para el conector y seleccione la entidad **Clientes potenciales**.

    ![Selección de clientes potenciales](./media/cloud-partner-portal-lead-management-instructions-https/image011.png)

9.  Flow muestra un formulario para brindar información sobre el cliente potencial. Puede asignar elementos de la solicitud de entrada y elige agregar contenido dinámico.

    ![Incorporación de contenido dinámico](./media/cloud-partner-portal-lead-management-instructions-https/image013.png)

10.  Asigne los campos que quiera y seleccione **Guardar** para guardar el flujo.

11. Se crea una dirección URL de HTTP POST en la solicitud. Copie esta dirección URL y úsela como el punto de conexión HTTPS.

    ![Dirección URL de HTTP POST](./media/cloud-partner-portal-lead-management-instructions-https/image015.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Configuración de la oferta para enviar clientes potenciales al punto de conexión HTTPS

Cuando configure la información de administración de clientes potenciales para la oferta, seleccione **Punto de conexión HTTPS** para el destino de clientes potenciales y pegue la dirección URL de HTTPS POST que copió en el paso anterior.  

![Incorporación de contenido dinámico](./media/cloud-partner-portal-lead-management-instructions-https/image017.png)

Cuando se generan los clientes potenciales, Microsoft enviará clientes potenciales a Flow, los que se enrutarán al sistema CRM que configuró.


## <a name="json-example"></a>Ejemplo JSON

``` json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "id": "http://example.com/example.json",
  "properties": {
    "ActionCode": {
      "id": "/properties/ActionCode",
      "type": "string"
    },
    "OfferTitle": {
      "id": "/properties/OfferTitle",
      "type": "string"
    },
    "LeadSource": {
      "id": "/properties/LeadSource",
      "type": "string"
    },
    "UserDetails": {
      "id": "/properties/UserDetails",
      "properties": {
        "Company": {
          "id": "/properties/UserDetails/properties/Company",
          "type": "string"
        },
        "Country": {
          "id": "/properties/UserDetails/properties/Country",
          "type": "string"
        },
        "Email": {
          "id": "/properties/UserDetails/properties/Email",
          "type": "string"
        },
        "FirstName": {
          "id": "/properties/UserDetails/properties/FirstName",
          "type": "string"
        },
        "LastName": {
          "id": "/properties/UserDetails/properties/LastName",
          "type": "string"
        },
        "Phone": {
          "id": "/properties/UserDetails/properties/Phone",
          "type": "string"
        },
        "Title": {
          "id": "/properties/UserDetails/properties/Title",
          "type": "string"
        }
      },
      "type": "object"
    }
  },
  "type": "object"
}
```
