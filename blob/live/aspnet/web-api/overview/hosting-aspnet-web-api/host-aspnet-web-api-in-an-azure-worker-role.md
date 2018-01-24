---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: "Héberger des API Web ASP.NET 2 dans un rôle de travail Azure | Documents Microsoft"
author: MikeWasson
description: "Ce didacticiel montre comment héberger ASP.NET Web API dans un rôle de travail Azure, à l’aide de OWIN pour l’auto-hébergement l’infrastructure API Web. Ouvrir l’Interface Web pour les de .NET (OWIN)..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 326c4a4e274dbc1aa6e09f1d07c4d135e4304484
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="bccd2-104">Héberger des API Web ASP.NET 2 dans un rôle de travail Azure</span><span class="sxs-lookup"><span data-stu-id="bccd2-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="bccd2-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bccd2-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bccd2-106">Ce didacticiel montre comment héberger ASP.NET Web API dans un rôle de travail Azure, à l’aide de OWIN pour l’auto-hébergement l’infrastructure API Web.</span><span class="sxs-lookup"><span data-stu-id="bccd2-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="bccd2-107">[Ouvrir l’Interface Web pour .NET](http://owin.org/) (OWIN) définit une abstraction entre les serveurs web .NET et des applications web.</span><span class="sxs-lookup"><span data-stu-id="bccd2-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bccd2-108">OWIN dissocie l’application web à partir du serveur, ce qui rend OWIN idéale pour l’auto-hébergement une application web dans votre propre processus, en dehors d’IIS – par exemple, à l’intérieur d’un rôle de travail Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd2-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="bccd2-109">Dans ce didacticiel, vous allez utiliser le package Microsoft.Owin.Host.HttpListener, qui fournit un serveur HTTP utilisé pour héberger automatique des applications OWIN.</span><span class="sxs-lookup"><span data-stu-id="bccd2-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bccd2-110">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="bccd2-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="bccd2-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bccd2-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="bccd2-112">API Web 2</span><span class="sxs-lookup"><span data-stu-id="bccd2-112">Web API 2</span></span>
> - [<span data-ttu-id="bccd2-113">Azure SDK pour .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="bccd2-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/en-us/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="bccd2-114">Créez un projet Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bccd2-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="bccd2-115">Démarrez Visual Studio avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="bccd2-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="bccd2-116">Des privilèges d’administrateur sont nécessaires pour déboguer l’application localement, à l’aide de l’émulateur de calcul Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd2-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="bccd2-117">Sur le **fichier** menu, cliquez sur **nouveau**, puis cliquez sur **projet**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="bccd2-118">À partir de **modèles installés**, sous Visual c#, cliquez sur **Cloud** puis cliquez sur **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="bccd2-119">Nommez le projet « AzureApp » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="bccd2-120">Dans le **nouveau Windows Azure Cloud Service** boîte de dialogue, double-cliquez sur **rôle de travail**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="bccd2-121">Laissez le nom par défaut (« WorkerRole1 »).</span><span class="sxs-lookup"><span data-stu-id="bccd2-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="bccd2-122">Cette étape ajoute un rôle de travail à la solution.</span><span class="sxs-lookup"><span data-stu-id="bccd2-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="bccd2-123">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="bccd2-124">La solution Visual Studio qui est créée contient deux projets :</span><span class="sxs-lookup"><span data-stu-id="bccd2-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="bccd2-125">&quot;AzureApp&quot; définit les rôles et la configuration de l’application Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd2-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="bccd2-126">&quot;WorkerRole1&quot; contient le code pour le rôle de travail.</span><span class="sxs-lookup"><span data-stu-id="bccd2-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="bccd2-127">En règle générale, une application Windows Azure peut contenir plusieurs rôles, bien que ce didacticiel utilise un seul rôle.</span><span class="sxs-lookup"><span data-stu-id="bccd2-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="bccd2-128">Ajouter les API Web et les Packages OWIN</span><span class="sxs-lookup"><span data-stu-id="bccd2-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="bccd2-129">À partir de la **outils** menu, cliquez sur **Gestionnaire de Package de bibliothèque**, puis cliquez sur **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="bccd2-130">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="bccd2-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="bccd2-131">Ajouter un point de terminaison HTTP</span><span class="sxs-lookup"><span data-stu-id="bccd2-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="bccd2-132">Dans l’Explorateur de solutions, développez le projet AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bccd2-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="bccd2-133">Développez le nœud rôles, cliquez sur WorkerRole1, puis sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="bccd2-134">Cliquez sur **points de terminaison**, puis cliquez sur **ajouter le point de terminaison**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="bccd2-135">Dans le **protocole** liste déroulante, sélectionnez « http ».</span><span class="sxs-lookup"><span data-stu-id="bccd2-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="bccd2-136">Dans **Port Public** et **Port privé**, tapez 80.</span><span class="sxs-lookup"><span data-stu-id="bccd2-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="bccd2-137">Ces numéros de port peut être différent.</span><span class="sxs-lookup"><span data-stu-id="bccd2-137">These port numbers can be different.</span></span> <span data-ttu-id="bccd2-138">Le port public est que les clients utilisent lorsqu’ils envoient une demande pour le rôle.</span><span class="sxs-lookup"><span data-stu-id="bccd2-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="bccd2-139">Configurer les API Web pour auto-hébergement</span><span class="sxs-lookup"><span data-stu-id="bccd2-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="bccd2-140">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="bccd2-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="bccd2-141">Nommez la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bccd2-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="bccd2-142">Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="bccd2-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="bccd2-143">Ajouter un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="bccd2-143">Add a Web API Controller</span></span>

<span data-ttu-id="bccd2-144">Ensuite, ajoutez une classe de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="bccd2-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="bccd2-145">Cliquez sur le projet WorkerRole1 et sélectionnez **ajouter** / **classe**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="bccd2-146">Nom de la classe TestController.</span><span class="sxs-lookup"><span data-stu-id="bccd2-146">Name the class TestController.</span></span> <span data-ttu-id="bccd2-147">Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="bccd2-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="bccd2-148">Par souci de simplicité, ce contrôleur définit seulement deux méthodes GET qui retournent du texte brut.</span><span class="sxs-lookup"><span data-stu-id="bccd2-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="bccd2-149">Démarrer l’hôte OWIN</span><span class="sxs-lookup"><span data-stu-id="bccd2-149">Start the OWIN Host</span></span>

<span data-ttu-id="bccd2-150">Ouvrez le fichier WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="bccd2-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="bccd2-151">Cette classe définit le code qui s’exécute lorsque le rôle de travail est démarré et arrêté.</span><span class="sxs-lookup"><span data-stu-id="bccd2-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="bccd2-152">Ajoutez le code suivant à l’aide d’instruction :</span><span class="sxs-lookup"><span data-stu-id="bccd2-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="bccd2-153">Ajouter un **IDisposable** membre à la `WorkerRole` classe :</span><span class="sxs-lookup"><span data-stu-id="bccd2-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="bccd2-154">Dans le `OnStart` (méthode), ajoutez le code suivant pour démarrer l’ordinateur hôte :</span><span class="sxs-lookup"><span data-stu-id="bccd2-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="bccd2-155">Le **WebApp.Start** méthode démarre l’hôte OWIN.</span><span class="sxs-lookup"><span data-stu-id="bccd2-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="bccd2-156">Le nom de la `Startup` classe est un paramètre de type à la méthode.</span><span class="sxs-lookup"><span data-stu-id="bccd2-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="bccd2-157">Par convention, l’hôte appelle la `Configure` méthode de cette classe.</span><span class="sxs-lookup"><span data-stu-id="bccd2-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="bccd2-158">Remplacer la `OnStop` pour les supprimer de la  *\_application* instance :</span><span class="sxs-lookup"><span data-stu-id="bccd2-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="bccd2-159">Voici le code complet pour WorkerRole.cs :</span><span class="sxs-lookup"><span data-stu-id="bccd2-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="bccd2-160">Générez la solution, appuyez sur F5 pour exécuter l’application localement dans l’émulateur de calcul Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd2-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="bccd2-161">En fonction de vos paramètres de pare-feu, vous devrez peut-être autoriser l’émulateur dans votre pare-feu.</span><span class="sxs-lookup"><span data-stu-id="bccd2-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="bccd2-162">Si vous obtenez une exception semblable à ce qui suit, consultez [ce billet de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) pour une solution de contournement.</span><span class="sxs-lookup"><span data-stu-id="bccd2-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="bccd2-163">« Impossible de charger le fichier ou l’assembly ' Microsoft.Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou une de ses dépendances.</span><span class="sxs-lookup"><span data-stu-id="bccd2-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="bccd2-164">Définition de manifeste de l’assembly trouvé ne correspond pas à la référence d’assembly.</span><span class="sxs-lookup"><span data-stu-id="bccd2-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="bccd2-165">(Exception de HRESULT : 0x80131040) »</span><span class="sxs-lookup"><span data-stu-id="bccd2-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="bccd2-166">L’émulateur de calcul affecte une adresse IP locale pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="bccd2-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="bccd2-167">Vous pouvez rechercher l’adresse IP en affichant l’interface d’émulateur de calcul.</span><span class="sxs-lookup"><span data-stu-id="bccd2-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="bccd2-168">Cliquez sur l’icône de l’émulateur dans la zone de notification de la barre des tâches, puis sélectionnez **Show Compute Emulator UI**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="bccd2-169">Rechercher l’adresse IP dans les déploiements de Service, déploiement [id], détails du Service.</span><span class="sxs-lookup"><span data-stu-id="bccd2-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="bccd2-170">Ouvrez un navigateur web et accédez à http://*adresse*/test/1, où *adresse* est l’adresse IP affectée par l’émulateur de calcul ; par exemple, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="bccd2-170">Open a web browser and navigate to http://*address*/test/1, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="bccd2-171">Vous devez voir la réponse à partir du contrôleur Web API :</span><span class="sxs-lookup"><span data-stu-id="bccd2-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="bccd2-172">Déployer vers Azure</span><span class="sxs-lookup"><span data-stu-id="bccd2-172">Deploy to Azure</span></span>

<span data-ttu-id="bccd2-173">Pour cette étape, vous devez disposer d’un compte Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd2-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="bccd2-174">Si vous n’en avez déjà, vous pouvez créer un compte d’évaluation gratuit en quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="bccd2-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bccd2-175">Pour plus d’informations, consultez [version d’évaluation gratuite de Microsoft Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="bccd2-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="bccd2-176">Dans l’Explorateur de solutions, cliquez sur le projet AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bccd2-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="bccd2-177">Sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="bccd2-178">Si vous n’êtes pas connecté à votre compte Azure, cliquez sur **connexion**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="bccd2-179">Une fois que vous êtes connecté, choisissez un abonnement, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="bccd2-180">Entrez un nom pour le service cloud et choisissez une région.</span><span class="sxs-lookup"><span data-stu-id="bccd2-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="bccd2-181">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="bccd2-182">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="bccd2-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="bccd2-183">La fenêtre de journal des activités Azure affiche la progression du déploiement.</span><span class="sxs-lookup"><span data-stu-id="bccd2-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="bccd2-184">Lorsque l’application est déployée, accédez à http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="bccd2-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="bccd2-185">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bccd2-185">Additional Resources</span></span>

- [<span data-ttu-id="bccd2-186">Une vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="bccd2-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="bccd2-187">Projet Katana sur GitHub</span><span class="sxs-lookup"><span data-stu-id="bccd2-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)