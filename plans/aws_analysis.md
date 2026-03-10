# AWS分析（無料枠とODBC）

AWSを利用した構成案と、無料枠およびODBCの利用可能性について。

## 1. AWS無料枠での実現
以下のサービスを組み合わせることで、無料枠内（常に無料または12ヶ月無料）での構築が可能です。

- **AWS Lambda**: 100万リクエスト/月まで無料。認証ロジックとリダイレクト処理を実行。
- **Amazon API Gateway**: HTTP APIの場合、最初の100万リクエスト/月まで12ヶ月無料（その後も低コスト）。Lambdaのトリガーとして利用。
- **AWS Secrets Manager / Parameter Store**: クライアントID等の秘匿情報管理（Secrets Managerは有料だが、Parameter Storeの標準パラメータは無料）。

## 2. ODBCの利用
Lambda環境でODBCを利用するには以下の手順が必要です。

- **Lambda Layers**: `unixODBC` や特定のDB用ドライバ（MySQL, PostgreSQL等）をコンパイルし、Layerとして追加します。
- **ライブラリ**: Pythonの場合は `pyodbc` 等を使用します。共有ライブラリ（.soファイル）のパス設定（LD_LIBRARY_PATH）に注意が必要です。
- **制限**: Lambdaのディスク容量（512MB〜）やメモリ制限内でドライバが動作する必要があります。
