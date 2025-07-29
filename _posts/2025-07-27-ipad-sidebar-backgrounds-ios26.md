---
layout: post
title: "iPad Sidebar Backgrounds in iOS 26"
date: 2025-07-27 22:00:00 -0300
categories: ios uikit til
tags: [iOS, UIKit, iPad, iOS26]
excerpt: "Example of the new UIBackgroundExtensionView API in iOS 26 that extends background images under translucent sidebars on iPad."
image: "/assets/images/ios26-sidebar-background-extension.png"
---

## UIBackgroundExtensionView for iPad Sidebar Backgrounds

[WWDC 25: Build a UIKit app with the new design (see timestamp)](https://developer.apple.com/videos/play/wwdc2025/284/?time=351) shows how to use `UIBackgroundExtensionView` to extend iPad background images under the new Liquid Glass sidebars. The image is reflected and extended under the sidebar for a neat visual effect:

![iOS 26 Sidebar Background Extension](/assets/images/ios26-sidebar-background-extension.png)


You'll notice how the mountain line seen under the translucent sidebar is a reflection of the visible mountain line.

Here's some sample code:

```swift
class PrimaryViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .clear
        
        // Create your background content
        let posterImageView = UIImageView(image: .yosmite)
        posterImageView.contentMode = .scaleAspectFill
        
        // Wrap it in UIBackgroundExtensionView
        let extView = UIBackgroundExtensionView()
        extView.contentView = posterImageView
        
        // Add and constrain to fill the view
        view.addSubview(extView)
        extView.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            extView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            extView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            extView.topAnchor.constraint(equalTo: view.topAnchor),
            extView.bottomAnchor.constraint(equalTo: view.bottomAnchor),
        ])
    }
}
```