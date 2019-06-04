---
title: 比較運算子 - C# 參考
description: 深入了解可供您檢查數值順序的 C# 比較運算子。
ms.date: 04/25/2019
author: pkulikov
f1_keywords:
- <_CSharpKeyword
- '>_CSharpKeyword'
- <=_CSharpKeyword
- '>=_CSharpKeyword'
helpviewer_keywords:
- comparison operators [C#]
- relational operators [C#]
- less than operator [C#]
- < operator [C#]
- greater than operator [C#]
- '> operator [C#]'
- less than or equal to operator [C#]
- <= operator [C#]
- greater than or equal to operator [C#]
- '>= operator [C#]'
ms.openlocfilehash: 6d3751ff1ee2c6ee2f058eeda4ffd5db188a988e
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65633766"
---
# <a name="comparison-operators-c-reference"></a><span data-ttu-id="9183a-103">比較運算子 (C#參考)</span><span class="sxs-lookup"><span data-stu-id="9183a-103">Comparison operators (C# Reference)</span></span>

<span data-ttu-id="9183a-104">[`<` (小於)](#less-than-operator-)、[`>` (大於)](#greater-than-operator-)、[`<=` (小於或等於)](#less-than-or-equal-operator-)，以及 [`>=` (大於或等於)](#greater-than-or-equal-operator-) 比較 (也稱為關聯式) 運算子，會比較其運算元。</span><span class="sxs-lookup"><span data-stu-id="9183a-104">The [`<` (less than)](#less-than-operator-), [`>` (greater than)](#greater-than-operator-), [`<=` (less than or equal)](#less-than-or-equal-operator-), and [`>=` (greater than or equal)](#greater-than-or-equal-operator-) comparison, also known as relational, operators compare their operands.</span></span> <span data-ttu-id="9183a-105">這些運算子可支援所有[整數](../keywords/integral-types-table.md)和[浮點](../keywords/floating-point-types-table.md)數字型別。</span><span class="sxs-lookup"><span data-stu-id="9183a-105">Those operators support all [integral](../keywords/integral-types-table.md) and [floating-point](../keywords/floating-point-types-table.md) numeric types.</span></span>

> [!NOTE]
> <span data-ttu-id="9183a-106">針對 `==`、`<`、`>`、`<=` 和 `>=` 運算子，如果任何運算元不是數字 (<xref:System.Double.NaN?displayProperty=nameWithType> 或 <xref:System.Single.NaN?displayProperty=nameWithType>)，則作業的結果是 `false`。</span><span class="sxs-lookup"><span data-stu-id="9183a-106">For the `==`, `<`, `>`, `<=`, and `>=` operators, if any of the operands is not a number (<xref:System.Double.NaN?displayProperty=nameWithType> or <xref:System.Single.NaN?displayProperty=nameWithType>), the result of operation is `false`.</span></span> <span data-ttu-id="9183a-107">這代表 `NaN` 的值皆不會大於、小於或等於任何其他 `double` (或 `float`) 的值，包括 `NaN`。</span><span class="sxs-lookup"><span data-stu-id="9183a-107">That means that the `NaN` value is neither greater than, less than, nor equal to any other `double` (or `float`) value, including `NaN`.</span></span> <span data-ttu-id="9183a-108">如需詳細資訊和範例，請參閱 <xref:System.Double.NaN?displayProperty=nameWithType> 或 <xref:System.Single.NaN?displayProperty=nameWithType> 參考文章。</span><span class="sxs-lookup"><span data-stu-id="9183a-108">For more information and examples, see the <xref:System.Double.NaN?displayProperty=nameWithType> or <xref:System.Single.NaN?displayProperty=nameWithType> reference article.</span></span>

<span data-ttu-id="9183a-109">列舉類型也支援比較運算子。</span><span class="sxs-lookup"><span data-stu-id="9183a-109">Enumeration types also support comparison operators.</span></span> <span data-ttu-id="9183a-110">相同[列舉](../keywords/enum.md)類型的運算元會比較基礎整數型別的對應值。</span><span class="sxs-lookup"><span data-stu-id="9183a-110">For operands of the same [enum](../keywords/enum.md) type, the corresponding values of the underlying integral type are compared.</span></span>

<span data-ttu-id="9183a-111">[`==` 和 `!=` 運算子](equality-operators.md) 會檢查其運算元是否相等。</span><span class="sxs-lookup"><span data-stu-id="9183a-111">The [`==` and `!=` operators](equality-operators.md) check if their operands are equal or not.</span></span>

## <a name="less-than-operator-"></a><span data-ttu-id="9183a-112">小於運算子 \<</span><span class="sxs-lookup"><span data-stu-id="9183a-112">Less than operator \<</span></span>

<span data-ttu-id="9183a-113">`<` 運算子會傳回 `true` (如果其第一個運算元小於第二個運算元)，否則會傳回 `false`：</span><span class="sxs-lookup"><span data-stu-id="9183a-113">The `<` operator returns `true` if its first operand is less than its second operand, `false` otherwise:</span></span>

[!code-csharp-interactive[less than example](~/samples/snippets/csharp/language-reference/operators/GreaterAndLessOperatorsExamples.cs#Less)]

## <a name="greater-than-operator-"></a><span data-ttu-id="9183a-114">大於運算子 ></span><span class="sxs-lookup"><span data-stu-id="9183a-114">Greater than operator ></span></span>

<span data-ttu-id="9183a-115">`>` 運算子會傳回 `true` (如果其第一個運算元大於第二個運算元)，否則會傳回 `false`：</span><span class="sxs-lookup"><span data-stu-id="9183a-115">The `>` operator returns `true` if its first operand is greater than its second operand, `false` otherwise:</span></span>

[!code-csharp-interactive[greater than example](~/samples/snippets/csharp/language-reference/operators/GreaterAndLessOperatorsExamples.cs#Greater)]

## <a name="less-than-or-equal-operator-"></a><span data-ttu-id="9183a-116">小於或等於運算子 \<=</span><span class="sxs-lookup"><span data-stu-id="9183a-116">Less than or equal operator \<=</span></span>

<span data-ttu-id="9183a-117">`<=` 運算子會傳回 `true` (如果其第一個運算元小於或等於第二個運算元)，否則會傳回 `false`：</span><span class="sxs-lookup"><span data-stu-id="9183a-117">The `<=` operator returns `true` if its first operand is less than or equal to its second operand, `false` otherwise:</span></span>

[!code-csharp-interactive[less than or equal example](~/samples/snippets/csharp/language-reference/operators/GreaterAndLessOperatorsExamples.cs#LessOrEqual)]

## <a name="greater-than-or-equal-operator-"></a><span data-ttu-id="9183a-118">大於或等於運算子 >=</span><span class="sxs-lookup"><span data-stu-id="9183a-118">Greater than or equal operator >=</span></span>

<span data-ttu-id="9183a-119">`>=` 運算子會傳回 `true` (如果其第一個運算元大於或等於第二個運算元)，否則會傳回 `false`：</span><span class="sxs-lookup"><span data-stu-id="9183a-119">The `>=` operator returns `true` if its first operand is greater than or equal to its second operand, `false` otherwise:</span></span>

[!code-csharp-interactive[greater than or equal example](~/samples/snippets/csharp/language-reference/operators/GreaterAndLessOperatorsExamples.cs#GreaterOrEqual)]

## <a name="operator-overloadability"></a><span data-ttu-id="9183a-120">運算子是否可多載</span><span class="sxs-lookup"><span data-stu-id="9183a-120">Operator overloadability</span></span>

<span data-ttu-id="9183a-121">使用者定義類型可以[多載](../keywords/operator.md) `<`、`>`、`<=` 及 `>=` 運算子。</span><span class="sxs-lookup"><span data-stu-id="9183a-121">A user-defined type can [overload](../keywords/operator.md) the `<`, `>`, `<=`, and `>=` operators.</span></span>

<span data-ttu-id="9183a-122">如果類型多載 `<` 或 `>` 運算子之一，則也必須多載 `<` 和 `>`。</span><span class="sxs-lookup"><span data-stu-id="9183a-122">If a type overloads one of the `<` or `>` operators, it must overload both `<` and `>`.</span></span> <span data-ttu-id="9183a-123">如果類型多載 `<=` 或 `>=` 運算子之一，則也必須多載 `<=` 和 `>=`。</span><span class="sxs-lookup"><span data-stu-id="9183a-123">If a type overloads one of the `<=` or `>=` operators, it must overload both `<=` and `>=`.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="9183a-124">C# 語言規格</span><span class="sxs-lookup"><span data-stu-id="9183a-124">C# language specification</span></span>

<span data-ttu-id="9183a-125">如需詳細資訊，請參閱 [C# 語言規格](~/_csharplang/spec/introduction.md)的[關係及類型測試運算子](~/_csharplang/spec/expressions.md#relational-and-type-testing-operators)一節。</span><span class="sxs-lookup"><span data-stu-id="9183a-125">For more information, see the [Relational and type-testing operators](~/_csharplang/spec/expressions.md#relational-and-type-testing-operators) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="9183a-126">另請參閱</span><span class="sxs-lookup"><span data-stu-id="9183a-126">See also</span></span>

- [<span data-ttu-id="9183a-127">C# 參考</span><span class="sxs-lookup"><span data-stu-id="9183a-127">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="9183a-128">C# 程式設計指南</span><span class="sxs-lookup"><span data-stu-id="9183a-128">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="9183a-129">C# 運算子</span><span class="sxs-lookup"><span data-stu-id="9183a-129">C# Operators</span></span>](index.md)
- <xref:System.IComparable%601?displayProperty=nameWithType>
- [<span data-ttu-id="9183a-130">等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="9183a-130">Equality operators</span></span>](equality-operators.md)