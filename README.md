# 
contracts/hardhat.config.ts
@@ -1,40 +1,12 @@
import '@nomiclabs/hardhat-waffle';
import '@nomiclabs/hardhat-solpp';
import '@nomiclabs/hardhat-etherscan';
import 'hardhat-typechain';
import 'hardhat-contract-sizer';
import '@nomiclabs/hardhat-etherscan';

const prodConfig = {
    // UPGRADE_NOTICE_PERIOD: 0,
    MAX_AMOUNT_OF_REGISTERED_TOKENS: 127,
    // PRIORITY_EXPIRATION: 101,
    DUMMY_VERIFIER: false
};
const testnetConfig = {
    UPGRADE_NOTICE_PERIOD: 0,
    MAX_AMOUNT_OF_REGISTERED_TOKENS: 127,
    // PRIORITY_EXPIRATION: 101,
    DUMMY_VERIFIER: false
};
const testConfig = {
    UPGRADE_NOTICE_PERIOD: 0,
    MAX_AMOUNT_OF_REGISTERED_TOKENS: 5,
    PRIORITY_EXPIRATION: 101,
    DUMMY_VERIFIER: true
};

const localConfig = Object.assign({}, prodConfig);
localConfig.DUMMY_VERIFIER = process.env.DUMMY_VERIFIER ? true : localConfig.DUMMY_VERIFIER;

const contractDefs = {
    rinkeby: testnetConfig,
    ropsten: testnetConfig,
    mainnet: prodConfig,
    test: testConfig,
    localhost: localConfig
};
import { Network, loadDefs } from './hardhat.utils';

export default {
    defaultNetwork: 'env',
    solidity: {
        version: '0.7.3',
        settings: {
@@ -51,11 +23,11 @@ export default {
        sources: './contracts'
    },
    solpp: {
        defs: process.env.ETH_NETWORK ? contractDefs[process.env.ETH_NETWORK] : contractDefs['test']
        defs: loadDefs(process.env.ETH_NETWORK as Network)
    },
    networks: {
        env: {
            url: process.env.WEB3_URL
            url: `${process.env.WEB3_URL}`
        }
    },
    etherscan: {
 zksync
zksync
