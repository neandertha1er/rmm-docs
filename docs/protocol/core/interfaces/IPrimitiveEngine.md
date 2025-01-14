---
description: IPrimitiveEngine
---

# IPrimitiveEngine.sol





## Methods

### BUFFER



```solidity title="Solidity"
function BUFFER() external view returns (uint256)
```





#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | uint256 | Amount of seconds after pool expiry which allows swaps, no swaps after buffer

### MIN_LIQUIDITY



```solidity title="Solidity"
function MIN_LIQUIDITY() external view returns (uint256)
```





#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | uint256 | Amount of liquidity burned on `create()` calls

### PRECISION



```solidity title="Solidity"
function PRECISION() external view returns (uint256)
```





#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | uint256 | Precision units to scale to when doing token related calculations

### allocate

Allocates risky and stable tokens to a specific curve with `poolId`

```solidity title="Solidity"
function allocate(bytes32 poolId, address recipient, uint256 delRisky, uint256 delStable, bool fromMargin, bytes data) external nonpayable returns (uint256 delLiquidity)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| poolId | bytes32 | Pool Identifier
| recipient | address | Address to give the allocated liquidity to
| delRisky | uint256 | Amount of risky tokens to add
| delStable | uint256 | Amount of stable tokens to add
| fromMargin | bool | Whether the `msg.sender` pays with their margin balance, or must send tokens
| data | bytes | Arbitrary data that is passed to the allocateCallback function

#### Returns

| Name | Type | Description |
|---|---|---|
| delLiquidity | uint256 | Amount of liquidity given to `recipient`

### calibrations

Fetches `Calibration` pool parameters

```solidity title="Solidity"
function calibrations(bytes32 poolId) external view returns (uint128 strike, uint32 sigma, uint32 maturity, uint32 lastTimestamp, uint32 gamma)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| poolId | bytes32 | Pool Identifier

#### Returns

| Name | Type | Description |
|---|---|---|
| strike | uint128 |          Strike price of the pool with stable token decimals
| sigma | uint32 |           Implied Volatility as an unsigned 32-bit integer constant w/ precision of 1e4, 10000 = 100%
| maturity | uint32 |        Timestamp of maturity in seconds
| lastTimestamp | uint32 |   Last timestamp used to calculate time until expiry, aka &quot;tau&quot;
| gamma | uint32 |           = 1 - fee %, as an unsigned 32-bit integer constant w/ precision of 1e4, 10000 = 100%

### create

Initializes a curve with parameters in the `settings` storage mapping in the Engine

```solidity title="Solidity"
function create(uint128 strike, uint32 sigma, uint32 maturity, uint32 gamma, uint256 riskyPerLp, uint256 delLiquidity, bytes data) external nonpayable returns (bytes32 poolId, uint256 delRisky, uint256 delStable)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| strike | uint128 | Strike price of the pool to calibrate to, with the same decimals as the stable token
| sigma | uint32 | Implied Volatility to calibrate to as an unsigned 32-bit integer w/ precision of 1e4, 10000 = 100%
| maturity | uint32 | Maturity timestamp of the pool, in seconds
| gamma | uint32 | Multiplied against swap in amounts to apply fee, equal to 1 - fee %, an unsigned 32-bit integer, w/ precision of 1e4, 10000 = 100%
| riskyPerLp | uint256 | Risky reserve per liq. with risky decimals, = 1 - N(d1), d1 = (ln(S/K)+(r*sigma^2/2))/sigma*sqrt(tau)
| delLiquidity | uint256 | Amount of liquidity to allocate to the curve, wei value with 18 decimals of precision
| data | bytes | Arbitrary data that is passed to the createCallback function

#### Returns

| Name | Type | Description |
|---|---|---|
| poolId | bytes32 |      Pool Identifier
| delRisky | uint256 |    Total amount of risky tokens provided to reserves
| delStable | uint256 |   Total amount of stable tokens provided to reserves

### deposit

Adds risky and/or stable tokens to a `recipient`&#39;s internal balance account

```solidity title="Solidity"
function deposit(address recipient, uint256 delRisky, uint256 delStable, bytes data) external nonpayable
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| recipient | address | Recipient margin account of the deposited tokens
| delRisky | uint256 | Amount of risky tokens to deposit
| delStable | uint256 | Amount of stable tokens to deposit
| data | bytes | Arbitrary data that is passed to the depositCallback function

