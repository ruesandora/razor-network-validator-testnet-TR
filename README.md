# razor-network-validator-testnet-TR

### Daha önce formunu doldurduğumuz ve KYC yaptığımız Razor Network testentinin validatör testi.

------------------------------------------------------------------------------------------------

# Öncelikle Skale Testnet v2 ağını metamaskımıza ekliyoruz.

| Network Name| Skale Testnet v2                                        |
|-------------|---------------------------------------------------------| 
| New RPC URL | https://staging-v2.skalenodes.com/v1/whispering-turais  |
|-------------|---------------------------------------------------------|              
| Chain ID    | 132333505628089                                         |
|-------------|---------------------------------------------------------|                 
| Symbol      | ETH                                                     |
|-------------|---------------------------------------------------------|                
|Explorer URL |https://whispering-turais.testnet-explorer.skalenodes.com|

# Razor tokenimizi metamaska ekliyoruz. Daha önce formu doldurup KYC yaptıysanız 100000 gelmesi lazım.

```
Contract address: 0xDc342De801De342EdE013C7E6e7a78Fa4f548041
```
# Faucettan token talep ediyoruz:

```
ttps://faucet.skale.network/
```

# Skale Endpoint kısmına bu adresi yapıştıyoruz:
```
https://staging-v2.skalenodes.com/v1/whispering-turais
```
# Şimdi staking kısmına geliyoruz, öncelikkle bir sunucuya ihtiyacımız var, gereksinimler: 

```
4bg RAM / 4 Core
```
# Sunucumuza giriş yapıyoruz, sunucumuzda docker kurulması gerek, eğer sunucuzda docker kuruluysa bu adımı atlayın.

```
sudo apt-get update
sudo apt install docker.io
sudo snap install docker
docker --version
sudo docker run hello-world
sudo docker images
```
# Go'nun kurulumu:

Eğer sisteminizde go 1.17 veya daha üstü bir sürüm varsa bu adımı atlayın. (`go version`) komutuyla denetleyin.

```
cd $HOME
wget -O go1.17.3.linux-amd64.tar.gz https://golang.org/dl/go1.17.3.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.17.3.linux-amd64.tar.gz && rm go1.17.3.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bashrc
echo 'export GOPATH=$HOME/go' >> $HOME/.bashrc
echo 'export GO111MODULE=on' >> $HOME/.bashrc
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bashrc && . $HOME/.bashrc
go version
```
# Razor Network'ün kurulması: 

```
docker run -d -it --name razor-go -v "$(echo $HOME)"/.razor:/root/.razor razornetwork/razor-go:v1.0.3-incentivised-testnet-phase2-patch2


docker exec -it razor-go razor setConfig --provider https://staging-v2.skalenodes.com/v1/whispering-turais --gasmultiplier 1 --buffer 20 --wait 30 --gasprice 0 --logLevel debug --gasLimit 2


docker exec -it razor-go razor import
```

Metamaskta razor test tokenlerimizin olduğu adresin private keyini buraya yapıştırıp ve şifre belirleyip import yapıyoruz.

Şimdi stake ediyoruz ve adresi kendi cüzdan adresiniz olacak şekilde düzenleyin, miktarı da düzenleyebilirsiniz. (kendi adresinizi girin)
```
docker exec -it razor-go razor addStake --address 0x4561aE6Bd8aF4E6E8668C55496cF73F882CfcbFa --value 990000 --logFile logs
```

# Delegation kabul etmek için: (adresi kendi adresiniz olarak düzenleyin)
```
docker exec -it razor-go razor setDelegation --address 0x4561aE6Bd8aF4E6E8668C55496cF73F882CfcbFa --status true --commission 1 --logFile logs
```

# Vote işlemlerinin arka planda çalışması için timuxa ihtiyacımız var. (vote komuntunda adresinizi düzenlemeyi unutmayın)
```
sudo apt install tmux
tmux new -s razor-go
docker exec -it razor-go razor vote --address 0x4561aE6Bd8aF4E6E8668C55496cF73F882CfcbFa --logFile logs
```

CTRL+b d ile oturumdan çıkıyoruz. (ctrl+b den sonra turşarı bırakıp d ye basın)

https://razorscan.io/ adrsine giderek cüzdanınızı bağlayıp unstake, delegate vs. yapabilirsiniz. 

Tam döküman: https://medium.com/razor-network/razor-network-incentivized-testnet-phase-ii-now-live-876dc231c5b9

Teşekkürler <3
