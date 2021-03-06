---
title: 屬性值繼承
ms.date: 03/30/2017
helpviewer_keywords:
- inheritance [WPF], property values
- value inheritance [WPF]
- properties [WPF], value inheritance
ms.assetid: d7c338f9-f2bf-48ed-832c-7be58ac390e4
ms.openlocfilehash: b63f307d9edffd14315d383d8e06419fa141aee1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79187198"
---
# <a name="property-value-inheritance"></a>屬性值繼承
屬性值繼承是 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 屬性系統的功能。 屬性值繼承可讓元素樹狀結構中的子元素，在將它設定於最接近之父元素中的任一處時，可從父元素中取得特殊屬性的值，並繼承該值。 父元素可能也會透過屬性值繼承來取得它的值，因此，系統有可能會不停地遞迴到頁面根元素。 屬性值繼承不是預設的屬性系統行為；屬性必須使用特殊的中繼資料值來建立，才能讓該屬性起始子元素上的屬性值繼承。  

<a name="Property_Value_Inheritance_is_Containment_Inheritance"></a>
## <a name="property-value-inheritance-is-containment-inheritance"></a>屬性值繼承是內含項目繼承  
 「繼承」在此處做為一個詞彙，它不完全等同於類型內容中的繼承概念，且為一般物件導向程式設計，其中衍生的類別會繼承來自其基底類別的成員定義。 該繼承的意義也適用於 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]︰定義於各種基底類別中的屬性 (Property) 均會在衍生的 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 類別用來做為元素時，公開為其屬性 (Attribute)，並公開為程式碼的成員。 屬性值繼承特別是關於屬性值如何根據元素樹狀結構內的父/子關聯性，從某一個元素繼承至另一個元素。 當您在 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 標記中定義應用程式，於其他元素內巢串元素時，幾乎可以直接看見該元素樹狀結構。 物件的樹狀結構也可以透過程式設計方式，將物件新增至其他物件的指定集合來建立，而屬性值繼承會在執行階段，於完成的樹狀結構中以相同方式來運作。  
  
<a name="Practical_Applications_of_Property_Value_Inheritance"></a>
## <a name="practical-applications-of-property-value-inheritance"></a>屬性值繼承的實際應用程式  
 API[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]包括啟用屬性繼承的幾個屬性。 一般而言，適用於這些屬性的案例是它們所包含的屬性適合在每個頁面上只設定該屬性一次，但該屬性也是其中一個基底元素類別的成員，因此，也會存在於大多數的子元素中。 例如，<xref:System.Windows.FrameworkElement.FlowDirection%2A>屬性控制應在頁面上顯示和排列方向流的內容。 一般而言，您會想要以一致性方式在所有子元素中處理文字流動的概念。 如果使用者或環境動作基於某些因素而在元素樹狀結構的某些層級中重設流動方向，則通常應該全部重設。 <xref:System.Windows.FrameworkElement.FlowDirection%2A>當屬性被繼承時，該值只需在元素樹中包含應用程式中每個頁面的表示需求的級別設置或重置一次。 甚至連初始的預設值也將以這種方式繼承。 屬性值繼承模型仍可讓個別的元素在故意混合流動方向的罕見情況下重設值。  
  
<a name="Making_a_Custom_Property_Inheritable"></a>
## <a name="making-a-custom-property-inheritable"></a>讓自訂屬性成為可繼承  
 藉由變更自訂屬性的中繼資料，您也可以讓自己的自訂屬性成為可繼承。 但請注意，將屬性指定為可繼承有一些效能考量。 假如該屬性沒有已建立的本機值，或是透過樣式、範本或資料繫結取得的值，可繼承的屬性就會為邏輯樹狀結構中的所有子元素提供其指派的屬性值。  
  
 若要讓參與值的屬性成為可繼承，請建立自訂的附加屬性，如[註冊附加屬性](how-to-register-an-attached-property.md)中所述。 使用中繼資料 （<xref:System.Windows.FrameworkPropertyMetadata>） 註冊屬性，並在該中繼資料中的選項設置中指定"繼承"選項。 也請確定屬性具有已建立的預設值，因為該值現在將會繼承。 儘管您已將屬性註冊為附加，但您可能也想要針對擁有者類型上的 get/set 存取建立屬性「包裝函式」，就像您針對「非附加的」相依性屬性所做的一樣。 執行此操作後，可以使用擁有者類型或派生類型的直接屬性包裝器設置可繼承屬性，也可以使用任何<xref:System.Windows.DependencyObject>上的附加屬性語法進行設置。  
  
 附加屬性在概念上類似于全域屬性;您可以檢查任何<xref:System.Windows.DependencyObject>值，並獲得有效的結果。 附加屬性的典型方案是在子項目上設置屬性值，如果相關屬性是附加屬性，並且始終隱式地作為附加屬性存在於樹中的每個元素 （<xref:System.Windows.DependencyObject>） 上，則該方案更為有效。  
  
> [!NOTE]
> 雖然屬性值繼承似乎適用於非附加的相依性屬性，但在執行階段的樹狀結構中，透過特定元素界限的非附加屬性繼承行為是未定義的。 始終用於<xref:System.Windows.DependencyProperty.RegisterAttached%2A>註冊在中繼資料中指定<xref:System.Windows.FrameworkPropertyMetadata.Inherits%2A>的屬性。  
  
<a name="InheritanceContext"></a>
## <a name="inheriting-property-values-across-tree-boundaries"></a>跨樹狀結構界限繼承屬性值  
 屬性繼承的運作方式是周遊元素的樹狀結構。 此樹狀結構通常會與邏輯樹狀結構平行。 但是，每當在定義元素樹（如 a <xref:System.Windows.Media.Brush>） 的標記中包含 WPF 核心級物件時，您都創建了一個不連續的邏輯樹。 真正的邏輯樹在概念上不會通過 展開<xref:System.Windows.Media.Brush>，因為邏輯樹是 WPF 框架級別的概念。 在使用 方法時，您可以看到這反映在結果中<xref:System.Windows.LogicalTreeHelper>。 但是，屬性值繼承可以橋接邏輯樹中的此差距，並且仍然可以傳遞繼承的值，只要可繼承屬性註冊為附加屬性，並且不會遇到故意的繼承阻塞邊界（如<xref:System.Windows.Controls.Frame>a ）。  
  
## <a name="see-also"></a>另請參閱

- [相依性屬性中繼資料](dependency-property-metadata.md)
- [附加屬性概觀](attached-properties-overview.md)
- [依賴項屬性值優先順序](dependency-property-value-precedence.md)
