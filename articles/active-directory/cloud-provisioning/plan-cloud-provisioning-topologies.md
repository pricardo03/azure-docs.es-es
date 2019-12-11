---
title: Topologías y escenarios admitidas para el aprovisionamiento en la nube de Azure AD Connect
description: En este tema se describen los requisitos previos y los requisitos de hardware del aprovisionamiento en la nube.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 142974423816b07d754a5425017aedc3195e2f4e
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795379"
---
# <a name="azure-ad-connect-cloud-provisioning-supported-topologies-and-scenarios"></a>Topologías y escenarios admitidas para el aprovisionamiento en la nube de Azure AD Connect
En este artículo se describen diversas topologías locales y de Azure Active Directory (Azure AD) que usan el aprovisionamiento en la nube de Azure AD Connect. En este artículo solo se incluyen las configuraciones y los escenarios admitidos.

> [!IMPORTANT]
> Microsoft no admite la modificación ni el funcionamiento del aprovisionamiento en la nube de Azure AD Connect con configuraciones o acciones distintas a estas que se documentan formalmente. Cualquiera de estas configuraciones o acciones pueden provocar un estado incoherente o incompatible del aprovisionamiento en la nube de Azure AD Connect. Como resultado, Microsoft no ofrece soporte técnico para estas implementaciones.

## <a name="things-to-remember-about-all-scenarios-and-topologies"></a>Aspectos que recordar sobre todos los escenarios y topologías
A continuación se muestra una lista con información que se debe tener en cuenta al seleccionar una solución.

- Los usuarios y grupos se deben identificar de forma única en todos los bosques.
- No se produce coincidencia entre bosques con el aprovisionamiento en la nube.
- Un usuario o grupo solo se debe representar una vez en todos los bosques.
- El delimitador de origen de los objetos se selecciona automáticamente.  Usa ms-DS-ConsistencyGuid si está presente; de lo contrario, se usa ObjectGUID.
- No se puede cambiar el atributo que se usa para el delimitador de origen.



## <a name="multi-forest-single-azure-ad-tenant"></a>Varios bosques, un único inquilino de Azure AD
![Topología para varios bosques y un solo inquilino](media/plan-cloud-provisioning-topologies/multi-forest.png)

La topología más común es la de varios bosques de AD, con uno o varios dominios y un único inquilino de Azure AD.  

## <a name="existing-forest-with-azure-ad-connect-new-forest-with-cloud-provisioning"></a>Bosque existente con Azure AD Connect, nuevo bosque con aprovisionamiento en la nube
![Topología para un único bosque y un solo inquilino](media/plan-cloud-provisioning-topologies/existing-forest-new-forest.png)

Este escenario es una topología similar a la del escenario de varios bosques; sin embargo, en este caso hay un entorno de Azure AD Connect existente y, a continuación, se incorpora un nuevo bosque con aprovisionamiento en la nube de Azure AD Connect.  Para ver un ejemplo de este escenario, consulte [Tutorial: Un bosque existente con un único inquilino de Azure AD](tutorial-existing-forest.md).

## <a name="piloting-azure-ad-connect-cloud-provisioning-in-an-existing-hybrid-ad-forest"></a>Realización de una prueba piloto del aprovisionamiento en la nube de Azure AD Connect en un bosque de AD híbrido existente
![Topología para un único bosque y un solo inquilino](media/plan-cloud-provisioning-topologies/migrate.png) El escenario de prueba piloto implica la existencia de Azure AD Connect y aprovisionamiento en la nube de Azure AD Connect en el mismo bosque y el establecimiento del ámbito de usuarios y grupos en consecuencia. NOTA:  Un objeto debe estar en el ámbito solo en una de las herramientas. 

Para ver un ejemplo de este escenario, consulte [Tutorial: Realización de una prueba piloto del aprovisionamiento en la nube de Azure AD Connect en un bosque de AD sincronizado existente](tutorial-pilot-aadc-aadccp.md).

## <a name="single-forest-single-azure-ad-tenant"></a>Un único bosque, un único inquilino de Azure AD
![Topología para un único bosque y un solo inquilino](media/plan-cloud-provisioning-topologies/single-forest.png)

La topología más sencilla es un único bosque local, con uno o varios dominios y un único inquilino de Azure AD.  Para ver un ejemplo de este escenario, consulte [Tutorial: Un único bosque con un único inquilino de Azure AD](tutorial-single-forest.md).

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)

