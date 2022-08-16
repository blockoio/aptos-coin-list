# token-list

A permissionless, on-chain token list for aptos.

This coin list adopts a model where listing is separated into a 2-step process:
1. Coin owner (owner of coin's module) registers CoinInfo into CoinRegistry
2. CoinList users create their own list, and indicate which coins they'd like to add to their own list

The primary structs are:
- `CoinInfo`: contains information about a particular coin, including items such as symbol, logo-url, project-url,
  coingecko-id, and a set of owner-defined extensions in the form of `SimpleMap<String, String>`.
- `CoinRegistry`: there is a single `CoinRegistry` globally that contains all registered CoinInfo. Only module owner of
  coin type is allowed to edit its own information in the registry.
- `CoinList`: each user can own a `CoinList`, which contains a subset of keys that the user is interested in.

CoinInfo is maintained exclusively in the CoinRegistry, and each user's CoinList only contains the set of included coin
TypeInfo. This allows CoinInfo to be registered permissionlessly, yet each user still has control over which subset of
all registered coins he wants to include into his own list.

Each user's full CoinList can be fetched in a single lookup using `move-to-ts`'s query feature
```typescript
const {client, account} = ...;
const app = new App(client).coin_list.coin_list;
const myList = await app.query_fetch_full_list(account, account.address(), []);
```

A major difference this design has with `aptos_framework::coin::CoinInfo` is that our `CoinInfo` do not have type
parameters, and are therefore not stored as independent resources. This allows us to iterate over a set of CoinInfo
values (not possible if they are type-parameterized).


# CLI Usage
```

Usage: yarn cli [options] [command]

Move TS CLI generated by move-to-ts

Options:
  -c, --config <path>                                path to your aptos config.yml (generated with "aptos init")
  -p, --profile <PROFILE>                            aptos config profile to use (default: "default")
  -h, --help                                         display help for command

Commands:
  coin-list:add-extension <TYPE_CoinType> <key> <value>
  coin-list:add-to-list <TYPE_CoinType>
  coin-list:add-to-registry-by-signer <TYPE_CoinType> <name> <symbol> <coingecko_id> <logo_url> <project_url> <is_update>
  coin-list:create-list
  coin-list:drop-extension <TYPE_CoinType> <key> <value>
  coin-list:initialize
  coin-list:remove-from-list <TYPE_CoinType>
  devnet-coins:deploy                                          Register devnet coins
  devnet-coins:mint-to-wallet <TYPE_CoinType> <amount>
  coin-list:query-fetch-all-registered-coin-info
  coin-list:query-fetch-full-list <list_owner_addr>
```

You can use the TypeScript CLI to get the list owned by `0x498d8926f16eb9ca90cab1b3a26aa6f97a080b3fcbe6e83ae150b7243a00fb68`

```bash
cd typescript
yarn install; yarn build
yarn cli -c APTOS_CONFIG_FILE coin-list:query-fetch-full-list 0x498d8926f16eb9ca90cab1b3a26aa6f97a080b3fcbe6e83ae150b7243a00fb68
```

Result:

```json
{
  "coin_info_list": [
    {
      "name": "USD Coin",
      "symbol": "USDC",
      "coingecko_id": "usd-coin",
      "decimals": "8",
      "logo_url": "https://assets.coingecko.com/coins/images/6319/small/USD_Coin_icon.png?1547042389",
      "project_url": "project_url",
      "token_type": {
        "type": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8::devnet_coins::DevnetUSDC",
        "account_address": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8",
        "module_name": "devnet_coins",
        "struct_name": "DevnetUSDC"
      },
      "extensions": {
        "data": []
      }
    },
    {
      "name": "Tether",
      "symbol": "USDT",
      "coingecko_id": "tether",
      "decimals": "8",
      "logo_url": "https://assets.coingecko.com/coins/images/325/small/Tether-logo.png?1598003707",
      "project_url": "project_url",
      "token_type": {
        "type": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8::devnet_coins::DevnetUSDT",
        "account_address": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8",
        "module_name": "devnet_coins",
        "struct_name": "DevnetUSDT"
      },
      "extensions": {
        "data": []
      }
    },
    {
      "name": "Solana",
      "symbol": "SOL",
      "coingecko_id": "solana",
      "decimals": "8",
      "logo_url": "https://assets.coingecko.com/coins/images/4128/small/solana.png?1640133422",
      "project_url": "project_url",
      "token_type": {
        "type": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8::devnet_coins::DevnetSOL",
        "account_address": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8",
        "module_name": "devnet_coins",
        "struct_name": "DevnetSOL"
      },
      "extensions": {
        "data": []
      }
    },
    {
      "name": "Ethereum",
      "symbol": "ETH",
      "coingecko_id": "ethereum",
      "decimals": "8",
      "logo_url": "https://assets.coingecko.com/coins/images/279/small/ethereum.png?1595348880",
      "project_url": "project_url",
      "token_type": {
        "type": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8::devnet_coins::DevnetETH",
        "account_address": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8",
        "module_name": "devnet_coins",
        "struct_name": "DevnetETH"
      },
      "extensions": {
        "data": []
      }
    },
    {
      "name": "Bitcoin",
      "symbol": "BTC",
      "coingecko_id": "bitcoin",
      "decimals": "8",
      "logo_url": "https://assets.coingecko.com/coins/images/1/small/bitcoin.png?1547033579",
      "project_url": "project_url",
      "token_type": {
        "type": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8::devnet_coins::DevnetBTC",
        "account_address": "0xf70ac33c984f8b7bead655ad239d246f1c0e3ca55fe0b8bfc119aa529c4630e8",
        "module_name": "devnet_coins",
        "struct_name": "DevnetBTC"
      },
      "extensions": {
        "data": [
          {
            "key": "key1",
            "value": "value1"
          }
        ]
      }
    }
  ]
}
```

Or you can fetch the all registered CoinInfo using:
```bash
yarn cli -c APTOS_CONFIG_FILE coin-list:query-fetch-all-registered-coin-info
```

This would return more result than the previous command.