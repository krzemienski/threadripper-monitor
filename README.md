# Threadripper Monitor

Most system monitors on Linux have a hard time with 32 cores and 64 threads. This program, made especially for AMD Threadripper processors, displays the CPU use of all the cores (and threads), in a nice graphical format that matches the physical implementation of the cores.

## Dependencies

- Python 3
- PyQt5
- PyQtChart
- `rapl-read-ryzen`

The Python dependencies are installable with `pip`, and come pre-packaged in wheels. RAPL has to be installed from [its Github repository](https://github.com/djselbeck/rapl-read-ryzen).

## Installing RAPL

If it is not yet merged, be sure to use my branch of RAPL, available [here](https://github.com/steckdenis/rapl-read-ryzen). It fixes how SMT interacts with power consumption.

    git clone https://github.com/djselbeck/rapl-read-ryzen.git
    cd rapl-read-ryzen
    gcc -O2 -o rapl -lm ryzen.c
    sudo cp rapl /usr/bin
    sudo chown root:root /usr/bin/rapl
    sudo chmod +s /usr/bin/rapl

The last two lines make RAPL a set-uid root binary. **Be aware of this**, as it can have security implications. RAPL has to be run as root, because it has to access the CPU MSRs. There may be another way of allowing RAPL to be easily run as an user, for instance by changing permissions around, but I did not manage to do that as easily as making RAPL set-uid root.

## Running

Once RAPL and the Python dependencies, the monitor is super easy to use:

    python3 main.py

And you are greeted by a nice GUI with animated graphs of the CPU usage and power consumption. The main area of the window displays the 2 or 4 dies of your CPU, with their two CCXes, each having 2, 3 or 4 cores, each having 2 threads. Threads become darker blue as they are used. A red bar also displays the power usage of individual cores, in addition to the main graph that shows the total power use of the whole package, and all the cores combined.

## Caveats

- Currently, this program only runs on Linux. Reading system information (CPU use and power consumption) on Windows is much harder, and there are already plenty of programs that do that on Windows.
- The CPU topology is detected by reading `/proc/cpuinfo` and matching the name of the CPU. Future CPUs will need this program to be updated.