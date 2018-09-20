---
title: 'Empirical Analysis of Play Store Applications'

permalink: /projects/playstoreanalysis
tags:
  - Android
  - Manifest files
---

## Goal

The goal of this project is to obtain large amount of Play Store applications and analyse patterns regarding how they are built. 
### 1. Manifest Files
Manifest file is usually like an index of a book. We can see how apps are structured, permissions the app requests from user, permissions other apps 
require to interact with this app. These files also tell us what components apps expose like services and activities.

### 2. UI Design XMLs
UI XML files tell us how the UI is built with minute details. That information could prove useful to mine patterns regrading how UI is built across categories, ratings and popularity etc.


## Tasks

### Building APK Repository

So we need ton of apk files from which we will have to extract manifest files and Design XMLs. Since there is no official API to download play store apps, we are using the tool [gplaycli](https://github.com/matlink/gplaycli) to download the apps. Also this means we need to keep all the apps updated.

### Manifest & Design file extraction (TO DO)

We intend to use apktool to extract the manifest and UI Design files

