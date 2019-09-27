### httpdの自動起動が息をしていない
`httpd`をVagrant環境のLinuxにインストール。  
いつだったか、DocumentRootになるディレクトリの所有者のせいで、httpdが立ち上がらない事象があったので、  
下記をVagrantfileに設置している。  
※共有ディレクトリをDocumentRootにしているため、所有者がvagrantになり、apacheが参照できないみたい
```
  config.vm.synced_folder "../../git/", "/var/www/html",
                            owner: 'apache',
                            mount_options: ['fmode=777', 'dmode=777']
```
でも自動起動が働いてない。なぜだ。こまった。

### 調べた
どうにもこうにも、共有ディレクトリがマウントされない状態で立ち上がることがあるらしい。迷惑。
([参考](https://qiita.com/ooba1192/items/96b7ab25d2bda1676aaa))  
で、参考サイトの通りにvagrantfileを修正。
```
config.vm.provision :shell, run: "always", :inline => <<-EOT
    sudo service httpd restart
EOT
```
マウントされたら上のコマンドを実行するようになる。  
ログの最後に上のコマンドの実行結果が出力されている↓
```
$ vagrant reload
==> default: Attempting graceful shutdown of VM...
==> default: Checking if box 'bento/centos-7' version '201907.24.0' is up to date...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /vagrant => 'VagrantRoot'
    default: /var/www/html => 'documentRoot'
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> default: flag to force provisioning. Provisioners marked to run always will still run.
==> default: Running provisioner: shell...
    default: Running: inline script
    default: Redirecting to /bin/systemctl restart httpd.service    // <-ここ
```
