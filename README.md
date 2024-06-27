# MyToken ERC20 Token

## Overview

This is a basic implementation of an ERC20 token named "My Token" with the symbol "MTK" and 18 decimals. The contract includes functionality for transferring tokens, approving token allowances, minting new tokens, and burning existing tokens. The owner of the contract has special privileges to mint new tokens.

## Features

- **Name:** My Token
- **Symbol:** MTK
- **Decimals:** 18
- **Initial Supply:** Specified during contract deployment

## Contract Details

### State Variables

- `string public name`: The name of the token.
- `string public symbol`: The symbol of the token.
- `uint8 public decimals`: The number of decimals the token uses.
- `uint256 public totalSupply`: The total supply of the token.
- `mapping(address => uint256) public balanceOf`: Balances of each account.
- `mapping(address => mapping(address => uint256)) public allowance`: Allowance for each account.
- `address public owner`: The owner of the contract.

### Events

- `event Transfer(address indexed from, address indexed to, uint256 value)`: Emitted when tokens are transferred.
- `event Approval(address indexed owner, address indexed spender, uint256 value)`: Emitted when an approval is made.
- `event Mint(address indexed to, uint256 value)`: Emitted when new tokens are minted.
- `event Burn(address indexed from, uint256 value)`: Emitted when tokens are burned.

### Modifiers

- `modifier onlyOwner()`: Ensures that the function can only be called by the owner.

### Constructor

```solidity
constructor(uint256 _initialSupply) {
    owner = msg.sender;
    totalSupply = _initialSupply * 10**uint256(decimals);
    balanceOf[msg.sender] = totalSupply;
}
```
- Initializes the contract with an initial supply and assigns it to the deployer of the contract.

### Functions

#### `transfer`

```solidity
function transfer(address _to, uint256 _value) external returns (bool) {
    require(_to != address(0), "Invalid recipient address");
    require(balanceOf[msg.sender] >= _value, "Insufficient balance");
    balanceOf[msg.sender] -= _value;
    balanceOf[_to] += _value;
    emit Transfer(msg.sender, _to, _value);
    return true;
}
```

- Transfers tokens from the caller to another address.

#### `approve`

```solidity
function approve(address _spender, uint256 _value) external returns (bool) {
    allowance[msg.sender][_spender] = _value;
    emit Approval(msg.sender, _spender, _value);
    return true;
}
```

- Approves an allowance for a spender.

#### `transferFrom`

```solidity
function transferFrom(address _from, address _to, uint256 _value) external returns (bool) {
    require(_from != address(0), "Invalid sender address");
    require(_to != address(0), "Invalid recipient address");
    require(balanceOf[_from] >= _value, "Insufficient balance");
    require(allowance[_from][msg.sender] >= _value, "Allowance exceeded");
    balanceOf[_from] -= _value;
    balanceOf[_to] += _value;
    allowance[_from][msg.sender] -= _value;
    emit Transfer(_from, _to, _value);
    return true;
}
```

- Transfers tokens from one address to another using an allowance.

#### `mint`

```solidity
function mint(address _to, uint256 _value) external onlyOwner {
    require(_to != address(0), "Invalid address");
    totalSupply += _value;
    balanceOf[_to] += _value;
    emit Mint(_to, _value);
    emit Transfer(address(0), _to, _value);
}
```

- Mints new tokens and assigns them to a specified address. Can only be called by the owner.

#### `burn`

```solidity
function burn(uint256 _value) external {
    require(balanceOf[msg.sender] >= _value, "Insufficient balance");
    totalSupply -= _value;
    balanceOf[msg.sender] -= _value;
    emit Burn(msg.sender, _value);
    emit Transfer(msg.sender, address(0), _value);
}
```

- Burns a specified amount of tokens from the caller's balance.

## Deployment

To deploy this contract, you will need to provide the initial supply of tokens as a parameter to the constructor.

Example deployment using Remix IDE:

1. Copy the contract code into Remix IDE.
2. Compile the contract using Solidity compiler version 0.8.0 or above.
3. Deploy the contract, specifying the initial supply in the deployment parameters.

## Ownwer

Yash Dhiman

## License

This project is licensed under the MIT License.
