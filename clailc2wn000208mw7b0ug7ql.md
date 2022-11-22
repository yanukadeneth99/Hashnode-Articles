# Build a House Deed Smart Contract using Clarity

# 😎 Greetings Folks!

I welcome everyone on this journey to build using Clarity!

Some of you might ask **What do I need to know before starting out and who is this for?**

> This guide is for people who want to learn by building or programmers who have already started their path in Dev.

I will be sharing my mindset and the journey that I took to build this application from scratch in the hope that you find something valuable here.

With that out of the way, let's talk about what we will be building today. 

# ❓ But.. what is Clarity?

You have started your journey, and you have learned about Ethereum and probably Solidity, but **have you ever wanted to build on Bitcoin?** That is impossible because Bitcoin doesn't have a virtual machine to run code, but is it?

## 💥 Introducing Stacks

[Stacks](https://www.stacks.co/) is a network that interconnects you, the developer, and Bitcoin, developed by the [Hiro](https://www.hiro.so/) team. It uses a consensus technique called [Proof of Transfer (PoX)](https://docs.stacks.co/docs/understand-stacks/proof-of-transfer) which allows network participants to secure the Stacks network which recieving rewards from Bitcoin.

[![PoX](https://i.imgur.com/kZI1lNj.png)](https://docs.stacks.co/docs/understand-stacks/proof-of-transfer)

Technically, stacks can anchor itself with any Proof of Work network, but it [choose Bitcoin](https://github.com/stacksgov/sips/blob/main/sips/sip-001/sip-001-burn-election.md).

## 🤔 So we can build on Bitcoin, why Clarity?

Clarity is designed in such a way that it [optimizes security and predictability](https://docs.stacks.co/docs/write-smart-contracts/). If you have used Solidity (a smart contracts programming language), you will know how difficult it is to create contracts adhering to certain security standards while including functionalities like DeFi, NFT or otherwise.

In Clarity, it's pretty much there, you just have to understand the way to develop and [standards](https://docs.stacks.co/docs/governance/sips) like [SIP 009](https://github.com/stacksgov/sips/blob/main/sips/sip-009/sip-009-nft-standard.md) (ERC 721) can easily be used. [Here](https://book.clarity-lang.org/ch10-02-creating-a-sip009-nft.html) is a tutorial to create an NFT Contract.

[Learn more about Clarity](https://book.clarity-lang.org/ch00-00-introduction.html)

# 😰 What's the plan?

We have all been there. Planning out the project can be a tedious task. It's crucial to not take **too much **time in thinking about the project. For this guide, we will be making **a House Deed Smart Contract**. Let's try to outline and draw out what we need to achieve to better visualize our end goal. 

![Visual Plan](https://i.imgur.com/xpqy6oS.png)

Apart from my _crappy_ drawing, this visualizes the flow of what we need to build in the smart contract. 

- A user owns a house
- An owner can sell a house (list for sale)
- Potential Buyers can buy the house
- If bought a listed house, the ownership of the house is transferred to the buyer

> It's important to note that you will never write down everything you will do in the project at this stage. What's more important is to find something to start than design every part of the project.

# 🥱 That's all great, how do I start?

Theory, plans, and ideas can be boring. Let's start with the building!

## 🧑‍💻 Setting up VSCode

For this guide, we will be installing an extension-based text editor called **Visual Studio Code**.

[Click here to download Visual Studio Code](https://code.visualstudio.com/download)

![Visual Studio Screenshot](https://i.imgur.com/Xr4e2EU.png)

> Installing VSCode is pretty self-explanatory, however, if you find any trouble installing and need help, you can reach out or join any of the development communities mentioned at the end of this guide.

You can use Visual Studio Code to code pretty much anything in multiple different languages. While you're at it, do install [this extension](https://marketplace.visualstudio.com/items?itemName=HiroSystems.clarity-lsp) to help with our Clarity development. You can [read this article](https://code.visualstudio.com/docs/editor/extension-marketplace#_install-an-extension) if you need help with the installation.

## ⚙️ Installing Clarinet

To write and compile smart contracts in Clarity, you need certain tools that will make your computer understand the code you've written.

Clarinet is like a bundled package that you can use to compile, test, and manage your Clarity smart contracts. [Follow this guide](https://book.clarity-lang.org/ch01-01-installing-tools.html) to install Clarinet. 

Your output should look like this after installation (at the time of writing) : 
![TerminalOutput](https://i.imgur.com/6Wcm23p.png)

# 📁 Organising Files

While it can be tempting to jump right in. It's important to organize your files and create a directory where you will have all your code. Open up the terminal and let's do the following :

- Initialising our Clarity Project in a folder called 'deeds'
```bash
clarinet new deeds && cd deeds
```
A new project will get initialised
![Clarinet-NewProject](https://i.imgur.com/mu9bUTp.png)

- Create our first contract called 'deeds'
```bash
clarinet contract new deeds
```
- Open the project in VSCode
```bash
code .
```

Our Project should look like this now
![VSCode Project](https://i.imgur.com/gsmW5uT.png)

- Initialise [Git](https://git-scm.com/) and Do our first Commit
```bash
git init
git add .
git commit -m "Initial Commit"
```

> Note that we have created both our contract name and directory name as deeds.

# 🖌 Writing our Contract

Open up `deeds.clar` in the `contracts` folder and it should look like this :

```clar
;; deeds
;; <add a description here>

;; constants
;;

;; data maps and vars
;;

;; private functions
;;

;; public functions
;;

```

Let's start by **clearing everything off** and starting with the title and description of the contract. I practice writing clean and commented code so that the code that I write can be easily understood by myself and any other developer that will glace through my work. 

>It's important to note that **writing comments is very important in open-source projects** as it is expected for many people to read your code. 

So let's start with the following :

```clar
;;* --- Deed Contract ---
;; This contract contains information on house deeds which can be created and listed
;; by anyone. The listed deeds can be bought and sold by other parties. Every
;; interaction is handled from this contract itself and cannot involve another
;; party (human).
```

Next step is usually writing the errors that you might probably get. Let's try to think of the possible errors that we might get in this smart contract. 

- A User might try to access or buy a deed that's not listed
- A User might try to access a deed that does not exist
- A User might try to force create a deed that already exists
- A User might try to edit a deed that not belong to him/her
- A User might try to sell a deed with price 0

```clar
;;? --- Errors ---
(define-constant err-deed-not-listed (err u100)) ;; Deed is not listed for sale
(define-constant err-deed-listed (err u101)) ;; Deed is listed for sale
(define-constant err-deed-does-not-exist (err u102)) ;; Deed does not exist
(define-constant err-deed-exists (err u103)) ;; Deed already exists
(define-constant err-invalid-variable (err u104)) ;; Invalid Value in a Tuple
(define-constant err-not-deed-owner (err u105)) ;; Not the Deed Owner
(define-constant err-price-expected-more (err u106)) ;; Price is zero or not enough
```

*You might notice that we don't really use the [`(define-constant err-deed-listed (err u101))`](https://book.clarity-lang.org/ch02-03-composite-types.html) error. This is one of the functions I assumed we would need but never needed. It is good to work on removing this sort of unused variables in the optimization stage, but since this is not about that, let's keep going.*

Next, let's write a variable to save my address if I will ever need to reference it anytime in the code. 

```clar
;;? --- Constants ---
;; Owner
(define-constant contract-owner tx-sender)
```

> Note : A [Constant](https://book.clarity-lang.org/ch04-01-constants.html) is a variable that can not change once set. A [principal](https://docs.stacks.co/docs/write-smart-contracts/principals) is like an account in this context. 

Let's think about the Deed. What would I want the deed to have. 

- A unique ID to refer
- The person who owns the deed
- A name you can set for the house or the person
- Link to the house images
- Number of Bed Rooms
- Number of Bath Rooms
- The Size X and Y
- The Selling Price of the House
- A Variable which holds whether that deed is listed for sale or not

```clar
;;? --- Data Maps ---
;; Deed ID => HouseInformation(owner, first name, image url, bedrooms, bathrooms, Land Width, Land Length, price of the house, whether it's listed for sale)
(define-map deeds uint { owner: principal, name: (string-ascii 15), images: (string-ascii 128), bedroom: uint, bathroom: uint, sizeX: uint, sizeY: uint, price: uint, listed: bool })
```

A [mapping](https://book.clarity-lang.org/ch04-03-maps.html) helps to keep the information we want while "mapping" that against an ID. Sounds good. Let's write a function to create a Deed. But wait, how do we know the ID to make? Looks like we need to keep a [variable](https://book.clarity-lang.org/ch04-02-variables.html) of the ID so that we can iterate it when creating new Deeds. Let's do that now

```clar
;;? --- Data Vars ---
;; Last Deed ID value
(define-data-var last-deed-id uint u0)
```
Let's write the function which creates a deed.

```clar
;;? --- Public Functions ---

;;* Creates a Deed
;; @param name Your name
;; @param images A IPFS or HTTPS Link to images
;; @param bedroom Number of bedrooms
;; @param bathroom Number of bathrooms
;; @param sizeX The Width of the land of the House
;; @param sizeY The Length of the land of the House
;; @returns bool True if all is good
(define-public (create-deed (name (string-ascii 15)) (images (string-ascii 128)) (bedroom uint) (bathroom uint) (sizeX uint) (sizeY uint))
  (let
    (
      (next-deed-id (+ (var-get last-deed-id) u1))
    )
    (asserts! (> bedroom u0) err-invalid-variable)
    (asserts! (> bathroom u0) err-invalid-variable)
    (asserts! (> sizeX u0) err-invalid-variable)
    (asserts! (> sizeY u0) err-invalid-variable)
    (asserts! (not (is-eq images "")) err-invalid-variable)
    (asserts! (not (is-eq name "")) err-invalid-variable)
    (map-set deeds next-deed-id {owner: tx-sender, name: name, images: images, bedroom: bedroom, bathroom: bathroom, sizeX: sizeX, sizeY: sizeY, price: u0, listed: false })
    (var-set last-deed-id next-deed-id)
    (ok true)
  )
)
```
In this function, we make sure no one can create a deed with 0 bedrooms, 0 bathrooms, and sizes while making sure the images and names are passed as well. 

The `(next-deed-id (+ (var-get last-deed-id) u1))` increases the temporarily created variable `next-deed-id` variable by `1`. 

> It's not compulsory, but Clarity uses [Polish Notation](https://book.clarity-lang.org/ch01-02-clarity-basics.html) as supposed to [Infix Notation](https://en.wikipedia.org/wiki/Infix_notation). Ex: `2 + 4` is written as `+ 2 4`

Once the [`asserts!`](https://book.clarity-lang.org/ch06-01-asserts.html) confirms that the data passed in valid, the `(var-set last-deed-id next-deed-id)` will set the `next-deed-id` as the `last-deed-id`.

> Note : I use [Natspec type comments](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html#tags) (ex : `@param`, `@dev`) because I come from a solidity background. It doesn't matter if you use them or not as long as you explain your codebase in an understandable manner.

Let's write the functions to change each of the parameters (ex: Price, Name, Image URL, etc). However, I see functionality that we need commonly in all of those functions, which is : **We need to make sure the user that is trying to edit is the owner of the deed**.
 
So let's make a [_private_ function](https://book.clarity-lang.org/ch05-02-private-functions.html) since we only want that function within the contract as a helper. 

```clar
;;? --- Private Functions ---
;;* Returns True if the caller is the owner of the Deed ID
;; @dev Throws if the deed does not exist
(define-private (is-valid-owner (deed-id uint)) 
  (let
    (
      (owner (unwrap! (get owner (map-get? deeds deed-id)) err-deed-does-not-exist))
    )
  (ok (is-eq (unwrap-panic (get owner (map-get? deeds deed-id))) tx-sender))
  )
)
```
This statement `(owner (unwrap! (get owner (map-get? deeds deed-id)) err-deed-does-not-exist))` using the [`unwrap!`](https://book.clarity-lang.org/ch06-03-unwrap-flavours.html) tries to get the owner out from the `deeds` mapping at the ID of `deed-id` which is entered by user. This statement either returns the owner if the deeds exists of throw a nice error if the user doesn't exist. 

Since by now, we know the owner exists and therefore the deed exists, the statement `(ok (is-eq (unwrap-panic (get owner (map-get? deeds deed-id))) tx-sender))` using [`unwrap-panic`](https://book.clarity-lang.org/ch06-03-unwrap-flavours.html) will surely get the `owner` out and compare if the current called [`tx-sender`](https://docs.stacks.co/docs/write-smart-contracts/principals#principals-and-tx-sender) is equal to the `owner` of the deed. It returns `true` if it is the caller, and returns `false` if it's not.

Let's write the functions to edit each of the values in our `deeds` mapping. Each of the editing functions will be extremely similar. All we need to do is to make sure we control the variable that is input from the user (ex: Not take in `0` or `null` values), and we're good to go!

```clar
;;* Change the price of an owned deed
;; @param deed-id The Deed ID
;; @param new-price The New price you want to replace the deed with
;; @returns bool True if all is good
(define-public (change-price (deed-id uint) (new-price uint))
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (asserts! (> new-price u0) err-price-expected-more)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {price: new-price}))
    (ok true)
  )
)

;;* Change the image URL of an owned deed
;; @param deed-id The Deed ID
;; @param new-images The New image URL you want to replace the deed with
;; @returns bool True if all is good
(define-public (change-images (deed-id uint) (new-images (string-ascii 128)))
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (asserts! (not (is-eq new-images "")) err-invalid-variable)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {images: new-images}))
    (ok true)
  )
)

;;* Change the user name of an owned deed
;; @param deed-id The Deed ID
;; @param new-name The New name you want to replace the deed with
;; @returns bool True if all is good
(define-public (change-name (deed-id uint) (new-name (string-ascii 15)))
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (asserts! (not (is-eq new-name "")) err-invalid-variable)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {name: new-name}))
    (ok true)
  )
)

;;* Change the bedroom count of an owned deed
;; @param deed-id The Deed ID
;; @param new-bedroom The Bedroom count you want to replace the deed with
;; @returns bool True if all is good
(define-public (change-bedroom (deed-id uint) (new-bedroom uint)) 
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (asserts! (> new-bedroom u0) err-invalid-variable)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {bedroom: new-bedroom}))
    (ok true)
  )
)

;;* Change the bathroom count of an owned deed
;; @param deed-id The Deed ID
;; @param new-bathroom The Bedroom count you want to replace the deed with
;; @returns bool True if all is good
(define-public (change-bathroom (deed-id uint) (new-bathroom uint)) 
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (asserts! (> new-bathroom u0) err-invalid-variable)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {bathroom: new-bathroom}))
    (ok true)
  )
)

;;* Change the size of an owned deed
;; @param deed-id The Deed ID
;; @param new-sizeX The New Land Width of the land of the house
;; @param new-sizeY The New Land Length of the land of the house
;; @returns bool True if all is good
(define-public (change-size (deed-id uint) (new-sizeX uint) (new-sizeY uint)) 
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (asserts! (> new-sizeX u0) err-invalid-variable)
    (asserts! (> new-sizeY u0) err-invalid-variable)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {sizeX: new-sizeX, sizeY: new-sizeY}))
    (ok true)
  )
)
```

**Don't feel overwhelmed. This is just the same function and copied over and edited**

The [`asserts!`](https://book.clarity-lang.org/ch06-01-asserts.html) will check whether the caller is the owner of the deed while confirming the new values are not `0` or `null`. Then, the [`map-set`](https://book.clarity-lang.org/ch04-03-maps.html) using the [`merge`](https://book.clarity-lang.org/ch04-03-maps.html) value will overwrite only the values we need. 

*We can write this alternatively by getting all the values of the deed and storing them locally. Then making changes to the ones we need and setting that whole block into the mapping again, but merging is easier.* 

Let's write the functions to list and unlist a deed for sale. Remember, we only want the owner to be able to do that. 

```clar
;;* Listing owned Deed for sale
;; @dev No need to check as it's all done within the programme
;; @param deed-id The ID of the Deed
;; @param price The price you want to sell the house for
;; @returns bool True if all is good
(define-public (list-for-sale (deed-id uint) (price uint))
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (asserts! (not (unwrap-panic (get listed (map-get? deeds deed-id)))) err-deed-listed)
    (asserts! (> price u0) err-price-expected-more)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {listed: true, price: price}))
    (ok true)
  )
)

;;* Unlist a deed from sale
;; @param deed-id The Deed ID
;; @returns bool True if all is good
(define-public (unlist-for-sale (deed-id uint))
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (asserts! (unwrap-panic (get listed (map-get? deeds deed-id))) err-deed-listed)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {listed: false}))
    (ok true)
  )
)
```
The [`asserts!`](https://book.clarity-lang.org/ch06-01-asserts.html) will check whether the caller is the owner. Like before, the [`merge`](https://book.clarity-lang.org/ch06-01-asserts.html) function will be called to change the values.

Next is writing the Buy function. It's important to do the transaction on-chain so that you do not have a middleman interfering with all the transactions.

```clar
;;* Buy House
;; @param deed-id The Deed ID
;; @dev PAYABLE - Need to pay to buy
;; @returns bool True if all is good
(define-public (buy-deed (deed-id uint))
  (let
    (
      (owner (unwrap! (get owner (map-get? deeds deed-id)) err-deed-does-not-exist))
      (listed (unwrap-panic (get listed (map-get? deeds deed-id))))
      (price (unwrap-panic (get price (map-get? deeds deed-id))))
    )
    (asserts! listed err-deed-not-listed)
    (asserts! (is-eq (> price u0)) err-price-expected-more)
    (try! (stx-transfer? price tx-sender owner)) ;; Transaction from the buyer to the owner
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {owner: tx-sender, listed: false}))
    (ok true)
  )
)
```

First, we make sure the deed exists. The [`unwrap!`](https://book.clarity-lang.org/ch06-03-unwrap-flavours.html) will throw us a nice error `err-deed-does-not-exist` if the deed does not exist. Then we will get the `listed` and `price` variables out from the mapping since we will be needing them.

Then using the [`asserts!`](https://book.clarity-lang.org/ch06-01-asserts.html) we will make sure the deed is listed and make sure the house is not listed for sale with a price of `0`. The [`(try! (stx-transfer? price tx-sender owner))`](https://book.clarity-lang.org/ch06-02-try.html) tries to do the transaction, and if so, the new owner is set to the buyer `tx-sender` and the listed is set to false incase someone else tries to buy it immediately. 

_In this contract, there is no way to get the price to 0 by functions so far since all the functions check whether the `price` is over `0`, so one might argue whether you need the check `(asserts! (is-eq (> price u0)) err-price-expected-more)` in place. In truth, you might not really want that since that has an effect. However, I make it a **feature** to not be able to sell the house for free because if there is a bug that someone exploits to set the price to `0`, this function will always check every time before buying and disables the buyer from buying free._

One final thing I want to add is a function in case the owner wants to transfer his deed to someone else for free. 

```clar
;;* Transfers an existing owned Deed
;; @param recipient Transfer person ID
;; @param deed-id The Deed ID
;; @returns bool True if all is good
(define-public (transfer-deed (recipient principal) (deed-id uint))
  (begin
    (asserts! (try! (is-valid-owner deed-id)) err-not-deed-owner)
    (map-set deeds deed-id (merge (unwrap-panic (map-get? deeds deed-id)) {owner: recipient}))
    (ok true)
  )
)
```

Before anything, let's make sure our code has no errors and does run as expected by running the following command.

```bash
clarinet check
```

If you get this, you are good to go :
![CodeChecked](https://i.imgur.com/twQa4e4.png)

*If you have any issues, you can refer to the [Github to this project](https://github.com/yanukadeneth99/Deeds/blob/master/contracts/deed.clar) to compare what you have been missing. Ignore the addition functions in the contract as that will be discussed later down the guide.*

## 😁 All Done

Whew! That was alot of code. It's time to take a good break and come back with a fresh mind to do some testing!

# 🛠 Testing

[Writing tests](https://book.clarity-lang.org/ch07-04-testing-your-contract.html) help you find bugs in your code and confirm that your code runs as expected. 

You should have already gotten a boilerplate code for tests in the `deeds_test.ts` file under the `tests` directory. Open that up. 

If you are not using Deno like me, You should have immediately freaked out about how to install the test suites in the application. Luckily, **you don't have to**, just ignore the errors that you get in VSCode and carry on the coding part. 

I like to keep the boilerplate for the tests, so let's write some quick tests over it.

## ✍️ Writing Tests

Open up the `deeds_test.ts` file inside the `tests` directory if you haven't already. 

The first thing I would is to create a variable to hold up contract name, and write a comment of all the error codes that I made simply because I hate to always switch between the contract and test to find out. 

```js
//* Stats to Run the test
const contractName = "deed";
/*
- Error List -
err u100 - Deed is not listed for sale
err u101 - Deed is listed for sale
err u102 - Deed does not exist
err u103 - Deed already exists
err u104 - Invalid Value in a Tuple
err u105 - Not the Deed Owner
err u106 - Price is zero or not enough
*/
```

Let's try to come up with a test to access all the functions without creating a deed. Sounds nasty? Let's do it!

But wait! How do we know whether it got created or not? There is no function to get the deed information or the deed id. Time to go back and fix that. 

Open up the `deeds.clar` again and let's add these functions

```clar
;;* Gets the passed deed information
;; @param deed-id The uint Deed ID you want to check
;; @returns tuple {principal owner, string-ascii 15 name}
;; OR (Depending on whether deed is listed for sale)
;; @returns tuple {principal owner, string-ascii 15 name, uint bedroom, uint bathroom, uint sizeX, uint sizeY, string-ascii 120 images, uint price}
(define-read-only (get-deed (deed-id uint))
  (let
    (
      (owner (unwrap! (get owner (map-get? deeds deed-id)) err-deed-does-not-exist))
      (name (unwrap-panic (get name (map-get? deeds deed-id))))
      (listed (unwrap-panic (get listed (map-get? deeds deed-id))))
      (bedroom (unwrap-panic (get bedroom (map-get? deeds deed-id))))
      (bathroom (unwrap-panic (get bathroom (map-get? deeds deed-id))))
      (sizeX (unwrap-panic (get sizeX (map-get? deeds deed-id))))
      (sizeY (unwrap-panic (get sizeY (map-get? deeds deed-id))))
      (images (unwrap-panic (get images (map-get? deeds deed-id))))
      (price (unwrap-panic (get price (map-get? deeds deed-id))))
    )
    (if (not listed)
      ;; Not listed - Return the owner and name
      (ok {owner: owner, name: name})

      ;; Listed - Returns all the information to view
      (ok {owner: owner, name: name, bedroom: bedroom, bathroom: bathroom, sizeX: sizeX, sizeY: sizeY, images: images, price: price})
    )
  )
)

;;* Gets the owner of a deed
;; @param deed-id The Deed ID
;; @returns principal The Owner address of the Deed
(define-read-only (get-owner (deed-id uint))
  (ok (unwrap! (get owner (map-get? deeds deed-id)) err-deed-does-not-exist))
)

;;* Gets the last Deed ID created
;; @returns uint The Last DEED ID
(define-read-only (get-last-deed-id)
  (ok (var-get last-deed-id))
)
```

We have written [`define-read-only`](https://book.clarity-lang.org/ch05-03-read-only-functions.html) functions which do not cost any gas at all and can be referred to freely. 

> Always remember that you will always need to go back and fix certain parts of the code to better fit what you are trying to do. That's the reason why you shouldn't spend too long perfecting one file only to feel the burnout when you realize you need to change things. Be open!

Next, we'll go back to the `deeds_test.ts` and write the tests

```js
//* Accessing all the functions using the Deed ID #1
Clarinet.test({
  name: "Cannot access any function without creating a deed",
  async fn(chain: Chain, accounts: Map<string, Account>) {
    let deployer = accounts.get("deployer")!;
    let wallet1 = accounts.get("wallet_1")!;

    let block = chain.mineBlock([
      // Transfer Deed
      Tx.contractCall(
        contractName,
        "transfer-deed",
        [types.principal(wallet1.address), types.uint(1)],
        deployer.address
      ),
      // Listing for sale
      Tx.contractCall(
        contractName,
        "list-for-sale",
        [types.uint(1), types.uint(100)],
        deployer.address
      ),
      // Unlisting from sale
      Tx.contractCall(
        contractName,
        "unlist-for-sale",
        [types.uint(1)],
        deployer.address
      ),
      // Buy Deed
      Tx.contractCall(
        contractName,
        "buy-deed",
        [types.uint(1)],
        deployer.address
      ),
      // Change Price
      Tx.contractCall(
        contractName,
        "change-price",
        [types.uint(1), types.uint(500)],
        deployer.address
      ),
      // Change Images
      Tx.contractCall(
        contractName,
        "change-images",
        [types.uint(1), types.ascii("Test")],
        deployer.address
      ),
      // Change Name
      Tx.contractCall(
        contractName,
        "change-name",
        [types.uint(1), types.ascii("Test")],
        deployer.address
      ),
      // Change Bedroom
      Tx.contractCall(
        contractName,
        "change-bedroom",
        [types.uint(1), types.uint(4)],
        deployer.address
      ),
      // Change Bathroom
      Tx.contractCall(
        contractName,
        "change-bathroom",
        [types.uint(1), types.uint(4)],
        deployer.address
      ),
      // Change Size
      Tx.contractCall(
        contractName,
        "change-size",
        [types.uint(1), types.uint(200), types.uint(200)],
        deployer.address
      ),
      // Get Deed
      Tx.contractCall(
        contractName,
        "get-deed",
        [types.uint(1)],
        deployer.address
      ),
      // Get Last Deed ID
      Tx.contractCall(contractName, "get-last-deed-id", [], deployer.address),
    ]);

    // Transfer Should Fail
    block.receipts[0].result.expectErr().expectUint(102);
    // Listing should fail
    block.receipts[1].result.expectErr().expectUint(102);
    // Unlist should fail
    block.receipts[2].result.expectErr().expectUint(102);
    // Buy Deed should fail
    block.receipts[3].result.expectErr().expectUint(102);
    // Changing Price should fail
    block.receipts[4].result.expectErr().expectUint(102);
    // Changing Images should fail
    block.receipts[5].result.expectErr().expectUint(102);
    // Changing Name should fail
    block.receipts[6].result.expectErr().expectUint(102);
    // Changing Bedroom should fail
    block.receipts[7].result.expectErr().expectUint(102);
    // Changing Bathroom should fail
    block.receipts[8].result.expectErr().expectUint(102);
    // Changing Size
    block.receipts[9].result.expectErr().expectUint(102);
    // Getting Deed
    block.receipts[10].result.expectErr().expectUint(102);
    // Getting Last Deed ID
    block.receipts[11].result.expectOk().expectUint(0);
  },
});
```

Let's run the tests by running the following command :

```bash
clarinet test
```

If you get some green, you have done this right! Feels good doesn't it?
![Test-ok](https://i.imgur.com/0MgSJW2.png)

Since this guide is too long, I won't be writing other potential tests that are important but refer to the uploaded [deeds_test.ts](https://github.com/yanukadeneth99/Deeds/blob/master/tests/deed_test.ts) file to see the full list of tests I have done.

## 🥺 Upload to GitHub

I have two rules of thumb when doing projects :

> Always have your code on GitHub

> Always have a nice README explaining the project

That is the rule I live by. You don't want to lose the progress and applications you have built just cause of a measly bug or crash. If you are working on a private code, you can always store it privately on GitHub. 

The README helps keep your project nice and explains your project to people who are interested in the project. Check out the [README.md](https://github.com/yanukadeneth99/Deeds/blob/master/README.md) that I made for the project.

Assuming you have already created a Repository on GitHub (If you haven't check [this](https://www.youtube.com/watch?v=iv8rSLsi1xo) to get started), let's re-commit the work we have done and push the code into GitHub. 

```bash
git add .
git commit -m "Completed the project"
git push -u origin master
```

# 🥳 Congrats!

Congratulations! You have actually built something. It's important to always appreciate what you have done no matter how little or big it is. 

Pat yourself in the back and take a small break to tell all your friends, share around in communities what you have built and do a [tweet](https://twitter.com/intent/tweet?url=https%3A%2F%2Fyashura.hashnode.dev%2Fbuild-a-house-deed-smart-contract-using-clarity&via=hashnode&text=I%20have%20created%20a%20Housing%20Deed%20Smart%20Contract%20following%20the%20article%20made%20by%20@yanukadeneth99%20using%20Clarity%20@hirosystems%20@Stacks&hashtags=clarity%2Chiro%2Csmartcontract%2Cweb3%2Cblockchain)!

# 🧐 So, what's next? 

I would like to take some time to talk about what you should do next and some tips that might help you further. 

## 1️⃣ Contribution

Contribution is always the most important mindset and activity in the web3 space. Most people think contribution is about fixing typos of doing PRs in GitHub. While that helps, proper contribution should be something that comes to you and never something you go behind. 

It's important to have the right mindset though. **Whenever you face a problem or an issue, instead of blaming the developers and makers of the application, try to think about how you can fix it and contribute to the system**. It does not matter if you are not a good developer. 

> Attempting to help build applications **will always be better than** complaining about an issue

Meaning, if you don't know how to fix it, contact the team explaining and issue and always provide yourself as help. 

ex: (BAD) - Hey Guys, I found [this](#) issue on your documentation that made me going around in circles trying to figure that out. It's on line 24 of [this](#) document. Could you check that out and fix when possible? 

ex: (GOOD) - Hey Guys, I found [this](#) issue on your documentation that made me look more into your product/service. I was working on [this](#) project and I released that the documentation could improve, specially [this](#) section on line 24. I would love to help you on fixing, updating or creating an alternative version of this. 

## 2️⃣ Join Communities

Always join good developer communities that help you if you have issues, and that you can help they have any. Establishin good networks is crucial in this space!

Here are some good communities that I recommend joining

- [LearnWeb3DAO](https://discord.gg/learnweb3)
- [Stacks/Hiro/Clarity](https://discord.com/invite/pPwMzMx9k8)
- [Alchemy](https://discord.com/invite/alchemyplatform)

Feel free to let me know of more communities so that I can keep on adding to the list!

## 3️⃣ Appreciation

Thank you for taking your time in reading this guide. Show some love if this guide helped you with anything. Contributions are always welcome in the repository!

**Good luck on your journey!**

# 📚 Links/Resources
- [The Book - Learn Clarity](https://book.clarity-lang.org/)
- [Github Project](https://github.com/yanukadeneth99/Deeds)
- [Hiro Documentation](https://docs.hiro.so/)
- [Clarity Functions](https://docs.stacks.co/docs/write-smart-contracts/clarity-language/language-functions)
- [My Twitter](https://twitter.com/yanukadeneth99)
- [LearnWeb3DAO - Best Free Web3 Education](https://learnweb3.io/)