# Introduction

Several software projects we use in this class decided to do major updates that contains breaking changes last year. This is a common issue in software engineering called [Dependency Hell](https://en.wikipedia.org/wiki/Dependency_hell). Understanding the software engineering aspect of this problem is beyond the scope of this class. We will focus on correctly configuring the environment so that we can run our class worksheets and our homework problem sets.

Please follow the instructions below carefully. And if you still have trouble running the worksheets, don't hesitate to reach out your TA and professor. There is a TA office hour this Thursday and my office hour this Friday. **Please come AFTER you have gone through, tried the instructions below and still cannot run the worksheets. Additionally it would be very helpful to your TA and me if you could show a summary of what is not working (such as entire error messages, complete screenshots of WHERE the error happens, and a history of the commands you executed).**

# Windows

*THE FOLLOWING INSTRUCTIONS ONLY APPLY TO WINDOWS MACHINES. IF YOU HAVE A MACOS, SKIP TO THE MACOS SECTION.*

## Cannot locate ipopt executable

One common issue on Windows is `WARNING: Could not locate the 'ipopt' executable, which is required for solver
ipopt` when we try to run `results = solver.solve(model)` under "Example 1: Rosenbrock".

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/windows_cannot_locate_ipopt_executable_error.PNG)

The cause of this error is that ipopt [stopped shipping an `ipopt` binary on Windows after 3.11](https://github.com/conda-forge/ipopt-feedstock/issues/55) due to Windows' lack of support for [`ampl-mp`](https://github.com/ampl/mp). For our class, we can safely use an older version of ipopt on Windows that still ships with a binary.

We will solve this bug using the conda command line, instead of the graphic user interface (GUI).

On Windows, Anaconda provides 2 command line interfaces for conda: one is "CMD.exe Prompt", and the other is "Powershell Prompt". We'll use "Powershell Prompt" for the rest of this instruction.

To launch a Powershell Prompt, open Anaconda Navigator. You will see both "CMD.exe Prompt", and "Powershell Prompt":

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/windows_anaconda_navigator.PNG)

Click "Launch" under "Powershell Prompt" to launch a Powershell. (We'll use Powershell for this tutorial, but CMD.exe could also work)

In the Powershell, run `conda list` by typing `conda list` and then hit `Enter` on your keyboard. You should see all packages that are installed by conda on your computer listed in *alphabetical order*.

Look for `ipopt`. If ipopt is listed as 

```
ipopt                     3.14.14              h1709daf_1    conda-forge
```

on your computer, your operating system is Windows, and you encounter the `Could not locate the 'ipopt' executable` error, then you need to downgraph ipopt to 3.11.x.

Here's a picture showing ipopt installed at version 3.14.14:

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/windows_conda_ipopt_latest_version.PNG)

If you don't see ipopt at all, that means you have not installed any version of ipopt at all. If you have not installed ipopt at all, simply follow the rest of this article to install ipopt.

Next, install ipopt 3.11.1 using

```
conda install conda-forge/label/cf202003::ipopt
```

`cf202003` is a [label that the ipopt developers created for the distribution of their software on conda](https://anaconda.org/conda-forge/ipopt).

You should see something like the following:

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/windows_conda_ipopt.PNG)

Hit `Enter` on your keyboard to select `y` and proceed. 

This will install ipopt version 3.11.1

After installation, run `conda list` again, and it should show ipopt installed as:

```
ipopt                     3.11.1                        2    conda-forge/label/cf202003
```

Now, verify that you can run Class 3 worksheet. First, you need to ***shutdown your current Jupyter Notebook instance.***, and restart a new one. Then run class 3 worksheet.

# macOS

*THE FOLLOWING INSTRUCTIONS ONLY APPLY TO macOS MACHINES. IF YOU HAVE A WINDOWS MACHINE, GO TO THE WINDOWS SECTION.*

## Cannot find `liblapack.3.dylib`

One common issue on macOS is unable to locate `liblapack.3.dylib` which happens when we try to run `results = solver.solve(model)` under "Example 1: Rosenbrock":

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/macos_cannot_locate_liblapack_error.png)

This error is caused by a bug in the `liblapack` project. We need to actually fix this bug to run our worksheets.

First open a Terminal by opening your macOS Launchpad, going into `Other`, and clicking on `Terminal`.

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/macos_terminal_1.png)

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/macos_terminal_2.png)

Terminal gives you a command line interface that you can use to interact with you macOS machine. It is more powerful than the graphic user interface that we normally use, as you will see later.

### Step 1: Find the anaconda directory

First, we need to navigate on the command line to the directory (folder) where Anaconda is installed.

When a Terminal is first launch, it typically defaults to your *home directory*. Now if you run the `ls` command (that is, type `ls` on your command line and then hit `Enter` on your keyboard), you can see all the files and directories that are inside your home directory. You might recognize many names such as `Downloads`, `Desktop`, and `Pictures`.

If you run the `pwd` command, you can see the full path of your home directory, in my case, it is `/Users/46784513`. `pwd` shows the full path of the directory your are *currently* in on the command line. `/Users/46784513` is too much to memorize and type, so a user's home directory is represented by a short-hand symbol `~`.

