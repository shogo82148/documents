---
Title: ミドルウェアのメトリック可視化に公式プラグイン集を使う
Date: 2014-09-26T18:10:26+09:00
URL: https://mackerel.io/ja/docs/entry/howto/mackerel-agent-plugins
EditURL: https://blog.hatena.ne.jp/mackerelio/mackerelio-docs-ja.hatenablog.mackerel.io/atom/entry/8454420450066401130
---

各種ミドルウェアに対応した公式プラグイン集を利用して簡単にミドルウェアのメトリックを収集できます。動作確認環境は、`mackerel-agent`と同一ですので、[Mackerelの概説の動作環境](https://mackerel.io/ja/docs/entry/overview#support-environments)をご参照ください。

AWS ELB・AWS RDSなど、`mackerel-agent`をインストールできないAWSクラウド環境は[AWSインテグレーション](https://mackerel.io/ja/docs/entry/integrations/aws)をご利用ください。サービスメトリックとして投稿するためには、[fluentd](http://www.fluentd.org/)をご利用ください。詳しくは「[AWSのCloudWatchのメトリックからELB関連のメトリックをサービスメトリックに投稿する](https://mackerel.io/ja/docs/entry/advanced/fluentd#example-elb)」をご参照ください。

プラグイン集の一覧は、下記READMEをご参照ください。

[https://github.com/mackerelio/mackerel-agent-plugins:embed:cite]

## 公式プラグイン集をインストールする

インストールにはお使いの環境に合わせてyumリポジトリもしくはaptリポジトリを使うことを推奨しています。リポジトリの設定方法は「[Mackerelエージェントをインストールする][]」をご参照ください。rpmファイルやdebファイルを直接取得したい場合は、[GitHub Releases][]を参照してください。

[Mackerelエージェントをインストールする]: https://mackerel.io/my/instruction-agent
[GitHub Releases]: https://github.com/mackerelio/mackerel-agent-plugins/releases

### rpmパッケージの場合

```
yum install mackerel-agent-plugins
```

### debパッケージの場合

```
apt-get install mackerel-agent-plugins
```

## 公式プラグイン集を利用する

各プラグインは`/usr/bin`にインストールされていますので、mackerel-agentの設定ファイルに利用するプラグインに合わせて以下のような設定を追加してください。設定の反映には、mackerel-agentの再起動が必要です。

Apache2用プラグインの場合
```
[plugin.metrics.apache2]
command = ["mackerel-plugin-apache2"]
```

各プラグインの利用方法の詳細は、後述のgithubにある各プラグインのREADMEを参照してください。

## プラグイン集のコード
公式のプラグイン集は、[https://github.com/mackerelio/mackerel-agent-plugins:title]で公開されています。
新しいミドルウェアに対応するプラグインのPull Requestなど、いつでもお待ちしています。

プラグインを作るためには[ホストのカスタムメトリックを投稿する](https://mackerel.io/ja/docs/entry/advanced/custom-metrics)を参照してください。
