**install go, if needed**
```
cd $HOME
VER="1.21.1"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```
**set vars**
```
echo "export WALLET="wallet"" >> $HOME/.bash_profile
echo "export MONIKER="test"" >> $HOME/.bash_profile
echo "export QUICKSILVER_CHAIN_ID="quicksilver-2"" >> $HOME/.bash_profile
echo "export QUICKSILVER_PORT="15"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

**download binary**
```
cd $HOME
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver
cd quicksilver
git checkout v1.6.2
make install
```
# config and init app
quicksilverd config node tcp://localhost:${QUICKSILVER_PORT}657
quicksilverd config keyring-backend os
quicksilverd config chain-id quicksilver-2
quicksilverd init "test" --chain-id quicksilver-2

# download genesis and addrbook
wget -O $HOME/.quicksilverd/config/genesis.json https://server-3.itrocket.net/mainnet/quicksilver/genesis.json
wget -O $HOME/.quicksilverd/config/addrbook.json  https://server-3.itrocket.net/mainnet/quicksilver/addrbook.json

# set seeds and peers
SEEDS="3b3c0037090a1b5ef9f7ac58ff79f33dffdd188a@quicksilver-mainnet-seed.itrocket.net:15656"
PEERS="4559f4c24037bfad4791b2a6d6d5c769a16cad53@quicksilver-mainnet-peer.itrocket.net:15656,7552fa33d654e0ebc2d3933dc823c1ac19e8e98f@65.109.125.172:21609,7b5fc2dfe1ca54840bd1ea7c332a7516d8ae772f@[2a01:4f9:6b:2e5b::14]:26656,6053a39e67c6bae83430e354f53d99e160e4964b@65.109.28.177:28656,5b63379fec9edfd0b1b475ae4d67c08bcb4abdc6@51.89.98.102:48656,7af3aeb6209f3404c2a86d4faa429de4b2d68c1e@195.3.221.249:60656,41fe8da4c67864723bf21055135954e0f6951c84@148.251.92.34:36656,a1f5e0b68f36091d5fc8f30aba914b6c191f21fa@65.108.128.201:11156,c8b01e6700d048b1aae34d76f5c56511b2a90ab1@57.128.133.24:26656,3a5d0b97feb595375c24665dcf17d793be129e8b@51.89.155.2:28656,9f5751f22f485a56cf28b55d7b5c9a196a469f91@185.144.99.20:36656,625eeb91fcc6242798f53426540825e5b37c7670@185.144.99.16:36656,ce94c5e02457accd8c0e5f3a61f381f4710b81ae@65.109.29.31:26656,83779cf55cc220c0bf9484560bcdfb93789a88e5@54.39.28.226:11156,be4ff5b09936e32d9a4f87f5a5118973160d58f2@78.47.214.204:26656,7cce841eed7c15ec560ef857e422a4dababb411f@161.97.85.201:26656,c9c72a2752cdfe798aa91dd6ddcc0f64597ad934@51.89.14.187:26656,b3daee00dd30d443253ab5d1be8515acdb9799dc@51.81.49.176:11156,76ff3e60af2eaf1633396f56d634a58a1ccc0537@135.181.210.171:26656,3485fff505789780dc5c9f52483666ae15cc5c11@207.180.231.123:11156,712ab8988efa4f681225456a1891f739fbd2da6b@51.178.76.62:26656,443ad7c991b2915b620673b10206c92e2b4040e0@173.67.177.120:26656,44aa838d26fe22ac4eaecff9e5643551d2228b79@136.243.104.103:12156,67c3cc1397d0a0f03a45d4cae6ff3380be7364f9@95.217.229.18:11656,f093d500e72f43e41691ba9b3b7b79dc841db8b0@142.132.156.99:21026,3308d9078fcca016fbd8dc8f3b19666326f41a6f@138.201.121.185:26672,568da80abacf69babac3030f5be3de340675ad20@88.208.242.3:26656,97a382115b40748ddf36bbc920ff085bbd942d19@168.119.139.86:46656,6da58393fe484687bc5f3067a891717f0e7d0760@167.235.15.79:26656,88fc9c304ecdb65b90339fc6dc644140a92746ed@88.198.49.30:26656,02144feb3901a3a15adb71824859c89a90b5a64d@46.4.79.183:26746,82b49e6cc0826642e745b7a7a621aecbf8083af7@65.109.94.225:56103,50a40c5aba326798ea9520ac0a1207e22a540a0e@95.214.55.100:26556,c5d4ce50da0d99cd7e0b48efd1be07f1348dff46@174.138.176.146:29656,4ac74efa43a964bf6f30078c81d5cbe072d53e5c@135.181.141.120:26656,b71ddbe0702383c73128f759a910a6d55ccee3b6@85.239.238.23:10656,4231d453fb95c2a5b57ff93da1d0304dd1958dd3@195.189.96.99:47656,3d95cb143ce9467df1088625a96300ec35234c3f@65.109.156.185:26656,c950a736496bec7abb89ff137e4c698fc061d9d4@193.34.212.165:36656,1829f35ba04f9ca7a6938f1cd4a056ec395ff558@57.128.20.159:25656,c51325ad19b790195da21d01fa16c148786a73de@161.97.108.208:26776"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
       -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.quicksilverd/config/config.toml


# set custom ports in app.toml
sed -i.bak -e "s%:1317%:${QUICKSILVER_PORT}317%g;
s%:8080%:${QUICKSILVER_PORT}080%g;
s%:9090%:${QUICKSILVER_PORT}090%g;
s%:9091%:${QUICKSILVER_PORT}091%g;
s%:8545%:${QUICKSILVER_PORT}545%g;
s%:8546%:${QUICKSILVER_PORT}546%g;
s%:6065%:${QUICKSILVER_PORT}065%g" $HOME/.quicksilverd/config/app.toml

# set custom ports in config.toml file
sed -i.bak -e "s%:26658%:${QUICKSILVER_PORT}658%g;
s%:26657%:${QUICKSILVER_PORT}657%g;
s%:6060%:${QUICKSILVER_PORT}060%g;
s%:26656%:${QUICKSILVER_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${QUICKSILVER_PORT}656\"%;
s%:26660%:${QUICKSILVER_PORT}660%g" $HOME/.quicksilverd/config/config.toml

# config pruning
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.quicksilverd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.quicksilverd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"50\"/" $HOME/.quicksilverd/config/app.toml

# set minimum gas price, enable prometheus and disable indexing
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.0001uqck"|g' $HOME/.quicksilverd/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.quicksilverd/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.quicksilverd/config/config.toml

# create service file
sudo tee /etc/systemd/system/quicksilverd.service > /dev/null <<EOF
[Unit]
Description=Quicksilver node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.quicksilverd
ExecStart=$(which quicksilverd) start --home $HOME/.quicksilverd
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

# reset and download snapshot
quicksilverd tendermint unsafe-reset-all --home $HOME/.quicksilverd
if curl -s --head curl https://server-3.itrocket.net/mainnet/quicksilver/quicksilver_2024-08-16_8674310_snap.tar.lz4 | head -n 1 | grep "200" > /dev/null; then
  curl https://server-3.itrocket.net/mainnet/quicksilver/quicksilver_2024-08-16_8674310_snap.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.quicksilverd
    else
  echo "no snapshot founded"
fi

# enable and start service
sudo systemctl daemon-reload
sudo systemctl enable quicksilverd
sudo systemctl restart quicksilverd && sudo journalctl -u quicksilverd -f
