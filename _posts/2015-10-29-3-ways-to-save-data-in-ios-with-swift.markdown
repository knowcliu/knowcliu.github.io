---
layout: post
title: "3 Ways to Save Data in iOS with Swift"
date: "2015-10-29 12:29:15 -0500"
---

# 1. NSUserDefaults

`NSUserDefaults` is a very simple key-value store that is great for simple user preferences or other small text values. It can even be used as a mechanism to share data between apps using App Groups. The read and write to the store is atomic, which is great for small stores, but could be very slow for larger data sets.

Here is how it works:

    let defaults = NSUserDefaults.standardUserDefaults()
    // Write
    defaults.setObject("Coding Explorer", forKey: userNameKeyConstant)
    // Read
    if let name = defaults.stringForKey(userNameKeyConstant) { println(name) }

# 2. Document Directory

The Document Directory is a great way to acces the file system in your sandbox to store larger files like images. 

    func saveImageWithName(image: UIImage, imageName: String) -> String? {
        // Save the file to Documents and store that location in the DB
        var documentsDirectory: String?
        var paths: [AnyObject] = NSSearchPathForDirectoriesInDomains(NSSearchPathDirectory.DocumentDirectory, NSSearchPathDomainMask.UserDomainMask, true)
        
        let data = NSData(data: UIImageJPEGRepresentation(image, 1.0)!)
        if paths.count > 0 {
            documentsDirectory = paths[0] as? String
            let savePath = documentsDirectory! + "/" + imageName + ".jpg"
            NSFileManager.defaultManager().createFileAtPath(savePath, contents: data, attributes: nil)
            return savePath
        }
        return nil
    }


The first step is to get a list of paths that match the Document Directory:

    var paths: [AnyObject] = NSSearchPathForDirectoriesInDomains(NSSearchPathDirectory.DocumentDirectory, NSSearchPathDomainMask.UserDomainMask, true)

Then, you really only expect to get one result, so use the first item in the list to construct a full directory path for the file you wish to store:

    documentsDirectory = paths[0] as? String
    let savePath = documentsDirectory! + "/" + imageName + ".jpg"

Finally, save the file to the path:
    
    NSFileManager.defaultManager().createFileAtPath(savePath, contents: data, attributes: nil)

Once you have the file stored in the Document Directory, you are free to use it:

    UIImage(named: myFileLocation)

You can also delete it:

    do {
        try NSFileManager.defaultManager().removeItemAtPath(myFileLocation!)
    } catch {
        print("Failed to remove file from path: \(myFileLocation)")
    }

# 3. Core Data

Core Data is an object graph and persistance framework similar to an ORM, but not quite the same. It lives on top of SQLite and consists of Entities and Relationships. It is beneficial for storing more complex data and has built-in support for batch processing, cleaning dirty data, pre-fetching, model versioning and many other features. Here are a few steps to get started.

1. Start a new project and in the 'Choose options for your new project' window, select 'Use Core Data'
<img src='/assets/use_core_data.png' alt='New Project, Use Core Data' title='New Project, Use Core Data' class='center50Img'>
2. This will add a file called 'SomethingNew.xcdatamodeld' to your project where you can go to access the Core Data model.
<img src='/assets/core_data_model_display.png' alt='Core Data Model Display' title='Core Data Model Display' class='center50Img'>
3. Click on the Add Entity plus sign icon in the main area then go to the Data Model Inspector in the Utilities area to configure the new entity. You can Add Attributes and configure them as well. 

After the data model is designed, you can subclass `NSManagedObject` in a new class to get access the model.

# 4. SQLite

# 5. iCloud

# Summary
Ok, I sort of lied... there are way more than three ways to store data in iOS, but I've shown you the three most popular. Enjoy!