To navigate to a different directory on the command line, we use the `cd` command. For most people, Anaconda is installed at `~/anaconda3`. If you're in your home directory, and running `ls` lists `anaconda3` as one of the directories under `~`, you can navigate to `~/anaconda3` by running `cd anaconda3`. You can also give `cd` a more complete path, for example `cd ~/anaconda3`. `cd ~/anaconda3` can take you to `~/anaconda3` no matter which directory you are currently in on the command line, whereas `cd anaconda3` can navigate to a directory named `anaconda3` if and only if `anaconda3` is inside the directory you're currently in.

> [!TIP]
> You can always return to your home directory by simply running `cd` with no input argument.

If Anaconda is not installed in your home directory, you can find where anaconda is installed by running `which anaconda`. For example, when I run `which anaconda`, it returns `/Users/46784513/anaconda3/bin/anaconda`

![](You can always return to your home directory by simply running `cd` with no input argument.)

This tells me, Anaconda is installed in the `/Users/46784513/anaconda3` directory (again `/Users/46784513/` is my home directory).

In some of your cases, you might see `~/opt/anaconda3/bin/anaconda` when you run `which anaconda`. That means, Anaconda is installed into the `~/opt` directory. To go the anaconda3 directory on your machine, run `cd ~/opt/anaconda3` on the command line.

Now if `which anaconda` returns nothing, that means you didn't properly install Anaconda at all. Go back to the previous announcement, and make sure you follow the instructions and install Anaconda first.

### Step 2: Check the existence of `liblapack.3.dylib`

Once you navigate to the `anaconda3` directory on the command line, we'll check if the `liblapack.3.dylib` file is indeed missing. 

Run `ls` and see if you have a directory named `lib`. Go into `lib` using `cd` (that is, run `cd lib`).

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/macos_ls_lib.png)

Now check if `liblapack.3.dylib` exists by running `ls -l liblapack.3.dylib`. 

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/macos_liblapack.png)

If it returns `liblapack.3.dylib -> libopenblas_vortexp-r0.3.21.dylib` (also seen in the picture above), that means `liblapack.3.dylib` is not a regular file; Rather, it is reference to a file named `libopenblas_vortexp-r0.3.21.dylib`. in other words, `liblapack.3.dylib` is an alias.

Now check if `libopenblas_vortexp-r0.3.21.dylib` exists by running `ls -l libopenblas_vortexp-r0.3.21.dylib`.

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/macos_libopenblas_vortexp.png)

If it returns `No such file or directory`, we have found the cause of our problem. We need to make sure the `libopenblas_vortexp-r0.3.21.dylib` file exists and it is the correct file.

Luckily the name `libopenblas_vortexp-r0.3.21.dylib` tells us that it's part of the `libopenblas` project. Now let's see what libraries (that is `.dylib` files) we have installed that belongs to the `libopenblas` project.

Run `ls -l libopenblas*` inside the `lib` directory (which you should already be in). If you're running the latest Anaconda version and have followed the instructions to try to install pyomo and ipopt, then you will see something like:

![](https://github.com/Rowena-Gan/ITOM-6264/blob/7dc4cd7839afcca73daa49ab6c15090dde797c9a/assets/ipopt_bug_2024_spring/macos_libopenblas.png)

That tells we have 2 regular libraries installed that are part of the `libopenblas` project: `libopenblas.dylib` and `libopenlabsp-r0.3.21.dylib`. We will use the `libopenlabsp-r0.3.21.dylib` library.

### Step 3: Create a symlink for `libopenblas_vortexp-r0.3.21.dylib`

Now we are going to create a symlink that points `libopenblas_vortexp-r0.3.21.dylib` to `libopenlabsp-r0.3.21.dylib`. Details about how symlink works are beyond the scope of this class. If you're interested, please feel free to read more on [Wikipedia](https://en.wikipedia.org/wiki/Symbolic_link), [freecodecamp](https://www.freecodecamp.org/news/symlink-tutorial-in-linux-how-to-create-and-remove-a-symbolic-link/), or [Linux manual page](https://man7.org/linux/man-pages/man1/ln.1.html).

Practically, we need to run the following command to create the symlink we need. Note: *the file names that are needed on your machine might differ from this example. Make sure you follow the steps above and use the correct file names.*

```
ln -s libopenblasp-r0.3.21.dylib libopenblas_vortexp-r0.3.21.dylib
```

> [!IMPORTANT]  
> The file names that you should use for `ln -s` on your machine might differ from this example. Make sure you follow the steps above and use the correct file names.

Now, verify that you can run Class 3 worksheet. First, you need to ***shutdown your current Jupyter Notebook instance.***, and restart a new one. Then run class 3 worksheet.

# Start with A Clean Environment

If you still cannot run class 3 worksheet after trying the above, it is best that you try starting from a clean environment. 

Anaconda provides official documentation on how to uninstall: https://docs.anaconda.com/free/anaconda/install/uninstall/
