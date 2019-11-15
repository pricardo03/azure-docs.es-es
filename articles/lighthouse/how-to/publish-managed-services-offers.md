---
title: Publicación de una oferta de servicios administrados en Azure Marketplace
description: Aprenda a publicar una oferta de servicio administrada que incorpore los clientes a la administración de recursos delegados de Azure.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 10/17/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 4b2ce1253fd4421b36105fdbae68c6e89173a3c6
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615461"
---
# <a name="publish-a-managed-services-offer-to-azure-marketplace"></a>Publicación de una oferta de servicios administrados en Azure Marketplace

En este artículo, aprenderá a publicar una oferta de servicios administrados pública o privada en [Azure Marketplace](https://azuremarketplace.microsoft.com) mediante [Cloud Partner Portal](https://cloudpartner.azure.com/), lo que permite que un cliente que compra la oferta se incorpore a la administración de recursos delegados de Azure.

> [!NOTE]
> Debe tener una [cuenta válida en el Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) para crear y publicar estas ofertas. Si aún no tiene una cuenta, el [proceso de registro](https://aka.ms/joinmarketplace) le guiará por los pasos necesarios para crear una cuenta en el Centro de partners e inscribirse en el programa comercial de Marketplace. El identificador de Microsoft Partner Network (MPN) [se asociará automáticamente](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started) con las ofertas que publique para realizar un seguimiento del impacto en las involucraciones de clientes.
>
> Si no quiere publicar una oferta en Azure Marketplace, puede incorporar clientes manualmente mediante el uso de plantillas de Azure Resource Manager. Para obtener más información, consulte [Onboard a customer to Azure delegated resource management](onboard-customer.md) (Incorporar a un cliente en la administración de recursos delegados de Azure).

Publicar una oferta de servicios administrados es similar a publicar cualquier otro tipo de oferta en Azure Marketplace. Para obtener más información sobre este proceso, consulte [Azure Marketplace and AppSource Publishing Guide](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide) (Guía de publicación de Azure Marketplace y AppSource) y [Administración de las ofertas del Marketplace de Azure y AppSource](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers). También debe revisar [las directivas de certificación de Marketplace comercial](https://docs.microsoft.com/legal/marketplace/certification-policies), en especial la sección [Servicios administrados](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services).

Una vez que un cliente agregue su oferta, podrá delegar uno o varios grupos de recursos o suscripciones específicos, que posteriormente se [incorporarán a la administración de recursos delegados de Azure](#the-customer-onboarding-process). Para poder incorporar una suscripción (o un grupos de recursos de una suscripción), la incorporación se debe autorizar mediante el registro manual del proveedor de recursos **Microsoft.ManagedServices**.

> [!IMPORTANT]
> Cada plan de una oferta de servicios administrados incluye una sección de **detalles del manifiesto**, donde se definen las entidades Azure Active Directory (Azure AD) del inquilino que tendrán acceso a las suscripciones o los grupos de recursos delegados para los clientes que adquieran el plan. Es importante tener en cuenta que cualquier grupo (o entidad de servicio o usuario) que incluya aquí tendrá los mismos permisos para todos los clientes que compren el plan. Para asignar grupos diferentes para que trabajen con cada cliente, tendrá que publicar un plan privado independiente que sea exclusivo para cada cliente.

## <a name="create-your-offer-in-the-cloud-partner-portal"></a>Crear una oferta en Cloud Partner Portal

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En el menú de navegación izquierdo, seleccione **Nueva oferta** y, a continuación, seleccione **Servicios administrados**.
3. Verá una sección **Editor** para la oferta con cuatro partes que se deben rellenar: **Configuración de la oferta**, **Planes**, **Marketplace** y **Soporte técnico**. Siga leyendo para obtener instrucciones sobre cómo completar estas secciones.

## <a name="enter-offer-settings"></a>Especificar los parámetros de la oferta

En la sección **Configuración de la oferta**, debe proporcionar los datos siguientes:

|Campo  |DESCRIPCIÓN  |
|---------|---------|
|**Id. de oferta**     | Identificador único de la oferta (en su perfil de anunciante). Este identificador solo puede contener caracteres alfanuméricos en minúsculas, guiones y caracteres de subrayado, con un máximo de 50 caracteres. Tenga en cuenta que el identificador de la oferta puede estar visible para los clientes en lugares como direcciones URL de producto e informes de facturación. Una vez publicada la oferta, no podrá cambiar este valor.        |
|**Id. del editor**     | Identificador del anunciante que se asociará con la oferta. Si tiene más de un identificador de anunciante, puede seleccionar el que quiera usar para esta oferta.       |
|**Nombre**     | Nombre (hasta 50 caracteres) que los clientes verán para su oferta en Azure Marketplace y en Azure Portal. Use un nombre de marca reconocible que los clientes entiendan: si está promocionando esta oferta a través de su propio sitio web, asegúrese de usar exactamente el mismo nombre aquí.        |

Al acabar, seleccione **Guardar**. Ahora está listo para pasar a la sección **Planes**.

## <a name="create-plans"></a>Creación de planes

Cada oferta debe tener uno o varios planes (a veces denominados SKU). Puede agregar varios planes para admitir diferentes conjuntos de características a precios diferentes o para personalizar un plan específico para un público limitado de clientes específicos. Los clientes pueden ver los planes disponibles para ellos en la oferta principal.

En la sección planes, seleccione **Nuevo plan**. A continuación, escriba un **Id. de plan**. Este identificador solo puede contener caracteres alfanuméricos en minúsculas, guiones y caracteres de subrayado, con un máximo de 50 caracteres. El identificador del plan puede estar visible para los clientes en lugares como direcciones URL de producto e informes de facturación. Una vez publicada la oferta, no podrá cambiar este valor.

### <a name="plan-details"></a>Detalles del plan

Complete las siguientes secciones en **Detalles del plan**:

|Campo  |DESCRIPCIÓN  |
|---------|---------|
|**Título**     | Nombre descriptivo del plan que se va a mostrar. La longitud máxima es de 50 caracteres.        |
|**Resumen**     | Breve descripción del plan que se muestra bajo el título. Tiene una longitud máxima de 100 caracteres.        |
|**Descripción**     | Texto de descripción que proporciona una explicación más detallada del plan.         |
|**Modelo de facturación**     | Aquí se muestran dos modelos de facturación, pero debe elegir **Traiga su propia licencia** para las ofertas de servicios administrados. Esto significa que facturará a sus clientes directamente por los costos relacionados con esta oferta y Microsoft no le aplicará ningún cargo.   |
|**¿Es un plan privado?**     | Indica si la SKU es pública o privada. El valor predeterminado es **No** (pública). Si deja esta selección, el plan no se restringirá a clientes específicos (o a un número determinado de clientes). Una vez publicado un plan público, no se puede cambiar a privado posteriormente. Para que este plan esté disponible solo para clientes específicos, seleccione **Sí**. Al hacerlo, tendrá que identificar a los clientes proporcionando sus identificadores de suscripción. Se pueden especificar de uno en uno (hasta 10 suscripciones) o mediante la carga de un archivo. csv (hasta 20 000 suscripciones). Asegúrese de incluir sus propias suscripciones aquí para poder probar y validar la oferta. Para obtener más información, vea [SKU y planes privados](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus).  |

### <a name="manifest-details"></a>Detalles del manifiesto

Complete la sección de **Detalles del manifiesto** de su plan. Esto crea un manifiesto con información de autorización para administrar los recursos del cliente. Esta información es necesaria para habilitar la administración de recursos delegados de Azure.

> [!NOTE]
> Como se indicó anteriormente, los usuarios y los roles en las entradas de **Autorización** se aplicarán a todos los clientes que compren el plan. Si quiere limitar el acceso a un cliente específico, deberá publicar un plan privado para uso exclusivo.

En primer lugar, proporcione una **Versión** para el manifiesto. Use el formato *n.n.n* (por ejemplo, 1.2.5).

A continuación, escriba su **Id. de inquilino**. Se trata de un GUID asociado con el identificador del inquilino de Azure Active Directory de la organización (es decir, el inquilino en el que trabajará para administrar los recursos de sus clientes). Si no lo tiene a mano, para buscarlo, mantenga el mouse sobre el nombre de la cuenta en la parte superior derecha de Azure Portal o seleccione **Cambiar directorio**.

Por último, agregue una o más entradas de **Autorización** al plan. Las autorizaciones definen las entidades que pueden tener acceso a los recursos y las suscripciones para los clientes que compran el plan. Además, asignan roles que conceden niveles de acceso específicos. Para obtener más detalles sobre los roles admitidos, consulte [Inquilinos, roles y usuarios en escenarios de Azure Lighthouse](../concepts/tenants-users-roles.md).

Para cada **Autorización**, deberá proporcionar la siguiente información. Puede seleccionar **Nueva autorización** tantas veces como sea necesario para agregar más definiciones de roles y usuarios.

  - **Id. de objeto de Azure AD**: identificador de Azure AD de un usuario, un grupo de usuarios o una aplicación a que se concederán determinados permisos (según se describe en la definición de roles) para los recursos de los clientes.
  - **Nombre para mostrar de objeto de Azure AD**: nombre descriptivo para ayudar al cliente a entender el propósito de esta autorización. El cliente verá este nombre al delegar recursos.
  - **Role Definition**: seleccione uno de los roles integrados de Azure AD disponibles en la lista. Este rol determinará los permisos que el usuario del campo **Id. de objeto de Azure AD** tendrá en los recursos de los clientes. Para obtener descripciones de estos roles, consulte los [roles integrados](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) y la [compatibilidad de roles con la administración de recursos delegados de Azure](../concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management).
  - **Roles asignables**: solo se requiere si ha seleccionado Administrador de acceso de usuario en **Definición de roles** para esta autorización. En ese caso, debe agregar uno o varios roles asignables aquí. El usuario del campo **Id. de objeto de Azure AD** podrá asignar estos **Roles asignables** a [identidades administradas](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview), algo necesario para [implementar directivas que pueden corregirse](deploy-policy-remediation.md). Tenga en cuenta que no se aplicará a este usuario ningún otro permiso asociado normalmente al rol Administrador de acceso de usuario. Si no selecciona uno o más roles aquí, el envío no pasará la certificación. (Si no seleccionó Administrador de acceso de usuario para esta definición de roles, este campo no tiene ningún efecto).

> [!TIP]
> En la mayoría de los casos, querrá asignar permisos a una entidad de servicio o un grupo de usuarios de Azure AD, en lugar de a una serie de cuentas de usuario individuales. Esto le permite agregar o quitar el acceso de usuarios individuales sin tener que actualizar y volver a publicar el plan cuando cambien los requisitos de acceso. Para recomendaciones adicionales, consulte [Inquilinos, roles y usuarios en escenarios de Azure Lighthouse](../concepts/tenants-users-roles.md).

Una vez que haya completado la información, puede seleccionar **Nuevo plan** tantas veces como necesite para crear planes adicionales. Cuando haya terminado, seleccione **Guardar** y continúe a la sección **Marketplace**.

## <a name="provide-marketplace-text-and-images"></a>Proporcionar texto e imágenes de Marketplace

En la sección **Marketplace** se proporciona el texto y las imágenes que verán los clientes en Azure Marketplace y Azure Portal.

Complete los campos siguientes en la sección **Información general**:

|Campo  |DESCRIPCIÓN  |
|---------|---------|
|**Título**     |  Título de la oferta, a menudo el nombre largo y formal. Este título se mostrará de forma destacada en Marketplace. La longitud máxima es de 50 caracteres. En la mayoría de los casos, debe coincidir con el **Nombre** que especificó en la sección **Configuración de la oferta**.       |
|**Resumen**     | Breve propósito o función de la oferta. Normalmente, se muestra bajo el título. Tiene una longitud máxima de 100 caracteres.        |
|**Resumen largo**     | Un resumen más largo del propósito o la función de la oferta. Tiene una longitud máxima de 256 caracteres.        |
|**Descripción**     | Más información sobre la oferta. Este campo tiene una longitud máxima de 3000 caracteres y admite el formato HTML sencillo. Debe incluir las palabras "servicio administrado" o "servicios administrados" en alguna parte de la descripción.       |
|**Identificador de marketing**     | Identificador único para direcciones URL. Se usará en las direcciones URL de Marketplace para esta oferta. Por ejemplo, si el identificador de anunciante es *contoso* y el identificador de marketing es *sampleApp*, la dirección URL para la oferta de Azure Marketplace será *https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp* .        |
|**Id. de suscripción de versión preliminar**     | Agregue entre uno y 100 identificadores de suscripción. Los clientes asociados con estas suscripciones podrán ver la oferta en Azure Marketplace antes de que se publique. Le recomendamos que incluya sus propias suscripciones aquí para que pueda obtener una vista previa de cómo aparece la oferta en Azure Marketplace antes de ponerla a disposición de los clientes.  (Los equipos de soporte técnico y de ingeniería de Microsoft también podrán ver su oferta durante este período de versión preliminar).   |
|**Vínculos útiles**     | Direcciones URL relacionadas con la oferta, como documentación, notas de la versión, preguntas más frecuentes, etc.        |
|**Categorías sugeridas (5 como máximo)**     | Una o varias categorías (hasta cinco) que se aplican a su oferta. Estas categorías ayudan a los clientes a detectar su oferta en Azure Marketplace y Azure Portal.        |

En la sección de **artefactos de marketing**, puede cargar los logotipos y otros recursos que se mostrarán con la oferta. Opcionalmente, puede cargar capturas de pantalla o vínculos a vídeos que pueden ayudar a los clientes a comprender la oferta.

Se requieren cuatro tamaños de logotipo: **Pequeño (40 x 40)** , **Medio (90 x 90)** , **Grande (115 x 115)** y **Ancho (255 x 155)** . Siga estas instrucciones para los logotipos:

- El diseño de Azure tiene una paleta de colores simple. Utilice pocos colores primarios y secundarios en el logotipo.
- Los colores del tema del portal son el blanco y el negro. No use estos colores como fondo de los logotipos. Use un color que haga que su logotipo destaque en el portal. Nosotros recomendamos usar colores primarios simples.
- Si usa un fondo transparente, asegúrese de que el logotipo y el texto no son blancos, negros ni azules.
- La apariencia del logotipo debe ser homogénea y evitar degradados. No utilice un fondo degradado en el logotipo.
- No coloque texto en el logotipo, ni siquiera el nombre de su empresa o de la marca.
- Asegúrese de que el logotipo no quede estirado.

El logotipo **Elemento principal (815 x 290)** es opcional, pero se recomienda. Si incluye un logotipo de elemento principal, siga estas instrucciones:

- No incluya ningún texto en el logotipo del elemento principal y asegúrese de dejar 415 píxeles de espacio vacío a la derecha del logotipo. Es necesario para dejar espacio para los elementos de texto que se van a incrustar mediante programación: el nombre para mostrar del anunciante, el título del plan, el resumen largo de la oferta.
- Es posible que el fondo del logotipo del elemento principal no sea blanco, negro ni transparente. Asegúrese de que el color de fondo no sea demasiado claro, ya que el texto incrustado se mostrará en blanco.
- Una vez que publique la oferta con un icono de elemento principal, no podrá quitarla (aunque puede actualizarla con otra versión, si quiere).

En la sección **Administración de clientes potenciales**, puede seleccionar el sistema CRM en el que se almacenarán los clientes potenciales. Tenga en cuenta que, según las [directivas de certificación de servicios administrados](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services), se requiere un **Destino de clientes potenciales**.

Por último, proporcione la **URL de directiva de privacidad** y los **Términos de uso** en la sección **Legal**. También puede especificar si quiere usar el [Contrato estándar](https://docs.microsoft.com/azure/marketplace/standard-contract) o no para esta oferta.

Asegúrese de guardar los cambios antes de continuar a la sección de **Soporte técnico.**

## <a name="add-support-info"></a>Agregar la información de soporte técnico

En la sección **Soporte técnico**, proporcione el nombre, el correo electrónico y el número de teléfono de un contacto de ingeniería y un contacto de atención al cliente. También debe proporcionar las direcciones URL de soporte técnico. Microsoft puede usar esta información cuando necesite ponerse en contacto con usted en relación con cuestiones empresariales y de soporte técnico.

Una vez agregada esta información, seleccione **Guardar.**

## <a name="publish-your-offer"></a>Publicación de la oferta

Cuando haya completado todas las secciones, el siguiente paso es publicar la oferta en Azure Marketplace. Seleccione el botón **Publicar** para iniciar el proceso de publicación de la oferta. Para obtener más información acerca de este proceso, consulte [Publicación de ofertas de Azure Marketplace y AppSource](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-publish-offer).

## <a name="the-customer-onboarding-process"></a>Proceso de incorporación del cliente

Cuando un cliente agregue su oferta, podrá [delegar uno o varios grupos de recursos o suscripciones específicos](view-manage-service-providers.md#delegate-resources) que se incorporarán para la administración de recursos delegada de Azure. Si un cliente ha aceptado una oferta pero aún no ha delegado los recursos, verá una nota en la parte superior de la sección **Ofertas de proveedor** de la página [**Service providers**](view-manage-service-providers.md) (Proveedores de servicio) en Azure Portal. Si un usuario del inquilino del cliente no puede realizar esta delegación, es probable que se deba a que no tiene el rol Propietario en la suscripción. Para buscar usuarios que puedan delegar la suscripción, el usuario puede seleccionar la suscripción en Azure Portal, abrir **Control de acceso (IAM)** y [ver todos los usuarios con el rol Propietario](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#view-roles-and-permissions).

Para que se pueda incorporar una suscripción (o grupos de recursos dentro de una suscripción), la suscripción debe autorizarse para la incorporación; para ello, el proveedor de recursos **Microsoft.ManagedServices** se debe registrar manualmente. Un usuario del inquilino del cliente con el rol de colaborador o de propietario puede hacer esto si sigue los pasos descritos en [Proveedores y tipos de recursos de Azure](../../azure-resource-manager/resource-manager-supported-services.md).

El cliente puede entonces confirmar que la suscripción está preparada para la incorporación de una de las siguientes maneras.

### <a name="azure-portal"></a>Portal de Azure

1. En Azure Portal, seleccione la suscripción.
1. Seleccione **Proveedores de recursos**.
1. Confirme que **Microsoft.ManagedServices** aparece **Registrado**.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Set-AzContext -Subscription <subscriptionId>
Get-AzResourceProvider -ProviderNameSpace 'Microsoft.ManagedServices'
```

Esto debería devolver resultados similares a estos:

```output
ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationDefinitions}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationAssignments}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {operations}
Locations         : {}
```

### <a name="azure-cli"></a>CLI de Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set –subscription <subscriptionId>
az provider show --namespace "Microsoft.ManagedServices" --output table
```

Esto debería devolver resultados similares a estos:

```output
Namespace                  RegistrationState
-------------------------  -------------------
Microsoft.ManagedServices  Registered
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [experiencias de administración entre inquilinos](../concepts/cross-tenant-management-experience.md).
- [Vea y administre sus clientes](view-manage-customers.md) desde **Mis clientes**, en Azure Portal.
