# Cosmos DB for PostgreSQL (Citus) の概要
Cosmos DB for PostgreSQL(Citus)は、クラウドで可用性の高いPostgreSQLデータベースを実行、管理、および拡張するために使用するマネージドサービスです。この手順では、Azureポータルを使用してCosmos DB for PostgreSQL(Citus)のサーバーグループを作成する方法について説明します。分散データ（ノード間でのシャーディングテーブル、サンプルデータの読み込み、複数のノードで実行されるクエリの実行）について説明します。

# Cosmos DB for PostgreSQL (Citus) の概要
Cosmos DB for PostgreSQL(Citus) は、スケールアウトするために構築された安心なPostgreSQLです。複数のマシンのクラスターに、データ、およびクエリを配布（シャード）します。Cosmos DB for PostgreSQL (Citus)は、（フォークではなく）拡張機能として、新しいPostgreSQLリリースをサポートしており、既存のPostgreSQLツールとの互換性を維持しながら、ユーザーが新機能の恩恵を受けられるようにします。既存のワークロードにおけるパフォーマンスとスケーラビリティの2つの主要な問題を解決します。通常のPostgreSQLと同様に、Cosmos DB for PostgreSQL (Citus)もデータベースを管理します。高可用性、バックアップ、監視、アラート、その他の追加機能を提供し、これらはPaaS（サービスとしてのプラットフォーム）の一部として提供されます。

# Cosmos DB for PostgreSQL (Citus)のアーキテクチャ:
クラスタには、コーディネーターと呼ばれる1つの特殊ノードが必ずあります（他のノードはワーカーと呼ばれます）。アプリケーションは、コーディネーターにクエリを送信し、コーディネーターは関連付けられたワーカーにクエリを中継し、結果を蓄積します。
各クエリについて、コーディネーターは、それを単一のワーカーにルーティングするか、必要なデータが単一あるいは複数のワーカーに存在するかに応じて、クエリをいくつかに並列化します。Cosmos DB for PostgreSQL (Citus)が複数のワーカーにクエリを分散する方法に関するシナリオを次に示します。

<IMG>

# Cosmos DB for PostgreSQL (Citus) の概要
まず、提供された資格情報を使用してAzureポータルにログインする必要があります。

## Azureポータルへのサイン・イン
1. Azure Portalに既にログインしている場合は、次のページにスキップします。このウィンドウの右下にあるNextをクリックします。
2. ブラウザで `https://portal.azure.com` を開き、ブラウザのウインドウを最大化します。
3. *[アカウントを選択する]* というダイアログが表示されたら、*[別のアカウントを使用する]* を選択します。
4. *[サインイン]* ダイアログの、*[メール、電話、Skype]* フィールドに
`userxxxxxx@cloudplatimmersionlabs.onmicrosoft.com` を入力し *[次へ]* をクリックします。
5. *[パスワード]* フィールドに`xxxxxxxx`を入力します。
6. *[サインイン]* をクリックします。
7. *[サインインの状態を維持しますか?]* とタイトルがついた、[いいえ]と[はい]ボタンがあるポップアップが表示されるかもしれません。[いいえ]を選択します。
8. Welcome to Microsoft AzureとタイトルがついたStart TourとMaybe Laterボタンがあるポップアップが表示されるかもしれません。Maybe Laterを選択します。

# Cosmos DB for PostgreSQL (Citus) を使い始める
これらの手順では、Azureポータルを使用してCosmos DB for PostgreSQL (Citus) サーバーグループを作成する方法について説明します。クラスターの規模によって作成に必要な時間は変わりますが、このハンズオンで作成するサーバーグループの規模であれば、通常約10分かかります。以下の手順では、プロセスを通じてこれがいかにシンプルで簡単であるかが分かります。

