# How to earn by predicting the winning teams of World Cup Football

# 😎 Welcome

Welcome, everyone! In this guide, we will be talking about what [WC NFT Fantasy](https://wc-nft-fantasy.xyz/) is, how to get started and some technical details on the project.

# 🤔 What is this project?

WC NFT Fantasy is a NFT prediction-based game that you can play to win. The game lets you pick 4 teams that you predict will win the FIFA World Cup 2022 in that order, and win the prize pool if you guessed it right! 

If multiple people guessed the right answer, then the winner will be selected by True on-chain Randomness ([Using Chainlink VRF](https://docs.chain.link/vrf/v2/introduction)).

You can visit [this page](https://wc-nft-fantasy.xyz/HowToPlay) if you want a quick overview to get started. 

# 🛍️ What do I need before getting started?

You need a minimum of **4 Matic + gas fees** to mint the first 4 teams and join the main prediction game. 

1 Matic costs $0.87 at the time of writing (0.87 x 4 ≈ $3.5).

# 🚶‍♂️ How do I enter the game?

- Visit the website [here](https://wc-nft-fantasy.xyz/). 

- Click on Connect Wallet at the top right of the website

![ConnectWallet1](https://i.imgur.com/8r45JKL.png)

- Click on your wallet to connect using your favourite wallet.

![ConnectWallet2](https://i.imgur.com/sD0CmdP.png)

_If you don’t have a wallet and want to get started, click [here](https://metamask.io/) to create a wallet. We recommend MetaMask._

_If you don’t understand what web3 is, you can learn more [here](https://learn.rainbow.me/understanding-web3)._

- Once connected, you can click on the **Mint** button on the Header, or scroll down until you see the section to Mint. 

![MintSection](https://i.imgur.com/sSvd6HY.png)

- Using the two buttons below to navigate, select the teams that you want in the exact order that you predict they will win. For example, if I predict that the following teams will win :

1 - Brazil, 2 - Argentina, 3 - France, 4 - England

It would look like this : 

![Countries](https://i.imgur.com/FCuwOKd.png)

_You can click on the **Delete Selection** button if you want to change the order_

- Once you are sure of your selection, scroll down and click on the **Mint** button. After the section, this should pop up :

![Confirmation](https://i.imgur.com/MUe0VlX.png)

Insert the amount of Matic that you wish to bet on. Note that the minimum amount is 4 Matic (1 Matic per NFT x 4 NFTs).

- After inserting the amount to mint, click on the button **Confirm** underneath and a confirmation from MetaMask (or your wallet) should pop up.

- Confirm the popup to proceed with the transaction. In a little while, everything should be in order and you should have the NFTs.

- Click on the **Play Game** button at the top of the website or go [here](https://wc-nft-fantasy.xyz/MainGame) to view your NFTs.

![MainGame](https://i.imgur.com/RBnVOwK.png)

On this page, you can view your NFTs, evolve your teams if eligible, swap teams if eligible, and deposit your prediction points after the game ends.

# 🤝 Swapping Teams

You swap teams when you feel another team has a chance than an existing selected team. **Swapping is very limited**, therefore, always be sure of the order that you choose to mint.

You can swap only between two teams at a given time. Assuming you minted the extra two teams that you can swap against, you can only perform the swap feature against the extra 2 teams in Round 32 and Round 16 of the game. 

You can however swap from the main chosen 4 teams after each round announcement at Round 32, Round 16, Round 8, and Round 4 respectively.

![SwapImage](https://i.imgur.com/IBoGNMg.png)

In order to perform a swap, go into the [Main Page](https://wc-nft-fantasy.xyz/MainGame).

# 🤷‍♀️ What do I do now?

Congrats! You have successfully entered the game. Now you have to watch the Football games and see how well you did your prediction.

# 🥇 What is Upgrading/Evolving?

There are four rounds that the Football game goes through before announcing its winners. After each round, if your team is in the tournament, you can upgrade or evolve your NFT.

Since you can only upgrade if the chosen team is selected for the next round, upgrading will change your NFT to make it way cooler and worth more. 

![Evolve](https://i.imgur.com/EMfFBwz.png)

Evolve Example : 
![EvolveExample](https://i.imgur.com/dMe6Ta2.png) 

_This will cost some gas for the transaction since it will Mint you a new token._

# ⚽️ Games

This will give you a quick overview of the types of games that exist in WC NFT Fantasy. The opening of the Quiz and Number game will be announced by us on Twitter, so check out the Teams section in this guide to follow us for that update!

## 📈 Prediction

This is the main game that you predict the winning teams in their order and win the main prize if guessed correctly. 

## 🧠 Quiz

This is a mini-game where you can win NFTs by answering the questions right (This may or may not be cool level 3 NFTs).

You can do the quiz [here](https://wc-nft-fantasy.xyz/QuizGame) when it's opened.

## 👀 Number Guessing

The Number Guess game is all about guessing a random number right to win a cool NFT if you do. This game will be available after the Quiz game mentioned above.

# 🛠 Technical Details

In this section, I will talk about the technical aspect of this project. This includes explanations of various technologies and techniques used, with links to frameworks/stacks used.

## ☀️ Frontend

We have used [Next JS](https://nextjs.org/) on the Front end, along with the following plugins : 

- [Rainbowkit](https://www.rainbowkit.com/) for Wallet handling and connection.
- [Axios](https://axios-http.com/) to fetch queries from the Subgraph (explained later).
- [Ethers](https://docs.ethers.io/v5/) for EVM Interactions and conversions.
- [GSAP](https://greensock.com/gsap/) for handling animations.
- [Next-SEO](https://www.npmjs.com/package/next-seo) for SEO Management within the website.
- [TailwindCSS](https://tailwindcss.com/) for CSS consistency within the project.

All the frontend code van be viewed in this [repository](https://github.com/CoolBuidlers/WC-NFT-Fantasy/tree/main/frontend).
Our frontend is more focused on UX (User Experience), therefore uses great UI (User Interface) and  built in a way focused to make the users interact with the smart contract backend easier and smoother.

## 🌒 Backend

The backend in our project is quite complicated and has a lot of contracts and techniques used to make it possible. I will try my best to explain this as smoother as possible. 

### Summary

We have created 9 different contracts (A total of more than 15) for this game to work.

The main [Prediction](https://polygonscan.com/address/0x8Ef484Bd170BCcA098D07B5956afcC97E000e5F6) uses [ERC1155](https://eips.ethereum.org/EIPS/eip-1155) to build an upgradable contract.

_Note that you cannot transfer NFTs until the tournament is completely over._

Here is a summary of each of those 9 contracts and how they work. 

- [Contract WCNFTFantasy/Prediction.sol](https://polygonscan.com/address/0x8ef484bd170bcca098d07b5956afcc97e000e5f6#code) - Allows predictors to mint their teams, deposit their winners, and this chooses the winners using [VRF](https://docs.chain.link/vrf/v2/introduction). [Keepers](https://docs.chain.link/chainlink-automation/introduction) gets called every 24 hours to check the conditions of the game.
- [Contract ChangeOrders](https://polygonscan.com/address/0xd9d944ea52265b9d307c0fba090e08e386a3e34c#code) - Allows predictors to change the order of their prediction every time the World Cup enters a new round.
- [Contract Evolve](https://polygonscan.com/address/0x510035be6e39bee67aa1ed5384346a2768dbe4b8#code) - Predictors can evolve their teams if that team has made it to the next World Cup round.
- Contract MintTeams [One](https://polygonscan.com/address/0xd9a725366610e49733aa1aa5c9a9c37ca46d4bd1#code)/[Two](https://polygonscan.com/address/0x23d03b8d3af3ef25a0af91be12963cc119d84261#code) - This is where the NFT exists gets called by [Prediction.sol](https://polygonscan.com/address/0x8ef484bd170bcca098d07b5956afcc97e000e5f6#code) for users to be able to mint their teams.
- Contract FetchTeams [One](https://polygonscan.com/address/0x34631d5a1cbecbb2f5c72d854ec6726f51c7429d#code)/[Two](https://polygonscan.com/address/0xde007979b532497921e99c9a7c73fd9dccd45b07#code)/[Three](https://polygonscan.com/address/0xf77c224b61638843a48a38d055d3b381f24b0d81#code)/[Four](https://polygonscan.com/address/0x523d874bae5fc9196d2dfb92ade6b2b1e365d6df#code) - Fetches the teams that are still in the World Cup.
- [Contract RetrieveRandomNumber](https://polygonscan.com/address/0xdb7ae51349ac32dbb2497aa33f510425a638618b#code) - Fetches a random number from [VRF](https://docs.chain.link/vrf/v2/introduction).
- Contract WorldCupData [Round 16](https://polygonscan.com/address/0xcf732660f48cd76f6b576b3959cbb7d97353e889#code)/[Round 8](https://polygonscan.com/address/0x5c824ef8e60ada057781eec04900d9b2b454558a#code)/[Round 4](https://polygonscan.com/address/0x7650dc69358075cdaefdb54df386171e43b54d0f#code) - Calls the API To retrieve the teams that are still in the World Cup.
- [Contract QuizGame](https://polygonscan.com/address/0xa9e7555f03de71af83d7a473e38ca450500efd6a#code) - Predictors can enter two Quiz games and potentially win a NFT.
- [Contract NumberGuessingGame](https://polygonscan.com/address/0xf19719b7d08276c08809a3027da279aa137683bc#code) - Predictors can guess the random number from the [VRF](https://docs.chain.link/vrf/v2/introduction) and potentially win a NFT.

_Note : You can click on any of the contract names to take you to view their code._

### Tech Used

- [Hardhat](https://hardhat.org) - To compile, test and deploy into the network.

- [Chainlink Price Feed](https://docs.chain.link/data-feeds) - Determine how much users pay for teams.
- [Chainlink VRF](https://docs.chain.link/docs/vrf/v2/introduction/) - Choose winners based on who predicted and the amount of points they scored.
- [Chainlink Keepers](https://docs.chain.link/chainlink-automation/introduction) - Autocall functions when different rounds start in the World Cup.
- [Chainlink Adapters](https://docs.chain.link/any-api/introduction) - Call the API to receive the players that are still in the World Cup.

- [IPFS](https://ipfs.tech/) - Stored and deployed the NFT Images and the NFT Metadata in a decentralized way so that they cannot get altered by anyone including us and, the data will always be publicly accessible.

- [Quicknode](https://www.quicknode.com/) - We used Quicknode to deploy all our smart contracts on the network.

- [Polygon](https://polygon.technology/) - Enables us to deploy contracts in L2 blockchain which helps handle traffic for the rush in the event of buying tickets.

All of the backend code can be viewed from our [repository](https://github.com/CoolBuidlers/WC-NFT-Fantasy/tree/main/backend).

## ⛓ Subgraphs

We have used [The Graph](https://thegraph.com) to query the data emitted from the events in the contract code and display them on the frontend.

You can view all of the files of the subgraph from our [repository](https://github.com/CoolBuidlers/WC-NFT-Fantasy/tree/main/subgraph).

# 👥 Team

This project was made by 4 people for the [Chainlink Hackathon](https://chainlinkfall2022.devpost.com).

- [Abbas Khan](https://twitter.com/KhanAbbas201)
- [Larry Cutts](https://twitter.com/LarryCutts6)
- [Yanuka Deneth](https://twitter.com/yanukadeneth99)
- [Haroon Hassan](https://twitter.com/codingHaroon)

You can check other projects we have done [here](https://github.com/CoolBuidlers).

# 📕 Resources

- [GitHub Repository](https://github.com/CoolBuidlers/WC-NFT-Fantasy)
- [Cool Buidlers](https://github.com/CoolBuidlers)

# ❤️ Thank you

Thank you for reading this article. Suggestions and Feedback and very much welcome! Don't forget to like and share around if this helps.