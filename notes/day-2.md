# app.js conf - Day II

## The Hidden Features from V8 to Boost Your React Native Apps
Kudo Chien - Expo ([@kudochien](https://twitter.com/kudochien))

* Worked on RN / Chromium / Expo / react-native-skia 🎨
* Different JS engines on RN: v8, JSC, Hermes
* How was RN v8 born?
	* cxxmodule, clang / folly, jsc-android - Android crash which couldn't be reproduced so acted as motivator to develop a new JS engine -> v8
* Microsoft RN fork - added support for v8
* To install
	* react-native-v8 - JSI runtime implementation
	* v8-android - v8 Engine
	* v8-ios - no JIT, no WebAssembly
* Lots of config for vanilla RN projects, whereas Expo projects can just use `expo install`
* Features
	* Remote debugging in Flipper
	* Expo project - press j for opening JS inspector
* Hermes
	* bytecode is not true security - can still be read as plaintext
* Benchmarking
	* react-native-js-benchmark
	* Focus on RN only
	* RenderComponentThroughput
		* v8-android-jit (optimal)
	* Memory - adb shell dumpsys meminfo
	* TTI - time to interactive
		* Use different bundle sizes 3 Mb / 10Mb / 15Mb
	* apk size
		* v8 is larger than hermes (everything comes with a cost)
* Snapshot
	* Look at .bin files
	* Can see the built in modules eg. regexp / bigint etc.
	* Custom snapshot visualisation moment.js
	* Snapshot unavailable to RN unfortunately
	* Solution: Bytecode caching
* Bytecode caching
	* Build time - generate bytecode cache & bundle
	* Even faster than Hermes
* Negatives
	* Incomplete cross toolchain
	* Bundle size
	* Stacktrace - JS exception + stack trace 
* Conclusion
	* JSC selection
		* Hermes is good when considering metrics
		* Use v8 if you need
			* WebAssembly
			* High JS computation (animation worklets - reanimated)
		* If using v8 go for v8-android-jit / v8-android-jit-nointl

---

## Building Cross-Platform Apps with React Native + Next.js
Fernando Rojo - BeatGig ([@fernandotherojo](https://twitter.com/fernandotherojo))

* Frontend & web built by single dev
* dripsy / moti / solito
* How to handle navigation?
	* Expo + next router + react navigation
	* Difference in abstractions between screens & urls
	* Web + Native have different patterns
	* Too many blurred lines in traditional cross platform - web should look like web and apps should feel like apps 💯
* solito
	* web: react router
	* mobile: react navigation - linking config (handled via flag)
	* Same API as Next.JS (<Link />)
	* router.push('/screen-name')
* Give it a URL and it will handle the rest
	* documenting the navigation patterns
	* useParam - navigation params
	* useRouter - manual navigation
* `npx create-solito-app`
	* Tailwind CSS template is on the roadmap
* App example
	* Platform specific layout
	* Extracting from urls rather than pre-built UI
	* Take a look at the solito library code
	* Focus on trying to find the right abstraction
* Menus (Zeego release)
	* Native have a lot of manual config and then animations + support for Android back button
	* Radix UI as base
	* Use native components for menu components
	* zeego - menus for RN
	*  [https://github.com/nandorojo/zeego](https://github.com/nandorojo/zeego) 

---

## Controlling hardware in a week with Expo and EAS
Derek Stavis - BREX ([@derekstavis](https://twitter.com/derekstavis))

* Bike - Sur-Ron
* Modding to increase speed / torque
* The controller - ASII BAC8000
* Challenges
	* Creating a console / dial
	* Lack of public / consumer grade app
	* Business app not available for consumer
* Solution
	* Build own app - Kilowatt 1.0
	* Simple setup, dashboard, ride tracking
* PoC
	* Modbus - understanding registers
	* Apktool - reverse engineering apps - decompile
		* XMLs, bundle & info
		* decompile .dlls (Xamarin)
	* xamarin-decompress
	* Verify using unit-tests to verify spec
	* Blutetooth
		* react-native-ble-plx
		* NativeModules
		* EAS - config-plugins
		* Use the dev client
		* Lists & connects
		* Don't always have controller - but `bleno` library can help mocking the controller
		*  [https://www.npmjs.com/package/bleno](https://www.npmjs.com/package/bleno) 
	* Mock UI wireframes
* Build app using EAS
* Data collection is heavy (bluetooth streams A LOT of data - the bridge is a bottleneck here)
* Charts are a challenge
	* No JS solution was appropriate
	* React Native Charts Wrapper
	* Add library, eas build with dev client
* SQLite became a bottleneck
	* 480k rows through the bridge
	* React Naive Quick SQLite - JSI based SQLite bindings
* Voice control
	* RN Siri Shortcut
	* Create custom shortcuts for the app
	* EAS build didn't work out of the box
	* Extra setup
		* Changes to AppDelegate
		* Didn't want to do that - we need a config plugin! ✨
* What's next
	* Sentry used to track errors
	* Pipe console logs into datadog 🐛
	* Beta helpful to refine UX

---

## Continuous monitoring app's performance, made simple
Michał Pierzchała - Callstack ([@thymikee](https://twitter.com/thymikee))

* Entropy - things will fall apart when unattended
* No order without energy
* Software development cycle
	* Add new features, add tests, QA, Release
	* Bug - Identify root cause - add regression test, send to QA and then ship fix
* 1 star review appears
	* Performance regressions
	* Add regression tests
	* Lengthy lists being slow / flicker
	* Rendering images
	* Using heavy SVG images etc
	* ~80% of performance issues origin from the JS thread
* Specification
	* Integrates with existing JS testing libraries
	* New library: reassure for performance testing
	*  [https://www.npmjs.com/package/reassure](https://www.npmjs.com/package/reassure) 
	* Run on remote server image
	* Integrate with GitHub
	* Compares feature branch to main
	* Challenges of creating a performance benchmark
		* Stability / Flakiness
		* JIT / Garbage collection / Module resolution caching
	* Statistical analysis
		* Repetition and average / deviation
		* Probability of normal distribution
	* Module resolution caching
		* Node.js caches required / imported modules
		* Single time operation
		* Performance comparison report
	* What we learned 
		* Cover the most important user scenarios
		* Test whole screens or sequences
		* Component level tests are possible but require more runs
	* End to end / performance / integration / unit

---

## React Native Everywhere!
Taz Singh - Guild ([@tazsingh](https://twitter.com/tazsingh))
* Toronto.JS founder
* Communities are important
* Common code for every platform
	* Platform spec implementations for native APIs
	* Fast moving start-up - most impact via code-sharing
* Create a React Native Design System (Mondrian)
	* No CSS
	* Doesn't exist in RN
	* ReactUI - set of components to start your application
	* Key is the gap prop
	* NativeBase - found out about this after creating Mondrian 😅
* Mobile & Desktop and Web design patterns
* Packages
	* react-navigation - roadmap for web
	* react-native-url-router
		* didn't work on web at first
* We're only as good as the community around us

---

## Using your phone to make an actual call - videoconferencing in React Native with Membrane
Mateusz Front - Software Mansion ([@uusszz](https://twitter.com/uusszz))

* Spec - video chat app
* WebRTC
	* real-time streaming media
	* Backed by Google
	* Advantages
		* standard for communication in live-streaming
		* Good quality even under poor network conditions
	* Disadvantages
		* Quality and APIs of media servers
		* RFCs are pretty dry to read
* Membrane
	* Versatile multimedia framework
	* Targeted for live-streaming
	* Developer friendly, high-level APIs
	* Customisable & Fault tolerant - create plugins for library
* react-native-membrane-webrtc
	*  [https://github.com/membraneframework/react-native-membrane-webrtc](https://github.com/membraneframework/react-native-membrane-webrtc) 
	* more info:  [https://membrane.stream](https://membrane.stream) 
* Live Demo - Live video chat app

---

## Scaling the mobile app
Jessica Lo & George Xu - Chime

* Financial industries
* Availability - access information & resources when required
	* More features - code complexity increases
	* More members - increased system load
	* Decreased availability 📉
* Checking account screen
	* Multiple rest endpoints to fetch data
	* Overloaded system resources with large user bases
	* Scaling features and user bases is a challenge
* Evaluate request pattern
	* Widgets - fetches the UI elements to be shown
	* Once response ready - each widget performs a separate fetch
	* Unnecessary volume of requests
	* GraphQL can help simplify complexity and reduce bottleneck
	* Only the required information is returned
* Feature isolation
	* Ensure graceful error handling
	* Critical and less critical features isolated
	* Ensures that the MVP features are functioning, even if other requests are failing
	* Server-side traffic prioritisation can be designed to fail serving non-critical requests under high load
* Prolonged outages
	* Lite mode
		* Generic error messages are not to be used
		* A good error message communicates and reassures users and keeps user's trust
		* Server-Side fallback in separate remote location for delivering dynamic error messages even when main server is down
		* As backend recovers, slowly reintroduce connection to users (slider to slowly bump)
* Takeaways
	* GraphQL - simplify client & bundle requests
	* Feature isolation - protect critical features and prioritise requests
	* Lite mode - to gracefully handle outages

---

## Migrating to Fabric
Tomasz Zawadzki - Software Mansion ([@tomekzaw](https://twitter.com/tomekzaw))

Krzysztof Piaskowy - Software Mansion ([@piaskowyk](https://twitter.com/piaskowyk))

* React Native architecture
* New Arch
	* NativeModules -> TurboModules
	* Paper -> Fabric
* Check if Fabric is enabled
	* iOS - pod install flag
	* Android - boolean flag in gradle.properties
* Missing NDK error (Android)
	* Native Development Kit
	* RN has a common codebase in C++ from v0.68+
	* Need to install NDK (24+ for Mac ARM) dep in Android Studio
* Unimplemented component error shown for unsupported Fabric component
* Migrating native components
	* Codegen - define interface in spec to generate native files
	* Name of spec file - suffix is important: FooBarNativeComponent
	* Codegen ensures type-safety between JS and Native (numbers, strings, arrays etc.)
	* `codegenNativeComponent` function
* TurboModules
	* Old architecture - serialisation to JSON and sent to bridge to native side - asynchronous send / receive, slow
	* JSI function calls are not serialised so can be executed synchronously
* Migration stories
	* 2021 start migrating Software Mansion libraries
	* Shopify support in migrating
* react-native-screens
	* Support both archs with same codebase
	* Eliminate code deprecation
	* iOS - use ifdefs
	* Android - folder structure
	* Autolinking on Android does not exist yet - need to manually link each library (on RN roadmap)
* react-native-gesture-handler
* react-native-reanimated
	* Heavy react-renderer integration
	* Shadow tree behaviour update from v0.68
* Benefits
	* No more bridge
	* Less delays
	* type safety (codegen)
	* Lower memory usage

---

## Expo at scale in fast paced startup; Build, Deploy, Publish
Kelvin Onkundi Ndemo - Twiga Foods ([@NdemoKelvin](https://twitter.com/NdemoKelvin))

* Why use Expo at a startup
	* Minimal setup
	* Good for first contact with RN (bootstrap)
	* Developer ecosystem
	* CI/CD platform for building application
	* OTA instantly without having to wait for lengthy review processes
* Integrated access to Native API via expo packages (eg. expo-camera)
* Expo Application Services
	* Cloud services, build, submit and update
	* Share builds internally via build profiles / expo dev client

---

## The only in-app notification library that you will ever need
Magdalena Jaśkowska - The Widlarz Group ([@mjaskowska](https://github.com/mjaskowska))

* react-native-notificated
*  [https://github.com/TheWidlarzGroup/react-native-notificated](https://github.com/TheWidlarzGroup/react-native-notificated) 
* Really useful library for displaying in-app notifications
* Live Demo
* Global config for different notification types - createNotifications
* Imperative API for firing notifications with configurable styling options
* Works out of the box 💪

---

## Infiltrate your own Expo / React Native app
Wouter van den Broek ([@wbroek](https://twitter.com/wbroek))

* Apps can be a vulnerability - malicious actors can gain info from your application bundle
* 7-eleven - password reset 
* Fake apps try to exploit list of contacts
* Supply chain attacks - npm packages / IDE, plugins / deps
	* Tweak the library to transmit data to a malicious server
* What can we do as devs?
	* Know your service
	* React Native / Android / iOS vulnerabilities
	* Platform security docs
	* Julia ( [@julepka](https://twitter.com/julepka) ) - React native security / Cossack Labs
* Owasp - top ten mobile security vulnerabilites
	*  [https://owasp.org/www-project-mobile-top-10/](https://owasp.org/www-project-mobile-top-10/) 
* How easy is it to tamper with an RN app?
	* react-native init
		*  [https://github.com/GantMan/jail-monkey](https://github.com/GantMan/jail-monkey) 
	* Relatively simple to edit and update JSBundle
	* Hopper disassembler 
		*  [https://www.hopperapp.com](https://www.hopperapp.com) 
	* Modify code 
	* jadx-gui
		*  [https://github.com/skylot/jadx](https://github.com/skylot/jadx) 
	* .env files are easy to look up in decompiler
	* Change url for OTA update
