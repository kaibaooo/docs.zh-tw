---
title: 在 GitHub 問題多類別分類案例中使用 ML.NET
description: 了解如何在多類別分類案例中使用 ML.NET 來分類 GitHub 問題，以將它們指派至特定區域。
ms.date: 01/24/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 6f01357906fd4398f68dadfb35dbce816f4302c0
ms.sourcegitcommit: d9a0071d0fd490ae006c816f78a563b9946e269a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/25/2019
ms.locfileid: "55066198"
---
# <a name="tutorial-use-mlnet-in-a-multiclass-classification-scenario-to-classify-github-issues"></a><span data-ttu-id="a24e5-103">教學課程：在多類別分類案例中使用 ML.NET 來分類 GitHub 問題。</span><span class="sxs-lookup"><span data-stu-id="a24e5-103">Tutorial: Use ML.NET in a multiclass classification scenario to classify GitHub issues.</span></span>

<span data-ttu-id="a24e5-104">此範例教學課程說明如何使用 Visual Studio 2017 中的 C#，透過 .NET Core 主控台應用程式，以 ML.NET 建立 GitHub 問題分類器。</span><span class="sxs-lookup"><span data-stu-id="a24e5-104">This sample tutorial illustrates using ML.NET to create a GitHub issue classifier via a .NET Core console application using C# in Visual Studio 2017.</span></span>

<span data-ttu-id="a24e5-105">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="a24e5-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="a24e5-106">了解問題</span><span class="sxs-lookup"><span data-stu-id="a24e5-106">Understand the problem</span></span>
> * <span data-ttu-id="a24e5-107">選取適當的機器學習工作</span><span class="sxs-lookup"><span data-stu-id="a24e5-107">Select the appropriate machine learning task</span></span>
> * <span data-ttu-id="a24e5-108">準備您的資料</span><span class="sxs-lookup"><span data-stu-id="a24e5-108">Prepare your data</span></span>
> * <span data-ttu-id="a24e5-109">建立學習管線</span><span class="sxs-lookup"><span data-stu-id="a24e5-109">Create the learning pipeline</span></span>
> * <span data-ttu-id="a24e5-110">載入分類器</span><span class="sxs-lookup"><span data-stu-id="a24e5-110">Load a classifier</span></span>
> * <span data-ttu-id="a24e5-111">將模型定型</span><span class="sxs-lookup"><span data-stu-id="a24e5-111">Train the model</span></span>
> * <span data-ttu-id="a24e5-112">使用不同的資料集來評估模型</span><span class="sxs-lookup"><span data-stu-id="a24e5-112">Evaluate the model with a different dataset</span></span>
> * <span data-ttu-id="a24e5-113">使用模型來預測測試資料結果的單一執行個體</span><span class="sxs-lookup"><span data-stu-id="a24e5-113">Predict a single instance of test data outcome with the model</span></span>
> * <span data-ttu-id="a24e5-114">使用載入模型預測測試資料結果</span><span class="sxs-lookup"><span data-stu-id="a24e5-114">Predict the test data outcomes with a loaded model</span></span>

