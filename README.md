# BaseMeme
solidity to deploy contract token MEME on base 
mkdir meme-token
cd meme-token
npm init -y
npm install --save-dev hardhat
npm install @openzeppelin/contracts
npx hardhat
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MemeToken is ERC20, Ownable {
    constructor(uint256 initialSupply) ERC20("MemeToken", "MEME") {
        _mint(msg.sender, initialSupply);
    }

    // Дополнительные функции можно добавить здесь
}
Объяснение кода:
ERC20: Используем стандартный токен ERC-20 из OpenZeppelin.
Ownable: Добавляем возможность назначения владельца контракта.
_mint: При создании контракта выпускается определенное количество токенов и отправляется на адрес создателя контракта.
Шаг 3: Настройка сети Base в Hardhat
require("@nomiclabs/hardhat-waffle");

module.exports = {
  solidity: "0.8.0",
  networks: {
    base: {
      url: "https://base-goerli.blockchainURL", // Укажите фактический URL блокчейна Base (например, Base Goerli Testnet)
      accounts: ["0xYOUR_PRIVATE_KEY"] // Замените на ваш приватный ключ
    }
  }
};
Создайте файл скрипта для развертывания в директории scripts, например deploy.js:// scripts/deploy.js
async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying contracts with the account:", deployer.address);

  const initialSupply = ethers.utils.parseUnits("1000000", 18); // 1,000,000 MEME with 18 decimals
  const MemeToken = await ethers.getContractFactory("MemeToken");
  const memeToken = await MemeToken.deploy(initialSupply);

  console.log("MemeToken deployed to:", memeToken.address);
}

main()
  .then(() => process.exit(0))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
