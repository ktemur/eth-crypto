# eth-crypto

Cryptographic functions for ethereum and how to use them together with web3 and solidity.

## Tutorials

## Functions

### Install

```bash
  npm install eth-crypto --save
```

```javascript
// es6
import EthCrypto from 'eth-crypto';

// node
const EthCrypto = require('eth-crypto');
```

## API

### createIdentity()

Creates a new ethereum-identity with privateKey, publicKey and address as hex-string.

```javascript
  const identity = EthCrypto.createIdentity();
  /* > {
      address: '0x3f243FdacE01Cfd9719f7359c94BA11361f32471',
      privateKey: '0x107be946709e41b7895eea9f2dacf998a0a9124acbb786f0fd1a826101581a07',
      publicKey: 'bf1cc3154424dc22191941d9f4f50b063a2b663a2337e5548abea633c1d06ece...'
  } */
```

### publicKeyByPrivateKey()

Derives the publicKey from a privateKey and returns it as hex-string.

```javascript
  const publicKey = EthCrypto.publicKeyByPrivateKey(
      '0x107be946709e41b7895eea9f2dacf998a0a9124acbb786f0fd1a826101581a07'
  );
  // > 'bf1cc3154424dc22191941d9f4f50b063a2b663a2337e5548abea633c1d06ece...'
```

### addressByPublicKey()

Derives the ethereum-address from the publicKey.

```javascript
  const address = EthCrypto.publicKeyToAddress(
      'bf1cc3154424dc22191941d9f4f50b063a2b663a2337e5548abea633c1d06ece...'
  );
  // > '0x3f243FdacE01Cfd9719f7359c94BA11361f32471'
```

### sign()

Signs the message with the privateKey. Returns the signature as object with hex-strings.

```javascript
  const signature = EthCrypto.sign(
      '0x107be946709e41b7895eea9f2dacf998a0a9124acbb786f0fd1a826101581a07', // privateKey
      'foobar' // message
  );
  /* > {
        v: '0x1b',
        r: '0xc04b809d8f33c46ff80c44ba58e866ff0d5126d9943b58bee03cc5279450cacc',
        s: '0x757a3393b695ba83b2aba0c35c150399bf7959493aad80d51d917435b1a7ebda'
    } */
```

### recover()

Recovers the signers address from the signature.

```javascript
    const signer = EthCrypto.recover(
      {
          v: '0x1b',
          r: '0xc04b809d8f33c46ff80c44ba58e866ff0d5126d9943b58bee03cc5279450cacc',
          s: '0x757a3393b695ba83b2aba0c35c150399bf7959493aad80d51d917435b1a7ebda'
      }, // signature
      'foobar' // signed message
  );
  // > '0x3f243FdacE01Cfd9719f7359c94BA11361f32471'
```

### encryptWithPublicKey()

Encrypts the message with the publicKey so that only the corresponding privateKey can decrypt it. Returns (async) the encrypted data as object with hex-strings.

```javascript
    const encrypted = await EthCrypto.encryptWithPublicKey(
        'bf1cc3154424dc22191941d9f4f50b063a2b663a2337e5548abea633c1d06ece...', // publicKey
        'foobar' // message
    );
    /* >  {
            iv: '02aeac54cb45283b427bd1a5028552c1',
            ephemPublicKey: '044acf39ed83c304f19f41ea66615d7a6c0068d5fc48ee181f2fb1091...',
            ciphertext: '5fbbcc1a44ee19f7499dbc39cfc4ce96',
            mac: '96490b293763f49a371d3a2040a2d2cb57f246ee88958009fe3c7ef2a38264a1'
        } */
```

### decryptWithPrivateKey()

Decrypts the encrypted data with the privateKey. Returns (async) the message as string.

```javascript
    const message = await EthCrypto.decryptWithPrivateKey(
        '0x107be946709e41b7895eea9f2dacf998a0a9124acbb786f0fd1a826101581a07', // privateKey
        {
            iv: '02aeac54cb45283b427bd1a5028552c1',
            ephemPublicKey: '044acf39ed83c304f19f41ea66615d7a6c0068d5fc48ee181f2fb1091...',
            ciphertext: '5fbbcc1a44ee19f7499dbc39cfc4ce96',
            mac: '96490b293763f49a371d3a2040a2d2cb57f246ee88958009fe3c7ef2a38264a1'
        } // encrypted-data
    );
    // 'foobar'
```