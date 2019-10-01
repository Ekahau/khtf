# khtf
Umbrella project for multiproject kotlin template.


This project setup aim to simplify crossplatform development with Kotlin.

Sometimes there is need for multiple apps on multiple platforms with shared business logic.
Using modular architecture, it is possible to reuse most of the business logic.


What could be shared (some examples):

* algorithms
  * image processing
  * serialization to custom formats
* communication with backend (endpoints)
* collecting user analytics
* common UI
  * user feedback
  * news and announcements
  * support contact
  * company brand colors and fonts
* social sharing

This example is structured as independent repositories. When multiple teams develop multiple apps, it is important for them to rely on specific versions of functionality.
Each module could be build and released as new version independently from others.

All repositories here could be separated into 4 layers:

1. Libs with common code only. 
2. Libs with common and native code.
3. Framework repositories, that combine multiple kotlin libs into single framework. Only needed for iOS.
4. Applications (ios, android, backend, desktop, etc), that use libs.

![concept](docs/concept.png)


## List of sub projects

* AndroidApp - Android app, that uses kotlin libraries
  * https://github.com/ekahau/khtf-androidapp
* IosApp - iOS app that uses IosAppFw framework (via cocoapods) to import kotlin libraries
  * https://github.com/Ekahau/khtf-iosapp
* IosAppFw - combine multiple kotlin libraries into single framework
  * https://github.com/Ekahau/khtf-iosapp-fw
* IosAppFwFramework - repository for compiled framework artifact, that could be used in iOs app
  * https://github.com/Ekahau/khtf-iosapp-fw
* podspec - cocoapods repository (aka catalog of available frameworks and locations where they are stored)
  * https://github.com/Ekahau/khtf-podspec
* Libs:
  * A, B, C - simple example to show how nested libs could be accessed from ioas/anddroid apps
    * https://github.com/Ekahau/khtf-lib-a
    * https://github.com/Ekahau/khtf-lib-b
    * https://github.com/Ekahau/khtf-lib-c
  * D, F - 
  * H, G - 

## Limitations

### Resource export propagation (iOS)

Currently kotlin does not support export resources with klib. 

Some possible alternatives:

1. export resources in each library as separate jar package. In FW project, gradle script could go through them and add to result framework.

E.g. structure: 

```
/resources
  /liba
    /file-from-lib-a.txt
  /libb
    /file-from-lib-b.txt
  /libc
    /images
      /btn-save.png
      /btn-close.png
    /config.json
```

2. Get resources from corresponding jar files. May be not very trivial for multiple nested libraries.

3. Adjust how kotlin compile each lib, so resources are also exported.
 
### Only one kotlin framework is allowed (iOS)

If multiple kotlin frameworks added to ios project, then app will fail to start.

Workaround: 

Create extra repository, that could compile all dependent libs into single framework.

Note: If multiple libs expose Class with same name, then compiled names will only differ with '\_' character.  E.g. "MyKotlinClass" and "MyKotlinClass_". It may become hard to distinguish them in ios code.

 
