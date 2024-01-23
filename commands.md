TODOs:

1. Try out the whole command lines for minting contract with the marketplace options OK
-> Needs to be done on 5.

2. Customize so the contract has three different minting options, depending on the price, a different NFT might be generated
3. The NFTs will have metadata with their rarity, depending on these they might be acquired according to the price
4. Test

#Commands

near login
#or add json file at ~/.near-credentials/${network}/${account_id}.json

export NFT_CONTRACT_ID=YOUR_ACCOUNT_NAME

echo $NFT_CONTRACT_ID

rustup target add wasm32-unknown-unknown
#needs to be set before compiling contracts

yarn
#install dependencies

yarn build 
#to compile contract, can be nft-contract, or the most recent market-contract

#Make subaccount
near create-account marketplace.$NFT_CONTRACT_ID --masterAccount $NFT_CONTRACT_ID --initialBalance 25

#new env var
export APPROVAL_NFT_CONTRACT_ID=approval.$NFT_CONTRACT_ID

#deploy the contract
near deploy --wasmFile out/main.wasm --accountId $APPROVAL_NFT_CONTRACT_ID

#Initialize
near call $APPROVAL_NFT_CONTRACT_ID new_default_meta '{"owner_id": "'$APPROVAL_NFT_CONTRACT_ID'"}' --accountId $APPROVAL_NFT_CONTRACT_ID

#Mint a token
near call $APPROVAL_NFT_CONTRACT_ID nft_mint '{"token_id": "approval-token-2", "metadata": {"title": "Approval Token", "description": "testing out the new approval extension of the standard", "media": "https://bafybeiftczwrtyr3k7a2k4vutd3amkwsmaqyhrdzlhvpt33dyjivufqusq.ipfs.dweb.link/goteam-gif.gif"}, "receiver_id": "'$APPROVAL_NFT_CONTRACT_ID'"}' --accountId $APPROVAL_NFT_CONTRACT_ID --amount 0.1

#See tokens for owner
near view $APPROVAL_NFT_CONTRACT_ID nft_tokens_for_owner '{"account_id": "'$APPROVAL_NFT_CONTRACT_ID'", "limit": 10}'

#Approving an account
near call $APPROVAL_NFT_CONTRACT_ID nft_approve '{"token_id": "approval-token-2", "account_id": "'$NFT_CONTRACT_ID'"}' --accountId $APPROVAL_NFT_CONTRACT_ID --deposit 0.1

#Check the approving account
near view $APPROVAL_NFT_CONTRACT_ID nft_tokens_for_owner '{"account_id": "'$APPROVAL_NFT_CONTRACT_ID'", "limit": 10}'

#Transfer NFT as an approved account
near call $APPROVAL_NFT_CONTRACT_ID nft_transfer '{"receiver_id": "'$NFT_CONTRACT_ID'", "token_id": "approval-token-2", "approval_id": 0}' --accountId $NFT_CONTRACT_ID --depositYocto 1

#Approve a new account once the NFT is in a different account
near call $APPROVAL_NFT_CONTRACT_ID nft_approve '{"token_id": "approval-token-2", "account_id": "'$APPROVAL_NFT_CONTRACT_ID'"}' --accountId $NFT_CONTRACT_ID --deposit 0.1