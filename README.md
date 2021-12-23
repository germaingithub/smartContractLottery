1. Users can enter lottery with ETH based on a USD fee
2. An admin will choose when the lottery is over
3. the lottery will select a random winner

How do we want to test this?

1. `mainnet-fork`
2. `development` with mocks
3. `testnet`

Processus from deployed Contract to End of Lottery
https://www.youtube.com/watch?v=5Vwl58U3Ipc&ab_channel=GGV

//Prerequisites
Please install or have installed the following:

nodejs and npm
python

//Installation
1. Install Brownie, if you haven't already. Here is a simple way to install brownie.
python3 -m pip install --user pipx
python3 -m pipx ensurepath
# restart your terminal
pipx install eth-brownie

2. Download the mix and install dependancies.
brownie bake chainlink-mix
cd chainlink-mix
pip install -r requirements.txt

This will open up a new Chainlink project. Or, you can clone from source:

git clone https://github.com/PatrickAlphaC/chainlink-mix
cd chainlink-mix 

Testnet Development
If you want to be able to deploy to testnets, do the following.

With environment variables
Set your WEB3_INFURA_PROJECT_ID, and PRIVATE_KEY environment variables.

You can get a WEB3_INFURA_PROJECT_ID by getting a free trial of Infura. At the moment, it does need to be infura with brownie. If you get lost, you can follow this guide to getting a project key. You can find your PRIVATE_KEY from your ethereum wallet like metamask.

You'll also need testnet rinkeby ETH and LINK. You can get LINK and ETH into your wallet by using the rinkeby faucets located here. If you're new to this, watch this video.

You can add your environment variables to the .env file:

export WEB3_INFURA_PROJECT_ID=<PROJECT_ID>
export PRIVATE_KEY=<PRIVATE_KEY>
AND THEN RUN source .env TO ACTIVATE THE ENV VARIABLES (You'll need to do this everytime you open a new terminal, or learn how to set them easier)

DO NOT SEND YOUR PRIVATE KEY WITH FUNDS IN IT ONTO GITHUB

Without environment variables
Add your account by doing the following:

brownie accounts new <some_name_you_decide>
You'll be prompted to add your private key, and a password. Then, in your code, you'll want to use load instead of add when getting an account.

account = accounts.load("some_name_you_decide")
Then you'll want to add your RPC_URL to the network of choice. For example:

brownie networks modify rinkeby host=https://your_url_here
If the network you want doesn't already exist, see the below section

Otherwise, you can build, test, and deploy on your local environment.

Local Development
For local testing install ganache-cli

npm install -g ganache-cli
or

yarn add global ganache-cli
All the scripts are designed to work locally or on a testnet. You can add a ganache-cli or ganache UI chain like so:

brownie networks add Ethereum ganache host=http://localhost:8545 chainid=1337
And update the brownie config accordingly. There is a deploy_mocks script that will launch and deploy mock Oracles, VRFCoordinators, Link Tokens, and Price Feeds on a Local Blockchain.

Deploy to a testnet / Scripts
brownie run scripts/1_deploy_lottery.py
brownie run scripts/2_start_lottery.py
brownie run scripts/3_enter_lottery.py
brownie run scripts/4_end_lottery.py
This will deploy your lottery, fund it with LINK, start your lottery, you'll enter it, and then end your lottery. You can also work with the console to do these.

You can deploy and work with a local network by deploying mocks.

Testing
There are 2 types of tests in this project.

unit tests, which run on a local blockchain.
integration tests, which run on a testnet
To run the unit tests:

brownie test
integration tests:

brownie test --network <network>
For more information on effective testing with Chainlink, check out Testing Smart Contracts

Tests are really robust here! They work for local development and testnets. There are a few key differences between the testnets and the local networks. We utilize mocks so we can work with fake oracles on our testnets.

To test development / local
brownie test
To test mainnet-fork
This will test the same way as local testing, but you will need a connection to a mainnet blockchain (like with the infura environment variable.)

brownie test --network mainnet-fork
To test a testnet
Kovan and Rinkeby are currently supported

brownie test --network kovan
Adding additional Chains
If the blockchain is EVM Compatible, adding new chains can be accomplished by something like:

brownie networks add Ethereum binance-smart-chain host=https://bsc-dataseed1.binance.org chainid=56
or, for a fork:

brownie networks add development binance-fork cmd=ganache-cli host=http://127.0.0.1 fork=https://bsc-dataseed1.binance.org accounts=10 mnemonic=brownie port=8545



