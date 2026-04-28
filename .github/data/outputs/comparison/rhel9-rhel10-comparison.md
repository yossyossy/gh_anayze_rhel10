---
oldVersion: rhel9
newVersion: rhel10
documentType: general-comparison
createdAt: 2026-04-26T00:00:00+09:00
source: 
  - https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/10.0_release_notes/index
  - .github/data/inputs/product_lifecycle_data_2026-04-26.json
---

# 比較結果: RHEL 9 → RHEL 10

---

## 1. ライフサイクル情報

| バージョン | GA 日 | フルサポート終了 | メンテナンスサポート終了 | ELS 終了 |
|-----------|------|----------------|----------------------|---------|
| RHEL 9    | 2022-05-18 | 2027-05-31 | 2032-05-31 | 2035-05-31 |
| RHEL 10   | 2025-05-20 | 2030-05-31 | 2035-05-31 | 2038-05-31 |

> **ELS**: Extended Life Cycle Support（アドオン）  
> ライフサイクルデータ出典: `product_lifecycle_data_2026-04-26.json`（公式 JSON から取得済み）

---

## 2. 主要コンポーネントのバージョン変更

| コンポーネント | RHEL 9（9.6 時点） | RHEL 10.0 | 変更種別 |
|--------------|-------------------|-----------|---------|
| **Kernel** | 5.14.0 系 | **6.12.0** | `modified` |
| **GCC** | 11.x | **14.2** | `modified` |
| **glibc** | 2.34 | **2.39** | `modified` |
| **Python** | 3.9（BaseOS）/ 3.11（AppStream） | **3.12**（BaseOS デフォルト） | `modified` |
| **OpenSSL** | 3.0.x | **3.x（FIPS PKCS#12 対応強化）** | `modified` |
| **GnuTLS** | 3.7.x | **3.8.9** | `modified` |
| **NSS** | 3.90.x | **3.112** | `modified` |
| **OpenSSH** | 8.7 | **9.9** | `modified` |
| **systemd** | 252 | **257** | `modified` |
| **GRUB2** | 2.06 | **2.12** | `modified` |
| **binutils** | 2.40 | **2.41** | `modified` |
| **annobin** | 12.32 | **12.55** | `modified` |
| **GDB** | 10.2 | **14.2** | `modified` |
| **Valgrind** | 3.19.x | **3.23.0** | `modified` |
| **SystemTap** | 4.x / 5.0 | **5.2** | `modified` |
| **Dyninst** | 11.x | **12.3.0** | `modified` |
| **elfutils** | 0.188 | **0.192** | `modified` |
| **libabigail** | 2.x | **2.6** | `modified` |
| **PCP** | 6.2.0 | **6.3.0** | `modified` |
| **Grafana** | 9.2 | **10.2.6** | `modified` |
| **LLVM Toolset** | 16.x | **19.1.7** | `modified` |
| **Rust Toolset** | 1.79.0 | **1.84.1** | `modified` |
| **Go Toolset** | 1.21 | **1.23（→1.24.4）** | `modified` |
| **Java (デフォルト)** | OpenJDK 17 | **OpenJDK 21** | `modified` |
| **Ruby** | 3.1 | **3.3** | `modified` |
| **Node.js** | 18 / 20 | **22** | `modified` |
| **Perl** | 5.32 | **5.40** | `modified` |
| **PHP** | 8.1 / 8.2 | **8.3** | `modified` |
| **MariaDB** | 10.5 / 10.11 | **10.11** | `modified` |
| **MySQL** | 8.0 | **8.4** | `modified` |
| **PostgreSQL** | 13 / 15 | **16** | `modified` |
| **Apache HTTP Server** | 2.4.57 | **2.4.62** | `modified` |
| **nginx** | 1.22 / 1.24 | **1.26** | `modified` |
| **Git** | 2.43 | **2.45** | `modified` |
| **Subversion** | 1.14 | **1.14** | `unchanged` |
| **Varnish Cache** | 6.6 | **7.4** | `modified` |
| **Squid** | 5.5 | **6.10** | `modified` |
| **Valkey** | （存在しない）| **7.2**（Redis 後継として新規追加） | `added` |
| **Redis** | 7.0 | **削除**（Valkey へ移行） | `removed` |
| **zlib** | zlib | **zlib-ng-compat**（互換ライブラリに置き換え） | `modified` |
| **cryptsetup** | 2.6.x | **2.7** | `modified` |
| **libreswan** | 4.12 | **4.15 → 5.2** | `modified` |
| **Kea DHCP** | （存在しない）| **新規追加**（ISC DHCP 置き換え） | `added` |
| **cmake** | 3.26 | **3.30.5** | `modified` |

