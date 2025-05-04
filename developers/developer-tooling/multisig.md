# Squads (Multisig)

Squads Protocol is a collection of programs (smart contracts) that enables developers to use smart account technology to build novel asset management and self-custody solutions.

### Program ID <a href="#program-id" id="program-id"></a>

<table><thead><tr><th width="192">Network</th><th>Program address</th></tr></thead><tbody><tr><td>Eclipse Mainnet</td><td><code>eSQDSMLf3qxwHVHeTr9amVAGmZbRLY2rFdSURandt6f</code></td></tr><tr><td>Eclipse Devnet</td><td><code>eSQDSMLf3qxwHVHeTr9amVAGmZbRLY2rFdSURandt6f</code></td></tr></tbody></table>

### Create Multisig <a href="#getting-started" id="getting-started"></a>

#### Public UI <a href="#getting-started" id="getting-started"></a>

The [public UI](https://backup.app.squads.so/create) is recommended. Navigate to settings and set the RPC URL to a valid [Eclipse endpoint](https://docs.squads.so/main/development/cli/installation), and the program ID to the one above.&#x20;

#### CLI

Follow [these steps](https://docs.squads.so/main/development/cli/installation) to install the CLI. Set the RPC URL to a valid [Eclipse endpoint](https://docs.squads.so/main/development/cli/installation), and the program ID to the one above.&#x20;

If you intend to create a Squad via the CLI, here's an example command that you would run:

{% code overflow="wrap" %}
```bash
squads-multisig-cli multisig-create --keypair <path-to-your-keypair> --members <member-1-key>,<permissions> --members <member-2-key>,<permissions> --threshold <threshold> --program-id eSQDSMLf3qxwHVHeTr9amVAGmZbRLY2rFdSURandt6f --rpc-url 
https://staging-rpc.dev2.eclipsenetwork.xyz
```
{% endcode %}

Permissions are numbers, which map to the following:

* Proposer - `1`
* Voter - `2`
* Executor - `4`
* All of the above - `7`
* Or any combination of permissions (i.e Proposer & Voter would be `3`)&#x20;

So an example member entry would look like this:

```bash
--members FcBpwMquaMURbYwpRFUrBrYgFwJzfWiBEGfHLbik1Wsm,7
```

{% hint style="info" %}
For more info on Squads, check out the [official docs](https://docs.squads.so/main/development).
{% endhint %}
