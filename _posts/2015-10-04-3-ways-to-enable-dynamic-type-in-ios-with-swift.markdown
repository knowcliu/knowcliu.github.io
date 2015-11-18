---
layout: post
title: "3 ways to enable Dynamic Type in iOS with Swift"
date: "2015-10-04 11:44:48 -0500"
---

Dynamic Type is the feature that enables the user to be in control of how large or small they want their font sizes. It is configured in Settings > General > Accessibility > Large Text

<img src='/assets/iphone_accessibility_settings.png' alt='iPhone Accessibility Settings' title='iPhone Accessibility Settings' class='center50Img'>

I will show you three ways to enable this great feature.

# 1. Storyboard
For all Labels, Buttons and Text in the storyboard, ensure the **Font style** is set to Text Styles rather than System Styles eg: Headline instead of System 16.0

<img src='/assets/attributes_inspector.png' alt='Attributes Inspector' title='Attributes Inspector' class='center50Img'>

<img src='/assets/attributes_inspector_font.png' alt='Attributes Inspector, Font pop-up' title='Attributes Inspector, Font pop-up' class='center50Img'>

<img src='/assets/attributes_inspector_text_styles.png' alt='Attributes Inspector, Text Styles, Headline' title='Attributes Inspector, Text Styles, Headline' class='center50Img'>

# 2. Preferred Font
For any text that is set programmatically, you can update the font property like so:
`someTextLabel.font = UIFont.preferredFontForTextStyle(UIFontTextStyleBody)`

# 3. NSNotification
You can do other things with your UI by observing the `UIContentSizeCategoryDidChangeNotification` notification.

    let center = NSNotificationCenter.defaultCenter()

    override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)

        center.addObserverForName(UIContentSizeCategoryDidChangeNotification,
            object: UIApplication.sharedApplication(),
            queue: NSOperationQueue.mainQueue()) { notification in
              let c = notification.userInfo?[UIContentSizeCategoryNewValueKey]
        }
    }

    override func viewDidDisappear(animated: Bool) {
        center.removeObserver(UIContentSizeCategoryDidChangeNotification)
    }


# Summary
I've demonstrated three ways you can build the Dynamic Type feature into your iOS apps. If you keep these tips in mind while you are developing your UI, you will have happy users. Enjoy!
