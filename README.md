This is a boxing repository, including the gem5 repository, the nvmain repository and the runtime system implementation. The gem5 and nvmain are forked and slightly modified, to enable the BUS_ACCESS_ST performance counter of ARMv8 and make gem5 compile together with nvmain. Follow the subsequent instructions to checkout, compile and run the entire simulation.

# Checkout

First, checkout this repository and change to the drectory:

`cd NVMSimulator`

Next, pull the subrepos:

`git submodule init`

`git submodule update`

# Compile gem5

Everything should be there to compile gem5 now. Pleas take care of the several dependencies and make sure the necessary software is installed on your machine. Known so far: `build-essential scons boost`

Switch to the gem5 folder:

`cd gem5`

Compile gem5 together with NVMain

`scons EXTRAS=../nvmain ./build/ARM/gem5.fast`

In case you run into syntax error trouble with python3, force the compilation process to use python2:

`SCONS_LIB_DIR=/usr/lib/python3.7/site-packages python2 \` which scons\`  -j 8 EXTRAS=../nvmain ./build/ARM/gem5.fast`

When the compilation completes, the gem5 is ready to use.

# Setting up a simulation

First, you need to compile the runtime system together with the application, this requires a ARM64 cross-compiler. We recommend to use the `aarch64-linux-gnu-gcc` for this purpose.

Switch to the runtime system:

`cd nvm-rt`

The target application is found in the folder `application` and can be modified there. Several test applications can be found in the `application.old` folder and can be copied to the `application` folder.

Once the application is setup properly, the runtime system can be compiled by typing

`make clean; make`

The simulation can be directly started by typing:

`make run`

Keep in mind, that the full system simulation makes output to a simulated UART device, thus debug outputs are only visible in the simulated UART output. To monitor this output, type in another terminal:

`cd gem5`

`tail -f m5out/system.terminal`

If the simulation exits properly, the memory trace should be copied to

`nvm-rt/build/trace.nvt`

If you cancel the simulation, the unfinished trace can be found at

`nvmain/Config/nvmain.nvt`

# Analyzing the tracefile

In the subfolder `analysis` you can find some scripts, which perform a write aggregating analysis of the memory trace. You need an installation of `R` with the packages `data.table` and `tikzDevice`. Note that your system requires a fully functioning LaTeX installation for this.

Source the `plot.R` and call the do_plot or do_plot_log functions.