---

## 3. アーキテクチャ・ハードウェア変更

| 項目 | RHEL 9 | RHEL 10 | 変更種別 |
|-----|--------|---------|---------|
| **x86-64 最低要件** | x86-64-v2 | **x86-64-v3**（AVX/AVX2/FMA 必須） | `modified` |
| **GCC デフォルト最適化** | x86-64-v2 | **x86-64-v3** | `modified` |
| **IBM Power 浮動小数点** | IBM double-double | **IEEE128**（GCC デフォルト） | `modified` |
| **POWER11 サポート** | なし | **追加**（-mcpu=power11） | `added` |
| **GPT パーティションテーブル** | 一部アーキのみデフォルト | **全アーキデフォルト**（DASD 除く） | `modified` |
| **32bit マルチリブ（*.i686）** | 対応 | **削除** | `removed` |

---

## 4. RHEL 9 GA 時点での主な新機能（RHEL 8 → 9 での追加）

- カーネル 5.14 系（eBPF 強化、Landlock LSM）
- Python 3.9 をデフォルトに
- GCC 11、glibc 2.34
- OpenSSL 3.0
- nftables がデフォルトのファイアウォールバックエンドに（iptables は非推奨）
- PipeWire がデフォルト音声デーモンに（PulseAudio は非推奨）
- cgroupv2 がデフォルトに
- systemd 250 系
- LUKS2 デフォルト
- SELinux userspace 3.x
- AppStream/BaseOS リポジトリ構成の継続
- Cockpit 強化
- SSSD による Kerberos 対応強化

---

## 5. RHEL 10 GA 時点での主な新機能（RHEL 9 → 10 での追加）

### 5.1. インストーラー・イメージ作成
- VNC → **RDP**（`inst.rdp` 等）によるグラフィカルリモートインストール
- 新規ユーザーにデフォルトで管理者権限を付与
- タイムゾーン設定 UI 刷新（地図廃止）
- `cockpit-composer` → `cockpit-image-builder`
- disk images（AWS/KVM）から `/boot` パーティション分離を廃止
- `bootc-image-builder` による高度パーティショニング対応
- RHEL image builder が `[customization.installer]` で Kickstart 注入をサポート

### 5.2. セキュリティ
- OpenSSH 9.9（**ポスト量子暗号 Tech Preview**、Ed25519 キーがデフォルト、DSA 削除）
- OpenSSL：FIPS 準拠 PKCS#12 作成、`pkcs11-provider` によるハードウェアトークン対応
- `pkcs11-provider` を標準提供（`openssl-pkcs11` エンジン置き換え）
- `tpm2-openssl` パッケージ（TPM 2.0 対応）新規追加
- Sequoia PGP ツール `sq`/`sqv` 追加
- GnuTLS：証明書圧縮（RFC 8879）、RSA-OAEP、PBMAC1 対応
- NSS 3.112：DTLS 1.3、ML-DSA（ポスト量子）、MLKEM1024
- SELinux userspace 3.8（`audit2allow -C` CIL 出力モード追加）
- `selinux-policy-epel` パッケージ分離（ポリシー軽量化）
- Clevis 21（PKCS#11 対応）
- Keylime 7.12（`keylime-policy` 統合ツール）
- Libreswan 5.2（IP-TFS: RFC 9347 対応）
- Audit イベントのルールベースフィルタリング・転送
- `sudo` RHEL システムロール追加
- **DEFAULT 暗号化ポリシーで RSA 鍵交換を拒否**（前方秘匿性を強制）
- crypto-policies に `@pkcs12`、`@smime` 等のスコープ追加

