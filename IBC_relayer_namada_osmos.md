We demonstate how to create IBC relayer channel on Namada SE testnet and Osmosis testnet.   

namadac balance --owner namada_relayer_morecon --node $RPC
naan: 1809.487509

osmosisd keys add  osmosis_relayer_morecon
Enter keyring passphrase (attempt 1/3):
- address: osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc
  name: osmosis_relayer_morecon
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AyBhfQ8sIabvZpm6rjN1xX/6f6q1+JFTtNdYGk9gN5qd"}'
  type: local
**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.
inside jungle bullet jealous round claim globe square feed increase balance eternal garment skill fiscal apple firm holiday permit tray boy summer swallow capable

# Accounts of Relayer for shielded-expedition and osmo-test-5
Setup system environments
```
export HERMES_CONFIG=$HOME/.hermes/config.toml   
export RPC=http://75.119.137.202:26657  
export namada_relayer_morecon=tnam1qqssedgx7nyak90d9r2dvdw8wn3tx3kr6u2vv5ua
export osmosis_relayer_morecon=osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc
```

echo "inside jungle bullet jealous round claim globe square feed increase balance eternal garment skill fiscal apple firm holiday permit tray boy summer swallow capable" > ./mnemonic_morecon
hermes --config $HERMES_CONFIG keys add --chain osmo-test-5 --mnemonic-file ./mnemonic_morecon
hermes --config $HERMES_CONFIG keys add --chain shielded-expedition.88f17d1d14 --key-file $HOME/.local/share/namada/shielded-expedition.88f17d1d14/wallet.toml 

hermes --config $HERMES_CONFIG \
  create channel \
  --a-chain shielded-expedition.88f17d1d14 \
  --b-chain osmo-test-5 \
  --a-port transfer \
  --b-port transfer \
  --new-client-connection --yes
2024-02-24T03:21:44.115377Z  INFO ThreadId(01) running Hermes v1.7.4+38f41c6
2024-02-24T03:21:44.204352Z  INFO ThreadId(01) Creating new clients, new connection, and a new channel with order ORDER_UNORDERED
2024-02-24T03:22:03.813736Z  INFO ThreadId(01) foreign_client.create{client=osmo-test-5->shielded-expedition.88f17d1d14:07-tendermint-0}: 🍭 client was created successfully id=07-tendermint-1173
2024-02-24T03:22:06.283171Z  INFO ThreadId(01) foreign_client.create{client=shielded-expedition.88f17d1d14->osmo-test-5:07-tendermint-0}: 🍭 client was created successfully id=07-tendermint-2288
2024-02-24T03:22:24.250366Z  INFO ThreadId(01) 🥂 shielded-expedition.88f17d1d14 => OpenInitConnection(OpenInit { Attributes { connection_id: connection-511, client_id: 07-tendermint-1173, counterparty_connection_id: None, counterparty_client_id: 07-tendermint-2288 } }) at height 0-52822
2024-02-24T03:23:03.066328Z  INFO ThreadId(01) 🥂 osmo-test-5 => OpenTryConnection(OpenTry { Attributes { connection_id: connection-2164, client_id: 07-tendermint-2288, counterparty_connection_id: connection-511, counterparty_client_id: 07-tendermint-1173 } }) at height 5-5589485
2024-02-24T03:23:29.043699Z  INFO ThreadId(01) 🥂 shielded-expedition.88f17d1d14 => OpenAckConnection(OpenAck { Attributes { connection_id: connection-511, client_id: 07-tendermint-1173, counterparty_connection_id: connection-2164, counterparty_client_id: 07-tendermint-2288 } }) at height 0-52828
2024-02-24T03:23:42.941670Z  INFO ThreadId(01) 🥂 osmo-test-5 => OpenConfirmConnection(OpenConfirm { Attributes { connection_id: connection-2164, client_id: 07-tendermint-2288, counterparty_connection_id: connection-511, counterparty_client_id: 07-tendermint-1173 } }) at height 5-5589495
2024-02-24T03:23:45.981113Z  INFO ThreadId(01) connection handshake already finished for Connection { delay_period: 0ns, a_side: ConnectionSide { chain: BaseChainHandle { chain_id: shielded-expedition.88f17d1d14 }, client_id: 07-tendermint-1173, connection_id: connection-511 }, b_side: ConnectionSide { chain: BaseChainHandle { chain_id: osmo-test-5 }, client_id: 07-tendermint-2288, connection_id: connection-2164 } }
2024-02-24T03:24:01.359741Z  INFO ThreadId(01) 🎊  shielded-expedition.88f17d1d14 => OpenInitChannel(OpenInit { port_id: transfer, channel_id: channel-340, connection_id: None, counterparty_port_id: transfer, counterparty_channel_id: None }) at height 0-52831
2024-02-24T03:24:15.096635Z  INFO ThreadId(01) 🎊  osmo-test-5 => OpenTryChannel(OpenTry { port_id: transfer, channel_id: channel-5850, connection_id: connection-2164, counterparty_port_id: transfer, counterparty_channel_id: channel-340 }) at height 5-5589503
2024-02-24T03:24:34.323336Z  INFO ThreadId(01) 🎊  shielded-expedition.88f17d1d14 => OpenAckChannel(OpenAck { port_id: transfer, channel_id: channel-340, connection_id: connection-511, counterparty_port_id: transfer, counterparty_channel_id: channel-5850 }) at height 0-52834
2024-02-24T03:24:49.651437Z  INFO ThreadId(01) 🎊  osmo-test-5 => OpenConfirmChannel(OpenConfirm { port_id: transfer, channel_id: channel-5850, connection_id: connection-2164, counterparty_port_id: transfer, counterparty_channel_id: channel-340 }) at height 5-5589511
2024-02-24T03:24:52.698498Z  INFO ThreadId(01) channel handshake already finished for Channel { ordering: ORDER_UNORDERED, a_side: ChannelSide { chain: BaseChainHandle { chain_id: shielded-expedition.88f17d1d14 }, client_id: 07-tendermint-1173, connection_id: connection-511, port_id: transfer, channel_id: channel-340, version: None }, b_side: ChannelSide { chain: BaseChainHandle { chain_id: osmo-test-5 }, client_id: 07-tendermint-2288, connection_id: connection-2164, port_id: transfer, channel_id: channel-5850, version: None }, connection_delay: 0ns }
SUCCESS Channel {
    ordering: Unordered,
    a_side: ChannelSide {
        chain: BaseChainHandle {
            chain_id: ChainId {
                id: "shielded-expedition.88f17d1d14",
                version: 0,
            },
            runtime_sender: Sender { .. },
        },
        client_id: ClientId(
            "07-tendermint-1173",
        ),
        connection_id: ConnectionId(
            "connection-511",
        ),
        port_id: PortId(
            "transfer",
        ),
        channel_id: Some(
            ChannelId(
                "channel-340",
            ),
        ),
        version: None,
    },
    b_side: ChannelSide {
        chain: BaseChainHandle {
            chain_id: ChainId {
                id: "osmo-test-5",
                version: 5,
            },
            runtime_sender: Sender { .. },
        },
        client_id: ClientId(
            "07-tendermint-2288",
        ),
        connection_id: ConnectionId(
            "connection-2164",
        ),
        port_id: PortId(
            "transfer",
        ),
        channel_id: Some(
            ChannelId(
                "channel-5850",
            ),
        ),
        version: None,
    },
    connection_delay: 0ns,
}

