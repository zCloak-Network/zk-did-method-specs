# ZK DID Method Specification

## Summary

Decentralized identifiers (DIDs) are a new type of identifiers that enables verifiable, self-sovereign digital identity. This ZK DID method specification describes a new DID method -- `ZK DID`, defines how the `ZK DIDs` are stored, presents more details information about the corresponding DID Documents and how to do CRUD operations on `ZK DID` Documents.

This specification conforms to the requirements specified in the [DID specification[1]](https://www.w3.org/TR/did-core/) currently published by the W3C Credentials Community Group.
 
Unlike other DID Method, `ZK DID` uses a brand new `verifiableDataRegistry` -- [Arweave[2]](https://www.arweave.org/), which is a global permanent hard drive built on a specially modified blockchain known as blockweave. 


## ZK DID Method Name

The namestring that shall identify this DID method is: `zk`

A DID that uses this method **MUST** begin with the following prefix: `did:zk`. Per the DID specification, this string **MUST** be in lowercase. 

The remainder of the DID, after the prefix, is specified below.


### ZK DID Method Specific Identifier

For now, the ZK DID method specific identifiers are categorized as two DID types, `EVM-based DID` and `SUI-based DID`.

### EVM-based DIDs

A ZK EVM-based DID has the following structure:

```
zk-did  = "did:zk:" + <user-ethereum-address> 
user-ethereum-address = "0x" 40*HEXDIG
```


### SUI-based DIDs

A ZK SUI-based DID has the following structure:

```
zk-did  = "did:zk:sui:" + <user-sui-address> 
user-sui-address = "0x" 40*HEXDIG
```

### ZK DID examples

| did                                                       | type        | description
|:----------------------------------------------------------|-------------|-----------------------------------------------------------------------------
| did:zk:0x51fA67337...82EB6a9B22A   | EVM-based DID  | a `did` which relates to a brand new `Ethereum` address -- '0x51fA67337...82EB6a9B22A'
| did:zk:sui:0xd5059a902ad02b...7ceaf7d6     | SUI-based DID  | a `did` which relates to a brand new `SUI` address -- '0xd5059a902ad02b...7ceaf7d6'



## DID Document


### Elements of a DID Document

A DID Document associated with a ZK DID is a set of data describing a DID subject. The representation of a DID Document when requested for production  MUST meet the DID Core specifications.

The following elements are needed for a W3C specification compliant DID Document representation:

- `@context`: A list of strings with links or JSONs for describing specifications that this DID Document is following to.
- `id`: Target DID with ZK DID Method prefix `did:zk:` or `did:zk:sui:` (according to the network) and a unique-id identifier.
- `controller`: A list of fully qualified DID strings or one string. Contains one or more ZK DIDs who can control this DID Document.
- `verificationMethod`: A list of Verification Methods.
- `authentication` (optional): A list of strings with key aliases or IDs.
- `assertionMethod` (optional): A list of strings with key aliases or IDs.
- `capabilityInvocation` (optional): A list of strings with key aliases or IDs
- `capabilityDelegation` (optional): A list of strings with key aliases or IDs.
- `keyAgreement` (optional): A list of strings with key aliases or IDs.
- `service` (optional): A set of Service Endpoint maps

// todo, check whether these elements are optional when the SDK is done.

### DID Document Example
* `EVM-based` DID Document example
```json
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4",
  "controller": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4",
  "verificationMethod": [
    {
      "id": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4#public-key-Hex-1",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "controller": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4",
      "publicKeyHex": "//todo"
    },
    {
      "id": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4#public-key-Hex-2",
      "type": "X25519KeyAgreementKey2019",
      "controller": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4",
      "publicKeyHex": "//todo"
    }
  ],
  "authentication": [
    "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4#public-key-Hex-1"
  ],
  "assertionMethod":[
    "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4#public-key-Hex-1"
  ],
  "capabilityDelegation": [
    "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4#public-key-Hex-1"
  ],
  "KeyAgreement":[
    "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4#public-key-Hex-2"
  ],
  "service": [
    {
      "id": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4#edv",
      "type": "EncryptedDataVault",
      "serviceEndpoint": "https://edv.example.com/"
    }
  ]
}
```

* `SUI-based` DID Document example
```json
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:zk:sui://todo",
  "controller": "did:zk:sui://todo",
  "verificationMethod": [
    {
      "id": "did:zk:sui://todo#public-key-Hex-1",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "controller": "did:zk:sui://todo",
      "publicKeyHex": "//todo"
    },
    {
      "id": "did:zk:sui://todo#public-key-Hex-2",
      "type": "X25519KeyAgreementKey2019",
      "controller": "did:zk:sui://todo",
      "publicKeyHex": "//todo"
    }
  ],
  "authentication": [
    "did:zk:sui://todo#public-key-Hex-1"
  ],
  "assertionMethod":[
    "did:zk:sui://todo#public-key-Hex-1"
  ],
  "capabilityDelegation": [
    "did:zk:sui://todo#public-key-Hex-1"
  ],
  "KeyAgreement":[
    "did:zk:sui://todo#public-key-Hex-2"
  ],
  "service": [
    {
      "id": "did:zk:sui://todo#edv",
      "type": "EncryptedDataVault",
      "serviceEndpoint": "https://edv.example.com/"
    }
  ]
}
```

### Supported cryptography

Currently, two public key and signing algorithms are supported:

- [`Ed25519`](https://en.wikipedia.org/wiki/EdDSA#Ed25519) -- the EdDSA signature scheme using [SHA-512](https://en.wikipedia.org/wiki/SHA-2) (SHA-2) and [Curve25519](https://en.wikipedia.org/wiki/Curve25519).
- [`Secp256k1`](https://en.bitcoin.it/wiki/Secp256k1) with the ECDSA algorithm -- defined in [*Standards for Efficient Cryptography (SEC)* (Certicom Research)](http://www.secg.org/sec2-v2.pdf).


### Verification method

Verification methods are used to define how to authenticate / authorise interactions with a DID subject.

- `id` (string): A string with prefix `did:zk:` or `did:zk:sui:`.
- `type` (string): A string that references exactly one verification method type. For now, only `EcdsaSecp256k1VerificationKey2019` and `X25519KeyAgreementKey2019` are supported.
- `controller`(string): A string with fully qualified ZK DID. The ZK DID must exist.
- `publicKeyHex`(HexString): A Hex String representation of the `publickKey`.

Example of Verification method in a DID Document:
```json
  {
      "id": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4#public-key-Hex-1",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "controller": "did:zk:0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4",
      "publicKeyHex": "//todo"
    },
```

### Arweave Blockweave (verifiableDataRegistry)

Currently, we build the ZK DID Method with an unique VDR -- **Arweave**. [Arweave]((https://www.arweave.org/)) is a distributed, cryptographically verified permanent archive built on a cryptocurrency that aims to provide feasible data permanence. It allows users to store data permanently on the network. 

In Arweave, *Tags* are important when User tries to retrieve certain transactions. *Tags* are key-value pairs of data that will be processed by Arweave. Users can query Arweave Transactions via `Tags`, `Owners`, `Recipients` and many other elements. This property plays a significant role during the resolution phrase.

###  CRUD Operations

ZK DID is registered and managed on a dedicated DID Registry on a specific blockweave -- [Arweave](https://www.arweave.org/) .

- Anyone can create ZK DID by interacting with zCloak DID Registry Manager. We provide necessary [`create`](//todo add link) function in our SDK.
- Anyone can read (resolve) the DID documents by accessing public Arweave network nodes. We also provide [`resolve`]((//todo add link)) funtion in our SDK to help achieve that.

#### Create (Register)

A ZK DID can be created via the [DID Create SDK[3]](//todo sdk link) as explained below.

A ZK DID supports the following keys: authentication key, key agreement key, assertion key  and dalegation key.

Each authentication key, assertion key and delegation key are generated through `ecdsa-secp256k1`. For key agreement key, currently, only `x25519` is supported.

To generate and store the new ZK DID on the Arweave, the DID controller must first sign a `DID Document`. //todo: check sdk make sure whether there exist a signature?

// todo: wait for the sdk implementation, modify the interface below

This operation creates a new DID using the `did:zk` method along with associated DID Document representation.

- `signatures`: CreateDidRequest should be signed by controller's authentication key. This field contains the signature value, which could be used to verify the signer's identity.
- `id`: Fully qualified ZK DID, with the prefix `did:zk:` or `did:zk:sui:`.
- `controller`, `verificationMethod`, `authentication`, `assertionMethod`, `capabilityInvocation`, `capabilityDelegation`, `keyAgreement`, `service`, `alsoKnownAs`, `contex`t: Optional parameters in accordance with DID Core specification properties.

##### Client request format for create DID 

// todo: need to update new function from the sdk, the sdk not have this function yet

```ts
CreateDid(context, id, controller, verificationMethod, authentication, assertionMethod, capabilityInvocation, capabilityDelegation, keyAgreement, service, signatures), 
```

##### Example of a create DID client request

```ts
let Create_Result = CreateDid(
      "context": [
          "https://www.w3.org/ns/did/v1",
          "https://w3id.org/security/suites/ed25519-2020/v1"
      ],
....
)
```

#### Read (Resolve)

A DID with the type `did:zk` can be resolved using the GetDid query to fetch a response from the Arweave. The response contains:

- `did Document`: DID Document associated with the specified DID in a W3C specification compliant DID Doc structure.


##### Client request format for read(resolve) DID

[DID resolution requests[4]](//todo wait for the sdk link) (click to check the SDK Code) can be sent by passing the fully-qualified ZK DID String and retrieve the corresponding DID Document.

```ts
resolve(did: string)
```

##### Example of an get/resolve DID client request

```ts
let DidString = "0xfcE45bB89a26c6Cae71751fe54e4ACEeeB6622A4";
let DidDoc = resolve(DidString);
```

#### Update (Replace)

Not support yet. Since the DID documents are stored on Arweave, which means they are stored permanently and cannot be updated through changing the origin DID document. So, at present, we don't support update.

#### Delete (Revoke)

Not support yet. The same reason as it can not be update -- the did document is stored permanently on Arweave.

### Security and Privacy Considerations

There are several securities and privacy considerations that implementers would want to take into consideration when implementing this specification. Due to the unique implementation of 
verifiableDataRegistry -- 'Arweave', several operations are not supported:

#### Key Rotation Not Supported

For now, the `did:zk` method is a purely generative method, which means that updates are not supported. This can be an issue if a `did:zk` is being used in a context where all supported dids are assumed to be capable of rotation.


#### Deactivation Not Supported

For now, the `did:zk` method is a purely generative method, which means that deactivations and "tombstoning" are not supported. This can be an issue if a `did:zk` is being used in a context where all supported dids are assumed to be capable of deactivation.

Besides, there're few rules that the developers should follow:
- The DID controller MUST store the private key securely. 
- The DID controller MUST ensure to protect integrity and confidentiality of user personal data. The DID controller MUST comply with all applicable privacy and data protection laws, regulations and principles.
- All information in the DID Document must be 'Non-personally Identifiable Information (Non-PII)' and cannot be updated for the purely generative property.

### Reference Implementations

- [zk-did-creator](https://github.com/zCloak-Network/zkid-sdk/tree/master/packages/did) - the create implementation of the ZK DID method.
- [zk-did-resolver](https://github.com/zCloak-Network/zkid-sdk/tree/master/packages/did-resolver) - ZK DID method resolver.

### References

[1] https://www.w3.org/TR/did-core/
[2] https://www.arweave.org/
[3] https://github.com/zCloak-Network/zkid-sdk/tree/master/packages/did
[4] https://github.com/zCloak-Network/zkid-sdk/tree/master/packages/did-resolver