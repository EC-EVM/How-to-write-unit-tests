# How to write unit tests for solidity smart contracts

### Tips for creating tests

To create effective unit tests for Solidity smart contracts, focus on comprehensive coverage, 
including both expected behaviors and edge cases, while keeping tests modular, independent, 
and security-focused. Use Test Driven Development (TDD) to guide your development, and integrate automated 
testing into your workflow for continuous validation. Monitor gas usage, verify events, 
and handle time-dependent logic properly. Additionally, employ assertive testing libraries, 
check for reverts, and utilize mock contracts to simulate interactions, 
ensuring your contract is thoroughly validated before deployment.

### Diagram of Ballot.sol
<img src="https://github.com/user-attachments/assets/e35b6c06-3d8e-420a-b58e-82f7945f8e09" width="500" >


### Identify key test scenarios
1. Deployment Tests<br />
Check Initial Setup: Verify that the contract is deployed with the correct number of proposals, and each proposal's name is correctly stored.<br />
Chairperson Assignment: Ensure that the contract assigns the deployer as the chairperson with the voting rights.<br />
2. Voting Rights Management<br />
Grant Voting Rights: Test that only the chairperson can give voting rights to an address and that they can't grant rights to someone who already voted.<br />
Prevent Unauthorized Voting Rights: Verify that non-chairperson accounts cannot grant voting rights.<br />
3. Voting Process<br />
Successful Vote: Ensure that a voter can successfully vote for a proposal, and the vote is correctly recorded.<br />
Double Voting Prevention: Test that the contract prevents a voter from voting more than once.<br />
Voting Without Rights: Ensure that users without voting rights cannot vote.<br />
4. Delegate Voting<br />
Successful Delegation: Verify that a voter can delegate their vote to another voter, and the delegation is correctly handled.<br />
Self-Delegation Prevention: Test that the contract prevents self-delegation.<br />
Circular Delegation Prevention: Ensure that the contract detects and prevents circular delegation chains.<br />
5. Winning Proposal Determination<br />
Correct Winner: After voting, verify that the contract correctly determines the winning proposal based on the highest vote count.<br />
Multiple Proposals with Same Votes: Test the behavior when two or more proposals receive the same number of votes, ensuring consistency.<br />
6. Edge Cases<br />
Zero Proposals: Ensure that the contract handles the case where no proposals are provided during deployment.<br />
No Votes Cast: Verify the contract’s behavior if no one votes, ensuring it doesn't crash when determining a winner.<br />
All Votes Delegated: Test scenarios where all voters delegate their votes to ensure that the votes are correctly counted.<br />
7. Security Tests<br />
Reentrancy Attack Prevention: Ensure the contract is safe against reentrancy attacks during voting or delegation.<br />
Gas Limit Testing: Test the contract’s behavior under high gas usage scenarios, especially with complex delegation chains.<br />


### Start by Importing
1. Import the essential utilities for writing and executing Solidity smart contract tests within the Hardhat environment:

  **expect** from Chai is used for assertions in tests.<br/>
  **toHex** and **hexToString** from **viem** convert data to and from hexadecimal format.<br/>
  **viem** provides tools for interacting with contracts in Hardhat.<br/>
  **loadFixture** helps in efficiently setting up and reusing test scenarios.<br/>

  ```typescript
  import { expect } from "chai";
  import { toHex, hexToString } from "viem";
  import { viem } from "hardhat";
  import { loadFixture } from "@nomicfoundation/hardhat-network-helpers";

  const PROPOSALS = ["ramen", "pizza", "burger"];
  ```
  **PROPOSALS** is an array of proposal strings used for contract input data.

2. Set up the testing environment

   The **deployContract** function sets up the testing environment by deploying the **Ballot** smart contract to the blockchain.
   It initializes a public client for blockchain interactions and retrieves two wallet clients:
   one for deploying the contract and another for additional interactions.
   The function converts proposal data into the required hexadecimal format,
   deploys the contract with these proposals, and returns an object containing the public client,
   deployer account, additional account, and the deployed contract instance for use in tests.

* Deploying contract in the Fixture function

```typescript
async function deployContract() {
  const publicClient = await viem.getPublicClient();
  const [deployer, otherAccount] = await viem.getWalletClients();
  const ballotContract = await viem.deployContract("Ballot", [
    PROPOSALS.map((prop) => toHex(prop, { size: 32 })),
  ]);
  return { publicClient, deployer, otherAccount, ballotContract };
}
```

3. Prepare a starting structure for your tests

### Starting structure for tests

```typescript
import { expect } from "chai";
import { toHex, hexToString } from "viem";
import { viem } from "hardhat";
import { loadFixture } from "@nomicfoundation/hardhat-network-helpers";

const PROPOSALS = ["ramen", "pizza", "burger"];

async function deployContract() {
  const publicClient = await viem.getPublicClient();
  const [deployer, otherAccount] = await viem.getWalletClients();
  const ballotContract = await viem.deployContract("Ballot", [
    PROPOSALS.map((prop) => toHex(prop, { size: 32 })),
  ]);
  return { publicClient, deployer, otherAccount, ballotContract };
}

describe("Ballot", async () => {
  describe("when the contract is deployed", async () => {
    it("has the provided proposals", async () => {
      // TODO
      throw Error("Not implemented");
    });

    it("has zero votes for all proposals", async () => {
      // TODO
      throw Error("Not implemented");
    });
    it("sets the deployer address as chairperson", async () => {
      // TODO
      throw Error("Not implemented");
    });
    it("sets the voting weight for the chairperson as 1", async () => {
      // TODO
      throw Error("Not implemented");
    });
  });

  describe("when the chairperson interacts with the giveRightToVote function in the contract", async () => {
    it("gives right to vote for another address", async () => {
      // TODO
      throw Error("Not implemented");
    });
    it("can not give right to vote for someone that has voted", async () => {
      // TODO
      throw Error("Not implemented");
    });
    it("can not give right to vote for someone that has already voting rights", async () => {
      // TODO
      throw Error("Not implemented");
    });
  });

  describe("when the voter interacts with the vote function in the contract", async () => {
    // TODO
    it("should register the vote", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when the voter interacts with the delegate function in the contract", async () => {
    // TODO
    it("should transfer voting power", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when an account other than the chairperson interacts with the giveRightToVote function in the contract", async () => {
    // TODO
    it("should revert", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when an account without right to vote interacts with the vote function in the contract", async () => {
    // TODO
    it("should revert", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when an account without right to vote interacts with the delegate function in the contract", async () => {
    // TODO
    it("should revert", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when someone interacts with the winningProposal function before any votes are cast", async () => {
    // TODO
    it("should return 0", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when someone interacts with the winningProposal function after one vote is cast for the first proposal", async () => {
    // TODO
    it("should return 0", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when someone interacts with the winnerName function before any votes are cast", async () => {
    // TODO
    it("should return name of proposal 0", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when someone interacts with the winnerName function after one vote is cast for the first proposal", async () => {
    // TODO
    it("should return name of proposal 0", async () => {
      throw Error("Not implemented");
    });
  });

  describe("when someone interacts with the winningProposal function and winnerName after 5 random votes are cast for the proposals", async () => {
    // TODO
    it("should return the name of the winner proposal", async () => {
      throw Error("Not implemented");
    });
  });
});
```