### 5.3. カーネル
- Kernel **6.12.0**
- Landlock LSM 完全サポート（ファイルシステムアクセス制限の sandbox）
- 動的 EFIVARS pstore バックエンド（再起動不要で切り替え可能）
- `rh_waived` カーネルパラメータ（waived 機能の手動有効化）
- `virtio-gpu` on IBM Z サポート（Tech Preview 除外）
- `rteval` のコンテナ化

### 5.4. ネットワーク
- NFS with TLS **完全サポート**
- `dnsconfd`（DNS over TLS、Split DNS 設定デーモン）正式提供
- `systemd-resolved` 完全サポート（DNSSEC 検証を除く）
- firewalld 2.3.0（StrictForwardPorts 機能等）
- `wpa_supplicant` 2.11
- `xdp-tools` 1.5.1

### 5.5. インフラサービス
- **Kea DHCP** 正式採用（ISC DHCP 削除）
- **Valkey**（Redis 後継）正式採用
- `tuned-ppd`（power-profiles-daemon 互換ドロップイン）
- `libcpuid`（正確な CPU モデル識別）
- `dnsconfd` 正式パッケージ
- CUPS が system crypto policy に準拠
- postfix 3.8.5
- rsyslog 8.2412.0（imjournal モジュールへのルールセットバインド）

### 5.6. ソフトウェア管理
- DNF modularity **廃止**（モジュラーコンテンツなし）
- `createrepo_c` 1.0.0（デフォルト圧縮が gz → zstd）
- `dnf-pre-transaction-actions` プラグイン追加

### 5.7. ファイルシステム・ストレージ
- GPT がデフォルトパーティションテーブル（全アーキテクチャ、DASD 除く）
- `dm-vdo` モジュール（kmod-kvdo 置き換え）
- NVMe SED Opal 自動化サポート（`nvme-cli` + `cryptsetup`）
- `cryptsetup` 2.7
- NFS with TLS 完全サポート

### 5.8. デスクトップ・グラフィックス
- GNOME Classic モードでウィンドウ概要が利用可能に
- フォント刷新（Red Hat フォント、Google Noto VF ファミリー）
- **Ptyxis**（コンテナ対応ターミナル）が GNOME Terminal の後継
- **Wayland** が完全標準化（xorg-x11-server 削除、Xwayland で互換）
- PipeWire がデフォルト音声（PulseAudio daemon 削除）

### 5.9. 仮想化
- HPE Compute Scale-Up Server：最大 4096 vCPU、64 TB メモリサポート
- WSL 向け RHEL 10 イメージ提供
- `nbdkit` 1.38

### 5.10. コンテナ・クラウド
- `cgroupv2` のみサポート（cgroupv1 削除）
- **crun** がデフォルトコンテナランタイム（runc 削除）
- CNI ネットワークスタック削除（Netavark/Aardvark-DNS に移行）
- `slirp4netns` → `pasta`（rootless コンテナのデフォルト）
- cloud-init が NetworkManager をデフォルトネットワークレンダラーに
- **Unified Kernel Image（UKI）** 完全サポート

### 5.11. 開発ツール
- GCC 14.2（x86-64-v3 デフォルト、`-fhardened` オプション、C23/C++23 対応強化）
- Python 3.12（BaseOS、型パラメータ構文、f-string 改善、per-interpreter GIL）
- `zlib-ng-compat`（ハードウェアアクセラレーション対応 zlib 互換ライブラリ）
- OpenJDK 21 がデフォルト Java
- LLVM Toolset 19.1.7（C++17 完全対応、C++20/23/26 強化）
- Rust Toolset 1.84.1（`LazyCell`/`LazyLock` 追加等）
- Go 1.23→1.24.4

---

## 6. 廃止・削除された機能の全一覧

### 6.1. RHEL 10 で削除された機能

