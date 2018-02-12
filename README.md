# EthereumOrderForFood - LunchParty

### Context
A food ordering system made to be used internally by a group of software developers, this tool is designed to make getting lunch at Amazon YYZ14 office easier. YYZ14 office is located near Union station in downtown Toronto, we do not have a cafeteria, and the food court options around the office are limited. Toronto being the large city it is, offers wide range of food choices for delivery.

### Problem
Getting satisfactory food for lunch is difficult. In general, we have the following pain points:
1. For us young engineers, cooking is not an option, but for some, they bring their own food, and we enjoy taking food back and eating together
2. Food options from nearby food courts are limited
3. We've been talking about ordering take-out for a while, but no one has ever taken initiative
4. Ordering take-out needs planning, people sometimes don't pay attention to Amazon Chime during work and don't participate, so planning is difficult to be done, hard to know who's participating, and who's ordering what
5. Normally one person pays the bill and the rest pay him, splitting money has traditionally been a pain

Instead of payment (although still a pain point), the biggest problem with ordering food for lunch is the planning. In order to plan a successful take out ordering, we need to know:
1. Who is in charge. There are things that a program simply cannot do, and the most critical is taking initiative. A real person must be in charge of ensuring the food is ordered correctly and arrives in time. The role should be rotated, so that all the responsibilities don't always fall onto the same person.
2. Whos wants to participate. This will be some sort of chicken-egg problem, because some people may decide to join due to a specific lunch time or the food we are ordering.
3. At around what time shall we eat. Traditionally this has been ~11:45 - 12:15, it may be delayed when ordering takeout because most restaurants open at 11:30, and delivery takes ~45min - 1hour. Woundn't be a problem if we use things like Foodera and UberEats, but then we really like Chinese food, and those services don't carry good chinese food.
4. What to order. There are two modes, everybody pick their own, or order together for sharing. It will always be the case where A wants to order from X and B wants to order from Y. X and Y might even be different food services. 

### Solving The Problem

##### The person in charge solve the problems
There are a lot of decisions to be made during the process. Instead of designing some sort of decision-making machine, oracle, (you name it), we should leave it to the person in charge. Because, as ya'all know, Trust An Engineer. Seriously tho, we are coworkers, human-to-human interactions should be solved by a human. The key to solving all the problems is a clear understanding of responsibilities. What we need is a mechanism that tend to put people who are motivated to solve the problems in charge, and help him with it.

##### Who wants to order becomes the person in charge
Let's assume by default, people don't like to trouble themselves for something they don't want, so if someone knowingly calls out to order delivery or willingly becomes the person in charge, said person can be expected to solve the problems. Eventually, the person in charge must understand who are coming, when to order food, and what food to order. To compensate for the trouble, the person in charge should receive a tiny gratitude payment from each participant. The amount will be insignificant, but serves the important purpose in acknowledging the effort made by the person in charge. Everyone likes to see a thank you.

##### You are out unless you are in
Because interactions between different individuals are always different, eg A likes B, B likes C, A hates C; or, day 1 A likes B, day 2 A hates B, we don't want to make any assumptions about who to invite on behalf of the person in charge. Therefore, by default, the lunch ordering will only happen and be knowny by the person in charge, and the people directly invited by the person in charge. More options may be added in the future, but the default should remain set in stone.

##### Ethereum as payment medium
This is a no-brainer. This is 2018, transactions using traditiional banking system is just too out-dated when it comes to lunch money. Additionally, we can really use smart contracts here.

##### Lunch accounts are a pre-funded smart contracts
Ethereum carries a transction fee, and it can be significant when it comes to transfering small amounts of value, ie lunch money. I cannot think of an immediate solution, as the act of transferring or authorization will both be transactions. However, I do think lunch money should all be prefunded into the lunch party smart contract. Having the money readily in the smart contract providers a stronger incentive to spend it on lunches, and additional monetary-control features (eg locked-in funds) can be easily added later.

A user shall have an unique identifier, in this case it is safe to be the Amazon login id. Each id can be associated with an array of deposit addresses, and each deposit address holds a certain balance. A user's total balance is calculated from the sum of all the balances that corresponds to his ID. Each of these deposit address can have full permission over the user's balance. It is critical not to expose the private key of any of these balances. In the worst case, this is just lunch money and will not cause too much harm if all funds are withdrawn using a stolen private key.

I.e, A has the option to prefund $200 dollar worth of ethereum, say 0.2eth, to minimize transactionc-cost per lunch. After the prefund, A would have a balance of 0.2eth. A can choose to withdraw at any moment. Payments happen inside the same smart contract, hence if A, B, C buy lunch, C is the person in charge and lunch cost 0.01eth for everyone, then after the lunch A and B should see -0.01eth and C should see +0.02eth in balances. Obviously this is just a number in the contract, no real ether is transferred other than during deposit or withdraw. 

##### Pay before order is placed using Smart Contract Escrow
Right before the ordering, the person in charge should know exactly who is ordering what and should pay how much. The action of the person in charge submitting the order and paying for food puts him in danger of not being paid, therefore this action should come after everyone's payment being locked in. There is no guarantee of the food actually showing up, so let's assume 6 hours is an acceptable SLA, and the funds will only be released to the person in charge after being locked down for 6 hours. We don't have timers in smart contracts and we don't want to send another transaction, so this 6 hour rule will only be applied when trying to withdraw, i.e., the balance would be updated immediately, but the added balance cannot be withdrawn from the contract within 6 hours. This 6 hours can be used to settle disputes, and provide room to implement additional features to support edge cases. Also, credit systems can be implemented based on this.


##### Authorization
This is the critical component. Say when the person in charge invites A, B, and C. Funds must be taken from each's balanaces and credited to the person in charge. This requires authorization from A, B, C. Each authorizan should eventually be a transaction to the smart contract from the address which user initially deposited funds from.
Exact authorizantion implemention should be reserved for later, as for starters, trust relationships have already been established as coworkers. 

##### Inclusion to fiat money
Let's accept the fact that some people will simply refuse to use Ether. The tool must make it easy to differentiate between people who will be paying with Ether and people who will be paying with Fiat.

##### Payment calculation
The payment should be cauclated as follows:
Total payment =
1. Base cost
2. Delivery fee / capita
3. Tip and Tax per percentage of all Tip and Tax total amount
4. Any additional miscellaneous cost
5. Small gratitude towards the person in charge

This will arrive to the total Fiat payment. Then convert to Eth amounts using a recent Ethereum/CAD ratio.

### V1 MVP
There are too many moving pieces. A MVP should be created with minimum effort to test the idea and discover additional friction points. Let's define what the MVP should look like:
1. selection of person in charge, inviting and communicating with participants will be done off-site, ie Amazon Chime or in person.
2. The tool should provide a basic realm for the food party, ie a specific order
3. Within the order, the tool should help calculate how much everyone pays, and display the calculations to everyone
4. The tool should give instrustions on how to fund and pay the person in charge.

