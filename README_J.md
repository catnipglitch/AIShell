# AI Shell

**AI Shell** リポジトリへようこそ！AI Shellは、人工知能のパワーをコマンドラインに直接もたらすCLIツールです！さまざまなAIアシスタントからコマンドのサポートを受けられるよう設計されており、コマンドラインでの生産性を向上させる多用途ツールです。私たちはこれらの様々なAIアシスタントプロバイダーを_エージェント_と呼んでいます。エージェントを使用して、さまざまな生成AIモデルや他のAI/ML/アシスタントプロバイダーと会話形式でやり取りできます。このリポジトリには、AI Shellエンジン、エージェント、独自のエージェントを作成する方法の詳細が含まれています。

このプロジェクトは以前**Project Mercury**として知られていたかもしれません。これは、AI Shellブランドを作成する前に使用されていたコードネームです。現在、新しいブランドへの移行プロセス中です。コードネームはまだ一部の場所で使用されているかもしれませんが、製品名は現在AI Shellです。

このプロジェクトは現在非常に初期の**パブリックプレビュー**段階にあります。このツールのユーザーエクスペリエンスを実験・洗練していく中で、コードに多くの重要な変更が加えられることが予想されます。開発を続ける中で、皆様のフィードバックと忍耐に感謝します。

## AI Shellを初めて使用される方へ

AI Shellの詳細については、AI Shellドキュメントの[概要][19]ページをご確認ください。

### 使用方法

AI Shellを使用する方法には、スタンドアロンとPowerShell 7との統合された並行体験の2つのモードがあります。詳細については、以下をご参照ください：
- [PowerShellでのAI Shellの使用開始][15]
- [AI Shell（スタンドアロン）の使用開始][16]

### PowerShellでのAI Shell

![PowerShellでのAI Shellデモを示すGIF][21]

### スタンドアロン体験

![AI Shellスタンドアロンデモを示すGIF][20]

## AI Shellの入手

AI ShellはWindows、MacOSおよびLinuxでサポートされていますが、最高の体験を得るにはWindows、[PowerShell 7][11]および[Windows Terminal][14]の使用をお勧めします。詳細については、[AI Shellのインストール][13]をご参照ください。

## AI Shellのローカルビルド

AI Shellをビルドするための前提条件：

- ビルドスクリプトには[PowerShell v7.4][18]以降のバージョンが必要です
- プロジェクトのビルドには[.NET SDK 8][09]が必要です

以下がインストールと使用の手順です。

1. このリポジトリをクローンします：`git clone https://github.com/PowerShell/AIShell`
2. `import-module ./build.psm1`を実行して`build.psm1`モジュールをインポートします
3. `Start-Build`コマンドを実行します（`-AgentsToInclude`パラメータでビルドするエージェントを指定できます）
4. ビルドが完了すると、リポジトリのルートディレクトリ内の`out\debug\app`フォルダに生成された実行ファイル`aish`が配置されます。簡単にアクセスできるよう、この場所を`PATH`環境変数に追加できます。ビルド成功後、完全パスがクリップボードにコピーされます。

## AIエージェント

AI Shellは、複数のAIエージェントを作成および登録するためのフレームワークを提供します。エージェントは、異なるAIモデルやアシスタンスプロバイダーとやり取りするために使用するライブラリです。AI Shellは`openai-gpt`と`azure`の2つのエージェントを同梱してリリースされています。しかし、プロジェクトをローカルでビルドする場合、さらに追加のエージェントがサポートされています：

エージェントのREADMEファイル：

- [`openai-gpt`][08]（AI Shellに同梱）
- [`ollama`][06]
- [`interpreter`][07]
- [`azure`][17]（AI Shellに同梱）

`aish`を実行すると、エージェントを選択するよう求められます。各エージェントの詳細については、各エージェントフォルダ内のREADMEをご参照ください。

独自のエージェントを作成する方法の詳細については、[エージェントの作成][03]をご参照ください。

`openai-gpt`エージェントを使用するには、有効なAzure OpenAIサービスまたはパブリックOpenAIキーが必要です。Azure OpenAIサービスの取得方法の詳細については、[Azure OpenAIサービスのデプロイ](./docs/development/AzureOAIDeployment/DeployingAzureOAI.md)をご参照ください。

### チャットコマンド

デフォルトでは、`aish`はAIモデルからの応答とやり取りするための基本的なチャット`/`コマンドセットを提供します。コマンドのリストを取得するには、チャットセッションで`/help`コマンドを使用します。

```
  Name       Description                                      Source
──────────────────────────────────────────────────────────────────────
  /agent     エージェント管理のためのコマンド。                   Core
  /cls       画面をクリアします。                               Core
  /code      生成されたコードとやり取りするためのコマンド。        Core
  /dislike   最後の応答を「よくない」として評価しフィードバックを送信。Core
  /exit      対話型セッションを終了します。                      Core
  /help      利用可能なすべてのコマンドを表示します。            Core
  /like      最後の応答を「良い」として評価しフィードバックを送信。Core
  /refresh   チャットセッションを更新します。                    Core
  /render    診断目的でマークダウンファイルをレンダリングします。 Core
  /retry     最後のクエリに対して新しい応答を再生成します。     Core
```

