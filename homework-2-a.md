<aside>
💡

1. Add a variable to hold the address of the deployer of the contract
2. Update that variable with the deployer's address when the contract is
deployed.
. Write an external function to return
3. Address 0x000000000000000000000000000000000000dEaD if called by
the deployer
4. The deployer's address otherwise
</aside>

# 👀 Solution

```ts
contract BootcampContract {
    address public owner;
    address public newAddress = 0x000000000000000000000000000000000000dEaD;
    uint256 number;

    constructor() { owner = msg.sender;}


    function store(uint256 num) public {
        number = num;
    }

    function retrieve() public view returns (uint256){
        return number;
    }

    function getNewAddres() external view  returns (address) {
        return newAddress;
    }
```
