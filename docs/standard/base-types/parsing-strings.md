---
title: 在 .NET 中剖析字串
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- parsing strings, about parsing strings
- IFormatProvider interface, parsing strings
- base types, parsing strings
- Parse method
- parsing strings
ms.assetid: 5e758b41-db93-456b-8999-99b7304b090d
ms.openlocfilehash: e4bf14981e538d95aebac3b0f36d38b61747989f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "73084320"
---
# <a name="parsing-strings-in-net"></a>在 .NET 中剖析字串
剖析作業會將代表 .NET 基底類型的字串轉換成該基底類型。 例如，剖析作業用來將字串轉換為浮點數或日期和時間值。 最常用來執行剖析作業的方法是 `Parse` 方法。 因為剖析是格式設定的反向作業 (這牽涉到將基底類型別轉換成其字串表示)，所以會套用許多相同的規則和慣例。 就像格式化會使用實作 <xref:System.IFormatProvider> 介面的物件來提供區分文化特性的格式化資訊，剖析也會使用實作 <xref:System.IFormatProvider> 介面的物件來判斷如何解譯字串表示。 如需詳細資訊，請參閱[格式類型](../../../docs/standard/base-types/formatting-types.md)。  
  
## <a name="in-this-section"></a>本節內容  
 [分析數位字串](../../../docs/standard/base-types/parsing-numeric.md)  
 描述如何將字串轉換成 .NET 數值類型。  
  
 [分析日期和時間字串](../../../docs/standard/base-types/parsing-datetime.md)  
 描述如何將字串轉換成 .NET **DateTime** 類型。  
  
 [分析其他字串](../../../docs/standard/base-types/parsing-other.md)  
 描述如何將字串轉換成 **Char**、**Boolean** 和 **Enum** 類型。  
  
## <a name="related-sections"></a>相關章節  
 [格式化類型](../../../docs/standard/base-types/formatting-types.md)  
 描述基本的格式化概念，例如格式規範和格式提供者。  
  
 [.NET 中的類型轉換](../../../docs/standard/base-types/type-conversion.md)  
 描述如何轉換類型。  
  
 [基底類型](../../../docs/standard/base-types/index.md)  
 描述您可對 .NET 基底類型執行的一般作業。
