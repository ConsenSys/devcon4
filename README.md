<p align="center">
	<img src="/static/notrekt-logo.png" height="200px"/>
</p>

# How To Not Get Rekt: Smart Contract Secure SDLC for Developers

## Preparation

Here's how to get set up for the workshop. It should be super easy except if you're using Windows.

First, you need a web3 capable browser and some testnet ETH. You probably also have both, but if not, get [Metamask](https://metamask.io) and grab some ETH from the Ropsten faucets:

- https://faucet.metamask.io/
- https://faucet.ropsten.be/

The workshop exercises are hosted in an separate repo. Get a local copy by cloning the repo:

```
$ git clone https://github.com/ConsenSys/mythx-playground/
```

In this workshop you'll get to know both [MythX](https://mythx.io) as well as the latest build of [Mythril](https://github.com/ConsenSys/mythril-classic).

If you run into insurmountable problems ask the instructors for help. There's also a dedicated [Discord channel](https://discord.gg/kGDd8FP) that we created exclusively for you, the valued workshop participant.

### Setting up MythX for Remix

TODO

### Setting up MythX for Truffle

TODO

## Part 1 - The Smart Contract Secure SDLC

TL;DR: Security should be incorporated during all phases of development. This info graphic sums things up nicely:

<p align="center">
	<img src="/static/sdlc.png" height="400px"/>
</p>

## Part 2 - Threat Modeling

Before and during buidling a smart contract system you should think about potential threats and countermeasures. This process is known as threat modeling. In part 2, [Gerhard Wagner](https://twitter.com/g3rh4rdw4gn3r) we'll do a presentation on threat modeling basics. Here's the workshop material:

- [Presentation slides](slides/How_to_Not_Get_Rekt_Volume_1_Threat_Modeling.pdf)
- [Blockchain incident threat list](threat-modeling/threat_list_blockchain_incident_db.md)

**Threat Modeling Exercise:**

After all this theory it's time for some hands-on action. Pick one of the following options:

- [Build a threat model for your own smart contract system](threat-modeling/exercise_your_own_system.md)
- [Build a threat model for Crypto Froggies](threat-modeling/exercise_sample_system.md)

## Part 3 - The Real Fun Begins

For the remainder of the workshop we'll be looking at different ways of identifying, fixing and preventing vulnerabilities during development.

### Integer Arithmetic Bugs

TODO

### Reentrancy

The infamous TheDAO was exploited by reentrancy in 2016. Although the community is more aware of it, reentrancy struck back in October 2018. 

TODO

### Broken Access Controls

TODO

Note the "SWC" identifier at the end: That's a reference to the [Smart Contract Weakness Classification (EIP 1470)](https://smartcontractsecurity.github.io/SWC-registry/). You can look up details about each issue there.

Now things will get serious! We'll take the attacker's side and exploit this exact contract. Depending on your level of skill, pick one of the following:

- Easy: [Token](https://ethernaut.zeppelin.solutions/level/0x6545df87f57d21cb096a0bfcc53a70464d062512)
- A bit more difficult: [TokenWhale](https://capturetheether.com/challenges/math/token-whale/).

### Weak Random Numbers

TODO

## Security Tests in CI

Ideally we want to know when vulnerabilities are introduced into the code base before they end up in the master branch.  In [exercise 3](https://github.com/ConsenSys/devcon4-playground/tree/master/exercise3), we'll try the [Guardrails Github app](https://github.com/apps/guardrails) (Make sure to allow Issues in the repository settings as Guardrails will create issues), which should help you detect the issue. It's on you to fix it. Clone the [devcon4-playground](https://github.com/ConsenSys/devcon4-playground/) repository and create a new PR to resolve the issue that we exploited in Part 1.

## Custom Assertions

The Solidity `assert()` statement is used to specify conditions that are expected to *always* hold. If you want to prove certain assumptions about your code, you can put them into asserts and use Mythril's symbolic execution engine to do all the hard work for you.

MythX will detect reachable exceptions by default. To *only* search for exceptions, add the `-mexceptions` option.

In [exercise 5](https://github.com/ConsenSys/devcon4-playground/tree/master/exercise5), we'll write a test to (dis-)prove an invariant in a [token contract](https://github.com/ConsenSys/devcon4-playground/blob/master/exercise5/token-with-backdoor.sol). The token code sample was shown by Josselin Feist on TruffleCon 2018 (shouts to our friends from [Trail of Bits](https://www.trailofbits.com)).

The invariant we want to verify is that a user's balance can never exceed 1,000. A nice way to do this is to create a separate test contract that inherits from the contract to be tested. Create a new file named `token-with-backdoor-test.sol` in the `exercise5` directory and add a function containing the assertion that balances[msg.sender] must never exceed 1,000:

```solidity
import "./token-with-backdoor.sol";

contract TokenTest is Token {

    // Verify that the user's balance can never exceed 1000

    function verify_balance() {
        assert(balances[msg.sender] <= 1000);
    }

}
```

TODO


## Credit

Created by [ConsenSys Diligence](https://consensys.net/diligence/) and the [Mythril](https://mythril.ai) team. Special thanks to Mick Ayzenberg ([Security Innovation](https://www.securityinnovation.com)), [Trail of Bits](https://www.trailofbits.com), Steve Marx of [CaptureTheEther](https://capturetheether.com) and ConsenSys fame and [Zeppelin Solutions](https://zeppelin.solutions) for vulnerable contract samples and challenges. Also, kudos to the [Truffle](https://truffleframework.com) and [Guardrails](https://www.guardrails.io) teams for working with us on Mythril Platform integration.
