---
title: 操作說明：平移元素
ms.date: 03/30/2017
helpviewer_keywords:
- graphics [WPF], translations
ms.assetid: 461c8273-15df-42f6-8672-89ba22887cc0
ms.openlocfilehash: ba6bda09a4ee189cdd1a32eed8f65b32d1a9abe4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79187309"
---
# <a name="how-to-translate-an-element"></a>操作說明：平移元素
此示例演示如何使用 轉換（移動）元素<xref:System.Windows.Media.TranslateTransform>。  
  
 該<xref:System.Windows.Media.TranslateTransform>類對於在不支援絕對位置的面板內移動元素特別有用。 例如，<xref:System.Windows.Media.TranslateTransform>通過將 應用於元素的屬性<xref:System.Windows.UIElement.RenderTransform%2A>，可以在<xref:System.Windows.Controls.StackPanel>或<xref:System.Windows.Controls.DockPanel>中移動元素。  
  
 使用<xref:System.Windows.Media.TranslateTransform.X%2A>屬性<xref:System.Windows.Media.TranslateTransform>指定以圖元為單位的數量沿 X 軸移動元素。 使用<xref:System.Windows.Media.TranslateTransform.Y%2A>屬性指定沿 y 軸移動元素的數量（以圖元為單位）。 最後，將<xref:System.Windows.Media.TranslateTransform>應用<xref:System.Windows.UIElement.RenderTransform%2A>到 元素的屬性。  
  
 下面的示例使用<xref:System.Windows.Media.TranslateTransform>將元素向右移動 50 圖元，向下移動 50 圖元。  
  
## <a name="example"></a>範例  
 [!code-xaml[transformsSample#53](~/samples/snippets/csharp/VS_Snippets_Wpf/transformsSample/CS/TranslateTransformExample.xaml#53)]  
  
 如需完整範例，請參閱 [2D 轉換範例](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/2DTransforms)。  
  
## <a name="see-also"></a>另請參閱

- [轉換概觀](transforms-overview.md)