#### インストーラー・イメージ作成

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| `auth` / `authconfig` Kickstart コマンド | 非推奨 | **削除** | `authselect` |
| `inst.xdriver` boot オプション | 対応 | **削除** | Wayland（X ドライバー不要） |
| `inst.usefbx` boot オプション | 対応 | **削除** | Wayland |
| `inst.nompath` / `dmraid` / `nodmraid` | 対応 | **削除** | なし |
| `timezone --isUtc` Kickstart | 非推奨 | **削除** | `--utc` |
| `timezone --ntpservers` Kickstart | 非推奨 | **削除** | `timesource --ntp-server` |
| `timezone --nontp` Kickstart | 非推奨 | **削除** | `timesource --ntp-disable` |
| `logging --level` Kickstart | 非推奨 | **削除** | なし |
| `%anaconda` Kickstart コマンド | 非推奨 | **削除** | カーネル引数 / Anaconda 設定ファイル |
| `pwpolicy` Kickstart コマンド | 非推奨 | **削除** | なし |
| GUI からの追加リポジトリ設定 | 対応 | **削除** | Kickstart / `inst.addrepo` |
| LUKS バージョン選択 GUI | 対応 | **削除** | Kickstart（`luks2` がデフォルト） |
| `initial-setup` パッケージ | 対応 | **削除** | `gnome-initial-setup` |
| `--excludeWeakdeps` (`%packages`) | 対応 | **削除** | `--exclude-weakdeps` |
| `--instLangs` (`%packages`) | 対応 | **削除** | `--inst-langs` |
| OpenStack image type（image builder） | 対応 | **削除** | `.qcow2` image type |
| `inst.vnc` / `inst.vncpassword` / `inst.vncconnect` | 対応 | **削除** | `inst.rdp` / `inst.rdp.password` / `inst.rdp.username` |
| タイムゾーンマップ（Anaconda GUI） | 対応 | **削除** | 新 UI（オプション選択式） |
| Anaconda ビルトインヘルプ | 対応 | **削除** | オンラインドキュメント参照 |
| Anaconda 自動バグ報告システム | 対応 | **削除** | 手動でログ収集・報告 |
| Anaconda GUI スクリーンショット（グローバルホットキー） | 対応 | **削除** | なし |

#### セキュリティ

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| `scap-workbench` | 対応 | **削除** | `oscap` / `oscap-ssh` |
| `oscap-anaconda-addon` | 対応 | **削除** | RHEL image builder + OpenSCAP 統合 |
| CVE OVALv2 フィード | 対応 | **削除** | CSAF / VEX |
| DSA アルゴリズム（NSS） | 非推奨 | **削除** | RSA / ECDSA |
| SEED アルゴリズム（NSS） | 非推奨（upstream 無効） | **削除** | なし |
| `fips-mode-setup` コマンド | 対応 | **削除** | インストール時に `fips=1` カーネルパラメータ |
| `/etc/system-fips` | 対応 | **削除** | `/proc/sys/crypto/fips_enabled` |
| TLS HeartBeat 拡張 | 対応 | **削除** | なし（攻撃面削減） |
| TLS SRP 認証（gnutls） | 対応 | **削除** | なし（TLS 1.3 非対応のため） |
| `pam_ssh_agent_auth` | 対応 | **削除** | なし |
| `openssl-pkcs11` エンジン | 非推奨（RHEL 9） | **削除** | `pkcs11-provider` |
| OpenSSL ENGINE API（ヘッダー `engine.h`） | 非推奨 | **削除**（ヘッダー削除、シンボルは残存） | OpenSSL Provider API |
| DSA 鍵（OpenSSH） | 非推奨 | **削除** | Ed25519 / RSA |
| `pam-ssh-agent` subcomponent（OpenSSH） | 対応 | **削除** | なし |
| RSA PKINIT 方法（Kerberos） | 対応 | **削除** | Diffie-Hellman（デフォルト） |
| Keylime ポリシー管理スクリプト群 | 対応 | **削除** | `keylime-policy` |
| DEFAULT ポリシーでの RSA 鍵交換 TLS 暗号 | 対応 | **削除**（前方秘匿性なし） | ECDH ベース暗号 |

#### ネットワーク

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| Network team driver（`teamd` / `libteam`） | 非推奨 | **削除** | bonding |
| ATM encapsulation | 非推奨 | **削除** | なし（ADSL 向けに限定） |
| `dhcp-client` / `dhclient` | 非推奨 | **削除** | NetworkManager 内蔵 DHCP クライアント |
| `mlx4` ドライバー（Mellanox ConnectX-3） | 対応 | **削除** | 新しいドライバー対応 NIC への移行 |

#### カーネル

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| `kexec_load` システムコール | 非推奨 | **削除** | `kexec_file_load` |

