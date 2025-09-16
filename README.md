# COSE 213 Git & GitHub Setup

## Overview

Welcome to your first COSE 213 assignment! This should help get you acquainted with the workflow of the coding assignments for this class. While the coding aspect of this assignment is trivial, the process that you need to follow is probably new to you. Understanding the workflow (and being able to pull it off!) is the main goal of this assignment. You'll need this to do all the remaining homework in the class.

This is important enough that we'll explain the same workflow in several different ways.

## Workflow

In general, the workflow for each assignment will be the same. I'll explain in words first, followed by the specific commands.

- First, you will click on a link in LMS that will take you to a GitHub Classroom page. If you have not yet created your personal copy of that assignment's repository, it will invite you to create your copy. On the other hand if you've done it before, you'll see a link to your personal copy of the assignment. You only have to create this copy one time for each assignment. Pretty straightforward. The repository can only be viewed by you and the COSE 213 teaching team. Other students can't see your repo. You can view the contents of your repo by going to the link which was created for you upon accepting the assignment.

- Now that you've created your git repository for the assignment, you can clone it. If you're not familiar with git and/or GitHub consider checking out the [git crash course](https://github.com/2025-2-COSE213-01/git/blob/main/git.md).

- Once you have your repository locally checked out, you can configure it for building and editing. This will involve setting up your editor and environment with the right tooling (Jupyterhub has this set up already). You'll only need to set up the tooling one time per computer, but you will need to configure each assignment one time when you first clone the repo.

- Then you can edit code! The specifics here differ per assignment, but the build/run process is the same.



## Windows Build Guide (CMake + Ninja + MSYS2/MinGW-w64)
This project uses CMake for configuration and Ninja for builds. The compiler is the latest MinGW-w64 (g++) from MSYS2.

1. Install prerequisites
   1) Install Chocolatey (at window powershell)

      ```console
      Set-ExecutionPolicy Bypass -Scope Process -Force; `
      [System.Net.ServicePointManager]::SecurityProtocol = `
      [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; `
      iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
      ```

   2) Install CMake / Ninja
      ```console
      choco install -y cmake --installargs 'ADD_CMAKE_TO_PATH=System'
      choco install -y git
      choco install -y ninja
      ```
   
2. Get a modern g++ (MSYS2 + MinGW-w64)
   1) Install MSYS2 from https://www.msys2.org (default: C:\msys64).

   2) Open the “MSYS2 MSYS” terminal and update twice:
      ```console
      pacman -Syu
      # close the window if prompted, reopen "MSYS2 MSYS"
      pacman -Syu
      ```

   3) Open “MSYS2 MinGW x64” and install the 64-bit toolchain:
      ```console
      # download all (it is a default. just press enter)
      pacman -S --needed mingw-w64-x86_64-toolchain
      ```

   4) Fix PATH (important)
      Add C:\msys64\mingw64\bin to the system Path.
      Remove any old entries like C:\MinGW\bin or duplicate MSYS paths.
      Verify in a new PowerShell:
      ```console
      where g++
      g++ --version     #You should see a recent version (typically GCC 12–14).
      ```


## macOS Build Guide (CMake + Ninja + AppleClang/LLVM)
This project uses CMake for configuration and Ninja for builds. The default compiler is AppleClang (Xcode Command Line Tools). You can optionally use Homebrew LLVM (clang) or GCC.

1. Install prerequisites
   1) Install Homebrew
      Apple Silicon uses /opt/homebrew, Intel uses /usr/local.
      ```console
      # Install
      /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

      # Add to PATH (Apple Silicon)
      /bin/echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
      eval "$(/opt/homebrew/bin/brew shellenv)"

      # (Intel)
      /bin/echo 'eval "$(/usr/local/bin/brew shellenv)"' >> ~/.zprofile
      eval "$(/usr/local/bin/brew shellenv)"
      ```

   2) Insatll CMake / Git / Ninja
      ```console
      brew install cmake git ninja
      ```


## SSH Key setup

The above `git clone` command uses an ssh key to securely identify who you are to GitHub. You'll need this regardless of where you do your development. To set this up, let's follow [GitHub's instructions on ssh keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). That process is summarized here so you can copy/paste. You only need to do this one time for a development machine.

First make a public/private key pair. Be sure to edit this command to use the email address that GitHub knows about. It will ask you questions; you can just hit Enter to accept the default file name and to not set a passphrase.

```console
ssh-keygen -t ed25519 -C "your_email@example.com" # generate a public/private key pair
```

Now you have two new files in your `~/.ssh` directory: `id_ed25519` (the private key) and `id_ed25519.pub` (the public key). We need to copy the public key into the clipboard, and (in the next step) paste it into GitHub. First, spill out the contents of your _public_ key to console like this:

```console
cat .ssh/id_ed25519.pub
```

It will produce something like `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFDNwrIYl7vb4SfjUTDVz6q3VLeo9WROMyM7N/mkkaot your_email@example.com` to the console. Select and copy that text to your clipboard.

Now open a browser to your GitHub SSH Key settings at [https://github.com/settings/keys](https://github.com/settings/keys). Click the "New SSH Key" button. Give it an obvious name that identifies how you use it, like "JupyterHub ssh key", and paste your public key into the "Key" box. Save it. The page reloads and you should see that key in your list. Groovy.

Go back to your terminal and try to clone the homework repository.

```console
git clone git@github.com:cu-cspb-2270-Spring-2024/hw-00-git-setup-like1shot.git
```

The first time you do this it will say something about "the authenticity of the host can't be established". You can ignore this and just hit Enter. It should clone your repository.

The good news is you now never have to do this again! Whenever you want to clone a repository, you can get it from your repository's GitHub page under the big green "Code" button (use the "SSH" tab and copy the URL).


## Annotated Directions

You can work your assignments on your local computer, though this will require some setup that many of you will not want to deal with now. You can always get your local machine set up later, and we will help you do this if your setup is fairly normal.


1. At this point you can begin working with your code. We recommend that you use the VSCode editor environment. Go to `File -> New -> Launcher` and click on `VS Code IDE`. This opens a new browser tab.

2. Within VS Code, select `File -> Open Folder...` and then find the directory you just cloned and press OK. It should be `/home/jovyan/hw-NN-homework-username`. In JupyterHub your username is always `jovyan`. This is unique to the CSEL environment. On your local machine it will be something else. New terminals by default will have a current working directory to the folder you open. Later on, if nothing you do seems to work, it might be that your editor is operating out of the wrong directory.

3. Before we start writing any code, let's compile the starter code and see how we can run the executables.

   - Open up the terminal: `Terminal > New Terminal` (or use Control Backtick (the \` character, to the left of the `1` key))

   - Make `build` directory:

   ```console
   mkdir build
   ```

   - Run `cmake` to set up the build environment (creates files in _build_ subdirectory that are needed to build the application). Note the double dots!

   ```console
   # (window)
   cmake -S . -B build -G "Ninja" -DCMAKE_C_COMPILER=gcc -DCMAKE_CXX_COMPILER=g++

   # (macOS)
   # (using AppleClang by default)
   cmake -S . -B build -G Ninja
   ```

   - Now compile and build the executables:

   ```console
   cmake --build build
   ```

   - Now we have two executable ready to run, `run_app` and `run_tests`

     - `run_app` is the executable that was build using your `/app` and `/code` source code files. You can use this to try a `Hello World` type of program to get familiar with the framework.
     - `run_tests` is the executable containing the test cases that will be used for your assignment. Running this prior to solving any of the required problems should give you a score of 0.
     - Both executables will be located within the build directory, so run the following terminal command to execute:

   ```console
   ./build/run_app
   ```
      - If you see the message "Assignment 00 Success! Your environment configuration is all clear", it means your environment has been successfully set up for future assignments. Please proceed to the "Completing the Assignment" steps below.
      - However, if you do not see this message, there is an issue with your environment configuration. Please try again the environment seeting flows, or contact the TA at [donghyeon_kim@korea.ac.kr] for assistance.


4. Each time you modify a source file (like `main.cpp`) you must rebuild the executables.

   - Save your changes to disk. The compiler will only be able to use changes that have been saved to disk.
   - Issue the `make` command (must be run from the `build` directory). This will recompile the source files and rebuild the executables.
   - Run the application or run the tests with `./run_app` or `./run_tests`. Note the dot-slash before the executable name - this is important.

## Completing the Assignment

We intentionally made this part trivial. In the future assignments, the initial setup steps described above should only take a moment to complete for any assignment. The bulk of your required work will be described in this section.

1. After running the application, determine what is being tested for this assignment See the **Test Cases** section below.
2. Make a modification to the code to **Pass** the test. (hint: each time you make changes to the code, be sure to save your edits to the code, _make_ the application with the new code, and then test the resulting application)
3. We suggest that you save your code each time you get a test to pass. Make sure to save it both locally and on the GitHub server.

### Checking code into GitHub

**Important:** one of the main advantages of using git and GitHub is that it gives you a very convenient way to back up your changes. For now, you will only need to know how to `add`, `commit`, and `push`. (You are encouraged to learn further git commands throughout the semester.) Any time you get to a point where you want to save your code (for example, you got a function to pass a test case), do the following in the terminal:

1. Stage your changes for a commit. This means that any changes you want to save, need to be added explicitly. (Note that you have all kinds of files that are present in your repository. You only need to add the ones you have changed and want to save.) For example, let's say we want to stage the changes we made in `functions.cpp`.

   ```console
   git add functions.cpp
   ```

2. Commit the changes to our local repository. The `-m` option allows you to add the required commit comment on the command line. Use a short comment that has meaning about what changes were made (e.g. "fixed output for test 2" or "added comments to all functions")

   ```console
   git commit -m 'this is my first commit'
   ```

3. Finally, push the changes from your local version of the repo to the remote repo on GitHub.

   ```console
   git push
   ```

You can run the above steps as many times as you want. Each time you commit/push your changes, a new commit number will be added to your repository. If ever you need to go back to a previous commit, you can do that with more advanced usage of the git tool.

### Test Cases

Open up the test code in the `/tests` directory to see what test cases you need to pass to get points on the assignment. For this assignment this is a file called `test_hw.cpp`. **It will be extremely useful for you to be able to read and understand what the test cases are doing.** You can learn from the unit tests and there are often hints contained in them (intentionally or otherwise).

#### Part 1

In `test_hw.cpp` scroll down until you find the test function labeled `TestFooA`. You can see that this is a unit test for the `fooA()` function. An assertion is used to check the output for correctness. Go to `/code/functions.cpp` to see if you can implement this function to return the correct output (again, this is intentionally trivial.) Once you have written the code and you want to check your solution, run `make` (remember to stay in the `build` directory). Then run the test executable `run_tests` and see if you pass the first test case. _If you are happy with your code, go ahead and run through the steps described above in the Checking code into GitHub section._

#### Part 2

Now let's take a look at the next test case, `TestApp_1`. This one actually runs your `main()` function to check whether it prints the desired results in `stdout`. Look at the test case to see what the desired result needs to be, then go into `/app/main.cpp` and add the code to get a matching result.
