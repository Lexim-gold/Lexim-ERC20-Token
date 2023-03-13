# Lexim ERC20 Token 
Lexim-issued ERC20 token backed by physical gold smart contract repository.

https://lexim.gold

## Contract Specification

Lexim-issued ERC20 is an ERC20 token that is Centrally Minted and Burned by Lexim,
representing the physical ownership of gold bars.

### ERC20 Token

The public interface of Lexim-issued ERC20 is the ERC20 interface
specified by [EIP-20](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md).

- `name()`
- `symbol()`
- `decimals()`
- `totalSupply()`
- `balanceOf(address who)`
- `transfer(address to, uint256 value)`
- `approve(address spender, uint256 value)`
- `allowance(address owner, address spender)`
- `transferFrom(address from, address to, uint256 value)`

And the usual events.

- `event Transfer(address indexed from, address indexed to, uint256 value)`
- `event Approval(address indexed owner, address indexed spender, uint256 value)`

Typical interaction with the contract will use `transfer` to move the token as payment.
Additionally, a pattern involving `approve` and `transferFrom` can be used to allow another 
address to move tokens from your address to a third party without the need for the middleperson 
to custody the tokens, such as in the 0x protocol. 

### Controlling the token supply

The total supply of Lexim-issued ERC20 is backed by physical gold under the custody of Lexim.
To make gold bar management more transparent we support a list of gold bar ids - unique gold bar serial numbers - that are used as backing for ERC20. The list is stored on the blockchain an available via the smart contract. Specifically via smart contract method

- `getGoldBars`

Gold bar id list change Events:
- `event GoldBarIdAdded(string indexed goldBarId)`
- `event GoldBarIdRemoved(string indexed goldBarId)`

### Pausing the contract

In the event of a critical security threat, Lexim has the ability to pause transfers of the Lexim token. The ability to pause is controlled by an address with a PAUSER_ROLE.

The simple model for pausing transfers following OpenZeppelin's
[Pausable](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Pausable).

### Fees

Lexim charges a set fee rate for all on-chain transfers of Lexim-issued ERC20 in order to offset storage fees of gold bars in our vaults.
The fee controller has the ability to set the fee accumulator and the fee rate (measured in 1/100th of a basis point).
Lexim will never change the fee rate without prior notice as we take transparency very seriously.

### Asset Controller Role

As required by our regulators, we have introduced a role for asset protection to freeze or seize the assets of a criminal party when required to do so by law, including by court order or other legal process.

The `CONTROLLER_ROLE` can freeze and unfreeze the Lexim-issued ERC20 balance of any address on chain.

Freezing is something that Lexim will not do on its own accord, and as such we expect to happen extremely rarely.

### Permits

In order to allow for gas-less transactions we have implemented support for EIP-2612 based on https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Permit.

### Upgradeability Proxy
To facilitate upgradeability on the immutable blockchain we follow a ERC-1822: Universal Upgradeable Proxy Standard based on OpenZeppelin implemetation.

