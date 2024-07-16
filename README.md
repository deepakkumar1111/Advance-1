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

# Author
Deepak Kumar

