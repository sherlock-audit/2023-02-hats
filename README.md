
# Hats Protocol contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Resources

- [Hats Protocol user guide](https://github.com/more-reese/hats-protocol/blob/develop/user_guide/user_guide.md)
- [Goerli Hats Protocol deployment](https://goerli.etherscan.io/address/0xe274f43bea6656b4c8316ee172dce6f5550533cc)
- [Goerli Hats Signer Gate deployment](https://goerli.etherscan.io/address/0xf1400e81b6c2c5d3e53ca6962102d0bec2c40780)

# On-chain context

This audit covers two sets of contracts.

1. Hats Protocol — our core protocol
2. Hats Signer Gate — a contract that utilizes Hats Protocol to enable DAOs to programmatically, dynamically, and revocably delegate signing authorities to a set of individuals.

## 1. Hats Protocol

```
DEPLOYMENT: Planned for all evm networks, likely beginning with mainnet, polygon, gnosis chain, optimism, arbitrum
ERC20: none
ERC721: none
ERC777: none
FEE-ON-TRANSFER: none
REBASING TOKENS: none
ADMIN: n/a
EXTERNAL-ADMINS: n/a
```

### Q: Are there any additional protocol roles? If yes, please explain in detail

1) The roles
2) The actions those roles can take
3) Outcomes that are expected from those roles
4) Specific actions/outcomes NOT intended to be possible for those roles

A:

**Hat wearers**:

- Can renounce a Hat they are wearing
- Can approve tree linkage requests to a Hat they are wearing
- Top Hat wearers can request linkages to other trees
- Have admin rights on child, grandchild, great-grandchild, etc. Hats

**Hat admins**:

- Can create child Hats
- Can mint Hats to not-ineligible wearers
- Can change the properties of mutable Hats
- Can transfer mutable Hats to other wearers
- Can approve tree linkage requests to child Hats

**Hat eligibility modules**:

- Can prevent admins from minting or transferring a Hat to a given wearer
- Can revoke a wearer's Hat
- Can determine a Hat's wearer's standing (good or bad)
- Cannot mint or transfer a Hat
- Cannot toggle off a Hat

**Hat toggle modules**:

- Can toggle a Hat's status between inactive and active
- Cannot revoke a Hat from a specific wearer
- Cannot mint or transfer a Hat
- Cannot determine a Hat's wearer's standing (good or bad)

___

### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?

A: Hats Protocol fully implements the ERC1155 *interface*.

However, because Hats tokens are non-transferable by their owner ("wearer"), Hats Protocol does not implement the ERC1155 Token Receiver logic. As a result, it does not fully comply with the ERC1155 *standard*, so we say that Hats are ERC1155-similar tokens.

___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding

A:

1. Admins of a mutable Hat can change its properties and transfer it to other wearer. This does create some risk of admin abuse, but this is acceptable since the admin is the one delegating authorities to the Hat wearers in the first place. Immutable Hats exist to provide a lower trust option, where appropriate.
2. There is a griefing / DoS risk on low-fee chains where an attacker could mint all ~4 billion top hats, but this is both unlikely and acceptable.

____

### Q: Please provide links to previous audits (if any)

A: [hats-protocol/audits/TrustSecurity_HatsProtocol_v02.pdf](./hats-protocol/audits/TrustSecurity_HatsProtocol_v02.pdf)

___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)?

A: None
_____

### Q: In case of external protocol integrations, are the risks of an external protocol pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality

A: ACCEPTABLE

_____

## 2. Hats Signer Gate

```
DEPLOYMENT: Planned for all evm networks, likely beginning with mainnet, polygon, gnosis chain, optimism, arbitrum
ERC20: none
ERC721: none
ERC777: none
FEE-ON-TRANSFER: none
REBASING TOKENS: none
ADMIN: restricted — "Owner" can transfer ownership, set min and target thresholds, add new modules to the Safe, and (in Multi Hats Signer Gate) add new signer Hats. "Owner" cannot remove modules, signer Hats, or valid signers, and cannot execute arbitrary Safe transactions.
EXTERNAL-ADMINS: n/a
```

Please answer the following questions to provide more context:

### Q: Are there any additional protocol roles? If yes, please explain in detail

1) The roles
2) The actions those roles can take
3) Outcomes that are expected from those roles
4) Specific actions/outcomes NOT intended to be possible for those roles

**Signers**

- Can claim signer authority if a) they are wearing a valid signer Hat, and b) maxSigners has not been reached
- Can sign transactions on the Safe
- Can indirectly renounce their signing authority by renouncing their Hat
- Cannot change min or target thresholds
- Cannot add to or remove modules (including this contract) from the Safe
- Cannot change the Safe's threshold
- Cannot remove this contract as a guard from the Safe

___

### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?

A: No

___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding

A: None

____

### Q: Please provide links to previous audits (if any)

A: [hats-zodiac/audits/TrustSecurity_HatsProtocol_v02.pdf](./hats-zodiac/audits/TrustSecurity_HatsProtocol_v02.pdf)

___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)?

A: None
_____

### Q: In case of external protocol integrations, are the risks of an external protocol pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality

A: NOT ACCEPTABLE

# Audit scope

[hats-protocol @ fafcfdf046c0369c1f9e077eacd94a328f9d7af0](https://github.com/Hats-Protocol/hats-protocol/tree/fafcfdf046c0369c1f9e077eacd94a328f9d7af0)

- [hats-protocol/src/Hats.sol](hats-protocol/src/Hats.sol)
- [hats-protocol/src/HatsIdUtilities.sol](hats-protocol/src/HatsIdUtilities.sol)

[hats-zodiac @ 9455cc0957762f5dbbd8e62063d970199109b977](https://github.com/Hats-Protocol/hats-zodiac/tree/9455cc0957762f5dbbd8e62063d970199109b977)

- [hats-zodiac/src/HatsSignerGate.sol](hats-zodiac/src/HatsSignerGate.sol)
- [hats-zodiac/src/HatsSignerGateBase.sol](hats-zodiac/src/HatsSignerGateBase.sol)
- [hats-zodiac/src/HatsSignerGateFactory.sol](hats-zodiac/src/HatsSignerGateFactory.sol)
- [hats-zodiac/src/MultiHatsSignerGate.sol](hats-zodiac/src/MultiHatsSignerGate.sol)

# About Hats Protocol and Hats Signer Gate

## Hats Protocol

Hats Protocol is a protocol for DAO-native roles and credentials that supports revocable delegation of authority and responsibility.

Hats are represented on-chain by non-transferable tokens that conform to the ERC1155 interface. An address with a balance of a given Hat token "wears" that hat, granting them the responsibilities and authorities that have been assigned to the Hat by the DAO.

See more in the [Hats Protocol README](./hats-protocol/README.md) and [Hats Protocol documentation](https://docs.hatsprotocol.org).

## Hats Signer Gate

A contract that grants multisig signing rights to addresses wearing a given Hat, enabling on-chain organizations (such as DAOs) to revocably delegate constrained signing authority and responsibility to individuals.

See more in the [Hats Zodiac README](./hats-zodiac/README.md).
