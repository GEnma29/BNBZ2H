# Badger NFT
We now want to create an NFT.
You can use the Open Zeppelin libraries to help with this.
1. Create a new project in the IDE of your choice
2. Create a BadgerNFT contract this should inherit from any ERC721
implementation from the Open Zeppelin standard libraries
3. Give your NFT a name and a symbol.
4. Write unit tests to check that you can
5. Deploy your contract to the test network and send some NFTs to your
colleagues.

For now we won't associate a digital asset with our token.

```ts 
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract BadgerNFT is ERC721, Ownable {
    using Counters for Counters.Counter;

    Counters.Counter private _tokenIdCounter;

    constructor() ERC721("BadgerNFT", "BC") {}

    function _baseURI() internal pure override returns (string memory) {
        return "https://ibb.co/2S1ZdvX";
    }

    function safeMint(address to) public onlyOwner {
        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
    }
}



```

## test file 

```ts 

const { expect } = require("chai");

const BadgerNFT = artifacts.require("BadgerNFT");

contract("BadgerNFT", async (accounts) => {
  const [alice, bob] = accounts;
  let NFTInstance;
  beforeEach(async () => {
    NFTInstance = await BadgerNFT.new();
  });
  it("should return correct name", async () => {
    const name = await NFTInstance.name();
    expect("BadgerNFT").to.equal(name);
  });
  it("Can Owner transfer NFT", async () => {
  const transfer = await NFTInstance.transferOwnership(bob)
  const {receipt} = transfer
  expect(true).to.equal(receipt.status);
  });
  it("Can transfer  NFT", async () => {
    const transfer = await NFTInstance.transferOwnership(bob)
    const {receipt} = transfer
    expect(true).to.equal(receipt.status);
    });

});



```
