# 0.30.0-rc.1 [2021-10-15]

- Add `ConnectionLimit::with_max_established` (see [PR 2137]).

- Add `Keypair::to_protobuf_encoding` (see [PR 2142]).

- Change `PublicKey::into_protobuf_encoding` to `PublicKey::to_protobuf_encoding` (see [PR 2145]).

- Change `PublicKey::into_peer_id` to `PublicKey::to_peer_id` (see [PR 2145]).

- Change `PeerId::from_public_key(PublicKey)` to `PeerId::from_public_key(&PublicKey)` (see [PR 2145]).

- Add `From<&PublicKey> for PeerId` (see [PR 2145]).

- Remove `TInEvent` and `TOutEvent` trait paramters on most public types.
  `TInEvent` and `TOutEvent` are implied through `THandler` and thus
  superflucious. Both are removed in favor of a derivation through `THandler`
  (see [PR 2183]).

- Require `ConnectionHandler::{InEvent,OutEvent,Error}` to implement `Debug`
  (see [PR 2183]).

- Remove `DisconnectedPeer::set_connected` and `Pool::add` (see [PR 2195]).

- Report `ConnectionLimit` error through `ConnectionError` and thus through
  `NetworkEvent::ConnectionClosed` instead of previously through
  `PendingConnectionError` and thus `NetworkEvent::{IncomingConnectionError,
  DialError}` (see [PR 2191]).

- Report abortion of pending connection through `DialError`,
  `UnknownPeerDialError` or `IncomingConnectionError` (see [PR 2191]).

- Remove deprecated functions `upgrade::write_one`, `upgrade::write_with_len_prefix`
  and `upgrade::read_one` (see [PR 2213]).

- Add `SignedEnvelope` and `PeerRecord` according to [RFC0002] and [RFC0003]
  (see [PR 2107]).

- Report `ListenersEvent::Closed` when dropping a listener in `ListenersStream::remove_listener`,
  return `bool` instead of `Result<(), ()>` (see [PR 2261]).

- Concurrently dial address candidates within a single dial attempt (see [PR 2248]) configured
  via `Network::with_dial_concurrency_factor`.

  - On success of a single address, provide errors of the thus far failed dials via
    `NetworkEvent::ConnectionEstablished::outgoing`.

  - On failure of all addresses, provide the errors via `NetworkEvent::DialError`.

[PR 2145]: https://github.com/libp2p/rust-libp2p/pull/2145
[PR 2213]: https://github.com/libp2p/rust-libp2p/pull/2213
[PR 2142]: https://github.com/libp2p/rust-libp2p/pull/2142
[PR 2137]: https://github.com/libp2p/rust-libp2p/pull/2137
[PR 2183]: https://github.com/libp2p/rust-libp2p/pull/2183
[PR 2191]: https://github.com/libp2p/rust-libp2p/pull/2191
[PR 2195]: https://github.com/libp2p/rust-libp2p/pull/2195
[PR 2107]: https://github.com/libp2p/rust-libp2p/pull/2107
[PR 2248]: https://github.com/libp2p/rust-libp2p/pull/2248
[PR 2261]: https://github.com/libp2p/rust-libp2p/pull/2261
[RFC0002]: https://github.com/libp2p/specs/blob/master/RFC/0002-signed-envelopes.md
[RFC0003]: https://github.com/libp2p/specs/blob/master/RFC/0003-routing-records.md

# 0.29.0 [2021-07-12]

- Switch from `parity-multiaddr` to upstream `multiaddr`.

- Update dependencies.

- Implement `Keypair::from_protobuf_encoding` for ed25519 keys (see [PR 2090]).