> [!NOTE]
> <span data-ttu-id="a24e5-115">本主題涉及 ML.NET，此功能目前為公開預覽版，因此内容可能會有變更。</span><span class="sxs-lookup"><span data-stu-id="a24e5-115">This topic refers to ML.NET, which is currently in Preview, and material may be subject to change.</span></span> <span data-ttu-id="a24e5-116">如需詳細資訊，請瀏覽 [ML.NET 簡介](https://www.microsoft.com/net/learn/apps/machine-learning-and-ai/ml-dotnet) (英文)。</span><span class="sxs-lookup"><span data-stu-id="a24e5-116">For more information, visit [the ML.NET introduction](https://www.microsoft.com/net/learn/apps/machine-learning-and-ai/ml-dotnet).</span></span>

## <a name="github-issue-sample-overview"></a><span data-ttu-id="a24e5-117">GitHub 問題範例概觀</span><span class="sxs-lookup"><span data-stu-id="a24e5-117">GitHub issue sample overview</span></span>

<span data-ttu-id="a24e5-118">該範例是主控台應用程式，會使用 ML.NET 來定型模型，以針對 GitHub 問題分類並預測 Area 標籤。</span><span class="sxs-lookup"><span data-stu-id="a24e5-118">The sample is a console app that uses ML.NET to train a model that classifies and predicts the Area label for a GitHub issue.</span></span> <span data-ttu-id="a24e5-119">它也會使用第二資料集來評估模型，以進行品質分析。</span><span class="sxs-lookup"><span data-stu-id="a24e5-119">It also evaluates the model with a second dataset for quality analysis.</span></span> <span data-ttu-id="a24e5-120">問題的資料集來自 dotnet/corefx GitHub 存放庫。</span><span class="sxs-lookup"><span data-stu-id="a24e5-120">The issue datasets are from the dotnet/corefx GitHub repo.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a24e5-121">必要條件</span><span class="sxs-lookup"><span data-stu-id="a24e5-121">Prerequisites</span></span>

* <span data-ttu-id="a24e5-122">已安裝「.NET Core 跨平台開發」工作負載的 [Visual Studio 2017 15.6 或更新版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)。</span><span class="sxs-lookup"><span data-stu-id="a24e5-122">[Visual Studio 2017 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the ".NET Core cross-platform development" workload installed.</span></span>

* <span data-ttu-id="a24e5-123">[Github issues tab separated file (issues_train.tsv)](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) (Github 問題定位字元分隔檔案 (issues_train.tsv))。</span><span class="sxs-lookup"><span data-stu-id="a24e5-123">The [Github issues tab separated file (issues_train.tsv)](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv).</span></span>
* <span data-ttu-id="a24e5-124">[Github issues test tab separated file (issues_test.tsv)](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) (Github 問題測試定位字元分隔檔案 (issues_test.tsv))。</span><span class="sxs-lookup"><span data-stu-id="a24e5-124">The [Github issues test tab separated file (issues_test.tsv)](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv).</span></span>

## <a name="machine-learning-workflow"></a><span data-ttu-id="a24e5-125">機器學習工作流程</span><span class="sxs-lookup"><span data-stu-id="a24e5-125">Machine learning workflow</span></span>

<span data-ttu-id="a24e5-126">本教學課程依循機器學習工作流程，可讓程序按順序移入。</span><span class="sxs-lookup"><span data-stu-id="a24e5-126">This tutorial follows a machine learning workflow that enables the process to move in an orderly fashion.</span></span>

<span data-ttu-id="a24e5-127">工作流程階段如下：</span><span class="sxs-lookup"><span data-stu-id="a24e5-127">The workflow phases are as follows:</span></span>

1. <span data-ttu-id="a24e5-128">**了解問題**</span><span class="sxs-lookup"><span data-stu-id="a24e5-128">**Understand the problem**</span></span>
2. <span data-ttu-id="a24e5-129">**準備您的資料**</span><span class="sxs-lookup"><span data-stu-id="a24e5-129">**Prepare your data**</span></span>
   * <span data-ttu-id="a24e5-130">**載入資料**</span><span class="sxs-lookup"><span data-stu-id="a24e5-130">**Load the data**</span></span>
   * <span data-ttu-id="a24e5-131">**擷取特徵 (轉換您的資料)**</span><span class="sxs-lookup"><span data-stu-id="a24e5-131">**Extract features (Transform your data)**</span></span>
3. <span data-ttu-id="a24e5-132">**建置和定型**</span><span class="sxs-lookup"><span data-stu-id="a24e5-132">**Build and train**</span></span> 
   * <span data-ttu-id="a24e5-133">**將模型定型**</span><span class="sxs-lookup"><span data-stu-id="a24e5-133">**Train the model**</span></span>
   * <span data-ttu-id="a24e5-134">**評估模型**</span><span class="sxs-lookup"><span data-stu-id="a24e5-134">**Evaluate the model**</span></span>
4. <span data-ttu-id="a24e5-135">**執行**</span><span class="sxs-lookup"><span data-stu-id="a24e5-135">**Run**</span></span>
   * <span data-ttu-id="a24e5-136">**模型取用**</span><span class="sxs-lookup"><span data-stu-id="a24e5-136">**Model consumption**</span></span>

### <a name="understand-the-problem"></a><span data-ttu-id="a24e5-137">了解問題</span><span class="sxs-lookup"><span data-stu-id="a24e5-137">Understand the problem</span></span>

<span data-ttu-id="a24e5-138">您必須先了解問題，以便將其分解成可支援模型建置和定型的組件。</span><span class="sxs-lookup"><span data-stu-id="a24e5-138">You first need to understand the problem, so you can break it down to parts that can support building and training the model.</span></span> <span data-ttu-id="a24e5-139">細分問題可讓您預測及評估結果。</span><span class="sxs-lookup"><span data-stu-id="a24e5-139">Breaking  down the problem allows you to predict and evaluate the results.</span></span>

<span data-ttu-id="a24e5-140">本教學課程旨在釐清傳入的 GitHub 問題屬於哪個區域，才能正確地標記這些問題，方便安排優先順序與排程。</span><span class="sxs-lookup"><span data-stu-id="a24e5-140">The problem for this tutorial is to understand what area incoming GitHub issues belong to in order to label them correctly for prioritization and scheduling.</span></span>

<span data-ttu-id="a24e5-141">您可以將問題細分如下：</span><span class="sxs-lookup"><span data-stu-id="a24e5-141">You can break down the problem to the following:</span></span>

* <span data-ttu-id="a24e5-142">問題標題文字</span><span class="sxs-lookup"><span data-stu-id="a24e5-142">the issue title text</span></span>
* <span data-ttu-id="a24e5-143">問題描述文字</span><span class="sxs-lookup"><span data-stu-id="a24e5-143">the issue description text</span></span>
* <span data-ttu-id="a24e5-144">模型定型資料的區域值</span><span class="sxs-lookup"><span data-stu-id="a24e5-144">an area value for the model training data</span></span>
* <span data-ttu-id="a24e5-145">您可操作性評估然後使用的預測區域值</span><span class="sxs-lookup"><span data-stu-id="a24e5-145">a predicted area value that you can evaluate and then use operationally</span></span>

<span data-ttu-id="a24e5-146">接著，您需要**判斷**該區域，這可協助您選取機器學習工作。</span><span class="sxs-lookup"><span data-stu-id="a24e5-146">You then need to **determine** the area, which helps you with the machine learning task selection.</span></span>

## <a name="select-the-appropriate-machine-learning-task"></a><span data-ttu-id="a24e5-147">選取適當的機器學習工作</span><span class="sxs-lookup"><span data-stu-id="a24e5-147">Select the appropriate machine learning task</span></span>

<span data-ttu-id="a24e5-148">處理此問題時，您會知道下列事實：</span><span class="sxs-lookup"><span data-stu-id="a24e5-148">With this problem, you know the following facts:</span></span>

<span data-ttu-id="a24e5-149">定型資料：</span><span class="sxs-lookup"><span data-stu-id="a24e5-149">Training data:</span></span>

<span data-ttu-id="a24e5-150">標記 GitHub 問題的區域有幾種 (**Area**)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a24e5-150">GitHub issues can be labeled in several areas (**Area**) as in the following examples:</span></span>

* <span data-ttu-id="a24e5-151">area-System.Numerics</span><span class="sxs-lookup"><span data-stu-id="a24e5-151">area-System.Numerics</span></span>
* <span data-ttu-id="a24e5-152">area-System.Xml</span><span class="sxs-lookup"><span data-stu-id="a24e5-152">area-System.Xml</span></span>
* <span data-ttu-id="a24e5-153">area-Infrastructure</span><span class="sxs-lookup"><span data-stu-id="a24e5-153">area-Infrastructure</span></span>
* <span data-ttu-id="a24e5-154">area-System.Linq</span><span class="sxs-lookup"><span data-stu-id="a24e5-154">area-System.Linq</span></span>
* <span data-ttu-id="a24e5-155">area-System.IO</span><span class="sxs-lookup"><span data-stu-id="a24e5-155">area-System.IO</span></span>

<span data-ttu-id="a24e5-156">預測新 GitHub 問題的 **Area**，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a24e5-156">Predict the **Area** of a new GitHub Issue such as in the following examples:</span></span>

* <span data-ttu-id="a24e5-157">Contract.Assert 與 Debug.Assert</span><span class="sxs-lookup"><span data-stu-id="a24e5-157">Contract.Assert vs Debug.Assert</span></span>
* <span data-ttu-id="a24e5-158">讓欄位在 System.Xml 中是唯讀的</span><span class="sxs-lookup"><span data-stu-id="a24e5-158">Make fields readonly in System.Xml</span></span>

<span data-ttu-id="a24e5-159">分類機器學習工作最適合用於此案例。</span><span class="sxs-lookup"><span data-stu-id="a24e5-159">The classification machine learning task is best suited for this scenario.</span></span>

### <a name="about-the-classification-task"></a><span data-ttu-id="a24e5-160">關於分類工作</span><span class="sxs-lookup"><span data-stu-id="a24e5-160">About the classification task</span></span>

<span data-ttu-id="a24e5-161">分類是一項機器學習工作，會使用資料來**判斷**項目或資料列的分類、類型或類別。</span><span class="sxs-lookup"><span data-stu-id="a24e5-161">Classification is a machine learning task that uses data to **determine** the category, type, or class of an item or row of data.</span></span> <span data-ttu-id="a24e5-162">例如，您可以使用分類來：</span><span class="sxs-lookup"><span data-stu-id="a24e5-162">For example, you can use classification to:</span></span>

* <span data-ttu-id="a24e5-163">識別情感是正面還是負面。</span><span class="sxs-lookup"><span data-stu-id="a24e5-163">Identify sentiment as positive or negative.</span></span>
* <span data-ttu-id="a24e5-164">將電子郵件分類為垃圾郵件或正常郵件。</span><span class="sxs-lookup"><span data-stu-id="a24e5-164">Classify email as spam, junk, or good.</span></span>
* <span data-ttu-id="a24e5-165">判斷病患的實驗樣本是否有癌症病兆。</span><span class="sxs-lookup"><span data-stu-id="a24e5-165">Determine whether a patient's lab sample is cancerous.</span></span>
* <span data-ttu-id="a24e5-166">依客戶回應銷售活動的傾向來分類客戶。</span><span class="sxs-lookup"><span data-stu-id="a24e5-166">Categorize customers by their propensity to respond to a sales campaign.</span></span>

<span data-ttu-id="a24e5-167">分類工作通常是下列其中一個類型：</span><span class="sxs-lookup"><span data-stu-id="a24e5-167">Classification tasks are frequently one of the following types:</span></span>

* <span data-ttu-id="a24e5-168">二元：不是 A 就是 B。</span><span class="sxs-lookup"><span data-stu-id="a24e5-168">Binary: either A or B.</span></span>
* <span data-ttu-id="a24e5-169">多元分類：可使用單一模型來預測的多重分類。</span><span class="sxs-lookup"><span data-stu-id="a24e5-169">Multiclass: multiple categories that can be predicted by using a single model.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="a24e5-170">建立主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="a24e5-170">Create a console application</span></span>

### <a name="create-a-project"></a><span data-ttu-id="a24e5-171">建立專案</span><span class="sxs-lookup"><span data-stu-id="a24e5-171">Create a project</span></span>

1. <span data-ttu-id="a24e5-172">開啟 Visual Studio 2017。</span><span class="sxs-lookup"><span data-stu-id="a24e5-172">Open Visual Studio 2017.</span></span> <span data-ttu-id="a24e5-173">從功能表列中選取 [檔案]  >  [新增]  >  [專案]。</span><span class="sxs-lookup"><span data-stu-id="a24e5-173">Select **File** > **New** > **Project** from the menu bar.</span></span> <span data-ttu-id="a24e5-174">在 [新增專案] 對話方塊中，選取 [Visual C#] 節點，然後選取 [.NET Core] 節點。</span><span class="sxs-lookup"><span data-stu-id="a24e5-174">In the **New Project** dialog, select the **Visual C#** node followed by the **.NET Core** node.</span></span> <span data-ttu-id="a24e5-175">然後選取 [主控台應用程式 (.NET Core)] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="a24e5-175">Then select the **Console App (.NET Core)** project template.</span></span> <span data-ttu-id="a24e5-176">在 [名稱] 文字方塊中，輸入 "SentimentAnalysis"，然後選取 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a24e5-176">In the **Name** text box, type "SentimentAnalysis" and then select the **OK** button.</span></span>

2. <span data-ttu-id="a24e5-177">在您的專案中建立一個名為 *Data* 的目錄以儲存資料集檔案：</span><span class="sxs-lookup"><span data-stu-id="a24e5-177">Create a directory named *Data* in your project to save your data set files:</span></span>

    <span data-ttu-id="a24e5-178">在 [方案總管] 中，於您的專案上按一下滑鼠右鍵，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="a24e5-178">In **Solution Explorer**, right-click on your project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="a24e5-179">輸入 "Data"，然後按 Enter。</span><span class="sxs-lookup"><span data-stu-id="a24e5-179">Type "Data" and hit Enter.</span></span>

3. <span data-ttu-id="a24e5-180">安裝「Microsoft.ML NuGet 套件」：</span><span class="sxs-lookup"><span data-stu-id="a24e5-180">Install the **Microsoft.ML NuGet Package**:</span></span>

    <span data-ttu-id="a24e5-181">在 [方案總管] 中，於您的專案上按一下滑鼠右鍵，然後選取 [管理 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="a24e5-181">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="a24e5-182">選擇 "nuget.org" 作為 [套件來源]，選取 [瀏覽] 索引標籤、搜尋 **Microsoft.ML**、從清單中選取該套件，然後選取 [安裝] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a24e5-182">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML**, select that package in the list, and select the **Install** button.</span></span> <span data-ttu-id="a24e5-183">在 [預覽變更] 對話方塊上，選取 [確定] 按鈕，然後在 [授權接受] 對話方塊上，如果您同意所列套件的授權條款，請選取 [我接受]。</span><span class="sxs-lookup"><span data-stu-id="a24e5-183">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>

### <a name="prepare-your-data"></a><span data-ttu-id="a24e5-184">準備您的資料</span><span class="sxs-lookup"><span data-stu-id="a24e5-184">Prepare your data</span></span>

1. <span data-ttu-id="a24e5-185">下載 [issues_train.tsv](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) 和 [issues_test.tsv](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) 資料集，並將它們儲存至先前建立的 *Data* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a24e5-185">Download the [issues_train.tsv](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) and the [issues_test.tsv](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) data sets and save them to the *Data* folder previously created.</span></span> <span data-ttu-id="a24e5-186">第一個資料集會將機器學習模型定型，第二個資料集則可用來評估您模型的準確率。</span><span class="sxs-lookup"><span data-stu-id="a24e5-186">The first dataset trains the machine learning model and the second can be used to evaluate how accurate your model is.</span></span>

2. <span data-ttu-id="a24e5-187">在 [方案總管] 中，於每個 \*.tsv 檔案上按一下滑鼠右鍵，然後選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="a24e5-187">In Solution Explorer, right-click each of the \*.tsv files and select **Properties**.</span></span> <span data-ttu-id="a24e5-188">在 [進階] 底下，將 [複製到輸出目錄] 的值變更為 [有更新時才複製]。</span><span class="sxs-lookup"><span data-stu-id="a24e5-188">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

### <a name="create-classes-and-define-paths"></a><span data-ttu-id="a24e5-189">建立類別及定義路徑</span><span class="sxs-lookup"><span data-stu-id="a24e5-189">Create classes and define paths</span></span>

<span data-ttu-id="a24e5-190">在 *Program.cs* 檔案頂端新增下列額外的 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="a24e5-190">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

[!code-csharp[AddUsings](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddUsings)]

<span data-ttu-id="a24e5-191">您需要建立三個全域欄位來持有最近下載之檔案的路徑，還要建立 `TextLoader` 的全域變數：</span><span class="sxs-lookup"><span data-stu-id="a24e5-191">You need to create three global fields to hold the paths to the recently downloaded files, and a global variable for the `TextLoader`:</span></span>

* <span data-ttu-id="a24e5-192">`_trainDataPath` 包含用來將模型定型的資料集路徑。</span><span class="sxs-lookup"><span data-stu-id="a24e5-192">`_trainDataPath` has the path to the dataset used to train the model.</span></span>
* <span data-ttu-id="a24e5-193">`_testDataPath` 包含用來評估模型的資料集路徑。</span><span class="sxs-lookup"><span data-stu-id="a24e5-193">`_testDataPath` has the path to the dataset used to evaluate the model.</span></span>
* <span data-ttu-id="a24e5-194">`_modelPath` 包含用來儲存定型模型的路徑。</span><span class="sxs-lookup"><span data-stu-id="a24e5-194">`_modelPath` has the path where the trained model is saved.</span></span>
* <span data-ttu-id="a24e5-195">`_mlContext` 是提供處理內容的 <xref:Microsoft.ML.MLContext>。</span><span class="sxs-lookup"><span data-stu-id="a24e5-195">`_mlContext` is the <xref:Microsoft.ML.MLContext> that provides processing context.</span></span>
* <span data-ttu-id="a24e5-196">`_trainingDataView` 是用來處理定型資料集的 <xref:Microsoft.ML.Data.IDataView>。</span><span class="sxs-lookup"><span data-stu-id="a24e5-196">`_trainingDataView` is the <xref:Microsoft.ML.Data.IDataView> used to process the training dataset.</span></span>
* <span data-ttu-id="a24e5-197">`_predEngine` 是用於單一預測的 <xref:Microsoft.ML.PredictionEngine%602>。</span><span class="sxs-lookup"><span data-stu-id="a24e5-197">`_predEngine` is the <xref:Microsoft.ML.PredictionEngine%602> used for single predictions.</span></span>
* <span data-ttu-id="a24e5-198">`_reader` 是 <xref:Microsoft.ML.Data.TextLoader>，用來載入並轉換資料集。</span><span class="sxs-lookup"><span data-stu-id="a24e5-198">`_reader` is the <xref:Microsoft.ML.Data.TextLoader> used to load and transform the datasets.</span></span>

<span data-ttu-id="a24e5-199">將下列程式碼新增至 `Main` 方法正上方的一行，以指定這些路徑和其他變數：</span><span class="sxs-lookup"><span data-stu-id="a24e5-199">Add the following code to the line right above the `Main` method to specify those paths and the other variables:</span></span>

[!code-csharp[DeclareGlobalVariables](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DeclareGlobalVariables)]

<span data-ttu-id="a24e5-200">您必須為輸入資料和預測建立一些類別。</span><span class="sxs-lookup"><span data-stu-id="a24e5-200">You need to create some classes for your input data and predictions.</span></span> <span data-ttu-id="a24e5-201">將新類別新增至專案：</span><span class="sxs-lookup"><span data-stu-id="a24e5-201">Add a new class to your project:</span></span>

1. <span data-ttu-id="a24e5-202">在 [方案總管] 中，於專案上按一下滑鼠右鍵，然後選取 [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="a24e5-202">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="a24e5-203">在 [新增項目] 對話方塊中，選取 [類別]，然後將 [名稱] 欄位變更為 *GitHubIssueData.cs*。</span><span class="sxs-lookup"><span data-stu-id="a24e5-203">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *GitHubIssueData.cs*.</span></span> <span data-ttu-id="a24e5-204">接著，選取 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a24e5-204">Then, select the **Add** button.</span></span>

    <span data-ttu-id="a24e5-205">*GitHubIssueData.cs* 檔案隨即在程式碼編輯器中開啟。</span><span class="sxs-lookup"><span data-stu-id="a24e5-205">The *GitHubIssueData.cs* file opens in the code editor.</span></span> <span data-ttu-id="a24e5-206">將下列 `using` 陳述式新增至 *GitHubIssueData.cs* 最上方：</span><span class="sxs-lookup"><span data-stu-id="a24e5-206">Add the following `using` statement to the top of *GitHubIssueData.cs*:</span></span>

[!code-csharp[AddUsings](../../../samples/machine-learning/tutorials/GitHubIssueClassification/GitHubIssueData.cs#AddUsings)]

<span data-ttu-id="a24e5-207">移除現有的類別定義，然後將下列程式碼 (具有 `GitHubIssue` 和 `IssuePrediction` 這兩個類別) 新增至 *GitHubIssueData.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="a24e5-207">Remove the existing class definition and add the following code, which has two classes `GitHubIssue` and `IssuePrediction`, to the *GitHubIssueData.cs* file:</span></span>

[!code-csharp[DeclareTypes](../../../samples/machine-learning/tutorials/GitHubIssueClassification/GitHubIssueData.cs#DeclareTypes)]

<span data-ttu-id="a24e5-208">`GitHubIssue` 是輸入資料集類別，並具有下列 <xref:System.String> 欄位：</span><span class="sxs-lookup"><span data-stu-id="a24e5-208">`GitHubIssue` is the input dataset class and has the following <xref:System.String> fields:</span></span>

* <span data-ttu-id="a24e5-209">`ID` 包含 GitHub 問題識別碼的值</span><span class="sxs-lookup"><span data-stu-id="a24e5-209">`ID` contains a value for the GitHub issue ID</span></span>
* <span data-ttu-id="a24e5-210">`Area` 包含 `Area` 標籤的值</span><span class="sxs-lookup"><span data-stu-id="a24e5-210">`Area` contains a value for the `Area` label</span></span>
* <span data-ttu-id="a24e5-211">`Title` 包含 GitHub 問題標題</span><span class="sxs-lookup"><span data-stu-id="a24e5-211">`Title` contains the GitHub issue title</span></span>
* <span data-ttu-id="a24e5-212">`Description` 包含 GitHub 問題描述</span><span class="sxs-lookup"><span data-stu-id="a24e5-212">`Description` contains the GitHub issue description</span></span>

<span data-ttu-id="a24e5-213">`IssuePrediction` 是在模型定型後，用來進行預測的類別。</span><span class="sxs-lookup"><span data-stu-id="a24e5-213">`IssuePrediction` is the class used for prediction after the model has been trained.</span></span> <span data-ttu-id="a24e5-214">它包含單一布林值 `string` (`Area`) 和 `PredictedLabel` `ColumnName` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a24e5-214">It has a single `string` (`Area`) and a `PredictedLabel` `ColumnName` attribute.</span></span> <span data-ttu-id="a24e5-215">`Label` 會用來建立和定型模型，也會與第二資料集搭配使用來評估模型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-215">The `Label` is used to create and train the model, and it's also used with a second dataset to evaluate the model.</span></span> <span data-ttu-id="a24e5-216">`PredictedLabel` 的使用時機是在進行預測和評估的期間。</span><span class="sxs-lookup"><span data-stu-id="a24e5-216">The `PredictedLabel` is used during prediction and evaluation.</span></span> <span data-ttu-id="a24e5-217">就評估而言，會使用含有定型資料、預設值及模型的輸入。</span><span class="sxs-lookup"><span data-stu-id="a24e5-217">For evaluation, an input with training data, the predicted values, and the model are used.</span></span>

<span data-ttu-id="a24e5-218">使用 ML.NET 建置模型時，您要從建立 <xref:Microsoft.ML.MLContext> 開始。</span><span class="sxs-lookup"><span data-stu-id="a24e5-218">When building a model with ML.NET, you start by creating an <xref:Microsoft.ML.MLContext>.</span></span> <span data-ttu-id="a24e5-219">這在概念上類似於在 Entity Framework 中使用 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="a24e5-219">This is comparable conceptually to using `DbContext` in Entity Framework.</span></span> <span data-ttu-id="a24e5-220">環境能夠為 ML 作業提供內容，可用來進行例外狀況追蹤和記錄。</span><span class="sxs-lookup"><span data-stu-id="a24e5-220">The environment provides a context for your ML job that can be used for exception tracking and logging.</span></span>

### <a name="initialize-variables-in-main"></a><span data-ttu-id="a24e5-221">在 Main 中初始化變數</span><span class="sxs-lookup"><span data-stu-id="a24e5-221">Initialize variables in Main</span></span>

<span data-ttu-id="a24e5-222">將具有包含隨機種子 (`seed: 0`) 之新 `MLContext` 執行個體的 `_mlContext` 全域變數初始化，以讓多個定型間的結果可重複/具有確定性。</span><span class="sxs-lookup"><span data-stu-id="a24e5-222">Initialize the `_mlContext` global variable  with a new instance of `MLContext` with a random seed (`seed: 0`) for repeatable/deterministic results across multiple trainings.</span></span>  <span data-ttu-id="a24e5-223">在 `Main` 方法中，以下列程式碼取代 `Console.WriteLine("Hello World!")`：</span><span class="sxs-lookup"><span data-stu-id="a24e5-223">Replace the `Console.WriteLine("Hello World!")` line with the following code in the `Main` method:</span></span>

[!code-csharp[CreateMLContext](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreateMLContext)]

## <a name="load-the-data"></a><span data-ttu-id="a24e5-224">載入資料</span><span class="sxs-lookup"><span data-stu-id="a24e5-224">Load the data</span></span>

<span data-ttu-id="a24e5-225">接著，您將 `_trainingDataView` <xref:Microsoft.ML.Data.IDataView> 全域變數初始化，並使用 `_trainDataPath` 參數來載入資料。</span><span class="sxs-lookup"><span data-stu-id="a24e5-225">Next, initialize the `_trainingDataView` <xref:Microsoft.ML.Data.IDataView> global variable and load the data with the `_trainDataPath` parameter.</span></span>

 <span data-ttu-id="a24e5-226">作為 `Transforms` 的輸入和輸出，`DataView` 是基本的資料管線類型，相當於 `LINQ` 的 `IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="a24e5-226">As the input and output of `Transforms`, a `DataView` is the fundamental data pipeline type, comparable to `IEnumerable` for `LINQ`.</span></span>

<span data-ttu-id="a24e5-227">在 ML.NET 中，資料相當於 `SQL view`。</span><span class="sxs-lookup"><span data-stu-id="a24e5-227">In ML.NET, data is similar to a `SQL view`.</span></span> <span data-ttu-id="a24e5-228">它是延遲評估、結構描述化且異質性的。</span><span class="sxs-lookup"><span data-stu-id="a24e5-228">It is lazily evaluated, schematized, and heterogenous.</span></span> <span data-ttu-id="a24e5-229">物件是管線的第一個部分，並且會載入資料。</span><span class="sxs-lookup"><span data-stu-id="a24e5-229">The object is the first part of the pipeline, and loads the data.</span></span> <span data-ttu-id="a24e5-230">在本教學課程中，它會載入具有問題標題、描述和相對應區域 GitHub 標籤的資料集。</span><span class="sxs-lookup"><span data-stu-id="a24e5-230">For this tutorial, it loads a dataset with issue titles, descriptions, and corresponding area GitHub label.</span></span> <span data-ttu-id="a24e5-231">`DataView` 用於建立並定型模型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-231">The `DataView` is used to create and train the model.</span></span>

<span data-ttu-id="a24e5-232">因為您先前建立的 `GitHubIssue` 資料模型類型符合資料集結構描述，所以您可以將初始化、對應和資料集載入合併成一行程式碼。</span><span class="sxs-lookup"><span data-stu-id="a24e5-232">Since your previously created `GitHubIssue` data model type matches the dataset schema, you can combine the initialization, mapping, and dataset loading into one line of code.</span></span>

<span data-ttu-id="a24e5-233">該行第一個部分 (`CreateTextReader<GitHubIssue>(hasHeader: true)`) 會從 `GitHubIssue` 資料模型類型推斷資料集結構描述，並使用資料集標頭，來建立 <xref:Microsoft.ML.Data.TextLoader>。</span><span class="sxs-lookup"><span data-stu-id="a24e5-233">The first part of the line (`CreateTextReader<GitHubIssue>(hasHeader: true)`) creates a <xref:Microsoft.ML.Data.TextLoader> by inferencing the dataset schema from the `GitHubIssue` data model type and using the dataset header.</span></span>

<span data-ttu-id="a24e5-234">您先前已在建立 `GitHubIssue` 類別時，定義了資料結構描述。</span><span class="sxs-lookup"><span data-stu-id="a24e5-234">You defined the data schema previously when you created the `GitHubIssue` class.</span></span> <span data-ttu-id="a24e5-235">針對您的結構描述：</span><span class="sxs-lookup"><span data-stu-id="a24e5-235">For your schema:</span></span>

* <span data-ttu-id="a24e5-236">第一個資料行 `ID`(GitHub 問題識別碼)</span><span class="sxs-lookup"><span data-stu-id="a24e5-236">the first column `ID` (GitHub Issue ID)</span></span>
* <span data-ttu-id="a24e5-237">第二個資料行 `Area`(用於定型的預測)</span><span class="sxs-lookup"><span data-stu-id="a24e5-237">the second column `Area` (the prediction for training)</span></span>
* <span data-ttu-id="a24e5-238">第三個資料行 `Title`(GitHub 問題標題) 是第一個用來預測 `Area` 的[特徵](../resources/glossary.md##feature)</span><span class="sxs-lookup"><span data-stu-id="a24e5-238">the third column `Title` (GitHub issue title) is the first [feature](../resources/glossary.md##feature)  used for predicting the `Area`</span></span>
* <span data-ttu-id="a24e5-239">第四個資料行 `Description` 是第二個用來預測 `Area`的特徵</span><span class="sxs-lookup"><span data-stu-id="a24e5-239">the fourth column  `Description` is the second feature used for predicting the `Area`</span></span>

<span data-ttu-id="a24e5-240">該行第二個部分 (`.Read(_trainDataPath)`) 會使用 <xref:Microsoft.ML.Data.TextLoader.Read%2A> 方法，將定型文字檔以 `_trainDataPath` 載入 `IDataView` (`_trainingDataView`) 全域變數。</span><span class="sxs-lookup"><span data-stu-id="a24e5-240">The second part of the line  (`.Read(_trainDataPath)`) uses <xref:Microsoft.ML.Data.TextLoader.Read%2A> method to load the training text file using `_trainDataPath` into the `IDataView` (`_trainingDataView`) global variable.</span></span>  

<span data-ttu-id="a24e5-241">若要初始化並載入 `_trainingDataView` 全域變數以將其用於管線，請在 `mlContext` 初始化後，新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="a24e5-241">To initialize and load the `_trainingDataView` global variable in order to use it for the pipeline, add the following code after the  `mlContext` initialization:</span></span>

[!code-csharp[LoadTrainData](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadTrainData)]


<span data-ttu-id="a24e5-242">將下列程式碼加入為 `Main` 方法中的下一行程式碼：</span><span class="sxs-lookup"><span data-stu-id="a24e5-242">Add the following as the next line of code in the `Main` method:</span></span>

[!code-csharp[CallProcessData](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallProcessData)]

<span data-ttu-id="a24e5-243">`ProcessData` 方法會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="a24e5-243">The `ProcessData` method executes the following tasks:</span></span>

* <span data-ttu-id="a24e5-244">擷取並轉換資料。</span><span class="sxs-lookup"><span data-stu-id="a24e5-244">Extracts and transforms the data.</span></span>
* <span data-ttu-id="a24e5-245">傳回處理管線。</span><span class="sxs-lookup"><span data-stu-id="a24e5-245">Returns the processing pipeline.</span></span>

<span data-ttu-id="a24e5-246">請使用下列程式碼，在緊接著 `Main` 方法之後，建立 `ProcessData` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-246">Create the `ProcessData` method, just after the `Main` method, using the following code:</span></span>

```csharp
public static EstimatorChain<ITransformer> ProcessData()
{

}
```

## <a name="extract-and-transform-the-data"></a><span data-ttu-id="a24e5-247">擷取並轉換資料</span><span class="sxs-lookup"><span data-stu-id="a24e5-247">Extract and transform the data</span></span>

<span data-ttu-id="a24e5-248">資料前處理和清除是相當重要的工作，發生在有效地使用資料集來進行機器學習之前。</span><span class="sxs-lookup"><span data-stu-id="a24e5-248">Pre-processing and cleaning data are important tasks that occur before a dataset is used effectively for machine learning.</span></span> <span data-ttu-id="a24e5-249">原始資料通常雜訊多且不可靠，而可能遺漏值。</span><span class="sxs-lookup"><span data-stu-id="a24e5-249">Raw data is often noisy and unreliable, and may be missing values.</span></span> <span data-ttu-id="a24e5-250">使用資料時，如果未進行這些模型化工作，可能會產生誤導的結果。</span><span class="sxs-lookup"><span data-stu-id="a24e5-250">Using data without these modeling tasks can produce misleading results.</span></span>

<span data-ttu-id="a24e5-251">ML.NET 的轉換管線撰寫一組自訂的轉換，可在定型或測試之前先套用到您的資料。</span><span class="sxs-lookup"><span data-stu-id="a24e5-251">ML.NET's transform pipelines compose a custom set of transforms that are applied to your data before training or testing.</span></span> <span data-ttu-id="a24e5-252">轉換的主要目的是將資料[特徵化](../resources/glossary.md#feature-engineering)。</span><span class="sxs-lookup"><span data-stu-id="a24e5-252">The transforms' primary purpose is data [featurization](../resources/glossary.md#feature-engineering).</span></span> <span data-ttu-id="a24e5-253">機器學習演算法了解[特徵化](../resources/glossary.md#feature)資料，因此下一步是將我們的文字資料轉換成 ML 演算法可辨識的格式。</span><span class="sxs-lookup"><span data-stu-id="a24e5-253">Machine learning algorithms understand [featurized](../resources/glossary.md#feature) data, so the next step is to transform our textual data into a format that our ML algorithms recognize.</span></span> <span data-ttu-id="a24e5-254">該格式為[數值向量](../resources/glossary.md#numerical-feature-vector)。</span><span class="sxs-lookup"><span data-stu-id="a24e5-254">That format is a [numeric vector](../resources/glossary.md#numerical-feature-vector).</span></span>

<span data-ttu-id="a24e5-255">在接下來的步驟中，我們透過 `GitHubIssue` 類別中定義的名稱來參考資料行。</span><span class="sxs-lookup"><span data-stu-id="a24e5-255">In the next steps, we refer to the columns by the names defined in the `GitHubIssue` class.</span></span>

<span data-ttu-id="a24e5-256">根據預設，在定型和評估模型時，**Label** 資料行中的值會視為要預測的正確值。</span><span class="sxs-lookup"><span data-stu-id="a24e5-256">When the model is trained and evaluated, by default, the values in the **Label** column are considered as correct values to be predicted.</span></span> <span data-ttu-id="a24e5-257">因為我們想要針對 `GitHubIssue` 預測 Area GitHub 標籤，所以請將 `Area` 資料行複製到 **Label** 資料行。</span><span class="sxs-lookup"><span data-stu-id="a24e5-257">As we want to predict the Area GitHub label for a `GitHubIssue`, copy the `Area` column into the **Label** column.</span></span> <span data-ttu-id="a24e5-258">作法是使用 `MLContext.Transforms.Conversion.MapValueToKey`，這是 <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A> 轉換類別的包裝函式。</span><span class="sxs-lookup"><span data-stu-id="a24e5-258">To do that, use the `MLContext.Transforms.Conversion.MapValueToKey`, which is a wrapper for the <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A> transformation class.</span></span>  <span data-ttu-id="a24e5-259">`MapValueToKey` 會傳回 <xref:Microsoft.ML.Data.EstimatorChain%601>，可作為有效的管線。</span><span class="sxs-lookup"><span data-stu-id="a24e5-259">The `MapValueToKey` returns an <xref:Microsoft.ML.Data.EstimatorChain%601> that will effectively be a pipeline.</span></span> <span data-ttu-id="a24e5-260">照一般作法為此 `pipeline` 命名，然後將定型程式附加至 `EstimatorChain`。</span><span class="sxs-lookup"><span data-stu-id="a24e5-260">Name this `pipeline` as you will then append the trainer to the `EstimatorChain`.</span></span> <span data-ttu-id="a24e5-261">將下列程式碼加入為下一行：</span><span class="sxs-lookup"><span data-stu-id="a24e5-261">Add this as the next line of code:</span></span>

[!code-csharp[MapValueToKey](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#MapValueToKey)]

<span data-ttu-id="a24e5-262">定型模型的演算法需要**數值**特徵，因此您接著必須呼叫 `mlContext.Transforms.Text.FeaturizeText`，這會將文字 (`Title` 和 `Description`) 資料行各轉換成數值向量，分別稱為 `TitleFeaturized` 和 `DescriptionFeaturized`。</span><span class="sxs-lookup"><span data-stu-id="a24e5-262">The algorithm that trains the model requires **numeric** features, so you have Next, call `mlContext.Transforms.Text.FeaturizeText` which featurizes the text (`Title` and `Description`) columns into a numeric vector for each called `TitleFeaturized` and `DescriptionFeaturized`.</span></span> <span data-ttu-id="a24e5-263">特徵轉換會指派不同的數值索引鍵值給每個資料行中的不同值，並由機器學習演算法使用。</span><span class="sxs-lookup"><span data-stu-id="a24e5-263">Featurizing assigns different numeric key values to the different values in each of the columns and is used by the machine learning algorithm.</span></span>
<span data-ttu-id="a24e5-264">將這兩個資料行的特徵轉換附加至管線，使用的程式碼如下：</span><span class="sxs-lookup"><span data-stu-id="a24e5-264">Append the featurization for both columns to the pipeline with the following code:</span></span>

[!code-csharp[FeaturizeText](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#FeaturizeText)]

<span data-ttu-id="a24e5-265">資料準備工作的最後一個步驟是使用 `Concatenate` 轉換類別，將所有特徵資料行合併為 **Features** 資料行。</span><span class="sxs-lookup"><span data-stu-id="a24e5-265">The last step in data preparation combines all of the feature columns into the **Features** column using the `Concatenate` transformation class.</span></span> <span data-ttu-id="a24e5-266">根據預設，學習演算法只會處理來自 **Features** 資料行的特徵。</span><span class="sxs-lookup"><span data-stu-id="a24e5-266">By default, a learning algorithm processes only features from the **Features** column.</span></span> <span data-ttu-id="a24e5-267">將這此轉換附加至管線，使用的程式碼如下：</span><span class="sxs-lookup"><span data-stu-id="a24e5-267">Append this transformation to the pipeline with the following code:</span></span>

[!code-csharp[Concatenate](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Concatenate)]

 <span data-ttu-id="a24e5-268">接著，附加 <xref:Microsoft.ML.Data.EstimatorChain`1.AppendCacheCheckpoint%2A> 來快取 DataView，以便在您多次逐一查看資料時，使用快取可獲得更高的效能，如下列程式碼所示</span><span class="sxs-lookup"><span data-stu-id="a24e5-268">Next, append a <xref:Microsoft.ML.Data.EstimatorChain`1.AppendCacheCheckpoint%2A> to cache the DataView so when you iterate over the data multiple times using the cache might get better performance, as with the following code</span></span>

[!code-csharp[AppendCache](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AppendCache)]

<span data-ttu-id="a24e5-269">在 `ProcessData` 方法的結尾傳回管線。</span><span class="sxs-lookup"><span data-stu-id="a24e5-269">Return the pipeline at the end of the `ProcessData` method.</span></span>

[!code-csharp[ReturnPipeline](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#ReturnPipeline)]

<span data-ttu-id="a24e5-270">這個步驟會處理前置處理/特徵轉換。</span><span class="sxs-lookup"><span data-stu-id="a24e5-270">This step handles preprocessing/featurization.</span></span> <span data-ttu-id="a24e5-271">使用 ML.NET 中可用的額外元件可讓您的模型產生更佳的結果。</span><span class="sxs-lookup"><span data-stu-id="a24e5-271">Using additional components available in ML.NET can enable better results with your model.</span></span>

## <a name="build-and-train-the-model"></a><span data-ttu-id="a24e5-272">建置和定型模型</span><span class="sxs-lookup"><span data-stu-id="a24e5-272">Build and train the model</span></span>

<span data-ttu-id="a24e5-273">將下列呼叫新增至 `BuildAndTrainModel` 方法作為 `Main` 方法中的下一行程式碼：</span><span class="sxs-lookup"><span data-stu-id="a24e5-273">Add the following call to the `BuildAndTrainModel`method as the next line of code in the `Main` method:</span></span>

[!code-csharp[CallBuildAndTrainModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallBuildAndTrainModel)]

<span data-ttu-id="a24e5-274">`BuildAndTrainModel` 方法會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="a24e5-274">The `BuildAndTrainModel` method executes the following tasks:</span></span>

* <span data-ttu-id="a24e5-275">建立定型演算法類別。</span><span class="sxs-lookup"><span data-stu-id="a24e5-275">Creates the training algorithm class.</span></span>
* <span data-ttu-id="a24e5-276">將模型定型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-276">Trains the model.</span></span>
* <span data-ttu-id="a24e5-277">根據定型資料預測區域。</span><span class="sxs-lookup"><span data-stu-id="a24e5-277">Predicts area based on training data.</span></span>
* <span data-ttu-id="a24e5-278">將儲存模型為 `.zip` 檔案。</span><span class="sxs-lookup"><span data-stu-id="a24e5-278">Saves the model to a `.zip` file.</span></span>
* <span data-ttu-id="a24e5-279">傳回模型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-279">Returns the model.</span></span>

<span data-ttu-id="a24e5-280">請使用下列程式碼，在緊接著 `Main` 方法之後，建立 `BuildAndTrainModel` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-280">Create the `BuildAndTrainModel` method, just after the `Main` method, using the following code:</span></span>

```csharp
public static void BuildAndTrainModel()
{

}
```

<span data-ttu-id="a24e5-281">請注意，兩個參數會傳遞至 BuildAndTrainModel 方法，一個是 `IDataView`，用於定型資料集 (`trainingDataView`)；另一個是 <xref:Microsoft.ML.Data.EstimatorChain%601>，用於在 ProcessData 中建立的處理管線 (`pipeline`)。</span><span class="sxs-lookup"><span data-stu-id="a24e5-281">Notice that two parameters are passed into the BuildAndTrainModel method; an `IDataView` for the training dataset (`trainingDataView`), and a <xref:Microsoft.ML.Data.EstimatorChain%601> for the processing pipeline created in ProcessData (`pipeline`).</span></span>

 <span data-ttu-id="a24e5-282">將下列程式碼新增為 `BuildAndTrainModel` 方法的第一行：</span><span class="sxs-lookup"><span data-stu-id="a24e5-282">Add the following code as the first line of the `BuildAndTrainModel` method:</span></span>

### <a name="choose-a-trainer-algorithm"></a><span data-ttu-id="a24e5-283">選擇的定型程式演算法</span><span class="sxs-lookup"><span data-stu-id="a24e5-283">Choose a trainer algorithm</span></span>

<span data-ttu-id="a24e5-284">若要新增定型程式，請呼叫會傳回 <xref:Microsoft.ML.Trainers.SdcaMultiClassTrainer> 物件的 `mlContext.Transforms.Text.FeaturizeText` 包裝函式方法。</span><span class="sxs-lookup"><span data-stu-id="a24e5-284">To add the trainer algorithm, call the `mlContext.Transforms.Text.FeaturizeText` wrapper method which returns a <xref:Microsoft.ML.Trainers.SdcaMultiClassTrainer> object.</span></span> <span data-ttu-id="a24e5-285">這是您將在此管線中使用的決策樹學習工具。</span><span class="sxs-lookup"><span data-stu-id="a24e5-285">This is a decision tree learner you'll use in this pipeline.</span></span> <span data-ttu-id="a24e5-286">`SdcaMultiClassTrainer` 附加到 `pipeline`，並接受轉換成特徵的 `Title` 和 `Description` (`Features`) 以及 `Label` 輸入參數，以從歷史資料學習。</span><span class="sxs-lookup"><span data-stu-id="a24e5-286">The `SdcaMultiClassTrainer` is appended to the `pipeline` and accepts the featurized `Title` and `Description` (`Features`) and the `Label` input parameters to learn from the historic data.</span></span>

<span data-ttu-id="a24e5-287">將下列程式碼加入 `BuildAndTrainModel` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-287">Add the following code to the `BuildAndTrainModel` method:</span></span>

[!code-csharp[SdcaMultiClassTrainer](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#SdcaMultiClassTrainer)]

<span data-ttu-id="a24e5-288">既然您已建立的定型程式演算法，請將它附加至 `pipeline`。</span><span class="sxs-lookup"><span data-stu-id="a24e5-288">Now that you've created a trainer algorithm, append it to the `pipeline`.</span></span> <span data-ttu-id="a24e5-289">您也需要將標籤對應到要轉變回其原始可讀取狀態的值。</span><span class="sxs-lookup"><span data-stu-id="a24e5-289">You also need to map the label to the value to return to its original readable state.</span></span> <span data-ttu-id="a24e5-290">執行這兩個動作所使用的程式碼如下：</span><span class="sxs-lookup"><span data-stu-id="a24e5-290">Do both of those actions with the following code:</span></span>

[!code-csharp[AddTrainer](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddTrainer)]

### <a name="train-the-model"></a><span data-ttu-id="a24e5-291">將模型定型</span><span class="sxs-lookup"><span data-stu-id="a24e5-291">Train the model</span></span>

<span data-ttu-id="a24e5-292">您會根據已載入和轉換的資料集將模型 <xref:Microsoft.ML.Data.TransformerChain%601>定型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-292">You train the model, <xref:Microsoft.ML.Data.TransformerChain%601>, based on the dataset that has been loaded and transformed.</span></span> <span data-ttu-id="a24e5-293">定義了評估工具之後，我們使用 <xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A> 定型模型，並提供已載入的定型資料。</span><span class="sxs-lookup"><span data-stu-id="a24e5-293">Once the estimator has been defined, you train your model using the <xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A> while providing the already loaded training data.</span></span> <span data-ttu-id="a24e5-294">這會傳回要用於預測的模型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-294">This returns a model to use for predictions.</span></span> <span data-ttu-id="a24e5-295">`trainingPipeline.Fit()` 會定型管線，並根據傳入的 `DataView` 傳回 `Transformer`。</span><span class="sxs-lookup"><span data-stu-id="a24e5-295">`trainingPipeline.Fit()` trains the pipeline and returns a `Transformer` based on the `DataView` passed in.</span></span> <span data-ttu-id="a24e5-296">實驗會等到定型之後才會執行。</span><span class="sxs-lookup"><span data-stu-id="a24e5-296">The experiment is not executed until this happens.</span></span>

<span data-ttu-id="a24e5-297">將下列程式碼加入 `BuildAndTrainModel` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-297">Add the following code to the `BuildAndTrainModel` method:</span></span>

[!code-csharp[TrainModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#TrainModel)]

<span data-ttu-id="a24e5-298">雖然 `model` 是可在多個資料列上運作的 `transformer`，但常見的生產環境案例需要根據個別範例進行預測。</span><span class="sxs-lookup"><span data-stu-id="a24e5-298">While the `model` is a `transformer` that operates on many rows of data, a very common production scenario is a need for predictions on individual examples.</span></span> <span data-ttu-id="a24e5-299"><xref:Microsoft.ML.PredictionEngine%602> 是從 `CreatePredictionEngine` 方法傳回的包裝函式。</span><span class="sxs-lookup"><span data-stu-id="a24e5-299">The <xref:Microsoft.ML.PredictionEngine%602> is a wrapper that is returned from the `CreatePredictionEngine` method.</span></span> <span data-ttu-id="a24e5-300">讓我們在 `BuildAndTrainModel` 方法中的第二行新增下列程式碼，來建立 `PredictionEngine`：</span><span class="sxs-lookup"><span data-stu-id="a24e5-300">Let's add the following code to create the `PredictionEngine` as the next line in the `BuildAndTrainModel` Method:</span></span>

[!code-csharp[CreatePredictionEngine](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine)]

<span data-ttu-id="a24e5-301">透過建立 `GitHubIssue` 的執行個體，在 `Predict` 方法中新增 GitHub 問題，以測試所定型模型的預測：</span><span class="sxs-lookup"><span data-stu-id="a24e5-301">Add a GitHub issue to test the trained model's prediction in the `Predict` method by creating an instance of `GitHubIssue`:</span></span>

[!code-csharp[CreateTestIssue1](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreateTestIssue1)]

<span data-ttu-id="a24e5-302">您可以用其來預測該問題資料單一執行個體的 `Area` 標籤。</span><span class="sxs-lookup"><span data-stu-id="a24e5-302">You can use that to predict the `Area` label of a single instance of the issue data.</span></span> <span data-ttu-id="a24e5-303">若要取得預測，請在資料上使用 <xref:Microsoft.ML.PredictionEngine%602.Predict%2A>。</span><span class="sxs-lookup"><span data-stu-id="a24e5-303">To get a prediction, use <xref:Microsoft.ML.PredictionEngine%602.Predict%2A> on the data.</span></span> <span data-ttu-id="a24e5-304">請注意，輸入資料是一個字串，且模型包含特徵化。</span><span class="sxs-lookup"><span data-stu-id="a24e5-304">Note that the input data is a string and the model includes the featurization.</span></span> <span data-ttu-id="a24e5-305">在定型和預測期間，您的管線會同步。</span><span class="sxs-lookup"><span data-stu-id="a24e5-305">Your pipeline is in sync during training and prediction.</span></span> <span data-ttu-id="a24e5-306">您無須特別為預測撰寫前處理/特徵化程式碼，同一個 API 會同時負責批次和單次預測。</span><span class="sxs-lookup"><span data-stu-id="a24e5-306">You didn’t have to write preprocessing/featurization code specifically for predictions, and the same API takes care of both batch and one-time predictions.</span></span>

[!code-csharp[Predict](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Predict)]

### <a name="model-operationalization-prediction"></a><span data-ttu-id="a24e5-307">模型操作化：預測</span><span class="sxs-lookup"><span data-stu-id="a24e5-307">Model operationalization: prediction</span></span>

<span data-ttu-id="a24e5-308">顯示 `GitHubIssue` 和相對應的 `Area` 標籤預測，以共用結果並根據結果相應地採取動作。</span><span class="sxs-lookup"><span data-stu-id="a24e5-308">Display `GitHubIssue` and corresponding `Area` label prediction in order to share the results and act on them accordingly.</span></span> <span data-ttu-id="a24e5-309">這稱為操作化，其中會使用傳回的資料作為操作原則的一部份。</span><span class="sxs-lookup"><span data-stu-id="a24e5-309">This is called operationalization, using the returned data as part of the operational policies.</span></span> <span data-ttu-id="a24e5-310">請使用下列 <xref:System.Console.WriteLine?displayProperty=nameWithType> 程式碼來為結果建立顯示：</span><span class="sxs-lookup"><span data-stu-id="a24e5-310">Create a display for the results using the following <xref:System.Console.WriteLine?displayProperty=nameWithType> code:</span></span>

[!code-csharp[OutputPrediction](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#OutputPrediction)]

### <a name="save-and-return-the-model-trained-to-use-for-evaluation"></a><span data-ttu-id="a24e5-311">儲存並傳回所定型以供評估使用的模型</span><span class="sxs-lookup"><span data-stu-id="a24e5-311">Save and return the model trained to use for evaluation</span></span>

<span data-ttu-id="a24e5-312">此時，您已有一個可整合至任何現有或新 .NET 應用程式之 <xref:Microsoft.ML.Data.TransformerChain%601> 類型的模型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-312">At this point, you have a model of type <xref:Microsoft.ML.Data.TransformerChain%601> that can be integrated into any of your existing or new .NET applications.</span></span> <span data-ttu-id="a24e5-313">若要將您所定型的模型儲存成 .zip 檔案，請在 `BuildAndTrainModel` 中的下一行新增下列程式碼，以呼叫 `SaveModelAsFile` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-313">To save your trained model to a .zip file, add the following code to call the `SaveModelAsFile` method as the next line in `BuildAndTrainModel`:</span></span>

[!code-csharp[CallSaveModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallSaveModel)]

<span data-ttu-id="a24e5-314">在 `BuildAndTrainModel` 方法的結尾傳回模型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-314">Return the model at the end of the `BuildAndTrainModel` method.</span></span>

[!code-csharp[ReturnModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#ReturnModel)]

## <a name="save-the-model-as-azip-file"></a><span data-ttu-id="a24e5-315">將模型儲存為 .zip 檔案</span><span class="sxs-lookup"><span data-stu-id="a24e5-315">Save the model as a.zip file</span></span>

<span data-ttu-id="a24e5-316">請使用下列程式碼，在緊接著 `BuildAndTrainModel` 方法之後，建立 `SaveModelAsFile` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-316">Create the `SaveModelAsFile` method, just after the `BuildAndTrainModel` method, using the following code:</span></span>

```csharp
private static void SaveModelAsFile(MLContext mlContext, ITransformer model)
{

}
```

<span data-ttu-id="a24e5-317">`SaveModelAsFile` 方法會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="a24e5-317">The `SaveModelAsFile` method executes the following tasks:</span></span>

* <span data-ttu-id="a24e5-318">將模型儲存為 .zip 檔案。</span><span class="sxs-lookup"><span data-stu-id="a24e5-318">Saves the model as a .zip file.</span></span>

<span data-ttu-id="a24e5-319">接下來，請建立儲存模型的方法，以便在其他應用程式中重複使用。</span><span class="sxs-lookup"><span data-stu-id="a24e5-319">Next, create a method to save the model so that it can be reused and consumed in other applications.</span></span> <span data-ttu-id="a24e5-320">`ITransformer` 具有 <xref:Microsoft.ML.Data.TransformerChain%601.SaveTo(Microsoft.ML.IHostEnvironment,System.IO.Stream)> 方法，它會採用 `_modelPath` 全域欄位和 <xref:System.IO.Stream>。</span><span class="sxs-lookup"><span data-stu-id="a24e5-320">The `ITransformer` has a <xref:Microsoft.ML.Data.TransformerChain%601.SaveTo(Microsoft.ML.IHostEnvironment,System.IO.Stream)> method that takes in the `_modelPath` global field, and a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="a24e5-321">為了將模型儲存為 zip 檔案，您會在呼叫 `SaveTo` 方法之前立即建立 `FileStream`。</span><span class="sxs-lookup"><span data-stu-id="a24e5-321">To save this as a zip file, you'll create the `FileStream` immediately before calling the `SaveTo` method.</span></span> <span data-ttu-id="a24e5-322">將下列程式碼加入 `SaveModelAsFile` 方法中作為的下一行：</span><span class="sxs-lookup"><span data-stu-id="a24e5-322">Add the following code to the `SaveModelAsFile` method as the next line:</span></span>

[!code-csharp[SaveModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#SaveModel)]

<span data-ttu-id="a24e5-323">您也可以透過使用下列程式碼，以 `_modelPath` 寫入主控台訊息來顯示檔案寫入的位置：</span><span class="sxs-lookup"><span data-stu-id="a24e5-323">You could also display where the file was written by writing a console message with the `_modelPath`, using the following code:</span></span>

```csharp
Console.WriteLine("The model is saved to {0}", _modelPath);
```

## <a name="evaluate-the-model"></a><span data-ttu-id="a24e5-324">評估模型</span><span class="sxs-lookup"><span data-stu-id="a24e5-324">Evaluate the model</span></span>

<span data-ttu-id="a24e5-325">建立並定型模型之後，現在必須使用不同的資料集來評估它，以確保和驗證品質。</span><span class="sxs-lookup"><span data-stu-id="a24e5-325">Now that you've created and trained the model, you need to evaluate it with a different dataset for quality assurance and validation.</span></span> <span data-ttu-id="a24e5-326">在 `Evaluate` 方法中，會傳入在 `BuildAndTrainModel` 中建立的模型以供評估。</span><span class="sxs-lookup"><span data-stu-id="a24e5-326">In the `Evaluate` method, the model created in `BuildAndTrainModel` is passed in to be evaluated.</span></span> <span data-ttu-id="a24e5-327">在緊接著 `BuildAndTrainModel` 之後，建立 `Evaluate` 方法，如以下程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="a24e5-327">Create the `Evaluate` method, just after `BuildAndTrainModel`, as in the following code:</span></span>

```csharp
public static void Evaluate()
{

}
```

<span data-ttu-id="a24e5-328">`Evaluate` 方法會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="a24e5-328">The `Evaluate` method executes the following tasks:</span></span>

* <span data-ttu-id="a24e5-329">載入測試資料集。</span><span class="sxs-lookup"><span data-stu-id="a24e5-329">Loads the test dataset.</span></span>
* <span data-ttu-id="a24e5-330">建立多類別評估工具。</span><span class="sxs-lookup"><span data-stu-id="a24e5-330">Creates the multiclass evaluator.</span></span>
* <span data-ttu-id="a24e5-331">評估模型並建立計量。</span><span class="sxs-lookup"><span data-stu-id="a24e5-331">Evaluates the model and create metrics.</span></span>
* <span data-ttu-id="a24e5-332">顯示計量。</span><span class="sxs-lookup"><span data-stu-id="a24e5-332">Displays the metrics.</span></span>

<span data-ttu-id="a24e5-333">請使用下列程式碼，在緊接著 `BuildAndTrainModel` 方法呼叫底下，從 `Main` 方法新增對新方法的呼叫：</span><span class="sxs-lookup"><span data-stu-id="a24e5-333">Add a call to the new method from the `Main` method, right under the `BuildAndTrainModel` method call, using the following code:</span></span>

[!code-csharp[CallEvaluate](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallEvaluate)]

<span data-ttu-id="a24e5-334">如同先前為定型資料集進行的操作，您也可以將初始化、對應與測試資料集載入合併成一行程式碼。</span><span class="sxs-lookup"><span data-stu-id="a24e5-334">As you did previously with the training dataset, you can combine the initialization, mapping, and test dataset loading into one line of code.</span></span> <span data-ttu-id="a24e5-335">您可以使用此資料集來評估模型，以作為品質檢查。</span><span class="sxs-lookup"><span data-stu-id="a24e5-335">You can evaluate the model using this dataset as a quality check.</span></span> <span data-ttu-id="a24e5-336">將下列程式碼加入 `Evaluate` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-336">Add the following code to the `Evaluate` method:</span></span>

[!code-csharp[LoadTestDataset](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadTestDataset)]

<span data-ttu-id="a24e5-337">`MulticlassClassificationContext.Evaluate` 是 <xref:Microsoft.ML.MulticlassClassificationContext.Evaluate%2A> 方法的包裝函式，會使用指定的資料集來計算模型的品質計量。</span><span class="sxs-lookup"><span data-stu-id="a24e5-337">The `MulticlassClassificationContext.Evaluate` is a wrapper for the <xref:Microsoft.ML.MulticlassClassificationContext.Evaluate%2A> method that computes the quality metrics for the model using the specified dataset.</span></span> <span data-ttu-id="a24e5-338">它傳回的 <xref:Microsoft.ML.Data.MultiClassClassifierMetrics> 物件包含多類別分類評估工具所計算的整體計量。</span><span class="sxs-lookup"><span data-stu-id="a24e5-338">It returns a <xref:Microsoft.ML.Data.MultiClassClassifierMetrics> object that contains the overall metrics computed by multiclass classification evaluators.</span></span>
<span data-ttu-id="a24e5-339">若要顯示這些計量以判斷模型的品質，您必須先取得計量。</span><span class="sxs-lookup"><span data-stu-id="a24e5-339">To display these to determine the quality of the model, you need to get the metrics first.</span></span>
<span data-ttu-id="a24e5-340">請注意，輸入特徵並傳回預測使用的是機器學習 `_trainedModel` 全域變數的 `Transform` 方法 (轉換器)。</span><span class="sxs-lookup"><span data-stu-id="a24e5-340">Notice the use of the `Transform` method of the machine learning `_trainedModel` global variable (a transformer) to input the features and return predictions.</span></span> <span data-ttu-id="a24e5-341">將下列程式碼加入 `Evaluate` 方法中作為的下一行：</span><span class="sxs-lookup"><span data-stu-id="a24e5-341">Add the following code to the `Evaluate` method as the next line:</span></span>

[!code-csharp[Evaluate](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Evaluate)]

<span data-ttu-id="a24e5-342">針對多類別分類評估的計量如下：</span><span class="sxs-lookup"><span data-stu-id="a24e5-342">The following metrics are evaluated for multiclass classification:</span></span>

* <span data-ttu-id="a24e5-343">微精確度 - 每個範例類別組同樣都會對精確度計量提出貢獻。</span><span class="sxs-lookup"><span data-stu-id="a24e5-343">Micro Accuracy - Every sample-class pair contributes equally to the accuracy metric.</span></span>  <span data-ttu-id="a24e5-344">建議讓微精確度盡量接近 1。</span><span class="sxs-lookup"><span data-stu-id="a24e5-344">You want Micro Accuracy to be as close to 1 as possible.</span></span>

* <span data-ttu-id="a24e5-345">大精確度 - 每個類別同樣都會對精確度計量提出貢獻。</span><span class="sxs-lookup"><span data-stu-id="a24e5-345">Macro Accuracy - Every class contributes equally to the accuracy metric.</span></span> <span data-ttu-id="a24e5-346">少數類別會加上與較大類別相同的權重。</span><span class="sxs-lookup"><span data-stu-id="a24e5-346">Minority classes are given equal weight as the larger classes.</span></span> <span data-ttu-id="a24e5-347">建議讓大精確度盡量接近 1。</span><span class="sxs-lookup"><span data-stu-id="a24e5-347">You want Macro Accuracy to be as close to 1 as possible.</span></span>

* <span data-ttu-id="a24e5-348">記錄檔遺失 - 請參閱[記錄檔遺失](../resources/glossary.md#log-loss)。</span><span class="sxs-lookup"><span data-stu-id="a24e5-348">Log-loss - see [Log Loss](../resources/glossary.md#log-loss).</span></span> <span data-ttu-id="a24e5-349">建議讓記錄檔遺失盡量接近零。</span><span class="sxs-lookup"><span data-stu-id="a24e5-349">You want Log-loss to be as close to zero as possible.</span></span>

* <span data-ttu-id="a24e5-350">記錄檔遺失減少 - 範圍介於 [-inf, 100]，其中 100 表示完美的預測，而 0 表示平均預測。</span><span class="sxs-lookup"><span data-stu-id="a24e5-350">Log-loss reduction - Ranges from [-inf, 100], where 100 is perfect predictions and 0 indicates mean predictions.</span></span> <span data-ttu-id="a24e5-351">建議讓記錄檔遺失減少盡量接近零。</span><span class="sxs-lookup"><span data-stu-id="a24e5-351">You want Log-loss reduction to be as close to zero as possible.</span></span>

### <a name="displaying-the-metrics-for-model-validation"></a><span data-ttu-id="a24e5-352">顯示模型驗證的計量</span><span class="sxs-lookup"><span data-stu-id="a24e5-352">Displaying the metrics for model validation</span></span>

<span data-ttu-id="a24e5-353">使用下列程式碼來顯示計量、共用結果，然後依結果採取動作：</span><span class="sxs-lookup"><span data-stu-id="a24e5-353">Use the following code to display the metrics, share the results, and then act on them:</span></span>

[!code-csharp[DisplayMetrics](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DisplayMetrics)]

## <a name="predict-the-test-data-outcome-with-the-saved-model"></a><span data-ttu-id="a24e5-354">使用儲存的模型來預測測試資料結果</span><span class="sxs-lookup"><span data-stu-id="a24e5-354">Predict the test data outcome with the saved model</span></span>

<span data-ttu-id="a24e5-355">請使用下列程式碼，在緊接著 `Evaluate` 方法呼叫底下，從 `Main` 方法新增對新方法的呼叫：</span><span class="sxs-lookup"><span data-stu-id="a24e5-355">Add a call to the new method from the `Main` method, right under the `Evaluate` method call, using the following code:</span></span>

[!code-csharp[CallPredictIssue](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallPredictIssue)]

<span data-ttu-id="a24e5-356">請使用下列程式碼，在緊接著 `Evaluate` 方法之後，建立 `PredictIssue` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-356">Create the `PredictIssue` method, just after the `Evaluate` method, using the following code:</span></span>

```csharp
private static void PredictIssue()
{

}
```

<span data-ttu-id="a24e5-357">`PredictIssue` 方法會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="a24e5-357">The `PredictIssue` method executes the following tasks:</span></span>

* <span data-ttu-id="a24e5-358">建立測試資料的單一問題。</span><span class="sxs-lookup"><span data-stu-id="a24e5-358">Creates a single issue of test data.</span></span>
* <span data-ttu-id="a24e5-359">根據測試資料預測區域。</span><span class="sxs-lookup"><span data-stu-id="a24e5-359">Predicts Area based on test data.</span></span>
* <span data-ttu-id="a24e5-360">合併測試資料和預測來進行報告。</span><span class="sxs-lookup"><span data-stu-id="a24e5-360">Combines test data and predictions for reporting.</span></span>
* <span data-ttu-id="a24e5-361">顯示預測的結果。</span><span class="sxs-lookup"><span data-stu-id="a24e5-361">Displays the predicted results.</span></span>

<span data-ttu-id="a24e5-362">首先，使用下列程式碼載入您先前儲存的模型：</span><span class="sxs-lookup"><span data-stu-id="a24e5-362">First, load the model that you saved previously with the following code:</span></span>

[!code-csharp[LoadModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadModel)]

<span data-ttu-id="a24e5-363">透過建立 `GitHubIssue` 的執行個體，在 `Predict` 方法中新增 GitHub 問題，以測試所定型模型的預測：</span><span class="sxs-lookup"><span data-stu-id="a24e5-363">Add a GitHub issue to test the trained model's prediction in the `Predict` method by creating an instance of `GitHubIssue`:</span></span>

[!code-csharp[AddTestIssue](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddTestIssue)]

[!code-csharp[CreatePredictionEngine](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine)]
  
<span data-ttu-id="a24e5-364">既然您有了模型，您就可以用它來預測 GitHub 問題資料單一執行個體的 Area GitHub 標籤。</span><span class="sxs-lookup"><span data-stu-id="a24e5-364">Now that you have a model, you can use that to predict the Area GitHub label of a single instance of the GitHub issue data.</span></span> <span data-ttu-id="a24e5-365">若要取得預測，請在資料上使用 <xref:Microsoft.ML.PredictionEngine%602.Predict%2A>。</span><span class="sxs-lookup"><span data-stu-id="a24e5-365">To get a prediction, use <xref:Microsoft.ML.PredictionEngine%602.Predict%2A> on the data.</span></span> <span data-ttu-id="a24e5-366">請注意，輸入資料是一個字串，且模型包含特徵化。</span><span class="sxs-lookup"><span data-stu-id="a24e5-366">Note that the input data is a string and the model includes the featurization.</span></span> <span data-ttu-id="a24e5-367">在定型和預測期間，您的管線會同步。</span><span class="sxs-lookup"><span data-stu-id="a24e5-367">Your pipeline is in sync during training and prediction.</span></span> <span data-ttu-id="a24e5-368">您無須特別為預測撰寫前處理/特徵化程式碼，同一個 API 會同時負責批次和單次預測。</span><span class="sxs-lookup"><span data-stu-id="a24e5-368">You didn’t have to write preprocessing/featurization code specifically for predictions, and the same API takes care of both batch and one-time predictions.</span></span> <span data-ttu-id="a24e5-369">將下列程式碼加入預測的 `PredictIssue` 方法：</span><span class="sxs-lookup"><span data-stu-id="a24e5-369">Add the following code to the `PredictIssue` method for the predictions:</span></span>

[!code-csharp[PredictIssue](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine)]

### <a name="model-operationalization-prediction"></a><span data-ttu-id="a24e5-370">模型操作化：預測</span><span class="sxs-lookup"><span data-stu-id="a24e5-370">Model operationalization: prediction</span></span>

<span data-ttu-id="a24e5-371">顯示 `Area` 以分類問題，並根據該分類採取動作。</span><span class="sxs-lookup"><span data-stu-id="a24e5-371">Display `Area` in order to categorize the issue and act on it accordingly.</span></span> <span data-ttu-id="a24e5-372">這稱為操作化，其中會使用傳回的資料作為操作原則的一部份。</span><span class="sxs-lookup"><span data-stu-id="a24e5-372">This is called operationalization, using the returned data as part of the operational policies.</span></span> <span data-ttu-id="a24e5-373">請使用下列 <xref:System.Console.WriteLine?displayProperty=nameWithType> 程式碼來為結果建立顯示：</span><span class="sxs-lookup"><span data-stu-id="a24e5-373">Create a display for the results using the following <xref:System.Console.WriteLine?displayProperty=nameWithType> code:</span></span>

[!code-csharp[DisplayResults](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DisplayResults)]

## <a name="results"></a><span data-ttu-id="a24e5-374">結果</span><span class="sxs-lookup"><span data-stu-id="a24e5-374">Results</span></span>

<span data-ttu-id="a24e5-375">您的結果應該與以下類似。</span><span class="sxs-lookup"><span data-stu-id="a24e5-375">Your results should be similar to the following.</span></span> <span data-ttu-id="a24e5-376">當管線進行處理時，會顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="a24e5-376">As the pipeline processes, it displays messages.</span></span> <span data-ttu-id="a24e5-377">您可能會看到警告或處理訊息。</span><span class="sxs-lookup"><span data-stu-id="a24e5-377">You may see warnings, or processing messages.</span></span> <span data-ttu-id="a24e5-378">為了清晰起見，下列結果中已將這些都移除。</span><span class="sxs-lookup"><span data-stu-id="a24e5-378">These have been removed from the following results for clarity.</span></span>

```console
=============== Single Prediction just-trained-model - Result: area-System.Net ===============
The model is saved to C:\Users\johalex\dotnet-samples\samples\machine-learning\tutorials\GitHubIssueClassification\bin\Debug\netcoreapp2.0\..\..\..\Models\model.zip
*************************************************************************************************************
*       Metrics for Multi-class Classification model - Test Data
*------------------------------------------------------------------------------------------------------------
*       MicroAccuracy:    0.74
*       MacroAccuracy:    0.687
*       LogLoss:          .932
*       LogLossReduction: 63.852
*************************************************************************************************************
=============== Single Prediction - Result: area-System.Data ===============
```

<span data-ttu-id="a24e5-379">恭喜您！</span><span class="sxs-lookup"><span data-stu-id="a24e5-379">Congratulations!</span></span> <span data-ttu-id="a24e5-380">您現在已成功建置可對 GitHub 問題分類和預測 Area 標籤的機器學習模型。</span><span class="sxs-lookup"><span data-stu-id="a24e5-380">You've now successfully built a machine learning model for classifying and predicting an Area label for a GitHub issue.</span></span> <span data-ttu-id="a24e5-381">您可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification) 存放庫中找到本教學課程的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="a24e5-381">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification) repository.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a24e5-382">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a24e5-382">Next steps</span></span>

<span data-ttu-id="a24e5-383">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="a24e5-383">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="a24e5-384">了解問題</span><span class="sxs-lookup"><span data-stu-id="a24e5-384">Understand the problem</span></span>
> * <span data-ttu-id="a24e5-385">選取適當的機器學習工作</span><span class="sxs-lookup"><span data-stu-id="a24e5-385">Select the appropriate machine learning task</span></span>
> * <span data-ttu-id="a24e5-386">準備您的資料</span><span class="sxs-lookup"><span data-stu-id="a24e5-386">Prepare your data</span></span>
> * <span data-ttu-id="a24e5-387">建立學習管線</span><span class="sxs-lookup"><span data-stu-id="a24e5-387">Create the learning pipeline</span></span>
> * <span data-ttu-id="a24e5-388">載入分類器</span><span class="sxs-lookup"><span data-stu-id="a24e5-388">Load a classifier</span></span>
> * <span data-ttu-id="a24e5-389">將模型定型</span><span class="sxs-lookup"><span data-stu-id="a24e5-389">Train the model</span></span>
> * <span data-ttu-id="a24e5-390">使用不同的資料集來評估模型</span><span class="sxs-lookup"><span data-stu-id="a24e5-390">Evaluate the model with a different dataset</span></span>
> * <span data-ttu-id="a24e5-391">使用模型來預測測試資料結果</span><span class="sxs-lookup"><span data-stu-id="a24e5-391">Predict the test data outcomes with the model</span></span>

<span data-ttu-id="a24e5-392">前進到下一個教學課程來深入了解</span><span class="sxs-lookup"><span data-stu-id="a24e5-392">Advance to the next tutorial to learn more</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a24e5-393">計程車車資預測工具</span><span class="sxs-lookup"><span data-stu-id="a24e5-393">Taxi Fare Predictor</span></span>](taxi-fare.md)