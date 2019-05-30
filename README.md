# VerusMediaSDK
##### Requirements:

* iOS 12.1

1. [ Installation ](#ins)
2. [ How to get Video Ads. ](#video)
2. [ How to get Native Ads. ](#native)
2. [ How to get Banner Ads. ](#banner)

<a name="ins"></a>
## Installation
•  We use CocoaPods as a dependency manager, our pod is located in a private repository and you can integrate it to your project like this:
```
#Podfile example

source 'https://github.com/verusmedia/ios-podspecs.git'
source 'https://github.com/CocoaPods/Specs.git'

target 'YourApp' do
    use_frameworks!
    pod 'VerusMediaSDK', '0.1.0-beta.1'
end
```

•  In every class that you're going to use our SDK you need to import it like this:
```swift
import VerusMediaSDK
```

•  If you want to show VeriView™ Video Ads, you need to add Camera Privacy settings to your plist: 
```
<key>NSCameraUsageDescription</key>
	<string>Watch the entire ad to earn rewards</string>
```
<a name="video"></a>
### How to get Video Ads
To get a video ad you need to send your placementId, and a verified flag. The verified flag is used determine whether we show a VeriView™ Video Ad or a regular Video Ad.

You also need to conform to `VMVideoAdDelegate`. It has a finished method that will be called with a `VMVideoAdStatus` when the Ad finishes.
The `VMVideoAdStatus` enumeration types are:

`COMPLETED`, `COMPLETED_VERIFIED`, `AD_ERROR_RS` and `NO_INTERNET`

Here is an example on how to get a Video Ad from a ViewController:

```swift
let view = VMVideoAdViewController.instance(placementId: "yourPlacementId", verified: true)
view?.delegate = self
self.present(view!, animated: true)
```
<a name="native"></a>
### How to get Native Ads
To get native ads you need to send your placementId as a parameter when instance a `VMNativeAd`.

You also need to conform to `VMNativeAdDelegate`, to return the UI elements where the ad is going to render, such as: **Title**, **Description**, **Image** and **Call to Action**.

After you instance your Native Ad, you need to call `loadNativeAd() function, which we recomend on doing in `viewDidAppear()`

Here is an example on how to get a Native Ad from a ViewController:

```swift
class ViewController: UIViewController, VMNativeAdDelegate {
    func setTitle() -> UILabel {
        return self.titleLabel
    }
    
    func setCta() -> UIButton {
        return self.ctaButton
    }
    
    func setImg() -> UIImageView {
        return self.imgView
    }
    
    func setDesc() -> UILabel {
        return self.descLabel
    }
    
    @IBOutlet weak var ctaButton: UIButton!
    @IBOutlet weak var titleLabel: UILabel!
    @IBOutlet weak var imgView: UIImageView!
    @IBOutlet weak var descLabel: UILabel!
    
    var nativeAd: VMNativeAd?
	
	override func viewDidLoad() {
        super.viewDidLoad()
        nativeAd = VMNativeAd(placementId: "yourPlacementId")
        nativeAd?.delegate = self
    }
    
    override func viewDidAppear(_ animated: Bool) {
        nativeAd?.loadNativeAd()
    }
```
<a name="banner"></a>
### How to get Banner Ads
To get a banner ad you need your placementId and your app bundleId.

You need to conform to `VMBannerAdViewDelegate` which will tell you when the Banner loads and when it fails to load.

You need to instance a `VMBannerAdView` and set its UI settings and delegate like this:

```swift
lazy var adView: VMBannerAdView = {
        let ad = VMBannerAdView.init(size: 	        VMBannerAdSize.vmIABMediumRectangle_300x250, placementId: "yourPlacementId", buildId: "yourBundleId")
        ad.delegate = self
        ad.tintColor = UIColor.black.withAlphaComponent(0.15)
        ad.contentMode = .center
        ad.translatesAutoresizingMaskIntoConstraints = false
        return ad
  }()
```
After that you need to set a view (in this example, bannerView) to host the Banner Ad and add it as a subview. Then you call `loadBannerAd()`. Like this:
```swift
bannerView.addSubview(adView)
adView.loadBannerAd()
```

