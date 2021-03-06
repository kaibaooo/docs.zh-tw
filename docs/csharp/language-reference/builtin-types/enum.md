---
title: 枚舉類型 - C# 引用
description: 瞭解表示選擇或選項群組合的 C# 枚舉類型
ms.date: 12/13/2019
f1_keywords:
- enum
- enum_CSharpKeyword
helpviewer_keywords:
- enum keyword [C#]
- enum type [C#]
- enumeration type [C#]
- bit flags [C#]
ms.assetid: bbeb9a0f-e9b3-41ab-b0a6-c41b1a08974c
ms.openlocfilehash: ab5eb1679f846bf0e25d90a4d0e0a71f0bdb0096
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "78847701"
---
# <a name="enumeration-types-c-reference"></a>枚舉類型（C# 引用）

*枚舉類型*（或*枚舉類型*）是由基礎[整體數值](integral-numeric-types.md)類型的一組命名常量定義的[數值型別](value-types.md)。 要定義枚舉類型，請使用 關鍵字`enum`並指定*枚舉成員*的名稱：

```csharp
enum Season
{
    Spring,
    Summer,
    Autumn,
    Winter
}
```

預設情況下，枚舉成員的關聯常量值為`int`類型 ;它們以零開始，然後按照定義文本順序增加一個。 您可以顯式指定任何其他[積分數值](integral-numeric-types.md)類型作為枚舉類型的基礎類型。 您還可以顯式指定關聯的常量值，如以下示例所示：

```csharp
enum ErrorCode : ushort
{
    None = 0,
    Unknown = 1,
    ConnectionLost = 100,
    OutlierReading = 200
}
```

不能在枚舉類型的定義中定義方法。 要將功能添加到枚舉類型，請創建[擴充方法](../../programming-guide/classes-and-structs/extension-methods.md)。

枚舉類型的`E`預設值是運算式`(E)0`生成的值，即使零沒有相應的枚舉成員也是如此。

您可以使用枚舉類型表示一組互斥值或選項群組合的選擇。 要表示選項的組合，請將枚舉類型定義為位標誌。

## <a name="enumeration-types-as-bit-flags"></a>作為位元旗標的列舉類型

如果希望枚舉類型表示選項的組合，請為這些選項定義枚舉成員，以便單個選項是位欄位。 也就是說，這些枚舉成員的相關值應該是兩者的權力。 然後，您可以使用[位邏輯運算子`|``&`，也可以](../operators/bitwise-and-shift-operators.md#enumeration-logical-operators)分別組合選項或相交選項群組合。 要指示枚舉型別宣告位欄位，請對其應用[Flags](xref:System.FlagsAttribute)屬性。 如以下示例所示，您還可以在枚舉類型的定義中包括一些典型的組合。

[!code-csharp[enum flags](snippets/EnumType.cs#Flags)]

有關詳細資訊和示例，請參閱<xref:System.FlagsAttribute?displayProperty=nameWithType>API 參考頁和非[獨佔成員以及](/dotnet/api/system.enum#non-exclusive-members-and-the-flags-attribute) <xref:System.Enum?displayProperty=nameWithType> API 參考頁的標誌屬性部分。

## <a name="the-systemenum-type-and-enum-constraint"></a>系統.枚舉類型和枚舉約束

類型<xref:System.Enum?displayProperty=nameWithType>是所有枚舉類型的抽象基類。 它提供了許多方法來獲取有關枚舉類型及其值的資訊。 有關詳細資訊和示例，<xref:System.Enum?displayProperty=nameWithType>請參閱 API 參考頁。

從 C# 7.3 開始，`System.Enum`可以在基類約束（稱為[枚舉約束](../../programming-guide/generics/constraints-on-type-parameters.md#enum-constraints)）中使用來指定類型參數是枚舉類型。

## <a name="conversions"></a>轉換

對於任何枚舉類型，枚舉類型與其基礎積分類型之間存在顯式轉換。 如果將枚舉值[轉換為](../operators/type-testing-and-cast.md#cast-operator-)其基礎類型，則結果為枚舉成員的關聯積分值。

[!code-csharp[enum conversions](snippets/EnumType.cs#Conversions)]

使用<xref:System.Enum.IsDefined%2A?displayProperty=nameWithType>方法確定枚舉類型是否包含具有特定關聯值的枚舉成員。

對於任何枚舉類型，分別存在與<xref:System.Enum?displayProperty=nameWithType>類型轉換的[裝箱和取消裝箱](../../programming-guide/types/boxing-and-unboxing.md)轉換。

## <a name="c-language-specification"></a>C# 語言規格

如需詳細資訊，請參閱 [C# 語言規格](~/_csharplang/spec/introduction.md)的下列幾節：

- [列舉](~/_csharplang/spec/enums.md)
- [列舉值和作業](~/_csharplang/spec/enums.md#enum-values-and-operations)
- [列舉邏輯運算子](~/_csharplang/spec/expressions.md#enumeration-logical-operators)
- [枚舉比較運算子](~/_csharplang/spec/expressions.md#enumeration-comparison-operators)
- [顯式枚舉轉換](~/_csharplang/spec/conversions.md#explicit-enumeration-conversions)
- [隱式枚舉轉換](~/_csharplang/spec/conversions.md#implicit-enumeration-conversions)

## <a name="see-also"></a>另請參閱

- [C# 參考](../index.md)
- [列舉類型格式字串](../../../standard/base-types/enumeration-format-strings.md)
- [設計指南 - 枚舉設計](../../../standard/design-guidelines/enum.md)
- [設計指南 - 枚舉命名約定](../../../standard/design-guidelines/names-of-classes-structs-and-interfaces.md#naming-enumerations)
- [Switch 陳述式](../keywords/switch.md)
