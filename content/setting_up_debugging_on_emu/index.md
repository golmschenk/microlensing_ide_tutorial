+++
title = "Setting up remote debugging on gs66-emu"
description = "Setting up remote debugging on gs66-emu"
date = 2022-08-04
+++

# Summary

This tutorial will demonstrate how to set up the CLion IDE to run and debug David Bennett's microlensing code on gs66-emu.

# 0. Requirements
1. Have CLion installed.
    * CLion can be downloaded [here](https://www.jetbrains.com/clion/download/#section=mac).
    * A free academic license for students, faculty, and those engaged in academic research can be obtained [here](https://www.jetbrains.com/community/education/#students).
2. Have the example project downloaded.
    * Download it from [here](https://google.com).
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

# 5. Add a deployment configuration
A "deployment" tells CLion where you want to deploy the code. In this case, where on gs66-emu we want the code to exist and run from.
1. Open CLion's preferences.
2. Within preferences, open "Build, Execution, Deployment > Deployment".
3. Create a new deployment with the "+" button.
4. Set the name to that CLion will refer to the server as (e.g., `gs66-emu`).
5. From the SSH configuration dropdown, select the configuration we previously created.
6. Click the "Autodetect" button to automatically find and use the user's home directory as the root directory for the deployment.
7. Go to the "Mappings" tab. Here we specify which local directories should be mapped to which remote directories.
8. Specify a name for the project directory on gs66-emu. The simplest option is to use the same directory name as you're using locally (`modeling_code_and_example_run`). This is relative to the root directory set above (i.e., your home directory).
9. Apply the changes.
{{ video(src="setting_up_the_deployment.mov") }}