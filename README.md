NahamCon CTF 2023 - “Red Light Green Light” - Category: AndroidDuring my participation in the Nahamcon CTF, a challenge involving Android app caught my attention. The primary goal of this write-up is to demonstrate the practical application of Frida to bypass security controls.

The challenge involved obtaining an Android app where a "Move" button was present. Upon pressing the button, a message would appear indicating that movement was not possible because the light was not green

![Screenshot_20230621-135931 (1)](https://github.com/mohammedshine/CTF-NahamCon2023-/assets/34446299/cc9631ab-c969-4b3c-9ac8-8043e80266c1)

Challenges of this nature typically require tampering with the code to modify the red light to green in order to proceed and obtain the flag. However, in this particular case, the app had security measures in place that prevented the recompilation of the modified APK, making it more difficult to bypass the intended restrictions.

![Untitled](https://github.com/mohammedshine/CTF-NahamCon2023-/assets/34446299/ebf27b49-593d-41f0-b5bc-24e890acc8b6)


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
![Untitled (1)](https://github.com/mohammedshine/CTF-NahamCon2023-/assets/34446299/67e916b2-349e-4bcd-a8ad-fba359b4773c)


Logic Bypassed
![Screenshot_20230621-142246](https://github.com/mohammedshine/CTF-NahamCon2023-/assets/34446299/1d2ade4e-4199-4785-9dd7-777ff4a6fb55)