# Cosmos DB for PostgreSQL (Citus)を作成する
次の手順に従って、Azureポータルを使用してCosmos DB for PostgreSQL (Citus) サーバーグループを作成するプロセスを理解します。
1. Azureポータルの左上にある *[+リソースの作成]* をクリックします。
2. Azure Marketplaceの[リソースの作成]ページで[データベース]を選択し、[人気のAzureサービス]ページで[Cosmos DB]を選択します。
3. [PostgreSQLデプロイオプションのAzureデータベースの選択]ページで、[Hyperscale (Citus) サーバーグループ]の下の[作成]ボタンをクリックします。
4. 以下の情報を[Hyperscale (Citus)サーバーグループ]の基本タブに入力します。
-	サブスクリプション: あなたのセッションのサブスクリプションがデフォルトになっています。
-	リソースグループ: [新規作成]をクリックし、任意のリソースグループ名（例: `citushandsonlab_rg`）を入力します。
-	サーバーグループ名: 任意のサーバーグループ名（例: `citushandsonlab`）を入力します。
-	場所: [Japan East]あるいは任意のAzureリージョンを選択します。
-	コンピューティングとストレージ: [サーバーグループの構成]をクリックし、ハンズオンのトレーナーの指示に従ってください。通常は[階層]から[Basic]あるいは[Standard]を選択し、コーディネータとワーカーの設定はそのままにして[保存]をクリックします。
-	管理者ユーザー名: 現時点では`citus`という値がデフォルトで変更することは出来ません。
-	パスワード: `xxxxxxxx`を入力し、[パスワードの確認]にも同じパスワードを入力します。

> 注: Cosmos DB for PostgreSQL (Citus) デプロイを作成する場合、最大20のワーカーノードを水平方向にスケーリングできます。20以上のノードが必要な場合は、サポートチケットを作成するだけで、有効になります。コーディネーターと同様に、すべてのワーカー（コア、ストレージ）をスケールアップできますが、ストレージはスケールダウンが出来ません。RAMは、コア数とサーバーの種類（コーディネーターまたはワーカー）で決まります。
5. [確認および作成]をクリックするとサマリーが表示されます。サマリーを確認後、[作成]ボタンをクリックするとデプロイが開始されます。
> 注: [作成]ボタンをクリックするとデプロイが開始され、デプロイメントを監視するページにリダイレクトされます。

6. Azureポータルの左上にある[ホーム]をクリックします。
7. [Azureサービス]の下にある[Cosmos DB]をクリックします。
8. 作成時に入力したサーバーグループ名（例: `citushandsonlab`）をクリックします。
Cosmos DB for PostgreSQL (Citus)サーバーグループを管理するAzure Portalの概要タブが表示されます。この概要タブの右上には、サーバーグループへの接続に使用するコーディネーター名（例: `c.citushandsonlab.postgres.database.azure.com`）が表示されます。
9. 左にある[接続文字列]をクリックし、接続文字列を表示して確認します。
10. 左にある[コンピューティングとストレージ]をクリックし、サーバーグループを構成する各ノードのサイズを表示します。

# Cosmos DB for PostgreSQL (Citus) の使用を開始する
Azureポータルのクラウドシェルを使用してCosmos DB for PostgreSQL (Citus)サーバーグループに接続するには、ストレージアカウントを作成する必要があります。ストレージアカウントを使用すると、クラウドシェルに関連付けられたファイルを保存できるため、スクリプトの実行、データファイルのダウンロード、Azureリソースの管理など、さまざまなAzureポータルアクティビティで使用できます。

# クラウドシェルを作成する
1. ポータルのバナーでクラウドシェルのアイコンをクリックします。 
2. [ストレージがマウントされていません]の画面で、[詳細設定の表示]をクリックします。
3. サブスクリプションを選択します。
4. リージョンに[東日本]あるいは任意のリージョンを選択します。
5. リソースグループは既存のCosmos DB for PostgreSQL (Citus)のサーバーグループを作成したグループあるいは任意のグループを使うようにしてください。
6. ストレージアカウントには、[新規作成]を選択し、任意のストレージアカウント名（例: `citushandsonlab`）をペーストします。
7. ファイルシェアには、[新規作成]を選択し、任意の共有ファイル名（例: `citushandsonlab`）を入力してください。
8. [作成]をクリックします。

