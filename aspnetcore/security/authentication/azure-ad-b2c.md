---
title: Authentification cloud avec Azure Active Directory B2C
author: camsoper
description: "Découvrez comment configurer l’authentification d’Azure Active Directory B2C avec ASP.NET Core."
ms.author: casoper
manager: wpickett
ms.date: 01/12/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/azure-ad-b2c
custom: mvc
ms.openlocfilehash: 5c4716022c61e33b0301fa0077f911dcc4b3628c
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c"></a><span data-ttu-id="2214a-103">Authentification cloud avec Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-103">Cloud authentication with Azure Active Directory B2C</span></span>

<span data-ttu-id="2214a-104">Auteur : [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="2214a-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="2214a-105">[Azure B2C Active Directory](/azure/active-directory-b2c/active-directory-b2c-overview) (B2C Active Directory de Azure) est une solution de gestion des identités de cloud pour vos applications web et mobiles.</span><span class="sxs-lookup"><span data-stu-id="2214a-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for your web and mobile apps.</span></span> <span data-ttu-id="2214a-106">Le service fournit l’authentification pour les applications hébergées dans le cloud et locales.</span><span class="sxs-lookup"><span data-stu-id="2214a-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="2214a-107">Types d’authentification incluent les comptes individuels, les comptes de réseau social et fédéré de comptes d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="2214a-107">Authentication types include include individual accounts, social network accounts, and federated enterprise accounts.</span></span>  <span data-ttu-id="2214a-108">En outre, Azure AD B2C peut fournir l’authentification multifacteur avec une configuration minimale.</span><span class="sxs-lookup"><span data-stu-id="2214a-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

<span data-ttu-id="2214a-109">Dans ce didacticiel, vous devez savoir comment :</span><span class="sxs-lookup"><span data-stu-id="2214a-109">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2214a-110">Créer un client Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-110">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="2214a-111">Inscrire une application dans Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-111">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="2214a-112">Utiliser Visual Studio pour créer une application de web ASP.NET Core configurée pour utiliser le locataire Azure AD B2C pour l’authentification</span><span class="sxs-lookup"><span data-stu-id="2214a-112">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="2214a-113">Configurer des stratégies de contrôle du comportement du client Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-113">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2214a-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="2214a-114">Prerequisites</span></span>

<span data-ttu-id="2214a-115">Les éléments suivants sont requis pour cette procédure pas à pas :</span><span class="sxs-lookup"><span data-stu-id="2214a-115">The following are required for this walkthrough:</span></span>

* <span data-ttu-id="2214a-116">[Abonnement Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).</span><span class="sxs-lookup"><span data-stu-id="2214a-116">[Microsoft Azure subscription](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).</span></span> 
* <span data-ttu-id="2214a-117">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (toute édition)</span><span class="sxs-lookup"><span data-stu-id="2214a-117">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="2214a-118">Créer le client Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-118">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="2214a-119">Créer un client Azure Active Directory B2C [comme décrit dans la documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span><span class="sxs-lookup"><span data-stu-id="2214a-119">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="2214a-120">Lorsque vous y êtes invité, associant le locataire d’un abonnement Azure est facultatif pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="2214a-120">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="2214a-121">Inscrire l’application dans Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-121">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="2214a-122">Dans le locataire Azure AD B2C nouvellement créé, inscrire votre application à l’aide de [les étapes décrites dans la documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) sous le **inscrire une application web** section.</span><span class="sxs-lookup"><span data-stu-id="2214a-122">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="2214a-123">Arrêter à la **créer une question secrète du client web application** section.</span><span class="sxs-lookup"><span data-stu-id="2214a-123">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="2214a-124">Une clé secrète client n’est pas nécessaire pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="2214a-124">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="2214a-125">Utilisez les valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="2214a-125">Use the following values:</span></span>

| <span data-ttu-id="2214a-126">Paramètre</span><span class="sxs-lookup"><span data-stu-id="2214a-126">Setting</span></span>                       | <span data-ttu-id="2214a-127">Value</span><span class="sxs-lookup"><span data-stu-id="2214a-127">Value</span></span>                     | <span data-ttu-id="2214a-128">Notes</span><span class="sxs-lookup"><span data-stu-id="2214a-128">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2214a-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="2214a-129">**Name**</span></span>                      | <span data-ttu-id="2214a-130">*\<nom de l’application\>*</span><span class="sxs-lookup"><span data-stu-id="2214a-130">*\<app name\>*</span></span>            | <span data-ttu-id="2214a-131">Entrez un **nom** pour l’application qui décrivent votre application aux consommateurs.</span><span class="sxs-lookup"><span data-stu-id="2214a-131">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="2214a-132">**Inclure l’application web ou des API**</span><span class="sxs-lookup"><span data-stu-id="2214a-132">**Include web app / web API**</span></span> | <span data-ttu-id="2214a-133">Oui</span><span class="sxs-lookup"><span data-stu-id="2214a-133">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="2214a-134">**Autoriser les flux implicites**</span><span class="sxs-lookup"><span data-stu-id="2214a-134">**Allow implicit flow**</span></span>       | <span data-ttu-id="2214a-135">Oui</span><span class="sxs-lookup"><span data-stu-id="2214a-135">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="2214a-136">**URL de réponse**</span><span class="sxs-lookup"><span data-stu-id="2214a-136">**Reply URL**</span></span>                 | `https://localhost:44300` | <span data-ttu-id="2214a-137">URL de réponse sont les points de terminaison où Azure AD B2C retourne tout jeton de demande de votre application.</span><span class="sxs-lookup"><span data-stu-id="2214a-137">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="2214a-138">Visual Studio fournit l’URL de réponse à utiliser.</span><span class="sxs-lookup"><span data-stu-id="2214a-138">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="2214a-139">Pour l’instant, entrez `https://localhost:44300` pour remplir le formulaire.</span><span class="sxs-lookup"><span data-stu-id="2214a-139">For now, enter `https://localhost:44300` to complete the form.</span></span> |
| <span data-ttu-id="2214a-140">**URI ID d’application**</span><span class="sxs-lookup"><span data-stu-id="2214a-140">**App ID URI**</span></span>                | <span data-ttu-id="2214a-141">Laissez vide</span><span class="sxs-lookup"><span data-stu-id="2214a-141">Leave blank</span></span>               | <span data-ttu-id="2214a-142">Non requis pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="2214a-142">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="2214a-143">**Inclure le client natif**</span><span class="sxs-lookup"><span data-stu-id="2214a-143">**Include native client**</span></span>     | <span data-ttu-id="2214a-144">Non</span><span class="sxs-lookup"><span data-stu-id="2214a-144">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="2214a-145">Si la configuration d’une URL de réponse non-localhost, vous devez connaître le [contraintes sur ce qui est autorisé dans la liste des URL de réponse](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span><span class="sxs-lookup"><span data-stu-id="2214a-145">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="2214a-146">Une fois que l’application est enregistrée, la liste des applications dans le client s’affiche.</span><span class="sxs-lookup"><span data-stu-id="2214a-146">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="2214a-147">Sélectionnez l’application qui a été enregistrée.</span><span class="sxs-lookup"><span data-stu-id="2214a-147">Select the app that was just registered.</span></span> <span data-ttu-id="2214a-148">Sélectionnez le **copie** icône à droite de la **ID d’Application** champ pour copier l’ID d’Application dans le Presse-papiers.</span><span class="sxs-lookup"><span data-stu-id="2214a-148">Select the **Copy** icon to the right of the **Application ID** field to copy the Application ID to the clipboard.</span></span>

<span data-ttu-id="2214a-149">Rien de plus d’informations peuvent être configurés dans le locataire Azure AD B2C à ce stade, mais laissez la fenêtre de navigateur ouverte.</span><span class="sxs-lookup"><span data-stu-id="2214a-149">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="2214a-150">Il existe davantage de configuration après la création de l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2214a-150">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="2214a-151">Créer une application ASP.NET Core dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2214a-151">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="2214a-152">Le modèle d’Application Web de Visual Studio peut être configuré pour utiliser le locataire Azure AD B2C pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="2214a-152">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="2214a-153">Dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="2214a-153">In Visual Studio:</span></span>

1. <span data-ttu-id="2214a-154">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2214a-154">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="2214a-155">Sélectionnez **Application Web** à partir de la liste des modèles.</span><span class="sxs-lookup"><span data-stu-id="2214a-155">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="2214a-156">Sélectionnez le **modifier l’authentification** bouton.</span><span class="sxs-lookup"><span data-stu-id="2214a-156">Select the **Change Authentication** button.</span></span>
    
    ![Bouton de modifier l’authentification](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="2214a-158">Dans le **modifier l’authentification** boîte de dialogue, sélectionnez **comptes d’utilisateur individuels**, puis sélectionnez **se connecter à un magasin d’utilisateur existant dans le cloud** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="2214a-158">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![Boîte de dialogue Modifier l’authentification](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="2214a-160">Remplissez le formulaire avec les valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="2214a-160">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="2214a-161">Paramètre</span><span class="sxs-lookup"><span data-stu-id="2214a-161">Setting</span></span>                       | <span data-ttu-id="2214a-162">Value</span><span class="sxs-lookup"><span data-stu-id="2214a-162">Value</span></span>                                             |
    |-------------------------------|---------------------------------------------------|
    | <span data-ttu-id="2214a-163">**Nom de domaine**</span><span class="sxs-lookup"><span data-stu-id="2214a-163">**Domain Name**</span></span>               | <span data-ttu-id="2214a-164">*\<le nom de domaine de votre client B2C\>*</span><span class="sxs-lookup"><span data-stu-id="2214a-164">*\<the domain name of your B2C tenant\>*</span></span>          |
    | <span data-ttu-id="2214a-165">**ID d’application**</span><span class="sxs-lookup"><span data-stu-id="2214a-165">**Application ID**</span></span>            | <span data-ttu-id="2214a-166">*\<Collez l’ID d’Application à partir du Presse-papiers\>*</span><span class="sxs-lookup"><span data-stu-id="2214a-166">*\<paste the Application ID from the clipboard\>*</span></span> |
    | <span data-ttu-id="2214a-167">**Chemin d’accès de rappel**</span><span class="sxs-lookup"><span data-stu-id="2214a-167">**Callback Path**</span></span>             | <span data-ttu-id="2214a-168">*\<la valeur par défaut\>*</span><span class="sxs-lookup"><span data-stu-id="2214a-168">*\<use the default value\>*</span></span>                       |
    | <span data-ttu-id="2214a-169">**Stratégie d’inscription ou de la connexion**</span><span class="sxs-lookup"><span data-stu-id="2214a-169">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                    |
    | <span data-ttu-id="2214a-170">**Stratégie de réinitialisation du mot de passe**</span><span class="sxs-lookup"><span data-stu-id="2214a-170">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                      |
    | <span data-ttu-id="2214a-171">**Modifier la stratégie de profil**</span><span class="sxs-lookup"><span data-stu-id="2214a-171">**Edit profile policy**</span></span>       | <span data-ttu-id="2214a-172">*\<Laissez vide\>*</span><span class="sxs-lookup"><span data-stu-id="2214a-172">*\<leave blank\>*</span></span>                                 |

    <span data-ttu-id="2214a-173">Sélectionnez le **copie** situé en regard **réponse URI** pour copier l’URI de réponse dans le Presse-papiers.</span><span class="sxs-lookup"><span data-stu-id="2214a-173">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="2214a-174">Sélectionnez **OK** pour fermer la **modifier l’authentification** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="2214a-174">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="2214a-175">Sélectionnez **OK** pour créer l’application web.</span><span class="sxs-lookup"><span data-stu-id="2214a-175">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="2214a-176">Terminer l’inscription d’une application B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-176">Finish the B2C app registration</span></span>

<span data-ttu-id="2214a-177">Revenir à la fenêtre de navigateur avec les propriétés de l’application B2C toujours ouvertes.</span><span class="sxs-lookup"><span data-stu-id="2214a-177">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="2214a-178">Modifier la fichier temporaire **URL de réponse** spécifié précédemment pour la valeur copiée à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2214a-178">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="2214a-179">Sélectionnez **enregistrer** en haut de la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="2214a-179">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="2214a-180">Si vous n’avez pas de copier l’URL de réponse, utilisez l’adresse SSL à partir de l’onglet débogage dans les propriétés du projet web et ajouter la **CallbackPath** valeur *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2214a-180">If you didn't copy the Reply URL, use the SSL address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="2214a-181">Configurer des stratégies</span><span class="sxs-lookup"><span data-stu-id="2214a-181">Configure policies</span></span>

<span data-ttu-id="2214a-182">Utilisez les étapes de la documentation d’Azure AD B2C à [créer une stratégie d’inscription ou connectez-vous](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), puis [créer une stratégie de réinitialisation de mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="2214a-182">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="2214a-183">Utilisez les valeurs de l’exemple fournis dans la documentation de **fournisseurs d’identité**, **des attributs d’abonnement**, et **revendications de l’Application**.</span><span class="sxs-lookup"><span data-stu-id="2214a-183">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="2214a-184">À l’aide de la **exécuter maintenant** pour tester les stratégies, comme décrit dans la documentation est facultative.</span><span class="sxs-lookup"><span data-stu-id="2214a-184">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="2214a-185">Vérifiez les noms de stratégie sont exactement comme décrit dans la documentation, ces stratégies ont été utilisés dans le **modifier l’authentification** boîte de dialogue dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2214a-185">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="2214a-186">Les noms de stratégie peuvent être vérifiés dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2214a-186">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2214a-187">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="2214a-187">Run the app</span></span>

<span data-ttu-id="2214a-188">Dans Visual Studio, appuyez sur **F5** pour générer et exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="2214a-188">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="2214a-189">Après le lancement de l’application web, sélectionnez **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="2214a-189">After the web app launches, select **Sign in**.</span></span>

![Connectez-vous à l’application](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="2214a-191">Le navigateur redirige vers le locataire Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2214a-191">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="2214a-192">Connectez-vous avec un compte existant (si un a été créé les stratégies de test) ou sélectionnez **s’inscrire maintenant** pour créer un nouveau compte.</span><span class="sxs-lookup"><span data-stu-id="2214a-192">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="2214a-193">Le **votre mot de passe oublié ?** lien est utilisé pour réinitialiser un mot de passe oublié.</span><span class="sxs-lookup"><span data-stu-id="2214a-193">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Connexion de Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="2214a-195">Une fois connecté avec succès, le navigateur effectue une redirection vers l’application web.</span><span class="sxs-lookup"><span data-stu-id="2214a-195">After successfully signing in, the browser redirects to the web app.</span></span>

![Opération réussie](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="2214a-197">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="2214a-197">Next steps</span></span>

<span data-ttu-id="2214a-198">Dans ce didacticiel, vous sera appris comment :</span><span class="sxs-lookup"><span data-stu-id="2214a-198">In this tutorial, you will learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2214a-199">Créer un client Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-199">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="2214a-200">Inscrire une application dans Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-200">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="2214a-201">Utiliser Visual Studio pour créer une Application de Web ASP.NET Core configuré pour utiliser le locataire Azure AD B2C pour l’authentification</span><span class="sxs-lookup"><span data-stu-id="2214a-201">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="2214a-202">Configurer des stratégies de contrôle du comportement du client Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2214a-202">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="2214a-203">Maintenant que l’application ASP.NET Core est configurée pour utiliser Azure AD B2C pour l’authentification, le [attribut Authorize](xref:security/authorization/simple) peut être utilisé pour sécuriser votre application.</span><span class="sxs-lookup"><span data-stu-id="2214a-203">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="2214a-204">Continuer à développer votre application par l’apprentissage à :</span><span class="sxs-lookup"><span data-stu-id="2214a-204">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="2214a-205">[Personnaliser l’interface utilisateur de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="2214a-205">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="2214a-206">[Configurer les exigences de complexité de mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span><span class="sxs-lookup"><span data-stu-id="2214a-206">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="2214a-207">[Activer l’authentification multifacteur](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span><span class="sxs-lookup"><span data-stu-id="2214a-207">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="2214a-208">Configurer les fournisseurs d’identité supplémentaires, telles que [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)et d’autres.</span><span class="sxs-lookup"><span data-stu-id="2214a-208">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="2214a-209">[Utiliser l’API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) pour récupérer des informations supplémentaires, telles que l’appartenance au groupe, du locataire Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2214a-209">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>