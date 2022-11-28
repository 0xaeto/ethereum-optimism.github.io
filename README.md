# Optimism Token List

The Optimism token list is used as the source of truth for the [Optimism Gateway](https://app.optimism.io/bridge) which is the main portal for moving assets between Layer 1 and Layer 2.

## Review process and merge criteria

### Process overview

1. Follow the instructions below to create a PR that would [add your token to the list](#adding-a-token-to-the-list).
2. Respond to any issues raised in comments on your PR generated by [automated checks](#automated-checks).
3. If your PR requires in-depth manual review, wait for a reviewer (**we review PRs once per week**).
4. Wait for final approval.

### Automated checks

Note that if your token was grandfathered in before the validation was added, your token may fail validation when you edit it.  If this is the case you need to add additional fields that were since added such as website field.

Our CI performs a series of automated checks on every PR.
Some issues raised by CI will trigger an error and must be resolved before your PR will be reviewed.
These issues are marked below as "auto-reject" issues.
Other issues will not immediately cause an error, but will require that a reviewer manually review your PR before it can be merged.
These issues are marked below as "requires manual review".

- Given tokens actually exist on all specified chains (auto-reject)
- L1 tokens are verified on Etherscan (auto-reject)
- Description exists and is under 150 characters (auto-reject)
- Website exists and loads properly (auto-reject)
- Token `name`, `symbol`, and `decimals` matches on-chain data (auto-reject)
  - If `overrides` are used (requires manual review)
- L2 token was deployed by the StandardTokenFactory (requires manual review)
- Ethereum token listed on the [CoinGecko Token List](https://tokenlists.org/token-list?url=https://tokens.coingecko.com/uniswap/all.json)
  - *Why CoinGecko? CoinGecko's token list updates every hour which means we get token list updates very quickly. CoinGecko also uses an in-depth [listing criteria](https://www.coingecko.com/en/methodology).*

### Final approval

All PRs are subject to a light-weight final approval, even if not marked as `requires manual review`.

## Adding a token to the list

### Create a folder for your token

Create a folder inside of the [data folder](https://github.com/ethereum-optimism/ethereum-optimism.github.io/tree/master/data) with the same name as the symbol of the token you are trying to add. For example, if you are adding a token with the symbol "ETH" you must create a folder called ETH.

### Add a logo to your folder

Add a logo to the data folder you just created. Your logo MUST be an SVG called `logo.svg`. Your logo should be at least 200x200 pt minimum and 256x256 pt preferred.

### Create a data file

Add a file to your folder called `data.json` with the following format:

```json
{
  "name": "Token Name",
  "symbol": "SYMBOL",
  "decimals": 18,
  "description": "A multi-chain token",
  "website": "https://token.com",
  "twitter": "@token",
  "tokens": {
    "ethereum": {
      "address": "0x1234123412341234123412341234123412341234"
    },
    "optimism": {
      "address": "0x2345234523452345234523452345234523452345"
    },
    "goerli": {
      "address": "0x5678567856785678567856785678567856785678"
    },
    "optimism-goerli": {
      "address": "0x6789678967896789678967896789678967896789"
    }
  }
}
```

Please include the token addresses for *all* chains where the token you are submitting has been deployed.
We currently accept tokens on the following chains:

- `ethereum`
- `optimism`
- `goerli`
- `optimism-goerli`

#### Non-bridgable tokens

If you would like to add your token to this token list but you do not want your token to be included on the Optimism Bridge app, please include the `nobridge` option.

```json
{
  "name": "Token Name",
  "symbol": "SYMBOL",
  "decimals": 18,
  "description": "A multi-chain token",
  "website": "https://token.com",
  "twitter": "@token",
  "nobridge": true,
  "tokens": {
    ...
  }
}
```

#### Non-standard tokens

If your token is not a standard ERC20 (e.g., DSToken), please include the `nonstandard` option.

```json
{
  "name": "Token Name",
  "symbol": "SYMBOL",
  "decimals": 18,
  "description": "A multi-chain token",
  "website": "https://token.com",
  "twitter": "@token",
  "nonstandard": true,
  "tokens": {
    ...
  }
}
```

#### Per-token overrides

If you require overrides for specific tokens, you can include the `overrides` field. You are able to override the `name`, `symbol`, `decimals`, or `bridge` for any token. You do not need to override every token at the same time.

```json
{
  "name": "Token Name",
  "symbol": "SYMBOL",
  "decimals": 18,
  "description": "A multi-chain token",
  "website": "https://token.com",
  "twitter": "@token",
  "tokens": {
    "ethereum": {
      "address": "0x1234123412341234123412341234123412341234",
      "overrides": {
        "name": "My Ethereum Token"
      }
    },
    "optimism": {
      "address": "0x2345234523452345234523452345234523452345",
      "overrides": {
        "bridge": "0x1111111111111111111111111111111111111111"
      }
    }
  }
}
```

### Create a pull request

Open a [pull request](https://github.com/ethereum-optimism/ethereum-optimism.github.io/pulls) with the changes that you've made. Please only add one token per pull request to simplify the review process. This means two new files inside of one new folder. If you want to add multiple tokens, please open different PRs for each token.

### Respond to validation checks

Your pull request will be validated by a series of automated checks. If one of these checks fails, please resolve these issues and make sure that validation succeeds. We will review your pull request for final approval once automated validation succeeds.

### Wait for the token list to update automatically

Once your PR is merged, the token list will update automatically to include your token. Please do NOT update the token list directly. All token list updates will be handled automatically when PRs are merged into the `master` branch. 
