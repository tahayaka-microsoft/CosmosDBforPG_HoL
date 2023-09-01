# Azure Database for PostgreSQL Hyperscale (Citus) の概要
Azure Database for PostgreSQLは、クラウドで可用性の高いPostgreSQLデータベースを実行、管理、および拡張するために使用するマネージドサービスです。この手順では、Azureポータルを使用してAzure Database for PostgreSQL Hyperscale (Citus) のサーバーグループを作成する方法について説明します。分散データ（ノード間でのシャーディングテーブル、サンプルデータの読み込み、複数のノードで実行されるクエリの実行）について説明します。

# Hyperscale (Citus) の概要
Azure Database for PostgreSQL Hyperscale (Citus) は、スケールアウトするために構築された安心なPostgreSQLです。複数のマシンのクラスターに、データ、およびクエリを配布（シャード）します。Hyperscale (Citus) は、（フォークではなく）拡張機能として、新しいPostgreSQLリリースをサポートしており、既存のPostgreSQLツールとの互換性を維持しながら、ユーザーが新機能の恩恵を受けられるようにします。既存のワークロードにおけるパフォーマンスとスケーラビリティの2つの主要な問題を解決します。通常のPostgreSQLと同様に、Hyperscale (Citus) もデータベースを管理します。高可用性、バックアップ、監視、アラート、その他の追加機能を提供し、これらはPaaS（サービスとしてのプラットフォーム）の一部として提供されます。

# Hyperscale (Citus) のアーキテクチャ:
クラスタには、コーディネーターと呼ばれる1つの特殊ノードが必ずあります（他のノードはワーカーと呼ばれます）。アプリケーションは、コーディネーターにクエリを送信し、コーディネーターは関連付けられたワーカーにクエリを中継し、結果を蓄積します。
各クエリについて、コーディネーターは、それを単一のワーカーにルーティングするか、必要なデータが単一あるいは複数のワーカーに存在するかに応じて、クエリをいくつかに並列化します。Hyperscale (Citus) が複数のワーカーにクエリを分散する方法に関するシナリオを次に示します。

<IMG>

# Azure Database for PostgreSQL Hyperscale (Citus) の概要
まず、提供された資格情報を使用してAzureポータルにログインする必要があります。

## Azureポータルへのサイン・イン
1. Azure Portalに既にログインしている場合は、次のページにスキップします。このウィンドウの右下にあるNextをクリックします。
2. ブラウザで https://portal.azure.com を開き、ブラウザのウインドウを最大化します。
3. [アカウントを選択する]というダイアログが表示されたら、[別のアカウントを使用する] を選択します。
4. [サインイン]ダイアログの、[メール、電話、Skype]フィールドに
userxxxxxx@cloudplatimmersionlabs.onmicrosoft.com を入力し[次へ]をクリックします。
5. [パスワード]フィールドにxxxxxxxxを入力します。
6. [サインイン]をクリックします。
7. [サインインの状態を維持しますか?]とタイトルがついた、[いいえ]と[はい]ボタンがあるポップアップが表示されるかもしれません。[いいえ]を選択します。
8. Welcome to Microsoft AzureとタイトルがついたStart TourとMaybe Laterボタンがあるポップアップが表示されるかもしれません。Maybe Laterを選択します。
