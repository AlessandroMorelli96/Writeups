# UnCrackable 1

Run the emulator:

```bash
/usr/local/share/android-sdk/emulator/emulator -no-boot-anim -netdelay none -accel on -no-snapshot -wipe-data -skin 1080x1920 -avd generic_10_64
```

First we need to try the app by installing it on a Android devoce:

```bash
adb install UnCrackable-Level1.apk
```

As we can see the app as a controll on if the device is rooted or not.

To uncopress the app, we can use apktool or by unzipping it with:

```bash
apktool d UnCrackable-Level1.apk -o UnCrackable-Level1
```

## Bypass Controll

To bypass controll on rooted device (and debugging), we change the file `UnCrackable-Level1/smali/sg/vantagepoint/uncrackable1/MainActivity.smali` to be like this:

```smali
# virtual methods
.method protected onCreate(Landroid/os/Bundle;)V
    .locals 1

    invoke-static {}, Lsg/vantagepoint/a/c;->a()Z

    move-result v0

    #if-nez v0, :cond_0     ROOT CONTROLL OLD
    if-eqz v0, :cond_0

    invoke-static {}, Lsg/vantagepoint/a/c;->b()Z

    move-result v0

    #if-nez v0, :cond_0     ROOT CONTROLL OLD
    if-eqz v0, :cond_0

    invoke-static {}, Lsg/vantagepoint/a/c;->c()Z

    move-result v0

    if-eqz v0, :cond_1

    :cond_0
    const-string v0, "Root detected!"

    invoke-direct {p0, v0}, Lsg/vantagepoint/uncrackable1/MainActivity;->a(Ljava/lang/String;)V

    :cond_1
    invoke-virtual {p0}, Lsg/vantagepoint/uncrackable1/MainActivity;->getApplicationContext()Landroid/content/Context;

    move-result-object v0

    invoke-static {v0}, Lsg/vantagepoint/a/b;->a(Landroid/content/Context;)Z

    move-result v0

    if-eqz v0, :cond_2
    #if-nez v0, :cond_2     IF WE ARE DEBUGGING

    const-string v0, "App is debuggable!"

    invoke-direct {p0, v0}, Lsg/vantagepoint/uncrackable1/MainActivity;->a(Ljava/lang/String;)V

    :cond_2
    invoke-super {p0, p1}, Landroid/app/Activity;->onCreate(Landroid/os/Bundle;)V

    const/high16 p1, 0x7f030000

    invoke-virtual {p0, p1}, Lsg/vantagepoint/uncrackable1/MainActivity;->setContentView(I)V

    return-void
.end method
```

Another way could be to modify the code of the bottom of the popup and bypass
the close of the app when pressed file MainActivity$1.smali.

To create an apk, you need to create a keystore to sign it:

```bash
keytool -genkey -v -keystore <name.keystore> -alias <alias> -keyalg RSA -keysize 2048 -validity 10000
```

than to crate the apk and sign it:

```bash
apktool b UnCrackable-Level1 -o UnCrackable-Level1-mine.apk
apksigner sign -v --in UnCrackable-Level1-mine.apk --v2-signing-enabled --ks <path.keystore> --ks-key-alias <alias>

adb install UnCrackable-Level1-mine.apk
```

Now if we run the app we can insert txt in input and verify our input.

## Pass the Verification

To do this we use ![Frida](https://frida.re/), and this is the javascript code
to use:

```javascript
Java.perform(function(){
    Java.use("sg.vantagepoint.a.a").a.implementation = function(param1,param2) {
        var flag = this.a(param1,param2);
        console.log(flag);
        var flag1 = "";
        for (var i=0; i < flag.length; i++){
            flag1 += String.fromCharCode(flag[i]);
        }
        console.log("Flag: " + flag1);
        return flag;
      };
});
```

We could have just use Frida also to reimplemnt the function exit, in this case
we do not need to patch the app. To do this we would have use a script like this:

```javascript
Java.perform(function(){
    Java.use("java.lang.System").exit.implementation = function(param1) {};
    Java.use("sg.vantagepoint.a.a").a.implementation = function(param1,param2) {
        var flag = this.a(param1,param2);
        console.log(flag);
        var flag1 = "";
        for (var i=0; i < flag.length; i++){
            flag1 += String.fromCharCode(flag[i]);
        }
        console.log("Flag: " + flag1);
        return flag;
      };

});
```

Then using Frida you can start the app and add your js as follow (You need to
install frida-server on the device first):

```bash
frida -U -l script.js owasp.mstg.uncrackable1
```

## Another solution

In this case we would like to use jdb, in this case we need to get the PID of
the app. We run the app on the emulator and then `adb shell ps` on the host, to
get the PID. To attach the debugger run:

```
adb forward tcp:<PORT> jdwp:<PID>
{ echo "suspend"; cat; } | jdb -attach localhost:<PORT>
```

```
FLAG: I want to believe
```
