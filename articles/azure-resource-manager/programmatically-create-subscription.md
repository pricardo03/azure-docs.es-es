---
title: Creación de suscripciones de Azure Enterprise mediante programación | Microsoft Docs
description: Aprenda a crear suscripciones adicionales de Azure Enterprise o de Desarrollo/pruebas - Enterprise mediante programación.
services: azure-resource-manager
author: jureid
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/10/2019
ms.author: jureid
ms.openlocfilehash: d6ae863aed629f5f5b1497d5a6e0f8108f4703c8
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848711"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Creación de suscripciones de Azure Enterprise mediante programación (versión preliminar)

Como cliente de Azure sujeto a [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), puede crear suscripciones de EA (MS-AZR-0017P) y de Desarrollo/pruebas de EA (MS-AZR-0148P) mediante programación. En este artículo, se ofrece información sobre cómo crear suscripciones mediante programación con Azure Resource Manager.

Al crear una suscripción a Azure a partir de esta API, dicha suscripción se rige por el contrato en cuyo marco ha obtenido los servicios de Microsoft Azure de Microsoft o de un distribuidor autorizado. Para obtener más información, vea [Información legal de Microsoft Azure](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

Debe tener un rol Propietario en la cuenta de inscripción en la que quiere crear suscripciones. Existen dos formas de obtener estos roles:

* El administrador de inscripciones puede [convertirle en propietario de una cuenta](https://ea.azure.com/helpdocs/addNewAccount) (inicio de sesión requerido), lo que le hace propietario de la cuenta de inscripción. Siga las instrucciones del correo electrónico de invitación que reciba para crear manualmente una suscripción inicial. Confirme la propiedad de cuenta y cree manualmente una suscripción inicial de Contrato Enterprise antes de continuar con el paso siguiente. No basta con limitarse a agregar la cuenta a la inscripción.

* Un propietario existente de la cuenta de inscripción puede [concederle acceso](grant-access-to-create-subscription.md). De forma similar, si desea usar una entidad de servicio para crear la suscripción a Contrato Enterprise, debe [conceder a dicha entidad de servicio la capacidad de crear suscripciones](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>Búsqueda de cuentas a las que tiene acceso

Cuando haya sido agregado a una inscripción de Azure EA como propietario de cuenta, Azure usa la relación cuenta-inscripción para determinar dónde se cobra la suscripción. Todas las suscripciones creadas en la cuenta se facturan a la inscripción de EA en la que se encuentra la cuenta. Para crear suscripciones, debe pasar valores sobre la cuenta de inscripción y las entidades de seguridad de usuario al propietario de la suscripción. 

Para ejecutar los comandos siguientes, debe iniciar sesión en el *directorio particular* del propietario de cuenta, que es el directorio en el que las suscripciones se crean de manera predeterminada.

## <a name="resttabrest"></a>[REST](#tab/rest)

Solicite mostrar todas las cuentas de inscripción a las que tiene acceso:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure responde con una lista de todas las cuentas de inscripción a las que tiene acceso:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

Use la propiedad `principalName` para identificar la cuenta a la que quiere que se facturen las suscripciones. Copie el elemento `name` de esa cuenta. Por ejemplo, si quisiera crear suscripciones en la cuenta de inscripción SignUpEngineering@contoso.com, copiaría ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Este es el identificador de objeto de la cuenta de inscripción. Pegue este valor en algún lugar para poder usarlo en el paso siguiente como `enrollmentAccountObjectId`.

## <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Abra [Azure Cloud Shell](https://shell.azure.com/) y seleccione PowerShell.

Use el cmdlet [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) para mostrar todas las cuentas de inscripción a las que tiene acceso.

```azurepowershell-interactive
Get-AzEnrollmentAccount
```

Azure responde con una lista de las cuentas de inscripción a las que tiene acceso:

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```
Use la propiedad `principalName` para identificar la cuenta a la que quiere que se facturen las suscripciones. Copie el elemento `ObjectId` de esa cuenta. Por ejemplo, si quisiera crear suscripciones en la cuenta de inscripción SignUpEngineering@contoso.com, copiaría ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Pegue este identificador de objeto en alguna parte para poder usarlo en el paso siguiente como `enrollmentAccountObjectId`.

## <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Use el comando [az billing enrollment-account list](https://aka.ms/EASubCreationPublicPreviewCLI) para mostrar todas las cuentas de inscripción a las que tiene acceso.

```azurecli-interactive 
az billing enrollment-account list
```

Azure responde con una lista de las cuentas de inscripción a las que tiene acceso:

```json
[
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "SignUpEngineering@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  },
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "BillingPlatformTeam@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  }
]
```

Use la propiedad `principalName` para identificar la cuenta a la que quiere que se facturen las suscripciones. Copie el elemento `name` de esa cuenta. Por ejemplo, si quisiera crear suscripciones en la cuenta de inscripción SignUpEngineering@contoso.com, copiaría ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Este es el identificador de objeto de la cuenta de inscripción. Pegue este valor en algún lugar para poder usarlo en el paso siguiente como `enrollmentAccountObjectId`.

---

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Creación de suscripciones con una cuenta de inscripción concreta

En el ejemplo siguiente se crea una suscripción denominada *Dev Team Subscription* en la cuenta de inscripción seleccionada en el paso anterior. La oferta de suscripción es *MS-AZR-0017P* (Contrato Enterprise de Microsoft normal). Opcionalmente, también agrega dos usuarios como propietarios de RBAC a la suscripción.

# <a name="resttabrest"></a>[REST](#tab/rest)

Realice la siguiente solicitud; para ello, reemplace `<enrollmentAccountObjectId>` por el elemento `name` copiado en el primer paso (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Si quiere especificar propietarios, obtenga información sobre [cómo obtener identificadores de objeto de usuario](grant-access-to-create-subscription.md#userObjectId).

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

{
  "displayName": "Dev Team Subscription",
  "offerType": "MS-AZR-0017P",
  "owners": [
    {
      "objectId": "<userObjectId>"
    },
    {
      "objectId": "<servicePrincipalObjectId>"
    }
  ]
}
```

| Nombre del elemento  | Obligatorio | type   | DESCRIPCIÓN                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Sin      | Cadena | Nombre para mostrar de la suscripción. Si no se especifica, se establece en el nombre de la oferta, por ejemplo, "Microsoft Azure Enterprise".                                 |
| `offerType`   | Sí      | Cadena | Oferta de la suscripción. Las dos opciones para EA son [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (uso en producción) y [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (desarrollo y pruebas, tiene que [activarse mediante el portal de EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Sin       | Cadena | Identificador de objeto de cualquier usuario que quiera agregarse como propietario de RBAC en la suscripción al crearla.  |

En la respuesta, se recupera un objeto `subscriptionOperation` para la supervisión. Cuando haya finalizado la creación de la suscripción, el objeto `subscriptionOperation` devolvería un objeto `subscriptionLink`, que tiene el identificador de suscripción.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

En primer lugar, instale este módulo de versión preliminar al ejecutar `Install-Module Az.Subscription -AllowPrerelease`. Para asegurarse de que `-AllowPrerelease` funciona, instale una versión reciente de PowerShellGet desde [Obtención del módulo PowerShellGet](/powershell/gallery/installing-psget).

Ejecute el comando [New-AzSubscription](/powershell/module/az.subscription) siguiente y reemplace `<enrollmentAccountObjectId>` por el elemento `ObjectId` recopilado en el primer paso (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Si quiere especificar propietarios, obtenga información sobre [cómo obtener identificadores de objeto de usuario](grant-access-to-create-subscription.md#userObjectId).

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountObjectId> -OwnerObjectId <userObjectId1>,<servicePrincipalObjectId>
```

| Nombre del elemento  | Obligatorio | type   | DESCRIPCIÓN                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Sin      | Cadena | Nombre para mostrar de la suscripción. Si no se especifica, se establece en el nombre de la oferta, por ejemplo, "Microsoft Azure Enterprise".                                 |
| `OfferType`   | Sí      | Cadena | Oferta de la suscripción. Las dos opciones para EA son [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (uso en producción) y [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (desarrollo y pruebas, tiene que [activarse mediante el portal de EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Sí       | Cadena | El identificador de objeto de la cuenta de inscripción con la que se crea la suscripción y contra la que se realizan los cargos. Este valor es un GUID que se obtiene de `Get-AzEnrollmentAccount`. |
| `OwnerObjectId`      | Sin       | Cadena | Identificador de objeto de cualquier usuario que quiera agregarse como propietario de RBAC en la suscripción al crearla.  |
| `OwnerSignInName`    | Sin       | Cadena | Dirección de correo electrónico de cualquier usuario que quiera agregarse como propietario de RBAC en la suscripción al crearla. Puede utilizar este parámetro en lugar de `OwnerObjectId`.|
| `OwnerApplicationId` | Sin       | Cadena | Identificador de aplicación de cualquier entidad de servicio que quiera agregarse como propietario de RBAC en la suscripción al crearla. Puede utilizar este parámetro en lugar de `OwnerObjectId`. Al usar este parámetro, la entidad de servicio debe tener [acceso de lectura al directorio](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Para obtener una lista completa de todos los parámetros, consulte [New-AzSubscription](/powershell/module/az.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

En primer lugar, instale esta extensión de versión preliminar al ejecutar `az extension add --name subscription`.

Ejecute el comando [az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) siguiente y reemplace `<enrollmentAccountObjectId>` por el elemento `name` copiado en el primer paso (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Si quiere especificar propietarios, obtenga información sobre [cómo obtener identificadores de objeto de usuario](grant-access-to-create-subscription.md#userObjectId).

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "<enrollmentAccountObjectId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Nombre del elemento  | Obligatorio | type   | DESCRIPCIÓN                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Sin      | Cadena | Nombre para mostrar de la suscripción. Si no se especifica, se establece en el nombre de la oferta, por ejemplo, "Microsoft Azure Enterprise".                                 |
| `offer-type`   | Sí      | Cadena | Oferta de la suscripción. Las dos opciones para EA son [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (uso en producción) y [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (desarrollo y pruebas, tiene que [activarse mediante el portal de EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Sí       | Cadena | El identificador de objeto de la cuenta de inscripción con la que se crea la suscripción y contra la que se realizan los cargos. Este valor es un GUID que se obtiene de `az billing enrollment-account list`. |
| `owner-object-id`      | Sin       | Cadena | Identificador de objeto de cualquier usuario que quiera agregarse como propietario de RBAC en la suscripción al crearla.  |
| `owner-upn`    | Sin       | Cadena | Dirección de correo electrónico de cualquier usuario que quiera agregarse como propietario de RBAC en la suscripción al crearla. Puede utilizar este parámetro en lugar de `owner-object-id`.|
| `owner-spn` | Sin       | Cadena | Identificador de aplicación de cualquier entidad de servicio que quiera agregarse como propietario de RBAC en la suscripción al crearla. Puede utilizar este parámetro en lugar de `owner-object-id`. Al usar este parámetro, la entidad de servicio debe tener [acceso de lectura al directorio](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Para obtener una lista completa de todos los parámetros, vea [az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

---

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Limitaciones de la API de creación de suscripciones de Azure Enterprise

- Con esta API solo pueden crearse suscripciones de Azure Enterprise.
- Hay un límite de 200 suscripciones por cuenta de inscripción. Después, solo se pueden crear más suscripciones para la cuenta mediante el centro de cuentas. Si quiere crear más suscripciones a través de la API, cree otra cuenta de inscripción.
- Los usuarios que no son propietarios de cuenta, pero se han agregado a una cuenta de inscripción a través de RBAC, no pueden crear suscripciones mediante el Centro de cuentas.
- No puede seleccionar el inquilino para la suscripción en que se va a crear. La suscripción siempre se crea en el inquilino principal del propietario de cuenta. Para mover la suscripción a otro inquilino, vea cómo [cambiar un inquilino de la suscripción](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener un ejemplo sobre cómo crear suscripciones con. NET, vea [ejemplo de código en GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Ahora que ha creado una suscripción, puede conceder dicha capacidad a otros usuarios y entidades de servicio. Para más información, vea [Concesión de acceso para crear suscripciones de EA (versión preliminar)](grant-access-to-create-subscription.md).
* Para obtener más información sobre cómo administrar grandes cantidades de suscripciones mediante grupos de administración, consulte [Organización de los recursos con grupos de administración de Azure](management-groups-overview.md).
