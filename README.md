# ERC20 TOKEN - DEGENTOKEN

This project is an implementation of an ERC20 token named "DegenToken" on the Ethereum blockchain. The token allows users to mint, transfer, and burn tokens. Additionally, users can redeem tokens for in-game items such as CRYSTAL, GEM, PLATINUM, and METAL.

## Description

The DegenToken project is a smart contract built on the Ethereum blockchain using the Solidity programming language. It follows the ERC20 token standard, providing basic functionalities such as minting, transferring, and burning tokens. Moreover, it includes a system where users can redeem their tokens for specific in-game items, enhancing the utility of the token within a gaming ecosystem. This project is intended for use in decentralized applications (DApps) and showcases the integration of tokenomics within a blockchain-based environment.

## Getting Started

### Installing

To download and run the project:

1. Clone the repository from GitHub: [DegenToken GitHub Repository](https://github.com/DevSGitub2/ETH-AVAX-Module-4.git)
2. Navigate to the project directory.
3. Ensure you have MetaMask installed and connected to the Ethereum network.

### Executing program

To deploy and interact with the DegenToken contract:

1. Open the project in [Remix IDE](https://remix.ethereum.org/)
2. Create a new file and paste the Solidity code for the DegenToken contract.
3. Compile the contract using the Solidity compiler in Remix.
4. Deploy the contract using your MetaMask account.
5. After deployment, you can interact with the contract's functions such as `mint`, `transferTokens`, and `redeemItem`.

### Showcasing Transaction History

- Once the contract is deployed, you can view and verify the transactions on [Snowtrace](https://snowtrace.io/) by searching for your contract address.

## Solidity Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract DegenToken is ERC20, Ownable, ERC20Burnable {
    struct PlayerItems {
        uint256 CRYSTAL;
        uint256 GEM;
        uint256 PLATINUM;
        uint256 METAL;
    }

    mapping(address => PlayerItems) public playerItems;

    uint256 public constant CRYSTAL = 1;
    uint256 public constant GEM = 2;
    uint256 public constant PLATINUM = 3;
    uint256 public constant METAL = 4;

    uint256 public constant CRYSTAL_PRICE = 100;
    uint256 public constant GEM_PRICE = 200;
    uint256 public constant PLATINUM_PRICE = 300;
    uint256 public constant METAL_PRICE = 400;

    event ItemRedeemed(address indexed player, uint256 itemId, uint256 price);
    event TokensTransferred(address indexed from, address indexed to, uint256 amount);
    event TokensBurned(address indexed account, uint256 amount);

    constructor(address initialOwner) ERC20("Degen", "DGN") Ownable(initialOwner) {}

    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
    }

    function transferTokens(address to, uint256 amount) public {
        require(amount <= balanceOf(msg.sender), "Low degen balance");
        _transfer(msg.sender, to, amount);
        emit TokensTransferred(msg.sender, to, amount);
    }

    function redeemItem(uint256 itemId) public {
        uint256 price;

        if (itemId == CRYSTAL) {
            price = CRYSTAL_PRICE;
            playerItems[msg.sender].CRYSTAL += 1;
        } else if (itemId == GEM) {
            price = GEM_PRICE;
            playerItems[msg.sender].GEM += 1;
        } else if (itemId == PLATINUM) {
            price = PLATINUM_PRICE;
            playerItems[msg.sender].PLATINUM += 1;
        } else if (itemId == METAL) {
            price = METAL_PRICE;
            playerItems[msg.sender].METAL += 1;
        } else {
            revert("Invalid item ID");
        }

        require(balanceOf(msg.sender) >= price, "Insufficient balance");
        _burn(msg.sender, price);

        emit ItemRedeemed(msg.sender, itemId, price);
    }

    function checkBalance() external view returns (uint256) {
        return balanceOf(msg.sender);
    }

    function getItemCount(address player, uint256 itemId) external view returns (uint256) {
        if (itemId == CRYSTAL) {
            return playerItems[player].CRYSTAL;
        } else if (itemId == GEM) {
            return playerItems[player].GEM;
        } else if (itemId == PLATINUM) {
            return playerItems[player].PLATINUM;
        } else if (itemId == METAL) {
            return playerItems[player].METAL;
        } else {
            revert("Invalid item ID");
        }
    }

    function listRedeemableItems() external pure returns (string memory) {
        return "1. CRYSTAL = 100 DGN\n2. GEM = 200 DGN\n3. PLATINUM = 300 DGN\n4. METAL = 400 DGN";
    }
}

```
Refer to Remix's documentation for additional help: https://remix-ide.readthedocs.io/

## Authors

Dev Sagar  
GitHub: [DevSGithub2](https://github.com/DevSGitub2)

## License

This project is licensed under the MIT License - see the LICENSE.md file for details.




