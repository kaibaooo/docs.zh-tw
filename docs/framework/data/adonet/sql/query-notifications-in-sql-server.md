---
title: SQL Server 中的查詢通知
ms.date: 03/30/2017
ms.assetid: 0f0ba1a1-3180-4af8-87f7-c795dc8f8f55
ms.openlocfilehash: 11d9a1a800bea4224853a57b128ca89c9f2cf781
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452366"
---
# <a name="query-notifications-in-sql-server"></a>SQL Server 中的查詢通知
查詢通知是根據 Service Broker 基礎結構而建置，可讓應用程式在資料變更時收到通知。 此功能對於從資料庫中提供資訊快取的應用程式 (如 Web 應用程式)，及需要在來源資料變更時收到通知的應用程式來說非常有用。  
  
 您可以使用 ADO.NET，以三種方式實作查詢通知：  
  
1. 由公開伺服器端功能的 `SqlNotificationRequest` 類別所提供的低層級實作，它可讓您使用通知要求執行命令。  
  
2. 由在來源應用程式與 SQL Server 之間提供高層級抽象通知功能之 `SqlDependency` 類別所提供的高層級實作，可讓您使用相依性偵測伺服器中的變更。 在多數情況下，對於使用 .NET Framework Data Provider for SQL Server 的 Managed 用戶端應用程式而言，這是利用 SQL Server 通知功能最簡單且最有效的方式。  
  
3. 此外，使用 ASP.NET 2.0 或更新版本建置的 Web 應用程式可以使用 `SqlCacheDependency` Helper 類別。  
  
 查詢通知可用於需要重新整理顯示或快取，以回應基礎資料變更的應用程式中。 Microsoft SQL Server 可讓 .NET Framework 應用程式傳送命令至 SQL Server，並要求當執行相同命令而產生與最初擷取的不同結果集時，就產生通知。 在伺服器所產生的通知會透過佇列傳送，以供稍後處理。  
  
 您可以針對 SELECT 和 EXECUTE 陳述式設定通知。 使用 EXECUTE 陳述式時，SQL Server 會針對執行的命令而非 EXECUTE 陳述式本身註冊通知。 此命令必須符合 SELECT 陳述式的需求和限制。 當註冊通知的命令包含多個陳述式時，資料庫引擎會為批次中的每個陳述式建立一個通知。  
  
 如果您要開發的應用程式在資料變更時需要可靠的次要秒數通知，請參閱**規劃有效率的查詢通知策略**和[規劃通知](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms187528(v=sql.105))一文中**查詢通知的替代**章節。 如需有關查詢通知和 SQL Server Service Broker 的詳細資訊，請參閱 SQL Server 檔中的下列文章連結。  
  
 **SQL Server 文件**  
  
- [使用查詢通知](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms175110(v=sql.105))  
  
- [為通知建立查詢](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms181122(v=sql.105))  
  
- [開發 (Service Broker)](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/bb522889(v=sql.105))  
  
- [Service Broker 開發人員資訊中心](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms166100(v=sql.105))  
  
- [開發人員手冊 (Service Broker)](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/bb522908(v=sql.105))  
  
## <a name="in-this-section"></a>本節內容  
 [啟用查詢通知](enabling-query-notifications.md)  
 討論如何使用查詢通知，包括其啟用及使用需求。  
  
 [ASP.NET 應用程式中的 SqlDependency](sqldependency-in-an-aspnet-app.md)  
 示範如何使用 ASP.NET 應用程式的查詢通知。  
  
 [使用 SqlDependency 偵測變更](detecting-changes-with-sqldependency.md)  
 示範如何偵測查詢結果何時會與原來接收的結果不同。  
  
 [使用 SqlNotificationRequest 執行 SqlCommand](sqlcommand-execution-with-a-sqlnotificationrequest.md)  
 示範如何將 <xref:System.Data.SqlClient.SqlCommand> 物件設定為搭配查詢通知使用。  
  
## <a name="reference"></a>參考  
 <xref:System.Data.Sql.SqlNotificationRequest>  
 說明 <xref:System.Data.Sql.SqlNotificationRequest> 類別及其所有成員。  
  
 <xref:System.Data.SqlClient.SqlDependency>  
 說明 <xref:System.Data.SqlClient.SqlDependency> 類別及其所有成員。  
  
 <xref:System.Web.Caching.SqlCacheDependency>  
 說明 <xref:System.Web.Caching.SqlCacheDependency> 類別及其所有成員。  
  
## <a name="see-also"></a>另請參閱

- [SQL Server and ADO.NET](index.md) (SQL Server 和 ADO.NET)
- [ADO.NET 概觀](../ado-net-overview.md)
