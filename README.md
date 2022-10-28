# ZK DID Method Specification

## Summary

Decentralized identifiers (DIDs) are a new type of identifiers that enables verifiable, self-sovereign digital identity. This ZK DID method specification describes a new DID method -- `ZK DID`, defines how the `ZK DIDs` are stored, presents more detailed information about the corresponding DID Documents and how to do CRUD operations on `ZK DID` Documents.

This specification conforms to the requirements specified in the [DID specification[1]](https://www.w3.org/TR/did-core/) currently published by the W3C Credentials Community Group.

Unlike other DID Methods, `ZK DID` uses a brand new `verifiableDataRegistry` -- [Arweave[2]](https://www.arweave.org/), which is a global permanent hard drive built on a specially modified blockchain known as blockweave. 


## ZK DID Method Name

The namestring that shall identify this DID method is: `zk`

A DID that uses this method **MUST** begin with the following prefix: `did:zk`. Per the DID specification, this string **MUST** be in lowercase. 

The remainder of the DID, after the prefix, is specified below.


### ZK DID Method Specific Identifier

For now, the ZK DID method specific identifiers are categorized as two DID types, `EVM-based DID` and `Non-EVM-based DID`.

### EVM-based DIDs

A ZK EVM-based DID has the following structure:

```
zk-did  = "did:zk:" + <user-ethereum-address> 
user-ethereum-address = "0x" 40*HEXDIG
```


### Non-EVM-based DIDs

A ZK Non-EVM-based DID has the following structure:

```
zk-did  = "did:zk:" + <non-evm-chain-name>  + [ ":" + <user-address> ]
non-evm-chain-name = "sui" | "aptos" | ...
user-address = [a-zA-Z0-9]{1,64}
```

### ZK DID examples

| did                                                       | type        | description|
|:----------------------------------------------------------|-------------|-----------------------------------------------------------------------------|
| did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D | EVM-based DID  | a `did` which relates to a brand new `Ethereum` address -- '0x51fA67337...82EB6a9B22A'|
| did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847 | Non-EVM-based DID  | a `did` which relates to a brand new `Non-EVM-based` address (in this case --`sui` address) -- '0xd5059a902ad02b...7ceaf7d6' |



## DID Document


### Elements of a DID Document

A DID Document associated with a ZK DID is a set of data describing a DID subject. The representation of a DID Document when requested for production  MUST meet the DID Core specifications.

The following elements are needed for a W3C specification compliant DID Document representation:
- `@context`: A list of strings with links or JSONs for describing specifications that this DID Document is following to.
- `id`: Target DID with ZK DID Method prefix `did:zk:` or `did:zk:<non-evm-chain-name>:` (according to the network) and a unique-id identifier.
- `controller`: A list of fully qualified DID strings or one string. Contains one or more ZK DIDs who can control this DID Document.
- `verificationMethod`: A list of Verification Methods.
- `authentication` (optional): A list of strings with key aliases or IDs.
- `assertionMethod` (optional): A list of strings with key aliases or IDs.
- `keyAgreement` (optional): A list of strings with key aliases or IDs.
- `capabilityInvocation` (optional): A list of strings with key aliases or IDs
- `capabilityDelegation` (optional): A list of strings with key aliases or IDs.
- `service` (optional): A set of Service Endpoint maps.


### DID Document Example
* `EVM-based` DID Document example

```json
{
  "@context": [ "https://www.w3.org/ns/did/v1"],
  "id": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1",
  "controller": [ "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1" ],
  "verificationMethod": [
    {
      "id": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-0",
      "controller": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "publicKeyMultibase": "zgz4zgTUcbvduVZ1Jf3MNMeVeRYP2eiKDJnY7A6PCq3ew"
    },
    {
      "id": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-1",
      "controller": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1",
      "type": "X25519KeyAgreementKey2019",
      "publicKeyMultibase": "zEi6rdNHidYZdHyvKYX9sdKka32o6Xq5kP1umoL3Hv1mL"
    }
  ],
  "authentication": [ "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-0" ],
  "assertionMethod": [ "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-0" ],
  "keyAgreement": [ "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-1" ],
  "capabilityInvocation": [ "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-0" ],
  "capabilityDelegation": [ "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-0" ],
  "service": []
}
```

* `Non-EVM-based` DID Document example
```json
{
  "@context": [ "https://www.w3.org/ns/did/v1"],
  "id": "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847",
  "controller": [ "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847" ],
  "verificationMethod": [
    {
      "id": "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847#key-0",
      "controller": "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847",
      "type": "Ed25519",
      "publicKeyMultibase": "z28Ua2trQFi6EcXutLiHto6wx6AvJM28i4XLNHkQUVm8fJ"
    },
    {
      "id": "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847#key-1",
      "controller": "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847",
      "type": "X25519KeyAgreementKey2019",
      "publicKeyMultibase": "zGnC9ZV1EJoq2DHLBXNboBzhUysTvTYAhUCRspymTjXe"
    }
  ],
  "authentication": [ "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847#key-0" ],
  "assertionMethod": [ "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847#key-0" ],
  "keyAgreement": [ "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847#key-1" ],
  "capabilityInvocation": [ "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847#key-0" ],
  "capabilityDelegation": [ "did:zk:sui:0xe9f5c3e0a721335e08bcc1168d606257418fc847#key-0" ],
  "service": []
}
```

### Supported cryptography

Currently, three public key and signing algorithms are supported:

- [`Ed25519`](https://en.wikipedia.org/wiki/EdDSA#Ed25519) -- the EdDSA signature scheme using [SHA-512](https://en.wikipedia.org/wiki/SHA-2) (SHA-2) and [Curve25519](https://en.wikipedia.org/wiki/Curve25519).
- [`Secp256k1`](https://en.bitcoin.it/wiki/Secp256k1) with the ECDSA algorithm -- defined in [*Standards for Efficient Cryptography (SEC)* (Certicom Research)](http://www.secg.org/sec2-v2.pdf).
- [`X25519`](https://en.wikipedia.org/wiki/Curve25519) --  is an elliptic curve[ Diffie-Hellman key exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) using [Curve25519](https://en.wikipedia.org/wiki/Curve25519). It allows two parties to jointly agree on a shared secret using an insecure channel.


### Verification method

Verification methods are used to define how to authenticate / authorise interactions with a DID subject.

- `id` (string): A string with prefix `did:zk:` or `did:zk:<non-evm-chain-name>:`.
- `type` (string): A string that references exactly one verification method type. For now, only `EcdsaSecp256k1VerificationKey2019`, `Ed25519` and `X25519KeyAgreementKey2019` are supported.
- `controller`(string): A string with fully qualified ZK DID. The ZK DID must exist.
- `publicKeyHex`(HexString): A Hex String representation of the `publickKey`.

Example of Verification method in a DID Document:
```json
  "verificationMethod": [
    {
      "id": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-0",
      "controller": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "publicKeyMultibase": "zgz4zgTUcbvduVZ1Jf3MNMeVeRYP2eiKDJnY7A6PCq3ew"
    },
    {
      "id": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1#key-1",
      "controller": "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1",
      "type": "X25519KeyAgreementKey2019",
      "publicKeyMultibase": "zEi6rdNHidYZdHyvKYX9sdKka32o6Xq5kP1umoL3Hv1mL"
    }
  ]
```

### Verifiable Data Registry using Arweave

Currently, we build the ZK DID Method with an unique VDR -- **Arweave**. [Arweave]((https://www.arweave.org/)) is a distributed, cryptographically verified permanent archive built on a cryptocurrency that aims to provide permanent data storage. It allows users to pay once and store their data permanently on the Arweave network.

In Arweave, *Tags* are important when a user tries to retrieve certain transactions. *Tags* are key-value pairs of data that will be processed by Arweave. Users can query Arweave Transactions via `Tags`, `Owners`, `Recipients` and many other elements. This property plays a significant role during the resolution phrase.

##  CRUD Operations

ZK DID is registered and managed on a dedicated DID Registry on Arweave.

- Anyone can create ZK DID by interacting with zCloak DID Registry Manager. We provide [`create`](https://github.com/zCloak-Network/zkid-sdk/blob/master/packages/did/src/did/helpers.ts#L150-L219) function in our SDK.
- Anyone can read (resolve) the DID documents by accessing public Arweave network nodes. We also provide [`resolve`](https://github.com/zCloak-Network/zkid-sdk/blob/master/packages/did-resolver/src/DidResolver.ts#L23-L28) funtion in our SDK to help achieve that.

### Create (Register)

A ZK DID can be created via the [DID Create SDK[3]](https://github.com/zCloak-Network/zkid-sdk/blob/master/packages/did/src/did/helpers.ts#L150-L219) as explained below.

A ZK DID supports the following keys: authentication key, key agreement key, assertion key, capabilityInvocation key and capabilityDelegation key.

Each authentication key, assertion key, capabilityInvocation key and capabilityDelegation key are generated through `ecdsa-secp256k1` or `Ed25519`. For key agreement key, currently, only `x25519` is supported.

To generate and store the new ZK DID on the Arweave, the DID controller must sign the `DID Document`. The DID Document along with the signature are stored in Arweave as one document. 

This operation creates a new DID using the `did:zk` method along with associated DID Document.

- `mnemonic`: valid bip39 mnemonic phrase
- `keyring`: an instance of 'KeyringInstance'


#### Client request format for create DID 

Check more coding details about the `Creating DID functionin` our [SDK](https://github.com/zCloak-Network/zkid-sdk/blob/master/packages/did/src/did/helpers.ts#L150-L219).

```ts
export function createEcdsaFromMnemonic(
  mnemonic: string,
  keyring: KeyringInstance = new Keyring(),
  resolver: DidResolver = defaultResolver
): Did
```

#### Example of Creating DID request

```ts
import { helpers, Did } from '@zcloak/did'
import { Keyring } from '@zcloak/keyring'
import type { IDidDetails } from '@zcloak/did/types';

const mnemonic = 'health correct setup usage father decorate curious copper sorry recycle skin equal';
const keyring: Keyring = new Keyring();
const did: Did = helpers.createEcdsaFromMnemonic(mnemonic, keyring);
```

### Read (Resolve)

A DID with the type `did:zk` can be resolved using the resolve query to fetch a response from the Arweave. The response contains:

- `did Document`: DID Document associated with the specified DID in a W3C specification compliant DID Doc structure.


#### Client request format for read(resolve) DID

[DID resolution requests[4]](https://github.com/zCloak-Network/zkid-sdk/blob/master/packages/did-resolver/src/DidResolver.ts#L23-L28) can be sent by passing the fully-qualified ZK DID String and retrieve the corresponding DID Document.

```ts
public resolve(did: string): Promise<DidDocument> 
```

#### Example of an get/resolve DID request

```ts
let DidString = "did:zk:0x11f8b77F34FCF14B7095BF5228Ac0606324E82D1";
let DidDoc = resolve(DidString);
```

### Update (Replace)

Not supported yet. Documents stored in Arweave are permanent and tamper-proof. Update of DID Document can be supported by appending a new version of the DID Document to the old one. This function will be supported in the future.

### Delete (Revoke)

Not supported yet. Documents stored in Arweave are permanent and tamper-proof. Revocation of a DID Document can be supported by appending a revoke document with a controller signature. This function will be supported in the future.

## Security and Privacy Considerations
There are several security and privacy considerations that implementers would want to take into consideration when implementing this specification. 

1. Data Forgery Prevention

ZK DID Method prevents forgery and falsification through Arweave and Digital Signature. With Arweave, all DID Documents are stored permanently. With digital signature, only the controller of the DID Document is capable of managing the DID Document.

2. Eavesdropping

Eavesdropping attacks are not applicable since all exchanged data is public and does not include any personal information about the user.

3. Cryptographic Agility

As described in the Supported Cryptography section, currently `EcdsaSecp256k1VerificationKey2019`, `Ed25519` and `X25519KeyAgreementKey2019` are supported.  This can be easily extended by using other multicodec encoded keys.

4. Keep DID Keys safe

Since the key material is part of the identifier, and there is no support for key rotation at present, if the key is compromised then the identifier becomes unusable and unrecoverable.

5. Keep personal data safe

The syntax and construction of a ZK DID and its associated DID Document helps to ensure that no Personally Identifiable Information (PII) or other personal data is exposed by these constructs.

Further, Implementers are strongly encouraged to review the [Security Considerations section](https://w3c.github.io/did-imp-guide/#security-considerations.) and the [Privacy Considerations section](https://w3c.github.io/did-imp-guide/#privacy-considerations) of the DID Implementation Guide.

In addition, consult the [Security Considerations section](https://www.w3.org/TR/did-core/#security-considerations) and the [Privacy Considerations section](https://www.w3.org/TR/did-core/#privacy-considerations) of the Decentralized Identifiers (DIDs) (DID-CORE) specification


## Reference Implementations

- [zk-did](https://github.com/zCloak-Network/zkid-sdk/tree/master/packages/did) -- the implementation of the ZK DID method.
- [zk-did-resolver](https://github.com/zCloak-Network/zkid-sdk/tree/master/packages/did-resolver) -- ZK DID method resolver.

## References

[1] https://www.w3.org/TR/did-core/

[2] https://www.arweave.org/

[3] https://github.com/zCloak-Network/zkid-sdk/tree/master/packages/did

[4] https://github.com/zCloak-Network/zkid-sdk/tree/master/packages/did-resolver