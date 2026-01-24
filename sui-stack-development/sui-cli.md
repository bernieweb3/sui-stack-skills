# Skill: Sui CLI Mastery

## Context
You are a **DevOps & Smart Contract Engineer** specializing in the Sui Ecosystem. You rely on the Sui CLI not just for deployment, but for advanced transaction building, debugging, and key management. You understand the difference between `sui client` and `sui move` commands.

## Core Commands

### 1. Network & Environment
Manage your active environment (`mainnet`, `testnet`, `devnet`, `localnet`).
*   **Switch Env:** `sui client switch --env <alias>`
*   **Add Env:** `sui client new-env --alias mainnet --rpc https://fullnode.mainnet.sui.io:443`
*   **Active Address:** `sui client active-address`
*   **Gas check:** `sui client gas`

### 2. Key Management (Keytool)
Do not manually manage private keys in plain text. Use `sui keytool`.
*   **List Keys:** `sui keytool list`
*   **Import Private Key:** `sui keytool import "<bech32-or-hex-key>" <scheme>`
*   **Generate New:** `sui keytool generate ed25519`
*   **Multi-sig (Advanced):** `sui keytool multi-sig-address ...`

### 3. Move Development
*   **Build:** `sui move build` (Compiles the package)
*   **Test:** `sui move test` (Runs unit tests)
    *   *Tip:* Use `sui move test --coverage` to see code coverage.
*   **Publish:** `sui client publish --gas-budget 200000000`
    *   *Tip:* Always set a gas budget to prevent unexpected failures.
    *   *Tip:* Use `--skip-dependency-verification` if re-publishing known deps (use with caution).
*   **Upgrade:** `sui client upgrade` (requires `UpgradeCap`)

### 4. Transaction Building (PTB)
For complex interactions, construct Programmable Transaction Blocks (PTB) via verification.
*   **Call Function:** `sui client call --package <PKG_ID> --module <MOD> --function <FUN> --args <ARG1> <ARG2> --gas-budget <GAS>`
*   **Inspect (Dry Run):** `sui client call ... --dry-run` (Crucial for debugging before spending gas)
*   **Serialize/Deserialize:** Use `sui client tx-block` commands to inspect Base64 encoded transaction blobs.

### 5. Object Inspection
*   **View Object:** `sui client object <OBJECT_ID>`
*   **View Dynamic Fields:** `sui client dynamic-field <PARENT_OBJECT_ID>`

## Best Practices
1.  **Replay Tool:** Use `sui-replay` (separate binary usually) to debug failed transactions on Mainnet by re-executing them locally with trace logging.
2.  **Verify Source:** When publishing, always ensure your local source matches what you intend to deploy.
3.  **Gas Coin Management:** If you have many small gas coins, use `sui client merge-coin` or `sui client pay` to consolidate them (or `pay` to split/transfer SUI).
4.  **RPC Selection:** If the CLI is slow, switch to a paid/private RPC endpoint (e.g., classic load balancing issues on public nodes).

## Common Troubleshooting
*   **"Insufficient Gas":** Ensure the gas coin used for payment is not *also* being passed as a move argument unless strictly intended (and handled correctly).
*   **"Equivocation":** Usually happens when reusing the same simplified object in test scenarios or weird local validator states. Reset localnet if stuck.
