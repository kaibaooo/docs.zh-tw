---
title: 使用索引和範圍探索資料範圍
description: 此進階教學課程將教導您使用索引和範圍探索資料，以檢查連續資料集的配量。
ms.date: 04/19/2019
ms.custom: mvc
ms.openlocfilehash: 118d3c9ccad98ec02195c2b5e26a2ca203990adf
ms.sourcegitcommit: 682c64df0322c7bda016f8bfea8954e9b31f1990
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557191"
---
# <a name="indices-and-ranges"></a><span data-ttu-id="e1811-103">索引和範圍</span><span class="sxs-lookup"><span data-stu-id="e1811-103">Indices and ranges</span></span>

<span data-ttu-id="e1811-104">範圍和索引提供簡潔的語法，以存取 <xref:System.Array>、<xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601> 的單一項目或範圍。</span><span class="sxs-lookup"><span data-stu-id="e1811-104">Ranges and indices provide a succinct syntax for accessing single elements or ranges in an <xref:System.Array>, <xref:System.Span%601>, or <xref:System.ReadOnlySpan%601>.</span></span> <span data-ttu-id="e1811-105">這些功能提供更簡潔且清楚的語法來存取序列中單一項目或項目範圍。</span><span class="sxs-lookup"><span data-stu-id="e1811-105">These features enable more concise, clear syntax to access single elements or ranges of elements in a sequence.</span></span>

<span data-ttu-id="e1811-106">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="e1811-106">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e1811-107">使用序列中範圍的語法。</span><span class="sxs-lookup"><span data-stu-id="e1811-107">Use the syntax for ranges in a sequence.</span></span>
> * <span data-ttu-id="e1811-108">了解每個序列開始和結束的設計決策。</span><span class="sxs-lookup"><span data-stu-id="e1811-108">Understand the design decisions for the start and end of each sequence.</span></span>
> * <span data-ttu-id="e1811-109">了解 <xref:System.Index> 和 <xref:System.Range> 類型的案例。</span><span class="sxs-lookup"><span data-stu-id="e1811-109">Learn scenarios for the <xref:System.Index> and <xref:System.Range> types.</span></span>

## <a name="language-support-for-indices-and-ranges"></a><span data-ttu-id="e1811-110">索引和範圍的語言支援</span><span class="sxs-lookup"><span data-stu-id="e1811-110">Language support for indices and ranges</span></span>

<span data-ttu-id="e1811-111">此語言支援仰賴兩個新的型別與兩個新的運算子。</span><span class="sxs-lookup"><span data-stu-id="e1811-111">This language support relies on two new types and two new operators.</span></span>
- <span data-ttu-id="e1811-112"><xref:System.Index?displayProperty=nameWithType> 代表序列的索引。</span><span class="sxs-lookup"><span data-stu-id="e1811-112"><xref:System.Index?displayProperty=nameWithType> represents an index into a sequence.</span></span>
- <span data-ttu-id="e1811-113">`^` 運算子，它會指定索引相對於序列結尾。</span><span class="sxs-lookup"><span data-stu-id="e1811-113">The `^` operator, which specifies that an index is relative to the end of a sequence.</span></span>
- <span data-ttu-id="e1811-114"><xref:System.Range?displayProperty=nameWithType> 代表序列的子範圍。</span><span class="sxs-lookup"><span data-stu-id="e1811-114"><xref:System.Range?displayProperty=nameWithType> represents a sub range of a sequence.</span></span>
- <span data-ttu-id="e1811-115">Range 運算子 (`..`)，它會指定範圍的開頭與結尾作為其運算子。</span><span class="sxs-lookup"><span data-stu-id="e1811-115">The Range operator (`..`), which specifies the start and end of a range as its operands.</span></span>

