<div id="splash">
    <div id="project">
          <span class="splash-title">
               Project
          </span>
          <br />
          <span id="project-value">
               Gamma Protocol
          </span>
    </div>
     <div id="details">
          <div id="left">
               <span class="splash-title">
                    Client
               </span>
               <br />
               <span class="details-value">
                    Opyn
               </span>
               <br />
               <span class="splash-title">
                    Date
               </span>
               <br />
               <span class="details-value">
                    August 2021
               </span>
          </div>
          <div id="right">
               <span class="splash-title">
                    Reviewers
               </span>
               <br />
               <span class="details-value">
                    Daniel Luca
               </span><br />
               <span class="contact">@cleanunicorn</span>
               <br />
               <span class="details-value">
                    Andrei Simion
               </span><br />
               <span class="contact">@andreiashu</span>
          </div>
    </div>
</div>


## Table of Contents
 - [Details](#details)
 - [Issues Summary](#issues-summary)
 - [Executive summary](#executive-summary)
     - [Week 1](#week-1)
     - [Week 2](#week-2)
 - [Scope](#scope)
 - [Recommendations](#recommendations)
 - [Issues](#issues)
     - [Donate will leave funds blocked in contract](#donate-will-leave-funds-blocked-in-contract)
     - [donate should check whitelist](#donate-should-check-whitelist)
     - [A user can be frontrun when they try to sync their vault](#a-user-can-be-frontrun-when-they-try-to-sync-their-vault)
     - [notPartiallyPaused, notFullyPaused and onlyAuthorized modifiers have unnecessary function counterparts](#notpartiallypaused-notfullypaused-and-onlyauthorized-modifiers-have-unnecessary-function-counterparts)
     - [Opening a vault type defaults to type zero](#opening-a-vault-type-defaults-to-type-zero)
     - [External contract calls have unclear purpose](#external-contract-calls-have-unclear-purpose)
     - [Reduce the number of methods where possible](#reduce-the-number-of-methods-where-possible)
     - [EIP-2612 permit is likely to change](#eip-2612-permit-is-likely-to-change)
 - [Artifacts](#artifacts)
     - [Surya](#surya)
     - [Coverage](#coverage)
     - [Tests](#tests)
 - [License](#license)


## Details

- **Client** Opyn
- **Date** August 2021
- **Lead reviewer** Daniel Luca ([@cleanunicorn](https://twitter.com/cleanunicorn))
- **Reviewers** Daniel Luca ([@cleanunicorn](https://twitter.com/cleanunicorn)), Andrei Simion ([@andreiashu](https://twitter.com/andreiashu))
- **Repository**: [Gamma Protocol](https://github.com/opynfinance/GammaProtocol.git)
- **Commit hash** `67a2bff57ec49c4bb7c9c454c8ad945fd5bdcf51`
- **Technologies**
  - Solidity
  - Node.JS

## Issues Summary

| SEVERITY | OPEN  | CLOSED |
| -------- | :---: | :----: |
|  Informational  |  3  |  0  |
|  Minor  |  2  |  0  |
|  Medium  |  2  |  0  |
|  Major  |  1  |  0  |

## Executive summary

This report represents the results of the engagement with **Opyn** to review **Gamma Protocol**.

The review was conducted over the course of **2 weeks** from **August 16 to August 27, 2021**. A total of **20 person-days** were spent reviewing the code.

### Week 1

During the first week, we started to get familiarized with the code and the project. We reviewed the code from the beginning to the end of the week.

We set up a few meetings throughout the week to discuss the code and learn how to navigate the codebase. We also discussed the project goals and the project scope.

We identified a few minor issues that we presented to the development team. A few of the already implemented and deployed features needed more clarification in terms of how they are currently configured, and how they fit into the overall project.

### Week 2

We continued to keep communication open with the development team while navigating the code and trying out different attack vectors. 

Even though the project has a lot of code, the way it is structured makes it easy to navigate and understand. Some of its parts exist outside of the blockchain context, meaning that they push trusted data into the chain, which then is used to calculate different security parameters.

During the final days of the week we finalized the report and presented it to the client.

## Scope

The initial review focused on the [Gamma Protocol](https://github.com/opynfinance/GammaProtocol.git) repository, identified by the commit hash `67a2bff57ec49c4bb7c9c454c8ad945fd5bdcf51`. ...

<!-- We focused on manually reviewing the codebase, searching for security issues such as, but not limited to, re-entrancy problems, transaction ordering, block timestamp dependency, exception handling, call stack depth limitation, integer overflow/underflow, self-destructible contracts, unsecured balance, use of origin, costly gas patterns, architectural problems, code readability. -->

**Includes:**
- core/Controller.sol
- core/MarginCalculator.sol
- core/Otoken.sol
- external/callees/PermitCallee.sol
- libs/MarginVault.sol
- other contracts supporting these files

**Excludes:**
- Everything else

## Recommendations

We identified a few possible general improvements that are not security issues during the review, which will bring value to the developers and the community reviewing and using the product.

<!-- ### Increase the number of tests

A good rule of thumb is to have 100% test coverage. This does not guarantee the lack of security problems, but it means that the desired functionality behaves as intended. The negative tests also bring a lot of value because not allowing some actions to happen is also part of the desired behavior.

-->

<!-- ### Set up Continuous Integration

Use one of the platforms that offer Continuous Integration services and implement a list of actions that compile, test, run coverage and create alerts when the pipeline fails.

Because the repository is hosted on GitHub, the most painless way to set up the Continuous Integration is through [GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions).

Setting up the workflow can start based on this example template.


```yml
name: Continuous Integration

on:
  push:
    branches: [master]
  pull_request:
    branches: [master]

jobs:
  build:
    name: Build and test
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x]
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: cp ./config.sample.js ./config.js
    - run: npm test

  coverage:
    name: Coverage
    needs: build
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x]
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: cp ./config.sample.js ./config.js
    - run: npm run coverage
    - uses: actions/upload-artifact@v2
      with:
        name: Coverage ${{ matrix.node-version }}
        path: |
          coverage/
```

This CI template activates on pushes and pull requests on the **master** branch.

```yml
on:
  push:
    branches: [master]
  pull_request:
    branches: [master]
```

It uses an [Ubuntu Docker](https://hub.docker.com/_/ubuntu) image as a base for setting up the project.

```yml
    runs-on: ubuntu-latest
```

Multiple Node.js versions can be used to check integration. However, because this is not primarily a Node.js project, multiple versions don't provide added value.

```yml
    strategy:
      matrix:
        node-version: [12.x]
```

A script item should be added in the `scripts` section of [package.json](./code/package.json) that runs all tests.

```json
{
   "script": {
      "test": "buidler test"
   }
}
```

This can then be called by running `npm test` after setting up the dependencies with `npm ci`.

If any hidden variables need to be defined, you can set them up in a local version of `./config.sample.js` (locally named `./config.js`). If you decide to do that, you should also add `./config.js` in `.gitignore` to make sure no hidden variables are pushed to the public repository. The sample config file `./config.sample.js` should be sufficient to pass the test suite.

```yml
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: cp ./config.sample.js ./config.js
    - run: npm test
```

You can also choose to run coverage and upload the generated artifacts.

```yml
    - run: npm run coverage
    - uses: actions/upload-artifact@v2
      with:
        name: Coverage ${{ matrix.node-version }}
        path: |
          coverage/
```

At the moment, checking the artifacts is not [that](https://github.community/t/browsing-artifacts/16954) [easy](https://github.community/t/need-clarification-on-github-actions/16027/2), because one needs to download the zip archive, unpack it and check it. However, the coverage can be checked in the **Actions** section once it's set up.

-->

<!-- ### Contract size

The contracts are dangerously close to the hard limit defined by [EIP-170](https://eips.ethereum.org/EIPS/eip-170), specifically **24676 bytes**.

Depending on the Solidity compiler version and the optimization runs, the contract size might increase over the hard limit. As stated in [the Solidity documentation](https://solidity.readthedocs.io/en/latest/using-the-compiler.html#using-the-commandline-compiler), increasing the number of optimizer runs increases the contract size.

> If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to `--optimize-runs=1`. If you expect many transactions and do not care for higher deployment cost and output size, set `--optimize-runs` to a high number.

Even if you remove the unused internal functions, it will not reduce the contract size because the Solidity compiler shakes that unused code out of the generated bytecode.

#### DELEGATECALL approach

Another way to improve contract size is by breaking them into multiple smaller contracts, grouped by functionality and using `DELEGATECALL` to execute that code. A standard that defines code splitting and selective code upgrade is the [EIP-2535 Diamond Standard](https://eips.ethereum.org/EIPS/eip-2535), which is an extension of [Transparent Contract Standard](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1538.md). A detailed explanation, documentation and implementations can be found in the [EIP-2535](https://eips.ethereum.org/EIPS/eip-2535). However, the current EIP is in **Draft** status, which means the interface, implementation, and overall architecture might change. Another thing to keep in mind is that using this pattern increases the gas cost. -->

## Issues


### [Donate will leave funds blocked in contract](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/issues/5)
![Issue status: Open](https://img.shields.io/static/v1?label=Status&message=Open&color=5856D6&style=flat-square) ![Major](https://img.shields.io/static/v1?label=Severity&message=Major&color=ff3b30&style=flat-square)

**Description**

`donate` function seems to allow users to donate an `_amount` of tokens of `_asset` to the margin pool:


[code/contracts/core/Controller.sol#L316-L322](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L316-L322)
```solidity
     * @notice send asset amount to margin pool
     * @dev use donate() instead of direct transfer() to store the balance in assetBalance
     * @param _asset asset address
     * @param _amount amount to donate to pool
     */
    function donate(address _asset, uint256 _amount) external {
        pool.transferToPool(_asset, msg.sender, _amount);
```

`transferToPool` is used in order to transfer the tokens from the user to the pool. This function takes care of the internal accounting inside the `MarginPool` contract and updates the `assetBalance` accordingly:


[code/contracts/core/MarginPool.sol#L74-L83](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/MarginPool.sol#L74-L83)
```solidity
    function transferToPool(
        address _asset,
        address _user,
        uint256 _amount
    ) public onlyController {
        require(_amount > 0, "MarginPool: transferToPool amount is equal to 0");
        assetBalance[_asset] = assetBalance[_asset].add(_amount);

        // transfer _asset _amount from _user to pool
        ERC20Interface(_asset).safeTransferFrom(_user, address(this), _amount);
```

We believe that the `farm` function was intended to be the one that would allow a designated farm account to withdraw the funds that were donated into the pool (ie. the difference between what is in the contract and `assetBalance`):


[code/contracts/core/MarginPool.sol#L173-L178](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/MarginPool.sol#L173-L178)
```solidity
        uint256 externalBalance = ERC20Interface(_asset).balanceOf(address(this));
        uint256 storedBalance = assetBalance[_asset];

        require(_amount <= externalBalance.sub(storedBalance), "MarginPool: amount to farm exceeds limit");

        ERC20Interface(_asset).safeTransfer(_receiver, _amount);
```

However, the issue is that because `transferToPool` also updates the internal accounting in the `assetBalance` mapping, the `require` at line L176 in `MarginPool` contract will fail for any value of `_amount` higher than 0.  Because the difference between the funds in the contract (`externalBalance `) and the amount accounted in the `assetBalance` mapping (`storedBalance`) will be 0 (*).  This leads to the situation where the donated funds are stuck in the contract:


[code/contracts/core/MarginPool.sol#L176](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/MarginPool.sol#L176)
```solidity
        require(_amount <= externalBalance.sub(storedBalance), "MarginPool: amount to farm exceeds limit");
```

(*) this is true as long as users follow the recommendation in the `donate` documentation and only send transfers via this function. Any tokens sent _directly_ to the `MarginPool` by use of `transfer` can actually be _withdrawn_ / _farmed_.

**Recommendation**

Since this is a live contract and other systems might already be aware of the extrenal `donate` function, it might make sense to just do a `ERC20Interface(_asset).safeTransfer` call directly, without calling the `transferToPool`. This way, when the `farm` function is called, the `ERC20Interface(_asset).balanceOf` call in `MarginPool` contract will indeed be higher than the `assetBalance[_asset]` and the difference can be farmed.


---


### [`donate` should check whitelist](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/issues/4)
![Issue status: Open](https://img.shields.io/static/v1?label=Status&message=Open&color=5856D6&style=flat-square) ![Medium](https://img.shields.io/static/v1?label=Severity&message=Medium&color=FF9500&style=flat-square)

**Description**

Any actor can call `Controller.donate()` to push some ERC20 tokens into the pool.


[code/contracts/core/Controller.sol#L315-L325](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L315-L325)
```solidity
    /**
     * @notice send asset amount to margin pool
     * @dev use donate() instead of direct transfer() to store the balance in assetBalance
     * @param _asset asset address
     * @param _amount amount to donate to pool
     */
    function donate(address _asset, uint256 _amount) external {
        pool.transferToPool(_asset, msg.sender, _amount);

        emit Donated(msg.sender, _asset, _amount);
    }
```

There are other instances where tokens are pushed into the pool, but an additional whitelist validation is done in each case.

**_depositLong**


[code/contracts/core/Controller.sol#L726-L731](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L726-L731)
```solidity
    /**
     * @notice deposit a long oToken into a vault
     * @dev only the account owner or operator can deposit a long oToken, cannot be called when system is partiallyPaused or fullyPaused
     * @param _args DepositArgs structure
     */
    function _depositLong(Actions.DepositArgs memory _args)
```


[code/contracts/core/Controller.sol#L740](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L740)
```solidity
        require(whitelist.isWhitelistedOtoken(_args.asset), "C17");
```


[code/contracts/core/Controller.sol#L748](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L748)
```solidity
        pool.transferToPool(_args.asset, _args.from, _args.amount);
```

**_depositCollateral**


[code/contracts/core/Controller.sol#L776-L781](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L776-L781)
```solidity
    /**
     * @notice deposit a collateral asset into a vault
     * @dev only the account owner or operator can deposit collateral, cannot be called when system is partiallyPaused or fullyPaused
     * @param _args DepositArgs structure
     */
    function _depositCollateral(Actions.DepositArgs memory _args)
```


[code/contracts/core/Controller.sol#L790](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L790)
```solidity
        require(whitelist.isWhitelistedCollateral(_args.asset), "C21");
```


[code/contracts/core/Controller.sol#L802](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L802)
```solidity
        pool.transferToPool(_args.asset, _args.from, _args.amount);
```

In order to keep the same rigorous accounting of only whitelisted tokens being accounted for in the pool, a whitelist check should be done in `donate` before transferring tokens.

**Recommendation**

Add an additional `require` check to validate the transferred token to the pool in the method `donate`.

---


### [A user can be frontrun when they try to sync their vault](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/issues/1)
![Issue status: Open](https://img.shields.io/static/v1?label=Status&message=Open&color=5856D6&style=flat-square) ![Medium](https://img.shields.io/static/v1?label=Severity&message=Medium&color=FF9500&style=flat-square)

**Description**

A user can call `Controller.sync()` to update their vault's latest timestamp.


[code/contracts/core/Controller.sol#L439-L449](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/ee386cf23a1b3992c46a8d5ec85fbc216e80dea8/code/contracts/core/Controller.sol#L439-L449)
```solidity
    /**
     * @notice sync vault latest update timestamp
     * @dev anyone can update the latest time the vault was touched by calling this function
     * vaultLatestUpdate will sync if the vault is well collateralized
     * @param _owner vault owner address
     * @param _vaultId vault id
     */
    function sync(address _owner, uint256 _vaultId) external nonReentrant notFullyPaused {
        _verifyFinalState(_owner, _vaultId);
        vaultLatestUpdate[_owner][_vaultId] = now;
    }
```

This timestamp is important when a vault is liquidated. The method `MarginCalculator.isLiquidatable` needs the vault's timestamp to be in the past as compared to the current timestamp.


[code/contracts/core/MarginCalculator.sol#L445-L449](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/ee386cf23a1b3992c46a8d5ec85fbc216e80dea8/code/contracts/core/MarginCalculator.sol#L445-L449)
```solidity
        // check that price timestamp is after latest timestamp the vault was updated at
        require(
            timestamp > _vaultLatestUpdate,
            "MarginCalculator: auction timestamp should be post vault latest update"
        );
```

If the vault was at some point in the past liquidatable, but now it isn't anymore, it could be liquidated, unless the user (or a benefactor) updates the user's vault by calling `Controller.sync(address _owner, uint256 _vaultId)`.

Also, if anyone is watching the blockchain and they see a transaction in the mempool calling `Controller.sync()` they can assume (and verify) if that vault could be liquidated. They could front-run the `Controller.sync()` transaction and liquidate the vault before the timestamp is updated.

The method seems to be vulnerable to front-running attacks.

**Recommendation**

- Update timestamp before liquidating?

TBD

---


### [`notPartiallyPaused`, `notFullyPaused` and `onlyAuthorized` modifiers have unnecessary function counterparts](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/issues/8)
![Issue status: Open](https://img.shields.io/static/v1?label=Status&message=Open&color=5856D6&style=flat-square) ![Minor](https://img.shields.io/static/v1?label=Severity&message=Minor&color=FFCC00&style=flat-square)

**Description**

`notPartiallyPaused` modifier calls `_isNotPartiallyPaused` function in order to ensure that the state variable `systemPartiallyPaused` is not set to _true_:


[code/contracts/core/Controller.sol#L218-L221](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L218-L221)
```solidity
    modifier notPartiallyPaused {
        _isNotPartiallyPaused();

        _;
```


[code/contracts/core/Controller.sol#L277-L279](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L277-L279)
```solidity
    function _isNotPartiallyPaused() internal view {
        require(!systemPartiallyPaused, "C4");
    }
```

`_isNotPartiallyPaused` is an internal view function and is not called by any other function. This means that there is added gas costs but also mental energy expenditure for readers of the code and developers maintaining the contract.

The same logic applies to `notFullyPaused` and `onlyAuthorized` (they make use internal view functions which  are called only from the modifier).

**Recommendation**

Include the `require` logic directly in the modifiers and delete the obsolete internal functions.


---


### [Opening a vault type defaults to type zero](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/issues/3)
![Issue status: Open](https://img.shields.io/static/v1?label=Status&message=Open&color=5856D6&style=flat-square) ![Minor](https://img.shields.io/static/v1?label=Severity&message=Minor&color=FFCC00&style=flat-square)

**Description**

A user can open a vault by calling `Controller.operate()` with a list of actions. One of those actions can be an open vault action.


[code/contracts/core/Controller.sol#L426-L432](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L426-L432)
```solidity
    /**
     * @notice execute a number of actions on specific vaults
     * @dev can only be called when the system is not fully paused
     * @param _actions array of actions arguments
     */
    function operate(Actions.ActionArgs[] memory _actions) external nonReentrant notFullyPaused {
        (bool vaultUpdated, address vaultOwner, uint256 vaultId) = _runActions(_actions);
```

Each of those actions is executed in the controller, in the internal method `Controller._runActions`.

If the action type is equal to `Actions.ActionType.OpenVault`, a new vault is opened for the user.


[code/contracts/core/Controller.sol#L665-L666](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L665-L666)
```solidity
            if (actionType == Actions.ActionType.OpenVault) {
                _openVault(Actions._parseOpenVaultArgs(action));
```

But first the arguments needed are parsed by `Actions._parseOpenVaultArgs`.


[code/contracts/libs/Actions.sol#L184-L189](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/libs/Actions.sol#L184-L189)
```solidity
    /**
     * @notice parses the passed in action arguments to get the arguments for an open vault action
     * @param _args general action arguments structure
     * @return arguments for a open vault action
     */
    function _parseOpenVaultArgs(ActionArgs memory _args) internal pure returns (OpenVaultArgs memory) {
```

The vault type is encoded in the `_args.data` property.


[code/contracts/libs/Actions.sol#L196-L202](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/libs/Actions.sol#L196-L202)
```solidity
        if (_args.data.length == 32) {
            // decode vault type from _args.data
            vaultType = abi.decode(_args.data, (uint256));
        }

        // for now we only have 2 vault types
        require(vaultType < 2, "A3");
```

This value is checked to be valid (only type 0 and 1 accepted), but it defaults to 0. If the encoded sent data is invalid, the type defaults to 0. If the `_args.data.length` is not of size 32, the vault type defaults to 0.

**Recommendation**

It might help ensure the encoded data is valid by having a stricter validation of the input data.

A similar check is already done in `_parseLiquidateArgs`.


[code/contracts/libs/Actions.sol#L323](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/libs/Actions.sol#L323)
```solidity
        require(_args.data.length == 32, "A21");
```


---


### [External contract calls have unclear purpose](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/issues/7)
![Issue status: Open](https://img.shields.io/static/v1?label=Status&message=Open&color=5856D6&style=flat-square) ![Informational](https://img.shields.io/static/v1?label=Severity&message=Informational&color=34C759&style=flat-square)

**Description**

A user can call `operate` with a number of actions they want to execute in one transaction.


[code/contracts/core/Controller.sol#L426-L437](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L426-L437)
```solidity
    /**
     * @notice execute a number of actions on specific vaults
     * @dev can only be called when the system is not fully paused
     * @param _actions array of actions arguments
     */
    function operate(Actions.ActionArgs[] memory _actions) external nonReentrant notFullyPaused {
        (bool vaultUpdated, address vaultOwner, uint256 vaultId) = _runActions(_actions);
        if (vaultUpdated) {
            _verifyFinalState(vaultOwner, vaultId);
            vaultLatestUpdate[vaultOwner][vaultId] = now;
        }
    }
```

These actions can be very specific to the system currently being reviewed, but they can also be more generic. The generic actions can be specified by sending an action of type `Actions.ActionType.Call`.


[code/contracts/core/Controller.sol#L685-L687](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L685-L687)
```solidity
            } else if (actionType == Actions.ActionType.Call) {
                _call(Actions._parseCallArgs(action));
            }
```

The method `_call` does an external call to a specified contract.


[code/contracts/core/Controller.sol#L1037](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L1037)
```solidity
        CalleeInterface(_args.callee).callFunction(msg.sender, _args.data);
```

This can be used as a callback function to trigger external code the user needs in between the vault-related actions.

There is also a modifier on top of this method that only allows a specific set of external contracts or any contract.


[code/contracts/core/Controller.sol#L262-L272](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L262-L272)
```solidity
    /**
     * @notice modifier to check if the called address is a whitelisted callee address
     * @param _callee called address
     */
    modifier onlyWhitelistedCallee(address _callee) {
        if (callRestricted) {
            require(_isCalleeWhitelisted(_callee), "C3");
        }

        _;
    }
```

This functionality is significantly restricted in a few ways:

- `Controller` contract calls only a specific method of the destination contract `.callFunction`
- list of arguments is fixed and already set up `(msg.sender, _args.data)`

The result of the execution is not processed in any way. Whether the call fails or succeeds, the execution is not affected.

Consider the following setup which makes an external call and emits different events based on the execution result.

```solidity
contract External {
    bool public shouldFail;
    
    function setFail(bool _shouldFail) public {
        shouldFail = _shouldFail;
    }
    
    function maybeFail() public {
        if (shouldFail) {
            revert("Emitted error");
        }
    }
}

contract Caller {
    External public e;
    
    constructor () {
        e = new External();
    } 
    
    function callMaybeFail() external {
        try e.maybeFail() {
            emit Success();
        } catch Error(string memory _reason) {
            emit ResultString(false, _reason);
        } catch (bytes memory _lowLevelReason) {
            emit ResultBytes(false, _lowLevelReason);
        }
    }
    
    event ResultBytes(bool, bytes);
    event ResultString(bool, string);
    event Success();
}
```

The example above lets the contract behave differently based on the success of the execution. It is unclear whether this additional functionality is needed or required in the current context. Additional complexity needs to be added in order to complete the functionality.

Extreme care needs to be taken if the external call functionality is extended.

In its current form, it seems like a very limited type of functionality, but an extension of this functionality easily brings major security problems.

A clearer, more specific description of the required functionality needs to be reviewed before implementing anything else in this direction.

**Recommendation**

Consider reviewing a more specific implementation plan before going forward with this functionality.

---


### [Reduce the number of methods where possible](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/issues/6)
![Issue status: Open](https://img.shields.io/static/v1?label=Status&message=Open&color=5856D6&style=flat-square) ![Informational](https://img.shields.io/static/v1?label=Severity&message=Informational&color=34C759&style=flat-square)

**Description**

There are a few methods that are only called once.

- `_isNotPartiallyPaused`


[code/contracts/core/Controller.sol#L215-L222](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L215-L222)
```solidity
    /**
     * @notice modifier to check if the system is not partially paused, where only redeem and settleVault is allowed
     */
    modifier notPartiallyPaused {
        _isNotPartiallyPaused();

        _;
    }
```


[code/contracts/core/Controller.sol#L274-L279](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L274-L279)
```solidity
    /**
     * @dev check if the system is not in a partiallyPaused state
     */
    function _isNotPartiallyPaused() internal view {
        require(!systemPartiallyPaused, "C4");
    }
```

- `_isNotFullyPaused`


[code/contracts/core/Controller.sol#L224-L231](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L224-L231)
```solidity
    /**
     * @notice modifier to check if the system is not fully paused, where no functionality is allowed
     */
    modifier notFullyPaused {
        _isNotFullyPaused();

        _;
    }
```


[code/contracts/core/Controller.sol#L281-L286](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L281-L286)
```solidity
    /**
     * @dev check if the system is not in an fullyPaused state
     */
    function _isNotFullyPaused() internal view {
        require(!systemFullyPaused, "C5");
    }
```

- `_isAuthorized`


[code/contracts/core/Controller.sol#L251-L260](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L251-L260)
```solidity
    /**
     * @notice modifier to check if the sender is the account owner or an approved account operator
     * @param _sender sender address
     * @param _accountOwner account owner address
     */
    modifier onlyAuthorized(address _sender, address _accountOwner) {
        _isAuthorized(_sender, _accountOwner);

        _;
    }
```


[code/contracts/core/Controller.sol#L288-L295](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/core/Controller.sol#L288-L295)
```solidity
    /**
     * @dev check if the sender is an authorized operator
     * @param _sender msg.sender
     * @param _accountOwner owner of a vault
     */
    function _isAuthorized(address _sender, address _accountOwner) internal view {
        require((_sender == _accountOwner) || (operators[_accountOwner][_sender]), "C6");
    }
```

**Recommendation**

In order to reduce code complexity and reduce gas costs, the methods can be removed and their body can be added directly in the respective modifier.


---


### [EIP-2612 permit is likely to change](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/issues/2)
![Issue status: Open](https://img.shields.io/static/v1?label=Status&message=Open&color=5856D6&style=flat-square) ![Informational](https://img.shields.io/static/v1?label=Severity&message=Informational&color=34C759&style=flat-square)

**Description**

The contract `PermitCallee` uses the [EIP-2612: permit – 712-signed approvals](https://eips.ethereum.org/EIPS/eip-2612) standard to update the allowance for a user, using a provided signature.


[code/contracts/external/callees/PermitCallee.sol#L11-L31](https://github.com/monoceros-alpha/review-opyn-gamma-2021-08/blob/879977f600169390eb27a169b69ba7c60f7ee614/code/contracts/external/callees/PermitCallee.sol#L11-L31)
```solidity
/**
 * @title PermitCallee
 * @author Opyn Team
 * @dev Contract for executing permit signature
 */
contract PermitCallee is CalleeInterface {
    function callFunction(address payable _sender, bytes memory _data) external override {
        (
            address token,
            address owner,
            address spender,
            uint256 amount,
            uint256 deadline,
            uint8 v,
            bytes32 r,
            bytes32 s
        ) = abi.decode(_data, (address, address, address, uint256, uint256, uint8, bytes32, bytes32));

        IERC20PermitUpgradeable(token).permit(owner, spender, amount, deadline, v, r, s);
    }
}
```

This standard isn't officially recommended anymore and will likely change in the future. This might be because of [EIP-3074: AUTH and AUTHCALL opcodes](https://eips.ethereum.org/EIPS/eip-3074).

**Recommendation**

Consider deprecating or not relying too much on the currently implemented functionality and consider using the more flexible **EIP-3074: AUTH and AUTHCALL opcodes**.


---


## Artifacts

### Surya

Sūrya is a utility tool for smart contract systems. It provides a number of visual outputs and information about the structure of smart contracts. It also supports querying the function call graph in multiple ways to aid in the manual inspection and control flow analysis of contracts.

<!-- **Contracts Description Table**

```text
surya mdreport report.md Contract.sol
```

-->

**📘 Controller**

***Files Description Table***


| File Name           | SHA-1 Hash                               |
| ------------------- | ---------------------------------------- |
| core/Controller.sol | 66a9209d597b4d4ff528d72730cc3e25af33badc |


***Contracts Description Table***


|    Contract    |           Type           |                             Bases                             |                |                                          |
| :------------: | :----------------------: | :-----------------------------------------------------------: | :------------: | :--------------------------------------: |
|       └        |    **Function Name**     |                        **Visibility**                         | **Mutability** |              **Modifiers**               |
|                |                          |                                                               |                |                                          |
| **Controller** |      Implementation      | Initializable, OwnableUpgradeSafe, ReentrancyGuardUpgradeSafe |                |                                          |
|       └        |  _isNotPartiallyPaused   |                          Internal 🔒                           |                |                                          |
|       └        |    _isNotFullyPaused     |                          Internal 🔒                           |                |                                          |
|       └        |      _isAuthorized       |                          Internal 🔒                           |                |                                          |
|       └        |        initialize        |                          External ❗️                           |       🛑        |               initializer                |
|       └        |          donate          |                          External ❗️                           |       🛑        |                   NO❗️                    |
|       └        | setSystemPartiallyPaused |                          External ❗️                           |       🛑        |            onlyPartialPauser             |
|       └        |   setSystemFullyPaused   |                          External ❗️                           |       🛑        |              onlyFullPauser              |
|       └        |      setFullPauser       |                          External ❗️                           |       🛑        |                onlyOwner                 |
|       └        |     setPartialPauser     |                          External ❗️                           |       🛑        |                onlyOwner                 |
|       └        |    setCallRestriction    |                          External ❗️                           |       🛑        |                onlyOwner                 |
|       └        |       setOperator        |                          External ❗️                           |       🛑        |                   NO❗️                    |
|       └        |   refreshConfiguration   |                          External ❗️                           |       🛑        |                onlyOwner                 |
|       └        |       setNakedCap        |                          External ❗️                           |       🛑        |                onlyOwner                 |
|       └        |         operate          |                          External ❗️                           |       🛑        |       nonReentrant notFullyPaused        |
|       └        |           sync           |                          External ❗️                           |       🛑        |       nonReentrant notFullyPaused        |
|       └        |        isOperator        |                          External ❗️                           |                |                   NO❗️                    |
|       └        |     getConfiguration     |                          External ❗️                           |                |                   NO❗️                    |
|       └        |        getProceed        |                          External ❗️                           |                |                   NO❗️                    |
|       └        |      isLiquidatable      |                          External ❗️                           |                |                   NO❗️                    |
|       └        |        getPayout         |                           Public ❗️                            |                |                   NO❗️                    |
|       └        |   isSettlementAllowed    |                          External ❗️                           |                |                   NO❗️                    |
|       └        |     canSettleAssets      |                          External ❗️                           |                |                   NO❗️                    |
|       └        |  getAccountVaultCounter  |                          External ❗️                           |                |                   NO❗️                    |
|       └        |        hasExpired        |                          External ❗️                           |                |                   NO❗️                    |
|       └        |         getVault         |                          External ❗️                           |                |                   NO❗️                    |
|       └        |   getVaultWithDetails    |                           Public ❗️                            |                |                   NO❗️                    |
|       └        |       getNakedCap        |                          External ❗️                           |                |                   NO❗️                    |
|       └        |   getNakedPoolBalance    |                          External ❗️                           |                |                   NO❗️                    |
|       └        |       _runActions        |                          Internal 🔒                           |       🛑        |                                          |
|       └        |    _verifyFinalState     |                          Internal 🔒                           |                |                                          |
|       └        |        _openVault        |                          Internal 🔒                           |       🛑        |    notPartiallyPaused onlyAuthorized     |
|       └        |       _depositLong       |                          Internal 🔒                           |       🛑        |    notPartiallyPaused onlyAuthorized     |
|       └        |      _withdrawLong       |                          Internal 🔒                           |       🛑        |    notPartiallyPaused onlyAuthorized     |
|       └        |    _depositCollateral    |                          Internal 🔒                           |       🛑        |    notPartiallyPaused onlyAuthorized     |
|       └        |   _withdrawCollateral    |                          Internal 🔒                           |       🛑        |    notPartiallyPaused onlyAuthorized     |
|       └        |       _mintOtoken        |                          Internal 🔒                           |       🛑        |    notPartiallyPaused onlyAuthorized     |
|       └        |       _burnOtoken        |                          Internal 🔒                           |       🛑        |    notPartiallyPaused onlyAuthorized     |
|       └        |         _redeem          |                          Internal 🔒                           |       🛑        |                                          |
|       └        |       _settleVault       |                          Internal 🔒                           |       🛑        |              onlyAuthorized              |
|       └        |        _liquidate        |                          Internal 🔒                           |       🛑        |            notPartiallyPaused            |
|       └        |          _call           |                          Internal 🔒                           |       🛑        | notPartiallyPaused onlyWhitelistedCallee |
|       └        |      _checkVaultId       |                          Internal 🔒                           |                |                                          |
|       └        |       _isNotEmpty        |                          Internal 🔒                           |                |                                          |
|       └        |   _isCalleeWhitelisted   |                          Internal 🔒                           |                |                                          |
|       └        |     _isLiquidatable      |                          Internal 🔒                           |                |                                          |
|       └        |    _getOtokenDetails     |                          Internal 🔒                           |                |                                          |
|       └        |     _canSettleAssets     |                          Internal 🔒                           |                |                                          |
|       └        |  _refreshConfigInternal  |                          Internal 🔒                           |       🛑        |                                          |


**📘 MarginCalculator**

***Files Description Table***


| File Name                 | SHA-1 Hash                               |
| ------------------------- | ---------------------------------------- |
| core/MarginCalculator.sol | 3a4048d34b7a3cf47549e3e4d5b9712e23918f7f |


***Contracts Description Table***


|       Contract       |             Type             |     Bases      |                |               |
| :------------------: | :--------------------------: | :------------: | :------------: | :-----------: |
|          └           |      **Function Name**       | **Visibility** | **Mutability** | **Modifiers** |
|                      |                              |                |                |               |
| **MarginCalculator** |        Implementation        |    Ownable     |                |               |
|          └           |        <Constructor>         |    Public ❗️    |       🛑        |      NO❗️      |
|          └           |      setCollateralDust       |   External ❗️   |       🛑        |   onlyOwner   |
|          └           |     setUpperBoundValues      |   External ❗️   |       🛑        |   onlyOwner   |
|          └           |    updateUpperBoundValue     |   External ❗️   |       🛑        |   onlyOwner   |
|          └           |         setSpotShock         |   External ❗️   |       🛑        |   onlyOwner   |
|          └           |      setOracleDeviation      |   External ❗️   |       🛑        |   onlyOwner   |
|          └           |      getCollateralDust       |   External ❗️   |                |      NO❗️      |
|          └           |       getTimesToExpiry       |   External ❗️   |                |      NO❗️      |
|          └           |         getMaxPrice          |   External ❗️   |                |      NO❗️      |
|          └           |         getSpotShock         |   External ❗️   |                |      NO❗️      |
|          └           |      getOracleDeviation      |   External ❗️   |                |      NO❗️      |
|          └           |    getNakedMarginRequired    |   External ❗️   |                |      NO❗️      |
|          └           |     getExpiredPayoutRate     |   External ❗️   |                |      NO❗️      |
|          └           |        isLiquidatable        |   External ❗️   |                |      NO❗️      |
|          └           |      getMarginRequired       |   External ❗️   |                |      NO❗️      |
|          └           |     getExcessCollateral      |    Public ❗️    |                |      NO❗️      |
|          └           |     _getExpiredCashValue     |   Internal 🔒   |                |               |
|          └           |      _getMarginRequired      |   Internal 🔒   |                |               |
|          └           |   _getNakedMarginRequired    |   Internal 🔒   |                |               |
|          └           |     _findUpperBoundValue     |   Internal 🔒   |                |               |
|          └           | _getPutSpreadMarginRequired  |   Internal 🔒   |                |               |
|          └           | _getCallSpreadMarginRequired |   Internal 🔒   |                |               |
|          └           |  _convertAmountOnLivePrice   |   Internal 🔒   |                |               |
|          └           | _convertAmountOnExpiryPrice  |   Internal 🔒   |                |               |
|          └           |        _getDebtPrice         |   Internal 🔒   |                |               |
|          └           |       _getVaultDetails       |   Internal 🔒   |                |               |
|          └           |  _getExpiredSpreadCashValue  |   Internal 🔒   |                |               |
|          └           |         _isNotEmpty          |   Internal 🔒   |                |               |
|          └           |      _checkIsValidVault      |   Internal 🔒   |                |               |
|          └           |      _isMarginableLong       |   Internal 🔒   |                |               |
|          └           |   _isMarginableCollateral    |   Internal 🔒   |                |               |
|          └           |       _getProductHash        |   Internal 🔒   |                |               |
|          └           |        _getCashValue         |   Internal 🔒   |                |               |
|          └           |      _getOtokenDetails       |   Internal 🔒   |                |               |


**📘 PermitCallee**

***Files Description Table***


| File Name                         | SHA-1 Hash                               |
| --------------------------------- | ---------------------------------------- |
| external/callees/PermitCallee.sol | b98ff2a706037baaa339e0f86e309798092ad81e |


***Contracts Description Table***


|     Contract     |       Type        |      Bases      |                |               |
| :--------------: | :---------------: | :-------------: | :------------: | :-----------: |
|        └         | **Function Name** | **Visibility**  | **Mutability** | **Modifiers** |
|                  |                   |                 |                |               |
| **PermitCallee** |  Implementation   | CalleeInterface |                |               |
|        └         |   callFunction    |   External ❗️    |       🛑        |      NO❗️      |


**📘 MarginVault**

***Files Description Table***


| File Name            | SHA-1 Hash                               |
| -------------------- | ---------------------------------------- |
| libs/MarginVault.sol | 5f067b7b716b0645029104f6de80ba8dcd279418 |


***Contracts Description Table***


|    Contract     |       Type        |     Bases      |                |               |
| :-------------: | :---------------: | :------------: | :------------: | :-----------: |
|        └        | **Function Name** | **Visibility** | **Mutability** | **Modifiers** |
|                 |                   |                |                |               |
| **MarginVault** |      Library      |                |                |               |
|        └        |     addShort      |   External ❗️   |       🛑        |      NO❗️      |
|        └        |    removeShort    |   External ❗️   |       🛑        |      NO❗️      |
|        └        |      addLong      |   External ❗️   |       🛑        |      NO❗️      |
|        └        |    removeLong     |   External ❗️   |       🛑        |      NO❗️      |
|        └        |   addCollateral   |   External ❗️   |       🛑        |      NO❗️      |
|        └        | removeCollateral  |   External ❗️   |       🛑        |      NO❗️      |


***Legend***

| Symbol | Meaning                   |
| :----: | ------------------------- |
|   🛑    | Function can modify state |
|   💵    | Function is payable       |


#### Graphs

**📘 Controller**

**Execution Graph**

![Controller Graph](./static/Controller_graph.png)

**Inheritance**

![Controller Graph](./static/Controller_inheritance.png)

**📘 MarginCalculator**

**Execution Graph**

![MarginCalculator Graph](./static/MarginCalculator_graph.png)

**Inheritance**

![MarginCalculator Graph](./static/MarginCalculator_inheritance.png)

**📘 PermitCallee**

**Execution Graph**

![PermitCallee Graph](./static/PermitCallee_graph.png)

**Inheritance**

![PermitCallee Graph](./static/PermitCallee_inheritance.png)

**📘 MarginVault**

**Execution Graph**

![MarginVault Graph](./static/MarginVault_graph.png)

**Inheritance**

![MarginVault Graph](./static/MarginVault_inheritance.png)


<!-- ***Contract***

```text
surya graph Contract.sol | dot -Tpng > ./static/Contract_graph.png
```

![Contract Graph](./static/Contract_graph.png)

```text
surya inheritance Contract.sol | dot -Tpng > ./static/Contract_inheritance.png
```

![Contract Inheritance](./static/Contract_inheritance.png)

```text
Use Solidity Visual Auditor
```

![Contract UML](./static/Contract_uml.png) -->

#### Describe

**📘 Controller**

```text
$ npx surya describe contracts/core/Controller.sol
 +  Controller (Initializable, OwnableUpgradeSafe, ReentrancyGuardUpgradeSafe)
    - [Int] _isNotPartiallyPaused
    - [Int] _isNotFullyPaused
    - [Int] _isAuthorized
    - [Ext] initialize #
       - modifiers: initializer
    - [Ext] donate #
    - [Ext] setSystemPartiallyPaused #
       - modifiers: onlyPartialPauser
    - [Ext] setSystemFullyPaused #
       - modifiers: onlyFullPauser
    - [Ext] setFullPauser #
       - modifiers: onlyOwner
    - [Ext] setPartialPauser #
       - modifiers: onlyOwner
    - [Ext] setCallRestriction #
       - modifiers: onlyOwner
    - [Ext] setOperator #
    - [Ext] refreshConfiguration #
       - modifiers: onlyOwner
    - [Ext] setNakedCap #
       - modifiers: onlyOwner
    - [Ext] operate #
       - modifiers: nonReentrant,notFullyPaused
    - [Ext] sync #
       - modifiers: nonReentrant,notFullyPaused
    - [Ext] isOperator
    - [Ext] getConfiguration
    - [Ext] getProceed
    - [Ext] isLiquidatable
    - [Pub] getPayout
    - [Ext] isSettlementAllowed
    - [Ext] canSettleAssets
    - [Ext] getAccountVaultCounter
    - [Ext] hasExpired
    - [Ext] getVault
    - [Pub] getVaultWithDetails
    - [Ext] getNakedCap
    - [Ext] getNakedPoolBalance
    - [Int] _runActions #
    - [Int] _verifyFinalState
    - [Int] _openVault #
       - modifiers: notPartiallyPaused,onlyAuthorized
    - [Int] _depositLong #
       - modifiers: notPartiallyPaused,onlyAuthorized
    - [Int] _withdrawLong #
       - modifiers: notPartiallyPaused,onlyAuthorized
    - [Int] _depositCollateral #
       - modifiers: notPartiallyPaused,onlyAuthorized
    - [Int] _withdrawCollateral #
       - modifiers: notPartiallyPaused,onlyAuthorized
    - [Int] _mintOtoken #
       - modifiers: notPartiallyPaused,onlyAuthorized
    - [Int] _burnOtoken #
       - modifiers: notPartiallyPaused,onlyAuthorized
    - [Int] _redeem #
    - [Int] _settleVault #
       - modifiers: onlyAuthorized
    - [Int] _liquidate #
       - modifiers: notPartiallyPaused
    - [Int] _call #
       - modifiers: notPartiallyPaused,onlyWhitelistedCallee
    - [Int] _checkVaultId
    - [Int] _isNotEmpty
    - [Int] _isCalleeWhitelisted
    - [Int] _isLiquidatable
    - [Int] _getOtokenDetails
    - [Int] _canSettleAssets
    - [Int] _refreshConfigInternal #


 ($) = payable function
 # = non-constant function
```

**📘 MarginCalculator**

```text
$ npx surya describe ./core/MarginCalculator.sol 
 +  MarginCalculator (Ownable)
    - [Pub] <Constructor> #
    - [Ext] setCollateralDust #
       - modifiers: onlyOwner
    - [Ext] setUpperBoundValues #
       - modifiers: onlyOwner
    - [Ext] updateUpperBoundValue #
       - modifiers: onlyOwner
    - [Ext] setSpotShock #
       - modifiers: onlyOwner
    - [Ext] setOracleDeviation #
       - modifiers: onlyOwner
    - [Ext] getCollateralDust
    - [Ext] getTimesToExpiry
    - [Ext] getMaxPrice
    - [Ext] getSpotShock
    - [Ext] getOracleDeviation
    - [Ext] getNakedMarginRequired
    - [Ext] getExpiredPayoutRate
    - [Ext] isLiquidatable
    - [Ext] getMarginRequired
    - [Pub] getExcessCollateral
    - [Int] _getExpiredCashValue
    - [Int] _getMarginRequired
    - [Int] _getNakedMarginRequired
    - [Int] _findUpperBoundValue
    - [Int] _getPutSpreadMarginRequired
    - [Int] _getCallSpreadMarginRequired
    - [Int] _convertAmountOnLivePrice
    - [Int] _convertAmountOnExpiryPrice
    - [Int] _getDebtPrice
    - [Int] _getVaultDetails
    - [Int] _getExpiredSpreadCashValue
    - [Int] _isNotEmpty
    - [Int] _checkIsValidVault
    - [Int] _isMarginableLong
    - [Int] _isMarginableCollateral
    - [Int] _getProductHash
    - [Int] _getCashValue
    - [Int] _getOtokenDetails


 ($) = payable function
 # = non-constant function
```

**📘 PermitCallee**

```text
$ npx surya describe ./external/callees/PermitCallee.sol 
 +  PermitCallee (CalleeInterface)
    - [Ext] callFunction #


 ($) = payable function
 # = non-constant function
```

**📘 MarginVault**

```text
$ npx surya describe ./libs/MarginVault.sol
 + [Lib] MarginVault 
    - [Ext] addShort #
    - [Ext] removeShort #
    - [Ext] addLong #
    - [Ext] removeLong #
    - [Ext] addCollateral #
    - [Ext] removeCollateral #


 ($) = payable function
 # = non-constant function
```

<!-- ```text
$ npx surya describe ./Contract.sol
``` -->

### Coverage

<!-- ```text
$ npm run coverage
``` -->

### Tests

<!-- ```text
$ npx buidler test
``` -->

## License

This report falls under the terms described in the included [LICENSE](./LICENSE).

<!-- Load highlight.js -->
<link rel="stylesheet"
href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/10.4.1/styles/default.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/10.4.1/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/highlightjs-solidity@1.0.20/solidity.min.js"></script>
<script type="text/javascript">
    hljs.registerLanguage('solidity', window.hljsDefineSolidity);
    hljs.initHighlightingOnLoad();
</script>
<link rel="stylesheet" href="./style/print.css"/>
