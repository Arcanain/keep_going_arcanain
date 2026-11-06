# keep_going_arcanain

Arcanainの各走行プロセスを起動するlaunchファイルをまとめるROS 2パッケージです。
リポジトリ名とパッケージ名は、どちらも `keep_going_arcanain` です。

## 役割

- 各launchで、独立したプロセスの実行に必要なノード一式を起動します。
- ノード本体と設定ファイルは既存の機能パッケージに置きます。
- `process_supervisor` が実行中のプロセスを終了させ、次のlaunchを起動します。
- 共通ノードの分離や常時起動化は行いません。

## 現在の状態

パッケージの雛形のみです。launchディレクトリとlaunchファイルは未作成です。
既存パッケージと `process_supervisor` は変更していません。

## 追加予定のlaunch

- `gnss_pure_pursuit.launch.py`
- `emcl_pure_pursuit.launch.py`
- `gnss_dwa.launch.py`

launchを追加する際は、起動するノードや参照するパッケージに合わせて
`package.xml` の実行時依存関係も追加します。

## ビルド

ROS 2環境を読み込んだワークスペースのルートで実行します。

```bash
colcon build --packages-select keep_going_arcanain
source install/setup.bash
```

## ライセンス

Apache License 2.0。既存の `process_supervisor` と同じライセンスを使用しています。
