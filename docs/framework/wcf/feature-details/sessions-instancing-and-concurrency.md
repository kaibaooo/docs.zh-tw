---
title: 工作階段、執行個體與並行
ms.date: 03/30/2017
ms.assetid: 50797a3b-7678-44ed-8138-49ac1602f35b
ms.openlocfilehash: a7466d819e15f3bfe8def2d9407dcf2c6e0c7346
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79184449"
---
# <a name="sessions-instancing-and-concurrency"></a>工作階段、執行個體與並行
「 *工作階段* 」(Session) 是兩個端點之間所傳送之所有訊息的相互關聯。 「*執行個體* 」(Instancing) 是指控制使用者定義之服務物件的存留時間，以及其相關的 <xref:System.ServiceModel.InstanceContext> 物件。 「*並行* 」(Concurrency) 是指控制在 <xref:System.ServiceModel.InstanceContext> 中同時執行的執行緒數目。  
  
 本主題將說明這些設定、這些設定的使用方式，以及這些設定間的各種互動。  
  
## <a name="sessions"></a>工作階段  
 當服務合約將 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> 屬性設定為 <xref:System.ServiceModel.SessionMode.Required?displayProperty=nameWithType>時，該合約會指出所有的呼叫 (即支援呼叫的基礎訊息交換模式) 必須是同一個對話的一部分。 如果合約指定其允許工作階段，但不需要工作階段，則用戶端可以連線，並選擇是否要建立工作階段。 如果工作階段終止並透過相同之工作階段型的通道傳送訊息，就會擲回例外狀況。  
  
 WCF 會話具有以下主要概念特徵：  
  
- 工作階段是由呼叫的應用程式明確地初始化及終止。  
  
- 工作階段期間傳遞的訊息是依其接收的順序進行處理。  
  
- 工作階段會將一群訊息互相關聯為對話。 此相互關聯的意義是一種抽象概念。 例如，某個工作階段架構通道可能會根據共用的網路連線將訊息相互關聯，而另一個工作階段架構通道可能會根據訊息本文內的共用標記將訊息相互關聯。 這些可以衍生自工作階段的功能視相互關聯的本質而定。  
  
- 沒有與 WCF 會話關聯的常規資料存儲。  
  
 如果您熟悉ASP.NET應用程式中的<xref:System.Web.SessionState.HttpSessionState?displayProperty=nameWithType>類及其提供的功能，您可能會注意到此類會話和 WCF 會話之間的以下差異：  
  
- ASP.NET會話始終由伺服器啟動。  
  
- ASP.NET會話隱式無序。  
  
- ASP.NET會話提供跨請求的一般資料存儲機制。  
  
 用戶端應用程式和服務應用程式會以不同的方式與工作階段互動。 用戶端應用程式會初始化工作階段，接著並接收及處理在工作階段內傳送的訊息。 服務應用程式可以將工作階段當做擴充點使用，以便加入其他行為。 其做法是直接使用 <xref:System.ServiceModel.InstanceContext> 或實作自訂的執行個體內容提供者。  
  
## <a name="instancing"></a>執行個體  
 執行個體行為 (使用 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 屬性設定) 會控制 <xref:System.ServiceModel.InstanceContext> 對傳入訊息建立回應的方法。 根據預設，每一個 <xref:System.ServiceModel.InstanceContext> 都會與一個使用者定義的服務物件產生關聯，因此 (在預設的情形下) 設定 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> 屬性也會控制使用者定義之服務物件執行個體。 <xref:System.ServiceModel.InstanceContextMode> 列舉型別 (Enumeration) 會定義執行個體模式。  
  
 以下為可用的執行個體模式：  
  
- <xref:System.ServiceModel.InstanceContextMode.PerCall>：為每個用戶端要求所建立的新 <xref:System.ServiceModel.InstanceContext> (因此為服務物件)。  
  
- <xref:System.ServiceModel.InstanceContextMode.PerSession>：為每個新用戶端工作階段所建立的新 <xref:System.ServiceModel.InstanceContext> (因此為服務物件)，並維持該工作階段的存留期 (這麼做需要支援工作階段的繫結)。  
  
