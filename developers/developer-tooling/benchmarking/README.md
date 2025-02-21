---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Benchmarking

To test the performance of Eclipse Testnet, you can run our benchmarking script. The following test benchmarks AMM swaps to assess the TPS of Eclipse Testnet.

## Prerequisites[​](https://icarus131.github.io/devcookbook/docs/Benchmarking#pre-requisites) <a href="#pre-requisites" id="pre-requisites"></a>

* [node.js](https://nodejs.org/en/download/)
* [git](https://git-scm.com/downloads)

Once you have the above installed, clone the benchmarking repository:

```bash
git clone https://github.com/Eclipse-Laboratories-Inc/eclipse-benchmarking/
```

## Running Benchmarking Test[​](https://icarus131.github.io/devcookbook/docs/Benchmarking#running-the-benchmarking-tests) <a href="#running-the-benchmarking-tests" id="running-the-benchmarking-tests"></a>

Navigate to the `token_swap` folder after cloning the repository. Install ts-node by running the following command:

```bash
npm i -g ts-node
```

{% hint style="warning" %}
You might have to give super user or admin permissions to install globally or use the `-g` flag.
{% endhint %}

To install the dependencies, run the following command:

```bash
npm i
```

Make sure to run this command inside the `token_swap` folder. The final step is to run the benchmarking tests using the following command:

```bash
ts-node spam.ts
```

This runs 10 instances of an AMM performing any specified number of swaps.

## Modifying Benchmarking[​](https://icarus131.github.io/devcookbook/docs/Benchmarking#modifying-the-benchmarking-tests) <a href="#modifying-the-benchmarking-tests" id="modifying-the-benchmarking-tests"></a>

The default test is 275 swaps. To modify the number of swaps, modify line 63 in the `benchmark.ts` file inside the `token_swap` folder. The following is the code snippet to modify:

```javascript
    ...
    await mintTo(connection, payer, mintA, userAccountA, owner, SWAP_AMOUNT_IN);

    console.log("Run test: benchmark swap");
    await benchmarkSwap(275);

    console.log("Success\n");
```

If you've followed the steps successfully, you should see an output similar to the following:&#x20;

<figure><img src="https://icarus131.github.io/devcookbook/assets/images/benchmarking-371ec579dea9b15878bbb125fda5211f.jpg" alt=""><figcaption></figcaption></figure>
