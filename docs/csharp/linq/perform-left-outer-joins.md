---
title: 執行左方外部聯結 (C# 中的 LINQ)
description: 了解如何使用 C# 中的 LINQ 執行左方外部聯結。
ms.date: 12/01/2016
ms.assetid: f542cee6-3169-4dcf-a631-3a6a79ccd473
ms.openlocfilehash: cc08a1c8670623a10d1e0bf10221d02037a8d7bc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "61688575"
---
# <a name="perform-left-outer-joins"></a>執行左方外部聯結

左方外部聯結是第一個集合中的每個項目都會傳回的聯結，不論它在第二個集合中是否有任何相互關聯的項目。 您可以對群組聯結的結果呼叫 <xref:System.Linq.Enumerable.DefaultIfEmpty%2A> 方法，使用 LINQ 執行左方外部聯結。

## <a name="example"></a>範例

下列範例示範如何對群組聯結的結果使用 <xref:System.Linq.Enumerable.DefaultIfEmpty%2A> 方法，來執行左方外部聯結。

產生兩個集合的左方外部聯結的第一個步驟，是使用群組聯結執行內部聯結。 （有關此過程的說明，請參閱[執行內部聯接](perform-inner-joins.md)。在此示例中，`Person`物件清單根據匹配`Pet``Person``Pet.Owner`的物件與物件聯接到物件清單中。

第二個步驟是在結果集中包含第一個 (左) 集合中的每個項目，即使該元素在右集合中沒有相符的項目。 對來自群組聯結的每個相符項目序列呼叫 <xref:System.Linq.Enumerable.DefaultIfEmpty%2A> 即可完成此作業。 在此範例中，會在每個相符 `Pet` 物件的序列上呼叫 <xref:System.Linq.Enumerable.DefaultIfEmpty%2A>。 如果任何 `Person` 物件的相符 `Pet` 物件序列是空的，方法會傳回包含單一預設值的集合，藉此確保結果集合會顯示每個 `Person` 物件。

> [!NOTE]
> 參考型別的預設值是 `null`，因此範例會先檢查 Null 參考，再存取每個 `Pet` 集合的每個項目。

[!code-csharp[CsLINQProgJoining#7](~/samples/snippets/csharp/concepts/linq/how-to-perform-left-outer-joins_1.cs)]

## <a name="see-also"></a>另請參閱

- <xref:System.Linq.Enumerable.Join%2A>
- <xref:System.Linq.Enumerable.GroupJoin%2A>
- [執行內部聯結](perform-inner-joins.md)
- [執行群組聯結](perform-grouped-joins.md)
- [匿名型別](../programming-guide/classes-and-structs/anonymous-types.md)
