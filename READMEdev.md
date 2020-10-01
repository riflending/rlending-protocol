Riflending Protocol DEV🚀
=================

Prerequisites 📋
------------
Operating system: macOS, debian 10 (buster), ubuntu LTS.

[Node.js](https://nodejs.org/en/download/)(LTS 12), [npm](https://docs.npmjs.com/cli/install) (optional), [yarn](https://yarnpkg.com/lang/en/docs/install/), make, [g++](#g++), [ganache-cli](#ganache-cli)/[ganache](https://www.trufflesuite.com/ganache), [node-gyp](https://github.com/nodejs/node-gyp#installation)(optional)
 
 #### ganache-cli

    #ubuntu/debian    
    npm install -g ganache-cli
    or
    yarn global add ganache-cli
    
#### g++

    #ubuntu/debian    
    sudo apt install g++

#### make

    #ubuntu/debian   
    sudo apt-get install make

### Recomend
Recomend for GNU SO.
Install build-essential package and node-gyp.

    #ubuntu/debian   
    sudo apt-get install build-essential
    npm install -g node-gyp

Also recommend set git pull in [default rebase mode](https://coderwall.com/p/tnoiug/rebase-by-default-when-doing-git-pull).

    #ubuntu/debian   
    git config --global pull.rebase true
    

------------


Installation
------------
To run riflending dev, pull the repository from GitHub
and install its dependencies with npm or yarn.
    
    git clone -b dev https://github.com/riflending/riflending-protocol

    cd riflending    
    yarn install --lock-file 
    or 
    npm install

REPL
----

The Compound Protocol has a simple scenario evaluation tool to test and evaluate scenarios which could occur on the blockchain. This is primarily used for constructing high-level integration tests. The tool also has a REPL to interact with local the Compound Protocol (similar to `truffle console`).

    yarn repl -n development
    yarn repl -n rinkeby

    > Read CToken cBAT Address
    Command: Read CToken cBAT Address
    AddressV<val=0xAD53863b864AE703D31b819d29c14cDA93D7c6a6>

You can read more about the scenario runner in the [Scenario Docs](https://github.com/compound-finance/compound-protocol/tree/master/scenario/SCENARIO.md) on steps for using the repl.

Testing
-------
Jest contract tests are defined under the [tests directory](https://github.com/compound-finance/compound-protocol/tree/master/tests). To run the tests run:

    yarn test

Integration Specs
-----------------

There are additional tests under the [spec/scenario](https://github.com/compound-finance/compound-protocol/tree/master/spec/scenario) folder. These are high-level integration tests based on the scenario runner depicted above. The aim of these tests is to be highly literate and have high coverage in the interaction of contracts.

Formal Verification Specs
-------------------------

The Compound Protocol has a number of formal verification specifications, powered by [Certora](https://www.certora.com/). You can find details in the [spec/formal](https://github.com/compound-finance/compound-protocol/tree/master/spec/formal) folder. The Certora Verification Language (CVL) files included are specifications, which when with the Certora CLI tool, produce formal proofs (or counter-examples) that the code of a given contract exactly matches that specification.
=======
See the [Scenario Docs](https://github.com/compound-finance/money-market/tree/master/scenario/SCENARIO.md) on steps for using the repl.

Testing
-------
Contract tests are defined under the [tests directory](https://github.com/compound-finance/money-market/tree/master/tests). To run the tests run:

    yarn test
>>>>>>> Compound Token and Governance (#519)

Code Coverage
-------------
To run code coverage, run:

    yarn coverage

Linting
-------
To lint the code, run:

    yarn lint

Docker
------

To run in docker:

    # Build the docker image
    docker build -t compound-protocol .

    # Run a shell to the built image
    docker run -it compound-protocol /bin/sh

From within a docker shell, you can interact locally with the protocol via ganache and truffle:

```bash
    /compound-protocol > yarn console -n goerli
    Using network goerli https://goerli-eth.compound.finance
    Saddle console on network goerli https://goerli-eth.compound.finance
    Deployed goerli contracts
      comptroller: 0x627EA49279FD0dE89186A58b8758aD02B6Be2867
      comp: 0xfa5E1B628EFB17C024ca76f65B45Faf6B3128CA5
      governorAlpha: 0x8C3969Dd514B559D78135e9C210F2F773Feadf21
      maximillion: 0x73d3F01b8aC5063f4601C7C45DA5Fdf1b5240C92
      priceOracle: 0x9A536Ed5C97686988F93C9f7C2A390bF3B59c0ec
      priceOracleProxy: 0xd0c84453b3945cd7e84BF7fc53BfFd6718913B71
      timelock: 0x25e46957363e16C4e2D5F2854b062475F9f8d287
      unitroller: 0x627EA49279FD0dE89186A58b8758aD02B6Be2867

    > await comp.methods.totalSupply().call()
    '10000000000000000000000000'
```

Console
-------

After you deploy, as above, you can run a truffle console with the following command:

    yarn console -n goerli

This command will start a saddle console conencted to Goerli testnet (see [Saddle README](https://github.com/compound-finance/saddle#cli)):

```javascript
    Using network goerli https://goerli.infura.io/v3/e1a5d4d2c06a4e81945fca56d0d5d8ea
    Saddle console on network goerli https://goerli.infura.io/v3/e1a5d4d2c06a4e81945fca56d0d5d8ea
    Deployed goerli contracts
      comptroller: 0x627EA49279FD0dE89186A58b8758aD02B6Be2867
      comp: 0xfa5E1B628EFB17C024ca76f65B45Faf6B3128CA5
      governorAlpha: 0x8C3969Dd514B559D78135e9C210F2F773Feadf21
      maximillion: 0x73d3F01b8aC5063f4601C7C45DA5Fdf1b5240C92
      priceOracle: 0x9A536Ed5C97686988F93C9f7C2A390bF3B59c0ec
      priceOracleProxy: 0xd0c84453b3945cd7e84BF7fc53BfFd6718913B71
      timelock: 0x25e46957363e16C4e2D5F2854b062475F9f8d287
      unitroller: 0x627EA49279FD0dE89186A58b8758aD02B6Be2867
    > await comp.methods.totalSupply().call()
    '10000000000000000000000000'
```

Deploying a CToken from Source
------------------------------

Note: you will need to set `~/.ethereum/<network>` with your private key or assign your private key to the environment variable `ACCOUNT`.

Note: for all sections including Etherscan verification, you must set the `ETHERSCAN_API_KEY` to a valid API Key from [Etherscan](https://etherscan.io/apis).

To deploy a new cToken, you can run the `token:deploy`. command, as follows. If you set `VERIFY=true`, the script will verify the token on Etherscan as well. The JSON here is the token config JSON, which should be specific to the token you wish to list.

```bash
npx saddle -n rinkeby script token:deploy '{
  "underlying": "0x577D296678535e4903D59A4C929B718e1D575e0A",
  "comptroller": "$Comptroller",
  "interestRateModel": "$Base200bps_Slope3000bps",
  "initialExchangeRateMantissa": "2.0e18",
  "name": "Compound Kyber Network Crystal",
  "symbol": "cKNC",
  "decimals": "8",
  "admin": "$Timelock"
}'
```

If you only want to verify an existing token an Etherscan, make sure `ETHERSCAN_API_KEY` is set and run `token:verify` with the first argument as the token address and the second as the token config JSON:

```bash
npx saddle -n rinkeby script token:verify 0x19B674715cD20626415C738400FDd0d32D6809B6 '{
  "underlying": "0x577D296678535e4903D59A4C929B718e1D575e0A",
  "comptroller": "$Comptroller",
  "interestRateModel": "$Base200bps_Slope3000bps",
  "initialExchangeRateMantissa": "2.0e18",
  "name": "Compound Kyber Network Crystal",
  "symbol": "cKNC",
  "decimals": "8",
  "admin": "$Timelock"
}'
```

Finally, to see if a given deployment matches this version of the Compound Protocol, you can run `token:match` with a token address and token config:

```bash
npx saddle -n rinkeby script token:match 0x19B674715cD20626415C738400FDd0d32D6809B6 '{
  "underlying": "0x577D296678535e4903D59A4C929B718e1D575e0A",
  "comptroller": "$Comptroller",
  "interestRateModel": "$Base200bps_Slope3000bps",
  "initialExchangeRateMantissa": "2.0e18",
  "name": "Compound Kyber Network Crystal",
  "symbol": "cKNC",
  "decimals": "8",
  "admin": "$Timelock"
}'
```

## Deploying a CToken from Docker Build
---------------------------------------

To deploy a specific version of the Compound Protocol, you can use the `token:deploy` script through Docker:

```bash
docker run --env ETHERSCAN_API_KEY --env VERIFY=true --env ACCOUNT=0x$(cat ~/.ethereum/rinkeby) compoundfinance/compound-protocol:latest npx saddle -n rinkeby script token:deploy '{
  "underlying": "0x577D296678535e4903D59A4C929B718e1D575e0A",
  "comptroller": "$Comptroller",
  "interestRateModel": "$Base200bps_Slope3000bps",
  "initialExchangeRateMantissa": "2.0e18",
  "name": "Compound Kyber Network Crystal",
  "symbol": "cKNC",
  "decimals": "8",
  "admin": "$Timelock"
}'
```

To match a deployed contract against a given version of the Compound Protocol, you can run `token:match` through Docker, passing a token address and config:

```bash
docker run --env ACCOUNT=0x$(cat ~/.ethereum/rinkeby) compoundfinance/compound-protocol:latest npx saddle -n rinkeby script token:match 0xF1BAd36CB247C82Cb4e9C2874374492Afb50d565 '{
  "underlying": "0x577D296678535e4903D59A4C929B718e1D575e0A",
  "comptroller": "$Comptroller",
  "interestRateModel": "$Base200bps_Slope3000bps",
  "initialExchangeRateMantissa": "2.0e18",
  "name": "Compound Kyber Network Crystal",
  "symbol": "cKNC",
  "decimals": "8",
  "admin": "$Timelock"
}'
```

Discussion
----------

For any concerns with the protocol, open an issue or visit us on [Discord](https://compound.finance/discord) to discuss.

For security concerns, please email [security@compound.finance](mailto:security@compound.finance).

_© Copyright 2020, RIF Lending_