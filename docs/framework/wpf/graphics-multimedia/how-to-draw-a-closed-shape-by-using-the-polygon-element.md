---
title: 如何：使用 Polygon 項目繪製封閉的形狀
ms.date: 03/30/2017
helpviewer_keywords:
- graphics [WPF], Polygon elements
- closed shapes [WPF], drawing with Polygon elements
- Polygon elements [WPF]
- drawing [WPF], closed shapes with Polygon elements
ms.assetid: 4b0ca008-29ce-48dd-8bc3-f3a20ffca6a6
ms.openlocfilehash: 324a5623ee658789b8600a43a89ce26cab7cd6cd
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452970"
---
# <a name="how-to-draw-a-closed-shape-by-using-the-polygon-element"></a>如何：使用 Polygon 項目繪製封閉的形狀
這個範例示範如何使用 <xref:System.Windows.Shapes.Polygon> 元素繪製封閉的圖形。 若要繪製封閉圖形，請建立 <xref:System.Windows.Shapes.Polygon> 元素，並使用其 <xref:System.Windows.Shapes.Polygon.Points%2A> 屬性來指定圖形的頂點。 會自動繪製一條線來連接第一個和最後一個點。 最後，指定 <xref:System.Windows.Shapes.Shape.Fill%2A>、<xref:System.Windows.Shapes.Shape.Stroke%2A>或兩者。  
  
## <a name="example"></a>範例  
 在 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]中，點的有效語法是以空格分隔的逗號分隔 x 和 y 座標配對清單。  
  
 [!code-xaml[drawingwithshapeelements#PolygonExample1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingWithShapeElements/CS/polygonexample.xaml#polygonexample1)]  
  
 雖然此範例使用包含多邊形的 <xref:System.Windows.Controls.Canvas>，但您可以使用多邊形元素（以及所有其他圖形元素）搭配任何支援非文字內容的 <xref:System.Windows.Controls.Panel> 或 <xref:System.Windows.Controls.Control>。  
  
 這個範例是較大範例的一部分;如需完整範例，請參閱[圖形元素範例](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements)。
