# 概要
構築メモとして様々な検証結果を上げて行きます。

# 構成情報
## VPS 一台
ConoHaを利用 `メモリ 8GB/CPU 6Core CentOS LTS`プランを利用

# [devstack](https://docs.openstack.org/devstack/latest/)の利用した構築
## ユーザーの追加及び変更
```
useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | tee /etc/sudoers.d/stack
su - stack
```

## [devstack](https://github.com/openstack/devstack/blob/master/README.rst)のインストール及び実行による[Ocata](https://www.openstack.org/software/ocata/)のインストール
### リポジトリの引き込み
```
git clone https://github.com/openstack-dev/devstack.git -b stable/ocata devstack/
cd devstack/
```
### 設定ファイルの確認(<vm-external-ip>を外部IPアドレスに設定してくれい)
```
cat >  local.conf <<EOF
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=\$ADMIN_PASSWORD
RABBIT_PASSWORD=\$ADMIN_PASSWORD
SERVICE_PASSWORD=\$ADMIN_PASSWORD
HOST_IP=<vm-external-ip>
RECLONE=yes
EOF
```
### 実行
```
FORCE=yes ./stack.sh
```



