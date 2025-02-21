# Eclipse Program Registry Guide

### What is the Eclipse Program Registry?[​](https://icarus131.github.io/devcookbook/docs/ProgramRegistry#what-is-the-eclipse-program-registry) <a href="#what-is-the-eclipse-program-registry" id="what-is-the-eclipse-program-registry"></a>

The Eclipse Program Registry serves as a somewhat comprehensive list of programs running on the Eclipse network. It can be found [here](https://github.com/Eclipse-Laboratories-Inc/program-registry).

This guide will walk you through the steps necessary to add your program to the registry.

#### Prerequisites[​](https://icarus131.github.io/devcookbook/docs/ProgramRegistry#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

Before you submit your program to the registry, ensure you have:

* A program deployed on the Eclipse blockchain.
* A public GitHub repository for your program's code.
  * Your repository should be prepared according to [this guide](https://solana.com/developers/guides/advanced/verified-builds#prepare-project) so the program can be verified against the source code.
* A published IDL (Interface Definition Language) on-chain.
  * The Anchor CLI docs [show you how to do this](https://www.anchor-lang.com/docs/cli#idl-init). You'll need to use `init`if it's your first time, or `upgrade`if you're updating the IDL.
  * When done correctly, a published IDL will look like [this](https://eclipsescan.xyz/account/LfacfEjtujQTWBXZVzgkiPBw7Mt4guHSsmAi7y3cycL#anchorProgramIdl) on our explorer.

#### Submission Process[​](https://icarus131.github.io/devcookbook/docs/ProgramRegistry#submission-process) <a href="#submission-process" id="submission-process"></a>

To submit your program to the Eclipse Program Registry, you need to create a pull request (PR) with the following items completed:

1. **Update programs.yaml File**

Add an entry for your program in the `programs.yaml` file. The example below uses our Canonical Bridge. Replace the info with your program's details.

```
- name: canonical_bridge
  description: The Eclipse Canonical Bridge facilitates depositing and withdrawing ether from the Eclipse Chain
  repo: https://github.com/Eclipse-Laboratories-Inc/syzygy/tree/main/solana-programs/canonical_bridge
  icon: https://i.imgur.com/y0JEPfQ.png
  framework: Anchor
  program_address: br1xwubggTiEZ6b7iNZUwfA3psygFfaXGfZ1heaN9AW
  categories:
    - Bridge
```

#### After Submission[​](https://icarus131.github.io/devcookbook/docs/ProgramRegistry#after-submission) <a href="#after-submission" id="after-submission"></a>

**After your pull request is created:**[**​**](https://icarus131.github.io/devcookbook/docs/ProgramRegistry#after-your-pull-request-is-created)

* The Eclipse team will review your submission.
* If all criteria are met, your PR will be merged.
* Upon successful merging, your program will be eligible to be featured in the Eclipse Program Explorer.
