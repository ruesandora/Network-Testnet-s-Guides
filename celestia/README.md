# Celestia Mamaki Testneti Node Kurulum, Validötor Oluşturma Rehberi
![image](https://user-images.githubusercontent.com/101149671/170767187-84536f33-7b4c-48da-bf9c-f50cb7fb0367.png)
# Sistem Gereksinimleri
  Memory: 8 GB RAM
  CPU: Quad-Core
  Disk: 250 GB SSD Storage
  Bandwidth: 1 Gbps for Download/100 Mbps for Upload
  
# Explorer
https://celestia.explorers.guru/validators
# Gerekli Kütüphane Kurulumu
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
ver="1.17.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```


Bu komuttan Sonra şu satırı Görmeniz gerek.
go version go1.17.2 linux/amd64

# Celestia Dosyalarını çekelim
```
cd $HOME
rm -rf celestia-app
git clone https://github.com/celestiaorg/celestia-app.git
cd celestia-app/
APP_VERSION=$(curl -s \
  https://api.github.com/repos/celestiaorg/celestia-app/releases/latest \
  | jq -r ".tag_name")
git checkout tags/$APP_VERSION -b $APP_VERSION
make install
```
# Node ayarları (Budama vs)
```
cd $HOME
celestia-appd init nodename --chain-id mamaki


pruning="custom"
pruning_keep_recent="100"
pruning_interval="10"

sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.celestia-app/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \
\"$pruning_keep_recent\"/" $HOME/.celestia-app/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \
\"$pruning_interval\"/" $HOME/.celestia-app/config/app.toml
```
# Yeni Cüzdan Oluşturma

```
celestia-appd keys add walletname
```

# Eski Cüzdan Geri Yükleme
```
celestia-appd keys add walletname --recover
```



Discord üzerinden Test Tokeni Almanız Lazım. https://discord.gg/jyns9FWJSa
```
$request walletadress
```

# Kuruluma devam edelim, Genesis dosylarını çekmemiz gerek
```
wget -O $HOME/.celestia-app/config/genesis.json "https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/genesis.json"

BOOTSTRAP_PEERS=$(curl -sL https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/bootstrap-peers.txt | tr -d '\n') && echo $BOOTSTRAP_PEERS
sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$BOOTSTRAP_PEERS\"/" $HOME/.celestia-app/config/config.toml

PEERS="7145da826bbf64f06aa4ad296b850fd697a211cc@176.57.189.212:26656, f7b68a491bae4b10dbab09bb3a875781a01274a5@65.108.199.79:20356, 853a9fbb633aed7b6a8c759ba99d1a7674b706a3@38.242.216.151:26656, fbddf6bf8d172a96678cfcd04a887cb54b1fc9b7@70.34.211.176:26656, 96995456b7fe3ab6524fc896dec76d9ba79d420c@212.125.21.178:26656, 268694eaf9446b2052b1161979bf2e09f3e45e81@173.212.254.166:26656, 28aaa8865f3e9bba69f257b08d5c28091b5b3167@176.57.150.79:26656"
  
sed -i.bak -e "s/^persistent-peers *=.*/persistent-peers = \"$PEERS\"/" $HOME/.celestia-app/config/config.toml
sed -i.bak -e "s/^timeout-commit *=.*/timeout-commit = \"25s\"/" $HOME/.celestia-app/config/config.toml
sed -i.bak -e "s/^skip-timeout-commit *=.*/skip-timeout-commit = false/" $HOME/.celestia-app/config/config.toml
sed -i.bak -e "s/^mode *=.*/mode = \"validator\"/" $HOME/.celestia-app/config/config.toml
Create Celestia-App Systemd File
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-appd.service
[Unit]
Description=celestia-appd Cosmos daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia-appd start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
cat /etc/systemd/system/celestia-appd.service
```

# Node'u Başlatma
```
cd $HOME/.celestia-app
celestia-appd tendermint unsafe-reset-all --home "$HOME/.celestia-app"
sudo systemctl enable celestia-appd
sudo systemctl start celestia-appd
Check If Daemon Has Been Started Correctly
sudo systemctl status celestia-appd
You can exit the status screen by pressing ctrl+c
Check Daemon Logs in Real Time
sudo journalctl -u celestia-appd.service -f
To check If Your Node is in Sync Before Going Forward
curl -s localhost:26657/status | jq .result | jq .sync_info
"catching_up": false
```

# Validator Oluşturma 
```
celestia-appd tx staking create-validator \
    --amount=1000000utia \
    --pubkey=$(celestia-appd tendermint show-validator) \
    --moniker=MonikerAdi \
    --chain-id=mamaki \
    --commission-rate=0.1 \
    --commission-max-rate=0.2 \
    --commission-max-change-rate=0.01 \
    --min-self-delegation=1000000 \
    --from=walletname
confirm transaction before signing and broadcasting [y/N]: y
```

# Validator Delegate Stake etme
celestia-appd tx staking delegate VALIDATOR_ADDRESS 1000000utia --chain-id mamaki --fees=1utia --from walletname

# Unjail İşlemi
celestia-appd tx slashing unjail --from=walletname --chain-id mamaki
