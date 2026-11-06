# keep_going_arcanain

各走行プロセスに必要なノードを起動するROS 2 launch専用パッケージです。
`process_supervisor` がプロセスを終了し、次のlaunchを起動します。

## プロセスと経路

CSVは `path_smoother/path/tsukuba_path_2026/` に配置します。

| 順序 | launch | 制御・自己位置推定 | CSV |
|---|---|---|---|
| 1 | process_1_gnss_dwa.launch.py | DWA + GNSS | tsukuba_1004_gnss_01.csv |
| 2 | process_2_emcl_pure_pursuit.launch.py | Pure pursuit + EMCL2 | tsukuba_1004_emcl.csv |
| 3 | process_3_gnss_dwa.launch.py | DWA + GNSS | tsukuba_1004_gnss_02.csv |

各launchは既存の機能パッケージのノード・設定・子launchを使います。
ノード本体、CSV、地図、URDFはこのリポジトリには含めません。
プロセス2は非Lifecycle版の `pure_pursuit_planner` (main) を使用します。
EMCL2側の地図サーバーには既存のLifecycle管理を使います。

## 前提

- Ubuntu / ROS 2 Humble環境で、依存パッケージを用意してください。
- ワークスペースの配置は既存実装に合わせて `~/ros2_ws` を前提とします。
- GNSS受信機ドライバーは別途起動してください。
- 速度指令の中継・停止判定と1→2の切り替え信号を担う
  `decision_controller` も別途起動する前提です。
- `path_smoother` は `path_publisher` が `path_file` パラメータに対応する版が必要です。
- `process_supervisor` の切り替え条件・プロセス終了方式は既存のままです。
  P3を最終プロセスとし、P4への自動切り替えは行いません。
  P3のゴール停止・終了は別途確認してください。

## ビルド

ROS 2環境を読み込んだ実機側のワークスペースで実行します。

```bash
cd ~/ros2_ws
colcon build --packages-select keep_going_arcanain path_smoother process_supervisor
source install/setup.bash
```

依存する機能パッケージも、使用するブランチの内容で事前にビルドしてください。

## 各プロセスの単独起動

```bash
ros2 launch keep_going_arcanain process_1_gnss_dwa.launch.py
ros2 launch keep_going_arcanain process_2_emcl_pure_pursuit.launch.py
ros2 launch keep_going_arcanain process_3_gnss_dwa.launch.py
```

上のコマンドは各プロセスを個別に確認するためのものです。
自動切り替え時は次を起動します。

```bash
ros2 run process_supervisor process_manage_node
```

## ライセンス

Apache License 2.0

## 実行前の未解決事項

既存の `emcl2/launch/emcl2_origin.py` は `config/emcl2_params.yaml` を参照しますが、
現在のソースにはそのファイルがありません。実験用の正しいEMCL設定を確認してから実行してください。
