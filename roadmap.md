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
 * Implement `cabal-client`, a toolbox for building cabal clients 
 * misc goals
   * Research easily transmittable cabal keys 
      * QR codes, shortened URIs, getting cabal key from swarming on a human friendly identifier
   * Implement Slack gateway
   * Sync over bluetooth

## noffle
* cabal is *stable*
  * get cabal-desktop working again
  * think about ways to scale peer count well past 128 hypercores
  * cabal-mobile works again
* cabal is *secure*
  * symmetric key encryption between peers (using cabal key)
  * ensure we're using the hash of the cabal key for discovery
* cabal is *useful*
  * unread messages/channels tracking (cabal-client)
  * private messages

