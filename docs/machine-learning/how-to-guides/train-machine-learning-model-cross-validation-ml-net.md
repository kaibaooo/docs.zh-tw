---
title: 使用交叉驗證定型機器學習模型
description: 了解如何使用交叉驗證在 ML.NET 中建置更強大的機器學習模型。 交叉驗證是一種定型和模型評估技巧，會將資料分割成幾個分割，在這些分割上定型多個演算法。
ms.date: 08/29/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to,title-hack-0625
ms.openlocfilehash: 87eae789478752423f3e682d4db6cead0391aa6e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "73976919"
---
# <a name="train-a-machine-learning-model-using-cross-validation"></a>使用交叉驗證定型機器學習模型

了解如何使用交叉驗證在 ML.NET 中定型更強固的機器學習模型。

交叉驗證是一種定型和模型評估技巧，會將資料分割成幾個分割，在這些分割上定型多個演算法。 這項技巧藉由定型程序提供的資料，改善模型的穩定性。 除了提升看不見的觀察值效能之外，在資料限制的環境中，它是使用較小資料集定型模型的有效工具。

## <a name="the-data-and-data-model"></a>資料和資料模型

檔案提供的資料具有下列格式：

```text
Size (Sq. ft.), HistoricalPrice1 ($), HistoricalPrice2 ($), HistoricalPrice3 ($), Current Price ($)
620.00, 148330.32, 140913.81, 136686.39, 146105.37
550.00, 557033.46, 529181.78, 513306.33, 548677.95
1127.00, 479320.99, 455354.94, 441694.30, 472131.18
1120.00, 47504.98, 45129.73, 43775.84, 46792.41
```

資料可以按類（如`HousingData`）建模並載入到 中[`IDataView`](xref:Microsoft.ML.IDataView)。

```csharp
public class HousingData
{
    [LoadColumn(0)]
    public float Size { get; set; }

    [LoadColumn(1, 3)]
    [VectorType(3)]
    public float[] HistoricalPrices { get; set; }

    [LoadColumn(4)]
    [ColumnName("Label")]
    public float CurrentPrice { get; set; }
}
```

## <a name="prepare-the-data"></a>準備資料

先前置處理資料，再用它建置機器學習模型。 在此示例中，`Size`和`HistoricalPrices`列合併到單個要素向量中，該向量是使用 方法輸出到稱為`Features`新列的[`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*)。 除了將資料變成 ML.NET 演算法預期的格式，串連資料行最佳化管線中的後續作業，方法是將作業一次套用到串連資料行，而不是各個資料行分別套用。

將列組合到單個向量中後，[`NormalizeMinMax`](xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax*)將應用於要獲取`Features``Size`的列，並在`HistoricalPrices`0-1 之間的相同範圍內。

```csharp
// Define data prep estimator
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", new string[] { "Size", "HistoricalPrices" })
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// Create data prep transformer
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(data);

// Transform data
IDataView transformedData = dataPrepTransformer.Transform(data);
```

## <a name="train-model-with-cross-validation"></a>使用交叉驗證定型模型

一旦資料經過預先處理，就可以定型模型。 首先，選取最能配合機器學習工作執行的演算法。 因為預測的值是數值連續值，所以工作為迴歸。 ML.NET實現的回歸演算法之一是該[`StochasticDualCoordinateAscentCoordinator`](xref:Microsoft.ML.Trainers.SdcaRegressionTrainer)演算法。 要使用 方法使用交叉驗證[`CrossValidate`](xref:Microsoft.ML.RegressionCatalog.CrossValidate*)訓練模型。

> [!NOTE]
> 雖然這個範例使用線性迴歸模型，但除異常偵測外，CrossValidate 適用於 ML.NET 中的所有其他機器學習工作。

```csharp
// Define StochasticDualCoordinateAscent algorithm estimator
IEstimator<ITransformer> sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// Apply 5-fold cross validation
var cvResults = mlContext.Regression.CrossValidate(transformedData, sdcaEstimator, numberOfFolds: 5);
```

[`CrossValidate`](xref:Microsoft.ML.RegressionCatalog.CrossValidate*)執行以下操作：

1. 將資料分割成 `numberOfFolds` 參數指定值的數目。 每個分區的結果都是一個[`TrainTestData`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestData)物件。
1. 對定型資料集使用指定的機器學習演算法評估工具，在每個分割上定型模型。
1. 使用測試資料集上[`Evaluate`](xref:Microsoft.ML.RegressionCatalog.Evaluate*)的方法評估每個模型的性能。
1. 針對每個模型傳回模型及其計量。

存儲的結果`cvResults`是[`CrossValidationResult`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601)物件的集合。 此物件包括已訓練的模型以及分別可從 和[`Model`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601.Model)[`Metrics`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601.Metrics)屬性訪問的指標。 在此示例中，`Model`屬性為 類型[`ITransformer`](xref:Microsoft.ML.ITransformer)，`Metrics`屬性為 類型。 [`RegressionMetrics`](xref:Microsoft.ML.Data.RegressionMetrics)

## <a name="evaluate-the-model"></a>評估模型

可以通過單個`Metrics`[`CrossValidationResult`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601)物件的屬性訪問不同訓練模型的指標。 在本例中，[R 平方計量](https://en.wikipedia.org/wiki/Coefficient_of_determination)是在變數 `rSquared` 中存取儲存。

```csharp
IEnumerable<double> rSquared =
    cvResults
        .Select(fold => fold.Metrics.RSquared);
```

如果您檢查 `rSquared` 變數的內容，輸出應該是介於 0-1 之間的五個值，接近 1 表示最佳。 使用如 R 平方的計量，選取表現最佳到最差的模型。 然後，選取要進行預測或執行其他作業的最上層模型。

```csharp
// Select all models
ITransformer[] models =
    cvResults
        .OrderByDescending(fold => fold.Metrics.RSquared)
        .Select(fold => fold.Model)
        .ToArray();

// Get Top Model
ITransformer topModel = models[0];
```
