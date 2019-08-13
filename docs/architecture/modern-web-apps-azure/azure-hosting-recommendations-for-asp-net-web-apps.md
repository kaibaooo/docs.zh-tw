---
title: ASP.NET Web Core 應用程式的 Azure 裝載建議
description: 使用 ASP.NET Core 和 Azure 架構現代化 Web 應用程式 | ASP.NET Web Core 應用程式的 Azure 裝載建議
author: ardalis
ms.author: wiwagn
ms.date: 06/06/2019
ms.openlocfilehash: 7cfb9ada4f963aa392a41cfb9f1b2df22f542d41
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675465"
---
# <a name="azure-hosting-recommendations-for-aspnet-core-web-apps"></a><span data-ttu-id="cf5dd-103">ASP.NET Web Core 應用程式的 Azure 裝載建議</span><span class="sxs-lookup"><span data-stu-id="cf5dd-103">Azure hosting recommendations for ASP.NET Core web apps</span></span>

> <span data-ttu-id="cf5dd-104">「企業營運領導者都繞過 IT 部門，從雲端中取得應用程式 (亦稱為 SaaS) 並為其付費，就像訂閱雜誌一樣。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-104">"Line-of-business leaders everywhere are bypassing IT departments to get applications from the cloud (also known as SaaS) and paying for them like they would a magazine subscription.</span></span> <span data-ttu-id="cf5dd-105">當不再需要服務的時候，他們可以取消訂閱，且不會有任何設備在角落裡閒置。」</span><span class="sxs-lookup"><span data-stu-id="cf5dd-105">And when the service is no longer required, they can cancel the subscription with no equipment left unused in the corner."</span></span>  
> <span data-ttu-id="cf5dd-106">_\- Daryl Plummer，Gartner 分析師_</span><span class="sxs-lookup"><span data-stu-id="cf5dd-106">_\- Daryl Plummer, Gartner analyst_</span></span>

<span data-ttu-id="cf5dd-107">無論您的應用程式需求和架構是什麼，Microsoft Azure 都可以支援。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-107">Whatever your application's needs and architecture, Microsoft Azure can support it.</span></span> <span data-ttu-id="cf5dd-108">您的裝載需求可以像靜態網站一樣簡單，也可以像由數十種服務組成的應用程式一樣複雜。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-108">Your hosting needs can be as simple as a static website or a sophisticated application made up of dozens of services.</span></span> <span data-ttu-id="cf5dd-109">針對 ASP.NET Core 整合型 Web 應用程式和支援的服務，建議幾種眾所周知的組態。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-109">For ASP.NET Core monolithic web applications and supporting services, there are several well-known configurations that are recommended.</span></span> <span data-ttu-id="cf5dd-110">本文中的建議根據裝載資源的類型分組，不論是完整應用程式、個別處理程序或是資料。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-110">The recommendations on this article are grouped based on the kind of resource to be hosted, whether full applications, individual processes, or data.</span></span>

## <a name="web-applications"></a><span data-ttu-id="cf5dd-111">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="cf5dd-111">Web applications</span></span>

<span data-ttu-id="cf5dd-112">可裝載的 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="cf5dd-112">Web applications can be hosted with:</span></span>

- <span data-ttu-id="cf5dd-113">App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="cf5dd-113">App Service Web Apps</span></span>

- <span data-ttu-id="cf5dd-114">容器 (數個選項)</span><span class="sxs-lookup"><span data-stu-id="cf5dd-114">Containers (several options)</span></span>

- <span data-ttu-id="cf5dd-115">虛擬機器 (VM)</span><span class="sxs-lookup"><span data-stu-id="cf5dd-115">Virtual Machines (VMs)</span></span>

<span data-ttu-id="cf5dd-116">其中，App Service Web Apps 是大多數情況下建議的方法，包括簡單的容器型應用程式。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-116">Of these, App Service Web Apps is the recommended approach for most scenarios, including simple container-based apps.</span></span> <span data-ttu-id="cf5dd-117">針對微服務架構，請考慮容器式方法。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-117">For microservice architectures, consider a container-based approach.</span></span> <span data-ttu-id="cf5dd-118">如果您需要更充分掌控執行應用程式的機器，請考慮 Azure 虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-118">If you need more control over the machines running your application, consider Azure Virtual Machines.</span></span>

