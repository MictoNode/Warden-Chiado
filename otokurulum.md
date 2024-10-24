<h1 align="center"> Warden Protocol Chiado Node ve Slinky otomatik kurulum.  </h1>


![image](https://github.com/molla202/Warden-Protocol/assets/91562185/32f62d0a-d2b1-4dfa-9b6a-60395461025f)



> Unlock the Potential of Intent-Based, Secure Cross-Chain Interactions



 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 * [Warden Website](https://wardenprotocol.org/)<br>
 * [Blockchain Explorer](https://explorer.corenodehq.com/Warden%20Testnet)<br>
 * [Discord](https://discord.gg/7rzkxXRK)<br>
 * [Twitter](https://twitter.com/wardenprotocol)<br>

## 💻 Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| CPU |	6|
| RAM	| 16+ GB |
| Storage	| 400 GB SSD |

### 🚧 Alttaki kodu yapıştırıp önce 1. sonra ise 2. adımı yükleyelim. Port ve moniker adını sizin yazdığınız şekilde otomatik olarak ayarlar. Sync olduktan sonra ise alttaki cüzdan işlemlerine devam ediyoruz.
```
bash <(curl -s https://raw.githubusercontent.com/hazennetworksolutions/warden-chiado/refs/heads/main/wardenmenu.sh)
```



### 🚧 Cüzdan olusturalım
```
wardend keys add cüzdan-adi
```
### 🚧 Validator Olusturma
NOT: Faucet adresi : https://faucet.chiado.wardenprotocol.org/
Not: altaki kodla pubkey öğren
```
wardend tendermint show-validator
```
Not: öğrendiğin pubkeyi aşağıda nano ile içine akataracağın yere yazıcan
```
nano /root/validator.json
```
NOT: baska bele validator olusturmalı proje kuruluysa içi dolu olabilir. önemli değil zaten bikere kullanıyoruz sil bastan ekle yok sa zaten içi boş
```
{
        "pubkey": pubyaz,
        "amount": "1000000000000000000award",
        "moniker": "myvalidator",
        "identity": "optional identity signature (ex. UPort or Keybase)",
        "website": "validator's (optional) website",
        "security": "validator's (optional) security contact email",
        "details": "validator's (optional) details",
        "commission-rate": "0.1",
        "commission-max-rate": "0.2",
        "commission-max-change-rate": "0.01",
        "min-self-delegation": "1"
}
```
Not: ctrl xy enter kaydet çık.
### Validator olusturucaz ama eşleşmesini beklemeniz gerek....
```
wardend tx staking create-validator /root/validator.json \
    --from=cüzdan-adi \
    --chain-id=chiado_10010-1 \
    --gas auto --gas-adjustment 1.6 --fees 250000000000000award \
    --node=http://localhost:11957
```
### oto validator olusturma yukardaki ile yapamadıysız deneyin
```
cd $HOME
```
# Create validator.json file
```
echo ""{\"pubkey\":{\"@type\":\"/cosmos.crypto.ed25519.PubKey\",\"key\":\"$(wardend tendermint show-validator | grep -Po '\"key\":\s*\"\K[^"]*')\"},
    \"amount\": \"1000000000000000000award\",
    \"moniker\": \"nodeismin\",
    \"identity\": \"keybasecode\",
    \"website\": \"\",
    \"security\": \"\",
    \"details\": \"details\",
    \"commission-rate\": \"0.1\",
    \"commission-max-rate\": \"0.2\",
    \"commission-max-change-rate\": \"0.01\",
    \"min-self-delegation\": \"1\"
}" > validator.json
```
# Create a validator using the JSON configuration
```
wardend tx staking create-validator validator.json \
    --from cuzdanismin \
    --chain-id chiado_10010-1 \
    --gas auto --gas-adjustment 1.6 --fees 250000000000000award \
    --node=http://localhost:11957
```


### Delege 
```
wardend tx staking delegate valoper-adresi miktar000000000000000000award \
--chain-id chiado_10010-1 \
--from "cüzdan-adi" \
--gas auto --gas-adjustment 1.6 --fees 250000000000000award \
--node=http://localhost:11957
```

### Komple Silme
```
sudo systemctl stop wardend
sudo systemctl disable wardend
sudo rm -rf /etc/systemd/system/wardend.service
sudo rm $(which wardend)
sudo rm -rf $HOME/.warden
sed -i "/WARDEN_/d" $HOME/.bash_profile
```
