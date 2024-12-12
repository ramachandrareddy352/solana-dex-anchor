
**Automated Market Makers (AMM)** - Your Gateway to Effortless Trading!
Welcome to the world of Automated Market Makers (AMM), where seamless trading is made possible with the power of automation. The primary goal of AMMs is to act as automatic buyers and sellers, readily available whenever users wish to trade their assets.

### Design

Let's go over the essential requirements for our smart contract design:

- Fee Distribution: Every pool must have a fee to reward Liquidity Providers (LPs). This fee is charged on trades and paid directly in the traded token. To maintain consistency across all pools, the fees will be shared.

- Single Pool per Asset Pair: Each asset pair will have precisely one pool. This approach avoids liquidity fragmentation and simplifies the process for developers to locate the appropriate pool.

- LPs Deposit Accounting: We need to keep track of LPs deposits in the smart contract.

To achieve an efficient and organized design, we can implement the following strategies:

- Shared Parameters: As pools can share certain parameters like the trading fee, we can create a single account to store these shared parameters for all pools. Additionally, each pool will have its separate account. This approach saves storage space, except when the configuration is smaller than 32 bytes due to the need to store the public key. In our case, we'll include an admin for the AMM to control fees, which exceeds the limit.

- Unique Pool Identification: To ensure each pool remains unique, we'll utilize seeds to generate a Program Derived Account (PDA). This helps avoid any ambiguity or confusion.

- SPL Token for Liquidity Accounting: We'll utilize the SPL token standard for liquidity accounting. This choice ensures easy composability and simplifies the handling of liquidity in the contract.

By implementing these strategies, we are creating a solana program that efficiently manages liquidity pools, rewards LPs, and maintains a seamless trading experience across various asset pairs.

## Principals

Here are some essential principles to consider when building on-chain programs in Solana:

- Store Keys in the Account: It's beneficial to store keys in the account when creating Program Derived Accounts (PDAs) using seeds. While this may increase account rent slightly, it offers significant advantages. By having all the necessary keys in the account, it becomes effortless to locate the account (since you can recreate its public key). Additionally, this approach works seamlessly with Anchor's has_one clause, streamlining the process.

- Simplicity in Seeds: When creating PDA seeds, prioritize simplicity. Using a straightforward logic for seeds makes it easier to remember and clarifies the relationship between accounts. A logical approach is to first include the seeds of the parent account and then use the current object's identifiers, preferably in alphabetical order. For example, in an AMM account storing configuration (with no parent), adding an identifier attribute, usually a pubkey, becomes necessary since the admin can change. For pools, which have the AMM as a parent and are uniquely defined by the tokens they facilitate trades for, it's advisable to use the AMM's pubkey as the seed, followed by token A's pubkey and then token B's.

- Minimize Instruction's Scope: Keeping each instruction's scope as small as possible is crucial for several reasons. It helps reduce transaction size by limiting the number of accounts touched simultaneously. Moreover, it enhances composability, readability, and security. However, a trade-off to consider is that it may lead to an increase in Lines Of Code (LOC).

- By following these principles, you can build on-chain programs in Solana that are efficient, well-organized, and conducive to seamless interactions, ensuring a robust foundation for your blockchain projects.

## Code Examples

```file structure
programs/token-swap/src/
├── constants.rs
├── errors.rs
├── instructions
│   ├── create_amm.rs
│   ├── create_pool.rs
│   ├── deposit_liquidity.rs
│   ├── mod.rs
│   ├── swap_exact_tokens_for_tokens.rs
│   └── withdraw_liquidity.rs
├── lib.rs
└── state.rs
```

**Instructions**
   
  1 **create amm**
  2 **create pool**
  3 **deposite liquidity**
  4 **swap exact tokens**