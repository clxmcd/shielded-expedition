# Brief description

This testcase is for the subclass of "Operating IBC/ Interoperability infrastructure". We've completed "Operate a Shielded Expedition-compatible Osmosis testnet relayer"  

We demonstate how to create IBC relayer channel on Namada SE testnet and Osmosis testnet and Transfer token via relayer channel.


**Created Channel:**   
channel-433 for shielded-expedition.88f17d1d14  
channel-5894 for osmo-test-5  

# Setup system environments
```
export HERMES_CONFIG="$HOME/.hermes/config.toml"  
export RPC_NODE="http://75.119.137.202:26657"  
export namada_relayer_morecon=tnam1qqssedgx7nyak90d9r2dvdw8wn3tx3kr6u2vv5ua
export osmosis_relayer_morecon=osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc
```

# Generate and import accounts 
Generate relayer account for osmos-test-5 and get faucet from https://faucet.testnet.osmosis.zone/
```
osmosisd keys add  osmosis_relayer_morecon
Enter keyring passphrase (attempt 1/3):
- address: osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc
  name: osmosis_relayer_morecon
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AyBhfQ8sIabvZpm6rjN1xX/6f6q1+JFTtNdYGk9gN5qd"}'
  type: local

**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

inside jungle bullet jealous round claim globe square feed increase balance eternal garment skill fiscal apple firm holiday permit tray boy summer swallow capable
```
Import relayer account for Namada
```
namadaw derive --alias namada_relayer_morecon

namadac balance --owner namada_relayer_morecon --node $RPC_NODE
naan: 1809.487509
```

# Install Hermes service
Downloading hermes-v1.7.4 and copy over to /usr/local/bin  

# Edit config.toml 
mkdir $HOME/.hermes
<details>
    <summary> vim $HOME/.hermes/config.toml </summary>
  
```
[global]
log_level = 'info'
 
[mode]

[mode.clients]
enabled = true
refresh = true
misbehaviour = true

[mode.connections]
enabled = false

[mode.channels]
enabled = false

[mode.packets]
enabled = true
clear_interval = 10
clear_on_start = false
tx_confirmation = true

[telemetry]
enabled = false
host = '127.0.0.1'
port = 3001

[[chains]]
id = 'shielded-expedition.88f17d1d14' 
type = 'Namada'
rpc_addr = 'http://75.119.137.202:26657'  
grpc_addr = 'http://75.119.137.202:9090' 
event_source = { mode = 'push', url = 'ws://75.119.137.202:26657/websocket', batch_delay = '500ms' } 
account_prefix = ''
key_name = 'namada_relayer_morecon' 
store_prefix = 'ibc'
gas_price = { price = 0.0001, denom = 'tnam1qxvg64psvhwumv3mwrrjfcz0h3t3274hwggyzcee' } 
rpc_timeout = '20s'

[[chains]]
id = 'osmo-test-5'
type = 'CosmosSdk'
rpc_addr = 'http://127.0.0.1:26657'  # set the IP and the port of the chain
grpc_addr = 'http://127.0.0.1:9090'
event_source = { mode = 'push', url = 'ws://127.0.0.1:26657/websocket', batch_delay = '500ms' } 
account_prefix = 'osmo'
key_name = 'osmosis_relayer_moreco'
address_type = { derivation = 'cosmos' }
store_prefix = 'ibc'
default_gas = 400000
max_gas = 120000000
gas_price = { price = 0.0025, denom = 'uosmo' }
gas_multiplier = 1.2
max_msg_num = 30
max_tx_size = 1800000
clock_drift = '15s'
max_block_time = '30s'
trusting_period = '4days'
trust_threshold = { numerator = '1', denominator = '3' }
rpc_timeout = '20s'
```
</details>

# Add accounts of Relayer to Hermes
Add osmo-test-5 account to hermes
```
echo "inside jungle bullet jealous round claim globe square feed increase balance eternal garment skill fiscal apple firm holiday permit tray boy summer swallow capable" > ./mnemonic_morecon
hermes --config $HERMES_CONFIG keys add --chain osmo-test-5 --mnemonic-file ./mnemonic_morecon
```
Add shielded-expedition account to hermes
```
hermes --config $HERMES_CONFIG keys add --chain shielded-expedition.88f17d1d14 --key-file $HOME/.local/share/namada/shielded-expedition.88f17d1d14/wallet.toml 
```
check accounts
```
ls $HOME/.hermes/keys/shielded-expedition.88f17d1d14/keyring-test
namada_relayer_morecon.json

ls $HOME/.hermes/keys/osmo-test-5/keyring-test/
osmosis_relayer_moreco.json
```
# Create channel
```
hermes --config $HERMES_CONFIG \  
  create channel \  
  --a-chain shielded-expedition.88f17d1d14 \  
  --b-chain osmo-test-5 \  
  --a-port transfer \  
  --b-port transfer \  
  --new-client-connection --yes
```
<details>
  <summary> SUCCESS Channel </summary>
  
```
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
```
</details>