> 注: クラウドシェルを作成・開始するのに1分程度を要します。

9. 次の手順でファイアウォールを構成するには、クラウドシェルのクライアントIPアドレスが必要です。コマンドプロンプトで次のコマンドを入力し、returnキーを押してから、クラウドシェルのIPアドレスをコピーまたはメモします。

```
curl -s https://ifconfig.co
```

> 注: bashコンソールでペーストするには右クリック後にpasteを選択します。

# Hyperscale (Citus) の利用を開始する
Azure Database for PostgreSQL Hyperscale (Citus) は、サーバーレベルでファイアウォールを使用します。既定では、ファイアウォールはすべての外部アプリケーションとツールがコーディネーターノードおよび内部のデータベースに接続するのを防ぎます。特定のIPアドレス範囲からの接続を許可するルールを追加する必要があります。

# サーバーレベルのファイアウォールのルールを設定する
□1. Hyperscaleサーバーグループの[ネットワーク]を選択します。
□2. [ネットワーク接続]ペインの[パブリックアクセス]内で、クラウドシェルで確認した[IPアドレス]を[開始IPアドレス]と[終了IPアドレス]に入力します。
□3. [ファイアウォール規則名]に[CloudShell]と入力します。
□4. ペインの左上にある[保存]をクリックします。

> 注: Hyperscale (Citus) サーバーは5432/TCPポートを介して通信します。企業ネットワーク内から接続しようとしている場合、5432/TCPポートへの送信トラフィックは、ネットワークのファイアウォールで許可されない場合があります。その場合は、IT部門が5432/TCPポートへの通信を許可しない限り、Hyperscale (Citus) サーバーに接続できません。

# Azure Database for PostgreSQL Hyperscale (Citus)に接続する
Hyperscale (Citus)を作成すると、citusという名前の既定のデータベースが作成されます。データベースサーバーに接続するには、接続文字列と管理者パスワードが必要です。最初の接続には最大2分かかる場合があります。何らかの理由でシェルがタイムアウトして再起動した場合は、curl -s https://ifconfig.coコマンドをもう一度実行し、ファイアウォールが新しいIPアドレスで更新されていることを確認する必要があります。

# `psql`でデータベースに接続する
□1. クラウドシェルの右上にある最大化ボックスをクリックして全画面にします。
□2. Bashプロンプトで、Psqlユーティリティを用いてAzure Database for PostgreSQLに接続します。最初の接続には最大2分かかる場合があります。以下のコマンドをコピー＆ペーストして[enter]を押します。

```
psql "host=c.citushandsonlab.postgres.database.azure.com port=5432 dbname=citus user=citus password='xxxxxxxx' sslmode=require"
```

テーブルを作成しスケールアウトする
Psqlを使用してHyperscale (Citus) コーディネーターノードに接続すると、いくつかの基本的なタスクを完了できます。
この経験では、主に分散テーブルとそれらに慣れることに焦点を当てます。これから作業するデータモデルは単純です：GitHubのユーザーデータとイベントデータ。イベントには、フォークの作成、組織に関連するgitコミットなどが含まれます。Psql経由で接続したら、テーブルを作成してみましょう。
□3. Psqlコンソールで以下をコピー＆ペーストしてテーブルを作成します。

```
CREATE TABLE github_events(
event_id bigint,
event_type text,
event_public boolean,
repo_id bigint,
payload jsonb,
repo jsonb,
user_id bigint,
org jsonb,
created_at timestamp
);
CREATE TABLE github_users(
user_id bigint,
url text,
login text,
avatar_url text,
gravatar_id text,
display_login text
);
```

