---
title: Docker 應用程式的開發程序
description: 提供 Docker 型應用程式開發選項的高層級概觀。 使用您選擇的 Visual Studio for Windows、Visual Studio for Mac 或適用於多平台支援 (Windows、Mac 與 Linux) 的 Visual Studio Code。
ms.date: 09/27/2018
ms.openlocfilehash: a32b27f3d98ed7ebf63b637ec0c979c22ee8e1e8
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "70296131"
---
# <a name="development-process-for-docker-based-applications"></a><span data-ttu-id="c207f-104">Docker 應用程式的開發程序</span><span class="sxs-lookup"><span data-stu-id="c207f-104">Development Process for Docker-Based Applications</span></span>

<span data-ttu-id="c207f-105">*依您喜歡的方式來開發容器化 .NET 應用程式，可以是 Visual Studio 和 Visual Studio Tools for Docker 導向的 IDE，或是 Docker CLI 和 Visual Studio Code 導向的 CLI/編輯器。*</span><span class="sxs-lookup"><span data-stu-id="c207f-105">*Develop containerized .NET applications the way you like, either IDE focused with Visual Studio and Visual Studio tools for Docker or CLI/Editor focused with Docker CLI and Visual Studio Code.*</span></span>

## <a name="development-environment-for-docker-apps"></a><span data-ttu-id="c207f-106">Docker 應用程式的開發環境</span><span class="sxs-lookup"><span data-stu-id="c207f-106">Development environment for Docker apps</span></span>

### <a name="development-tool-choices-ide-or-editor"></a><span data-ttu-id="c207f-107">開發工具選擇：IDE 或編輯器</span><span class="sxs-lookup"><span data-stu-id="c207f-107">Development tool choices: IDE or editor</span></span>

<span data-ttu-id="c207f-108">不論您偏好使用完整且強大的 IDE，還是輕量型的敏捷式編輯器，Microsoft 都有相關工具可供您用來開發 Docker 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c207f-108">Whether you prefer a full and powerful IDE or a lightweight and agile editor, Microsoft has tools that you can use for developing Docker applications.</span></span>

<span data-ttu-id="c207f-109">**Visual Studio (適用於 Windows)。**</span><span class="sxs-lookup"><span data-stu-id="c207f-109">**Visual Studio (for Windows).**</span></span> <span data-ttu-id="c207f-110">使用 Visual Studio 開發 Docker 型應用程式時，建議使用 Visual Studio 2017 15.7 版或更新版本，這隨附於已內建的 Docker 工具。</span><span class="sxs-lookup"><span data-stu-id="c207f-110">When developing Docker-based applications with Visual Studio, it's recommended to use Visual Studio 2017 version 15.7 or later, that comes with tools for Docker already built-in.</span></span> <span data-ttu-id="c207f-111">Docker 工具可讓您直接在目標 Docker 環境中開發、執行和驗證應用程式。</span><span class="sxs-lookup"><span data-stu-id="c207f-111">The tools for Docker let you develop, run, and validate your applications directly in the target Docker environment.</span></span> <span data-ttu-id="c207f-112">您可以按 F5 鍵，直接在 Docker 主機中執行並偵錯您的應用程式 (單一容器或多個容器)，或按 CTRL+F5 來編輯及重新整理您的應用程式，而不需要重建容器。</span><span class="sxs-lookup"><span data-stu-id="c207f-112">You can press F5 to run and debug your application (single container or multiple containers) directly into a Docker host, or press CTRL+F5 to edit and refresh your application without having to rebuild the container.</span></span> <span data-ttu-id="c207f-113">這是以 Docker 為基礎之應用程式的最強大開發選擇。</span><span class="sxs-lookup"><span data-stu-id="c207f-113">This is the most powerful development choice for Docker-based apps.</span></span>

<span data-ttu-id="c207f-114">**Visual Studio for Mac。**</span><span class="sxs-lookup"><span data-stu-id="c207f-114">**Visual Studio for Mac.**</span></span> <span data-ttu-id="c207f-115">它是一個 IDE，為 Xamarin Studio 的演進版，在 macOS 中執行並從 2017 年中開始支援 Docker。</span><span class="sxs-lookup"><span data-stu-id="c207f-115">It's an IDE, evolution of Xamarin Studio, running in macOS and supports Docker since mid-2017.</span></span> <span data-ttu-id="c207f-116">針對在 Mac 電腦上工作同時想要使用功能強大之 IDE 的開發人員，這應該是偏好選項。</span><span class="sxs-lookup"><span data-stu-id="c207f-116">This should be the preferred choice for developers working in Mac machines who also want to use a powerful IDE.</span></span>

