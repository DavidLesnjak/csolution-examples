# CubeMX Example

This *csolution project* shows the usage of [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html) which uses a [generator import file](https://open-cmsis-pack.github.io/cmsis-toolbox/YML-CBuild-Format#generator-import-file) to obtain the device configuration and HAL driver source files.

This project uses two different target-types to configure execution from Flash ROM or execution from RAM. It uses a [variable](https://open-cmsis-pack.github.io/cmsis-toolbox/YML-Input-Format#variables) to define the regions header file for each target in the file [`CubeMX.csolution.yml`](./CubeMX.csolution.yml).

## Prerequisites

### Tools

- [CMSIS-Toolbox 2.6.0](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/releases) or higher
- [Keil MDK 5.40](https://www2.keil.com/mdk5/) or higher
- [STM32CubeMX 6.11](https://www.st.com/en/development-tools/stm32cubemx.html) or higher

>**Note:**
>
> This example uses the Arm Compiler 6 (AC6). [Using GCC Compiler](#using-gcc-compiler) explains how to configure for different compiler. A similar process also works for IAR.

### Packs

- Required packs are listed in the file [`CubeMX.csolution.yml`](./CubeMX.csolution.yml)

## Build Project

This command builds the project and downloads missing packs. If the file `CubeMX.cbuild-set.yml` is missing it creates this file with the context-set selection of the first `target-type` and the first `build-type`.

```bash
> cbuild setup CubeMX.csolution.yml --context-set --packs
```

## Run CubeMX

To change the device configuration, start STM32CubeMX using the following command.

```bash
> csolution CubeMX.csolution.yml run --generator CubeMX --context-set
```

For using CubeMX refer to the documentation
[**CMSIS-Toolbox > Configure STM32 Devices with CubeMX**](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/tree/main/docs/CubeMX).

The source code generated by CubeMX is imported using the file [STM32CubeMX\<target-type>\CubeMX.cgen.yml](./STM32CubeMX/MyBoard_ROM/CubeMX.cgen.yml). This allows you to create different settings for each target, for example evaluation board and custom hardware.

>**Note:**
>
> Refer to the pack overview page, for example the [STM32F4_DFP](https://www.keil.arm.com/packs/stm32f4xx_dfp-keil) for information on how to use an IDE workflow with CubeMX.

## Execute Project

You may use the debugger of choice for executing this program.

## Using GCC Compiler

By default this project is the Arm Compiler 6 (AC6). Using STM32CubeMX it can be reconfigured for the GCC or IAR compiler. To configure it for the GCC compiler execute these steps:

- In the file [`CubeMX.csolution.yml`](./CubeMX.csolution.yml) set `compiler: GCC`.

- Launch the STM32CubeMX generator with this CMSIS-Toolbox command: `csolution <solution_name>.csolution.yml run -g CubeMX -c <context>`

- In STM32CubeMX:
    - Open from the menu Project Manager - Project - Toolchain/IDE:
    - Select STM32CubeIDE and disable Generate Under Root.
    - Click GENERATE CODE to recreate the CubeMX generated files for the GCC compiler.

In the file [CubeMX.cproject.yml](./CubeMX.cproject.yml), update `linker:` node configuration to reference appropriate [GCC linker script template](https://open-cmsis-pack.github.io/cmsis-toolbox/build-overview#linker-script-templates). You may customize this GCC linker script template file to your project's requirements.

[Rebuild the project](#build-project) using the CMSIS-Toolbox command `cbuild` with the option `--rebuild`.
