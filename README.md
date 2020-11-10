# micropython-i2s-driver

This repository adds I2S support to MicroPython for the ESP32 family. The code was taken from the [PR](https://github.com/micropython/micropython/pull/4471), and was updated and modified to include the I2S-PDM mode. This mode is required by some MEMS microphones e.g. SPM1423.

I could have forked the micropython repository and include the I2S driver. However, I chose to include in this repository only the I2S files that are required, so that you can always use the last version of MicroPython and add these files to add I2S support.

# DIY
To include I2S support to MicroPython, you need to compile the firmware from scratch. To do that follow these steps:
1. Clone the MicroPython repository:
    ```
    git clone --recursive https://github.com/micropython/micropython.git
    ```
2. Copy the files of this repository inside the folder `ports/esp32`. 

:warning: If you want to directly replace the original files with the provided in this repository, be sure that you've taken the same commit hash. MicroPython changes a lot, and you'll compiling issues if you ignore this warning.

If you don't want to replace the files `modmachine.c`, `modmachine.h` and `Makefile`, make the following modifications to the originals:
    * `modmachine.c`: add the line
    ```
        { MP_ROM_QSTR(MP_QSTR_I2S), MP_ROM_PTR(&machine_hw_i2s_type) },
    ```
    in the code block below `// wake abilities` (approx. line 256)

    * `modmachine.h`: add the line
    ```
    extern const mp_obj_type_t machine_hw_i2s_type;
    ```
    in the code block declaring the `extern const` variables. 

    * `Makefile` add the line:
    ```
    	machine_i2s.c \
    ```
    inside the difinition of `SRC_C = \` (approx. line 323)
3. Compile and deploy MicroPython following the instructions from this [tutorial](https://lemariva.com/blog/2020/03/tutorial-getting-started-micropython-v20).


I've included a compiled version (using IDF4.x) of the firmware inside the `firmware` folder ([b7883ce](https://github.com/micropython/micropython/commit/b7883ce74c5a9b9689d812d134117d625fd42e73)). You can deploy this version following the instructions of the [tutorial](https://lemariva.com/blog/2020/03/tutorial-getting-started-micropython-v20). The firmware has support for I2S and BLE, but it doesn't have for PSRAM. If your ESP32 has a PSRAM chip, I recommend that you compile the firmware from scratch following the above steps.
