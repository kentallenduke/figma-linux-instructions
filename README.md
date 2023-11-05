# figma-linux-instructions
A guide on how I installed Figma Linux with Wayland, Vulkan, and hardware acceleration on Fedora

After countless hours trying to get Figma Linux running optimally on my machines, I figured out how to get Wayland, Vulkan, and hardware acceleration working on Fedora 39 Workstation without using the web version of Figma. 

![figma-linux](https://github.com/Figma-Linux/figma-linux/assets/10662332/36b6f2b7-03b5-46b8-8e80-749264537f75)

I'm noticing a significant boost in performance, crisper text, and better power savings. The only shortcoming is that the window which Figma will run on will lose its shadow. [This is due to a technical limitation with frameless windows on Linux.](https://github.com/electron/electron/issues/2380)

I am writing this workaround because I know many of us have been struggling to get Figma working well on Linux. This guide assumes you are on Fedora 39 Workstation (this will work on Fedora 38 Workstation). Here's how I did it. 

### 1. Download and building the source code ###

Follow the instructions on [Building from source.](https://github.com/Figma-Linux/figma-linux#building-from-source)

- Download the ZIP file by clicking the green "< > Code" button and clicking "Download ZIP." Make sure the branch is set to "dev."
- Save the ZIP anywhere you choose. I put mine in my Downloads folder.
- Launch nautilus. Navigate to the directory you saved your download. Extract the files, then open the folder you've unzipped.
- Right inside the folder, then click "Open Terminal."
- Install Rust by copy-pasting the following into terminal:
 ``curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh``
- Install prerequesites from npm by copy-pasting the following into terminal:
``npm i``
- Create the AppImage by copy-pasting the following into terminal:
- ``npm run builder``

If everything went well, you should have an AppImage file in your `build/installers` folder. In my case, it's  ``~/home/kentduke/Downloads/figma-linux-dev/build/installers/figma-linux_0.10.0_linux_x86_64.AppImage``.

### 2. Setting up AppImage and Installing Figma ###
![gearlever-figma](https://github.com/Figma-Linux/figma-linux/assets/10662332/fb7762f3-548a-451b-a55b-f98fc9cc47bc)

In this step, we are going to be using a handy tool called [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever) to install Figma and register it so that it appears on GNOME's launcher.

- Install [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever) from GNOME Software or through terminal.
- Open Gear Lever.
- Install Figma either by dragging and dropping the AppImage onto Gear Lever or clicking "Open File" and navigating to the AppImage file.
- A blue tooltip will appear asking to grant permission. Authenticate.

You should see "figma-linux" in GNOME's application launcher. Don't open it yet.

### 3. Enabling Wayland, hardware acceleration, and Vulkan

We're halfway there! Now that you've installed Figma, it's time to configure it. Thanks to the updates in Electron, we can now enable Wayland and hardware acceleration.

- Go to your local applications folder. In my case, it's `~/home/kentduke/.local/share/applications`.
- Edit the Desktop file that Gear Lever created for figma. Mine is `gearlever_figmalinux_6c9f53.desktop`.
- Go to the Exec line, then copy and paste this:
``sh -c "/home/kentduke/AppImages/gearlever_figmalinux_6c9f53.appimage --enable-features=UseOzonePlatform --ozone-platform=wayland --enable-vulkan --enable-gpu-rasterization --enable-oop-rasterization --enable-gpu-compositing --enable-accelerated-2d-canvas --enable-zero-copy --canvas-oop-rasterization --disable-features=UseChromeOSDirectVideoDecoder --enable-accelerated-video-decode --enable-accelerated-video-encode --enable-features=VaapiVideoDecoder,VaapiVideoEncoder,VaapiIgnoreDriverChecks,RawDraw,Vulkan --enable-hardware-overlays --enable-unsafe-webgpu"``
Replace "kentduke" with your username. 

![Screenshot from 2023-11-05 14-47-54](https://github.com/Figma-Linux/figma-linux/assets/10662332/364c456b-5824-4643-9d99-4c63807ad5cc)

This is how my EXEC line looks like on my desktop file. Save your file and exit.

### 4. Signing into Figma ###

Assuming everything was set up correctly, you'll notice that Figma will look a lot more crisp when you launch it. However, we have one major hurdle to go over: signing into Figma. If you follow the instructions that Figma gives you, the external web browser can't hand off your credentials to Figma on Linux. Luckily, there's a simple workaround you can do to log in.

- Click on the Figma logo at the top left of the window.
![Screenshot from 2023-11-05 15-02-29](https://github.com/Figma-Linux/figma-linux/assets/10662332/ef14bf6c-2741-4057-9287-11466d6c682a)

- Click the blue "Get started for free" on the right side of the window.
- Log in with your Figma credentials.

### 5. Conclusion and installing custom fonts ###

That's it! Hopefully you should have Figma running with Wayland, Vulkan, and hardware acceleration enabled. If you need to load local fonts, download your font files first, then copy its folder over to `/usr/share/fonts`.
