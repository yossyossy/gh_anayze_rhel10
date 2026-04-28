---
oldVersion: rhel9
newVersion: rhel10
documentType: review-points
createdAt: 2026-04-26T00:00:00+09:00
---

# レビューポイント: RHEL 9 → RHEL 10 移行

移行時に特に注意が必要な確認事項を影響度順にまとめる。

---

## 影響度: HIGH（システム動作・セキュリティ・パフォーマンスに直接影響）

### [HIGH-01] x86-64-v3 への最低要件変更
- **概要**: RHEL 10 の x86-64 は x86-64-v3 以上が必須（AVX/AVX2/FMA 命令セット必須）
- **確認事項**: 移行対象サーバーのCPUが x86-64-v3 に対応しているか確認すること
  - Intel: Sandy Bridge（2011年）以降は概ね対応
  - AMD: Excavator（2015年）以降は概ね対応
  - 古い仮想マシン（Nehalem / Ivy Bridge 等の非推奨 vCPU モデル）は **起動不可になる可能性あり**
- **対応**: `grep -m1 flags /proc/cpuinfo | grep -E 'avx2|bmi|bmi2'` で確認

### [HIGH-02] ネットワークチーム（teamd）→ ボンディング移行（必須）
- **概要**: `teamd` / `libteam` が完全削除。RHEL 10 にアップグレード前に **必ずボンディングへ移行する必要がある**
- **確認事項**: `nmcli connection show` でチーム接続が存在しないか確認
- **対応**: RHEL 9 上でチーム設定をボンディングに変換してからアップグレード
- **公式ガイド**: [Migrating a network team to network bond](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_and_managing_networking/configuring-network-teaming_configuring-and-managing-networking#proc_migrating-a-network-team-configuration-to-network-bond_configuring-network-teaming)

### [HIGH-03] cgroupv1 完全削除
- **概要**: `cgroupv1` が削除され `cgroupv2` のみに。systemd も cgroupv1 サポートを obsolete 扱いに
- **確認事項**: アプリケーション・コンテナが `cgroupv2` に対応しているか
  - 古い Docker / コンテナエンジンは要確認
  - `cat /sys/fs/cgroup/cgroup.controllers` で cgroupv2 を確認
- **対応**: コンテナランタイムを最新版にアップデート

### [HIGH-04] XFS V4 on-disk format が使用不可に
- **概要**: XFS V4 形式のファイルシステムは RHEL 10 でマウント不可
- **確認事項**: `xfs_info <mountpoint> | grep ftype` で確認
  - V4 の特徴: `ftype=0`（V5 は `ftype=1`）
- **対応**: V4 XFS は事前に V5 へ変換（バックアップ・再作成が必要）

### [HIGH-05] ISC DHCP クライアント（dhclient）削除
- **概要**: `dhcp-client` パッケージ削除。`dhclient` コマンドが使用不可に
- **確認事項**: `/etc/dhcp/dhclient.conf` や NetworkManager 設定で `dhclient` を明示指定していないか
- **対応**: NetworkManager 内蔵 DHCP クライアントに移行（多くの場合自動移行）

### [HIGH-06] OpenSSL ENGINE API 廃止・engine.h 削除
- **概要**: `openssl-pkcs11` パッケージ削除、`engine.h` ヘッダー削除。ENGINE を使用するアプリが動作不可になる可能性
- **確認事項**: nginx、stunnel、wpa_supplicant の PKCS#11 / TPM 使用設定
  - **nginx**: PKCS#11 / TPM が現時点では機能しない（Known Issue）
  - **wpa_supplicant**: EAP-TLS + PKCS#11 が機能しない（Known Issue）
- **対応**: `pkcs11-provider` を使用する新しい OpenSSL Provider API への移行

### [HIGH-07] fips-mode-setup コマンド削除
- **概要**: インストール後に `fips-mode-setup --enable` を実行する運用が不可に
- **確認事項**: FIPS モード有効化の手順が変更されているか運用ドキュメントを確認
- **対応**: インストール時に `fips=1` カーネルパラメータを指定する

### [HIGH-08] DEFAULT 暗号化ポリシーでの RSA 鍵交換の拒否
- **概要**: TLS で RSA 鍵交換（PKCS#1 v1.5）を使う暗号スイートが DEFAULT ポリシーで拒否される
- **確認事項**: 接続先が ECDH 非対応の場合は接続失敗する
- **対応**: 対向システムを更新するか、一時的に `LEGACY` ポリシーを適用

### [HIGH-09] runc 削除 → crun への移行
- **概要**: デフォルトコンテナランタイムが runc → crun に変更
- **確認事項**: `podman system migrate --new-runtime=crun` の実行が必要
- **対応**: アップグレード後に上記コマンドを実行すること

### [HIGH-10] TigerVNC 削除
- **概要**: TigerVNC サーバー・クライアントが完全削除。VNC によるリモートデスクトップが不可に
- **確認事項**: TigerVNC を利用した運用・管理フロー
- **対応**: `gnome-remote-desktop`（RDP サーバー）に移行

---

## 影響度: MEDIUM（設定変更が必要、影響範囲が限定的）

### [MED-01] 32bit マルチリブパッケージ削除
- **概要**: `*.i686` パッケージへのリンクが不可に
- **確認事項**: 32bit 依存のアプリケーションが存在しないか
- **対応**: 64bit バイナリへの再コンパイルまたはサードパーティ提供の代替を検討

### [MED-02] DNF modularity 廃止
- **概要**: RHEL 10 ではモジュラーコンテンツなし。`module` Kickstart コマンド非推奨
- **確認事項**: Kickstart ファイルに `module` コマンドが含まれていないか
- **対応**: モジュール指定を削除し、通常の RPM インストールに変更

### [MED-03] Kickstart コマンドの変更
- **概要**: 複数の Kickstart オプション・コマンドが変更・削除
- **確認事項**: 既存 Kickstart ファイルのレビューが必要（特に以下）:
  - `auth` / `authconfig` → `authselect`
  - `timezone --ntpservers` → `timesource --ntp-server`
  - `%anaconda` セクション → カーネル引数
  - `--excludeWeakdeps` → `--exclude-weakdeps`
  - `inst.gpt` → `inst.disklabel=gpt`

### [MED-04] sendmail / redis / dhcp の削除
- **概要**: 主要インフラサービスパッケージが削除
  - `sendmail` → `postfix`
  - `redis` → `valkey`（API 互換）
  - `dhcp`（ISC DHCP サーバー） → `ISC Kea`
- **確認事項**: これらサービスを使用しているか確認し、移行計画を策定

### [MED-05] Python 3.9 → 3.12 への移行
- **概要**: デフォルト Python が 3.9/3.11 から 3.12 に変更
- **確認事項**:
  - Python アプリケーションの 3.12 互換性確認
  - `f-string`、型パラメータ構文等の新構文は互換性あり
  - 削除された旧 API への依存を確認
- **対応**: AppStream 経由で複数バージョンを並行インストール可能

### [MED-06] BDB → LMDB（389-ds-base）
- **概要**: Directory Server の バックエンドが BDB から LMDB に変更
- **確認事項**: IdM / LDAP サーバーのデータ移行手順を確認
- **対応**: 既存インスタンスは LMDB で再作成が必要

### [MED-07] PulseAudio デーモン削除
- **概要**: PulseAudio デーモンが削除（クライアントライブラリは残存）
- **確認事項**: `pulseaudio` デーモンを明示的に起動している設定がないか
- **対応**: PipeWire がデフォルト（RHEL 9 から既にデフォルト）

### [MED-08] xorg-x11-server 削除
- **概要**: X.Org Server が削除。Wayland + Xwayland の構成に
- **確認事項**:
  - X11 専用アプリケーションは Xwayland 経由で動作可能
  - `inst.xdriver` boot オプションは使用不可
  - リモートデスクトップは VNC → RDP に変更が必要

### [MED-09] cloud-init の NetworkManager 移行
- **確認事項**: RHEL 9.6 で `sysconfig`（`ifcfg` 形式）を使用している場合、アップグレード前に NetworkManager keyfile 形式へ変換が必要
- **対応**: `nmcli connection migrate` コマンドで変換

### [MED-10] NIS サーバーエミュレーター（IdM）削除
- **概要**: IdM の NIS 機能が削除
- **確認事項**: NIS 互換 API を利用しているシステムの有無
- **対応**: LDAP/Kerberos ベースの認証に移行

### [MED-11] pcs コマンド構文変更
- **概要**: 複数の `pcs` コマンドが新構文に変更（旧 `show` → 新 `config` 等）
- **確認事項**: クラスター管理スクリプトの更新が必要
- **対応**: スクリプト内の `pcs` コマンドを新構文に書き換え

### [MED-12] ca-certificates truststore パス変更
- **概要**: `/etc/pki/tls/certs` から `/etc/pki/ca-trust/extracted` に変更
- **確認事項**: `/etc/pki/tls/certs/ca-bundle.crt` を直接参照しているアプリがないか
- **対応**: `/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem` を使用するよう変更

### [MED-13] FTP 非推奨（将来削除）
- **概要**: `ftp`/`lftp`/`vsftpd` が非推奨
- **確認事項**: FTP を使用した運用・自動化スクリプトの有無
- **対応**: `sftp`（OpenSSH）または WebDAV への移行計画を策定

---

## 影響度: LOW（軽微な変更、またはデフォルト動作での影響が小さい）

### [LOW-01] GNOME Classic session の手動インストール化
- **概要**: デフォルトインストールから除外（手動で `dnf install gnome-classic-session` で追加可能）
- **確認事項**: GNOME Classic を使用しているユーザーへの周知

### [LOW-02] LibreOffice RPM パッケージ削除
- **概要**: RHEL パッケージとしての LibreOffice は削除。Flathub または公式 RPM を使用
- **確認事項**: LibreOffice を使用しているユーザー環境

### [LOW-03] gedit → GNOME Text Editor
- **対応**: `gedit` コマンドが使えなくなる。スクリプト等で `gedit` を指定している場合は変更要

### [LOW-04] GNOME Terminal → Ptyxis
- **概要**: デフォルトターミナルが変更
- **確認事項**: ターミナルプロファイル設定の移行

### [LOW-05] Qt5 → Qt6 移行
- **確認事項**: Qt5 に依存したアプリケーション（社内開発のものを含む）の Qt6 対応状況

### [LOW-06] root ユーザーの GECOS フィールド変更
- **概要**: `/etc/passwd` の root の説明が `root` → `Super User` に変更
- **確認事項**: `getent passwd root` の出力をパースしているスクリプト

### [LOW-07] TBB（Threading Building Blocks）バージョン互換性なし
- **概要**: TBB 2021.11.0 は過去バージョンと ABI 非互換
- **確認事項**: TBB を使用しているアプリケーションの再ビルドが必要

### [LOW-08] ポスト量子暗号（PQC）対応（Tech Preview）
- **概要**: OpenSSH 9.9、crypto-policies、NSS 等が PQC アルゴリズムを Tech Preview でサポート
- **確認事項**: PQC 対応の評価・テスト環境として活用可能

---

## アップグレード前チェックリスト

```
□ CPU が x86-64-v3 以上であることを確認
□ ネットワークチーム（teamd）設定をボンディングに移行済み
□ XFS V4 ファイルシステムが存在しないことを確認（または移行済み）
□ Kickstart ファイルの廃止オプション確認・更新
□ Python アプリケーションの 3.12 互換性確認
□ OpenSSL ENGINE API を使用しているアプリケーションの確認
□ PKCS#11 / TPM を使用するサービス（nginx、wpa_supplicant 等）の確認
□ sendmail / redis / dhcp を使用している場合の移行計画
□ TigerVNC によるリモートデスクトップの移行計画（RDP へ）
□ ca-certificates パス変更の影響確認
□ cloud-init + sysconfig（ifcfg）形式の場合、keyfile への変換
□ コンテナランタイム移行（runc → crun）: `podman system migrate --new-runtime=crun`
□ 32bit パッケージ依存の確認
□ IdM の BDB → LMDB 移行計画
□ pcs コマンドを使用したクラスター管理スクリプトの更新
```

---

## 参考 URL

| ドキュメント | URL |
|-------------|-----|
| RHEL 10.0 リリースノート | https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/10.0_release_notes/index |
| RHEL 10 導入時の考慮事項 | https://docs.redhat.com/ja/documentation/red_hat_enterprise_linux/10/html/considerations_in_adopting_rhel_10/index |
| RHEL 9 → 10 アップグレードガイド | https://docs.redhat.com/ja/documentation/red_hat_enterprise_linux/10/html/upgrading_from_rhel_9_to_rhel_10/index |
| サポートアップグレードパス | https://access.redhat.com/articles/4263361 |
| RHEL ライフサイクル | https://access.redhat.com/support/policy/updates/errata |
