---
title: Visualización de las cuentas de facturación en Azure Portal | Microsoft Docs
description: Aprenda como ver las cuentas de facturación en Azure Portal
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: ec3236ffba409c390a12e46dd29263b18857c647
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74222726"
---
# <a name="view-billing-accounts-in-azure-portal"></a>Visualización de las cuentas de facturación en Azure Portal  

Cuando se registra para usar Azure, se crea una cuenta de facturación. Use su cuenta de facturación para administrar las facturas, los pagos y hacer seguimiento de los costos. Puede tener acceso a varias cuentas de facturación. Por ejemplo, es posible que te hayas registrado en Azure para tus proyectos personales. También se puede tener acceso a Azure a través del Contrato Enterprise de la organización o del contrato de cliente de Microsoft. Para cada uno de estos escenarios, se dispondría de una cuenta de facturación independiente.

Azure Portal admite actualmente los siguientes tipos de cuentas de facturación:

- **Microsoft Online Services Program**: Se crea una cuenta de facturación para Microsoft Online Services Programa cuando se registra en el sitio web de Azure. Por ejemplo, cuando se registra para obtener una [cuenta gratuita de Azure](https://azure.microsoft.com/offers/ms-azr-0044p/), como una [cuenta con tarifas de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) o como un [suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/).

- **Contrato Enterprise**: Se crea una cuenta de facturación para un Contrato Enterprise cuando la organización firma este [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) para utilizar Azure.

- **Contrato de cliente de Microsoft**: Se crea una cuenta de facturación para un contrato de cliente de Microsoft cuando una organización trabaja con un representante de Microsoft para firmar un contrato de cliente de Microsoft. Algunos clientes en regiones seleccionadas, que se registran en el sitio web de Azure para una [cuenta con tarifas de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) o actualizar su [cuenta gratuita de Azure](https://azure.microsoft.com/offers/ms-azr-0044p/), también pueden tener una cuenta de facturación para un contrato de cliente de Microsoft. Para obtener más información, consulte [Empezar a trabajar con la cuenta de facturación para un contrato de cliente de Microsoft](billing-mca-overview.md).

<!--Todo Add section to identify the type of accounts -->

## <a name="scopes-for-billing-accounts"></a>Ámbitos para las cuentas de facturación
Un ámbito es un nodo dentro de una cuenta de facturación que los usuarios utilizan para ver y administrar la facturación. Es donde los usuarios administran los datos de facturación, los pagos, las facturas y realizar la administración de cuentas generales. 

### <a name="microsoft-online-services-program"></a>Microsoft Online Services Program

|Ámbito  |Definición  |
|---------|---------|
|Cuenta de facturación     | Representa a un propietario único (administrador de cuenta) para una o varias suscripciones de Azure. Un administrador de cuenta está autorizado para realizar varias tareas de facturación, como crear suscripciones, ver facturas o cambiar la facturación de las suscripciones.  |
|Subscription     |  Representa una agrupación de los recursos de Azure. La factura se genera en este ámbito. Tiene sus propios métodos de pago que se usan para pagar esta factura.|


### <a name="enterprise-agreement"></a>Contrato Enterprise

|Ámbito  |Definición  |
|---------|---------|
|Cuenta de facturación    | Representa una inscripción de Contrato Enterprise. La factura se genera en este ámbito. Se estructura mediante el uso de cuentas de inscripción y departamentos.  |
|department     |  Agrupación opcional de las cuentas de inscripción.      |
|Cuenta de inscripción     |  Representa al propietario de una cuenta única. Las suscripciones de Azure se crean bajo este ámbito.  |


### <a name="microsoft-customer-agreement"></a>Contrato de cliente de Microsoft

|Ámbito  |Tareas  |
|---------|---------|
|Cuenta de facturación     |   Representa un contrato de cliente para varios productos y servicios de Microsoft. Se estructura mediante el uso de perfiles de facturación y las secciones de la factura.   |
|Perfil de facturación     |  Representa una factura y sus métodos de pago. La factura se genera en este ámbito. Puede tener varias secciones de factura.      |
|Sección de factura     |   Representa un grupo de costos en una factura. Las suscripciones y otras compras están asociadas a este ámbito.    |


## <a name="switch-billing-scope-in-the-azure-portal"></a>Cambio del ámbito de facturación en Azure Portal


1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-view-all-accounts/billing-search-cost-management-billing.png)

3. Seleccione **Todos los ámbitos de facturación** en el lado izquierdo.

   ![Captura de pantalla que muestra todos los ámbitos de facturación](./media/billing-view-all-accounts/billing-list-of-accounts.png)

   ** No verá **Todos los ámbitos de facturación** si solo tiene acceso a un ámbito.

4. Seleccione un ámbito para ver los detalles.



## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes
- Sepa cómo empezar a [analizar los costos](../cost-management/quick-acm-cost-analysis.md).