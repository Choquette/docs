---

copyright:
  years: 2015, 2017
lastupdated: "2016-08-25"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

#アプリの更新
{: #updatingapps}


cf push コマンドまたは {{site.data.keyword.Bluemix}} DevOps Services を使用して、{{site.data.keyword.Bluemix_notm}} 内のアプリケーションを更新することができます。多くの場合、Node.js などの組み込みビルドパックにおいても、-c パラメーターを使用して、アプリケーションの開始にどのコマンドを使用するかを指定する必要があります。
{:shortdesc}

##カスタム・ドメインの作成と使用
{: #domain}

CF アプリおよびコンテナー・グループには、デフォルトの {{site.data.keyword.Bluemix_notm}} システム・ドメイン (mybluemix.net) の代わりに、カスタム・ドメインをアプリケーションの URL 内で使用できます。

ドメインは、{{site.data.keyword.Bluemix_notm}} で各組織に割り振られた URL 経路を指定します。カスタム・ドメインを使用するには、パブリック DNS サーバーにカスタム・ドメインを登録し、{{site.data.keyword.Bluemix_notm}} 内にカスタム・ドメインを構成し、パブリック DNS サーバー上の {{site.data.keyword.Bluemix_notm}} システム・ドメインにカスタム・ドメインをマップする必要があります。ご使用のカスタム・ドメインが {{site.data.keyword.Bluemix_notm}} システム・ドメインにマップされると、そのカスタム・ドメインへの要求は {{site.data.keyword.Bluemix_notm}} 内のアプリケーションに経路指定されます。

{{site.data.keyword.Bluemix_notm}} でのカスタム・ドメインの作成と使用は、{{site.data.keyword.Bluemix_notm}} ユーザー・インターフェース、または cf コマンド・ライン・インターフェースを使用して行います。

* {{site.data.keyword.Bluemix_notm}} ユーザー・インターフェースを使用する

  1. 自分の組織のカスタム・ドメインを作成します。

	1. **「{{site.data.keyword.avatar}}」**アイコン ![「アバター」アイコン](../icons/i-avatar-icon.svg)&gt;**「組織の管理」**&gt;**「組織の詳細の表示」**&gt;**「組織の編集」**&gt;**「ドメイン」**と進みます。

	2. **「ドメイン」**タブで**「ドメインの追加」**をクリックし、カスタム・ドメイン名を入力し、**「保存」**をクリックします。

	**注**: 例えば、`mycompany.com` を使用して、経路 `www.mycompany.com` をアプリに関連付けることができます。`example.mycompany.com` を使用して、経路 `www.example.mycompany.com` をアプリに関連付けることもできます。

  2. カスタム・ドメインを使用した経路をアプリケーションに追加します。

    1. メニューバーで、ドロップダウン・メニューから**「コンソール」**を選択し、経路を追加する先のアプリケーションの行をクリックします。**「概要」** ページが表示されます。

	2. **「アプリの表示」**メニューから**「経路とアクセスの編集」**を選択します。

	3. **「経路の追加」**をクリックし、アプリケーションに使用する経路を指定します。
	4. **「保存」**をクリックします。

* cf コマンド・ライン・インターフェースを使用する

  1. 次のコマンドを入力して、自分の組織のカスタム・ドメインを作成します。

    ```
cf create-domain <your org name> mydomain
    ```

    *organization_name*

        組織の名前。

    *mydomain*

        使用するカスタム・ドメイン名。

  2. カスタム・ドメインを使用した経路をアプリケーションに追加します。CF アプリの場合、次のコマンドを入力します。

    ```
    cf map-route myapp mydomain -n host_name
    ```
    コンテナー・グループの場合、次のコマンドを入力します。
     ```
     cf ic route map -n host_name -d mydomain mycontainergroup
     ```
    *myapp*

    	CF アプリの場合、アプリケーションの名前。

    *mydomain*

    	カスタム・ドメインの名前。

    *host_name*

        アプリケーションに使用する経路内のホスト名。

    *mycontainergroup*

        コンテナー・グループの場合、コンテナー・グループの名前。


{{site.data.keyword.Bluemix_notm}} でカスタム・ドメインを構成した後、登録された DNS サーバー上の {{site.data.keyword.Bluemix_notm}} システム・ドメインにカスタム・ドメインをマップする必要があります。

  1. DNS サーバー上のカスタム・ドメイン・ネームに 'CNAME' レコードをセットアップします。CNAME レコードのセットアップ手順は、使用している DNS プロバイダーによって異なります。例えば、GoDaddy を使用する場合、GoDaddy の[ドメインのヘルプ ![「外部リンク」アイコン](../icons/launch-glyph.svg)](https://www.godaddy.com/help/add-a-cname-record-19236){: new_window} にあるガイダンスに従ってください。
  2. アプリケーションが実行されている {{site.data.keyword.Bluemix_notm}} 地域のセキュア・エンドポイントにカスタム・ドメイン・ネームをマップします。以下の地域エンドポイントを使用して、{{site.data.keyword.Bluemix_notm}} で組織に割り振られた URL 経路を指定します。

    * US-SOUTH: `secure.us-south.bluemix.net`
    * EU-GB: `secure.eu-gb.bluemix.net`
    * AU-SYD: `secure.au-syd.bluemix.net`

ブラウザーまたはコマンド・ライン・インターフェースで、myapp アプリケーションにアクセスするための URL を次のように入力します。

```
http://host_name.mydomain
```

**注:** 孤立した経路を削除するには、以下のコマンドを使用します。

```
cf delete-route domain -n hostname -f
```

*domain* はご使用のドメインの名前で、*hostname* はご使用のアプリケーションの経路のホスト名です。**cf delete-route** コマンドの詳細を確認するには、`cf delete-route -h` と入力してください。

##Blue-Green デプロイメント
{: #blue_green}

{{site.data.keyword.Bluemix_notm}} は、継続的デリバリーを可能にし、ダウン時間イベントを最小限にする Blue-Green デプロイメント手法をサポートしています。

*Blue-Green デプロイメント* は、Blue と Green という 2 つのほぼ同じ実稼働環境からなるゼロ・ダウン時間デプロイメント手法です。この 2 つは、開発者が意図的に変更した成果物によって (通常、アプリケーションのバージョンによって) 異なります。いずれの時点でも、少なくとも 1 つの環境がアクティブです。Blue-Green デプロイメント手法を使用すると、以下のような利点があります。

* テストの最終ステージからライブ・プロダクションへ、ソフトウェアを素早く移行する。
* アプリケーションへのトラフィックを阻害することなく、アプリケーションの新規バージョンをデプロイする。
* ロールバックを迅速に行う。ご使用の環境のいずれかに問題がある場合は、他の環境に素早く切り替えることができます。

{{site.data.keyword.Bluemix_notm}} に既にデプロイ済みのアプリケーションがあり、そのアプリケーションを新規バージョンに更新する場合は、以下の 2 つのアプローチのいずれかを使用して Blue-Green デプロイメントを確実にすることができます。

###例: cf rename コマンドの使用

この例では、アプリケーションの名前は Blue です。この例では、**cf rename** コマンドを使用して、アプリケーションのトラフィックを阻害することなく *Blue* のバージョンを更新する方法を示しています。オプションで、*Blue* の更新されたバージョンが導入された時点で、現在の古いバージョンを削除することもできます。

1. *Blue* アプリを {{site.data.keyword.Bluemix_notm}} にプッシュします。

  ```
cf push Blue
  ```

  **結果:** *Blue* アプリが実行中で、URL `Blue.mybluemix.net` に応答しています。

2. **cf rename** コマンドを使用して、*Blue* アプリを *Green* に名前変更します。

  ```
cf rename Blue Green
  ```

  **cf apps** コマンドを使用して、現行スペースに含まれるアプリケーションをリストします。

  ```
  ...
  name             requested state   instances   memory   disk   urls
  Green            started           1/1         1G       1G	 Blue.mybluemix.net
  ...
  ```

  **結果:** *Green* アプリが実行中で、URL `Blue.mybluemix.net` に応答しています。

3. 必要な変更を行い、更新された *Blue* バージョンを準備します。更新されたこの *Blue* アプリを {{site.data.keyword.Bluemix_notm}} にプッシュします。

  ```
cf push Blue
  ```

  **cf apps** コマンドを使用して、現行スペースに含まれるアプリケーションをリストします。

  ```
  ...
  name             requested state   instances   memory   disk   urls
  Green            started           1/1         1G       1G	 Blue.mybluemix.net
  Blue             started           1/1         1G       1G	 Blue.mybluemix.net
  ...
  ```

  **結果:**
    * 使用しているアプリケーションの 2 つのインスタンス、*Blue* と *Green* がデプロイされています。
	* *Green* アプリが実行中で、URL `Blue.mybluemix.net` に応答しています。

4. オプション: 使用しているアプリの古いバージョン (*Green*) を削除する場合は、**cf delete** コマンドを使用します。

  ```
cf delete Green -f
  ```

  **cf route** コマンドを使用して、スペース内の経路をリストします。

  ```
  ...
  host             domain           apps
  Blue             mybluemix.net    Blue
  ...
  ```

  **結果:** *Blue* アプリが、URL `Blue.mybluemix.net` に応答しています。

###例: cf map-route コマンドの使用

この例では、*Blue* は以前デプロイされたアプリケーションで *Green* は更新後のバージョンです。この例では、**cf map-route** コマンドを使用して、アプリケーションのトラフィックを阻害することなく *Blue* のバージョンを更新する方法を示しています。オプションで、*Blue* の更新されたバージョンが導入された時点で、現在の古いバージョンを削除することもできます。

1. *Blue* アプリを {{site.data.keyword.Bluemix_notm}} にプッシュします。

  ```
cf push Blue
  ```

  **結果:** *Blue* アプリが実行中で、URL `Blue.mybluemix.net` に応答しています。

2. 必要な変更を行い、*Green* バージョンを準備します。この *Green* アプリを {{site.data.keyword.Bluemix_notm}} にプッシュします。

  ```
cf push Green
  ```

  **cf route** コマンドを使用して、現行スペースに含まれるアプリケーションをリストします。

  ```
  ...
  host             domain           apps
  Blue             mybluemix.net    Blue
  Green            mybluemix.net    Green
  ...
  ```

  **結果:**

    * 使用しているアプリの 2 つのインスタンス、*Blue* と *Green* がデプロイされています。
	* *Blue* アプリが、URL `Blue.mybluemix.net` に応答しています。そして、*Green* アプリは、URL `Green.mybluemix.net` に応答しています。

3. *Blue* アプリを *Green* アプリにマップし、`Blue.mybluemix.net` へのトラフィックがすべて、*Blue* アプリと *Green* アプリの両方に経路指定されるようにします。

  ```
cf map-route Green mybluemix.net -n Blue
  ```

  cf routes コマンドを使用して、スペース内の経路をリストします。

  ```
  ...
  host             domain           apps
  Blue             mybluemix.net    Blue, Green
  Green            mybluemix.net    Green
  ...
  ```

  **結果:**

    * これで、CF ルーターが、Blue.mybluemix.net 向けのトラフィックを、Blue アプリと Green アプリの両方に送信するようになりました。
	* CF ルーターは、Green.mybluemix.net 向けのトラフィックを、引き続き Green アプリに送信します。

4. *Green* が期待どおりに稼動していることを確認したら、*Blue* アプリから `Blue.mybluemix.net` 経路を削除します。

  ```
cf unmap-route Blue mybluemix.net -n Blue
  ```

  cf routes コマンドを使用して、スペース内の経路をリストします。

  ```
  ...
  host             domain           apps
  Blue             mybluemix.net    Green
  Green            mybluemix.net    Green
  ...
  ```

  **結果:** CF ルーターは、*Blue* アプリへのトラフィックの送信を停止します。*Green* アプリが、`Green.mybluemix.net` と `Blue.mybluemix.net` の両方の URL に応答しています。

5. *Green* アプリへの `Green.mybluemix.net` 経路を削除します。

  ```
cf unmap-route Green mybluemix.net -n Green
  ```

  **結果:** CF ルーターは、*Blue* アプリへのトラフィックの送信を停止します。*Green* アプリが、URL `Blue.mybluemix.net` に応答しています。

6. オプション: アプリケーションの古いバージョン (*Blue*) を削除したい場合は、`cf delete` コマンドを使用します。

  ```
cf delete Blue -f
  ```

  cf route コマンドを使用して、スペース内の経路をリストします。

  ```
  ...
  host             domain           apps
  Blue             mybluemix.net    Green
  ...
  ```

  **結果:** *Green* アプリが、URL `Blue.mybluemix.net` に応答しています。


# 関連リンク
{: #rellinks}

## 関連リンク
{: #general}

* [Blue-Green デプロイメント ![「外部リンク」アイコン](../icons/launch-glyph.svg)](http://martinfowler.com/bliki/BlueGreenDeployment.html){:new_window}
* [IBM {{site.data.keyword.Bluemix_notm}} DevOps Services ![「外部リンク」アイコン](../icons/launch-glyph.svg)](https://hub.jazz.net/){:new_window}
