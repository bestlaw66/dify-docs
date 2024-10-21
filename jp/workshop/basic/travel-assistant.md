# AIエージェントの実践：個人のオンライン旅行アシスタントの構築方法

>　著者: Steven Lynn。Difyのテクニカルライター。

前回の実験[AI画像生成アプリの構築方法](build-ai-image-generation-app.md)では、エージェントの概念を初めて理解し、システムのプロンプトワードを試してみました。

今回は、エージェントのプロンプトワードをさらに深く探求し、より体系的なプロンプトワードの書き方を学んでいきます。

### 今回の学ぶポイント

* Difyを使用したエージェントの構築方法
* 構造化されたプロンプトワードの書き方
* 変数の使用方法

### 1. 準備

エージェントを新しく作成する前に、以下の手順を完了させてください：

* [Dify](https://dify.ai)に登録とログインします。ローカルに展開したい場合は、[コミュニティ版 - Docker Composeデプロイ](../../getting-started/install-self-hosted/docker-compose.md)を参照してください。
* 少なくとも1つのモデルプロバイダを設定します（Difyは200回のOpenAIメッセージクレジットを提供していますが、実験を円滑に進行させるためには、独自にLLMのAPIキーを設定することをお勧めします）。

### 2. ツールの設定

#### Google

オンライン旅行アシスタントを構築するには、ネットワークに接続された検索エンジンを参考資料として使用する必要があります。この記事では、Googleを例に説明します。

もちろん、他の検索エンジン、例えば[Bing](https://docs.dify.ai/ja-jp/guides/tools/tool-configuration/bing)や、AI駆動の[Perplexity](https://docs.dify.ai/ja-jp/guides/tools/tool-configuration/perplexity-search)を使用することも可能です。

Difyが提供するGoogleツールはSerpAPIに基づいており、事前にSerpAPIのAPIキー管理ページにアクセスしてAPIキーを申請し、`Dify - ツール`の該当箇所に貼り付ける必要があります。

具体的な手順は以下の通りです：

1. SerpAPIのAPIキーを追加：

[SearpAPI - API Key](https://serpapi.com/manage-api-key)にアクセスし、まだ登録していない場合は登録ページにリダイレクトされます。

SerpAPIは1か月に100回の無料呼び出しを提供しており、この実験を完了するのに十分です。追加クレジットが必要な場合は、残高を追加するか、他のオープンソースのソリューションを使用できます。

<figure><img src="../../../zh_CN/.gitbook/assets/image (368).png" alt=""><figcaption></figcaption></figure>

2. **Dify - ツール - Google**に移動：

`認可`に進むをクリックし、APIキーを入力して保存します。

<figure><img src="../../../zh_CN/.gitbook/assets/travel-assistant-1.png" alt=""><figcaption></figcaption></figure>

#### webscraper

この実験では、特定のウェブページからコンテンツを抽出するためのスクレイピングツールが必要ですが、Difyは組み込みのツールを提供しているため、追加の設定は不要です。

<figure><img src="../../../zh_CN/.gitbook/assets/travel-assistant-3.png" alt=""><figcaption></figcaption></figure>

#### Wikipedia

目的地の知識を正確に紹介するために、Wikipediaは非常に良い情報源です。Difyにもこのツールが組み込まれており、追加の設定は不要です。

<figure><img src="../../../zh_CN/.gitbook/assets/travel-assistant-4.png" alt=""><figcaption></figcaption></figure>

### 3. エージェントの構築

まず、空の`アプリ - エージェント`を作成を選択します。

ツールとして`Google`、`webscraper`、`Wikipedia`を追加して有効にします。

<figure><img src="../../../zh_CN/.gitbook/assets/travel-assistant-5.png" alt=""><figcaption></figcaption></figure>

### 4. **サンプル出力**

サンプル出力は必須ではありませんが、エージェントに期待する出力の形式を示す参考になります。

以下は旅行アシスタントのサンプル出力です：

```
## サンプル

### 詳細な旅行計画

**ホテルのおすすめ**
1. ケンジントンホテル（詳細はこちら：www.doylecollection.com/hotels/the-kensington-hotel）
   - 評価：4.6⭐
   - 価格：1泊約350ドル
   - 紹介：摂政時代の連棟住宅に位置し、南ケンジントン駅まで徒歩5分、ヴィクトリア＆アルバート博物館まで徒歩10分の優雅なホテルです。

2. レンブラントホテル（詳細はこちら：www.sarova-rembrandthotel.com）
   - 評価：4.3⭐
   - 価格：1泊約130ドル
   - 紹介：1911年に建てられた元ハロッズ百貨店のアパートで、現代的なホテルはヴィクトリア＆アルバート博物館の向かいにあり、南ケンジントン駅（ヒースロー空港行き直通）まで徒歩5分です。

**1日目 - 到着と落ち着き**
- **午前**：空港に到着。冒険の旅の始まりです！弊社の代表が空港でお迎えし、スムーズなチェックインをサポートいたします。
- **午後**：ホテルにチェックインし、少し休んでエネルギーを回復します。
- **夜**：周辺の宿泊施設を散策し、地元の雰囲気に浸ってください。近くの飲食店を見つけて、初めての楽しい夕食をお楽しみください。

**2日目 - 文化と自然の旅**
- **午前**：世界でもトップクラスの学府である帝国カレッジから1日をスタートしましょう。キャンパスツアーをお楽しみください。
- **午後**：自然史博物館（その魅力的な展示で知られています）またはヴィクトリア＆アルバート博物館（芸術とデザインを称える）を訪れる選択肢があります。その後、静かなハイドパークでリラックスし、蛇形湖でボートに乗ることもできます。
- **夜**：地元の料理を探索しましょう。伝統的なイギリスのパブで夕食をお召し上がりいただくことをお勧めします。

**追加サービス：**
- **コンシェルジュサービス**：滞在中に、レストランの予約、チケット購入、交通手配、特別なリクエストへの対応を行うコンシェルジュサービスをご利用いただけます。
- **24時間サポート**：旅行中に発生する可能性のある問題や要望に対応するため、24時間対応のサポートを提供しています。

楽しい旅を！豊かな経験と素晴らしい思い出でいっぱいになりますように！
```

### 問題1：ユーザー入力の規範化方法

通常、私たちがエージェントに入力する内容は自然言語ですが、自然言語の欠点の一つはその規範化が難しいことです。時には、エージェントにとって不要な情報や価値のない情報が含まれている可能性があります。この場合、変数を導入することで入力を規範化することができます。

Difyは現在、`短文`、`段落`、`ドロップダウンオプション`、`数値`、`APIベースの変数`といった種類の変数をサポートしています。

本実験では、`短文`タイプの変数を選択するだけで十分です。

**変数**の中から適切な変数タイプを選ぶことで、ユーザーに目的地、旅行日数、予算を尋ねることができます。

| 変数キー       | 変数タイプ | フィールド名 | 必須 |
| ----------- | ---- | ---- | -- |
| destination | 短文   | 目的地  | はい  |
| day         | 短文   | 旅行日数 | はい  |
| budget      | 短文   | 旅行予算 | はい  |

注意が必要なのは、`変数キー`、つまり変数の名前は、大文字小文字の英字、数字、アンダースコアのみをサポートしている点です。`フィールド名`は、ユーザーが見ることのできるヒントの内容です。

変数を追加した後、ユーザーはアプリ開発者の意図に従って必要な背景情報をアプリに提供できるようになります。その結果は以下のようになります：

<figure><img src="../../../zh_CN/.gitbook/assets/image (369).png" alt=""><figcaption></figcaption></figure>