### factory



```solidity title="Solidity"
function factory() external view returns (address)
```





#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | address | undefined

### invariantOf

Fetches the current invariant based on risky and stable token reserves of pool with `poolId`

```solidity title="Solidity"
function invariantOf(bytes32 poolId) external view returns (int128 invariant)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| poolId | bytes32 | Pool Identifier

#### Returns

| Name | Type | Description |
|---|---|---|
| invariant | int128 |   Signed fixed point 64.64 number, invariant of `poolId`

### liquidity

Fetches position liquidity an account address and poolId

```solidity title="Solidity"
function liquidity(address account, bytes32 poolId) external view returns (uint256 liquidity)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| account | address | undefined
| poolId | bytes32 | Pool Identifier

#### Returns

| Name | Type | Description |
|---|---|---|
| liquidity | uint256 |   Liquidity owned by `account` in `poolId`

### margins

Fetches the margin balances of `account`

```solidity title="Solidity"
function margins(address account) external view returns (uint128 balanceRisky, uint128 balanceStable)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| account | address | Margin account to fetch

#### Returns

| Name | Type | Description |
|---|---|---|
| balanceRisky | uint128 |    Balance of the risky token
| balanceStable | uint128 |   Balance of the stable token

### remove

Unallocates risky and stable tokens from a specific curve with `poolId`

```solidity title="Solidity"
function remove(bytes32 poolId, uint256 delLiquidity) external nonpayable returns (uint256 delRisky, uint256 delStable)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| poolId | bytes32 | Pool Identifier
| delLiquidity | uint256 | Amount of liquidity to remove

#### Returns

| Name | Type | Description |
|---|---|---|
| delRisky | uint256 |      Amount of risky tokens received from removed liquidity
| delStable | uint256 |     Amount of stable tokens received from removed liquidity

### reserves

Fetches the global reserve state for a pool with `poolId`

```solidity title="Solidity"
function reserves(bytes32 poolId) external view returns (uint128 reserveRisky, uint128 reserveStable, uint128 liquidity, uint32 blockTimestamp, uint256 cumulativeRisky, uint256 cumulativeStable, uint256 cumulativeLiquidity)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| poolId | bytes32 | Pool Identifier

#### Returns

| Name | Type | Description |
|---|---|---|
| reserveRisky | uint128 |         Risky token balance in the reserve
| reserveStable | uint128 |        Stable token balance in the reserve
| liquidity | uint128 |            Total supply of liquidity for the curve
| blockTimestamp | uint32 |       Timestamp when the cumulative reserve values were last updated
| cumulativeRisky | uint256 |      Cumulative sum of risky token reserves of the previous update
| cumulativeStable | uint256 |     Cumulative sum of stable token reserves of the previous update
| cumulativeLiquidity | uint256 |  Cumulative sum of total supply of liquidity of the previous update

### risky



```solidity title="Solidity"
function risky() external view returns (address)
```





#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | address | undefined

### scaleFactorRisky



```solidity title="Solidity"
function scaleFactorRisky() external view returns (uint256)
```





#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | uint256 | Multiplier to scale amounts to/from, equal to 10^(18 - riskyDecimals)

### scaleFactorStable



```solidity title="Solidity"
function scaleFactorStable() external view returns (uint256)
```





#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | uint256 | Multiplier to scale amounts to/from, equal to 10^(18 - stableDecimals)

### stable



```solidity title="Solidity"
function stable() external view returns (address)
```





#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | address | Stable token address

### swap

Swaps between `risky` and `stable` tokens

```solidity title="Solidity"
function swap(address recipient, bytes32 poolId, bool riskyForStable, uint256 deltaIn, uint256 deltaOut, bool fromMargin, bool toMargin, bytes data) external nonpayable
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| recipient | address | Address that receives output token `deltaOut` amount
| poolId | bytes32 | Pool Identifier
| riskyForStable | bool | If true, swap risky to stable, else swap stable to risky
| deltaIn | uint256 | Amount of tokens to swap in
| deltaOut | uint256 | Amount of tokens to swap out
| fromMargin | bool | Whether the `msg.sender` uses their margin balance, or must send tokens
| toMargin | bool | Whether the `deltaOut` amount is transferred or deposited into margin
| data | bytes | Arbitrary data that is passed to the swapCallback function

