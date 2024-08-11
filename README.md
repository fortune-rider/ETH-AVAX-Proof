# ETH-AVAX-Proof

This is a simple program that shows the working of some error handling functions in solidity such as require(), revert() and assert(). This program is a simple contract written in Solidity, a programming language used for developing smart contracts on the Ethereum blockchain. The contract shows the working of a few functions. This program serves as a simple introduction on how to execute the functions on the blockchain using solidity and can be used as a stepping stone for more complex projects in the future.

## Description

For this assessment, I have created a contract that does the following functions :

1. There is a mint function that works only if the condition ((msg.sender)==owner) is true.
2. The next function we have is a burn function which uses a modifier to ensure only the owner can burn the tokens.
   
This assessment is focused on familiarising with the different functions that can be done to the tokens we have using solidity. It is one of the simplest solidity programs that helps us understand and gain knowledge about this language and about the working of tokens in the world of cryptocurrency. This is just a demonstration of minting and burning of tokens and is not the right way to do it.

## Getting Started

### Executing Program
* Running the code
  
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at (https://remix.ethereum.org/). Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension (eg. Functions.sol). Copy and paste the following code into the file:

```javascript

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract MyToken {

    constructor() {
        owner=msg.sender;
    }

    //public variables
    string public name = "simple";
    string public symbol = "SIMP";
    uint public totalSupply = 0;
    address public owner;

    //emits Events
    event Mint(address indexed to, uint amount);
    event Burn(address indexed from, uint amount);

    // custom error
    error InsufficientBalance(uint balance, uint withdrawAmount);

    // mapping variable here
    mapping(address => uint) public balances;

    //Modifiers
    modifier onlyOwner {
        assert(msg.sender==owner);
        _;
    }
    // mint function
    function mint (address _address, uint _value) public{
        require(msg.sender==owner, "Not Authorised");
        totalSupply += _value;
        balances[_address] += _value;
        emit Mint(_address, _value);
    }

    // burn function
    function burn (address _address, uint _value) public onlyOwner {
        if(balances[_address] < _value) {
            revert InsufficientBalance({balance: balances[_address], withdrawAmount: _value});
        }
        else{
            totalSupply -= _value;
            balances[_address] -= _value;
            emit Burn(_address, _value);
        }
    }

}

```

* Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. Select your contract from the dropdown menu, and then click on the "Deploy" button.
* Scroll down and you will be able to see the name of your functions and your variables. Go to the "supply and "balance" variables and make sure they are 0 at the beginning of the deployment.
* Next go to the mint function and input the owner address and the number of wei to be minted. Now if you click on the "balances" and "supply" buttons you will be able to see the number of wei which you have minted. Now if yout ry to input any address other than the owner address, then it will return a error message because of the require() function used in this section.
* Now we have the burn function which burns or destroys the tokens.Here there are two conditions to execute this function. First is the modifier connected to this function which asserts that the msg.sender==owner using the assert(). The next condition is that the number of wei burned is less than the balance, otherwise the rest of the code is not executed to save gas using the revert() function. If both the conditions are satisfied then the inputed number of tokens are burned.
