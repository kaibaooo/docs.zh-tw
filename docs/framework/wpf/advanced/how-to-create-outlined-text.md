---
title: 如何：建立外框文字
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- typography [WPF], linear gradient brush
- outlined text [WPF]
- gradient brush [WPF]
- linear gradient brush [WPF]
- typography [WPF], outline effects
ms.assetid: 4aa3cf6e-1953-4f26-8230-7c1409e5f28d
ms.openlocfilehash: d0ce46b9895589fd4635b567136204368a6431ad
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79186864"
---
# <a name="how-to-create-outlined-text"></a>如何：建立外框文字
在大多數情況下，當您在[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]應用程式中向文本字串添加修飾時，您將使用文本作為離散字元或字形的集合。 例如，您可以創建線性漸層畫筆並將其應用於<xref:System.Windows.Controls.Control.Foreground%2A><xref:System.Windows.Controls.TextBox>物件的屬性。 顯示或編輯文字方塊時，線性漸層畫筆將自動應用於文本字串中的當前字元集。  
  
 ![以線性漸層筆刷顯示的文字](./media/how-to-create-outlined-text/text-linear-gradient.jpg)
  
 但是，您還可以將文本轉換為<xref:System.Windows.Media.Geometry>物件，從而創建其他類型的視覺豐富的文本。 例如，可以基於文本字串的<xref:System.Windows.Media.Geometry>輪廓創建物件。  
  
 ![以線性漸層筆刷繪製外框的文字](./media/how-to-create-outlined-text/text-outline-linear-gradient.jpg)  
  
 將文本轉換為物件時<xref:System.Windows.Media.Geometry>，它不再是字元的集合，無法修改文本字串中的字元。 不過，您可以修改其筆劃與填滿屬性來影響轉換文字的外觀。 筆劃是指轉換文字的外框，填滿是指轉換文字外框內的區域。  
  
 以下示例說明了通過修改描邊和填滿轉換文本來創建視覺效果的幾種方法。  
  
 ![使用不同填色和筆觸色彩的文字](./media/how-to-create-outlined-text/fill-stroke-text-effect.jpg)  
  
 ![影像筆刷套用至筆觸的文字](./media/how-to-create-outlined-text/image-brush-application.jpg)
  
 還可以修改轉換文本的邊界框矩形或高光。 下面的示例說明了通過修改轉換文本的描邊和高光來創建視覺效果的方法。  
  
 ![應用影像筆刷進行描邊和高光的文本](./media/how-to-create-outlined-text/image-brush-text-application.jpg)

## <a name="example"></a>範例  
 將文本轉換為<xref:System.Windows.Media.Geometry>物件的鍵是使用該<xref:System.Windows.Media.FormattedText>物件。 創建此物件後，可以使用<xref:System.Windows.Media.FormattedText.BuildGeometry%2A>和<xref:System.Windows.Media.FormattedText.BuildHighlightGeometry%2A>方法將文本轉換為<xref:System.Windows.Media.Geometry>物件。 第一種方法返回格式化文字的幾何體;第二種方法返回格式化文字的邊界框的幾何體。 以下代碼示例演示如何創建<xref:System.Windows.Media.FormattedText>物件並檢索格式化文字及其邊界框的幾何體。  
  
 [!code-csharp[OutlineTextControlViewer#CreateText](~/samples/snippets/csharp/VS_Snippets_Wpf/OutlineTextControlViewer/CSharp/OutlineTextControl.cs#createtext)]
 [!code-vb[OutlineTextControlViewer#CreateText](~/samples/snippets/visualbasic/VS_Snippets_Wpf/OutlineTextControlViewer/visualbasic/outlinetextcontrol.vb#createtext)]  
  
 為了顯示檢索到<xref:System.Windows.Media.Geometry>的物件，您需要訪問<xref:System.Windows.Media.DrawingContext>顯示轉換文本的物件。 在這些代碼示例中，這是通過創建從支援使用者定義的呈現的類派生的自訂控制項物件來實現的。  
  
 要在<xref:System.Windows.Media.Geometry>自訂控制項中顯示物件，請為<xref:System.Windows.UIElement.OnRender%2A>方法提供重寫。 重寫的方法應使用 方法<xref:System.Windows.Media.DrawingContext.DrawGeometry%2A>繪製<xref:System.Windows.Media.Geometry>物件。  
  
 [!code-csharp[OutlineTextControlViewer#OnRender](~/samples/snippets/csharp/VS_Snippets_Wpf/OutlineTextControlViewer/CSharp/OutlineTextControl.cs#onrender)]
 [!code-vb[OutlineTextControlViewer#OnRender](~/samples/snippets/visualbasic/VS_Snippets_Wpf/OutlineTextControlViewer/visualbasic/outlinetextcontrol.vb#onrender)]  
  
  有關示例自訂使用者控制項物件的源，請參閱[C# 和](https://github.com/dotnet/samples/blob/master/snippets/csharp/VS_Snippets_Wpf/OutlineTextControlViewer/CSharp/OutlineTextControl.cs)[大綱文本控制.vb 的 OutlineTextControl.cs。](https://github.com/dotnet/samples/blob/master/snippets/visualbasic/VS_Snippets_Wpf/OutlineTextControlViewer/visualbasic/outlinetextcontrol.vb)
  
## <a name="see-also"></a>另請參閱

- [繪製格式化的文字](drawing-formatted-text.md)
