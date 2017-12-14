---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: "Partie 6 : L’appartenance d’ASP.NET | Documents Microsoft"
author: JoeStagner
description: "Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks. Partie 6 ajoute l’appartenance d’ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: efb0e2bed1172f42c7f1539f016fba305c47e3eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="9801a-104">Partie 6 : L’appartenance d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9801a-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="9801a-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9801a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9801a-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plateforme .NET.</span><span class="sxs-lookup"><span data-stu-id="9801a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9801a-107">Il illustre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="9801a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9801a-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9801a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9801a-109">Partie 6 ajoute l’appartenance d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9801a-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a><span data-ttu-id="9801a-110">Utilisation de l’appartenance d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9801a-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="9801a-111">Cliquez sur la sécurité</span><span class="sxs-lookup"><span data-stu-id="9801a-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="9801a-112">Assurez-vous que nous utilisons l’authentification par formulaire.</span><span class="sxs-lookup"><span data-stu-id="9801a-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="9801a-113">Pour créer deux utilisateurs, utilisez le lien « Create User ».</span><span class="sxs-lookup"><span data-stu-id="9801a-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="9801a-114">Lorsque vous avez terminé, reportez-vous à la fenêtre de l’Explorateur de solutions et actualiser l’affichage.</span><span class="sxs-lookup"><span data-stu-id="9801a-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="9801a-115">Notez que le fichier ASPNETDB. MDF fine a été créé.</span><span class="sxs-lookup"><span data-stu-id="9801a-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="9801a-116">Ce fichier contient les tables pour prendre en charge des services ASP.NET de base telles que l’appartenance.</span><span class="sxs-lookup"><span data-stu-id="9801a-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="9801a-117">Maintenant, nous pouvons commencer le processus de validation de mise en œuvre.</span><span class="sxs-lookup"><span data-stu-id="9801a-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="9801a-118">Commencez par créer une page CheckOut.aspx.</span><span class="sxs-lookup"><span data-stu-id="9801a-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="9801a-119">La page CheckOut.aspx doit uniquement être disponible pour les utilisateurs connectés sont donc nous limitent l’accès à enregistrées dans les utilisateurs et redirige les utilisateurs qui ne sont pas connectés à la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="9801a-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="9801a-120">Pour ce faire, nous allons ajouter les éléments suivants à la section de configuration de notre fichier web.config.</span><span class="sxs-lookup"><span data-stu-id="9801a-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="9801a-121">Le modèle pour les applications ASP.NET Web Forms automatiquement ajouté une section de l’authentification sur le fichier web.config et établie la page de connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="9801a-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="9801a-122">Nous devons modifier le Login.aspx fichier code-behind pour migrer un panier d’achat anonyme lorsque l’utilisateur se connecte.</span><span class="sxs-lookup"><span data-stu-id="9801a-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="9801a-123">Modifier la Page\_charge l’événement comme suit.</span><span class="sxs-lookup"><span data-stu-id="9801a-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="9801a-124">Ajoutez un gestionnaire d’événements « LoggedIn » comme suit pour définir le nom de session pour l’utilisateur qui vient d’être connecté et modifier l’id de session temporaire dans le panier d’achat à celle de l’utilisateur en appelant la méthode MigrateCart dans notre classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="9801a-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="9801a-125">(Implémentée dans le fichier .cs)</span><span class="sxs-lookup"><span data-stu-id="9801a-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="9801a-126">Implémentez la méthode MigrateCart() comme suit.</span><span class="sxs-lookup"><span data-stu-id="9801a-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="9801a-127">Dans checkout.aspx nous allons utiliser un contrôle EntityDataSource et un GridView dans notre page, vérifiez que nous l’avons fait dans notre page de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="9801a-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="9801a-128">Notez que notre contrôle GridView spécifie un gestionnaire d’événements « ondatabound » nommé MyList\_RowDataBound par conséquent, nous allons implémenter ce gestionnaire d’événements comme suit.</span><span class="sxs-lookup"><span data-stu-id="9801a-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="9801a-129">Cela maintient méthode en cours d’exécution total de l’achat panier car chaque ligne est liée et met à jour la ligne en bas du contrôle GridView.</span><span class="sxs-lookup"><span data-stu-id="9801a-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="9801a-130">À ce stade, nous avons implémenté une « « présentation de la commande doit être placé.</span><span class="sxs-lookup"><span data-stu-id="9801a-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="9801a-131">Nous allons traiter un scénario de panier vide en ajoutant quelques lignes de code à notre Page\_événement de chargement :</span><span class="sxs-lookup"><span data-stu-id="9801a-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="9801a-132">Lorsque l’utilisateur clique sur le bouton « Submit » nous exécutera le code suivant dans le Gestionnaire d’événement de Click de bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="9801a-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="9801a-133">La « substance » du processus de soumission de commande doit être implémentée dans la méthode SubmitOrder() de notre classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="9801a-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="9801a-134">SubmitOrder sera :</span><span class="sxs-lookup"><span data-stu-id="9801a-134">SubmitOrder will:</span></span>

- <span data-ttu-id="9801a-135">Prendre tous les articles dans le panier d’achat et de les utiliser pour créer un nouvel enregistrement de commande et les enregistrements OrderDetails associés.</span><span class="sxs-lookup"><span data-stu-id="9801a-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="9801a-136">Calculer la Date d’expédition.</span><span class="sxs-lookup"><span data-stu-id="9801a-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="9801a-137">Désactivez le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="9801a-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="9801a-138">Dans le cadre de cet exemple d’application, nous allons calculer une date d’expédition par simple ajout de deux jours à la date actuelle.</span><span class="sxs-lookup"><span data-stu-id="9801a-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="9801a-139">L’application maintenant en cours d’exécution permettra de nous pour tester le processus d’achat à partir du début à la fin.</span><span class="sxs-lookup"><span data-stu-id="9801a-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9801a-140">[Précédent](tailspin-spyworks-part-5.md)
[Suivant](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="9801a-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>