#### ファイルシステム・ストレージ

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| RHEL Resilient Storage Add-On / GFS2 | 対応 | **削除** | なし（RHEL 7/8/9 のみ継続） |
| block translation table driver（btt.ko） | 対応 | **削除** | なし |
| XFS V4 on-disk format | 非推奨 | **削除** | XFS V5（RHEL 7.3 以降デフォルト） |
| `kmod-kvdo` モジュール（VDO） | 対応 | **削除** | `dm-vdo`（カーネル組み込み） |
| multipath.conf の廃止オプション群 | 非推奨 | **削除** | 新オプション |

#### 高可用性・クラスター

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| `pcsd` Web UI（スタンドアロン） | 対応 | **削除**（add-on 化） | RHEL web console add-on として使用 |
| 複数の `pcs` コマンド（旧構文） | 対応 | **削除** | 新コマンド構文（`pcs constraint config` 等） |

#### 開発ツール

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| 32bit マルチリブパッケージ（*.i686） | 対応 | **削除** | RHEL 9 以前でのサポートは継続 |

#### アイデンティティ管理

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| `pam_console` モジュール | 対応 | **削除** | `systemd-logind` |
| NIS サーバーエミュレーター（IdM） | 対応 | **削除** | なし |
| BDB（Berkeley DB）in `389-ds-base` | 対応 | **削除** | LMDB（Lightning Memory-Mapped Database） |
| `compat-hesiod` | 対応 | **削除** | なし |
| `libnsl2` | 対応 | **削除** | なし |
| `python3-netifaces` | 対応 | **削除** | `python-ifaddr` |

#### SSSD

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| SSSD `enumeration` 機能（AD / IdM） | 非推奨（RHEL 9） | **削除** | なし |
| `libsss_simpleifp` | 非推奨（RHEL 9） | **削除** | なし |
| SSSD files provider | 対応 | **削除** | SSSD proxy provider |
| `ad_allow_remote_domain_local_groups` | 非推奨（RHEL 9.6） | **削除** | なし |
| `reconnection_retries` オプション | 対応 | **削除** | なし（内部 IPC 化により不要） |

#### デスクトップ

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| TigerVNC（サーバー/クライアント） | 対応 | **削除** | `gnome-remote-desktop`（RDP サーバー）/ `gnome-connections`（VNC クライアント） |
| Totem メディアプレイヤー | 対応 | **削除** | Flathub 等サードパーティ |
| `power-profiles-daemon` | 対応 | **削除** | `tuned-ppd`（互換ドロップイン） |
| `gedit` | 対応 | **削除** | GNOME Text Editor |
| Qt5 ライブラリ | 対応 | **削除** | Qt6 |
| WebKitGTK | 対応 | **削除** | なし（RHEL 10 に代替なし） |
| Evolution（メール/カレンダー） | 対応 | **削除** | Flathub |
| Festival 音声合成 | 対応 | **削除** | Espeak NG |
| Eye of GNOME（eog） | 対応 | **削除** | Loupe |
| Cheese カメラアプリ | 対応 | **削除** | Snapshot |
| Devhelp | 対応 | **削除** | オンライン API ドキュメント |
| `gtkmm`（GTK 3 ベース） | 対応 | **削除** | gtkmm（GTK 4 ベース） |
| LibreOffice RPM パッケージ | 対応 | **削除** | Flathub / LibreOffice 公式 RPM |
| GNOME Terminal | 対応 | **削除** | **Ptyxis**（コンテナ対応ターミナル） |
| Inkscape | 対応 | **削除** | Flathub 等サードパーティ |
| GNOME Classic session（デフォルトインストール） | 対応 | **削除**（手動インストールは可能） | `dnf install gnome-classic-session` |
| GNOME Tweaks | 対応 | **削除** | GNOME Settings（機能統合済み） |

#### グラフィックス

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| PulseAudio デーモン | 非推奨（RHEL 9.0 から PipeWire がデフォルト） | **削除** | PipeWire |
| Motif（X11 DE / mwm） | 非推奨 | **削除** | GTK / Qt |
| `xorg-x11-server`（X.Org Server） | 非推奨 | **削除** | Wayland（Xwayland で X11 互換） |

