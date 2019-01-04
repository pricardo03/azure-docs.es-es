---
title: 'Tutorial: Integración de Azure Active Directory con ProMaster (by Inlogik) | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ProMaster (by Inlogik).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 348dbd37-dc4f-49df-bb90-53d249d456b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: jeedes
ms.openlocfilehash: 871abffc07ee29df388205395110e89a85dd7344
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "53017661"
---
# <a name="tutorial-azure-active-directory-integration-with-promaster-by-inlogik"></a>Tutorial: Integración de Azure Active Directory con ProMaster (by Inlogik)

En este tutorial, aprenderá a integrar ProMaster (by Inlogik) con Azure Active Directory (Azure AD).

Integrar ProMaster (by Inlogik) con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a ProMaster (by Inlogik).
- Puede permitir que los usuarios inicien sesión automáticamente en ProMaster (by Inlogik) (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ProMaster (by Inlogik), necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único de ProMaster (by Inlogik)

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de ProMaster (by Inlogik) desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-promaster-by-inlogik-from-the-gallery"></a>Adición de ProMaster (by Inlogik) desde la galería

Para configurar la integración de ProMaster (by Inlogik) en Azure AD, deberá agregar ProMaster (by Inlogik) desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ProMaster (by Inlogik) desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **ProMaster (by Inlogik)**, seleccione **ProMaster (by Inlogik)** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![ProMaster (by Inlogik) en la lista de resultados](./media/promaster-tutorial/tutorial_promaster_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con ProMaster (by Inlogik) con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ProMaster (by Inlogik) para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ProMaster (by Inlogik).

Para configurar y probar el inicio de sesión único de Azure AD con ProMaster (by Inlogik), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de ProMaster (by Inlogik)](#create-a-promaster-by-inlogik-test-user)**: para tener un homólogo de Britta Simon en ProMaster (by Inlogik) que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación ProMaster (by Inlogik).

**Para configurar el inicio de sesión único de Azure AD con ProMaster (by Inlogik), siga estos pasos:**

1. En la página de integración de la aplicación **ProMaster (by Inlogik)** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/promaster-tutorial/tutorial_promaster_samlbase.png)

3. En la sección **Dominio y direcciones URL de ProMaster (by Inlogik)**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de ProMaster (by Inlogik)](./media/promaster-tutorial/tutorial_promaster_url1.png)

     a. En el cuadro de texto **Identificador**, escriba la dirección URL con el siguiente patrón:

    | |
    | - |-|
    |  `https://secure.inlogik.com/<COMPANYNAME>`|
    | `https://<CUSTOMDOMAIN>/SAMLBASE`|
    | |

    b. En el cuadro de texto **URL de respuesta**, escriba la dirección URL con el siguiente patrón:

    | |
    | - |-|
    | `https://secure.inlogik.com/<COMPANYNAME>/saml/acs`|
    | `https://<CUSTOMDOMAIN>/SAMLBASE/saml/acs`|
    | |

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de ProMaster (by Inlogik)](./media/promaster-tutorial/tutorial_promaster_url2.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL con el siguiente patrón:

    | |
    | - |-|
    | `https://secure.inlogik.com/<COMPANYNAME>/saml/acs `|
    | `https://<CUSTOMDOMAIN>/SAMLBASE/saml/acs`|
    | |

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico del cliente de ProMaster (by Inlogik)](mailto:michael.boldiston@inlogik.com) para obtener estos valores.

5. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/promaster-tutorial/tutorial_promaster_certificate.png)

6. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/promaster-tutorial/tutorial_general_400.png)

7. Para configurar el inicio de sesión único en **ProMaster (by Inlogik)**, hay que enviar la **dirección URL de metadatos de federación de la aplicación** al [equipo de soporte técnico de ProMaster (by Inlogik)](mailto:michael.boldiston@inlogik.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/promaster-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/promaster-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/promaster-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/promaster-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-promaster-by-inlogik-test-user"></a>Creación de un usuario de prueba de ProMaster (by Inlogik)

En esta sección, creará un usuario llamado Britta Simon en ProMaster (by Inlogik). Trabaje con el  [equipo de soporte técnico de ProMaster (by Inlogik)](mailto:michael.boldiston@inlogik.com)  para agregar a los usuarios en la plataforma ProMaster (by Inlogik). Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a ProMaster (by Inlogik) para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a ProMaster (by Inlogik), realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **ProMaster (by Inlogik)**.

    ![El vínculo de ProMaster (by Inlogik) en la lista de aplicaciones](./media/promaster-tutorial/tutorial_promaster_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ProMaster (by Inlogik) en el panel de acceso, debería iniciar sesión automáticamente en su aplicación de ProMaster (by Inlogik).
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/promaster-tutorial/tutorial_general_01.png
[2]: ./media/promaster-tutorial/tutorial_general_02.png
[3]: ./media/promaster-tutorial/tutorial_general_03.png
[4]: ./media/promaster-tutorial/tutorial_general_04.png

[100]: ./media/promaster-tutorial/tutorial_general_100.png

[200]: ./media/promaster-tutorial/tutorial_general_200.png
[201]: ./media/promaster-tutorial/tutorial_general_201.png
[202]: ./media/promaster-tutorial/tutorial_general_202.png
[203]: ./media/promaster-tutorial/tutorial_general_203.png

