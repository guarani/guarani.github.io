---
layout: post
title: "Swift 6.2 @concurrent in Practice"
date: 2025-08-24 15:00:00 -0300
categories: swift concurrency instruments performance
tags: [Swift, Swift6.2, Concurrency, Instruments, @concurrent, Performance]
excerpt: "@concurrent in practice"
---

## New Changes to Concurrency

When a list of images is loaded from the filesystem, it's common to have code that looks something like this:

```swift
func makeThumbnail() async -> UIImage? {
    return UIImage(contentsOfFile: imagePath)?
        .preparingThumbnail(of: CGSize(width: 100, height: 100))
}

var body: some View {
    Image(uiImage: thumbnail ?? placeholderImage)
        .task {
            thumbnail = await makeThumbnail()
        }
}
```

This works great to ensure the work of creating a thumbnail is done on a concurrent background thread. 

But what happens when Swift 6.2's Approachable Concurrency is enabled in Xcode 26? 

## The Problem: Nonisolated Async Functions Change Behavior


With Approachable Concurrency enabled, **nonisolated async functions run on the caller's actor by default**. Since SwiftUI view bodies and `.task` closures are bound to `@MainActor`, the thumbnail generation suddenly runs on the main thread again - causing a significant hang.

## The Fix: `@concurrent` to the Rescue

The solution is Swift 6.2's new `@concurrent` attribute, which explicitly switches off from the caller's actor:

```swift
@concurrent func makeThumbnail() async -> UIImage? {
    return UIImage(contentsOfFile: imagePath)?
        .preparingThumbnail(of: CGSize(width: 100, height: 100))
}
```

The calling task stays exactly the same.

The result is no more hangs and the images load concurrently again.
