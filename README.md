NahamCon CTF 2023 - “Red Light Green Light” - Category: AndroidDuring my participation in the Nahamcon CTF, a challenge involving Android app caught my attention. The primary goal of this write-up is to demonstrate the practical application of Frida to bypass security controls.

The challenge involved obtaining an Android app where a "Move" button was present. Upon pressing the button, a message would appear indicating that movement was not possible because the light was not green

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed6f7e90-11bd-4277-b8b1-3c0ee713fac8/Untitled.png)

Challenges of this nature typically require tampering with the code to modify the red light to green in order to proceed and obtain the flag. However, in this particular case, the app had security measures in place that prevented the recompilation of the modified APK, making it more difficult to bypass the intended restrictions.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8ff2baf6-c9bb-448d-ab90-8e4bd265967f/Untitled.png)

During Static Analysis the code seemed to declare a variable named decrypt of type Decrypt, and a boolean variable named **red** which is initially set to true.

The checklight() method then checks if the value of the red boolean variable is false. If red is false, it means the light is green, it invokes decrypt and probably shows the flag else it shows the “You cannot move” message.

The below frida script can be used to change the value of red.

```jsx
Java.perform(function () {
  var MainActivity = Java.use('com.nahamcon2023.redlightgreenlight.MainActivity');

  MainActivity.checkLight.implementation = function (view) {
    this.checkLight(view);
    this.red.value = false;
  };
});
```

**Command:** frida -U -f com.nahamcon2023.redlightgreenlight -l redtofalse.js

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed491502-f6f1-4af2-9a7b-1c1818ff9ef8/Untitled.png)

Logic Bypassed

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/34d86154-74c5-4896-b3bf-a069a2144708/Untitled.png)