- Deprecate `upgrade::write_one`.
  Deprecate `upgrade::write_with_len_prefix`.
  Deprecate `upgrade::read_one`.
  Introduce `upgrade::read_length_prefixed` and `upgrade::write_length_prefixed`.
  See [PR 2111](https://github.com/libp2p/rust-libp2p/pull/2111).

[PR 2090]: https://github.com/libp2p/rust-libp2p/pull/2090

# 0.28.3 [2021-04-26]

- Fix build with secp256k1 disabled [PR 2057](https://github.com/libp2p/rust-libp2p/pull/2057].

# 0.28.2 [2021-04-13]

- Update dependencies.

# 0.28.1 [2021-03-17]

- Update `paritytech-multiaddr` to `>=v0.11.2`.

# 0.28.0 [2021-03-17]

- `Network::dial()` understands `/p2p` addresses and `Transport::dial`
  gets a "fully qualified" `/p2p` address when dialing a specific peer,
  whether through the `Network::peer()` API or via `Network::dial()`
  with a `/p2p` address.

- `Network::dial()` and `network::Peer::dial()` return a `DialError`
  on error.

- Shorten and unify `Debug` impls of public keys.

# 0.27.1 [2021-02-15]

- Update dependencies.

# 0.27.0 [2021-01-12]

- (Re)add `Transport::address_translation` to permit transport-specific
  translations of observed addresses onto listening addresses.
  [PR 1887](https://github.com/libp2p/rust-libp2p/pull/1887)

- Update dependencies.

# 0.26.0 [2020-12-17]

- Make `PeerId` be `Copy`, including small `PeerId` API changes.
  [PR 1874](https://github.com/libp2p/rust-libp2p/pull/1874/).

# 0.25.2 [2020-12-02]

- Require `multistream-select-0.9.1`.

# 0.25.1 [2020-11-25]

- Add missing multiaddr upgrade.

# 0.25.0 [2020-11-25]

- The `NetworkConfig` API is now a builder that moves `self`.
  [PR 1848](https://github.com/libp2p/rust-libp2p/pull/1848/).

- New configurable connection limits for established connections and
  dedicated connection counters. Removed the connection limit dedicated
  to outgoing pending connection _per peer_. Connection limits are now
  represented by `u32` intead of `usize` types.
  [PR 1848](https://github.com/libp2p/rust-libp2p/pull/1848/).

- Update `multihash`.

- Update `multistream-select`.

# 0.24.0 [2020-11-09]

- Remove `ConnectionInfo` trait and replace it with `PeerId`
  everywhere. This was already effectively the case because
  `ConnectionInfo` was implemented on `PeerId`.

# 0.23.1 [2020-10-20]

- Update dependencies.

# 0.23.0 [2020-10-16]

- Rework transport boxing and move timeout configuration
  to the transport builder.
  [PR 1794](https://github.com/libp2p/rust-libp2p/pull/1794).

- Update dependencies.

# 0.22.1 [2020-09-10]

- Require at least parity-multiaddr v0.9.2 in order to fulfill `Ord` bound on
  `Multiaddr`. [PR 1742](https://github.com/libp2p/rust-libp2p/pull/1742).

# 0.22.0 [2020-09-09]

- Simplify incoming connection handling. The `IncomingConnectionEvent`
  has been removed. Instead, pass the `IncomingConnection` obtained
  from `NetworkEvent::IncomingConnection` to `Network::accept()`.
  [PR 1732](https://github.com/libp2p/rust-libp2p/pull/1732).

- Allow any closure to be passed as an executor.
  [PR 1686](https://github.com/libp2p/rust-libp2p/pull/1686)

- Remove `PeerId` compatibility mode for "identity" and SHA2 hashes.
  Historically, before 0.12, `PeerId`s were incorrectly always hashed with SHA2.
  Starting from version 0.13, rust-libp2p accepted both hashed and non-hashed keys as
  input.  Starting from version 0.16 rust-libp2p compared `PeerId`s of "identity" and
  SHA2 hashes equal, which made it possible to connect through secio or noise to nodes
  with an identity hash for the same peer ID. Starting from version 0.17, rust-libp2p
  switched to not hashing the key (i.e. the correct behaviour) while retaining
  equality between peer IDs using the "identity" hash and SHA2. Finally, with
  this release, that will no longer be the case and it is assumed that peer IDs
  whose length is less or equal to 42 bytes always use the "identity" hash so
  two peer IDs are equal if and only if they use the same hash algorithm and
  have the same hash digest. [PR 1608](https://github.com/libp2p/rust-libp2p/pull/1608).

- Return dialer address instead of listener address as `remote_addr` in
  `MemoryTransport` `Listener` `ListenerEvent::Upgrade`
  [PR 1724](https://github.com/libp2p/rust-libp2p/pull/1724).

# 0.21.0 [2020-08-18]

- Remove duplicates when performing address translation
  [PR 1697](https://github.com/libp2p/rust-libp2p/pull/1697).

- Add `transport::Builder::multiplex_ext` for further customisation during
`StreamMuxer` creation. [PR 1691](https://github.com/libp2p/rust-libp2p/pull/1691).

- Refactoring of connection close and disconnect behaviour.  In particular, the former
  `NetworkEvent::ConnectionError` is now `NetworkEvent::ConnectionClosed` with the `error`
  field being an `Option` and `None` indicating an active (but not necessarily orderly) close.
  This guarantees that `ConnectionEstablished` events are always eventually paired
  with `ConnectionClosed` events, regardless of how connections are closed.
  Correspondingly, `EstablishedConnection::close` is now `EstablishedConnection::start_close`
  to reflect that an orderly close completes asynchronously in the background, with the
  outcome observed by continued polling of the `Network`. In contrast, `disconnect`ing
  a peer takes effect immediately without an orderly connection shutdown.
  See [PR 1619](https://github.com/libp2p/rust-libp2p/pull/1619) for further details.

- Add `ConnectedPoint::get_remote_address`
  ([PR 1649](https://github.com/libp2p/rust-libp2p/pull/1649)).

# 0.20.1 [2020-07-17]

- Update ed25519-dalek dependency.

# 0.20.0 [2020-07-01]

- Conditional compilation fixes for the `wasm32-wasi` target
  ([PR 1633](https://github.com/libp2p/rust-libp2p/pull/1633)).

- Rename `StreamMuxer::poll_inbound` to `poll_event` and change the
return value to `StreamMuxerEvent`. This new `StreamMuxerEvent` makes
it possible for the multiplexing layer to notify the upper layers of
a change in the address of the underlying connection.

- Add `ConnectionHandler::inject_address_change`.

# 0.19.2 [2020-06-22]

- Add PartialOrd and Ord for PeerId
  ([PR 1594](https://github.com/libp2p/rust-libp2p/pull/1594)).

- Updated dependencies.

- Deprecate `StreamMuxer::is_remote_acknowledged`
  ([PR 1616](https://github.com/libp2p/rust-libp2p/pull/1616)).