#### インフラサービス

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| `sendmail` | 対応 | **削除** | `postfix` |
| `redis` | 対応 | **削除** | `valkey` |
| `dhcp`（ISC DHCP サーバー） | 対応 | **削除** | `ISC Kea` / `dhcpcd` |
| `mod_security` | 対応 | **削除** | EPEL リポジトリ |
| `spamassassin` | 対応 | **削除** | EPEL リポジトリ（libdb 依存のため） |
| `xsane` | 対応 | **削除** | なし（GTK3 への移植未完） |
| `perl(Mail::Sender)` モジュール | 対応 | **削除** | なし |

#### コンテナ

| コマンド / 機能 | RHEL 9 での状態 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|----------------|---------|
| `cgroupv1`（legacy / hybrid） | 非推奨 | **削除** | `cgroupv2` |
| `runc` コンテナランタイム | 対応 | **削除** | `crun` |
| `containernetworking-plugins`（CNI） | 対応 | **削除** | Netavark / Aardvark-DNS |

---

### 6.2. RHEL 10 で非推奨になった機能（将来削除予定）

| コマンド / 機能 | RHEL 10 での状態 | 代替手段 |
|---------------|----------------|---------|
| `cockpit-composer` | 非推奨 | `cockpit-image-builder` |
| `squashfs` / `squashfs` パッケージ | 非推奨 | EROFS |
| `module` Kickstart コマンド | 非推奨 | なし（DNF modularity 廃止） |
| `inst.gpt` boot オプション | 非推奨 | `inst.disklabel=gpt` |
| ENGINE API in OpenSSL | 非推奨（ヘッダー削除済み） | OpenSSL Provider API |
| HMAC-SHA-1（FIPS モード内） | 非推奨 | HMAC-SHA-256 等 |
| `ipset` | 非推奨（メンテナンスなし） | `nftables` sets |
| `utmp` / `utmpx`（glibc） | 非推奨（2106 年オーバーフロー） | 将来削除予定 |
| FTP クライアント・サーバー（`ftp`/`lftp`/`vsftpd`） | 非推奨 | `sftp`（OpenSSH）/ WebDAV |
| Web console ホストスイッチャー | 非推奨（デフォルト無効） | ログインページ / Cockpit Client Flatpak |
| `sshd` ロール変数（System Roles） | 非推奨 | `sshd_config` 変数 |
| `libslirp` ネットワークバックエンド | 非推奨 | `pasta` |
| `i440fx` VM マシンタイプ | 非推奨 | `q35` |
| レガシー vCPU モデル（Nehalem / Ivy Bridge / AMD Opteron G4/G5） | 非推奨 | 最新 CPU モデル |
| `virt-manager` | 非推奨 | Cockpit（RHEL web console） |
| `libvirtd`（モノリシックデーモン） | 非推奨（RHEL 9 から） | モジュラー libvirt デーモン |
| SHA1 ベース SecureBoot 署名 | 非推奨 | SHA-2 以降の署名 |
| Virtual floppy driver（`isa-fdc`） | 非推奨 | なし |
| qcow2-v2 イメージ形式 | 非推奨 | qcow2-v3 |
| `slirp4netns` ネットワークモード | 非推奨 | `pasta` |
| `nodejs-18` / `nodejs-18-minimal` コンテナイメージ | 非推奨 | `nodejs-22` / `nodejs-22-minimal` |
| SMB1 プロトコル（Samba） | 非推奨（Samba 4.11 から） | SMB2 / SMB3 |
| 非推奨パッケージ群（daxio, libpmem, libpmemblk 等） | 非推奨 | 各代替を個別確認 |

---

## 7. インプレースアップグレードパス

| 移行元 | 移行先 | サポート状態 | 対応アーキテクチャ |
|--------|--------|------------|----------------|
| RHEL 9.6 | RHEL 10.0 | **サポート済み** | x86-64-v3、ARM64、POWER9、IBM Z（z14） |
| RHEL 8.x | RHEL 10.0 直接 | **非サポート** | — |
| RHEL 8.x → RHEL 9 → RHEL 10 | 2 段階アップグレード | **サポート済み** | — |

> **注意**: RHEL 9.6 → RHEL 10.0 のインプレースアップグレード前に、ネットワークチーム設定をボンディングに移行する必要がある。

---
