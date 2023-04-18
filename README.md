# FusionMedia
FusionMedia helps to integrate the media player to play the media files.

<p align="center">
<img src="images/image3.gif" alt="gif" width="400"/>
</p>


## Add it to the `Package.swift`

```swift
dependencies: [
    .package(
      name: "FusionMedia", url: "https://github.com/scade-platform/FusionMedia.git", .branch("main")
    ),
    .package(
      name: "ScadeExtensions", url: "https://github.com/scade-platform/ScadeExtensions.git",
      .branch("main")),
  ],
  targets: [
    .target(
      name: "FusionAudioPlayerDemo",
      dependencies: [
        .product(name: "FusionMedia", package: "FusionMedia"),
        .product(name: "ScadeExtensions", package: "ScadeExtensions"),
      ],
      exclude: ["main.page"],
      swiftSettings: [
        .unsafeFlags(["-F", SCADE_SDK], .when(platforms: [.macOS, .iOS])),
        .unsafeFlags(["-I", "\(SCADE_SDK)/include"], .when(platforms: [.android])),
      ]
    )
  ]
```

## Import & Use `FusionMedia`

```swift
  var player: FusionMedia.AudioPlayer?
  
  let playImage = "./Assets/play1.svg"
  let stopImage = "./Assets/stop1.svg"
  let maxVolume = 100
  var duration: Double = 180
  var timer: Timer!
```


```swift
    //setup player
 self.player = FusionMedia.AudioPlayer(url: URL(forResource: "Assets/Ketsa-Good_Vibe.mp3"))
```


### Setup the Slider to navigate to the play track
```swift
// setup slider
    if let player = self.player {
      self.positionSlide.maxValue = Int(player.duration)
      self.volumeSlide.maxValue = maxVolume
      self.volumeSlide.position = Int(player.volume * Float(maxVolume))
    }
```

```swift
    self.positionSlide.onSlide { e in self.onPositionChanged(ev: e) }
    self.volumeSlide.onSlide { e in self.onVolumeChanged(ev: e) }

    // setup stop/play button
    self.playStopButton.onClick { _ in self.playStopButtonClicked() }
```

```swift
  func onPositionChanged(ev: SCDWidgetsSliderLineEvent?) {
    let sliderPosition = Double(ev!.newValue)
      player?.seek(to: sliderPosition)
  }
```

### Change Volume level
```swift
  func onVolumeChanged(ev: SCDWidgetsSliderLineEvent?) {
    let sliderPosition = ev!.newValue

    player?.volume = Float(sliderPosition) / Float(self.maxVolume)
  }
```

## Stop the Media Player
```swift
 func playStopButtonClicked() {

    guard let player = player else { return }

    // toggle state and change button accordingly

    let imageUrl = player.isPlaying ? playImage : stopImage
    self.playStopButton.url = imageUrl
    player.isPlaying ? player.stop() : player.play()

    timer = Timer.scheduledTimer(withTimeInterval: 1, repeats: true) { timer in
      self.positionSlide.position = Int(player.currentTime)
      if !player.isPlaying {
        timer.invalidate()
      }
    }
  }
```

## Contribution

<p>Consider contributing by creating a pull request (PR) or opening an issue. By creating an issue, you can alert the repository's maintainers to any bugs or missing documentation you've found. 🐛📝 If you're feeling confident and want to make a bigger impact, creating a PR, can be a great way to help others. 📖💡 Remember, contributing to open source is a collaborative effort, and any contribution, big or small, is always appreciated! 🙌 So why not take the first step and start contributing today? 😊</p>
