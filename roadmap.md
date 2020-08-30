# Cabal Roadmap

Some rough notes on individual interests that we can try to fit into one big
collective roadmap!

## cblgh
* Implement cabal-web
    * Prototype using WebSockets
    * Prototype using WebRTC
* Implement rekey-ing cabals
    * Implement blind peers (others can rehost your cabal without being able to read it)
    * Implement private messages
    * Implement private channels
    * Implement `/walkaway`
 * Implement support for binary data e.g. images
 * Implement protocol support for VoIP
 * ~~Implement `cabal-client`, a toolbox for building cabal clients~~, [done](https://github.com/cabal-club/cabal-client)
 * misc goals
   * ~~Research easily transmittable cabal keys~~ Accomplished
      * ~~QR codes~~ [done](https://github.com/cabal-club/cabal-cli/pull/136), 
      * ~dns-shortened URIs~ [done](https://github.com/datprotocol/dat-dns/pull/15), 
      * ~getting cabal key from swarming on a human friendly identifier~ [done](https://github.com/cabal-club/cabal-client/pull/63)
   * Implement Slack gateway
   * Sync over bluetooth
   * Fix cabal-irc bridge

## nettle
* cabal is *stable*
  * ~get cabal-desktop working again~ done, the [latest releases](https://github.com/cabal-club/cabal-desktop/releases/) are :fire:
  * think about ways to scale peer count well past 128 hypercores, underway with [sparse kappa views](https://github.com/peermaps/kappa-sparse-query)
  * cabal-mobile works again
* cabal is *secure*
  * ~symmetric key encryption between peers (using cabal key)~ [done](https://github.com/cabal-club/cabal-core/commit/3452e480ac2f9aa81f894d6449fc5d71c12c0a52)
  * ~ensure we're using the hash of the cabal key for discovery~ [done](https://github.com/cabal-club/cabal-core/commit/e7af46c89a569c95bcc7ec49c4fd1aec15be3771)
* cabal is *useful*
  * ~unread messages/channels tracking~ [done](https://github.com/cabal-club/cabal-client) 
    * (cblgh: would be good to persist to unread state to disk, tho)
 * misc goals
  * private messages

