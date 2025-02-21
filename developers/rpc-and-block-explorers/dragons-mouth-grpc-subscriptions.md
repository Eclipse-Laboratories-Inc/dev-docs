---
description: Streaming Account Updates for Backend Applications
---

# Dragon's Mouth gRPC Subscriptions

Dragon's Mouth is Triton's Geyser-fed gRPC interface that supports streaming:

* Account Writes
* Transactions
* Entries
* Block notifications
* Slot notifications

It also supports unary operations:

* getLatestBlockhash
* getBlockHeight
* getSlot
* isValidBlockhash

{% hint style="info" %}
You can find the official Triton docs: [https://docs.triton.one/](https://docs.triton.one/)
{% endhint %}

## Protocol files <a href="#protocol-files" id="protocol-files"></a>

You can find the latest version of protobuf files in:

* Github: [https://github.com/rpcpool/yellowstone-grpc/tree/master/yellowstone-grpc-proto/proto](https://github.com/rpcpool/yellowstone-grpc/tree/master/yellowstone-grpc-proto/proto)
* Rust Crate: [https://crates.io/crates/yellowstone-grpc-proto](https://crates.io/crates/yellowstone-grpc-proto)

## NodeJS/Typescript Client

You can include NodeJS Yellowstone gRPC client as a dependency by running the following command:

```
npm install --save @triton-one/yellowstone-grpc

# or, for yarn:

yarn add @triton-one/yellowstone-grpc
```

A sample Typescript/Nodejs client is available at [https://github.com/rpcpool/yellowstone-grpc/tree/master/examples/typescript](https://github.com/rpcpool/yellowstone-grpc/tree/master/examples/typescript).

### Initializing the client

Once you have installed the client dependency, you can initialize it as follows:

```typescript
import Client from "@triton-one/yellowstone-grpc";

const client = new Client("https://api.rpcpool.com:443", "<insert your token here>");

// now you can call the client methods, e.g.:

const version = await client.getVersion(); // gets the version information
console.log(version);
```

Please note that the client is asynchronous, so it is expected that all calls are executed inside an async block or async function.

### Subscription Streams

You can get updates and send requests through the _subscription stream_. You can create it by calling the `client.subscribe()` method:

```typescript
import { SubscribeRequest } from "@triton-one/yellowstone-grpc";

// Create a subscription stream.
const stream = client.subscribe();

// Collecting all incoming events.
stream.on("data", (data) => {
  console.log("data", data);
});

// Create a subscription request.
const request: SubscribeRequest = {
  // you can use the standard JSON request format here.
  // the following documentation describes available requests and filters.
  ...
};

// Sending a subscription request.
await new Promise<void>((resolve, reject) => {
  stream.write(request, (err) => {
    if (err === null || err === undefined) {
      resolve();
    } else {
      reject(err);
    }
  });
}).catch((reason) => {
  console.error(reason);
  throw reason;
});
```

## grpcurl Client

`grpcurl` is a good client for testing. You will also need the following two Protobuf proto files to describe the protocol:

Once you have these two downloaded, you can access Triton's staging environment with your token (contact Triton customer support for a token) to run gRPC requests:

```
./grpcurl \
  -proto geyser.proto \
  -d '{"slots": { "slots": { } }, "accounts": { "usdc": { "account": ["9wFFyRfZBsuAha4YcuxcXLKwMxJR43S7fPfQLusDBzvT"] } }, "transactions": {}, "blocks": {}, "blocks_meta": {}}' \
  -H "x-token: <token>" \
  api.rpcpool.com:443 \
  geyser.Geyser/Subscribe
```

## Rust Client

A sample Rust client is available at [https://github.com/rpcpool/yellowstone-grpc/tree/master/examples/rust](https://github.com/rpcpool/yellowstone-grpc/tree/master/examples/rust).

## Goland Client

A sample Golang client is available at [https://github.com/rpcpool/yellowstone-grpc/tree/master/examples/golang](https://github.com/rpcpool/yellowstone-grpc/tree/master/examples/golang).

## Examples of Subscription Requests

<details>

<summary>Subscribe to an account</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {
    "wsol/usdc": {
      "account": ["8BnEgHoWFysVcuFFX7QztDmzuH8r5ZFvyP3sYwn1XTh6"]
    }
  },
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": [],
  "commitment": CommitmentLevel.CONFIRMED
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": { "slots": {} }, "accounts": { "wsol/usdc": { "account": ["8BnEgHoWFysVcuFFX7QztDmzuH8r5ZFvyP3sYwn1XTh6"] } }, "transactions": {}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": [], "commitment": 1}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to an account with <code>account_data_slice</code></summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {},
  "accounts": {
    "usdc": {
      "owner": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"],
      "filters": [{
          "tokenAccountState": true
      }, {
          "memcmp": {
              "offset": 0,
              "data": {
                  "base58": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"
              }
          }
      }]
    }
  },
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "entry": {},
  "commitment": CommitmentLevel.CONFIRMED
  "accountsDataSlice": [{ "offset": 32, "length": 40 }],
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{
    "accounts": {
        "usdc": {
            "owner": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"],
            "filters": [{
                "token_account_state": true
            }, {
                "memcmp": {
                    "offset": 0,
                    "data": {
                        "base58": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"
                    }
                }
            }]
        }
    },
    "accounts_data_slice": [{ "offset": 32, "length": 40 }]
}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to a program</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {
    "solend": {
      "owner": ["So1endDq2YkqhipRh3WViPa8hdiSpxWy6z3Z6tMCpAo"]
    }
  },
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": [],
  "commitment": CommitmentLevel.PROCESSED
}
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": { "slots": {} }, "accounts": { "solend": {  "owner": ["So1endDq2YkqhipRh3WViPa8hdiSpxWy6z3Z6tMCpAo"] } }, "transactions": {}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": [], "commitment": 0}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to multiple programs</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {
    "programs": {
      "owner": [
        "So1endDq2YkqhipRh3WViPa8hdiSpxWy6z3Z6tMCpAo",
        "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin"
      ]
    }
  },
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": { "slots": {} }, "accounts": { "programs": {  "owner": [ "So1endDq2YkqhipRh3WViPa8hdiSpxWy6z3Z6tMCpAo", "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin"] } }, "transactions": {}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to all finalized non-vote and non-failed transactions</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {},
  "transactions": {
    "alltxs": {
      "vote": false,
      "failed": false
    }
  },
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": [],
  "commitment": CommitmentLevel.FINALIZED
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": { "slots": {} }, "accounts": {}, "transactions": { "alltxs": { "vote": false, "failed": false }}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": [], "commitment": 2}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to non-vote transactions mentioning an account</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {},
  "transactions": {
    "serum": {
      "vote": false,
      "accountInclude": [
        "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin"
      ]
    }
  },
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": { "slots": {} }, "accounts": {}, "transactions": { "serum": { "vote": false, "account_include": [ "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin" ]}}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to transactions excluding accounts</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {},
  "transactions": {
    "serum": {
      "accountExclude": [
        "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin",
        "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
      ]
    }
  },
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": { "slots": {} }, "accounts": {}, "transactions": { "serum": { "account_exclude": [ "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin", "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA" ]}}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to transactions mentioning accounts &#x26; excluding certain accounts</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {},
  "transactions": {
    "serum": {
      "accountInclude": [
        "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin"
      ],
      "accountExclude": [
        "9wFFyRfZBsuAha4YcuxcXLKwMxJR43S7fPfQLusDBzvT"
      ]
    }
  },
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": { "slots": {} }, "accounts": {}, "transactions": { "serum": { "account_include": [ "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin" ], "account_exclude": [ "9wFFyRfZBsuAha4YcuxcXLKwMxJR43S7fPfQLusDBzvT" ] }}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to a transaction signature</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
const request = {
  "slots": {},
  "accounts": {},
  "transactions": {
    "sign": {
      "signature": "5rp2hL9b6kexex11Mugfs3vfU9GhieKruj4CkFFSnu52WLxiGn4VcLLwsB62XURhMmT1j4CZiXT6FFtYbXsLq2Zs"
    }
  },
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": {}, "accounts": {}, "transactions": { "sign": { "signature": "5rp2hL9b6kexex11Mugfs3vfU9GhieKruj4CkFFSnu52WLxiGn4VcLLwsB62XURhMmT1j4CZiXT6FFtYbXsLq2Zs"}}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to slots</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
const request = {
  "slots": {
    "incoming_slots": {}
  },
  "accounts": {},
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": { "incoming_slots": {} }, "accounts": {}, "transactions": {}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to blocks</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
const request = {
  "slots": {},
  "accounts": {},
  "transactions": {},
  "blocks": {
    "blocks": {}
  },
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": {}, "accounts": { }, "transactions": {}, "blocks": { "blocks": {} }, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endcode %}

</details>

<details>

<summary>Subscribe to block metadata</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
const request = {
  "slots": {},
  "accounts": {},
  "transactions": {},
  "blocks": {},
  "blocksMeta": {
    "blockmetadata": {}
  },
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": {}, "accounts": {}, "transactions": {}, "blocks": {}, "blocks_meta": { "blockmetadata": {} }, "accounts_data_slice": []}
```
{% endcode %}

</details>

<details>

<summary>Unsubscribing</summary>

{% code title="NodeJS" overflow="wrap" %}
```typescript
const request = {
  "slots": {},
  "accounts": {},
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endcode %}

{% code title="gRPC" overflow="wrap" %}
```
{"slots": {}, "accounts": {}, "transactions": {}, "blocks": {}, "blocks_meta": {}}
```
{% endcode %}

</details>

## Managing commitment levels <a href="#managing-commitment-levels" id="managing-commitment-levels"></a>

The gRPC streams happen by default on the processed commitment level.

Triton also supports specifying confirmed and finalized commitment levels. In these cases, Dragon's Mouth will buffer the incoming updates for you and release them once the updates have become confirmed or finalized.

For maximum performance, however, it is recommended to use handling commitment levels client side.

To specify commitment level in your Dragon's Mouth gRPC calls provide the following values:

```
enum CommitmentLevel {
  PROCESSED = 0;
  CONFIRMED = 1;
  FINALIZED = 2;
}
```

### Benefits of working at processed <a href="#benefits-of-working-at-processed" id="benefits-of-working-at-processed"></a>

The benefit of working on processed is that you can process transactions as soon as they arrive, but only commit to them once you know whether they are confirmed or finalized. This means that you can get faster response times in your UI by doing a lot of the processing work at a lower commitment level and then be able to surface the changes as soon as you see that the event is committed.

### How to manage \`confirmed\` and \`finalized\` <a href="#how-to-manage-confirmed-and-finalized" id="how-to-manage-confirmed-and-finalized"></a>

To manage confirmed and finalized you need to buffer events by slot. Each event (transaction or account write) will have a slot attached to it. You store these events in a buffer ordered by slot.

You then also make sure you subscribe to [slot notifications](https://docs.triton.one/project-yellowstone/dragons-mouth-grpc-subscriptions#subscribe-to-slots). This will give you information about when a slot is confirmed or finalized. Depending on the commitment level you are interested in, you should release your buffer when you receive the slot notification for a particular slot at a particular commitment level.

You will receive all the transaction notifications or account write notifications for the slot **before** you receive the "confirmed" and "finalized" notification for that slot.

### The special thing about finalized <a href="#the-special-thing-about-finalized" id="the-special-thing-about-finalized"></a>

Unfortunately, due to a quirk in the way that Geyser works on SVM not every slot finalized notification is issued. This means that you need some special processing if you want to handle finalized correctly.

The special handling is the following: whenever you see a `finalized` slot notification, you need to check the parents and grand parents (and great-grandparents and so on) of that slot and mark those as finalized too even if you didn't receive a notification for them.
