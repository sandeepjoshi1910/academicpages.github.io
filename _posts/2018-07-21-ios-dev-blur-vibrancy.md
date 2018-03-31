---
title: 'iOS Dev Reference - Blur and Vibrancy'
date: 2018-02-17
permalink: /posts/iosblurandvibrancy
tags:
  - iOS
  - UI
  - blur
  - vibrancy
---

We often need to get blur on an image view or have a blur view on any dynamic background. There are two ways to get this done.
- Have a blur view with desired blur effect and add any label.
- Have a blur view and vibrancy view. Then add some content like text on top of these.
Adding a Blur view -<br>

``` swift
let blurEffect = UIBlurEffect(style: .regular)
let blurredEffectView = UIVisualEffectView(effect: blurEffect)
blurredEffectView.frame = view.bounds
view.addSubview(blurredEffectView)

```

Add a Label to blur view

``` swift

let tLabel : UILabel = UILabel()
tLabel.text = "Good Morning"
tLabel.font = UIFont(name: "Futura", size: 30)
tLabel.textColor = UIColor.white.withAlphaComponent(0.6)
tLabel.frame = blurredEffectView.contentView.bounds
blurredEffectView.contentView.addSubview(tlabel)

```

Adding vibrancy and a label

``` swift

let vibrancyEffect = UIVibrancyEffect(blurEffect: blurEffect)
let vibrancyView = UIVisualEffectView(effect: vibrancyEffect)
vibrancyView.frame = blurredEffectView.contentView.bounds
vibrancyView.contentView.addSubview(tLabel)
tLabel.frame = vibrancyView.contentView.bounds
blurredEffectView.contentView.addSubview(vibrancyView)
view.addSubview(blurredEffectView)

```

<img src='/images/ios_blur.png'>

### References

- [RayWanderlich’s UIVisualEffectView tutorial]
- [Working with UIVisualEffectView]
- [StackOverflow question on blur & vibrancy]
- [OmniDev website’s guide on blur & vibrancy]
- [Hacking with Swift’s article on blur & vibrancy]