osmosisd query bank balances osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc
balances:
- amount: "99988373"
  denom: uosmo

namadac balance --owner namada_relayer_morecon --node $RPC
naan: 1793.487509

osmosisd tx ibc-transfer transfer \
  transfer \
  channel-5850 \
  tnam1qqssedgx7nyak90d9r2dvdw8wn3tx3kr6u2vv5ua \
  2000000uosmo \
  --from osmosis_relayer_morecon \
  --gas auto \
  --gas-prices 0.025uosmo \
  --gas-adjustment 1.2 \
  --node http://127.0.0.1:26657 \
  --home $HOME/.osmosisd \
  --chain-id osmo-test-5 \
  --yes
Enter keyring passphrase (attempt 1/3):
gas estimate: 131114
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: 364E3B33524296D0CE6457E5A9CC1291E463801AF63B53E0E6798758CCB6A5C5

namadac --base-dir $HOME/.local/share/namada \
    ibc-transfer \
    --amount 1 \
    --source namada_relayer_morecon \
    --receiver osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc \
    --token naan \
    --channel-id channel-340 \
    --node http://75.119.137.202:26657 \
    --memo tpknam1qqm2cd40luwxhpaath4f9nf38d8f8rhy47jl25nn03gtlw4cpjkezjl85ud
Enter your decryption password: 
Transaction added to mempool.
Wrapper transaction hash: 9815F361BC2F50996AA1E555F6797729BF05EC12A9BFCC86263F323EAB6DD066
Inner transaction hash: 658B9890A1CA89EEAFE15864E1254C8F62CDF916633E31CAD81421759154511B
Wrapper transaction accepted at height 52900. Used 26 gas.
Waiting for inner transaction result...
Transaction was successfully applied at height 52901. Used 6193 gas.

namadac balance --owner namada_relayer_morecon --node $RPC
naan: 1789.987509
transfer/channel-340/uosmo: 2000000

query bank balances osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc
balances:
- amount: "1"
  denom: ibc/73703A34EF871C7320A4FA693A23679D43BD8409553D4F71A5F1787BED80A090
- amount: "97985095"
  denom: uosmo
pagination:
  next_key: null
  total: "0"

