# Cabal Roadmap

Some rough notes on individual interests that we can try to fit into one big
collective roadmap!

## cblgh
* Finish [kappa workshop](https://noffle.github.io/kappa-arch-workshop/)
    * Write a kappa view using noffle's primitives
    * Write a nonsensical kappa-view from scratch
* Learn about hypercores
    * Read [jimpick's docs](https://dat-dev-story.hashbase.io/extra-guides/breakdown.html)
    * Read [lachenmeyer's docs](https://dissecting-dat-lachenmayer.hashbase.io/)
    * Read [hypercore docs](https://github.com/mafintosh/hypercore)
    * Write on [WTC!HYH](https://hackmd.io/AFhXnZ-3Q3KyAWp0ZbYgUg#)
* Implement cabal-web
    * Dissect [dat shopping list](https://github.com/jimpick/dat-shopping-list)
    * Host a small server on Glitch
    * Prototype using WebSockets
        * Write a small echo bot client+server
    * Prototype using WebRTC
* Implement rekey-ing cabals
    * Implement blind peers
    * Implement private messages
    * Implement private channels
    * Implement `/walkaway`
* Learn React
    * Fix desktop
* Learn (enough) React-Native
    * Fix mobile app

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

