---
title: Creación de una cuenta de Marketplace comercial en el Centro de partners
description: Aprenda a crear una cuenta de Marketplace comercial en el Centro de partners.
author: mattwojo
manager: evansma
ms.author: parthp
ms.service: marketplace
ms.topic: guide
ms.date: 07/05/2019
ms.openlocfilehash: d6a0fec6e2efbd943a9b240df0db2c270a1b7e45
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955584"
---
# <a name="create-a-commercial-marketplace-account-in-partner-center"></a>Creación de una cuenta de Marketplace comercial en el Centro de partners

Para poder publicar ofertas en [Azure Marketplace](https://azuremarketplace.microsoft.com/) o [AppSource](https://appsource.microsoft.com/), tendrá que crear una cuenta en el programa Marketplace comercial en el Centro de partners.

## <a name="create-a-partner-center-account"></a>Creación de una cuenta del Centro de partners

En este artículo, se explica cómo crear una cuenta del Centro de partners, incluyendo cómo: 

- [Registrarse mediante la página de inscripción del Centro de partners](#to-create-a-commercial-marketplace-account-in-partner-center)
- [Iniciar sesión con una cuenta profesional](#sign-in-with-a-work-account)
- [Aceptar los términos y condiciones](#agree-to-terms-and-conditions) 
- [Proporcionar su perfil de publicador](#provide-your-publisher-profile)

>[!Important]
>Si tiene una cuenta en [Cloud Partner Portal (CPP)](https://cloudpartner.azure.com) que se ha trasladado al Centro de partners, no es necesario que cree una cuenta. Consulte [Publicadores que se trasladan desde CPP](#publishers-moving-from-cpp) para obtener más información. 

### <a name="before-you-begin"></a>Antes de empezar

Para crear una cuenta en el Centro de partners, asegúrese de que tiene:

- Autoridad para firmar contratos legales en nombre de su empresa.
- Nombre legal de empresa de su compañía, dirección y contacto principal (puede ser usted mismo).

Comprobaremos esta información durante el proceso de creación de cuenta.

### <a name="to-create-a-commercial-marketplace-account-in-partner-center"></a>Para crear una cuenta de Marketplace comercial en el Centro de partners

Revise la información de la página de inscripción de [**Bienvenida al Centro de partners de Microsoft**](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/azureisv) y regístrese para obtener una cuenta.

#### <a name="sign-in-with-a-work-account"></a>Iniciar sesión con una cuenta profesional

Vincule el dominio de cuenta de correo electrónico de trabajo de su empresa con la nueva cuenta del Centro de partners. Mediante la asociación de estas cuentas, los empleados de la empresa pueden iniciar sesión en el Centro de partners con los nombres de usuario y contraseñas de sus cuentas profesionales.

>[!Note]
>Para comprobar si su compañía ya tiene una cuenta profesional y obtener información sobre cómo crear una cuenta profesional o cómo configurar varias cuentas profesionales para usarlas con el Centro de partners, visite la [cuenta profesional de su empresa y el Centro de partners](./company-work-accounts.md). 

#### <a name="agree-to-terms-and-conditions"></a>Aceptar los términos y condiciones

Debe aceptar los términos y condiciones del [contrato de anunciante de Microsoft Marketplace](http://go.microsoft.com/fwlink/?LinkID=699560).

#### <a name="provide-your-publisher-profile"></a>Proporcionar su perfil de publicador

El perfil de publicador incluye el nombre de su compañía y el identificador de MPN. Si aún no lo ha hecho, tendrá que unirse a [Microsoft Partner Network](https://partner.microsoft.com/commercial). Una vez que se haya unido a Microsoft Partner Network, se le proporcionará un identificador de MPN. 

Cree un identificador de publicador. Este identificador identificará de forma exclusiva a su empresa y sus ofertas en Marketplace y AppSource. 

Una vez que haya confirmado la información del perfil de publicador, acepte los términos y condiciones y cree la cuenta del Centro de partners seleccionando **Aceptar y continuar**. 

>[!Important]
>*Tiene que estar autorizado para actuar en nombre de su empresa para poder aceptar estos términos.*

Le agradecemos que haya creado una cuenta en el Centro de partners. Ahora se le dirigirá a la página de [introducción a Marketplace comercial](./commercial-marketplace-overview.md).

### <a name="publishers-moving-from-cpp"></a>Publicadores que se trasladan desde CPP

Si su cuenta se ha migrado desde [Cloud Partner Portal (CPP)](https://cloudpartner.azure.com), no es necesario que cree una cuenta en el Centro de partners, ya que habrá recibido un vínculo personalizado a su nueva cuenta del Centro de partners mediante el correo electrónico y en una notificación de banner que se muestra después de iniciar sesión en una cuenta existente de CPP.

Una vez haya habilitado la nueva cuenta de Centro de partners con la visita a este vínculo personalizado, puede volver a su cuenta mediante el [panel de información de Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) en Centro de partners.

El contrato de publicación y la información de perfil de empresa se migrarán a la nueva cuenta de Centro de partners, junto con cualquier información de perfil de pago de cuenta, permisos y cuentas de usuario configurados previamente, así como ofertas activas asociadas con su cuenta de CPP. 

Una vez que la información de la cuenta se haya migrado de CPP al Centro de partners, ya no usará CPP para hacer actualizaciones de cuentas o administrar usuarios, permisos y facturación. Durante un tiempo limitado, las actualizaciones de cuenta que haga en el Centro de partners se actualizarán automáticamente con su cuenta CPP de solo lectura hasta que el portal CPP entre finalmente en desuso.

## <a name="add-new-publishers-to-the-commercial-marketplace-program"></a>Incorporación de nuevos publicadores al programa Marketplace comercial

Una organización puede tener varios publicadores asociados con una cuenta de Marketplace comercial. Un usuario existente puede agregar más publicadores después de iniciar sesión en el Centro de Partners. Para ello, debe seleccionar **Configuración de cuenta** -> **Publicadores** -> **Agregar publicador**.

>[!Note]
>Antes de agregar un nuevo publicador, puede revisar los publicadores existentes. Para hacerlo, inicie sesión en el Centro de partners y seleccione **Configuración de cuenta** -> **Publicadores** para ver una lista de los publicadores existentes.

Otro usuario del mismo inquilino de Azure Active Directory puede agregar un nuevo publicador con los pasos siguientes:

1. Inicie el flujo de suscripción en el [Centro de partners de Microsoft](https://partner.microsoft.com/en-us/dashboard/account/v3/enrollment/introduction/azureisv).
1. Seleccione **Sign in with a work account** (Iniciar sesión con una cuenta profesional) y escriba su dirección de correo electrónico profesional.
1. Seleccione el botón **Agregar publicador**.
1. Elija el identificador de MPN que quiere asociar al publicador.
1. Actualice los **detalles del publicador** en el formulario. <br>

   1. **Nombre de publicador**: nombre que se mostrará en Azure Marketplace o AppSource con la oferta. <br>
   1. **PublisherID**: identificador que usa el Centro de partners para identificar de forma única el publicador. El valor predeterminado de este campo se asigna a un `PublisherID` existente y único en el sistema, que no se puede volver a usar, por lo que es necesario actualizar este campo. <br>
   1. **Información de contacto**: actualice la información de contacto cuando sea necesario.

1. Después de completar el proceso, puede administrar el publicador recién creado desde el programa **Marketplace comercial** que se muestra en el menú de navegación izquierdo. Si no ve el programa **Marketplace comercial**, actualice la página.  El nuevo publicador aparecerá en la lista de **Publicadores**.

## <a name="next-steps"></a>Pasos siguientes

- [Administración de la cuenta de Marketplace comercial en el Centro de partners](./manage-account.md) 
