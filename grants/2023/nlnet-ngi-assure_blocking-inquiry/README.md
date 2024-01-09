# Blocking Behaviour in Secure Scuttlebutt and ActivityPub / Mastodon

To inform the block functionality of [Cable](https://github.com/cabal-club/cable/)'s moderation
system (as of yet unreleased) and blocking's impact on replication an inquiry was made into how
blocking in Secure Scuttlebutt and ActivityPub is described, works, and how users perceive
blocks to work. The inquiry was conducted by using already existing artifacts and information
such as discussion threads, code repository issues, and protocol specifications.

**Inquiry outcome**

Clearly and unambiguously specifying block behaviour and its outcomes, as done in a
specification document rather than in running code, increases client implementation compliance,
cross-implementation consistency, and subsequently prevents clouding the userbase's
understanding of the functionality.

Inheriting blocks from explicitly trusted users should be considered and care should be taken
if the route is chosen. Functionality for announcing blocks as well as taking actions privately
should be present and considered carefully. There is utility in enabling users to let the
blocked user know of the block, but if a choice _must_ be made between 1) always announcing to
the blocker or 2) always keeping the blocker ignorant, the choice must be to keep the blocker
ignorant.

## ActivityPub (AP)

In the ActivityPub-based Mastodon and other Mastodon-compatible social media clients,
user-to-user blocking functionality is widely understood. This is likely due to the familiar
usage context (Mastodon) as well as the existence of the ActivityPub specification (see
relevant excerpts below).

However, due to Mastodon and ActivityPub's federated model wherein users exist as actors on
servers (instances), there does seem to be a source of confusion for users as regards the
effects of a single user blocking an entire instance.

Mastodon: User Blocking Instance Confusion

> * https://github.com/mastodon/mastodon/issues/21739
> * https://docs.joinmastodon.org/user/moderating/#block-domain
> * https://mastodon.social/@Gargron/100174812342300163


ActivityPub: Block Activity

> 
> The Block activity is used to indicate that the posting actor does not want another actor (defined in the object property) to be able to interact with objects posted by the actor posting the Block activity. The server SHOULD prevent the blocked user from interacting with any object posted by the actor.
> 
> Servers SHOULD NOT deliver Block Activities to their object.
> 
> https://www.w3.org/TR/activitypub/#block-activity-outbox


ActivityPub: Undo Activity
 
> The Undo activity is used to undo a previous activity. See the Activity Vocabulary documentation on Inverse Activities and "Undo". For example, Undo may be used to undo a previous Like, Follow, or Block. The undo activity and the activity being undone MUST both have the same actor. Side effects should be undone, to the extent possible. For example, if undoing a Like, any counter that had been incremented previously should be decremented appropriately.
> 
> There are some exceptions where there is an existing and explicit "inverse activity" which should be used instead. Create based activities should instead use Delete, and Add activities should use Remove.
> 
>  Servers performing delivery to the inbox or sharedInbox properties of actors on other servers MUST provide the object property in the activity: Create, Update, Delete, Follow, Add, Remove, Like, Block, Undo. Additionally, servers performing server to server delivery of the following activities MUST also provide the target property: Add, Remove. 
>
> https://www.w3.org/TR/activitypub/#undo-activity-outbox

## Secure Scuttlebutt (SSB)

In SSB, the state of blocking varies across different clients (mostly depending on which
modules are used or not) and so does the understanding of blocking mechanics among the
userbase, ultimately leading to a great source of confusion.

It is important to note that the situation is not a result of carelessness, but instead stems
from a difference in methodology, its unique peer-to-peer nature and how data spreads depending
on whom is followed, the project's grassroots beginnings, and a "we make the road by walking
it" spirit.

Note: The [SSB Protocol Guide](https://ssbc.github.io/scuttlebutt-protocol-guide/) doesn't mention
block in a significant manner, which is why it is not used in this summary.

From module `ssb-friends.js`:

> Relations between feeds are represented as non-zero numbers, as follows:
> 
> In SSB we use `1` to represent a follow, `-1` to represent a block, `-2` to
> represent unfollow.
> 
> A feed with distance `2` is a "friend of a friend" (we follow someone `+1`
> who follows them `+1` which sums up as `2`). The distance `-2` can mean either
> blocked by a friend or unfollowed by us.
> 
> If a friend follows someone another friend blocks, the friends follow wins,
> but if you block them directly, that block wins over the friend's follow.
> 
> https://github.com/ssbc/ssb-friends/blob/51823ee1ee2b8305adfd0d5c6498b608f3be840a/README.md?plain=1#L175-L185

From module `ssb-replicate.js`:

>     //if they are blocked, just end immediately.
>     //better would be same error as if we didn't have this feed.
>     if(blocks[upto.id] && blocks[upto.id][remote_id])
>       return function (abort, cb) { cb(true) }
>
> https://github.com/ssbc/ssb-replicate/blob/5035687b2f7305375360c2e8f69a5819cd66969e/legacy.js#L162-L166

From module `ssb-replicate.js`:

>      /*
>        edge case: if A is replicating B from C in real time as B blocks A,
>        C allows A to continue replicating B's messages, until the block message appears.
>        but would not allow A to replicate any of B on the next connection.
>        if B blocks in a private message to C, this may allow A to replicate past the block message,
>        or if B blocks A via a blocklist, there may be no message.
>        this is still currently good enough but will need to change in the future.
>      */
> 
> https://github.com/ssbc/ssb-replicate/blob/5035687b2f7305375360c2e8f69a5819cd66969e/legacy.js#L197-L207

An issue in the repository for Patchwork (a well-used SSB GUI client):
 
> Issue: Blocked people still visible when a friend replies to them #1132
> 
> I'm looking at A's profile page. A has replied to a post by B. It shows up like this:
> 
>     B: my full original post
>                 View full thread (22)
>     A: my reply to B
> 
> Since I've blocked B I shouldn't see their post. I don't know what behavior would be better:
> 
> 1. Hide this entire thread
> 2. Replace B's original post with "(message from blocked user)" which links to the blocked user's profile page
> 
> https://github.com/ssbc/patchwork/issues/1132

Andre Staltz (in thread _Using blurhash to remember blocked feeds_):
 
> Currently, when blocking someone on SSB (js stack), it just stops replicating that feed, but it doesn’t delete their content. This will change soon in Manyverse because block will mean deleting all of that person’s content from your device.
> 
> %GB1PN9TpBvYsx7Yb5j+XpweuUusnZy9EDPA4kpSLe98=.sha256

Dominic Tarr (in a thread relating to inheriting blocks from other users):

> @cryptix not-replicating friend’s blocks is actually just a one-line change in ssb-friends@3. to clarify the case @moid brings up: If you directly follow someone, you would still replicate C if another friend blocks (B) them, but if you don’t follow them, and a friend blocks them, then stop replicating
> 
> %LZX5xK7BqiA40zTGWWuuXrSySKmRZ5jdrSsoEvE4hdo=.sha256

Cinnamon (in the same thread as Dominic, replying to another user):

> Blocking actually is just mute, right now. It only affects the person who did the blocking, not anyone else.
> 
> For example your feed has been blocked by one person I follow, but I can still see you.
> 
> %Gzsrhl5DMX46jM7pM1KOni6dwAqGZ1w4XY9KILUWmew=.sha256

