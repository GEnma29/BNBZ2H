## DogCoin contract

<aside>
💡

Homework 4
Solidity
To this contract

1. Add a variable to hold the address of the deployer of the contract
2. Update that variable with the deployer's address when the contract is
deployed.
. Write an external function to return
3. Address 0x000000000000000000000000000000000000dEaD if called by
the deployer
4. The deployer's address otherwise
DogCoin contract
5. In Remix, create a new file called DogCoin.sol .
6. Define the pragma compiler version to 0.8.18 .
. Before the pragma version, add a license identifier
// SPDX-License-Identifier: UNLICENSED .
. Create a contract called DogCoin.
. Create a variable to hold the total supply, with an initial amount of 2 million.
. Make a public function that returns the total supply.
. Make a public function that can increase the total supply in steps of 1000.
. Declare an address variable called owner . this address will be allowed to
change the total supply
. Next, create a modifier which only allows an owner to execute certain
functions.
7. Make your change total supply function public , but add your modifier so
that only the owner can execute it.
8. Create a constructor to initialise the state of the contract and within the
constructor, store the owner's address.
9. Create an event that emits the new value whenever the total supply changes.
When the supply changes, emit this event.
10. In order to keep track of user balances, we need to associate a user's
address with the balance that they have.
a) What is the best data structure to hold this association ?
b) Using your choice of data structure, set up a variable called balances to
keep track of the number of tokens that a user has.
11. We want to allow the balances variable to be read from the contract, there
are 2 ways to do this.
What are those ways ?
Use one of the ways to make your balances variable visible to users of the
contract.
12. Now change the constructor, to give all of the total supply to the owner of
the contract.
13. Now add a public function called transfer to allow a user to transfer their
tokens to another address. This function should have 2 parameters :
Why do we not need the sender's address here ?
What would be the implication of having the sender's address as a parameter ?
14. Add an event to the transfer function to indicate that a transfer has taken
place, it should log the amount and the recipient address.
15. We want to keep a record for each user's transfers. Create a struct called
Payment that stores the transfer amount and the recipient's address.
16. We want to have a payments array for each user sending the payment.
Create a mapping which returns an array of Payment structs when given this
user's address.
Resources
</aside>


## 👀 Solution
```ts 
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts@4.8.2/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts@4.8.2/access/Ownable.sol";

contract DogCoin is ERC20, Ownable {

      struct Payment {
        address _address;
        uint256 amount;
    }
    mapping(address => Payment[]) public transactions;

    Payment[] public payments;

    constructor() ERC20("DogCoin", "DC") {
        _mint(msg.sender, 2000000 * 10 ** decimals());
    }

    mapping(address => uint) balances;
    // events 

    event incrementTotalSupplyEvent(address indexed owner, uint256 totalSupply);

    event makeTransfer(address to, uint256 amount);


    function getTotalSupply() public  view returns (uint256) {
         return totalSupply();
    }

    function incrementTotalSupply() external onlyOwner()  returns (uint256){
         _mint(msg.sender, 1000 * 10 ** decimals());
         emit incrementTotalSupplyEvent(owner(), totalSupply());
         return totalSupply();
    }
    function customTransfer(address to, uint256 amount) public  returns (bool){
        transfer(to,amount);
        emit makeTransfer(to, amount);
        transactions[msg.sender].push(Payment({_address: to, amount:amount}));
        payments.push(Payment({_address: to, amount:amount}));
        return true;
    }
    function getAllTransactions() public view returns (Payment[] memory) {
        return payments;
    }

}
```
