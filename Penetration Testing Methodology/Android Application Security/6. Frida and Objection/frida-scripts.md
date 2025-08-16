# Frida Scripts

#### `Java.use` - A JavaScript wrapper for Java classes

* It also allows us to instantiate instances of that class.
* Example :-
  * Here using the String class of Java we assigned a string (instance of that class) using constructor `$new` and then normally use any Java functions to that variable (si).
  * Normally we will not use $dispose but if we use make sure to not call that var that has been disposed otherwise it will create a very long error.

```bash
[Android Emulator 5554::FridaTarget ]-> var sc = Java.use("java.lang.String")
[Android Emulator 5554::FridaTarget ]-> sc
"<class: java.lang.String>"
[Android Emulator 5554::FridaTarget ]-> var si = sc.$new("$new is the frida way to call a new constructor and this is a
n insatance of previous class")
[Android Emulator 5554::FridaTarget ]-> si
"<instance: java.lang.String>"
[Android Emulator 5554::FridaTarget ]-> si.toStri
[Android Emulator 5554::FridaTarget ]-> si.toString()
"$new is the frida way to call a new constructor and this is an insatance of previous class"
[Android Emulator 5554::FridaTarget ]-> si.charAt(5)
"i"
[Android Emulator 5554::FridaTarget ]->si.$dispose()
```

#### `Java.enumerateLoadedClasses(callbacks)` and `Java.enumerateLoadedClassesSync()` - To list all available Java classes

* `Java.enumerateLoadedClasses(callbacks)` : returns callback for each loaded classes
* `Java.enumerateLoadedClassesSync()` : returns a list of all loaded classes (in an array)

#### Changing implementation of a class

* Can be used to change the original implementation and return what we want.
* useful when working with native libraries.

```bash
string_class.charAt.implementation = (c) => {
    console.log("charAt overridden!");
    return "X";
}
```

* simple example to change the implementation of charAt.

#### `Java.perform(() ⇒ {//my custom code here})`

* while writing a frida script make sure to wrap it in `Java.perform(() ⇒ {//my custom code here})` to make sure it runs inside java vm where all the classes have already been loaded and easily accessible.
* Example code to run a function/method from frida (frida target hextree example)

```jsx
Java.perform(() => {
    let FlagClass = Java.use("io.hextree.fridatarget.FlagClass");
    let flagclassinstance = FlagClass.$new();
    console.log(flagclassinstance.flagFromStaticMethod());
    console.log(flagclassinstance.flagFromInstanceMethod());
    console.log(flagclassinstance.flagIfYouCallMeWithSesame("sesame"));
})
```

# Tracing Activities

* For tracing activities we also need to understand the activity lifecycle which is nicely documented here [**Android Activity Documentation**](https://developer.android.com/reference/android/app/Activity)**.**

![Android Activity Lifecycle](https://developer.android.com/images/activity_lifecycle.png)

* This image provides an brief overview of android activity lifecycle.
* **`onCreate()`** - Called when the activity is first created.
  * Provides Bundle containing activity’s previous frozen state ,if there was one.
  * Always followed by **`onStart()`**
* **`onRestart()`** - Called after the activity was stopped and before stating again
  * Always followed by **`onStart()`**
* **`onStart()`** - Called when activity is becoming visible to users.
  * Followed by **`onResume()`** (if activity comes to the foreground) or **`onStop()`** (If it becomes hidden)
* **`onResume()`** - Called when the activity interacts with the user (i.e. activity is at the top and running).
  * Always followed by **`onPause()`** .
  * This is best to hook if we want to know which activity is at the top currently.
* **`onPause()`** - Called when activity is no longer in foreground. (before transition to stopped/hidden or destroyed).
  * Next activity will not be resumed until this method returns.
  * Followed by **`onResume()`** (if activity returns back to front) or **`onStop()`** (if it becomes invisible to users)
* **`onStop()`** - Called when activity is no longer visible to user. (either another activity is being brought on front so previous one stops or the activity is being destroyed).
  * Followed by either **`onRestart()`** (if this activity is coming back to interact with the user or **`onDestroy()`** (If this activity is going away)).
* **`onDestroy()`** - Called before the activity is fully destroyed
  * This can happen either because the activity finished or the system closed the activity to save space (or due to some other reason).
    * To distinguish b/w these two scenarios we can use **`isFinishing()`** method. (this method return a boolean value)
* Frida Script to do so:-

```jsx
Java.perform(() => {
    let activityclass = Java.use("android.app.Activity");
    activityclass.onResume.implementation = function() {
        console.log("Activity resumed: " + this.getClass().getName());
        this.onResume();
    }
})
```

***

# Tracing Fragments

* Same as Tracing Activities just change the package name to use

```jsx
Java.perform(() => {
    let fragmentclass = Java.use("androidx.fragment.app.Fragment");
    fragmentclass.onResume.implementation = function(){
        console.log("Fragment resumed: " + this.getClass().getName());
        this.onResume();
    }
})
```

***

# Intercepting Arguments and Return values

* Interception Arguments and return values is not that much different from the actual logging thing.
* Example Snippet:-

```jsx
Java.perform(() => {
    let InterceptionFragment = Java.use("io.hextree.fridatarget.ui.InterceptionFragment");
    InterceptionFragment.function_to_intercept.implementation = function (argument){
        console.log("Original Argument: " + argument);
        
        // return this.function_to_intercept("i want to pass this")
        // or 
        return "ye bhi chalega";
        
    };
    let LicenseManager = Java.use("io.hextree.fridatarget.LicenseManager");
    LicenseManager.isLicenseValid.implementation = function(){
        return true;
    };
    LicenseManager.isLicenseStillValid.implementation = function (context, unixTimestamp) {
        console.log(`LicenseManager.isLicenseStillValid is called: context=${context}, unixTimestamp=${unixTimestamp}`);
        unixTimestamp = 1672531260
        this.isLicenseStillValid(context, unixTimestamp);
    };
})
```

***