<span data-ttu-id="c207f-117">**Visual Studio Code 和 Docker CLI**。</span><span class="sxs-lookup"><span data-stu-id="c207f-117">**Visual Studio Code and Docker CLI**.</span></span> <span data-ttu-id="c207f-118">如果您偏好使用支援任何開發語言之輕量型且跨平台的編輯器，您可以使用 Microsoft Visual Studio Code (VS Code) 和 Docker CLI。</span><span class="sxs-lookup"><span data-stu-id="c207f-118">If you prefer a lightweight and cross-platform editor that supports any development language, you can use Microsoft Visual Studio Code (VS Code) and the Docker CLI.</span></span> <span data-ttu-id="c207f-119">這是適用於 Mac、Linux 和 Windows 的跨平台開發方法。</span><span class="sxs-lookup"><span data-stu-id="c207f-119">This is a cross-platform development approach for Mac, Linux, and Windows.</span></span> <span data-ttu-id="c207f-120">此外，Visual Studio Code 支援 Docker 的延伸模組 (例如適用於 Dockerfile 的 IntelliSense)，以及可從編輯器執行 Docker 命令的捷徑工作。</span><span class="sxs-lookup"><span data-stu-id="c207f-120">Additionally, Visual Studio Code supports extensions for Docker such as IntelliSense for Dockerfiles and shortcut tasks to run Docker commands from the editor.</span></span>

<span data-ttu-id="c207f-121">透過安裝 [Docker Desktop Community Edition (CE)](https://hub.docker.com/search/?type=edition&offering=community)，您可以使用單一 Docker CLI 來建置 Windows 和 Linux 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c207f-121">By installing [Docker Desktop Community Edition (CE)](https://hub.docker.com/search/?type=edition&offering=community), you can use a single Docker CLI to build apps for both Windows and Linux.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="c207f-122">其他資源</span><span class="sxs-lookup"><span data-stu-id="c207f-122">Additional resources</span></span>

- <span data-ttu-id="c207f-123">**Visual Studio**。</span><span class="sxs-lookup"><span data-stu-id="c207f-123">**Visual Studio**.</span></span> <span data-ttu-id="c207f-124">官方網站。</span><span class="sxs-lookup"><span data-stu-id="c207f-124">Official site.</span></span> \
  [https://visualstudio.microsoft.com/vs/](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)

- <span data-ttu-id="c207f-125">**Visual Studio Code**。</span><span class="sxs-lookup"><span data-stu-id="c207f-125">**Visual Studio Code**.</span></span> <span data-ttu-id="c207f-126">官方網站。</span><span class="sxs-lookup"><span data-stu-id="c207f-126">Official site.</span></span> \
  <https://code.visualstudio.com/download>

- <span data-ttu-id="c207f-127">**適用於 Windows Community Edition (CE) 的 Docker Desktop** </span><span class="sxs-lookup"><span data-stu-id="c207f-127">**Docker Desktop for Windows Community Edition (CE)** </span></span>\
  [https://hub.docker.com/editions/community/docker-ce-desktop-windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
  
- <span data-ttu-id="c207f-128">**適用於 Mac Community Edition (CE) 的 Docker Desktop** </span><span class="sxs-lookup"><span data-stu-id="c207f-128">**Docker Desktop for Mac Community Edition (CE)** </span></span>\
  [https://hub.docker.com/editions/community/docker-ce-desktop-mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)

## <a name="net-languages-and-frameworks-for-docker-containers"></a><span data-ttu-id="c207f-129">Docker 容器的 .NET 語言和 Framework</span><span class="sxs-lookup"><span data-stu-id="c207f-129">.NET languages and frameworks for Docker containers</span></span>

<span data-ttu-id="c207f-130">如本指南稍早章節所述，開發 Docker 容器化 .NET 應用程式時，您可以使用 .NET Framework、.NET Core 或開放原始碼 Mono 專案。</span><span class="sxs-lookup"><span data-stu-id="c207f-130">As mentioned in earlier sections of this guide, you can use .NET Framework, .NET Core, or the open-source Mono project when developing Docker containerized .NET applications.</span></span> <span data-ttu-id="c207f-131">當目標設為 Linux 或 Windows 容器時，您可以根據所使用的 .NET Framework，以 C\#、F\# 或 Visual Basic 進行開發。</span><span class="sxs-lookup"><span data-stu-id="c207f-131">You can develop in C\#, F\#, or Visual Basic when targeting Linux or Windows Containers, depending on which .NET framework is in use.</span></span> <span data-ttu-id="c207f-132">如需 .NET 語言的詳細資訊，請參閱部落格文章 [The .NET Language Strategy](https://devblogs.microsoft.com/dotnet/the-net-language-strategy/) (.NET 語言策略)。</span><span class="sxs-lookup"><span data-stu-id="c207f-132">For more details about.NET languages, see the blog post [The .NET Language Strategy](https://devblogs.microsoft.com/dotnet/the-net-language-strategy/).</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="c207f-133">[上一頁](../architect-microservice-container-applications/scalable-available-multi-container-microservice-applications.md)
>[下一頁](docker-app-development-workflow.md)</span><span class="sxs-lookup"><span data-stu-id="c207f-133">[Previous](../architect-microservice-container-applications/scalable-available-multi-container-microservice-applications.md)
[Next](docker-app-development-workflow.md)</span></span>