### updateLastTimestamp

Updates the time until expiry of the pool by setting its last timestamp value

```solidity title="Solidity"
function updateLastTimestamp(bytes32 poolId) external nonpayable returns (uint32 lastTimestamp)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| poolId | bytes32 | Pool Identifier

#### Returns

| Name | Type | Description |
|---|---|---|
| lastTimestamp | uint32 | Timestamp loaded into the state of the pool&#39;s Calibration.lastTimestamp

### withdraw

Removes risky and/or stable tokens from a `msg.sender`&#39;s internal balance account

```solidity title="Solidity"
function withdraw(address recipient, uint256 delRisky, uint256 delStable) external nonpayable
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| recipient | address | Address that tokens are transferred to
| delRisky | uint256 | Amount of risky tokens to withdraw
| delStable | uint256 | Amount of stable tokens to withdraw



## Events

### Allocate

Adds liquidity of risky and stable tokens to a specified `poolId`

```solidity title="Solidity"
event Allocate(address indexed from, address indexed recipient, bytes32 indexed poolId, uint256 delRisky, uint256 delStable)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| from `indexed` | address | undefined |
| recipient `indexed` | address | undefined |
| poolId `indexed` | bytes32 | undefined |
| delRisky  | uint256 | undefined |
| delStable  | uint256 | undefined |

### Create

Creates a pool with liquidity

```solidity title="Solidity"
event Create(address indexed from, uint128 indexed strike, uint32 sigma, uint32 indexed maturity, uint32 gamma)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| from `indexed` | address | undefined |
| strike `indexed` | uint128 | undefined |
| sigma  | uint32 | undefined |
| maturity `indexed` | uint32 | undefined |
| gamma  | uint32 | undefined |

### Deposit

Added stable and/or risky tokens to a margin accouynt

```solidity title="Solidity"
event Deposit(address indexed from, address indexed recipient, uint256 delRisky, uint256 delStable)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| from `indexed` | address | undefined |
| recipient `indexed` | address | undefined |
| delRisky  | uint256 | undefined |
| delStable  | uint256 | undefined |

### Remove

Adds liquidity of risky and stable tokens to a specified `poolId`

```solidity title="Solidity"
event Remove(address indexed from, bytes32 indexed poolId, uint256 delRisky, uint256 delStable)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| from `indexed` | address | undefined |
| poolId `indexed` | bytes32 | undefined |
| delRisky  | uint256 | undefined |
| delStable  | uint256 | undefined |

### Swap

Swaps between `risky` and `stable` assets

```solidity title="Solidity"
event Swap(address indexed from, address indexed recipient, bytes32 indexed poolId, bool riskyForStable, uint256 deltaIn, uint256 deltaOut)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| from `indexed` | address | undefined |
| recipient `indexed` | address | undefined |
| poolId `indexed` | bytes32 | undefined |
| riskyForStable  | bool | undefined |
| deltaIn  | uint256 | undefined |
| deltaOut  | uint256 | undefined |

### UpdateLastTimestamp

Updates the time until expiry of the pool with `poolId`

```solidity title="Solidity"
event UpdateLastTimestamp(bytes32 indexed poolId)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| poolId `indexed` | bytes32 | undefined |

