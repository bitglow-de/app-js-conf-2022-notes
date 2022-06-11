# app.js conf - Day I

## Keynote and Expo announcements
Charlie Cheever - Expo
* Zoe app - started off as a personal nutrition tracker
	* Changed over to covid symptom tracker which was used by a lot of people in the UK
* OTAs via expo-updates
* Ejecting and adding NativeModules was a pain point that expo focused on to improve
	* Expo Dev Client - Like Expo Go for your app
	* Out of beta and ready to use 🎉
	* EAS Build - cloud solution for dev, test & submission - this made the difference for us and sped up our dev flow 🚀
	* Roadmap - Expo as a platform
	* Config plugins - automated spec of a NativeModule
		* Really easy integration with Expo 
		* No more ejecting 💪
* Using the Internet -> Large increase in Mobile usage
	* 90% of time spent in apps and 10% on websites
	* Distribution is a headache - Expo is looking to improve these issues
	* EAS Update - enterprise grade rather than expo-updates. CD iterations 
	* EAS Submit - submission via Google Play & App Store
	* EAS Metadata - submission forms of app stores and making the process easier (e.g. does your app contain mild violence etc.?) - this is going to be a game changer since you will be able to completely submit an app via the CLI
	* Web  - getting a website with the same features. An app should always consider universal availability - iOS, Android + Web
		* solito - React Native & Next.JS

