# Flutter and meta-flutter

Flutter is an open-source UI software development kit (SDK) created by Google for building natively compiled, multi-platform applications from a single codebase. Using the Dart programming language, it enables fast development for mobile (iOS, Android), web, desktop (Windows, macOS, Linux), and embedded devices. It features a "hot reload" function, allowing developers to view code changes 
instantly without losing app state.

# Flutter in AGL

AGL embeds the meta-flutter embedder developed by Toyota (https://github.com/meta-flutter).
It comes with its own workspace_automation tooling to setup a full development environment.


# Setting up your workspace for AGL

If you clone AGL-repo, there will be a matching workspace-automation in   ./external/workspace-automation and there will also be a matching  ./external/workspace-config/ .

## Quick start

The following steps are the gist of the full process. The detailed explanation follows.

```
#Use in-tree version in  ./external/workspace-automation/
#Call:
./flutter_workspace.py \
	    --enable agl-ivi-demo-flutter-qemu-unagi \
	    [ --flutter-version 3.38.3 ]
… wait …

source setup_env.sh			# all the magic ;)

# run AGL emulation
run-agl-ivi-demo-flutter-qemu

cd <path_to_your_flutter_project>

flutter run [-d agl-qemu-unagi]
```

## Case 1: Basic Setup (no AGL APIs, same as 'Linux Desktop')
* This will compile and run your flutter application on the 
Linux desktop (aka Ubuntu 22.x in our case).
* You can develop, debug, run all on your PC.
* No IDE
* No specifics of the target environment!

```
./flutter_workspace.py --flutter-version 3.38.3
source setup_env.sh
cd <path_to_your_flutter_project>
flutter run -d linux
```

Pros and cons:
```
+ simple and straightforward setup
+ native performance running the app

- no target environment specifics / apis available
- integration to target environment necessary later-on
- no big difference to plain flutter development environment
```

## Case 2: Setup with AGL/AGL APIs + Qemu
* This will compile and run your flutter application for use within a VM that is running in the background.
* You can develop, debug, run for the target environment.
* No IDE, yet.

```
./flutter_workspace.py \ 
--enable agl-ivi-demo-flutter-qemu-unagi \ 
[ --flutter-version 3.38.3 ]
source setup_env.sh
run-agl-ivi-demo-flutter-qemu
cd <path_to_your_flutter_project>
flutter doctor -v 
should show: "AGL unagi-latest QEMU Image (mobile)" 
flutter run -d agl-qemu-unagi
```

Pros and cons:
```
+ simple and straightforward setup
+ target environment specifics / apis
+ no separate integration step lateron needed

- requires emulated target VM (qemu), thus slower

```

## Case 3: AGL + QEMU + VScode integration
* This will compile and run your flutter application for use within a VM that is running in the background.
* You can develop, debug, run for the target environment.
* Within IDE ;)

```
./flutter_workspace.py \ 
--enable agl-ivi-demo-flutter-qemu-unagi \
[--flutter-version 3.38.3]
source setup_env.sh
run-agl-ivi-demo-flutter-qemu
cd <path_to_your_flutter_project>
flutter doctor -v 
should show: "AGL unagi-latest QEMU Image (mobile)" 
code .
select run config under flutter and choose
"agl-unagi latest"
```

Pros and cons:
```
+ simple and straightforward setup
+ target environment specifics / apis
+ no separate integration step lateron needed
+ IDE integration

- requires emulated target VM (qemu), thus a bit slower
```

