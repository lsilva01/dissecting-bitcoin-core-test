# Dissecting Bitcoin Core

Proposal: Review and update of the book "Dissection of Bitcoin"

## Brief Contents
* [Architecture](1_0_Bitcoin_Architecture.asciidoc)
* [ADDR Relay](2_0_ADDR_relay.asciidoc)
* [Evicting An Incoming Connection](2_2_evicting_incoming_connection.asciidoc)
* [Evicting An Outgoing Connection](2_3_evicting_outgoing_connection.asciidoc)
* [Adresses](3_0_bitcoin_address.asciidoc)
* [Transactions](3_1_transactions.asciidoc)
* [Serialization](4_0_serialization.asciidoc)
* [Functional Test Framework](5_0_functional_test_framework.asciidoc)

## Contents in Detail
* [Architecture](1_0_Bitcoin_Architecture.asciidoc)
  * [Executables](1_0_Bitcoin_Architecture.asciidoc#executables)
  * [Protocol - P2P](1_0_Bitcoin_Architecture.asciidoc#protocol_p2p)
  * [Concurrency Model](1_0_Bitcoin_Architecture.asciidoc#concurrency_model)
    * [TraceThread](1_0_Bitcoin_Architecture.asciidoc#trace_tread)
    * [Script Verification](1_0_Bitcoin_Architecture.asciidoc#script-verification)
    * [Loading Blocks](1_0_Bitcoin_Architecture.asciidoc#loading-blocks)
    * [Servicing RPC Calls](1_0_Bitcoin_Architecture.asciidoc#servicing-rpc-calls)
    * [Load Peer Addresses From DNS Seeds](1_0_Bitcoin_Architecture.asciidoc#load-peer-adresses-from-dns-seeds)
    * [Send And Receive Messages To And From Peers](1_0_Bitcoin_Architecture.asciidoc#send-and-receive-messages-to-and-from-peers)
    * [Initializing Network Connections](1_0_Bitcoin_Architecture.asciidoc#initializing-network-connections)
    * [Opening Added Network Connections](1_0_Bitcoin_Architecture.asciidoc#opening-added-network-connections)
    * [Process Messages from `net` -> `net_processing`](1_0_Bitcoin_Architecture.asciidoc#process-messages-from-net-net-processing)
  * [Notifications Mechanism (`ValidationInterface`)](1_0_Bitcoin_Architecture.asciidoc#notification-mechanism)
  * [Regions](1_0_Bitcoin_Architecture.asciidoc#regions)
    * [`net.{h,cpp}`](1_0_Bitcoin_Architecture.asciidoc#nethcpp)
    * [`net_processing.{h,cpp}`](1_0_Bitcoin_Architecture.asciidoc#net_processinghcpp)
    * [`validation.{h,cpp}`](1_0_Bitcoin_Architecture.asciidoc#validationhcpp)
    * [`txmempool.{h,cpp}`](1_0_Bitcoin_Architecture.asciidoc#txmempoolhcpp)
    * [`coins.{h,cpp} & txdb.{h,cpp}`](1_0_Bitcoin_Architecture.asciidoc#coinshcpptxdbhcpp)
    * [`dbwrapper.{h,cpp} & indexes/`](1_0_Bitcoin_Architecture.asciidoc#dbwrapperhcppandindexes)
    * [`script/`](1_0_Bitcoin_Architecture.asciidoc#script_region)
    * [`consensus/`](1_0_Bitcoin_Architecture.asciidoc#consensus_region)
    * [`policy/`](1_0_Bitcoin_Architecture.asciidoc#policy_region)
    * [`interface/`](1_0_Bitcoin_Architecture.asciidoc#interface_region)
    * [`qt/`](1_0_Bitcoin_Architecture.asciidoc#qt_region)
    * [`rpc/`](1_0_Bitcoin_Architecture.asciidoc#rpc_region)
    * [`wallet/`](1_0_Bitcoin_Architecture.asciidoc#wallet_region)
    * [`miner.{h,cpp}`](1_0_Bitcoin_Architecture.asciidoc#miner_region)
  * [Summary](1_0_Bitcoin_Architecture.asciidoc#summary)
  * [References](1_0_Bitcoin_Architecture.asciidoc#references)
* [ADDR Relay](2_0_ADDR_relay.asciidoc)
  * [Connection Types](2_0_ADDR_relay.asciidoc#connection-types)
  * [Peer seeding sources](2_0_ADDR_relay.asciidoc#peer_seeding_sources)
    * [1. Loaded from `peers.dat`](2_0_ADDR_relay.asciidoc#loaded_from_peers_dat)
    * [2. DNS Seeds](2_0_ADDR_relay.asciidoc#dns_seeds)
    * [3. Fixed seeds](2_0_ADDR_relay.asciidoc#fixed_seeds)
    * [4. `ADDR_FETCH` via `-seednode`](2_0_ADDR_relay.asciidoc#addr_fetch)
    * [5. Manual connection in `-connect` mode](2_0_ADDR_relay.asciidoc#connect_mode)
    * [6. Manual connection with `-addnode`](2_0_ADDR_relay.asciidoc#addnode_manual)
  * [Initial Connection](2_0_ADDR_relay.asciidoc#initial_connection)
  * [`ADDR` or `ADDRV2`](2_0_ADDR_relay.asciidoc#addr_or_addrv2)
  * [`RelayAddress()`](2_0_ADDR_relay.asciidoc#relay_address)
  * [`MaybeSendAddr()`](2_0_ADDR_relay.asciidoc#maybe_send_addr)
  * [`GETADDR`](2_0_ADDR_relay.asciidoc#getaddr)
  * [Summary](2_0_ADDR_relay.asciidoc#summary)
* [Address Manager (AddrMan)](2_1_AddrMan.asciidoc)
  * [Original Implementation](2_1_AddrMan.asciidoc#original_implementation)
  * [Eclipse Attack](2_1_AddrMan.asciidoc#eclipse_attack)
    * [Deployed Countermeasures for Eclipse Attacks](2_1_AddrMan.asciidoc#deployed_eclipse)
      * [Countermeasure 4: Feeler Connections](2_1_AddrMan.asciidoc#eclipse_c4)
      * [Countermeasure 3: test-before-evict](2_1_AddrMan.asciidoc#eclipse_c3)
      * [Countermeasure 1: Deterministic random eviction](2_1_AddrMan.asciidoc#eclipse_c1)
      * [Countermeasure 2: Random selection](2_1_AddrMan.asciidoc#eclipse_c2)
      * [Countermeasure 6: More buckets](2_1_AddrMan.asciidoc#eclipse_c6)
      * [Countermeasure 5: Anchor connections](2_1_AddrMan.asciidoc#eclipse_c5)
    * [Undeployed (or partially deployed) countermeasures](2_1_AddrMan.asciidoc#undeployed_eclipse)
      * [Countermeasure 7: More outgoing connections](2_1_AddrMan.asciidoc#eclipse_c7)
      * [Countermeasure 8: Ban unsolicited ADDR messages](2_1_AddrMan.asciidoc#eclipse_c8)
      * [Countermeasure 9: Diversify incoming connections](2_1_AddrMan.asciidoc#eclipse_c9)
      * [Countermeasure 10: Anomaly detection](2_1_AddrMan.asciidoc#eclipse_c10)
  * [Erebus Attack](2_1_AddrMan.asciidoc#erebus_attack)
    * [Deployed Countermeasures for Erebus Attack](2_1_AddrMan.asciidoc#deployed_erebus)
      * [Countermeasure 2: More outgoing connections](2_1_AddrMan.asciidoc#erebus_c2)
      * [Countermeasure 3:  Selecting peers with AS topology information](2_1_AddrMan.asciidoc#erebus_c3)
    * [Undeployed countermeasures](2_1_AddrMan.asciidoc#undeployed_erebus)
      * [Countermeasure 1: Table size reduction](2_1_AddrMan.asciidoc#erebus_c1)
      * [Countermeasure 4: Smarter eviction policy](2_1_AddrMan.asciidoc#erebus_c4)
  * [Current Implementation](2_1_AddrMan.asciidoc#current_implementation)
    * [`CNetAddr`](2_1_AddrMan.asciidoc#cnetaddr)
    * [`CService`](2_1_AddrMan.asciidoc#cservice)
    * [`CAddress`](2_1_AddrMan.asciidoc#caddress)
    * [`CAddrInfo`](2_1_AddrMan.asciidoc#caddrinfo)
    * [`CAddrMan`](2_1_AddrMan.asciidoc#caddrman)
  * [Adding New Address](2_1_AddrMan.asciidoc#adding_new_address)
  * [Connecting to an Address](2_1_AddrMan.asciidoc#connecting_to_an_address)
  * [Test-Before-Evict](2_1_AddrMan.asciidoc#test-vefore-evict)
  * [Responding to `GETADDR` Messages](2_1_AddrMan.asciidoc#responding_getaddr_messages)
  * [Summary](2_1_AddrMan.asciidoc#summary)
  * [References](2_1_AddrMan.asciidoc#references)
* [Evicting An Incoming Connection](2_2_evicting_incoming_connection.asciidoc)
  * [Eviction Mechanism](2_2_evicting_incoming_connection.asciidoc#eviction_mechanism)
    * [Rule 1: Protect 4 peers with the highest network group](2_2_evicting_incoming_connection.asciidoc#rule_1)
    * [Rule 2: Protect 8 peers with the lowest minimum ping time](2_2_evicting_incoming_connection.asciidoc#rule_2)
    * [Rule 3: Protect 4 peers that most recently sent us novel transactions](2_2_evicting_incoming_connection.asciidoc#rule_3)
    * [Rule 4: Protect up to 8 block-relay only peers that have sent novel blocks](2_2_evicting_incoming_connection.asciidoc#rule_4)
    * [Rule 5: Protect 4 peers that most recently sent us novel blocks](2_2_evicting_incoming_connection.asciidoc#rule_5)
    * [Rule 6: Protect up to 1/4 peers connected via the onion service](2_2_evicting_incoming_connection.asciidoc#rule_6)
    * [Rule 7: Any remaining slots of the 1/4 peers connected, or at least 2 additional slots, are allocated to protect localhost peers](2_2_evicting_incoming_connection.asciidoc#rule_7)
    * [Rule 8: Protect the remaining peers until completing half of them all, prioritizing those with longest uptime](2_2_evicting_incoming_connection.asciidoc#rule_8)
    * [Rule 9: Consider only the peers preferred for eviction to be disconnected](2_2_evicting_incoming_connection.asciidoc#rule_9)
    * [Rule 10: Identify the network group with the most connections and evict the youngest member](2_2_evicting_incoming_connection.asciidoc#rule_10)
  * [Summary](2_2_evicting_incoming_connection.asciidoc#summary)
  * [References](2_2_evicting_incoming_connection.asciidoc#references)
* [Evicting An Outgoing Connection](2_3_evicting_outgoing_connection.asciidoc)
  * [Disconnecting from bad outbound peers with bad headers chains in IBD](2_3_evicting_outgoing_connection.asciidoc#disconnecting_headers_chains_ibd)
  * [Disconnection of outbound peers on bad/slow chains](2_3_evicting_outgoing_connection.asciidoc#disconnection_bad_slow)
  * [Connecting to a new outbound peer if the node's tip is stale](2_3_evicting_outgoing_connection.asciidoc#connecting_new_peer)
  * [Summary](2_3_evicting_outgoing_connection.asciidoc#summary)
* [Adresses](3_0_bitcoin_address.asciidoc)
* [Transactions](3_1_transactions.asciidoc)
* [Serialization](4_0_serialization.asciidoc)
  * [Original Implementation](4_0_serialization.asciidoc#original-implementation)
  * [Relevant Changes](4_0_serialization.asciidoc#relevant-changes)
    * [PR #352 - secure_allocator and zero_after_free_allocator](4_0_serialization.asciidoc#pr-352---secure_allocator-and-zero_after_free_allocator)
    * [PR #1567 - CHashWriter](4_0_serialization.asciidoc#pr-1567---chashwriter)
    * [PR #2409 - CSerializeData](4_0_serialization.asciidoc#pr-2409---cserializedata)
    * [PR #4508 - CSizeComputer](4_0_serialization.asciidoc#pr-4508---csizecomputer)
    * [PR #5510 - Big endian support](4_0_serialization.asciidoc#pr-5510---big-endian-support)
    * [PR #6914 - prevector](4_0_serialization.asciidoc#pr-6914---prevector)
  * [Transactions Serialization - Current Implementation (BIP 144)](4_0_serialization.asciidoc#transactions-serialization---current-implementation)
    * [VarInt](4_0_serialization.asciidoc#varint)
    * [SerializeTransaction()](https://github.com/leonardojobim/Dissection-Bitcoin-Test/blob/main/4_0_serialization.asciidoc#serializetransaction)
    * [UnserializeTransaction()](4_0_serialization.asciidoc#unserializetransaction)
    * [Serializing a transaction - Process](4_0_serialization.asciidoc#serializing-a-transaction---process)
      * [Sending Requested Transaction](4_0_serialization.asciidoc#sending-requested-transaction)
* [Functional Test Framework](5_0_functional_test_framework.asciidoc)
  * [`test/test_framework/*`](5_0_functional_test_framework.asciidoc#test_test_framework)
  * [BitcoinTestFramework](5_0_functional_test_framework.asciidoc#bitcointestframework_section)
  * [TestNode](5_0_functional_test_framework.asciidoc#testnode_section)
  * [P2PInterface](5_0_functional_test_framework.asciidoc#p2pinterface_section)
  * [Test 1 - Sending Money](5_0_functional_test_framework.asciidoc#test_1_sending_money)
  * [Test 2 - Expected Mempool Behavior](5_0_functional_test_framework.asciidoc#test_2_expected_mempool_behavior)
  * [Test 3 - Adding `P2PInterface` Connections](5_0_functional_test_framework.asciidoc#test_3_adding_p2pinterface_connections)
  * [Summary](5_0_functional_test_framework.asciidoc#summary)
  * [References](5_0_functional_test_framework.asciidoc#references)