Evan Bacon - Expo ([@Baconbrix](https://twitter.com/Baconbrix))
* Deployment - EAS Build
	* Get to processing in the stores
	* Can't submit app until the metadata is filled out
	* `eas metadata` 📈
	* Automate entire submission process from CLI
	* Define in a new file: `store.config.json`
	* Spec can also be generated based on an existing apps store metadata ♥️
	* Interface helps to also validate and identify language Apple doesn't like in the descriptions
	* App store default: deploy to local store
	* EAS Metada - auto changelogs, white labels and global deployment
	* Currently in beta release
* Development - Expo CLI
	* The dialog is going away with update your cli
	* New: npx create-expo-app 
	* Much faster and easier to setup an app
	* New CLI: npx expo to replace expo-cli
	* Local install in node_modules rather than global cli on machine + ️
* Advanced bundling
	* Universal bundling from local server
	* Exotic mode is quicker for JS bundling
* React Suspense for native development
	* lazy and suspense
	* Bundle when component loaded
* Eject is gone and replaced with prebuild
	* Convert into native apps
* Config plugins
	* Adopted spec for customising prebuild
	* Detox can be hooked in using @config-plugins/detox 😱
* Prebuild & EAS build
	* Apple auth is much easier and performed using the CLI - devs can focus on development

Tomasz Sapeta - Software Mansion ([@tsapeta](https://twitter.com/tsapeta))

Expo Modules
* Copying code into AppDelegate is a common use-case
* Hooks have been built to hook into the Native Lifecycle methods (extensions)
* ExpoAppDelegateSubscriber 🍎
* ReactActivityLifecycleListener 🤖
* Bridge modules - Asynchronous, perf expensive, serializes in JSON
* Solved by Turbo Modules - faster, sync but C++ increases build time
* Expo modules - designed for Swift & Kotlin, use JSI directly, cross-platform and easy & safe

Sweet API (Software Mansion) - similar declarative language on both platforms
* Convertibles - standard platform classes (e.g. URL)
* Records - Typesafe declarations of arguments
* Native classes 
* Typed arrays - not supported out of the box but could be useful for some niche use-cases e.g. audio processing (Expo Modules is working on support)
* RC coming up
* npm create expo.module

---

## Animations should be fun
Catalin Miron ([@mironcatalin](https://twitter.com/mironcatalin))

* reanimated 3 primitives
	* useSharedValue - drive animations and react to data
	* Run on UI but can be accessed from JS (hence shared)
	* useDerivedValue - derived from shared value (eg. +100)
	* useAnimatedStyle - reactive style (dynamic) based on shared value
	* useAnimatedProps - dynamic non-style props 
	* interpolators - to map changing values to other values
	* timings / physics
	* useAnimatedScrollHandler - scroll callbacks
	* useAnimatedReaction - useful for set state and reacting to changes of shared values
* Interpolate
	* Pass shared value - and range to map changes onto range
	* interpolate in the range
	* extrapolate is about defining behaviour outside of the defined range
	* Use a graphical representation to aid understanding
* The problem with index - items in a carousel [i-1], [i], [i+1]
	* useAnimatedScrollHandler and sharedValue
	* Need index and scroll position
	* (index - 1) * width, index * width, (index + 1) * width
	* Item coming from left, item in middle, item coming from right
	* extrapolate [0,1,0]
* sticky - the interpolation exam
	* sticky after - in list and sticks
	* sticky before - initially sticky moves up
	* sticky bottom - stays at bottom of screen
	* useAnimatedScrollHandler
* fluidity
	* math - atan2
	* withSpring to delay with natural behaviours
* Layout animations
	* Mounting / unmounting
		* Entering / exiting - ie how does the component appear and disappear
	* Layout modifiers
* useInterpolated
* useLayout hook
* CellRendererComponent (FlatList xy is always 0 but this gets access to screen y position)
* moti
	* Animate styles, properties easily
	* Based on reanimated

---

## The state of one codebase for all the platforms
Sanket Sahu - GeekyAnts ([@sanketsahu](https://twitter.com/sanketsahu))
* Approach
	* use native components and bridge to call across
		* No need to reimplement platform level UI patterns
		* Mapping every component for every target platform
* UI Patterns differs on different platforms (eg. Web / Android / iOS)
* Navigation is difficult
	* Mobile: Stacks - preserves state when going back
* Accessibility
	* ScreenReaders
* Animations & Gestures
* Mobile patterns - native APIs eg. camera
* UI Libraries
	* Showtime (universal)
	* Dripsy (styling on RN) - style once, run anywhere
	* Tamagui - universal RN+web
	* NativeBase - universal RN+web
* Navigation
	* React Router - Web
	* Next.js - file based routing
	* React Navigation 
	* React native url router
	* Solito - RN + Next.js - shared Link component
* Accessibility
	* React Aria (Adobe) - Accessibility built in
	* React Native Aria (GeekyAnts) - Accessible components for RN
* Animations & Gestures
	* framer-motion - declarative but only on web
	* reanimated - supports Fabric for native animations
	* moti
* NativeModules
	* expo modules - roadmap to include web support
* Build systems
	* EAS build - currently mobile only 
	* solito - EAS / Vercel build pipelines
* Many platform vision
	* Embracing platform constraints
	* Competition drives innovation (RN + Flutter)
	* Expanding to new platforms

---

## Publish updates with Expo and React Native
Quinlan Jung - Expo ([@quinlanjung](https://twitter.com/quinlanjung))

* Big corp is trying to make their own app for employee management
* How to roll out to the end users?
	* expo publish
	* eas update - works with all react native apps
		* --branch name
		* --message human readable message
	* How are updates deployed to users
		* expo-updates asks server for latest update
* Policies
	* Default: Query update on splash screen in background
		* Show old app
		* Next time the user opens app - the new bundle is loaded
		* Minimises TTI - Time to interactive 
		* Disadvantage: Takes longer until user can start using update
	* fallbackToCacheTimeout
		* Use it to show updated app if bundle is downloaded within specified time
		* Bad network conditions: The update is shown on next launch
		* optimal conditions - the update is loaded and shown immediately
	* Manual policies to be configured
* What is an update?
	* Update layer - JS / images etc. - non-native code that can be loaded
	* Native layer - cannot change with update - need to create a new binary
* Location tracking update
	* Download new location package
	* New native deps, which means it cannot be OTA'd
	* New app pushed to store with new dependencies
	* Design team: Make an update to UI (green dot) to be pushed by eas update
	* Users on original app query server for update (but from a binary without the location native deps)
	* App cannot be used so recover from error using previous bundle
	* Takeaway: Not all updates are compatible with all runtimes
	* Ask server for update compatible with certain runtime
	* How to visualise runtimes - use the deployments UI
		* Channel: production with runtime version 2.0.0. Breaking changes are then appropriately handled and avoided
* How is it on web
	* Request JS - delivered from server
	* New browser runtime, the Chrome servers will push this update (happens rarely)
	* Mobile: JS changes - delivered often
		* Native changes - rarely updated
* Auto resignation
	* Only JS / TS updates
	* Employees unhappy - how do we revert this
	* Redeploy previous update: --republish with --groupId

---

## Measuring and improving React Native performance
Alexandre Moureaux - BAM ([@almouro](https://twitter.com/almouro))

* 60FPS - The grail of performance
* JS thread is important
	* If it's blocked then you can't interact with the app
* Performance Flipper plugin
* Performance measures
	* Test on a lower-end device - Samsung Galaxy A21s is 10x slower than iPhone 13 Pro
	* Make measures deterministic - average measures over several iterations, same conditions, automate the behaviour for consistency
		* adb shell input swipe for controlling Android
	* Disable JS Dev to minimise difference between prod and dev
	* Use the best analysis tools
		* JS Thread - React DevTools / JS Flamegraph
		* UI Thread - Android (systrace), iOS (Xcode instruments)
* App: TF1 info which is a news app written in RN
	* Goal: Low-end device, JS > 0 FPS, UI = 60 FPS
	* Samsung J3 2017
	* Setup measurements for 10s
	* Reload with dev mode disabled
	* Wait for feed to load
	* Hit start measure
	* Use adb shell swipe to simulate scrolling in FlatList
	* Record in performance monitoring (Flipper)
	* React DevTools (Flipper) - profiler record why each component re-rendered
* Browsing commits (Flamegraph)
	* Try to find the most expensive commits
	* React tree of components in the apps and can debug each component commits
	* orange - components with high self-time
	* CellRendererKeys
	* 2 metrics - 4.9ms (initial render without children) of 2999.2 ms (render with children)
	* Check the initial render and inspect the debug message panel - is it a re-render?
	* List children are re-rendering unnecessarily 
	* Virtualisation helps to only render the items that will appear on the screen lazily
	* By design - renderItem is being called
	* The list items should be memoised!
	* Slight improvement, but there is still some way to go
	* Horizontal lists are re-rendering because of context change
	* Scroll the parent list, the render item is also called
	* Loop property on carousel also adds 3 slides to each side
* Non-intuitive fix
	* 4 slides of carousel
	* List is taking a long time to render children
	* react-native-snap-carousel
	* Nesting virtualised list is tricky
* What's next
	* Automated measures
	* Hopefully lighthouse for mobile

---

## Bringing the New React Native Architecture to the OSS community
Nicola Corti - Meta ([@cortinico](https://twitter.com/cortinico))

* 2018 talk on the new architecture
* Edge cases in Facebook codebase caused the migration to the new architecture to take longer (2021)
* Open source community is next up on the roadmap
* Why
	* Removal of the bridge
	* Shared C++ implementation
	* Share platform specific optimizations
	* Improved type safety
	* New capabilities
* Pillars
	* Fabric - new renderer
	* TurboModules - new native module implementation
	* Codegen
	* Removal of the bridge
* Codegen
	* Creation of a spec which can be used to generate platform specific code
* Build tools
	* Gradle - compile C++ code - (.mk files) 
	* Java / Kt - react native gradle plugin
	* Will replace react.gradle plugin (deprecation)
	* Cocoapods - custom logic in Ruby
* Hermes
	* Recommended engine for the new architecture
	* Roadmap: Hermes as a default engine
	* v0.69 - bundled version of Hermes
		* building a version of Hermes alongside RN each time
		* This means that the build process can take much longer initially
* Modern languages
	* TypeScript - now supported by the codegen (can also use flow)
	* Kotlin - Fully supported in RN
		* Roadmap: Migrate template project to Kotlin
	* Swift - Has been considered but not on the roadmap
* Timeline & Versions
	* v0.68 - first stable version with new architecture support
	* v0.69
		* React 18 support
		* Bundled Hermes
	* v0.70
		* Android autolinking for Fabric components
		* CMake support
		* Unified codegen config
* React 18 & React Native
	* v0.67 / v0.68 support React 17
	* v0.69+ support React 18
		* old architecture - no concurrent features (Fabric)
		* new architecture - concurrent enabled + ️
* Docs & Material
	* Not yet finished internal rollout
	* Whatever is released, it has already been stress tested at Facebook
	* Take a look at the new architecture section in the docs
	*  [https://reactnative.dev/architecture/overview](https://reactnative.dev/architecture/overview) 
* New App Template
	* RCT_NEW_ARCH_ENABLED=1 pod install
	* gradle.properties flag: newArchEnabled=true
	* Logs in metro will display if you are running Fabric enabled code
* App Sample Repo
	*  [https://github.com/react-native-community/RNNewArchitectureApp](https://github.com/react-native-community/RNNewArchitectureApp) 
* Library Sample Repo
	*  [https://github.com/react-native-community/RNNewArchitectureLibraries](https://github.com/react-native-community/RNNewArchitectureLibraries) 
* Working group:
	* reanimated
	* typescript template
	* gesture handler
	* Support the community 💪
* Blog idea: How to migrate your app/library to the new architecture

---

## Building for all and for us: how contributing to open source projects is helping Shopify and React Native build the future of Mobile
Monica Restrepo - Shopify ([@ImRestrepo](https://twitter.com/ImRestrepo))

* RN at shopify
	* 2015 - exploration in hackathons of RN (still native codebase)
	* 2016 - 2018 - Prototyping  and PoC and developing Arrive app (Shop app) 
	* 2019 - Point of Sale (PoS) 
* Contributions to community
	* Tackle issues with Framework and push the framework to higher levels
	* Self-responsibility for improving the platform as a whole
	* react-native-skia
		* Bringing this to RN
		* Tackle RNs limited graphical capabilites
	* Support Software Mansion open source projects
		* React Native Screens
		* Reanimated
	* Fast scrolling lists (FlashList)
		* No more blank cells
		* Instant performance
		* v0.6.1
	* Rapid and solid evolution to improve development experience
		* Detecting the weak points and working with the community to improve the platform
		* Make it better for the engineers (and community)
		* Invest in the platform
	* Pushing the development curve of React Native

---

## How to build a universal design system
Axel Delafosse ([@axeldelafosse](https://twitter.com/axeldelafosse))

* showtime-xyz design system
	*  [https://github.com/showtime-xyz/showtime-frontend](https://github.com/showtime-xyz/showtime-frontend) 
	* Used on the site  [https://showtime.xyz](https://showtime.xyz) 
* Why
	* Easier to maintain, universal components, create a visually pleasing app
	* HQ components, works with Expo & Next.JS
* How
	* Own design system built on universal, meta-framework
	* Universal is a superset of RN components
* Roadmap
	* v1 - react-native-web
		* work seamlessly in other apps
		* accessibility & docs
	* v2 - support from community & investors

---

## React Native at Traveloka
Vanya Deasy Safrina - Traveloka ([@vanydeasy](https://twitter.com/vanydeasy))

* lifestyle app - travel, local services in SE Asia
* Research phase / Initial products
* Growth and increase in investment in React Native
* Reusability, Faster time to market, reduced development bandwidth for a specific platform
* District system
	* core - Util / UI - common components / utils
	* libraries e.g. Auth / i18n / Form to be used on all apps / platforms
	* B2B apps / tools - B2C mobile app
* Traveloka app
	* Brownfield approach
	* android / ios folders
	* shared - shareable specific building blocks to other units
		* mobile web version of same feature from mobile app
* Shareable code between platforms
	* advantages
		* Minimum maintenance cost
		* Better performance
	* disadvantages
		* increased development cost
		* platform implementation differences
* Is it possible to share UI components between Native & RN?
	* Can only render custom native with flex:1 or pre-defined dimensions
	* How can we render dynamic sized components?
		* View Manager to show component
		* Wrapper is responsible for observing size change
		* Override subviews to measure and report size back to JS
	* React Native UI components on native screens
		* Props to pass primitive values from JS to native
	* Native modules to pass callback functions from RN to native
		* Find the tag and notify
		* Use the delegate to report JS changes
	* Events to pass return value back to native
* Independent release
	* Flexibility to do CodePush release JS code at any time
	* Release independently
* Split bundle
	* Custom metro config
	* Product bundle + vendor bundle
* In-House CodePush
	* Instance for each bundle
	* Developed on native
* Arch improvements 
	* Maximise usability
	* Shorter time to market
* Independent release
* React Native & Native shared components
* Each product can manage independent release timeline

---

## Web3 for Mass Adoption with StarkNet and React Native

Sean Han - Playoasis([@0xs34n](https://twitter.com/0xs34n))

Janek Rahrt - Argent ([@0xjanek](https://twitter.com/0xjanek))

* Architecture of blockchain under the hood
* Argent-X - Wallet on StarkNet
* Why should I care about Web3
	* Sovereign ownership of digital assets
	* Permissionless
	* Secure - no point of failure
* Ownership
	* Cryptography - pub / private key
	* Blockchain - public address, private key to verify the owner
	* Value not owned by a company
	* Currency are not owned by government
* Permissionless
	* Anyone can fork
	* Participate in securing the network
* Security
	* Data breaches are not on large scale - individual basis
	* No single point of failure
	* More participants, more security
	* Same goals
* What are people doing with Web3
	* DeFi - Finance without Middle Men
	* NFTs - own a piece of the internet
* DeFi
	* Transaction with programs on the blockchain
	* No middle-men: No banks and clearing houses required to facilitate transfers
	* Removes need for brokerage / loans without bank - individualism
	* Secured by open code
	* Money becomes programmable
* NFT
	* Digital Artists can earn income independently
	* Create value across separate ecosystems
	* Culture becomes tokenized
	* Crypto proof of owner
	* Royalties - traceability
* Some challenges
	* Trilemma - scalable / decentralised / secure (can only have 2 at once)
	* Traditional chains: Decentralized + secure
	* High-TPS chains - scalable + secure
* StarkNet solves scalability
	* Batching transactions into single proof
	* Acts as interface between end users / devs and layer 1 chain (eg. ethereum)
	* Large savings with transaction fees
* What is StarkNet JS
	* Allows apps to connect to network
	* Works nicely with React Native
	*  [https://github.com/argentlabs/starknet.js](https://github.com/argentlabs/starknet.js) 


