<p style="font-size:14px" align="right">
<a href="https://t.me/autosultan_group" target="_blank">Join our telegram <img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
</p>
<p align="center">
  <img height="300" height="auto" src="https://user-images.githubusercontent.com/109174478/209359981-dc19b4bf-854d-4a2a-b803-2547a7fa43f2.jpg">
</p>

# TUTORIAL BONUS BLOCK NODE

## Install Node
```
wget -O bonus-block.sh https://raw.githubusercontent.com/sipalingnode/bonus-block/main/bonus-block.sh && chmod +x bonus-block.sh && ./bonus-block.sh
```
## Import Wallet (Gunakan wallet yang sama dengan yang konek di webnya)
```
bonus-blockd keys add $WALLET --recover
```
**Kemudian masukkan pharse nya**
## Save Info Wallet
```
BONUS_WALLET_ADDRESS=$(bonus-blockd keys show $WALLET -a)
BONUS_VALOPER_ADDRESS=$(bonus-blockd keys show $WALLET --bech val -a)
echo 'export BONUS_WALLET_ADDRESS='${BONUS_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export BONUS_VALOPER_ADDRESS='${BONUS_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```
## Cek Status Node
```
bonus-blockd status 2>&1 | jq .SyncInfo
```
**Jika sudah FALSE boleh lanjut step berikutnya. Kalo masih TRUE tunggu sampe status FALSE**
## Cek saldo wallet
```
bonus-blockd query bank balances $BONUS_WALLET_ADDRESS
```
## Create Validator
```
bonus-blockd tx staking create-validator \
  --amount 100000ubonus \
  --from $WALLET \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --identity "F57A71944DDA8C4B" \
  --website https://dwentz.xyz \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(bonus-blockd tendermint show-validator) \
  --moniker $NODENAME \
  --gas=auto \
  --gas-adjustment=1.2 \
  --gas-prices=0.025ubonus \
  --chain-id $BONUS_CHAIN_ID
 ```
 
# Perintah Berguna
**Cek Log Node**
```
journalctl -fu bonus-blockd -o cat
```
**Cek Node Info**
```
bonus-blockd status 2>&1 | jq .NodeInfo
```
**Cek Validator Info**
```
bonus-blockd status 2>&1 | jq .ValidatorInfo
```
**Cek Node ID**
```
bonus-blockd tendermint show-node-id
```
**Create Wallet**
```
bonus-blockd keys add $WALLET
```
**List Wallet**
```
bonus-blockd keys list
```
**Hapus Wallet**
```
bonus-blockd keys delete $WALLET
```
**Voting**
```
bonus-blockd tx gov vote 1 yes --from $WALLET --chain-id=$BONUS_CHAIN_ID --gas=auto --fees=2500000ubonus
```
**Delegate**
```
bonus-blockd tx staking delegate $BONUS_VALOPER_ADDRESS 1000000000000ubonus --from=$WALLET --chain-id=$BONUS_CHAIN_ID --gas=auto --fees=250000ubonus
```
**Withdraw Reward**
```
bonus-blockd tx distribution withdraw-all-rewards --from=$WALLET --chain-id=$BONUS_CHAIN_ID --gas=auto --fees=2500000ubonus
```
**Withdraw Reward & Komisi**
```
bonus-blockd tx distribution withdraw-rewards $BONUS_VALOPER_ADDRESS --from=$WALLET --commission --chain-id=$BONUS_CHAIN_ID --gas=auto --fees=2500000ubonus
```
## Delete Node
```
sudo systemctl stop bonus-blockd && \
sudo systemctl disable bonus-blockd && \
rm -rf /etc/systemd/system/bonus-blockd.service && \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf BonusBlock-chain && \
rm -rf bonus.sh && \
rm -rf .bonusblock && \
rm -rf $(which bonus-blockd)
```