# Launch Hermes
```
tmux new -s my-hermes
hermes --config $HOME/.hermes/config.toml start 
```
# Send faucets to verify if channels work normally.
```
osmosisd query bank balances osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc
balances:
- amount: "99988373"
  denom: uosmo

namadac balance --owner namada_relayer_morecon --node $RPC_NODE
naan: 1793.487509
```
```
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
```
```
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
```
```
namadac balance --owner namada_relayer_morecon --node $RPC_NODE
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
```

# Conclusion
Namada wallet tnam1qqssedgx7nyak90d9r2dvdw8wn3tx3kr6u2vv5ua received uosmo:  
transfer/channel-340/uosmo: 2000000  

Osmos wallet osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc received naan:  
amount: "1"  
denom: ibc/73703A34EF871C7320A4FA693A23679D43BD8409553D4F71A5F1787BED80A090

---
# UPDATE 2024-2-17

## Update Client
```
hermes update client --host-chain shielded-expedition.88f17d1d14 --client 07-tendermint-1173
```
**New channel:**  
Namada: channel-433   
Osmosis: channel-5894

<details>
  <summary> SUCCESS UpdateClient </summary>
```
SUCCESS [
    UpdateClient(
        UpdateClient {
            common: Attributes {
                client_id: ClientId(
                    "07-tendermint-1173",
                ),
                client_type: Tendermint,
                consensus_height: Height {
                    revision: 5,
                    height: 5653043,
                },
            },
            header: Some(
                Tendermint(
                     Header {...},
                ),
            ),
        },
    ),
    SendPacket(
        SendPacket {
            packet: Packet {
                sequence: Sequence(
                    28,
                ),
                source_port: PortId(
                    "transfer",
                ),
                source_channel: ChannelId(
                    "channel-433",
                ),
                destination_port: PortId(
                    "transfer",
                ),
                destination_channel: ChannelId(
                    "channel-5894",
                ),
                data: [123, 34, 100, 101, 110, 111, 109, 34, 58, 34, 116, 114, 97, 110, 115, 102, 101, 114, 47, 99, 104, 97, 110, 110, 101, 108, 45, 52, 51, 51, 47, 117, 111, 115, 109, 111, 34, 44, 34, 97, 109, 111, 117, 110, 116, 34, 58, 34, 49, 49, 53, 49, 49, 49, 49, 34, 44, 34, 115, 101, 110, 100, 101, 114, 34, 58, 34, 116, 110, 97, 109, 49, 113, 114, 114, 101, 122, 48, 48, 112, 57, 116, 97, 107, 100, 99, 107, 121, 110, 107, 122, 115, 53, 104, 117, 55, 113, 52, 55, 115, 56, 48, 107, 101, 115, 113, 100, 48, 108, 112, 53, 116, 34, 44, 34, 114, 101, 99, 101, 105, 118, 101, 114, 34, 58, 34, 111, 115, 109, 111, 49, 99, 107, 110, 57, 112, 121, 115, 97, 122, 103, 112, 52, 104, 51, 50, 104, 116, 108, 48, 118, 97, 118, 104, 55, 115, 55, 54, 52, 48, 106, 100, 103, 100, 57, 50, 55, 55, 122, 34, 44, 34, 109, 101, 109, 111, 34, 58, 34, 34, 125],
                timeout_height: Never,
                timeout_timestamp: Timestamp {
                    time: Some(
                        Time(
                            2024-02-27 3:18:30.807414412,
                        ),
                    ),
                },
            },
        },
    ),
]
```
</details>

IBC transfer for testing
```
namadac --base-dir $HOME/.local/share/namada \
    ibc-transfer \
    --amount 1 \
    --source namada_relayer_morecon \
    --receiver osmo1lj40hntzcufr6l68e0gnkm88vgd2k7ys3qz5nc \
    --token naan \
    --channel-id channel-433 \
    --node http://75.119.137.202:26657 \
    --memo tpknam1qqm2cd40luwxhpaath4f9nf38d8f8rhy47jl25nn03gtlw4cpjkezjl85ud
Enter your decryption password: 
Transaction added to mempool.
Wrapper transaction hash: D0CBEA1A28B6F2DAB91EDE692FE2655D7CC26E20BF14E27DE478E632F611F195
Inner transaction hash: 60DB065D5D776DECF65BD6A5A575BCEACAB4CEAF83F1B60C874BA2236A23B75A
Wrapper transaction accepted at height 76095. Used 26 gas.
Waiting for inner transaction result...
Transaction was successfully applied at height 76096. Used 6193 gas.

osmosisd tx ibc-transfer transfer \
  transfer \
  channel-5894 \
  tnam1qqssedgx7nyak90d9r2dvdw8wn3tx3kr6u2vv5ua \
  1000000uosmo \
  --from osmosis_relayer_morecon \
  --gas auto \
  --gas-prices 0.025uosmo \
  --gas-adjustment 1.2 \
  --node http://127.0.0.1:26657 \
  --home $HOME/.osmosisd \
  --chain-id osmo-test-5 \
  --yes
Enter keyring passphrase (attempt 1/3):
gas estimate: 116598
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
txhash: E6FDDD5C8C90610CFEDAF062F34936FA7C0F803A074C3A083FC7B40E23F25CB3
```
