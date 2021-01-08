---
title: "Stop wasting your Android gradle daemons"
date: 2021-01-08T16:48:02+09:00
draft: false
---

As you probably noticed, switch between your dear Android Studio and your terminal
is not without some latency / weird error messages about some Gradle daemons that could not be reused.

{{< image src="/assets/2021-01-08-gradle-custom-shortcut/not-reused.png" position="center" >}}

The reason is because Android Studio is coming by default with its own JDK installation,
allowing to use Android Studio without having to install Java
or to be sure all your team is using the same compiler.

That's convenient for sure, but it's not something your Gradle project is aware and so,
if you're trying to use Gradle without Android Studio's help,
you will probably fallback to your default system JDK,
which is not the one that Android Studio is using.
It can affects you in 2 ways:

1. not re-using the existing Gradle daemon (!!)
1. not re-using your build cache (if your default JDK has a different version than the AS's one)

Let's see quickly how to fix this issue, either in your terminal
or by leveraging an Android Studio's hidden gem.

# Use the same JDK in your terminal

This fix is easy: just set your JAVA_HOME to the one bundled with the Android Studio you're using.

```bash
# Consider a default OSX installation
export ANDROID_STUDIO_HOME="/Applications/Android Studio"

# Override your JDK path
export JAVA_HOME="$ANDROID_STUDIO_HOME/Contents/jre/jdk/Contents/Home"

# Now you can use Gradle without wasting your daemon.
# Better adopt https://github.com/gdubw/gdub at the same time.
gw help
```

But if you're using the awesome [Jetbrains Toolbox](https://www.jetbrains.com/toolbox-app/)
to manage multiple installations, things will be a little more complicated because all IDEs
are stored in **~/Library/Application Support/JetBrains/Toolbox/apps**.
I wrote a [script detecting all Android Studio installations][script] but clearly,
it's not what we can define as an easy solution.

[script]: https://github.com/pgreze/dotfiles/blob/main/bin/aidea.py

That's why stay in Android Studio would be more convenient,
so let's try to discover how we can use it to quickly run arbitrary commands.

# Android Studio hidden gem: run arbitrary Gradle commands from your IDE

Android Studio is providing a Gradle helper, allowing to discover and
run commands directly from your IDE:

{{< image src="/assets/2021-01-08-gradle-custom-shortcut/gradle-helper.jpg" position="center" style="width: 500px" >}}

It's a convenient tool for beginners, but if you already know which command you're looking for,
you need to find it in one of these folders, and it requires several clicks before finding your command,
if its not lost in the middle of 1000 other ones...

Luckily there's a way to **run any command, just by entering its name, only with your keyboard**.
It's relying on the IntelliJ's ability to assign custom shortcut to any available action.

First use Command + Shift + A (on OSX) to search the **Execute Gradle Task** action:

{{< image src="/assets/2021-01-08-gradle-custom-shortcut/search-gradle-run.png" position="center" style="width: 500px" >}}

Now press ⌥ + enter (on OSX) to assign your own custom shortcut (I chose ⌥ + G).

{{< image src="/assets/2021-01-08-gradle-custom-shortcut/assign-custom-shortcut.png" position="center" style="width: 500px" >}}

And now try your new shortcut allowing to execute any command,
with the option to run it for all modules or the one you prefer,
and without leaving Android Studio and override your terminal 🙌

{{< image src="/assets/2021-01-08-gradle-custom-shortcut/run-gradle-command.png" position="center" >}}