### <a name="app-service-web-apps"></a><span data-ttu-id="cf5dd-119">App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="cf5dd-119">App Service Web Apps</span></span>

<span data-ttu-id="cf5dd-120">App Service Web Apps 提供全受管的平台，針對裝載 Web 應用程式最佳化。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-120">App Service Web Apps offers a fully managed platform optimized for hosting web applications.</span></span> <span data-ttu-id="cf5dd-121">其為一種平台即服務 (PaaS) 供應項目，可讓您專注於商務邏輯；Azure 則負責執行和調整應用程式所需的基礎結構。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-121">It's a platform as a service (PaaS) offering that lets you focus on your business logic, while Azure takes care of the infrastructure needed to run and scale the app.</span></span> <span data-ttu-id="cf5dd-122">App Service Web Apps 的一些主要功能：</span><span class="sxs-lookup"><span data-stu-id="cf5dd-122">Some key features of App Service Web Apps:</span></span>

- <span data-ttu-id="cf5dd-123">DevOps 最佳化 (持續整合與傳遞、多種環境、A/B測試、指令碼支援)。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-123">DevOps optimization (continuous integration and delivery, multiple environments, A/B testing, scripting support).</span></span>

- <span data-ttu-id="cf5dd-124">全域規模與高可用性。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-124">Global scale and high availability.</span></span>

- <span data-ttu-id="cf5dd-125">連接至 SaaS 平台和您的內部部署資料。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-125">Connections to SaaS platforms and your on-premises data.</span></span>

- <span data-ttu-id="cf5dd-126">安全性與合規性。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-126">Security and compliance.</span></span>

- <span data-ttu-id="cf5dd-127">Visual Studio 整合。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-127">Visual Studio integration.</span></span>

<span data-ttu-id="cf5dd-128">Azure App Service 是大部分 Web 應用程式的最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-128">Azure App Service is the best choice for most web apps.</span></span> <span data-ttu-id="cf5dd-129">部署和管理整合至平台中、網站可快速調整以處理高流量負載，以及內建的負載平衡和流量管理員提供高可用性。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-129">Deployment and management are integrated into the platform, sites can scale quickly to handle high traffic loads, and the built-in load balancing and traffic manager provide high availability.</span></span> <span data-ttu-id="cf5dd-130">您可以使用線上移轉工具，輕鬆將現有網站移動到 Azure App Service、使用 Web 應用程式資源庫中的開放原始碼應用程式，或使用您選擇的架構和工具來建立新網站。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-130">You can move existing sites to Azure App Service easily with an online migration tool, use an open-source app from the Web Application Gallery, or create a new site using the framework and tools of your choice.</span></span> <span data-ttu-id="cf5dd-131">WebJobs 功能可輕鬆將背景工作處理新增至 App Service Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-131">The WebJobs feature makes it easy to add background job processing to your App Service web app.</span></span> <span data-ttu-id="cf5dd-132">如果您有使用本機資料庫裝載在內部部署的現有 ASP.NET 應用程式，則有一個明確的路徑，可以使用 Azure SQL Database，將應用程式移轉至 App Service Web 應用程式 (或者如果想要的話，對內部部署資料庫伺服器進行安全存取)。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-132">If you have an existing ASP.NET application hosted on-premises using a local database, there's a clear path to migrate the app to an App Service Web App with an Azure SQL Database (or secure access to your on-premises database server, if preferred).</span></span>

![將內部部署 .NET 應用程式移轉至 Azure App Service 的建議策略](./media/image1-6.png)

<span data-ttu-id="cf5dd-134">在大部分情況下，從本機裝載的 ASP.NET 應用程式移至 App Service Web 應用程式是一個簡單的程序。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-134">In most cases, moving from a locally hosted ASP.NET app to an App Service Web App is a straightforward process.</span></span> <span data-ttu-id="cf5dd-135">幾乎或完全不需要修改應用程式本身，而且它可以快速開始使用 Azure App Service Web Apps 提供的許多功能。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-135">Little or no modification should be required of the app itself, and it can quickly start to take advantage of the many features that Azure App Service Web Apps offer.</span></span>