- <xref:System.ServiceModel.InstanceContextMode.Single>：單一的 <xref:System.ServiceModel.InstanceContext> (因此為服務物件)，負責處理所有應用程式之存留期的用戶端要求。  
  
 下列程式碼範例示範預設的 <xref:System.ServiceModel.InstanceContextMode> 值，在服務類別上會明確地設定 <xref:System.ServiceModel.InstanceContextMode.PerSession> 。  
  
```csharp  
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]
public class CalculatorService : ICalculatorInstance
{
    ...  
}  
```  
  
 當 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 屬性控制釋放 <xref:System.ServiceModel.InstanceContext> 的頻率時， <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> 和 <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A?displayProperty=nameWithType> 屬性會控制釋放服務物件的時機。  
  
### <a name="well-known-singleton-services"></a>已知的單一服務  
 單一執行個體服務物件的其中一個變化有時可能會很有用處：您可以自行建立服務物件，並建立使用該物件的服務主機。 若要這麼做，您必須同時將 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 屬性設定為 <xref:System.ServiceModel.InstanceContextMode.Single> ，否則在開啟服務主機時會擲回例外狀況。  
  
 請使用 <xref:System.ServiceModel.ServiceHost.%23ctor%28System.Object%2CSystem.Uri%5B%5D%29?displayProperty=nameWithType> 建構函式建立此類服務。 當您想要將特定物件執行個體提供給單一服務使用時，它提供了另一種實作自訂 <xref:System.ServiceModel.Dispatcher.IInstanceContextInitializer?displayProperty=nameWithType> 的方式。 當服務實現類型難以構造時（例如，如果它不實現無參數的公共建構函式），則可以使用此重載。  
  
 請注意，當向此建構函式提供物件時，與 Windows 通信基礎 （WCF） 具現化行為相關的某些功能的工作方式不同。 例如，提供單一物件執行個體時，呼叫 <xref:System.ServiceModel.InstanceContext.ReleaseServiceInstance%2A?displayProperty=nameWithType> 將沒有任何作用。 同樣的，也會忽略任何其他執行個體的釋放機制。 <xref:System.ServiceModel.ServiceHost> 的行為就像是所有作業都已將 <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> 屬性設定為 <xref:System.ServiceModel.ReleaseInstanceMode.None?displayProperty=nameWithType> 。  
  
### <a name="sharing-instancecontext-objects"></a>共用 InstanceContext 物件  
 您也可以自行執行該關聯，以控制哪個工作階段通道或呼叫應與哪個 <xref:System.ServiceModel.InstanceContext> 物件產生關聯。  
  
## <a name="concurrency"></a>並行  
 並行是指控制在 <xref:System.ServiceModel.InstanceContext> 內同時為作用中的執行緒數目。 這是透過使用 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A?displayProperty=nameWithType> 與 <xref:System.ServiceModel.ConcurrencyMode> 列舉型別的方式予以控制。  
  
 以下為可用的三種並行模式：  
  
- <xref:System.ServiceModel.ConcurrencyMode.Single>：每一個執行個體內容同時最多可以在執行個體內容中，擁有一個處理訊息的執行緒。 其他希望使用相同執行個體內容的執行緒必須封鎖，直到原始的執行緒結束執行個體內容為止。  
  
- <xref:System.ServiceModel.ConcurrencyMode.Multiple>：每一個服務執行個體都可以同時擁有多個處理訊息的執行緒。 此服務實作必須是安全執行緒，才能使用這種並行模式。  
  
- <xref:System.ServiceModel.ConcurrencyMode.Reentrant>：每一個服務執行個體在同一時間內會處理一個訊息，但接受可重新進入 (Re-entrant) 的作業呼叫。 服務僅在通過 WCF 用戶端物件發出呼叫時才接受這些調用。  
  
