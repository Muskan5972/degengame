# degengame
A cutting-edge decentralized gaming platform built on the Avalanche blockchain is called Degengame. The in-game economy is powered by the Degen token, an ERC20-compliant digital asset that makes it simple for players to acquire, exchange, and redeem tokens. Created in Solidity, the DeGame smart contract was deployed on the Avalanche blockchain. Through the use of Ownable and ERC20 standards from OpenZeppelin, the contract offers dependability and security.
## Description
Avalanche blockchain serves as the foundation for the innovative decentralized gaming platform DeGame. Blockchain technology, which offers security, transparency, and real ownership of digital assets, is being incorporated into the platform with the goal of revolutionizing online gaming. The Degen token (DGN), an electronic money that complies with ERC20 and powers all in-game exchanges and interactions, is the center of DeGame's economy.
## Key Features

### 1. Degen Token Integration

- *Token*: Degen
- *Symbol*: DGN
- *Decimals*: 0
- *Role*: The Degen token is the primary medium of exchange within the DeGame ecosystem. Players earn DGN tokens as rewards, use them to purchase in-game items, and redeem them for various benefits.

## Getting Started

To execute this program, you can use the Remix IDE, which is an open IDE for Solidity. Visit [Remix IDE](https://remix.ethereum.org/) to get started.

## Executing Program

1. *Create a New File*:
    - Click on the '+' icon and save the file with a .sol extension (e.g., DeGame.sol).
    - Copy and paste the following code into your file:

solidity

// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenToken is ERC20, Ownable {

    struct Item {
        uint id;
        string name;
        uint price;
        bool available;
    }

    mapping(uint => Item) private items;
    uint private itemCount;

    mapping(address => Item[]) private redeemedItems;

    event ItemListed(uint indexed id, string name, uint price);
    event ItemRedeemed(address indexed account, uint indexed itemId, string itemName, uint itemPrice);

    constructor(address initialOwner) ERC20("Degen", "DGN") Ownable(initialOwner) {
    }

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    function transfer(address to, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), to, amount);
        return true;
    }

    function redeem(uint itemId) public {
        require(itemId > 0 && itemId <= itemCount, "Invalid item ID");
        require(items[itemId].available, "Item not available");
        require(balanceOf(msg.sender) >= items[itemId].price, "Insufficient balance");

        _transfer(msg.sender, owner(), items[itemId].price);
        redeemedItems[msg.sender].push(items[itemId]);
        items[itemId].available = false;

        emit ItemRedeemed(msg.sender, itemId, items[itemId].name, items[itemId].price);
    }

    function listItem(string memory name, uint price) public onlyOwner {
        itemCount++;
        items[itemCount] = Item(itemCount, name, price, true);

        emit ItemListed(itemCount, name, price);
    }
    function _addInitialItems() internal onlyOwner {
        listItem("Sword", 100 * 10 ** 18);
        listItem("Water Shield", 150 * 10 ** 18);
        listItem("Health Potion", 50 * 10 ** 18);
        listItem("Cape", 50 * 10 ** 18);
    }

    function getItem(uint itemId) public view returns (Item memory) {
        require(itemId > 0 && itemId <= itemCount, "Invalid item ID");
        return items[itemId];
    }

    function getItemCount() public view returns (uint) {
        return itemCount;
    }

    function checkBalance(address account) public view returns (uint256) {
        return balanceOf(account);
    }

    function burn(uint256 amount) public {
        require(amount > 0, "Amount must be greater than zero");
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");

        _burn(msg.sender, amount);
    }

    function getRedeemedItems(address account) public view returns (Item[] memory) {
        return redeemedItems[account];
    }
}

After pasting this code, you must compile it from the left-hand sidebar. Click on the 'Solidity Compiler', then the 'Compile DeGame.sol' button.

After successfully compiling the code, you must deploy the software. You have another choice on the left sidebar called 'Deploy & Run Transactions', and then you will notice a deploy button; before clicking on it, ensure that the file displayed is 'DeGame.sol'. Then you will be able to see the file in the 'Deployed/Unpinned Contracts' section. Click on it, and all of the public variables and functions will be exposed to you. Execute and fetch the data as desired. Also, keep in mind that you'll see the function here have neither written nor implemented these extra functions; they are functions of the libraries we have inherited (ERC20 and ownable), and it is up to you whether or not you utilize them.

This allows you to deploy the contract locally, but to deploy it on avalanche, you must first have enough avax tokens in your wallet and then connect your wallet to the remix IDE. To do so, return to the deploy & run transaction bar and pick Injected Provider from the Environment menu. Then, grant authorization from your wallet to connect to the IDE and click deploy. Once the deployment is completed, copy the contract address and navigate to https://testnet.snowtrace.io/ and then paste the address into the search field and hit Enter. Now that everything has been completed and all connections have been successfully established, you can create transactions in the remix IDE that will be viewable on Snowtrace. Now you can execute all of the functions on your own.

## Author
Muskan @Muskan5972
## License
This project is licensed under the MIT license.
