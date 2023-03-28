## Homework 10 

For the Dog coin contract you created
1. Add an internal function called _mint to increase the total supply,
any increase in supply should be given to the contract owner.
2. Add some unit tests to test the following
1. The total supply can be increased in steps of 1000
2. Only the owner can increase the total supply
3. The correct events are produced when transfers occur.

```ts

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract BadgerCoin is ERC20, Ownable {
    constructor() ERC20("BadgerCoin", "BC") {
          _mint(msg.sender,  1000000 * 10 ** decimals());
    }
    function increment(uint256 supply) public onlyOwner returns (uint256) {
        _mint(msg.sender,supply);
        return totalSupply();
    }
}


```

## test 

```ts 
import { time, loadFixture } from "@nomicfoundation/hardhat-network-helpers";
import { anyValue } from "@nomicfoundation/hardhat-chai-matchers/withArgs";
import { expect, assert } from "chai";
import { ethers } from "hardhat";
import { getAddress } from "../utilities/get-address-to.utilities";

describe("BadgerCoin", function (){

  async function deployOneYearLockFixture() {

    const testSupply = 1000000000000000000000000n
    const decimals = 18;
    function toWei(count: number) {
      return count * 10 ** 18
    }

    // Contracts are deployed using the first signer/account by default
    const [owner, otherAccount] = await ethers.getSigners();

    const BadgerCoin = await ethers.getContractFactory("BadgerCoin");
    const badgerCoin = await BadgerCoin.deploy();

    return { badgerCoin,decimals, owner, otherAccount, testSupply , toWei };
  }
  describe("Deployment", function () {
    it("Should set the right supply", async function () {
      const { badgerCoin, testSupply } = await loadFixture(deployOneYearLockFixture);
      const supply = await badgerCoin.totalSupply()
      console.log(supply)
      expect(await badgerCoin.totalSupply()).to.equal(testSupply);
    });
    it("Should set the right decimals", async function () {
      const { badgerCoin, decimals } = await loadFixture(deployOneYearLockFixture);
      expect(await badgerCoin.decimals()).to.equal(decimals);
    });
    it("Should balanceOf function returns the correct result", async function () {
      const { badgerCoin, owner, testSupply } = await loadFixture(deployOneYearLockFixture);
      expect(await badgerCoin.balanceOf(owner.address)).to.equal(testSupply);
    });
    it("Should make a transaction", async function () {
      const { badgerCoin, owner, otherAccount , toWei } = await loadFixture(deployOneYearLockFixture);
     
        const amount = toWei(0)
        const {from, v,data, to} = await badgerCoin.transfer(otherAccount.address, amount)
        const address = getAddress(data)
        expect(from).to.equal(owner.address)
        expect(address).to.equal(otherAccount.address.toLocaleLowerCase())
        expect(v).to.equal(amount)
    })
    it("Should increment the supply  a transaction", async function () {
      const { badgerCoin, owner, otherAccount , toWei } = await loadFixture(deployOneYearLockFixture);
     
        const amount = toWei(1)
        const transfer = await badgerCoin.increment(100)
        console.log(transfer)
    })
    });
  })



```