<span data-ttu-id="cf5dd-136">除了未針對雲端最佳化的應用程式之外，Azure App Service Web Apps 還是許多簡單整合型 (非分散式) 應用程式 (例如許多 ASP.NET Core 應用程式) 的絕佳解決方案。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-136">In addition to apps that are not optimized for the cloud, Azure App Service Web Apps are an excellent solution for many simple monolithic (non-distributed) applications, such as many ASP.NET Core apps.</span></span> <span data-ttu-id="cf5dd-137">利用這種方法，架構會是基本的，而且易於了解和管理：</span><span class="sxs-lookup"><span data-stu-id="cf5dd-137">In this approach, the architecture is basic and simple to understand and manage:</span></span>

![基本 Azure 架構](./media/image1-5.png)

<span data-ttu-id="cf5dd-139">單一資源群組中的少數資源通常就足以管理這類應用程式。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-139">A small number of resources in a single resource group is typically sufficient to manage such an app.</span></span> <span data-ttu-id="cf5dd-140">通常部署為單一單元的應用程式 (而不是組成許多不同處理程序的應用程式) 非常適合這種[基本架構方法](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app)。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-140">Apps that are typically deployed as a single unit, rather than those apps that are made up of many separate processes, are good candidates for this [basic architectural approach](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app).</span></span> <span data-ttu-id="cf5dd-141">雖然在架構上很簡單，但此方法仍然可讓裝載的應用程式向上擴充 (每個節點多個資源) 以及橫向擴充 (更多裝載的節點) 以符合任何增加需求。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-141">Though architecturally simple, this approach still allows the hosted app to scale both up (more resources per node) and out (more hosted nodes) to meet any increase in demand.</span></span> <span data-ttu-id="cf5dd-142">透過自動調整，應用程式可以設定為根據需求和節點間的平均負載，自動調整裝載應用程式的節點數目。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-142">With autoscale, the app can be configured to automatically adjust the number of nodes hosting the app based on demand and average load across nodes.</span></span>

### <a name="app-service-web-apps-for-containers"></a><span data-ttu-id="cf5dd-143">用於容器的 App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="cf5dd-143">App Service Web Apps for Containers</span></span>