github_eventsテーブルのペイロードフィールドには、JSONBデータ型があります。JSONBはPostgreSQLのバイナリ形式のJSONデータ型です。JSONデータ型を使用すると、柔軟なスキーマを1つの列に簡単に格納できます。PostgreSQLは、この型にGINインデックス（Generalized Inverted Index、汎用転置インデックス）を作成し、その中のすべてのキーと値にインデックスを付けることができます。インデックスを使用すると、さまざまな条件でペイロードを高速かつ簡単に照会できます。データを読み込む前に、いくつかのインデックスを作成してみましょう。

4. Psqlコンソールで以下をコピー＆ペーストしてインデックスを作成します。
```
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

次に、コーディネーターノード上のPostgreSQLテーブルを指定し、Hyperscale (Citus) にワーカー全体でシャードするように伝えます。そのために、それをシャードするキーを指定する各テーブルに対してクエリを実行します。この例では、user_idのイベントテーブルとユーザーテーブルの両方をシャードします。

5. Psqlコンソールで以下をコピー＆ペーストします。
```
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```
このコマンドはテーブルごとに、ワーカーノードにシャードを作成します。各シャードは、一連のユーザーを保持する単純なpostgresqlテーブルです（user_idでシャード化したので）。また、コーディネーターノードにメタデータを作成して、分散テーブルのセットとワーカーノードのシャードの局所性を追跡します。user_idで両方のテーブルをシャードしたので、テーブルは自動的にコロケーションされます。つまり、両方のテーブルの1つのuser_idに関連するすべてのデータが同じワーカーノード上にあります。これは、コロケーションされたシャード全体で、ワーカーノード上での２つのテーブル間の結合をローカルに実行する場合に役立ちます。コロケーションの詳細は次の「マルチテナントアプリケーション」で説明します。

<IMG>

> 注: Hyperscale (Citus) サーバー内には、3種類のテーブルがあります。
>	- 分散テーブル – ワーカーノードを跨いで分散（スケールアウト）。一般的に大きなテーブルはパフォーマンスを改善するために分散したテーブルであるべきです。
>	- 参照テーブル – 全てのノードに複製されます。分散テーブルとの結合を可能にします。典型的には国や製品カテゴリのような小さなテーブルに用いられます。
>	- ローカルテーブル – コーディネーターノードに置かれるテーブルで、シャーディングのメタデータの管理テーブルが典型例です。

データをロードする準備が整いました。以下のコマンドでBashのクラウドシェルを「シェル実行」し、ファイルをダウンロードします。

6. Psqlコンソールでデータファイルをダウンロードするために以下をコピー＆ペーストとします。

```
\! curl -O https://examples.citusdata.com/users.csv
\! curl -O https://examples.citusdata.com/events.csv
```
7. Psqlコンソールでデータファイルをロードするために以下をコピー＆ペーストします。

```
\copy github_events from 'events.csv' WITH CSV
\copy github_users from 'users.csv' WITH CSV
```
重い本番ワークロードの場合、COPYコマンドが単一ノードのPostgresよりもHyperscale (Citus) で高速な理由は、COPYがファンアウトされ複数のワーカーノードで並行して実行されることによります。

# クエリの実行
ここから、実際にいくつかのクエリを実行するので、楽しい時間です。簡単なカウント(*)から始めて、読み込んだデータの量を確認します。
8. Psqlコンソールでgithub_eventsテーブルのレコードカウントを取得するために以下をコピー＆ペーストします
```
SELECT count(*) from github_events;
```
この単純なクエリは、先ほど作成されたシャードキーuser_idに基づいてコーディネーターがすべてのワーカーに対して伝播（プロパゲート）し、コーディネーターが集計したレコード数が返されました。
JSONBペイロード列には、多くのデータがありますが、イベントの種類によって異なります。PushEventイベントには、プッシュの個別のコミットの数を含むサイズが含まれています。これを使用して、1時間あたりのコミットの合計数を検索できます。

9. Psqlコンソールで時間あたりのコミット数を見るために以下をコピー＆ペーストします。

```
SELECT date_trunc('hour', created_at) AS hour,
       sum((payload->>'distinct_size')::int) AS num_commits
