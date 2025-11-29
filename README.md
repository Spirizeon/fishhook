# minekit

C-based toolkit for spawning a revereshell userspace program through a kernel module trigger script
> ⚠️ **Disclaimer:** This code is intended for educational and lab use only.  
> Use it only on systems and networks you own or have explicit permission to test.



## Requirements

- A Linux environment
- `gcc` (or compatible C compiler)
- `make`
- (Optional) Linux kernel headers installed, if the project interacts closely with kernel modules

On a Debian/Ubuntu-like system, you can usually get these with:

```bash
sudo apt update
sudo apt install build-essential
````



## Building

To compile the project, run:

```bash
make
```

This will invoke the `Makefile` and produce the binaries (commonly named something like `rev` and `kmod_launcher` in the project directory).

To clean the build artifacts, run:

```bash
make clean
```

Refer to the `Makefile` to see the exact target names and any additional build options.



## Usage

### 1. Using the `launch` Script

The easiest way to run the project is through the provided `launch` script:

```bash
chmod +x launch
./launch
```

Depending on how the script is written, it may:

* Build the binaries if they don’t exist,
* Load or interact with the kernel module via `kmod_launcher`,
* Start the reverse helper (`rev`) with predefined arguments.

Check the script contents to adjust IP addresses, ports, or kernel module paths.


### 2. Running the Binaries Directly

You can also run the compiled binaries yourself.

#### `kmod_launcher`

Typical usage might look like:

```bash
./kmod_launcher [options]
```

Common patterns could include:

* Specifying the path to a kernel module,
* Setting device names or parameters,
* Triggering module load/unload or simple I/O.

Open `kmod_launcher.c` to see the exact expected arguments and behavior.

#### `rev`

Typical usage might look like:


`./rev [options]`


Depending on the implementation, this may:

* Connect back to a listener (IP/port),
* Exchange data with a remote endpoint,
* Act as a test harness for the module’s functionality.

Again, refer to `rev.c` for the specific arguments and protocol expected.

> 💡 If the programs implement `-h` or `--help`, you can try:
>
> ```bash
> ./kmod_launcher -h
> ./rev -h
> ```




## Troubleshooting

* **Build Fails**

  * Ensure `gcc` and `make` are installed.
  * Check the `Makefile` for hardcoded paths or flags that don’t match your environment.
  * Run with verbose output if needed, e.g. `make V=1` (if supported).

* **Permission Errors**

  * Some operations (like loading kernel modules or binding certain ports) require root:

    ```bash
    sudo ./kmod_launcher ...
    sudo ./launch
    ```

* **No Output / Hanging**

  * Verify that any remote listener (for a reverse connection) is running and reachable.
  * Double-check IP addresses and port numbers in the code and scripts.



