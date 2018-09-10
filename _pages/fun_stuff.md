---
permalink: /fun_stuff/
title: "Fun Stuff"
author_profile: true

---


## Podcast summary & recommendation

### SE Daily - Video Search with Rasty Turek

Video search is essential to track all the places on the internet where a particular content is present. A youtube video which might be downloaded and put on a lot of other platforms. The task to track the content's(or part of the content) presence on all the different places is a daunting task. The episode discusses how the company Pex solves the problem. This is useful in various scenarios.
* A copyrighted video which might be copied and put into different platforms.
* A content on a platform which might be unavailable in certain countries(like youtube) might have been copied and put in a different platform. 
* To track the activity of how a leaked video gained popularity. 
* To track any remaining copies of content of a militant/terrorist organization.
* A creator can track her/his popularity by not just looking at one platform but on all the places where the content might be present and the type of audience the content is serving. 

Pex uses a crawler and indexes all the videos on the internet. It uses a propreitary algorithm to generate a unique signature for each video which can then be compared with the signature of another video. Once you have a database of all the signatures, given a video, a signature is generated and a comparision is done on the exisiting signatures to see if the video or part of the video is present anywhere else.

This means the index is to be built all the time as new content is generated constantly throughout the globe. In order to check if a particular content is already indexed and that content has not changed, simple techniques like checking the video length and verifying the hash of first few bytes of the video are used. Other challenges include distorted versions of the video like rotation, periodic skipping of frames, etc. The signature generation has to be complex enough to resilient to all sorts of content distortion. 

The crawling has to be fast enough to index the content being produced at a dizzying rate to enable the search to be as real time as possible but also to not be overloading the servers. The requests are cached to make sure crawling isn't done if a page doesn't change as often. 

The episode contains much more exiting and detalied explanation of how it is done including how PostGres is used at Pex.


## Books I recommend

- Superforecasting by Phillip. E. Tetlock
- Steve Jobs by Walter Issacson
- Rosie Project by Greame Simison
- The Wright Brothers by David McCullough
- Pygmalion by George Bernard Shaw
- The Animal Farm by George Orwell
- David & Goliath by Malcom Gladwell

## Podcasts I like

* Software Engineering Daily
* Longform
* Our last Week
* Freakonomics
* The Intersection
* Stay Tuned by Preet Bharara


