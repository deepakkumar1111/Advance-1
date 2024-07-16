# AVAX-PROOF-Module-1
# DeFi Kingdom Clone on Avalanche

Welcome to the thrilling world of blockchain-based gaming! This project is a DeFi Kingdom clone built on the Avalanche blockchain. Players can collect, build, and battle with digital assets, earning rewards through various in-game activities.

## Getting Started

### Prerequisites

- Unix Computer (MacOS or Linux) or WSL in Windows 
- Remix IDE 
- Metamask

### Setting Up Your EVM Subnet on Avalanche

1. Deploy your custom EVM subnet using the Avalanche CLI. Refer to the guide and Avalanche documentation for detailed instructions.

2. Define your native currency as an ERC20 token. This will be the in-game currency for your DeFi Kingdom clone.

3. Connect your EVM subnet to Metamask.

| **SUBNET** | **CHAIN** | **CHAINID** | **VMID**                                          | **TYPE**    |
|------------|-----------|-------------|---------------------------------------------------|-------------|
| mySubnet   | mySubnet  | 12345567    | qDN9XsRy6efv5tZw3q2ngfQzQZJyMQJEF3d3Nu4YisNi9iR4G | Subnet-EVM  |
(This is a sample you can change the values of subnet, chain, chainid)
### Deploying Smart Contracts

#### ERC20 Token Contract

```solidity
// ERC20.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
}

contract ERC20 is IERC20 {
    string public name = "GOPALYDY";
    string public symbol = "GYD";
    uint8 public decimals = 18;
    uint public override totalSupply;
    mapping(address => uint) public override balanceOf;
    mapping(address => mapping(address => uint)) public override allowance;

    function transfer(address recipient, uint amount) external override returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external override returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external override returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```
# Vault.sol File
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;

    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }

    function deposit(uint _amount) external {
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }

        _mint(msg.sender, shares);
        require(token.transferFrom(msg.sender, address(this), _amount), "Transfer failed");
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        require(token.transfer(msg.sender, amount), "Transfer failed");
    }
}
```
# Author
Deepak Kumar