<span data-ttu-id="e1811-116">讓我們從索引的規則開始。</span><span class="sxs-lookup"><span data-stu-id="e1811-116">Let's start with the rules for indices.</span></span> <span data-ttu-id="e1811-117">假設有一個陣列 `sequence`。</span><span class="sxs-lookup"><span data-stu-id="e1811-117">Consider an array `sequence`.</span></span> <span data-ttu-id="e1811-118">`0` 索引與 `sequence[0]` 相同。</span><span class="sxs-lookup"><span data-stu-id="e1811-118">The `0` index is the same as `sequence[0]`.</span></span> <span data-ttu-id="e1811-119">`^0` 索引與 `sequence[sequence.Length]` 相同。</span><span class="sxs-lookup"><span data-stu-id="e1811-119">The `^0` index is the same as `sequence[sequence.Length]`.</span></span> <span data-ttu-id="e1811-120">請注意，`sequence[^0]` 會擲回例外狀況，就樣 `sequence[sequence.Length]` 會這樣做一樣。</span><span class="sxs-lookup"><span data-stu-id="e1811-120">Note that `sequence[^0]` does throw an exception, just as `sequence[sequence.Length]` does.</span></span> <span data-ttu-id="e1811-121">針對任何數字 `n`，索引 `^n` 與 `sequence[sequence.Length - n]` 相同。</span><span class="sxs-lookup"><span data-stu-id="e1811-121">For any number `n`, the index `^n` is the same as `sequence[sequence.Length - n]`.</span></span>

