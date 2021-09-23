# UnCrackable 2

As the challenge "UnCrackable 2" we need to bypass the Root controll, as before
we use apktool to unpack and read the smali of the app:

```bash
apktool d UnCrackable-Level2.apk -o UnCrackable-Level2
```

Then to read the code in a confortable way we can use
![jadx](https://github.com/skylot/jadx) and read the content of the apk:

```bash
mv UnCrackable2.apk UnCrackable2.zip
unzip UnCrackable2.zip
jadx -d out classes.dex
cd out/sources/sg/vantagepoint/uncrackable2
jadx-gui MainActivity.java
```

We read that there are some controll on Root and Debugging, as we expected. So
now we can modify the MainActivity.smali and comment all the controll on the
Root detection, to look like this:

```smali
...
# virtual methods
.method protected onCreate(Landroid/os/Bundle;)V
    .locals 4

    invoke-direct {p0}, Lsg/vantagepoint/uncrackable2/MainActivity;->init()V

    #invoke-static {}, Lsg/vantagepoint/a/b;->a()Z

    #move-result v0

    #if-nez v0, :cond_0

    #invoke-static {}, Lsg/vantagepoint/a/b;->b()Z

    #move-result v0

    #if-nez v0, :cond_0

    #invoke-static {}, Lsg/vantagepoint/a/b;->c()Z

    #move-result v0

    #if-eqz v0, :cond_1

    #:cond_0
    #const-string v0, "Root detected!"

    #invoke-direct {p0, v0}, Lsg/vantagepoint/uncrackable2/MainActivity;->a(Ljava/lang/String;)V

    #:cond_1
    invoke-virtual {p0}, Lsg/vantagepoint/uncrackable2/MainActivity;->getApplicationContext()Landroid/content/Context;

    move-result-object v0
...
```

than if we recompile and sign the apk it should work with no problem:

```bash
apktool b UnCrackable-Level2 -o UnCrackable-Level2-mine.apk
apksigner sign -v --in UnCrackable-Level2-mine.apk --v2-signing-enabled --ks <key.keystore> --ks-key-alias <alias>
adb install UnCrackable-Level2-mine.apk
```

As with the first challenge to find the flag we use ![Frida](https://frida.re/)
and this is the script to find the flag:

```javascript
Interceptor.attach(Module.getExportByName('libc.so', 'strncmp'), {
    onEnter: function(args) {
        var param1 = Memory.readUtf8String(args[0]);
        var param2 = Memory.readUtf8String(args[1]);
        if(param1.localeCompare("AAAAAAAAAAAAAAAAAAAAAAA") == 0 || param2.localeCompare("AAAAAAAAAAAAAAAAAAAAAAA") == 0){
            console.log("Params strncmp: " + param1 + " " + param2);
        }
    },
});
```

```bash
frida -U -l script.js owasp.mstg.uncrackable2 --no-pause
```

```text
FLAG: Thanks for all the fish
```
