# getting_started
This document is intended to help you get started with a new Unity project featuring VEL components for Quest 3.  

* Start a blank Unity 3D project
	* add a proper gitignore file to the folder (and git init)
	
* In build settings
	* Switch to Android
	* Turn on development build and script debugging (You'll want to disable this later)
	
* In player settings
	* Change your company name
	* Change your product name
	* Override the package name edu.uga.engr.vel.NAME
	* uncheck Auto Graphics API and take out Vulkan.  OpenGLES is slower, but much more compatible with things
	* Change to linear color space
	* Change to min api 32, target 32 
	* Change to IL2CPP
	* Change to .NET framework
	* Only check ARM64 
	* Change IL2CPP to faster builds
	
* In package manager settings 
	* add a new scoped registry (VEL, https://npm.ugavel.com, edu.uga)
	* add a new scoped registry (Meta, https://npm.developer.oculus.com, com.meta)
	
* In Quality settings
	* medium (for android, turn off shadows)

* In Package Manager (My Registries)
	* From Meta
		* Core
	* From VEL
		* VelUtils
		* VelUtils-OVR
		* VelNet
		* VEL-Connect
		* If you want webrtc input/output
			* Vel Share Unity (or 
			* On top of that, add a package from git url: https://github.com/endel/NativeWebSocket.git#upm 
* Back in project settings
	* Under XR Plugin Managent (install)
		* Enable Oculus
	* Under Meta XR
		* Fix all recommended and outstanding items
		
* Create a new, completely blank scene and delete the sample scene
* From Meta Core SDK prefabs, drop in the OVRCameraRig
	* Add a no-gravity rigid body to it (or with gravity if you prefer, with a floor collider)
	* In OVR Manager component
		* Set Tracking to floor level
		* Check Simultaneous Hands and Controllers 
		* Check wide motion hand poses
		* Under Quest Features 
			* Skip unneeded shaders
			* Hand Tracking Support (controllers and hands)
			* Hand Tracking Frequency Max
			* Hand Tracking Version 2.0
			* Anchor support enabled
			* Passthrough support required
		* Enable Passthrough
		* Launch simultaneous hands and controllers
	* Add the velutils Rig component
		* Set the head, left and right hand components to the ovrcamera rig center eye anchor, left hand anchor, right hand anchor
	* Add the velutils Movement component
		* Set the rig
		* Check the Set Physics Timestep to Refresh box
		* Enable grab movement for left/right (eventually delete this...)
	* Since we want hand tracking, add some OVR Custom Hand Prefabs to each of the two hand anchors
		* auto map bones in each, update root scale
		
* Add an ovr passthrough layer component
	* Placement Underlay

* On the center eye camera component
	* Change the clear flags to solid color
	* Change the color to clear black (#0000)
	
* Add a cube so you have something to look at
* Add a directional light (not 100% necessary)

* Plugin headset, verify that USB debugging is working (should show up in build settings run device) and Run 
* Build and Run, put in Builds folder (so gitignored) called main.apk
	* Note, first build takes quite a lot of time (mine was 431s), but subsequent builds can be very fast (My second was 36s, 3rd build 33s0)

* If you want Unity UI input
	* Add a canvas
		* Set to worldspace
		* Scale to .001,.001,.001
		* Add a button, import TMP
	* On the Event System Object
		* Delete Standalone Input Module (or uncheck it)
		* Add WorldMouseInputModule from velutils
	* On the left/right hand anchors, add a new game object called world mouse
		* On each of these, add a world mouse with laser component, and setup side to be left or right (with appropriate input setting)
		* change side to left/right respectively
		* Change laser to red
		* Under project settings/graphics, add an always included shader - Unlit/Color