```csharp-interactive
string[] words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

<span data-ttu-id="e1811-122">您可以使用 `^1` 索引擷取最後一個字。</span><span class="sxs-lookup"><span data-stu-id="e1811-122">You can retrieve the last word with the `^1` index.</span></span> <span data-ttu-id="e1811-123">將下列程式碼新增於初始設定下方：</span><span class="sxs-lookup"><span data-stu-id="e1811-123">Add the following code below the initialization:</span></span>

[!code-csharp[LastIndex](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastIndex)]

<span data-ttu-id="e1811-124">指定範圍「開頭」與「結尾」的範圍。</span><span class="sxs-lookup"><span data-stu-id="e1811-124">A range specifies the *start* and *end* of a range.</span></span> <span data-ttu-id="e1811-125">範圍具有排除性，這表示「結尾」不包含在範圍內。</span><span class="sxs-lookup"><span data-stu-id="e1811-125">Ranges are exclusive, meaning the *end* is not included in the range.</span></span> <span data-ttu-id="e1811-126">範圍 `[0..^0]` 代整個範圍，就像 `[0..sequence.Length]` 代表整個範圍一樣。</span><span class="sxs-lookup"><span data-stu-id="e1811-126">The range `[0..^0]` represents the entire range, just as `[0..sequence.Length]` represents the entire range.</span></span> 

<span data-ttu-id="e1811-127">下列程式碼會建立具有 "quick"、"brown" 和 "fox" 字組的子範圍。</span><span class="sxs-lookup"><span data-stu-id="e1811-127">The following code creates a subrange with the words "quick", "brown", and "fox".</span></span> <span data-ttu-id="e1811-128">其會包含 `words[1]` 到 `words[3]`。</span><span class="sxs-lookup"><span data-stu-id="e1811-128">It includes `words[1]` through `words[3]`.</span></span> <span data-ttu-id="e1811-129">項目 `words[4]` 不在範圍內。</span><span class="sxs-lookup"><span data-stu-id="e1811-129">The element `words[4]` isn't in the range.</span></span> <span data-ttu-id="e1811-130">將下列程式碼新增至相同的方法。</span><span class="sxs-lookup"><span data-stu-id="e1811-130">Add the following code to the same method.</span></span> <span data-ttu-id="e1811-131">複製並貼在互動式視窗的底部。</span><span class="sxs-lookup"><span data-stu-id="e1811-131">Copy and paste it at the bottom of the interactive window.</span></span>

[!code-csharp[Range](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Range)]

<span data-ttu-id="e1811-132">下列程式碼會建立具有 "lazy" 和 "dog" 的子範圍。</span><span class="sxs-lookup"><span data-stu-id="e1811-132">The following code creates a subrange with "lazy" and "dog".</span></span> <span data-ttu-id="e1811-133">其包含 `words[^2]` 及 `words[^1]`。</span><span class="sxs-lookup"><span data-stu-id="e1811-133">It includes `words[^2]` and `words[^1]`.</span></span> <span data-ttu-id="e1811-134">未包含結尾索引 `words[^0]`。</span><span class="sxs-lookup"><span data-stu-id="e1811-134">The end index `words[^0]` isn't included.</span></span> <span data-ttu-id="e1811-135">同時新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e1811-135">Add the following code as well:</span></span>

[!code-csharp[LastRange](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastRange)]

<span data-ttu-id="e1811-136">下列範例會建立不限定開始、結束或兩者的範圍：</span><span class="sxs-lookup"><span data-stu-id="e1811-136">The following examples create ranges that are open ended for the start, end, or both:</span></span>

[!code-csharp[PartialRange](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_PartialRanges)]

<span data-ttu-id="e1811-137">您也可以將範圍或索引宣告為變數。</span><span class="sxs-lookup"><span data-stu-id="e1811-137">You can also declare ranges or indices as variables.</span></span> <span data-ttu-id="e1811-138">然後即可在 `[` 和 `]` 字元內使用變數：</span><span class="sxs-lookup"><span data-stu-id="e1811-138">The variable can then be used inside the `[` and `]` characters:</span></span>

[!code-csharp[IndexRangeTypes](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_RangeIndexTypes)]

<span data-ttu-id="e1811-139">下列範例顯示這些選擇的許多原因。</span><span class="sxs-lookup"><span data-stu-id="e1811-139">The following sample shows many of the reasons for those choices.</span></span> <span data-ttu-id="e1811-140">修改 `x`、`y` 和 `z` 以嘗試不同的組合。</span><span class="sxs-lookup"><span data-stu-id="e1811-140">Modify `x`, `y`, and `z` to try different combinations.</span></span> <span data-ttu-id="e1811-141">在您實驗時，請使用 `x` 小於 `y`，且 `y` 小於 `z` 的有效組合值。</span><span class="sxs-lookup"><span data-stu-id="e1811-141">When you experiment, use values where `x` is less than `y`, and `y` is less than `z` for valid combinations.</span></span> <span data-ttu-id="e1811-142">在新方法中新增下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="e1811-142">Add the following code in a new method.</span></span> <span data-ttu-id="e1811-143">嘗試不同的組合：</span><span class="sxs-lookup"><span data-stu-id="e1811-143">Try different combinations:</span></span>

[!code-csharp[SemanticsExamples](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Semantics)]

## <a name="scenarios-for-indices-and-ranges"></a><span data-ttu-id="e1811-144">索引和範圍的案例</span><span class="sxs-lookup"><span data-stu-id="e1811-144">Scenarios for Indices and Ranges</span></span>

<span data-ttu-id="e1811-145">當您希望對整個序列的子範圍執行某些分析時，您會經常使用範圍和索引。</span><span class="sxs-lookup"><span data-stu-id="e1811-145">You'll often use ranges and indices when you want to perform some analysis on subrange of an entire sequence.</span></span> <span data-ttu-id="e1811-146">在準確讀取牽涉的子範圍時，新語法較清晰。</span><span class="sxs-lookup"><span data-stu-id="e1811-146">The new syntax is clearer in reading exactly what subrange is involved.</span></span> <span data-ttu-id="e1811-147">區域函式 `MovingAverage` 採用 <xref:System.Range> 作為其引數。</span><span class="sxs-lookup"><span data-stu-id="e1811-147">The local function `MovingAverage` takes a <xref:System.Range> as its argument.</span></span> <span data-ttu-id="e1811-148">然後，方法在計算最小值、最大值和平均值時，僅會列舉該範圍。</span><span class="sxs-lookup"><span data-stu-id="e1811-148">The method then enumerates just that range when calculating the min, max, and average.</span></span> <span data-ttu-id="e1811-149">請在您的專案中嘗試下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e1811-149">Try the following code in your project:</span></span>

[!code-csharp[MovingAverages](~/samples/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_MovingAverage)]

<span data-ttu-id="e1811-150">您可以從 [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/RangesIndexes) 存放庫下載完整的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e1811-150">You can download the completed code from the [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/RangesIndexes) repository.</span></span>