FROM github_events
WHERE event_type = 'PushEvent'
GROUP BY hour
ORDER BY hour;
```

> 注: 結果ビューでスタックした場合は、[q]と入力し、[Enter]を押してビューモードを終了します。

これまでのところ、クエリにはgithub_eventsテーブルだけが関係していましたが、この情報をgithub_usersテーブルと組み合わせることができます。ユーザーとイベントの両方を同じ識別子（user_id)でシャードしたので、一致するユーザーIDを持つ両方のテーブルの行は同じワーカーノードに配置され、簡単に結合できます。user_idでクエリを結合すると、Hyperscale (Citus) コーディネーターは、ワーカーノードで並行して実行するために、結合の実行をシャード、つまりワーカーノード、に指示することになります。

10. Psqlコンソールでレポジトリ数が最大のユーザを見つけるために以下をコピー＆ペーストします。

```
SELECT login, count(*)
FROM github_events ge
JOIN github_users gu
ON ge.user_id = gu.user_id
WHERE event_type = 'CreateEvent' AND
      payload @> '{"ref_type": "repository"}'
GROUP BY login
ORDER BY count(*) DESC
LIMIT 20;
```

本番ワークロードでは、次の理由により、上記のクエリはHyperscale (Citus) 上で高速に実行されます。

- シャードが小さく、インデックスも小さい。これはリソースの利用効率の向上とインデックス/キャッシュのヒット率の向上に寄与します。
- 複数のワーカーノードによる並列実行

# まとめ

このラボではデータベースクラスターを展開しシャーディングキーを設定することで、Microsoft Azure上でPostgreSQLを水平にスケールする方法を学びました。
他にも特にここで学んだこととして以下がありました。

-	Azure Database for PostgreSQL Hyperscale (Citus) を展開する方法
-	Azureクラウドシェルの作り方
-	Psqlを用いたHyperscale (Citus) への接続方法
-	スキーマの作成方法、シャーディングキーの設定方法、サーバーグループへのデータのロード方法

Azure Database for PostgreSQL Hyperscale (Citus) を使用すると、PostgreSQLデータベースクラスター（「サーバーグループ」と呼ばれる）にデータとクエリを分散できるため、サーバーグループ内のすべてのノードで、すべてのメモリ、コンピューティング、およびディスクが利用できるというパフォーマンス上の利点をアプリケーションに提供できます。
自分のサブスクリプションでHyperscale (Citus) を試してみたい場合は、以下のリンクを参照してください。

[Azure portal で Hyperscale (Citus) サーバー グループを作成する](https://docs.microsoft.com/ja-jp/azure/postgresql/hyperscale/quickstart-create-portal)

# マルチテナントアプリケーション

この経験では、Azure Database for PostgreSQL Hyperscale (Citus)上でマルチテナントアプリケーションを作成するプロセスを説明します。
サービスとしてのソフトウェア (SaaS) アプリケーションを構築する場合は、データモデルにテナントの概念が既に組み込まれている可能性があります。通常、ほとんどの情報はテナント/顧客/アカウントに関連し、データベーステーブルはこの自然な関係をキャプチャします。
SaaSアプリケーションの場合、各テナントデータは１つのデータベースインスタンスにまとめて格納され、他のテナントから分離され、非表示に保つことができます。これは３つの点で効率的です。まず、アプリケーションの改善はすべてのクライアントに適用されます。次に、テナント間でデータベースを共有する場合、ハードウェアを効率的に使用します。最後に、すべてのテナントに対して、テナントごとに異なるデータベース サーバーよりも、すべてのテナントに対して１つのデータベースを管理する方がはるかに簡単です。
従来、単一のリレーショナルデータベースインスタンスでは、大規模なマルチテナントアプリケーションに必要なデータの量にスケーリングするのが困難でした。開発者は、データが単一のデータベースノードの容量を超えた場合に、リレーショナルモデルの利点を放棄することを余儀なくされました。
Hyperscale (Citus) を使用すると、データベースが実際には水平方向にスケーラブルなマシンクラスタであるのに、ユーザーは単一のPostgreSQLデータベースに接続しているかのようにマルチテナントアプリケーションを作成できます。クライアントコードは最小限の変更のみを必要とし、完全なSQL機能を引き続き使用できます。
このガイドでは、マルチテナントアプリケーションのサンプルを取り上げ、Hyperscale (Citus) を使用してスケーラビリティを考慮してモデル化する方法について説明します。その過程で、ノイズの多い隣のテナントからテナントを分離する、より多くのデータに対応するハードウェアのスケーリング、テナント間で異なるデータの格納など、マルチテナントアプリケーションの一般的な課題について検討します。Azure Database for PostgreSQL Hyperscale (Citus)は、これらの課題を処理するために必要なすべてのツールを提供します。では作成してみましょう。

# 広告業向けのマルチテナントアプリケーションを作成する

顧客がオンライン広告のパフォーマンスを追跡できるようにするマルチテナントSaaSアプリケーションのバックエンドの例を構築します。ユーザーはある瞬間に自らの会社（自分の会社）に関連するデータをリクエストするので、マルチテナントアプリケーションが向いているのは当然のことです。
このマルチテナントSaaSアプリケーションの簡略化されたスキーマを検討することから始めましょう。アプリケーションは、広告キャンペーンを実行する複数の企業を追跡する必要があります。キャンペーンには多数の広告があり、各広告にはクリック数とインプレッションの記録が関連付けられています。
以下はスキーマの例です。
1. Psqlコンソールに以下のCREATE TABLEコマンドをコピー＆ペーストして会社（テナント）とそのキャンペーンのテーブルを作成します。

```
CREATE TABLE companies (
id bigserial PRIMARY KEY,
name text NOT NULL,
image_url text,
created_at timestamp without time zone NOT NULL,
updated_at timestamp without time zone NOT NULL
);
CREATE TABLE campaigns (
id bigserial,
company_id bigint REFERENCES companies (id),
name text NOT NULL,
cost_model text NOT NULL,
state text NOT NULL,
monthly_budget bigint,
blacklisted_site_urls text[],
created_at timestamp without time zone NOT NULL,
updated_at timestamp without time zone NOT NULL,
PRIMARY KEY (company_id, id)
);
```

2. Psqlコンソールに以下のCREATE TABLEコマンドをコピー＆ペーストして会社の広告のテーブルを作成します。

```
CREATE TABLE ads (
id bigserial,
company_id bigint,
campaign_id bigint,
name text NOT NULL,
image_url text,
target_url text,
impressions_count bigint DEFAULT 0,
clicks_count bigint DEFAULT 0, created_at timestamp without time zone NOT NULL,
updated_at timestamp without time zone NOT NULL,
PRIMARY KEY (company_id, id),
FOREIGN KEY (company_id, campaign_id)
REFERENCES campaigns (company_id, id)
);
```

3. Psqlコンソールに以下のCREATE TABLEコマンドをコピー＆ペーストして各広告のクリック数とインプレッション数の状態を追跡します。

```
CREATE TABLE clicks (
id bigserial,
company_id bigint,
ad_id bigint,
clicked_at timestamp without time zone NOT NULL,
site_url text NOT NULL,
cost_per_click_usd numeric(20,10),
user_ip inet NOT NULL,
user_data jsonb NOT NULL,
PRIMARY KEY (company_id, id),
FOREIGN KEY (company_id, ad_id)
REFERENCES ads (company_id, id)
);
CREATE TABLE impressions (
id bigserial,
company_id bigint,
ad_id bigint,
seen_at timestamp without time zone NOT NULL,
site_url text NOT NULL,
cost_per_impression_usd numeric(20,10),
user_ip inet NOT NULL,
user_data jsonb NOT NULL,
PRIMARY KEY (company_id, id),
FOREIGN KEY (company_id, ad_id)
REFERENCES ads (company_id, id)
);
```

4. Psqlコンソールに以下のCREATE TABLEコマンドをコピー＆ペーストして今作成したテーブルを確認します。

```
\dt
```

マルチテナントアプリケーションはテナントごとにのみ一意性を適用できるため、すべての主キーと外部キーに会社 ID が含まれます。この要件により、分散環境ではこれらの制約を強制的に適用する必要が生じます。

# リレーショナルデータモデルをスケールする

リレーショナル データ モデルは、アプリケーションに最適です。データの整合性を保護し、柔軟なクエリを可能にし、変化するデータに対応します。従来の唯一の問題は、リレーショナル データベースが、大規模な SaaS アプリケーションに必要なワークロードにスケーリングできないと考えられていたことです。開発者は NoSQL データベースを受け入れるか、そのサイズに到達するまでバックエンド サービスの塊を受け入れるしかありませんでした。
Hyperscale (Citus)を使用すると、データモデルを維持し、スケールすることができます。Hyperscale (Citus) はアプリケーションに単一の PostgreSQL データベースとして表示されますが、内部的には、要求を並列処理できる調整可能な数の物理サーバー (ノード) にクエリをルーティングします。
マルチテナント アプリケーションには、通常、クエリはテナントの組み合わせではなく、一度に 1 つのテナントに対して情報を要求するという、優れたプロパティを持っています。たとえば、営業担当者が CRM で見込顧客情報を検索する場合、検索結果は営業担当者の雇用主、つまり所属企業に固有であって、その他の企業の見込み案件情報およびメモは含まれません。
アプリケーション クエリは店舗や会社などの単一のテナントに制限されているため、マルチテナント アプリケーション クエリを高速化する方法の 1 つは、特定のテナントのすべてのデータを同じノードに格納することです。これにより、ノード間のネットワークオーバーヘッドが最小限に抑えられ、Hyperscale (Citus) がすべてのアプリケーションの結合、キー制約、およびトランザクションを効率的にサポートできるようになります。これにより、アプリケーションを完全に書き直したり再設計したりしなくても、複数のノードにまたがってスケーリングできます。

<IMG>

Hyperscale (Citus) では、テナントに関連するすべてのテーブルに、どのテナントがどの行を所有しているかを明確にマークする列があることを確認します。広告運用分析アプリケーションでは、テナントは会社なので、会社に関連するすべてのテーブルに company_id 列が含まれるようにする必要があります。これらのテーブルは分散テーブルと呼ばれています。たとえば:キャンペーンは企業向けであるため、キャンペーンテーブルには company_id 列が必要です。広告、クリック、インプレッションについても同じです。
したがって、この例では company_id は “distribution column” (シャーディング キーまたは分散キーとも呼ばれます) になります。つまり、company_id を使用して、ワーカー ノード間のすべてのテーブルをシャード/分散します。これにより、すべてのテーブルが結び付きます。つまり、すべてのテーブルの 1 つの会社に関連するすべてのデータが同じワーカー ノード上にあります。このようにして、Hyperscale (Citus) に対して、同じ会社の行がマークされている場合に、この列を使用して同じノードに行を読み取りおよび書き込むように伝えることができます。たとえば、上記の図では、company_id 5 のすべてのテーブルのすべての行が同じワーカー ノード上にあります。
この時点で、SQL をダウンロード・実行してスキーマを作成することにより、あなた専用のHyperscale (Citus) クラスターを目で追ってみましょう。スキーマの準備ができたら、Hyperscale (Citus) にワーカーノードにシャードを作成するように伝えることができます。

# ノード間にテーブルをシャードする

