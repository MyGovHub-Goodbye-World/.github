# YumeVote

![image](https://github.com/user-attachments/assets/1d97315c-0210-4fcc-81c6-b9704b67123f)

## Problem Statement
The trust between Government and citizens is crucial. With many elections happening, citizens demand a transparent and trustable election process. With nowadays way of conducting the election, some people might find the result unreliable. This results in poor citizen satisfaction and a higher rate of “absent vote”. Therefore, with the collaboration between IoT and Blockchain technology, this particular problem will be overcome well.
## Requirements
- Individual Verifiability - A voter can check their vote has been properly counted by themselves without revealing to the public who they are, but the government will always know who they are if they retrace with the public key
- Universal Verifiability - Everyone (every citizens) can check each and every votes are valid and tallied correctly. The verifier wouldn’t necessarily know who the citizen is, but they will know it is valid by checking the sign message with the published public keys
- Eligibility Verifiability - Votes must be made a valid citizen
### Votes Requirements
- Cast as intended
- Recorded as Cast
- Tallied as Recorded
## Context
1. Each citizen will have a private and public key pair. The public key pair will be made public for everybody to see.
2. Each citizen will have a National ID Card which includes their unique hash (generated through their personal metadata) and their private key, which can be corresponds to their own identity in government’s database
3. When a citizen casts their vote, they sign their vote with their private key and generate digital signature. A Vote ID as a receipt is also generated. (Vote ID, Vote (who they vote for), Digital Signature). This data can be stored on a web2 database. A hash of Vote ID, Vote and Digital Signature is generated and stored on the blockchain for auditing purposes
4. Citizen should have the ability to verify their own vote by using the VoteID receipt that has been generated for them. There can be a webpage, where the user can input the VoteID, and the system will display VoteID, Vote, Digital Signature, Hash stored on the blockchain and reverify themself manually or automatically aided by the system
5. There exists a public history page which displays a list of all votes that has been casted. Everybody can use the published public key database to check if an individual vote has been signed by the private key of the exposed public key without revealing the private key itself.
Limitation
Since the government currently has access on how each private and public key pair is related to a particular citizen, they can always know who cast the vote by doing No.5 mentioned above, and then inferring the value by checking their own private database.

## Functional Requirements
1. An emulated government database with citizen data (does not have to be real), a hash of aforementioned metadata, private key and public key
2. An emulated government blockchain with Maschain with Audit Service enabled that have one Auditing Smart Contract stores the hash of the public key exposed
3. An emulated government api service that exposes all the public key data, a hash of those public keys (to ensure the government does not went wrong internally) is kept on the blockchain and txID is made public for everybody to see and recheck themselves as necessary
4. A voting DRE IoT machine made with Arduino that reads the private key and the hash from a RFID National Card (emulated), and interacts with the voting service api. The DRE Machine should be able to read the RFID Card, show a list of candidates they can vote, sign the digital signature, and send the data to the voting service api. If the api mentioned the vote is invalid because the identity and digital signature can be verified, then this citizen is using a fake id. If the vote is valid, a Vote ID will be returned by the Voting Service API.
5. Vote Database that stores Vote ID (which will be returned to the user as mentioned in No.3), Vote (which candidate was it (Joe Biden or Donald Trump)), Digital Signature.
6. Vote Blockchain with Maschain with Audit Service enabled that will store the ID and the Hash of a particular vote entry from Vote Database
7. Voting Service API that allows the 3. IoT Machine to vote. When a vote is received, the verification process starts by using a similar way as No.5 in the Context section. (Comparing every public key with the digital signature). If the vote is valid as signed and proved by the digital signature and exposed public keys, the entry is added to the 4. Voting Database. The hash of this particular entry is also added onto Vote Blockchain.
8. Result Webpage that allows the users to see in real-time how the latest results
9. History Webpage that allows the anybody to do recheck and reverify the final results themselves by displaying Vote ID, Vote, Digital Signature, and Hash
10. Vote Individual Verifier Webpage that allows individual to review who they have voted for by allowing them to input the Vote ID

## Systems and Repositories

1. [Emulated Government Service API](https://github.com/YumeVote/emulated-government-service-api)
   An API Service that emulates what the government would need to reveal to the public for the system to work
2. [Voting Service API](https://github.com/YumeVote/voting-service-api)
   An API Service that verifies the identity of the citizen, casts votes in the system and make the vote verifiable on Maschain
3. [Voting IOT Machine UI](https://github.com/YumeVote/voting-service-api)
   A emulated GUI application which acts as a POS IOT Machine (DRE) to allow people to vote for election on the Maschain
4. [Public Facing Website](https://github.com/yumevote/yumevote)
   A react-based website which the voters or any third parties could use to verify the authenticity of the votes and trace their own vote by looking at audit history
