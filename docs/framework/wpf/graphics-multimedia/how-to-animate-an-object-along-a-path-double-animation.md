---
title: 操作說明：沿著路徑建立物件的動畫 (Double 動畫)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- animation [WPF], objects along paths (double animation)
- double animation [WPF]
ms.assetid: 5a3c4a99-f303-42ad-a52a-e4794bb1798e
ms.openlocfilehash: 084caac26fd68b6914ec3858652ec44557a0dbd7
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452853"
---
# <a name="how-to-animate-an-object-along-a-path-double-animation"></a>操作說明：沿著路徑建立物件的動畫 (Double 動畫)
這個範例示範如何使用 <xref:System.Windows.Media.Animation.DoubleAnimationUsingPath> 類別，沿著 <xref:System.Windows.Media.PathGeometry>所定義的路徑移動物件。  
  
## <a name="example"></a>範例  
 下列範例會使用兩個 <xref:System.Windows.Media.Animation.DoubleAnimationUsingPath> 物件，沿著幾何路徑移動矩形：  
  
- 第一個 <xref:System.Windows.Media.Animation.DoubleAnimationUsingPath> 會繪製套用至矩形之 <xref:System.Windows.Media.TranslateTransform> <xref:System.Windows.Media.TranslateTransform.X%2A> 的動畫。 它會使矩形沿著路徑水平移動。  
  
- 第二個 <xref:System.Windows.Media.Animation.DoubleAnimationUsingPath> 會繪製套用至矩形之 <xref:System.Windows.Media.TranslateTransform> <xref:System.Windows.Media.TranslateTransform.Y%2A> 的動畫。 它會使矩形沿著路徑垂直移動。  
  
 [!code-xaml[PathAnimationGallery_snippet#DoubleAnimationUsingPathWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_snippet/CS/doubleanimationusingpathexample.xaml#doubleanimationusingpathwholepage)]  
  
 [!code-csharp[PathAnimationGallery_procedural_snip#DoubleAnimationUsingPathWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/CSharp/DoubleAnimationUsingPathExample.cs#doubleanimationusingpathwholepage)]
 [!code-vb[PathAnimationGallery_procedural_snip#DoubleAnimationUsingPathWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/VisualBasic/DoubleAnimationUsingPathExample.vb#doubleanimationusingpathwholepage)]  
  
 如需完整範例，請參閱[路徑動畫範例](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations)。  
  
 使用幾何路徑移動物件的另一種方式是使用 <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath> 物件。 如需範例，請參閱[沿著路徑建立物件的動畫（矩陣動畫）](how-to-animate-an-object-along-a-path-matrix-animation.md)。  
  
## <a name="see-also"></a>另請參閱

- [動畫概觀](animation-overview.md)
- [路徑動畫操作說明主題](path-animation-how-to-topics.md)
