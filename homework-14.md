## NFT Storage
- create account on [NFT Storage](https://nft.storage)
- Upload file 
- copy CDI and paste on NFT contract 

## our NFT contract 

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
        return "bafkreicbywbrxqw47qblknkunivsxc6hrjbukbl7jt3cnqo6lm35v3dnle";
    }

    function safeMint(address to) public onlyOwner {
        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
    }
}

```
