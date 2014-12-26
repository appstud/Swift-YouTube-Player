# YouTubePlayer

Embed and control YouTube videos in your iOS applications! Neato, right? Let's see how it works.

## Example

```Swift
// Import Swift module
import YouTubePlayer
```

Build and lay out the view however you wish, whether in IB:
```Swift
@IBOutlet var videoPlayer: YouTubePlayer!
```
…or programmatically:
```Swift
// init YouTubePlayer w/ playerFrame rect (assume playerFrame declared)
var videoPlayer = YouTubePlayer(frame: playerFrame)
```

Give the player a video to load, whether from ID or URL.
```Swift
// Load video from YouTube ID
videoPlayer.loadVideoID("nfWlot6h_JM")
```
```Swift
// Load video from YouTube URL
let myVideoURL = NSURL(string: "https://www.youtube.com/watch?v=wQg3bXrVLtg")
videoPlayer.loadVideoURL(myVideoURL!)
```

## Controlling VideoPlayerView

Each `VideoPlayerView` has controls for loading, playing, pausing, stopping, clearing, and seeking videos. They are:

* `func loadVideoURL(videoURL: NSURL)`
* `func loadVideoID(videoID: String)`
* `func play()`
* `func pause()`
* `func stop()`
* `func clear()`
* `func seekTo(seconds: Float, seekAhead: Bool)`

Please note that calls to all but the first two methods will result in a JavaScript runtime error if they are called before the player is ready. The player will not be ready until shortly after a call to either `loadVideoURL(videoURL: NSURL)` or `loadVideoID(videoID: String)`. You can check the readiness of the player at any time by checking its `ready: Bool` property. These functions run asynchronously, so it is not guaranteed that a call to a play function will be safe if it immediately follows a call to a load function. I plan to update the library soon to add completion handlers to be called when the player is ready.

In the meantime, you can also the `YouTubePlayerDelegate` method `playerReady(videoPlayer: YouTubePlayer)` to ensure code is executed immediately when the player becomes ready.

## Responding to events

YouTube's iFrame player emits certain events based on the lifecycle of the player. The `YouTubePlayerDelegate` outlines these methods that get called during a player's lifecycle. They are:

* `func playerReady(videoPlayer: YouTubePlayer)`
* `func playerStateChanged(videoPlayer: YouTubePlayer, playerState: YouTubePlayerState)`
* `func playerQualityChanged(videoPlayer: YouTubePlayer, playbackQuality: YouTubePlaybackQuality)`

*Side note:* unfortunately, due to the way Swift protocols work, these are all required delegate methods. Setting a delegate on an instance of `YouTubePlayer` is optional, but any delegate must conform to `YouTubePlayerDelegate` and therefore must implement every one of the above methods. I wish there were a better way around this, but declaring the protocol as an `@objc` protocol means I wouldn't be able to use enum values as arguments, because Swift enums are incompatible with Objective-C enumerations. Feel free to file an issue if you know of some solution that lets us have optional delegate methods as well as the ability to pass Swift enums to these delegate methods.