また、エージェントは独自のコマンドを実装できます。例えば、`openai-gpt`エージェントはエージェント用に定義されたGPTを管理するための`/gpt`コマンドを登録しています。`/like`や`/dislike`などの一部のコマンドは、エージェントにフィードバックを送信するコマンドです。フィードバックの消費はエージェント次第です。

### コマンドのキーバインディング

AI Shellは`/code`コマンドのキーバインディングをサポートしています。現在はハードコードされていますが、今後のリリースではカスタムキーバインディングがサポートされる予定です。

| キーバインディング        | コマンド         | 機能                                                                        |
| ------------------------- | ---------------- | --------------------------------------------------------------------------- |
| <kbd>Ctrl+d, Ctrl+c</kbd> | `/code copy`     | 生成されたコードスニペットを_すべて_クリップボードにコピーします            |
| <kbd>Ctrl+\<n\></kbd>     | `/code copy <n>` | _n番目_の生成されたコードスニペットをクリップボードにコピーします           |
| <kbd>Ctrl+d, Ctrl+d</kbd> | `/code post`     | 生成されたコードスニペットを_すべて_接続されたアプリケーションに送信します  |
| <kbd>Ctrl+d, \<n\></kbd>  | `/code post <n>` | _n番目_の生成されたコードスニペットを接続されたアプリケーションに送信します |

### 設定

現在、AI Shellは非常に基本的な設定をサポートしています。`~/.aish`下に`config.json`という名前のファイルを作成してAI Shellを設定できますが、起動時に使用するデフォルトエージェントを宣言するのみをサポートしています。これにより、`aish.exe`を実行するたびにエージェントを選択する必要がなくなります。

AI Shellの設定は、カスタムキーバインディング、カラーテーマなどをサポートするよう、今後のリリースで改善される予定です。

```json
{
  "DefaultAgent": "openai-gpt"
}
```

## プロジェクトへの貢献

詳細については[CONTRIBUTING.md][02]をご参照ください。

## プライバシー

AI Shellは個人データや個人を特定できる情報（PII）をキャプチャ、収集、保存、または処理することはありません。すべてのデータのやり取りはツールが提供する機能の範囲に限定され、個人データの収集は一切行いません。

AI Shellと統合されている一部のエージェントは、パフォーマンスの向上、ユーザーエクスペリエンスの向上、または問題のトラブルシューティングのためにテレメトリデータを収集する場合があります。各エージェントのテレメトリ実践とデータ収集ポリシーの詳細については、個々のエージェントのREADMEまたはドキュメントを参照することをお勧めします。

詳細については、[Microsoftのプライバシーステートメント](https://www.microsoft.com/en-us/privacy/privacystatement?msockid=1fe60b30e66967f13fb91f29e73f661a)をご覧ください。

## サポート

サポートについては、[サポート][05]声明をご参照ください。

## 行動規範

このプロジェクトに参加する前に、[行動規範][01]をご確認ください。

## セキュリティポリシー

セキュリティ問題については、[セキュリティポリシー][12]をご参照ください。

## フィードバック

まだ開発中であり、皆様のフィードバックを大切にしています！バグ、提案、またはフィードバックについては、このリポジトリに[問題][10]を提出してください。より率直なフィードバックを提供し、リリース前の将来のバージョンや機能のテストに登録したい場合は、この[フォーム][22]にご記入ください。

<!-- link references -->
[01]: ./docs/CODE_OF_CONDUCT.md
[02]: ./docs/CONTRIBUTING.md
[03]: ./docs/development/CreatingAnAgent.md
[05]: ./docs/SUPPORT.md
[06]: ./shell/agents/AIShell.Ollama.Agent/README.md
[07]: ./shell/agents/AIShell.Interpreter.Agent/README.md
[08]: https://learn.microsoft.com/powershell/utility-modules/aishell/how-to/agent-openai
[09]: https://dotnet.microsoft.com/en-us/download
[10]: https://github.com/PowerShell/AIShell/issues
[11]: https://learn.microsoft.com/powershell/scripting/install/installing-powershell
[12]: ./docs/SECURITY.md
[13]: https://learn.microsoft.com/powershell/utility-modules/aishell/install-aishell
[14]: https://learn.microsoft.com/windows/terminal/
[15]: https://learn.microsoft.com/powershell/utility-modules/aishell/get-started/aishell-powershell
[16]:https://learn.microsoft.com/powershell/utility-modules/aishell/get-started/aishell-standalone
[17]: https://learn.microsoft.com/powershell/utility-modules/aishell/how-to/agent-azure
[18]: https://github.com/PowerShell/PowerShell/releases/tag/v7.4.6
[19]: https://learn.microsoft.com/powershell/utility-modules/aishell/overview
[20]: ./docs/media/DemoGIFs/standalone-startup.gif
[21]: ./docs/media/DemoGIFs/azure-agent.gif
[22]: https://aka.ms/AIShell-Feedback
[logo]: ./docs/media/AIShellIconSVG.svg
