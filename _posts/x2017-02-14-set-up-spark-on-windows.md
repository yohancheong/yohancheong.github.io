---
layout: post
title: Set Up Spark on Windows
---

You just need to point IntelliJ to wherever Spark is located which you can do by adding them as External Libraries, to do this, 

1. go to the mynewproject folder we created in the video 

2. Right Click on it and go to Module Settings (Shortcut for this is just F4)

3. On the left had side panel under Project Settings click on Libraries

4. Click on the plu sign to add a new library and then select Scala SDK and then choose Browse

5. Go to where Spark is installed on your Computer (it should be under C://Spark if you installed it on Windows, or under a hidden folder like bin/Cellar for a MAC Homebrew install)

6. Then go to Spark/jars/ and select all the .jar files there (you don't actually have to pick them all, its just easier then manually selecting only the ones you need)

7. Then hit Ok and Apply these new external libraries

8. You should now be able to write out Spark libraries on IntelliJ, keep in mind though, using Spark with Scala Worksheets instead of just s normal Scala script can be cumbersome/buggy because of the restriction of being able to only have one Spark Context open at a time, and the interactive nature of the worksheet may constantly be calling new SparkContexts over and over again leading to error notices.

Here is a video on Youtube that actually walks through this (but note he uses Maven):

https://www.youtube.com/watch?v=GGf6OqjaGw4



http://stackoverflow.com/questions/38127062/java-lang-nosuchmethoderror-scala-predef-arrowassocljava-lang-objectljava-l