> [!NOTE]
> 您應該了解，開發能夠安全地使用一個以上之執行緒的程式碼，可能會很難順利地撰寫。 在使用 <xref:System.ServiceModel.ConcurrencyMode.Multiple> 或 <xref:System.ServiceModel.ConcurrencyMode.Reentrant> 值之前，請確定已適當地設計您的服務以使用這些模式 如需詳細資訊，請參閱 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A>。  
  
 並存的使用與執行個體模式有關。 在<xref:System.ServiceModel.InstanceContextMode.PerCall>具現化中，併發性不相關，因為每條消息都由一個新的<xref:System.ServiceModel.InstanceContext>消息處理，因此，在 中不會超過一個執行緒處於活動狀態。 <xref:System.ServiceModel.InstanceContext>  
  
 下列程式碼會示範將 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> 屬性設定為 <xref:System.ServiceModel.ConcurrencyMode.Multiple>。  
  
```csharp
[ServiceBehavior(ConcurrencyMode=ConcurrencyMode.Multiple, InstanceContextMode = InstanceContextMode.Single)]
public class CalculatorService : ICalculatorConcurrency
{
    ...  
}  
```  
  
## <a name="sessions-interact-with-instancecontext-settings"></a>與 InstanceContext 設定互動的工作階段  
 工作階段和 <xref:System.ServiceModel.InstanceContext> 會根據合約內 <xref:System.ServiceModel.SessionMode> 列舉型別值的組合以及服務實作上的 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 屬性而進行互動，以控制通道與特定服務物件的關聯。  
  
 下表根據服務的 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> 屬性與 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 屬性等值的組合，說明支援工作階段或不支援工作階段之傳入通道的結果。  
  
|InstanceContextMode 值|<xref:System.ServiceModel.SessionMode.Required>|<xref:System.ServiceModel.SessionMode.Allowed>|<xref:System.ServiceModel.SessionMode.NotAllowed>|  
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|  
|PerCall|- 具有會話通道的行為：會話和<xref:System.ServiceModel.InstanceContext>每個呼叫。<br />- 具有無會話通道的行為：引發異常。|- 具有會話通道的行為：會話和<xref:System.ServiceModel.InstanceContext>每個呼叫。<br />- 具有無會話通道的行為：<xref:System.ServiceModel.InstanceContext>每個呼叫的 A。|- 具有會話通道的行為：引發異常。<br />- 具有無會話通道的行為：<xref:System.ServiceModel.InstanceContext>每個呼叫的 A。|  
|PerSession|- 具有會話通道的行為：會話和<xref:System.ServiceModel.InstanceContext>每個通道。<br />- 具有無會話通道的行為：引發異常。|- 具有會話通道的行為：會話和<xref:System.ServiceModel.InstanceContext>每個通道。<br />- 具有無會話通道的行為：<xref:System.ServiceModel.InstanceContext>每個呼叫的 A。|- 具有會話通道的行為：引發異常。<br />- 具有無會話通道的行為：<xref:System.ServiceModel.InstanceContext>每個呼叫的 A。|  
|Single|- 具有會話通道的行為：會話和所有呼叫的<xref:System.ServiceModel.InstanceContext>會話和一個。<br />- 具有無會話通道的行為：引發異常。|- 具有會話通道的行為：會話和<xref:System.ServiceModel.InstanceContext>創建或使用者指定的單例。<br />- 具有無會話通道的行為：<xref:System.ServiceModel.InstanceContext>為創建或使用者指定的單例。|- 具有會話通道的行為：引發異常。<br />- 具有無會話通道的行為：<xref:System.ServiceModel.InstanceContext>對於每個創建的單例或使用者指定的單例的 a。|  
  
## <a name="see-also"></a>另請參閱

- [使用工作階段](../../../../docs/framework/wcf/using-sessions.md)
- [HOW TO：建立需要工作階段的服務](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-that-requires-sessions.md)
- [HOW TO：控制服務執行個體](../../../../docs/framework/wcf/feature-details/how-to-control-service-instancing.md)
- [併發](../../../../docs/framework/wcf/samples/concurrency.md)
- [執行個體](../../../../docs/framework/wcf/samples/instancing.md)
- [會話](../../../../docs/framework/wcf/samples/session.md)
