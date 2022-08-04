+++
title = "Setting up remote Fortran debugging on gs66-emu"
description = "Setting up remote Fortran debugging on gs66-emu"
date = 2022-08-04
+++

# Summary

This tutorial will demonstrate how to set up the CLion IDE to run and debug David Bennett's microlensing code on gs66-emu.

# 0. Requirements
1. Have CLion installed.
    * CLion can be downloaded [here](https://www.jetbrains.com/clion/download/#section=mac).
    * A free academic license for students, faculty, and those engaged in academic research can be obtained [here](https://www.jetbrains.com/community/education/#students).
2. Have the example project downloaded.
    * Download it from [here](https://drive.google.com/file/d/1aIkgFaO3Ov_kkRmt6WNiqgcrLlrnbc_q/view?usp=sharing).
3. Have your SSH configuration file set up to allow single command connection to gs66-emu.

# 1. Open the project in CLion
1. Start CLion.
2. Click the "Open" button.
3. Select the project directory.
4. If a dialogue box asks you if you want to "Trust" the project, do so.
{{ video(src="open_project_in_clion.mov") }}

# 2. Install the Fortran plugin
Skip this section if you have installed the Fortran plugin previously.
1. From the menu bar, select "Clion > Preferences".
2. Open the "Plugins" section (or search for "Plugins" in the preferences search bar).
3. Select the "Marketplace" tab.
4. Search for "Fortran".
5. Install the Fortran plugin.
6. Click the "Restart IDE" button that appears and confirm the restart.
{{ video(src="install_fortran_plugin.mp4") }}

# 3. Confirm your SSH connection to gs66-emu
It is possible to use more complex SSH commands from within CLion. However, for this tutorial, it is expected that you can connect to gs66-emu with a single command from your SSH configuration (usually configured in `.ssh/config`).
1. Open a terminal.
2. Confirm you can connect to gs66-emu with a single command.
{{ video(src="confirming_ssh_to_emu.mov") }}

# 4. Add the SSH configuration to CLion
1. From the menu bar, select "Clion > Preferences".
2. Within preferences, open "Tools > SSH Configurations".
3. In the "SSH Configurations" window, click the "+" button to add a new configuration.
4. In the "Authentication type" dropdown, select "OpenSSH config and authentication agent".
5. Add the host name as specified in your SSH configuration file (e.g., `gs66-emu`).
6. Set the port to `22`.
7. Confirm with the "Test Connection" button.
8. Apply the changes.
{{ video(src="setting_up_ssh_configuration.mov") }}

# 5. Add the remote toolchain
The toolchain is the collection of compiler, debugger, etc. used to build and execute the code. This will set up CLion to use the Intel "ifort" compiler on gs66-emu. More toolchains can be added later (e.g., a local toolchain).
1. Open CLion's preferences.
2. Within preferences, open "Build, Execution, Deployment > Toolchains".
3. Create a new toolchain with the "+" button, and select a "Remote Host" type.
4. Rename the toolchain to something useful (e.g., `gs66-emu-intel`).
5. In the "Credentials" dropdown, select the SSH configuration we previously created.
6. Click "Add environment" > "From file", and add the path `/local/data/emussd1/greg_shared/intel_environment_file.sh`. This environment file will set the environment variables for the toolchain.
7. Apply the changes.
   {{ video(src="setting_up_the_remote_toolchain.mov") }}

# 6. Add a deployment configuration
A "deployment" tells CLion where you want to deploy the code. In this case, where on gs66-emu we want the code to exist and run from.
1. Open CLion's preferences.
2. Within preferences, open "Build, Execution, Deployment > Deployment".
3. When we created our toolchain, CLion created a default deployment for us based on that. Select that deployment.
4. For the "Root Path", click the "Autodetect" button to automatically find and use the user's home directory as the root directory for the deployment.
5. Go to the "Mappings" tab. Here we specify which local directories should be mapped to which remote directories.
6. Specify a name for the project directory on gs66-emu. The simplest option is to use the same directory name as you're using locally (`modeling_code_and_example_run`). This is relative to the root directory set above (i.e., your home directory).
7. Apply the changes.
{{ video(src="setting_up_the_deployment.mov") }}

# 7. Add the CMake configuration
This tutorial uses CMake instead of GNU Make. CMake has several advantages over Make. Notably for our case, it interacts with IDEs more easily. CLion can also function with Make, but it requires a few extra steps and is a bit less robust. If you prefer Make, let me know and I will provide the instructions for setting up the project via Make in CLion.
1. Open CLion's preferences.
2. Within preferences, open "Build, Execution, Deployment > CMake".
3. Within the existing CMake configuration, change the toolchain to the one created in the previous step.
4. Apply the changes.
{{ video(src="setting_up_the_cmake_configuration.mov") }}

# 8. Load the CMake project
This step tells CLion what should be considered the build instructions for the project (e.g., the CMake instructions file).
1. In the "Project" panel, open "modeling_code > CMakeLists.txt". This is equivalent to a "Makefile" for GNU Make.
2. In the yellow notification bar at the top of the file, click "Load CMake Project". It will take a moment as the IDE uploads the project and configuration to gs66-emu for the first time. You can see the progress, but clicking the progress bar at the bottom of the IDE.
{{ video(src="load_the_cmake_project.mp4") }}

# 9. Set up the run configuration
By default, a CLion will just run a built executable from the project root with no inputs. This step changes that.
1. Near the top right of CLion, there is a dropdown box showing the currently selected run configuration. Click the dropdown, and select "Edit Configurations...".
2. If not already selected, in the left panel, select the "minuit_all_rvg4Ctpar.xO" configuration.
3. For "Working directory" use the folder button to change the working directory to path to the `example_run` directory. Note, the full local path will be shown, but all local paths are mapped to remote paths as we specified in the deployment.
4. Click the checkbox for "Redirect input from", and use the folder button to select `example_run/run_1.in`.
5. Apply the changes.
{{ video(src="set_up_the_run_configuration.mov") }}

# 10. Run the run configuration
1. Click the play button next near the top right of CLion. This will run the code remotely on gs66-emu using the configurations specified. You should see the output in the output window. CLion will automatically rebuild the executable on changed code before a run.
2. Stop the code with the stop button next to the output window.
{{ video(src="running_the_code.mov") }}

# 11. Use the debugger
For demonstration purposes, we will add a breakpoint to the code, but this is not necessary to run the code in debug mode (which will catch unexpected errors without any breakpoints in the code).
1. Open `modeling_code/fcnrvg4_Ctpar.f` in the editor.
2. Add a break point at line 688 by clicking in the gutter next to the line number.
3. Click the bug button next to the play button at the top right of CLion. The code will begin, then pause at the breakpoint. Note, the IDE pauses before the line of code with the breakpoint is run.
4. In the debug panel at the bottom, there are two tabs: "Debugger" and "Console". Console shows the default output of the script. Make sure the "Debugger" tab is selected.
5. All variables the debugger can find will be displayed with their current values. (Note: With this setup, the variables were taking much longer to load than I would expect. I need to investigate if this is because the toolchain versions are older on gs66-emu or if it's due to the codebase. And what can be done to improve this speed, as I expect much faster response times normally. You may also receive an "IDE error occurred" message. This is a bug in the version of the Fortran plugin at the time of writing. You can ignore this.)
6. The half-play button in the panel will continue the code until it hits another breakpoint or other error.
7. The various arrow buttons can be used to step through the code line-by-line from the current location.
8. The calculator button can be used to execute (mostly) arbitrary commands at that point in the code.
{{ video(src="using_the_debugger.mp4") }}

# Extras
* At the right side of CLion, there is a tab for "Remote Host", which provides a graphical file explorer of gs66-emu. This allows for easy editing and drag-and-drop interaction will files on any configured remote host.
* The menu "Tools > Open Remote Host Terminal" will open a terminal on gs66-emu.
* Executables built with CMake and CLion can be found in their respective configuration directories on gs66-emu. For example, the executable in my case can be found at `/local/data/emussd1/golmsche/modeling_code_and_example_run/modeling_code/cmake-build-debug-gs66-emu-intel/minuit_all_rvg4Ctpar.xO`.