### Withdraw

Removes stable and/or risky from a margin account

```solidity title="Solidity"
event Withdraw(address indexed from, address indexed recipient, uint256 delRisky, uint256 delStable)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| from `indexed` | address | undefined |
| recipient `indexed` | address | undefined |
| delRisky  | uint256 | undefined |
| delStable  | uint256 | undefined |



## Errors

### BalanceError

Thrown when the balanceOf function is not successful and does not return data

```solidity title="Solidity"
error BalanceError()
```





### CalibrationError

Thrown when the parameters of a new pool are invalid, causing initial reserves to be 0

```solidity title="Solidity"
error CalibrationError(uint256 delRisky, uint256 delStable)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| delRisky | uint256 | undefined |
| delStable | uint256 | undefined |

### DeltaInError

Thrown when the deltaIn parameter is 0

```solidity title="Solidity"
error DeltaInError()
```





### DeltaOutError

Thrown when the deltaOut parameter is 0

```solidity title="Solidity"
error DeltaOutError()
```





### GammaError

Thrown when gamma, equal to 1 - fee %, is outside its bounds: 9000 &lt;= gamma &lt; 10000; 1000 = 10% fee

```solidity title="Solidity"
error GammaError(uint256 value)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| value | uint256 | undefined |

### InvariantError

Thrown when the invariant check fails

```solidity title="Solidity"
error InvariantError(int128 invariant, int128 nextInvariant)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| invariant | int128 | Pre-swap invariant updated with new tau |
| nextInvariant | int128 | Post-swap invariant |

### LockedError

Thrown when a callback function calls the engine __again__

```solidity title="Solidity"
error LockedError()
```





### MinLiquidityError

Thrown when liquidity is lower than or equal to the minimum amount of liquidity

```solidity title="Solidity"
error MinLiquidityError(uint256 value)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| value | uint256 | undefined |

### PoolDuplicateError

Thrown when a pool with poolId already exists

```solidity title="Solidity"
error PoolDuplicateError()
```





### PoolExpiredError

Thrown when calling an expired pool, where block.timestamp &gt; maturity, + BUFFER if swap

```solidity title="Solidity"
error PoolExpiredError()
```





### RiskyBalanceError

Thrown when the expected risky balance is less than the actual balance

```solidity title="Solidity"
error RiskyBalanceError(uint256 expected, uint256 actual)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| expected | uint256 | Expected risky balance |
| actual | uint256 | Actual risky balance |

### RiskyPerLpError

Thrown when riskyPerLp is outside the range of acceptable values, 0 &lt; riskyPerLp &lt; 1eRiskyDecimals

```solidity title="Solidity"
error RiskyPerLpError(uint256 value)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| value | uint256 | undefined |

### SigmaError

Thrown when sigma is outside the range of acceptable values, 100 &lt;= sigma &lt;= 1e7 with 4 precision

```solidity title="Solidity"
error SigmaError(uint256 value)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| value | uint256 | undefined |

### StableBalanceError

Thrown when the expected stable balance is less than the actual balance

```solidity title="Solidity"
error StableBalanceError(uint256 expected, uint256 actual)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| expected | uint256 | Expected stable balance |
| actual | uint256 | Actual stable balance |

### StrikeError

Thrown when strike is not valid, i.e. equal to 0 or greater than 2^128

```solidity title="Solidity"
error StrikeError(uint256 value)
```




#### Parameters

| Name | Type | Description |
|---|---|---|
| value | uint256 | undefined |

### UninitializedError

Thrown when the pool with poolId has not been created

```solidity title="Solidity"
error UninitializedError()
```





### ZeroDeltasError

Thrown when the risky or stable amount is 0

```solidity title="Solidity"
error ZeroDeltasError()
```





### ZeroLiquidityError

Thrown when the liquidity parameter is 0

```solidity title="Solidity"
error ZeroLiquidityError()
```
