TODOs:

1. Try out the whole command lines for minting contract with the marketplace options
2. Customize so the contract has three different minting options, depending on the price, a different NFT might be generated
3. The NFTs will have metadata with their rarity, depending on these they might be acquired according to the price
4. Test

#Commands

near login
#or add json file at ~/.near-credentials/${network}/${account_id}.json

export NEARID=YOUR_ACCOUNT_NAME

echo $NEARID

rustup target add wasm32-unknown-unknown
#needs to be set before compiling contracts

yarn
#install dependencies

yarn build 
#to compile contract, can be nft-contract, or the most recent market-contract