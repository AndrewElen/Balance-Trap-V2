# Balance Trap V2

**Balance Trap V2** is a smart contract trap for [Drosera](https://drosera.io) that monitors balance changes and reacts to suspicious activity by calling a function on an external contract.

---

## 📌 Project Description
Balance Trap V2 monitors the balances of Ethereum addresses and, upon detecting **balance changes of 2% or more**, calls the `saveAlert(string)` function on the receiver contract (`LogAlertReceiver`). This allows real-time alerts for events that require attention.

---

## ⚙️ Installation and Setup

###  Clone the repository
```bash
git clone https://github.com/AndrewElen/Balance-Trap-V2.git
cd Balance-Trap-V2

Install dependencies
npm install
Hardhat is used for compiling and deploying contracts.

Example Drosera configuration (drosera.toml)
ethereum_rpc = "https://ethereum-hoodi-rpc.publicnode.com"
drosera_rpc = "https://relay.hoodi.drosera.io"
eth_chain_id = 560048
drosera_address = "0x91cB447BaFc6e0EA0F4Fe056F5a9b1F14bb06e5D"

[traps.mytrap]
path = "out/BalanceTrapV2.sol/BalanceTrapV2.json"
response_contract = "0x3eFf8AE7Ae843b31CB0639F03213C8E6Db48eae8"
response_function = "saveAlert(string)"
cooldown_period_blocks = 33
min_number_of_operators = 1
max_number_of_operators = 2
block_sample_size = 10
private_trap = true
whitelist = ["0xd1b3Dd4C3442d24105550886Fd6EB38dD9592356"]
address = "0x908559bbb34FE19a1dAb91479FB2A633133f9339"
balance_change_threshold_percent = 2
⚠️ This example uses placeholder values. Do not publish real private keys on public repositories.

Compile and deploy LogAlertReceiver
forge create --rpc-url https://ethereum-hoodi-rpc.publicnode.com \
--broadcast --private-key <YOUR_PRIVATE_KEY> \
src/LogAlertReceiver.sol:LogAlertReceiver
Replace <YOUR_PRIVATE_KEY> with your actual private key (keep it secret).

Apply the trap configuration
drosera run drosera.toml

🛠️ Project Structure
Balance-Trap-V2/
├─ contracts/          # Smart contracts (BalanceTrapV2.sol, LogAlertReceiver.sol)
├─ scripts/            # Deployment and test scripts
├─ out/                # Compiled contract JSON files
├─ test/               # Unit tests
├─ README.md           # Documentation
└─ package.json        # Project settings

🔧 Usage Examples
// LogAlertReceiver.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LogAlertReceiver {
    event Alert(string message);

    function saveAlert(string calldata message) external {
        emit Alert(message);
    }
}

// Example of subscribing to BalanceTrapV2 events
const trap = require('./out/BalanceTrapV2.sol/BalanceTrapV2.json');

trap.on('BalanceChanged', (data) => {
  if (data.changePercent >= 2) {
    console.log('Balance change >= 2% detected:', data);
    trap.saveAlert("Suspicious activity!");
  }
});

⚠️ Important Notes

Always test on a testnet before deploying to mainnet.

Never publish private keys in public repositories.

Ensure RPC endpoints and addresses are correct for your network.

The trap is private (private_trap = true) and only reacts to addresses in the whitelist.

The trap triggers only for balance changes of 2% or more.

📚 Resources

Drosera
 — Smart contract monitoring platform

Hardhat
 — Ethereum development framework

Forge
 — Smart contract compilation and deployment tool

Ethers.js
 — Ethereum JavaScript library

![Снимок экрана от 2025-09-02 12-20-44](https://github.com/AndrewElen/Balance-Trap-V2/raw/main/Снимок%20экрана%20от%202025-09-02%2012-20-44.png)

![Снимок экрана от 2025-09-02 12-13-44](https://github.com/AndrewElen/Balance-Trap-V2/raw/main/Снимок%20экрана%20от%202025-09-02%2012-13-44.png)



