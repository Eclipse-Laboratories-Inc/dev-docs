# AllDomains

## **What is AllDomains?**

**AllDomains** is a Web3 platform to create, register, and trade customizable Web3 Identities.

With AllDomains any user can choose not only a name they desire but also the community they wish to associate with, adding a new layer of customization to their Web3 identity. Be it a Token, an NFT collection, or a DAO, **everyone can create their own TLD** and unite their whole community under a unique brand.

* You can register a domain at: [https://alldomains.id/](https://alldomains.id/)
* AllDomains documentation: [https://docs.alldomains.id/protocol](https://docs.alldomains.id/protocol)

{% hint style="info" %}
If you want urgent help with alldomains, please go to their discord and open a ticket: [https://discord.com/invite/alldomains](https://discord.com/invite/alldomains)
{% endhint %}

{% hint style="info" %}
You can also reach the alldomains team directly on the Eclipse discord by tagging the role "alldomains"
{% endhint %}

## AllDomains - Eclipse SDK

### How to integrate AllDomains Protocol: <a href="#how-to-integrate-alldomains-protocol" id="how-to-integrate-alldomains-protocol"></a>

Libraries are open source with MIT licenses:&#x20;

* Typescript: [https://github.com/onsol-labs/tld-parser](https://github.com/onsol-labs/tld-parser)
* Rust: [https://github.com/onsol-labs/tld-parse](https://github.com/onsol-labs/tld-parser)[-rs](https://github.com/onsol-labs/tld-parser-rs)

### **1. Installation**

```
npm install @onsol/tldparser
```

### **2. Domain Resolution**

The following code shows how to get the Public Key owner of a domain name

* Works with **any AllDomains TLD**

```javascript
import { TldParser } from "@onsol/tldparser";
import { Connection } from "@solana/web3.js";

const RPC_URL = 'https://mainnetbeta-rpc.eclipse.xyz';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get the owner pubkey of a domain name
async function resolveDomain(domain){

    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    return await parser.getOwnerFromDomainTld(domain);
}

//get the owner pubkey of "miester.turbo";
resolveDomain("miester.turbo");
```

### **3. Get domains owned by a Public Key**

#### **a. Get all owned domains**

```javascript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://mainnetbeta-rpc.eclipse.xyz';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get all the domains owned by a public key
async function getOwnedDomains(owner: PublicKey){

    // initialize a Tld Parser
    const parser = new TldParser(connection);

    // get all owned domains
    let allUserDomains = await parser.getParsedAllUserDomains(owner);

    return allUserDomains;
}

// get all owned domains
getOwnedDomains(new PublicKey(""));
```

#### **b. Get only unwrapped owned domains**

```javascript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://mainnetbeta-rpc.eclipse.xyz';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get only the unwrapped domains owned by a public key
async function getOwnedUnwrappedDomains(owner: PublicKey){

    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    // get only unwrapped domains
    let unwrappedUserDomains = await parser.getParsedAllUserDomainsUnwrapped(owner);

    return unwrappedUserDomains;
}

// get only unwrapped owned domains
getOwnedUnwrappedDomains(new PublicKey(""));
```

### **4. Get domains owned by a Public Key from a specific TLD**

#### **a. Get all owned domains from a specific TLD**

```javascript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://mainnetbeta-rpc.eclipse.xyz';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get the all the domains owned by a public key in a TLD
async function getOwnedDomainsFromTld(owner, tld){

    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    // get all owned domains from a TLD
    let ownedDomainsFromTld = parser.getParsedAllUserDomainsFromTld(owner, tld);

    return ownedDomainsFromTld;
}

// get all owned domains in the ".turbo" Tld, without the "."
getOwnedDomainsFromTld(new PublicKey(""), "turbo");
```

#### **b. Get all only unwrapped owned domains from a specific TLD**

```javascript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://mainnetbeta-rpc.eclipse.xyz';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get only the unwrapped domains owned by a public key from a TLD
async function getOwnedUnwrappedDomainsFromTld(owner, tld){

    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    // get only unwrapped domains from a TLD
    let ownedUnwrappedDomainsFromTld = parser.getParsedAllUserDomainsFromTldUnwrapped(owner, tld);

    return ownedUnwrappedDomainsFromTld;
}

// get owned unwraped domains in the ".turbo" Tld, without the "."
getOwnedUnwrappedDomainsFromTld(new PublicKey(""), "turbo");
```

### **5. Get all active AllDomains TLDs**

```javascript
import { getAllTld } from "@onsol/tldparser";
import { Connection } from "@solana/web3.js";

const RPC_URL = 'https://mainnetbeta-rpc.eclipse.xyz';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get all active AllDomains TLDs
const allTlds = await getAllTld(connection);
```

### **6. Get all domains registered in All TLDs**

```javascript
import { NameRecordHeader, TldParser, findAllDomainsForTld, getAllTld } from "@onsol/tldparser";
import { Connection } from "@solana/web3.js";

const RPC_URL = 'https://mainnetbeta-rpc.eclipse.xyz';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// slow
// please use no. 8 for a batch/faster implementation 
// this code doesn't check if a domain is expired or not
async function getAllRegisteredDomains(connection: Connection){
    //get all TLDs
    const allTlds = await getAllTld(connection);
    const parser = new TldParser(connection);
    let domains = [];
    for (let tld of allTlds) {

        //get the parent name record for a TLD
        const parentNameRecord = await NameRecordHeader.fromAccountAddress(connection, tld.parentAccount);
            
        //get all name accounts in a specific TLD
        const allNameAccountsForTld = await findAllDomainsForTld(connection, tld.parentAccount);

        if (!parentNameRecord || !parentNameRecord.owner) return;
        
        //parse all name accounts in a specific TLD
        for (let nameAccount of allNameAccountsForTld) {

            //get domain as string without the tld
            const domain = await parser.reverseLookupNameAccount(nameAccount, parentNameRecord.owner);
            domains.push(                {
                nameAccount: nameAccount,
                domain: `${domain}${tld.tld}`
            });
        }
    }
    return domains;
}

// get all domains registered on AllDomains
console.log(await getAllRegisteredDomains(connection))
```

### 7. Get User Main Domain

```javascript
import { MainDomain, findMainDomain } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";


// returns MainDomain struct if it is available. it will return undefined if it is not.
// MainDomains are usually shown as mainDomain.domain + mainDomain.tld 
// the mainDomain.nameAccount is proof that the user has set it.
const fetchMainDomain = async (connection: Connection, pubkey: string | PublicKey): Promise<MainDomain | undefined> => {
    if (typeof pubkey === "string") {
        pubkey = new PublicKey(pubkey)
    }
    const [mainDomainPubkey] = findMainDomain(pubkey);
    let mainDomain = undefined;
    try {
        mainDomain = await MainDomain.fromAccountAddress(
            connection,
            mainDomainPubkey,
        );
        return mainDomain
    } catch (e) {
        console.log("No main domain found")
    }
    return mainDomain
};


const CONNECTION = new Connection("");
console.log(await fetchMainDomain(CONNECTION, "2EGGxj2qbNAJNgLCPKca8sxZYetyTjnoRspTPjzN2D67"))

// MainDomain {
//     nameAccount: PublicKey [PublicKey(9YzfCEHb62bQ47snUyjkxhC9Eb6y7CSodK3m8CKWstjV)] {
//       _bn: <BN: 7f0fb1f72ae0af9c5e7f5e4190d02ed2a720e88fb5787425157b9a9ec3fc39ec>
//     },
//     tld: '.abc',
//     domain: 'miester'
//   }
```

### **8. Batch get AllDomains Domains**

```javascript
import {
    ANS_PROGRAM_ID,
    findNameHouse,
    findNftRecord,
    getAllTld,
    getHashedName,
    getNameAccountKeyWithBump,
    NameRecordHeader,
    NftRecord,
    TldParser,
} from "@onsol/tldparser";
import {
    Connection,
    GetProgramAccountsResponse,
    PublicKey,
} from "@solana/web3.js";
import { setTimeout } from "timers/promises";

const connection = new Connection("");

async function findAllDomainsForTld(
    connection: Connection,
    parentAccount: PublicKey,
): Promise<{ pubkey: PublicKey; nameRecordHeader: NameRecordHeader }[]> {
    const filters: any = [
        {
            memcmp: {
                offset: 8,
                bytes: parentAccount.toBase58(),
            },
        },
    ];

    const accounts: GetProgramAccountsResponse =
        await connection.getProgramAccounts(ANS_PROGRAM_ID, {
            filters: filters,
        });
    return accounts.map((a) => {
        return {
            pubkey: a.pubkey,
            nameRecordHeader: NameRecordHeader.fromAccountInfo(a.account),
        };
    });
}

export async function performReverseLookupBatched(
    connection: Connection,
    nameAccounts: PublicKey[],
    tldHouse: PublicKey,
): Promise<(string | undefined)[]> {
    let reverseLookupDomains: (string | undefined)[] = [];

    while (nameAccounts.length > 0) {
        const currentBatch = nameAccounts.splice(0, 100);

        const promises = currentBatch.map(async (nameAccount) => {
            const reverseLookupHashedName = await getHashedName(
                nameAccount.toBase58(),
            );
            const [reverseLookUpAccount] = getNameAccountKeyWithBump(
                reverseLookupHashedName,
                tldHouse,
                undefined,
            );
            return reverseLookUpAccount;
        });

        const reverseLookUpAccounts: PublicKey[] = await Promise.all(promises);
        const reverseLookupAccountInfos =
            await connection.getMultipleAccountsInfo(reverseLookUpAccounts);

        const batchDomains = reverseLookupAccountInfos.map(
            (reverseLookupAccountInfo) => {
                const domain = reverseLookupAccountInfo?.data
                    .subarray(200, reverseLookupAccountInfo?.data.length)
                    .toString();
                return domain;
            },
        );

        reverseLookupDomains = reverseLookupDomains.concat(batchDomains);
    }

    return reverseLookupDomains;
}

// onlyDomains will grab only domains and no nfts
async function getAllRegisteredDomains(
    tldExpected?: string,
    onlyDomains: boolean = false,
) {
    // get all TLDs
    const allTlds = await getAllTld(connection);

    const domains = [];
    for (const tld of allTlds) {
        if (tldExpected) {
            if (tld.tld != tldExpected) continue;
        } 
        // get the parent name record for a TLD
        const parentNameRecord = await NameRecordHeader.fromAccountAddress(
            connection,
            tld.parentAccount,
        );
        if (!parentNameRecord) continue;
        if (!parentNameRecord.owner) continue;

        // get all name accounts in a specific TLD
        const allNameAccountsForTld = await findAllDomainsForTld(
            connection,
            tld.parentAccount,
        );
        await setTimeout(50);
        
        const [nameHouseAccount] = findNameHouse(parentNameRecord.owner);

        const nameAccountPubkeys = allNameAccountsForTld.map((a) => a.pubkey);

        const domainsReverse = await performReverseLookupBatched(
            connection,
            nameAccountPubkeys,
            parentNameRecord.owner,
        );

        let index = 0;
        for (const domain of domainsReverse) {
            const [nftRecord] = findNftRecord(
                allNameAccountsForTld[index].pubkey,
                nameHouseAccount,
            );
            let finalOwner =
                allNameAccountsForTld[index].nameRecordHeader.owner?.toString();
            if (finalOwner == nftRecord.toString() && !onlyDomains) {
                const nftRecordData = await NftRecord.fromAccountAddress(
                    connection,
                    nftRecord,
                );
                const largestAccounts =
                    await connection.getTokenLargestAccounts(
                        nftRecordData.nftMintAccount,
                    );
                if (largestAccounts.value.length > 0) {
                    const largestAccountInfo =
                        await connection.getParsedAccountInfo(
                            largestAccounts.value[0].address,
                        );
                    if (largestAccountInfo?.value?.data) {
                        finalOwner = new PublicKey(
                            // @ts-ignore
                            largestAccountInfo.value.data.parsed.info.owner,
                        ).toString();
                    }
                }
                await setTimeout(50);
            }
            domains.push({
                nameAccount: allNameAccountsForTld[index].pubkey,
                domain: `${domain}${tld.tld}`,
                owner: finalOwner,
                expiresAt:
                    allNameAccountsForTld[index].nameRecordHeader.expiresAt,
                createdAt:
                    allNameAccountsForTld[index].nameRecordHeader.createdAt,
            });
            index += 1;
        }
    }
    return domains;
}

async function main() {
    // or ".eyekon" or ".superteam" or ".monke"
    // if tldExpected is undefined it will grab all domains
    const tldExpected: string | undefined = ".zk";
    // if set true it will grab only domains and no nfts
    const onlyDomains = false;
    const domains = await getAllRegisteredDomains(tldExpected, onlyDomains);
    console.log(JSON.stringify(domains));
    // console.log(domains?.length)
}

// get all domains registered on AllDomains
main();
```

### **9. Batch get Main Domains**

```javascript
import {
  MainDomain,
  findMainDomain,
} from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";
import pLimit from "p-limit";

// const connection = new Connection("");

export const getMultipleMainDomains = async (
  connection: Connection,
  pubkeys: PublicKey[],
): Promise<{ pubkey: string; mainDomain: string | undefined }[]> => {
  const mainDomainKeys = pubkeys.map((pubkey) => findMainDomain(pubkey)[0]);
  const mainDomainAccounts =
    await connection.getMultipleAccountsInfo(mainDomainKeys);

  return pubkeys.map((pubkey, index) => {
    const mainDomainAccount = mainDomainAccounts[index];
    if (!!mainDomainAccount?.data) {
      const mainDomainData = MainDomain.fromAccountInfo(mainDomainAccount)[0];
      return {
        pubkey: pubkey.toString(),
        mainDomain: mainDomainData.domain + mainDomainData.tld,
      };
    }
    return { pubkey: pubkey.toString(), mainDomain: undefined };
  });
};

// retrives users main domain in batches of 100 with pLimit of 20 it calls the rpc 20 times per second
// thus, 2000 accounts data can be fetched per second.
// pLimit could be increased depending on your rpc limits.
async function getMainDomainsSample(connection: Connection, data: any[]) {
  const mainDomains: { pubkey: string; mainDomain: string | undefined }[] = [];
  const batches = [];
  for (let i = 0; i < data.length; i += 100) {
    batches.push(data.slice(i, i + 100));
  }
  const limit = pLimit(20);

  await Promise.all(
    batches.map(async (batch) => {
      await limit(async () => {
        const publicKeys = batch.map((data) =>
            data.userPubkey ? new PublicKey(data.userPubkey) : PublicKey.default,
        );
        mainDomains.push(
          ...(await getMultipleMainDomains(connection, publicKeys)),
        );
      });
    }),
  );
  return mainDomains
}
```
