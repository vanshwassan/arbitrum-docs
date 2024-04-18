### How does Stylus manage security issues in smart contracts when interacting with so many different languages?

<p>
  All languages are compiled to WASM for them to be able to work with Stylus. So it just needs to
  verify that the produced WASM programs behave as they should inside the new virtual machine.
</p>

<p></p>

### Is there any analogue of the fallback function from Solidity in the Rust Stylus SDK?

<p>
  Currently there isn't any analogue. However, you can use a minimal entrypoint and perform raw
  delegate calls, forwarding your calldata. You can find more information in{' '}
  <a href="https://docs.arbitrum.io/stylus/reference/rust-sdk-guide#bytes-in-bytes-out-programming">
    Bytes-in, bytes-out programming
  </a>{' '}
  and{' '}
  <a href="https://docs.arbitrum.io/stylus/reference/rust-sdk-guide#call-static_call-and-delegate_call">
    call, static_call and delegate_call
  </a>
  .
</p>

<p></p>

### Why are constructors not yet supported for Stylus contracts?

<p>
  Constructors use EVM bytecode to initialize state. While one could add EVM bytecode manually to
  their Stylus deployment, we don't allow WASM execution in the constructor so there's no way to
  express this in the SDK.
</p>

<p>
  We're working on models that will make init easier, so there might be better solutions available
  in the future. For now, we suggest calling an init method after deploying.
</p>

<p></p>

### Is it possible to verify Stylus contracts on the block explorer?

<p>
  Currently it is not possible to verify contracts compiled to WASM on the block explorer, but we
  are actively working with providers to have the verification process ready for when Stylus reaches
  mainnet-ready status.
</p>

<p></p>

### Do Stylus contracts compile down to EVM bytecode like prior other attempts?

<p>
  No. Stylus contracts are compiled down to WASM. The user writes a program in Rust / C / C++ which
  is then compiled down to WebAssembly.
</p>

<p></p>

### How is a Stylus contract deployed?

<p>
  Stylus contracts are deployed on chain as a blob of bytes, just like EVM ones. The only difference
  is that when the contract is executed, instead of invoking the EVM, we invoke a separate WASM
  runtime. Note that a special EOF-inspired prefix distinguishes Stylus contracts from traditional
  EVM contracts: when a contract's bytecode starts with the magic <code>0xEF000000</code> prefix,
  it's a Stylus WASM contract.
</p>

<p></p>

### Is there a new transaction type to deploy Stylus contracts?

<p>
  You deploy a Stylus contract the same way that Solidity contracts are deployed. There are no
  special transaction types. As a UX note: a WASM will revert until a special instrumentation
  operation is performed by a call to the new  <code>ArbWasm</code> precompile, which readies the
  program for calls on-chain.
</p>

<p>
  You can find instructions for deploying a Stylus contract in our{' '}
  <a href="https://docs.arbitrum.io/stylus/stylus-quickstart#checking-your-stylus-project-is-valid">
    Quickstart
  </a>
  .
</p>

### Do Stylus contracts use a different type of ABI?

<p>
  Stylus contracts use solidity ABIs. Methods, signatures, logs, calls, etc. work exactly as in the
  EVM. From a user's / explorer's perspective, it all just looks and behaves like solidity.
</p>

<p></p>

### Does the Stylus SDK for Rust support custom data structures?

<p>
  For in-memory usage, you should be able to use any implementation of custom data structures
  without problems.
</p>

<p>
  For storage usage, it might be a bit more complicated. Stylus uses the EVM storage system, so
  you'll need to define the data structure on top of it. However, in the SDK there's a storage trait
  that custom types can implement to back their collections with the EVM state trie. The SDK macros
  are compatible with them too, although fundamentally it's still a global key-value system.
</p>

<p>
  You can read more about it in the{' '}
  <a href="https://docs.arbitrum.io/stylus/reference/rust-sdk-guide#storage">
    Stylus Rust SDK page
  </a>
  .
</p>

<p>
  As an alternative solution, you can use{' '}
  <a href="https://docs.arbitrum.io/stylus/reference/rust-sdk-guide#bytes-in-bytes-out-programming">
    entrypoint-style contracts
  </a>{' '}
  for your custom data structures.
</p>

<p></p>

<p></p>