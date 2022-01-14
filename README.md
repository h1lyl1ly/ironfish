# ironfish Linux

Update packages
```
sudo apt update && sudo apt upgrade -y
```
Install the necessary packages
```
sudo apt install wget jq bc build-essential -y
```
Install Docker
```
. <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/installers/docker.sh)
```
Create a container with a node
```
docker run -dit --name iron_fish_node --restart always --network host -v $HOME/.ironfish:/root/.ironfish ghcr.io/iron-fish/ironfish:latest
```
Think of a **name** for the node, run the command and enter the name, thus adding it to the system as a variable
```
. <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/miscellaneous/insert_variable.sh) -n iron_fish_moniker
```
Set a name for the node
```
ironfish config:set nodeName $iron_fish_moniker; \
ironfish config:set blockGraffiti $iron_fish_moniker
```
Change a piece of code so that synchronization does not stop
```
docker exec -t iron_fish_node sh -c "sed -i 's%REQUEST_BLOCKS_PER_MESSAGE.*%REQUEST_BLOCKS_PER_MESSAGE = 5%' /usr/src/app/node_modules/ironfish/src/syncer.ts"
```
Restart the node to apply all changes
```
docker restart iron_fish_node
```
Create a name for the wallet, run the command and enter the name, thereby adding it to the system as a variable
```
. <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/miscellaneous/insert_variable.sh) -n iron_fish_wallet_name
```
Create wallet
```
ironfish accounts:create $iron_fish_wallet_name
```
Set Wallet as Default Wallet
```
ironfish accounts:use $iron_fish_wallet_name
```
Export the created wallet
```
ironfish accounts:export $iron_fish_wallet_name "iron_fish_${iron_fish_wallet_name}.json"
```
Copy the wallet import file to the host
```
docker cp iron_fish_node:/usr/src/app/iron_fish_${iron_fish_wallet_name}.json $HOME/iron_fish_${iron_fish_wallet_name}.json
```
**Make a backup copy of the file to import the wallet, saving it in a safe place (the command displays the path)**
```
echo $HOME/iron_fish_${iron_fish_wallet_name}.json
```
Finish








