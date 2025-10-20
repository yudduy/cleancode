---
skill: web3-security
description: Web3/blockchain security patterns, signature verification, wallet integration. Activates when wagmi, viem, ethers, or Web3 imports detected.
category: domain
activation: auto
priority: 9
---

# Web3 Security Patterns

## Signature Verification

### Anti-Replay Protection
```typescript
// ❌ BAD: No replay protection
const signature = await signMessage({ message: address })
const verified = await verifyMessage({ message: address, signature })

// ✅ GOOD: Nonce + timestamp
const nonce = generateNonce()
const timestamp = Date.now()
const message = `Sign in\nNonce: ${nonce}\nTimestamp: ${timestamp}`

// Verify on server:
const verified = await verifyMessage({ message, signature })
if (!verified || !checkNonce(nonce) || timestamp + 300000 < Date.now()) {
  throw new Error('Invalid signature')
}
```

## Wallet Connection Security

```typescript
// ✅ Always validate chain
const { chain } = useNetwork()
if (chain?.id !== expectedChainId) {
  throw new Error('Wrong network')
}

// ✅ Validate address format
import { isAddress } from 'viem'
if (!isAddress(address)) {
  throw new Error('Invalid address')
}
```

## Transaction Safety

```typescript
// ✅ Validate all inputs
const amount = parseEther(userInput)
if (amount > balance) {
  throw new Error('Insufficient balance')
}

// ✅ Set gas limits
await contract.write.transfer([to, amount], {
  gas: 100000n // Prevent gas estimation attacks
})
```

## Common Vulnerabilities

- ❌ Signature replay attacks
- ❌ Front-running
- ❌ Unchecked external calls
- ❌ Integer overflow/underflow
- ❌ Reentrancy

**Always:** Validate inputs, check signatures, prevent replay attacks
