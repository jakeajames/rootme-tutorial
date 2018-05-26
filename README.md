# rootme-tutorial
How to run Xcode apps as root and unsandboxed while still debugging them easiliy?

Many new jailbreak devs are find it difficult and/or a hassle to run apps as root and debug them at the same time. I got this idea when working on a project, a cydia application (eta **not** son) which needed root, but at the same time was complex and I needed a good and easy way to debug it. That's why I made this tool. At first it was private, considering any App Store apps can bundle this method (if they're smart enough to bypass the App Store checks) but I just reminded a project of MasBog where he could detect if an app was being debugged or not, I implemented that and made rootme only work while the app is being debugged. Huge thanks to MasBog!

# How to set it up?

- Install rootme from my repo (https://jakeashacks.com/cydia) and run it, preferably using SSH on a computer or NewTerm 2. (This way you can terminate it when you're done using Control + C). 
- Grab AppSupport headers and add them into your include path (https://github.com/theos/headers/tree/05405174749d912f7726121fcb5f27de73af0f08/AppSupport)
- Grab rocketbootstrap headers (https://github.com/theos/headers/tree/05405174749d912f7726121fcb5f27de73af0f08/AppSupport)
- Take any xcode project and link it with librocketbootstrap.dylib (Take this from your device, it's on /usr/lib) and AppSupport.tbd (download from this repo)
- Include "rocketbootstrap.h" and "AppSupport/CPDistributedMessagingCenter.h" on your main.m file
- Add this code on it:
    ʹNSLog(@"uid now %d", getuid());
    CPDistributedMessagingCenter *messageCenter = [CPDistributedMessagingCenter centerNamed:@"com.jakeashacks.rootme"];
    rocketbootstrap_distributedmessagingcenter_apply(messageCenter);
    [messageCenter sendMessageAndReceiveReplyName:@"rootme" userInfo:[NSDictionary dictionaryWithObject:[NSString stringWithFormat:@"%d", getpid()] forKey:@"pid"]];
    NSLog(@"uid after %d", getuid());ʹ
- Done! Enjoy!

Notes:
- rootme will give your app root permissions on uid and gid
- rootme will fully unsandbox your app
- killing an app using the stop button on Xcode doesn't seem to work but you can kill it from the app switcher yourself
- if you find any bugs tell me
- Will not open-source because it'll be easy for people to manipulate it. 
- Credits to: masbog, coolstar, stek29, ninjaprawn, Ian Beer
