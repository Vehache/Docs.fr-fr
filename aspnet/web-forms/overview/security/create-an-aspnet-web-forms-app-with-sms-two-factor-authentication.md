---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: "Créer un site Web ASP.NET application formulaires avec authentification à deux facteurs SMS (c#) | Documents Microsoft"
author: Erikre
description: "Ce didacticiel vous montre comment créer une application Web Forms ASP.NET avec l’authentification à deux facteurs. Ce didacticiel a été conçu pour compléter le didacticiel intitulé Cr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 92ab5f2d7a9a29089f3d340849e229d015613509
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a><span data-ttu-id="651f9-104">Créer un site Web ASP.NET application formulaires avec authentification à deux facteurs SMS (c#)</span><span class="sxs-lookup"><span data-stu-id="651f9-104">Create an ASP.NET Web Forms app with SMS Two-Factor Authentication (C#)</span></span>
====================
<span data-ttu-id="651f9-105">Par [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="651f9-105">by [Erik Reitan](https://github.com/Erikre)</span></span>

[<span data-ttu-id="651f9-106">Télécharger l’application de formulaires Web ASP.NET avec la messagerie et d’authentification à deux facteurs SMS</span><span class="sxs-lookup"><span data-stu-id="651f9-106">Download ASP.NET Web Forms App with Email and SMS Two-Factor Authentication</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> <span data-ttu-id="651f9-107">Ce didacticiel vous montre comment créer une application Web Forms ASP.NET avec l’authentification à deux facteurs.</span><span class="sxs-lookup"><span data-stu-id="651f9-107">This tutorial shows you how to build an ASP.NET Web Forms app with Two-Factor Authentication.</span></span> <span data-ttu-id="651f9-108">Ce didacticiel a été conçu pour compléter le didacticiel intitulé [créer une application Web Forms ASP.NET sécurisée avec l’enregistrement de l’utilisateur, par courrier électronique de confirmation et le mot de passe de la réinitialisation](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).</span><span class="sxs-lookup"><span data-stu-id="651f9-108">This tutorial was designed to complement the tutorial titled [Create a secure ASP.NET Web Forms app with user registration, email confirmation and password reset](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).</span></span> <span data-ttu-id="651f9-109">En outre, ce didacticiel est basé sur de Rick Anderson [didacticiel MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="651f9-109">In addition, this tutorial was based on Rick Anderson's [MVC tutorial](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>


## <a name="introduction"></a><span data-ttu-id="651f9-110">Introduction</span><span class="sxs-lookup"><span data-stu-id="651f9-110">Introduction</span></span>

<span data-ttu-id="651f9-111">Ce didacticiel vous guide à travers les étapes requises pour créer une application Web Forms ASP.NET qui prend en charge l’authentification à deux facteurs à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="651f9-111">This tutorial guides you through the steps required to create an ASP.NET Web Forms application that supports Two-Factor Authentication using Visual Studio.</span></span> <span data-ttu-id="651f9-112">Authentification à deux facteurs est une étape d’authentification utilisateur supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="651f9-112">Two-Factor Authentication is an extra user authentication step.</span></span> <span data-ttu-id="651f9-113">Cette étape supplémentaire génère un unique confidentiel (PIN) pendant la connexion.</span><span class="sxs-lookup"><span data-stu-id="651f9-113">This extra step generates a unique personal identification number (PIN) during sign-in.</span></span> <span data-ttu-id="651f9-114">Le code confidentiel est généralement envoyé à l’utilisateur par courrier électronique ou SMS.</span><span class="sxs-lookup"><span data-stu-id="651f9-114">The PIN is commonly sent to the user as an email or SMS message.</span></span> <span data-ttu-id="651f9-115">L’utilisateur de votre application passe ensuite à la marque comme une mesure supplémentaire d’authentification pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="651f9-115">The user of your app then enters the PIN as an extra authentication measure when signing-in.</span></span>

### <a name="tutorial-tasks-and-information"></a><span data-ttu-id="651f9-116">Tâches de didacticiel et les informations sur :</span><span class="sxs-lookup"><span data-stu-id="651f9-116">Tutorial Tasks and Information:</span></span>

- [<span data-ttu-id="651f9-117">Créer une application de formulaires Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="651f9-117">Create an ASP.NET Web Forms App</span></span>](#createWebForms)
- [<span data-ttu-id="651f9-118">Le programme d’installation de SMS et l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="651f9-118">Setup SMS and Two-Factor Authentication</span></span>](#SMS)
- [<span data-ttu-id="651f9-119">Activer l’authentification à deux facteurs pour l’utilisateur inscrit</span><span class="sxs-lookup"><span data-stu-id="651f9-119">Enable Two-Factor Authentication for Registered User</span></span>](#use2FA)
- [<span data-ttu-id="651f9-120">Ressources supplémentaires pour MSBuild</span><span class="sxs-lookup"><span data-stu-id="651f9-120">Additional Resources</span></span>](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a><span data-ttu-id="651f9-121">Créer une application de formulaires Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="651f9-121">Create an ASP.NET Web Forms App</span></span>

<span data-ttu-id="651f9-122">Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="651f9-122">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="651f9-123">Installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure ainsi.</span><span class="sxs-lookup"><span data-stu-id="651f9-123">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher as well.</span></span> <span data-ttu-id="651f9-124">En outre, vous devrez créer un [Twilio](https://www.twilio.com/try-twilio) de compte, comme expliqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="651f9-124">Also, you will need to create a [Twilio](https://www.twilio.com/try-twilio) account, as explained below.</span></span>

> [!NOTE]
> <span data-ttu-id="651f9-125">Important : Vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure pour effectuer ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="651f9-125">Important: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="651f9-126">Créer un projet (**fichier**  - &gt; **nouveau projet**) et sélectionnez le **Application Web ASP.NET** modèle, ainsi que le .NET Framework la version 4.5.2 à partir de la **nouveau projet** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="651f9-126">Create a new project (**File** -&gt; **New Project**) and select the **ASP.NET Web Application** template along with the .NET Framework version 4.5.2 from the **New Project** dialog box.</span></span>
2. <span data-ttu-id="651f9-127">À partir de la **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **Web Forms** modèle.</span><span class="sxs-lookup"><span data-stu-id="651f9-127">From the **New ASP.NET Project** dialog box, select the **Web Forms** template.</span></span> <span data-ttu-id="651f9-128">Laissez l’authentification par défaut en tant que **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="651f9-128">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="651f9-129">Ensuite, cliquez sur **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="651f9-129">Then, click **OK** to create the new project.</span></span>  
    <span data-ttu-id="651f9-130">![Boîte de dialogue Nouveau projet ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="651f9-130">![New ASP.NET Project dialog box](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)</span></span>
3. <span data-ttu-id="651f9-131">Activer Secure Sockets Layer (SSL) pour le projet.</span><span class="sxs-lookup"><span data-stu-id="651f9-131">Enable Secure Sockets Layer (SSL) for the project.</span></span> <span data-ttu-id="651f9-132">Suivez les étapes disponibles dans le **activer SSL pour le projet** section de la [prise en main de la série de didacticiels Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).</span><span class="sxs-lookup"><span data-stu-id="651f9-132">Follow the steps available in the **Enable SSL for the Project** section of the [Getting Started with Web Forms tutorial series](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).</span></span>
4. <span data-ttu-id="651f9-133">Dans Visual Studio, ouvrez le **Package Manager Console** (**outils**  - &gt; **le Gestionnaire de Package NuGet**  - &gt; **Package Manager Console**) et entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="651f9-133">In Visual Studio, open the **Package Manager Console** (**Tools** -&gt; **NuGet Package Manger** -&gt; **Package Manager Console**), and enter the following command:</span></span>  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a><span data-ttu-id="651f9-134">Le programme d’installation de SMS et l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="651f9-134">Setup SMS and Two-Factor Authentication</span></span>

<span data-ttu-id="651f9-135">Ce didacticiel utilise Twilio, mais vous pouvez utiliser n’importe quel fournisseur SMS.</span><span class="sxs-lookup"><span data-stu-id="651f9-135">This tutorial uses Twilio, but you can use any SMS provider.</span></span>

1. <span data-ttu-id="651f9-136">Créer un [Twilio](https://www.twilio.com/try-twilio) compte.</span><span class="sxs-lookup"><span data-stu-id="651f9-136">Create a [Twilio](https://www.twilio.com/try-twilio) account.</span></span>
2. <span data-ttu-id="651f9-137">À partir de la **tableau de bord** onglet de votre compte Twilio, copie le **SID de compte** et **jeton d’authentification.**</span><span class="sxs-lookup"><span data-stu-id="651f9-137">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth Token.**</span></span> <span data-ttu-id="651f9-138">Vous allez ajouter les à votre application plus tard.</span><span class="sxs-lookup"><span data-stu-id="651f9-138">You will add them to your app later.</span></span>
3. <span data-ttu-id="651f9-139">À partir de la **numéros** , onglet de la copie de votre Twilio **numéro de téléphone** également.</span><span class="sxs-lookup"><span data-stu-id="651f9-139">From the **Numbers** tab, copy your Twilio **phone number** as well.</span></span>
4. <span data-ttu-id="651f9-140">Rendre le Twilio **SID de compte**, **Auth jeton** et **numéro de téléphone** disponibles pour l’application.</span><span class="sxs-lookup"><span data-stu-id="651f9-140">Make the Twilio **Account SID**, **Auth Token** and **phone number** available to the app.</span></span> <span data-ttu-id="651f9-141">Pour simplifier les choses, vous souhaitez stocker ces valeurs dans le *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="651f9-141">To keep things simple you will store these values in the *web.config* file.</span></span> <span data-ttu-id="651f9-142">Lorsque vous déployez dans Azure, vous pouvez stocker les valeurs de manière sécurisée dans le **appSettings** section sur le site web de l’onglet Configurer. En outre, lorsque vous ajoutez le numéro de téléphone, utiliser uniquement des chiffres.</span><span class="sxs-lookup"><span data-stu-id="651f9-142">When you deploy to Azure, you can store the values securely in the **appSettings** section on the web site configure tab. Also, when adding the phone number, only use numbers.</span></span>   
 <span data-ttu-id="651f9-143">Notez que vous pouvez également ajouter des informations d’identification SendGrid.</span><span class="sxs-lookup"><span data-stu-id="651f9-143">Notice that you can also add SendGrid credentials.</span></span> <span data-ttu-id="651f9-144">SendGrid est un service de notification par courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="651f9-144">SendGrid is an email notification service.</span></span> <span data-ttu-id="651f9-145">Pour plus d’informations sur l’activation de SendGrid, consultez la section de « Raccordement de SendGrid » du didacticiel intitulée [créer une application sécurisée de formulaires ASP.NET Web avec l’enregistrement de l’utilisateur, par courrier électronique de confirmation et le mot de passe de la réinitialisation.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)</span><span class="sxs-lookup"><span data-stu-id="651f9-145">For details about how to enable SendGrid, see the 'Hook Up SendGrid' section of the tutorial titled [Create a Secure ASP.NET Web Forms App with user registration, email confirmation and password reset.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)</span></span>

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > <span data-ttu-id="651f9-146">Sécurité - magasin jamais des données sensibles dans votre code source.</span><span class="sxs-lookup"><span data-stu-id="651f9-146">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="651f9-147">Dans cet exemple, le compte et les informations d’identification sont stockées dans le **appSettings** section de la *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="651f9-147">In this example, the account and credentials are stored in the **appSettings** section of the *Web.config* file.</span></span> <span data-ttu-id="651f9-148">Sur Azure, vous pouvez stocker en toute sécurité des ces valeurs sur le  **[configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  onglet dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="651f9-148">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="651f9-149">Pour plus d’informations, consultez la rubrique de Rick Anderson [meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure](https://go.microsoft.com/fwlink/?LinkId=513141).</span><span class="sxs-lookup"><span data-stu-id="651f9-149">For related information, see Rick Anderson's topic titled [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](https://go.microsoft.com/fwlink/?LinkId=513141).</span></span>
5. <span data-ttu-id="651f9-150">Configurer le `SmsService` classe dans le *application\_Start\IdentityConfig.cs* en jaune des modifications de fichier en effectuant ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="651f9-150">Configure the `SmsService` class in the *App\_Start\IdentityConfig.cs* file by making the following changes highlighted in yellow:</span></span> 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. <span data-ttu-id="651f9-151">Ajoutez le code suivant `using` instructions au début de la *IdentityConfig.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="651f9-151">Add the following `using` statements to the beginning of the *IdentityConfig.cs* file:</span></span> 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. <span data-ttu-id="651f9-152">Mise à jour la *Account/Manage.aspx* fichier en supprimant les lignes mises en surbrillance en jaune :</span><span class="sxs-lookup"><span data-stu-id="651f9-152">Update the *Account/Manage.aspx* file by removing the lines highlighted in yellow:</span></span>  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. <span data-ttu-id="651f9-153">Dans le `Page_Load` Gestionnaire de la *Manage.aspx.cs* code-behind, supprimez les commentaires de la ligne de code mise en surbrillance en jaune afin qu’il apparaisse comme suit :</span><span class="sxs-lookup"><span data-stu-id="651f9-153">In the `Page_Load` handler of the *Manage.aspx.cs* code-behind, uncomment the line of code highlighted in yellow so that it appears as follows:</span></span> 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. <span data-ttu-id="651f9-154">Dans le code-behind de *compte*/*TwoFactorAuthenticationSignIn.aspx.cs*, mettre à jour le `Page_Load` gestionnaire en ajoutant le code suivant est mis en surbrillance en jaune :</span><span class="sxs-lookup"><span data-stu-id="651f9-154">In the codebehind of *Account*/*TwoFactorAuthenticationSignIn.aspx.cs*, update the `Page_Load` handler by adding the following code highlighted in yellow:</span></span> 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

 <span data-ttu-id="651f9-155">En rendant le code ci-dessus à modifier, DropDownList « Fournisseurs » contenant les options d’authentification ne rétablit pas à la première valeur.</span><span class="sxs-lookup"><span data-stu-id="651f9-155">By making the above code change, the "Providers" DropDownList containing the authentication options will not be reset to the first value.</span></span> <span data-ttu-id="651f9-156">Ainsi, l’utilisateur de sélectionner correctement de toutes les options à utiliser lors de l’authentification, pas seulement la première.</span><span class="sxs-lookup"><span data-stu-id="651f9-156">This will allow the user to successfully select all options to use when authenticating, not just the first.</span></span>
10. <span data-ttu-id="651f9-157">Dans **l’Explorateur de solutions**, avec le bouton droit *Default.aspx* et sélectionnez **définir comme Page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="651f9-157">In **Solution Explorer**, right-click *Default.aspx* and select **Set As Start Page**.</span></span>
11. <span data-ttu-id="651f9-158">En testant votre application, tout d’abord créer l’application (**Ctrl**+**MAJ**+**B**), puis exécutez l’application (**F5**) et Sélectionnez **inscrire** pour créer un nouveau compte d’utilisateur ou sélectionnez **connecter** si le compte d’utilisateur a déjà été inscrit.</span><span class="sxs-lookup"><span data-stu-id="651f9-158">By testing your app, first build the app (**Ctrl**+**Shift**+**B**) and then run the app (**F5**) and either select **Register** to create a new user account or select **Log in** if the user account has already been registered.</span></span>
12. <span data-ttu-id="651f9-159">Une fois que vous (en tant que l’utilisateur) avez connecté, cliquez sur l’ID d’utilisateur (adresse électronique) dans la barre de navigation pour afficher les **gérer le compte** page (Manage.aspx).</span><span class="sxs-lookup"><span data-stu-id="651f9-159">Once you (as the user) have logged in, click on the User ID (email address) in the navigation bar to display the **Manage Account** page (Manage.aspx).</span></span>  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. <span data-ttu-id="651f9-160">Cliquez sur **ajouter** regard **numéro de téléphone** sur la **gérer le compte** page.</span><span class="sxs-lookup"><span data-stu-id="651f9-160">Click **Add** next to **Phone Number** on the **Manage Account** page.</span></span>  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. <span data-ttu-id="651f9-161">Ajouter le numéro de téléphone où vous (en tant que l’utilisateur) souhaitez recevoir des messages SMS (SMS) et cliquez sur le **Submit** bouton.</span><span class="sxs-lookup"><span data-stu-id="651f9-161">Add the phone number where you (as the user) would like to receive SMS messages (text messages) and click the **Submit** button.</span></span>   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
 <span data-ttu-id="651f9-162">À ce stade, l’application utilisera les informations d’identification à partir de la *Web.config* contacter Twilio.</span><span class="sxs-lookup"><span data-stu-id="651f9-162">At this point, the app will use the credentials from the *Web.config* to contact Twilio.</span></span> <span data-ttu-id="651f9-163">Vous recevrez un message SMS (SMS) au numéro de téléphone associé au compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="651f9-163">A SMS message (text message) will be sent to the phone associated with the user account.</span></span> <span data-ttu-id="651f9-164">Vous pouvez vérifier que le message de Twilio a été envoyé en consultant le tableau de bord Twilio.</span><span class="sxs-lookup"><span data-stu-id="651f9-164">You can verify that the Twilio message was sent by viewing the Twilio dashboard.</span></span>
15. <span data-ttu-id="651f9-165">En quelques secondes, le téléphone associé au compte d’utilisateur obtenez un message texte contenant le code de vérification.</span><span class="sxs-lookup"><span data-stu-id="651f9-165">In a few seconds, the phone associated with the user account will get a text message containing the verification code.</span></span> <span data-ttu-id="651f9-166">Entrez le code de vérification, appuyez sur **Submit**.</span><span class="sxs-lookup"><span data-stu-id="651f9-166">Enter the verification code and press **Submit**.</span></span>  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a><span data-ttu-id="651f9-167">Activer l’authentification à deux facteurs pour un utilisateur inscrit</span><span class="sxs-lookup"><span data-stu-id="651f9-167">Enable Two-Factor Authentication for a Registered User</span></span>

<span data-ttu-id="651f9-168">À ce stade, vous avez activé l’authentification à deux facteurs pour votre application.</span><span class="sxs-lookup"><span data-stu-id="651f9-168">At this point, you have enabled two-factor authentication for your app.</span></span> <span data-ttu-id="651f9-169">Pour un utilisateur à utiliser l’authentification à deux facteurs, ils peuvent simplement modifier leurs paramètres à l’aide de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="651f9-169">For a user to use two-factor authentication, they can simply change their settings using the UI.</span></span> 

1. <span data-ttu-id="651f9-170">En tant qu’utilisateur de votre application vous pouvez activer l’authentification à deux facteurs pour votre compte en cliquant sur l’ID d’utilisateur (alias de messagerie) dans la barre de navigation pour afficher les **gérer le compte** page. Ensuite, cliquez sur le **activer** lien pour activer l’authentification à deux facteurs pour le compte.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="651f9-170">As a user of your app you can enable two-factor authentication for your specific account by clicking on the user ID (email alias) in the navigation bar to display the **Manage Account** page.Then, click on the **Enable** link to enable two-factor authentication for the account.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)</span></span>
2. <span data-ttu-id="651f9-171">Fermez la session, puis reconnectez-vous.</span><span class="sxs-lookup"><span data-stu-id="651f9-171">Log off, then log back in.</span></span> <span data-ttu-id="651f9-172">Si vous avez activé la messagerie, vous pouvez sélectionner les SMS ou par courrier électronique pour l’authentification à deux facteurs.</span><span class="sxs-lookup"><span data-stu-id="651f9-172">If you've enabled email, you can select either SMS or email for two-factor authentication.</span></span> <span data-ttu-id="651f9-173">Si vous n’avez pas activé par courrier électronique, consultez le didacticiel intitulé [créer une application sécurisée de formulaires ASP.NET Web avec l’enregistrement de l’utilisateur, E-mail de Confirmation et de réinitialisation de mot de passe](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="651f9-173">If you haven't enabled email, see the tutorial titled [Create a Secure ASP.NET Web Forms App with User Registration, Email Confirmation and Password Reset](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)</span></span>
3. <span data-ttu-id="651f9-174">La page d’authentification à deux facteurs s’affiche où vous pouvez saisir le code (à partir de SMS ou par courrier électronique).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="651f9-174">The Two-Factor Authentication page is displayed where you can enter the code (from SMS or email).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)</span></span>  
 <span data-ttu-id="651f9-175">En cliquant sur le **n’oubliez pas de ce navigateur** case à cocher vous exempter d’avoir à utiliser l’authentification à deux facteurs pour vous connecter lorsque vous utilisez le navigateur et l’appareil sur lequel vous avez coché la case.</span><span class="sxs-lookup"><span data-stu-id="651f9-175">Clicking on the **Remember this browser** check box will exempt you from needing to use two-factor authentication to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="651f9-176">Tant que les utilisateurs malveillants ne peuvent pas accéder à votre appareil, l’activation de l’authentification à deux facteurs et en cliquant sur le **n’oubliez pas de ce navigateur** vous donne accès de mot de passe d’une étape pratique, tout en conservant fort protection de l’authentification à deux facteurs pour tous les accès à partir des appareils non approuvé.</span><span class="sxs-lookup"><span data-stu-id="651f9-176">As long as malicious users can't gain access to your device, enabling two-factor authentication and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong two-factor authentication protection for all access from non-trusted devices.</span></span> <span data-ttu-id="651f9-177">Pour cela, sur n’importe quel appareil privé que vous utilisez régulièrement.</span><span class="sxs-lookup"><span data-stu-id="651f9-177">You can do this on any private device you regularly use.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="651f9-178">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="651f9-178">Additional Resources</span></span>

- [<span data-ttu-id="651f9-179">Authentification à deux facteurs avec l’identité de ASP.NET à l’aide de SMS et par courrier électronique</span><span class="sxs-lookup"><span data-stu-id="651f9-179">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [<span data-ttu-id="651f9-180">Des liens vers ASP.NET Identity recommandé de ressources</span><span class="sxs-lookup"><span data-stu-id="651f9-180">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [<span data-ttu-id="651f9-181">Déployer une application de formulaires Web ASP.NET sécurisée avec base de données SQL, OAuth et l’appartenance à un site Web Azure</span><span class="sxs-lookup"><span data-stu-id="651f9-181">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to an Azure Website</span></span>](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [<span data-ttu-id="651f9-182">Série de didacticiels ASP.NET Web Forms - ajouter un fournisseur d’OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="651f9-182">ASP.NET Web Forms tutorial series - Add an OAuth 2.0 Provider</span></span>](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [<span data-ttu-id="651f9-183">Série de didacticiels Web Forms ASP.NET - activer SSL pour le projet</span><span class="sxs-lookup"><span data-stu-id="651f9-183">ASP.NET Web Forms tutorial series - Enable SSL for the Project</span></span>](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [<span data-ttu-id="651f9-184">Confirmation du compte et la récupération de mot de passe avec l’identité de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="651f9-184">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="651f9-185">Création de l’application de Facebook et la connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="651f9-185">Creating the app in Facebook and connecting the app to the project</span></span>](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)