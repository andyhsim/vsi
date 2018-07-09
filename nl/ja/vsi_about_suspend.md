---



copyright:
  years: 2017, 2018
lastupdated: "2018-06-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}

# 請求一時停止について (ベータ)
{: #requirements}

請求一時停止フィーチャーがサポートされている仮想サーバーの電源を切ると、一定のコンピュート・リソースについてはコストが発生しません。サーバーの電源が切られると請求は自動的に停止します。請求一時停止フィーチャーは、コスト削減に役立ち、仮想サーバーのリソースが再び必要になったときに仮想サーバーを再プロビジョンする必要がなくなります。請求一時停止は、新規プロビジョンでのみサポートされ、既存のインスタンスではサポートされません。
{:shortdesc}

請求一時停止フィーチャーにアクセスできるためには、{{site.data.keyword.slapi_short}} を使用して請求一時停止パッケージを指定して、新規仮想サーバー・インスタンスをプロビジョンする必要があります。その新規仮想サーバー・インスタンスは、以下の設定値で構成される必要があります。

* 毎時 SAN
* 以下のいずれかのファミリー:
  * 平衡型
  * コンピュート
  * メモリー
* 以下のいずれかの {{site.data.keyword.cloud}} データ・センター:

| データ・センター |         |
| ------------ | ------- | 
|SEO01         |  WDC01  |
|SAO01         |  WDC04  |
|TOK02         |  WDC06  |
|DAL01         |  WDC07  |
|DAL05         |  LON02  |
|DAL06         |  LON04  |
|DAL09         |  LON06  |
|DAL10         |  FRA02  |
|DAL12         |  FRA04  |
|DAL13         |  FRA05  |
{: caption="表 1. サポートされるデータ・センター" caption-side="top"}

請求一時停止フィーチャーを、仮想サーバー・インスタンスのプロビジョンおよび再プロビジョンをより素早く行うための代替方法として使用できます。
{:tip}

## プロビジョニング

このベータ機能では、請求一時停止フィーチャーがサポートされている仮想サーバー・インスタンスをプロビジョンするには、{{site.data.keyword.slapi_short}} を使用する必要があります。 API サンプルについては、[Place Order Object を使用したパブリック・インスタンスのプロビジョニング](../vsi/vsi_provision_api.html#provisioning-a-public-instance-using-place-order-object)を参照してください。 

プロビジョニング・プロセス中に、特定の請求一時停止パッケージ ID を指定する必要もあります。{{site.data.keyword.slapi_short}} で、キー名 `SUSPEND_CLOUD_SERVER` を使用して請求一時停止パッケージ ID を照会できます。サーバー・パッケージの検索例については、[Understanding and building an order using the {{site.data.keyword.slapi_short}} order CLI ![「外部リンク」アイコン](../icons/launch-glyph.svg "「外部リンク」アイコン")](https://softlayer.github.io/article/understanding-ordering/){: new_window} を参照してください。

## 請求の詳細

仮想サーバー・インスタンスの電源が切れているときに、どのコストの発生が停止するのか、どのコストが持続するのかを把握しておくことは重要です。

| リソース                      | 請求が停止する    | 請求が持続する   |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| ポート速度                    |          X        |                  |
| OS のライセンス               |          X        |                  |
| モニタリング・アドオン        |          X        |                  |
| 2 次パブリック IP アドレス    |                   |         X        |
| ストレージ                    |                   |         X        |
{: caption="表 1. リソース請求の詳細" caption-side="top"}   

**注:** 請求一時停止フィーチャーがサポートされている仮想サーバー・インスタンスをプロビジョンすると、仮想サーバー・インスタンスの使用時および使用停止時の両方で、分ごとに使用時間が計算されます。インスタンスの電源を切ることによって請求一時停止フィーチャーを開始することがまったくなくても、インスタンスのライフサイクルにわたって分ごとに請求の計算が行われます。 

### 最小使用料金
請求一時停止フィーチャーがサポートされている仮想サーバー・インスタンスには、月当たりの最小使用量料金があります。使用量が 25 % より大きい場合、その使用量に対して請求されます。使用量が 25 % より小さい場合、現行請求サイクル内でインスタンスが存在した時間の 25 % に対して課金されます。 

### 請求書
仮想サーバーの請求を一時停止した場合、請求書にいくつか変更が見られます。使用量ベースの詳細として、関連料金が示されるようになります。例えば、利用可能時間、使用された時間、および課金される合計時間数を反映する、次のような項目が追加されることがあります。

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

## リソース詳細

### ストレージ

仮想サーバー・インスタンスで請求を一時停止した場合、関連付けられたストレージは持続しますが、仮想サーバー・インスタンスの電源が切れている間はデータにアクセスできません。インスタンスに対する請求を再開すると、データにアクセスできるようになり、通常の請求が再び開始されます。

### IP アドレス

仮想サーバー・インスタンスで請求が一時停止されている間、すべてのパブリック IP アドレスは保持されます。

{{site.data.keyword.slapi_short}} を介して、または、{{site.data.keyword.slportal_full}} の「デバイス詳細」ページにアクセスすることによって、デバイスが停止しているかどうかと、状況が変化した関連日付を確認できます。

### 制限

サポートされる同時インスタンスの数は、アカウント全体のデバイス割り当て量の一部です。 同時インスタンスの制限について詳しくは、[FAQ: 仮想サーバー](vsi_faqs_vs.html#concurrent)を参照してください。

## 次のステップ
請求一時停止がサポートされる仮想サーバーをプロビジョンした後、デバイスに対する請求の一時停止および再開を行うことができます。
仮想サーバー・インスタンスで請求が一時停止されると、デバイスに対する請求を再開するまで、そのインスタンスでは同じアクションの一部を実行できません。

仮想サーバー・インスタンスに対する請求を一時停止するには、仮想サーバーの電源を切ります。詳しくは、[仮想サーバーの管理](vsi_managing.html)を参照してください。