<span data-ttu-id="cf5dd-144">除了支援直接裝載 Web 應用程式之外，[用於容器的 App Service Web Apps](https://azure.microsoft.com/services/app-service/containers/) 還可以用來在 Windows 和 Linux 上執行容器化的應用程式。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-144">In addition to support for hosting web apps directly, [App Service Web Apps for Containers](https://azure.microsoft.com/services/app-service/containers/) can be used to run containerized applications on Windows and Linux.</span></span> <span data-ttu-id="cf5dd-145">您可以使用這項服務，輕鬆地部署並執行可隨業務調整的容器化應用程式。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-145">Using this service, you can easily deploy and run containerized applications that can scale with your business.</span></span> <span data-ttu-id="cf5dd-146">此應用程式具備上述所列的所有 App Service Web Apps 功能。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-146">The apps have all of the features of App Service Web Apps listed above.</span></span> <span data-ttu-id="cf5dd-147">此外，用於容器的 Web Apps 支援簡化的 CI/CD 搭配 Docker Hub、Azure Container Registry 和 GitHub。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-147">In addition, Web Apps for Containers support streamlined CI/CD with Docker Hub, Azure Container Registry, and GitHub.</span></span> <span data-ttu-id="cf5dd-148">您可以使用 Azure DevOps 定義將變更發佈至登錄的組建和部署管線。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-148">You can use Azure DevOps to define build and deployment pipelines that publish changes to a registry.</span></span> <span data-ttu-id="cf5dd-149">這些變更之後可以在預備環境中加以測試，並使用部署位置自動部署至生產環境，進而允許零停機時間升級。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-149">These changes can then be tested in a staging environment and automatically deployed to production using deployment slots, allowing zero-downtime upgrades.</span></span> <span data-ttu-id="cf5dd-150">回復到先前的版本可以輕易地完成。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-150">Rolling back to previous versions can be done just as easily.</span></span>

<span data-ttu-id="cf5dd-151">在某些情況下，用於容器的 Web Apps 最合理。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-151">There are a few scenarios where Web Apps for Containers make the most sense.</span></span> <span data-ttu-id="cf5dd-152">如果您有可以容器化的現有應用程式，則無論是在 Windows 還是 Linux 容器中，您都可以使用此工具組輕鬆地裝載這些應用程式。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-152">If you have existing apps that you can containerize, whether in Windows or Linux containers, you can host these easily using this toolset.</span></span> <span data-ttu-id="cf5dd-153">只要發佈容器，然後將用於容器的 Web Apps 設定為從選擇的登錄提取該映像的最新版本即可。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-153">Simply publish your container and then configure Web Apps for Containers to pull the latest version of that image from your registry of choice.</span></span> <span data-ttu-id="cf5dd-154">這是「隨即轉移」方法，可從傳統的應用程式裝載模型移轉至雲端最佳化模型。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-154">This is a "lift and shift" approach to migrating from classic app hosting models to a cloud-optimized model.</span></span>

![將容器化的內部部署 .NET 應用程式移轉至用於容器的 Azure Web Apps](./media/image1-8.png)

<span data-ttu-id="cf5dd-156">如果您的開發小組可移至容器型開發程序，這個方法也適用。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-156">This approach also works well if your development team is able to move to a container-based development process.</span></span> <span data-ttu-id="cf5dd-157">開發應用程式含有容器的「內部迴圈」包含建置含有容器的應用程式。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-157">The "inner loop" of developing apps with containers includes building the app with containers.</span></span> <span data-ttu-id="cf5dd-158">對程式碼以及容器設定所做的變更會推送至原始檔控制，而自動化組建則負責將新的容器映像發佈至 Docker Hub 或 Azure Container Registry 之類的登錄。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-158">Changes made to the code as well as to container configuration are pushed to source control, and an automated build is responsible for publishing new container images to a registry like Docker Hub or Azure Container Registry.</span></span> <span data-ttu-id="cf5dd-159">這些映像之後會當作其他開發以及部署到生產環境的基礎使用，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="cf5dd-159">These images are then used as the basis for additional development, as well as for deployments to production, as shown in the following diagram:</span></span>

![端對端 Docker DevOps 生命週期工作流程](./media/image1-7.png)

<span data-ttu-id="cf5dd-161">使用容器開發可提供許多優點，特別是在容器用於生產環境時。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-161">Developing with containers offers many advantages, especially when containers are used in production.</span></span> <span data-ttu-id="cf5dd-162">相同的容器設定用來裝載每個環境中所執行的應用程式，從本機開發電腦到將系統建置到生產環境並進行測試。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-162">The same container configuration is used to host the app in each environment in which it runs, from local development machine to build and test systems to production.</span></span> <span data-ttu-id="cf5dd-163">這可大幅降低因電腦設定或軟體版本不同而產生缺失的可能性。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-163">This greatly reduces the likelihood of defects resulting from differences in machine configuration or software versions.</span></span> <span data-ttu-id="cf5dd-164">開發人員也可以使用他們最具生產力的任何工具，包括作業系統，因為容器可以在任何 OS 上執行。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-164">Developers can also use whatever tools they're most productive with, including operating system, since containers can run on any OS.</span></span> <span data-ttu-id="cf5dd-165">在某些情況下，涉及許多容器的分散式應用程式在單一開發電腦上執行可能非常耗費資源。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-165">In some cases, distributed applications involving many containers may be very resource-intensive to run on a single development machine.</span></span> <span data-ttu-id="cf5dd-166">在此情況下，升級為使用 Kubernetes 和 Azure Dev Spaces 可能是有意義的，如下一節所述。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-166">In this scenario, it may make sense to upgrade to using Kubernetes and Azure Dev Spaces, covered in the next section.</span></span>

<span data-ttu-id="cf5dd-167">因為較大應用程式的部分會分割成較小的獨立*微服務*，其他設計模式則可用來改善應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-167">As portions of larger applications are broken up into their own smaller, independent *microservices*, additional design patterns can be used to improve app behavior.</span></span> <span data-ttu-id="cf5dd-168">*API 閘道*可以簡化存取以及將用戶端與其後端分離，而不是直接使用個別的服務。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-168">Instead of working directly with individual services, an *API gateway* can simplify access and decouple the client from its back end.</span></span> <span data-ttu-id="cf5dd-169">為不同的前端提供個別的服務後端時，也可讓服務隨著其取用者擴展。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-169">Having separate service back ends for different front ends also allows services to evolve in concert with their consumers.</span></span> <span data-ttu-id="cf5dd-170">常見的服務可以透過個別*側車*容器存取，其中可能包含使用*大使*模式的常見用戶端連線程式庫。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-170">Common services can be accessed via a separate *sidecar* container, which might include common client connectivity libraries using the *ambassador* pattern.</span></span>

![註明幾種常見設計模式的微服務範例架構。](./media/image1-10.png)

[<span data-ttu-id="cf5dd-172">深入了解要在建置微服務型系統時考慮的設計模式。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-172">Learn more about design patterns to consider when building microservice-based systems.</span></span>](https://docs.microsoft.com/azure/architecture/microservices/design/patterns)

### <a name="azure-kubernetes-service"></a><span data-ttu-id="cf5dd-173">Azure Kubernetes Service</span><span class="sxs-lookup"><span data-stu-id="cf5dd-173">Azure Kubernetes Service</span></span>

<span data-ttu-id="cf5dd-174">Azure Kubernetes Service (AKS) 管理您託管的 Kubernetes 環境，可讓您快速輕鬆地部署及管理容器化應用程式，而不需要容器協調流程專業知識。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-174">Azure Kubernetes Service (AKS) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized applications without container orchestration expertise.</span></span> <span data-ttu-id="cf5dd-175">您可以視需要佈建、升級資源及調整其規模，而不需要讓應用程式離線，因此也會排除持續性操作和維護的負擔。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-175">It also eliminates the burden of ongoing operations and maintenance by provisioning, upgrading, and scaling resources on demand, without taking your applications offline.</span></span>

<span data-ttu-id="cf5dd-176">AKS 透過將大部分責任轉移給 Azure，來降低管理 Kubernetes 叢集的複雜度和操作額外負荷。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-176">AKS reduces the complexity and operational overhead of managing a Kubernetes cluster by offloading much of that responsibility to Azure.</span></span> <span data-ttu-id="cf5dd-177">Azure 是託管的 Kubernetes 服務，為您處理狀況監控和維護等重要工作。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-177">As a hosted Kubernetes service, Azure handles critical tasks like health monitoring and maintenance for you.</span></span> <span data-ttu-id="cf5dd-178">此外，您只需針對叢集內的代理程式節點，而不需針對 master 支付費用。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-178">Also, you pay only for the agent nodes within your clusters, not for the masters.</span></span> <span data-ttu-id="cf5dd-179">AKS 是受控 Kubernetes 服務，提供：</span><span class="sxs-lookup"><span data-stu-id="cf5dd-179">As a managed Kubernetes service, AKS provides:</span></span>

- <span data-ttu-id="cf5dd-180">自動化的 Kubernetes 版本升級和修補。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-180">Automated Kubernetes version upgrades and patching.</span></span>
- <span data-ttu-id="cf5dd-181">簡單的叢集規模調整。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-181">Easy cluster scaling.</span></span>
- <span data-ttu-id="cf5dd-182">自我修復託管控制介面 (master)。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-182">Self-healing hosted control plane (masters).</span></span>
- <span data-ttu-id="cf5dd-183">節省成本 - 只需支付執行中代理程式集區節點的費用。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-183">Cost savings - pay only for running agent pool nodes.</span></span>

<span data-ttu-id="cf5dd-184">有了 Azure 負責管理您 AKS 叢集中的節點，您就不再需要手動執行許多工作，像是叢集升級。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-184">With Azure handling the management of the nodes in your AKS cluster, you no longer need to perform many tasks manually, like cluster upgrades.</span></span> <span data-ttu-id="cf5dd-185">由於 Azure 會為您處理這些重要的維護工作，因此 AKS 不提供對叢集的直接存取 (例如透過 SSH)。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-185">Because Azure handles these critical maintenance tasks for you, AKS doesn't provide direct access (such as with SSH) to the cluster.</span></span>

<span data-ttu-id="cf5dd-186">運用 AKS 的小組也可以使用 Azure Dev Spaces。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-186">Teams who are leveraging AKS can also take advantage of Azure Dev Spaces.</span></span> <span data-ttu-id="cf5dd-187">Azure Dev Spaces 可協助小組透過允許小組直接使用其在 AKS 中執行的整個微服務架構或應用程式，藉此將注意力放在其微服務應用程式的開發與快速反覆運算。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-187">Azure Dev Spaces helps teams to focus on the development and rapid iteration of their microservice application by allowing teams to work directly with their entire microservices architecture or application running in AKS.</span></span> <span data-ttu-id="cf5dd-188">Azure Dev Spaces 也會提供一種獨立更新微服務架構部分的方法，而不會影響到其餘的 AKS 叢集或其他開發人員。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-188">Azure Dev Spaces also provides a way to independently update portions of your microservices architecture in isolation without affecting the rest of the AKS cluster or other developers.</span></span>

![Azure Dev Spaces 工作流程範例](./media/image1-9.gif)

<span data-ttu-id="cf5dd-190">Azure Dev Spaces：</span><span class="sxs-lookup"><span data-stu-id="cf5dd-190">Azure Dev Spaces:</span></span>

- <span data-ttu-id="cf5dd-191">將本機電腦設定時間和資源需求降到最低</span><span class="sxs-lookup"><span data-stu-id="cf5dd-191">Minimize local machine setup time and resource requirements</span></span>
- <span data-ttu-id="cf5dd-192">讓小組更快速地逐一查看</span><span class="sxs-lookup"><span data-stu-id="cf5dd-192">Allow teams to iterate more rapidly</span></span>
- <span data-ttu-id="cf5dd-193">減少小組所需的整合環境數量</span><span class="sxs-lookup"><span data-stu-id="cf5dd-193">Reduce number of integration environments required by team</span></span>
- <span data-ttu-id="cf5dd-194">開發/測試時，在分散式系統中移除模擬特定服務的需求</span><span class="sxs-lookup"><span data-stu-id="cf5dd-194">Remove need to mock certain services in distributed system when developing/testing</span></span>

[<span data-ttu-id="cf5dd-195">深入了解 Azure Dev Spaces</span><span class="sxs-lookup"><span data-stu-id="cf5dd-195">Learn more about Azure Dev Spaces</span></span>](https://docs.microsoft.com/azure/dev-spaces/about)

### <a name="azure-virtual-machines"></a><span data-ttu-id="cf5dd-196">Azure 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="cf5dd-196">Azure Virtual Machines</span></span>

<span data-ttu-id="cf5dd-197">如果您的現有應用程式需要進行大量修改才能在 App Service 中執行，您可以選擇虛擬機器以簡化移轉至雲端的程序。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-197">If you have an existing application that would require substantial modifications to run in App Service, you could choose Virtual Machines in order to simplify migrating to the cloud.</span></span> <span data-ttu-id="cf5dd-198">但是，與 Azure App Service 相較，正確設定、保護和維護 VM 需要更多的時間和 IT 專業知識。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-198">However, correctly configuring, securing, and maintaining VMs requires much more time and IT expertise compared to Azure App Service.</span></span> <span data-ttu-id="cf5dd-199">如果您正在考慮 Azure 虛擬機器，請確保將修補、更新和管理 VM 環境所需的持續性維護工作納入考量。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-199">If you're considering Azure Virtual Machines, make sure you take into account the ongoing maintenance effort required to patch, update, and manage your VM environment.</span></span> <span data-ttu-id="cf5dd-200">Azure 虛擬機器是基礎結構即服務 (IaaS)，而 App Service 是 PaaS。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-200">Azure Virtual Machines is infrastructure as a service (IaaS), while App Service is PaaS.</span></span> <span data-ttu-id="cf5dd-201">建議您也考慮將您的應用程式作為 Windows 容器部署至用於容器的 Web App，是否是您範例的可用選項。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-201">You should also consider whether deploying your app as a Windows Container to Web App for Containers might be a viable option for your scenario.</span></span>

## <a name="logical-processes"></a><span data-ttu-id="cf5dd-202">邏輯處理序</span><span class="sxs-lookup"><span data-stu-id="cf5dd-202">Logical processes</span></span>

<span data-ttu-id="cf5dd-203">可以與應用程式的其餘部分分離的個別邏輯處理序，能以「無伺服器」方式獨立部署至 Azure 功能。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-203">Individual logical processes that can be decoupled from the rest of the application may be deployed independently to Azure Functions in a "serverless" manner.</span></span> <span data-ttu-id="cf5dd-204">Azure 函式讓您只需撰寫特定問題所需的程式碼，而無需擔心應用程式或基礎結構運行。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-204">Azure Functions lets you just write the code you need for a given problem, without worrying about the application or infrastructure to run it.</span></span> <span data-ttu-id="cf5dd-205">您可以從各種程式設計語言中進行選擇，包括 C\#，F\#、Node.js、Python 和 PHP，允許您為手邊工作選擇最具生產力的語言。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-205">You can choose from a variety of programming languages, including C\#, F\#, Node.js, Python, and PHP, allowing you to pick the most productive language for the task at hand.</span></span> <span data-ttu-id="cf5dd-206">與大多數基於雲端的解決方案一樣，您只需支付使用時間，並且可以信任 Azure 函式，來根據需要調升規模。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-206">Like most cloud-based solutions, you pay only for the amount of time your use, and you can trust Azure Functions to scale up as needed.</span></span>

## <a name="data"></a><span data-ttu-id="cf5dd-207">資料</span><span class="sxs-lookup"><span data-stu-id="cf5dd-207">Data</span></span>

<span data-ttu-id="cf5dd-208">Azure 提供各種資料儲存選項，以便您的應用程式可以針對有問題的資料使用適當的資料提供者。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-208">Azure offers a wide variety of data storage options, so that your application can use the appropriate data provider for the data in question.</span></span>

<span data-ttu-id="cf5dd-209">針對交易式的關聯式資料，Azure SQL Database 是最佳選項。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-209">For transactional, relational data, Azure SQL Databases are the best option.</span></span> <span data-ttu-id="cf5dd-210">對於高性能的唯讀資料，由 Azure SQL Database 支援之 Redis 快取是很好的解決方案。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-210">For high performance read-mostly data, a Redis cache backed by an Azure SQL Database is a good solution.</span></span>

<span data-ttu-id="cf5dd-211">非結構化的 JSON 資料可以透過各種方式進行儲存；從 SQL 資料庫的資料列到 Blob 或 Azure 儲存體中的表單到 DocumentDB。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-211">Unstructured JSON data can be stored in a variety of ways, from SQL Database columns to Blobs or Tables in Azure Storage, to DocumentDB.</span></span> <span data-ttu-id="cf5dd-212">其中 DocumentDB 提供最佳的查詢功能，且是大量 JSON 型文件的推薦選項，因為這些文件必須支援查詢。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-212">Of these, DocumentDB offers the best querying functionality, and is the recommended option for large numbers of JSON-based documents that must support querying.</span></span>

<span data-ttu-id="cf5dd-213">用於協調應用程式行為的暫時性命令或事件型資料，可以使用 Azure 服務匯流排或 Azure 儲存體佇列。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-213">Transient command- or event-based data used to orchestrate application behavior can use Azure Service Bus or Azure Storage Queues.</span></span> <span data-ttu-id="cf5dd-214">Azure 儲存體匯流排提供更大的靈活性，並且是應用程式內部和之間非一般訊息的建議服務。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-214">Azure Storage Bus offers more flexibility and is the recommended service for non-trivial messaging within and between applications.</span></span>

## <a name="architecture-recommendations"></a><span data-ttu-id="cf5dd-215">架構建議</span><span class="sxs-lookup"><span data-stu-id="cf5dd-215">Architecture recommendations</span></span>

<span data-ttu-id="cf5dd-216">您的應用程式需求應指定其架構。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-216">Your application's requirements should dictate its architecture.</span></span> <span data-ttu-id="cf5dd-217">有許多不同的 Azure 服務可供選擇。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-217">There are many different Azure services available.</span></span> <span data-ttu-id="cf5dd-218">選擇正確的服務是一項重要決策。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-218">Choosing the right one is an important decision.</span></span> <span data-ttu-id="cf5dd-219">Microsoft 提供參考架構資源庫，以協助識別針對常見案例最佳化的常見架構。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-219">Microsoft offers a gallery of reference architectures to help identify typical architectures optimized for common scenarios.</span></span> <span data-ttu-id="cf5dd-220">您可以尋找緊密對應至應用程式需求的參考架構，或至少提供一個起點。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-220">You may find a reference architecture that maps closely to your application's requirements, or at least offers a starting point.</span></span>

<span data-ttu-id="cf5dd-221">圖 11-2 顯示參考架構範例。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-221">Figure 11-2 shows an example reference architecture.</span></span> <span data-ttu-id="cf5dd-222">此圖描述針對市場行銷進行最佳化的 Sitecore 內容管理系統網站之建議的架構方法。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-222">This diagram describes a recommended architecture approach for a Sitecore content management system website optimized for marketing.</span></span>

![](./media/image11-2.png)

<span data-ttu-id="cf5dd-223">**圖 11-1。**</span><span class="sxs-lookup"><span data-stu-id="cf5dd-223">**Figure 11-1.**</span></span> <span data-ttu-id="cf5dd-224">Sitecore 行銷網站參考架構。</span><span class="sxs-lookup"><span data-stu-id="cf5dd-224">Sitecore marketing website reference architecture.</span></span>

<span data-ttu-id="cf5dd-225">**參考資料 - Azure 裝載建議**</span><span class="sxs-lookup"><span data-stu-id="cf5dd-225">**References – Azure hosting recommendations**</span></span>

- <span data-ttu-id="cf5dd-226">Azure 解決方案架構</span><span class="sxs-lookup"><span data-stu-id="cf5dd-226">Azure Solution Architectures</span></span>\
  <https://azure.microsoft.com/solutions/architecture/>

- <span data-ttu-id="cf5dd-227">Azure 的基本 Web 應用程式架構</span><span class="sxs-lookup"><span data-stu-id="cf5dd-227">Azure Basic Web Application Architecture</span></span>\
  <https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app>

- <span data-ttu-id="cf5dd-228">微服務的設計模式</span><span class="sxs-lookup"><span data-stu-id="cf5dd-228">Design Patterns for Microservices</span></span>\
  <https://docs.microsoft.com/azure/architecture/microservices/design/patterns>

- <span data-ttu-id="cf5dd-229">Azure 開發人員指南</span><span class="sxs-lookup"><span data-stu-id="cf5dd-229">Azure Developer Guide</span></span>\
  <https://azure.microsoft.com/campaigns/developer-guide/>

- <span data-ttu-id="cf5dd-230">Web Apps 概觀</span><span class="sxs-lookup"><span data-stu-id="cf5dd-230">Web Apps overview</span></span>\
  <https://docs.microsoft.com/azure/app-service/app-service-web-overview>

- <span data-ttu-id="cf5dd-231">用於容器的 Web App</span><span class="sxs-lookup"><span data-stu-id="cf5dd-231">Web App for Containers</span></span>\
  <https://azure.microsoft.com/services/app-service/containers/>

- <span data-ttu-id="cf5dd-232">Azure Kubernetes Service (AKS) 簡介</span><span class="sxs-lookup"><span data-stu-id="cf5dd-232">Introduction to Azure Kubernetes Service (AKS)</span></span>\
  <https://docs.microsoft.com/azure/aks/intro-kubernetes>

>[!div class="step-by-step"]
>[<span data-ttu-id="cf5dd-233">上一步</span><span class="sxs-lookup"><span data-stu-id="cf5dd-233">Previous</span></span>](development-process-for-azure.md)