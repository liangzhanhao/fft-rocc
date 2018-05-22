# fft-rocc
8 point FFT and IFFT RoCC as a patch for rocket-chip, and simple test program as a patch for rocket-rocc-examples


The Compatible rocket-chip commit: 18e3bf37012967cfe3f8da8e3e8fa07bbbb52f33

The Compatible rocket-rocc-examples commit: cc86e29b4f7adcc277a5fe6f65fd448a606bbeac

## Get the rocket-chip commit

To switch rocket-chip to commit 18e3bf3:

	$ git fetch --all
	$ git reset --hard 18e3bf3
	$ git submodule update --init --recursive

## Get the rocket-rocc-examples repo

The rocket-rocc-examples repo is an environment for testing example RoCCs in rocket-chip. It is recommended to write test programs for your custom RoCC. To get the rocket-rocc-examples repo (As this repo hasn't been update over 3 months, the directly cloned repo is what we want):

	$ git clone https://github.com/seldridge/rocket-rocc-examples.git
	$ git submodule update --init

## Patch rocket-chip

	$ cd $ROCKETCHIP_DIR
	$ git apply $THIS_REPO_DIR/rocket-chip.patch

## Patch rocket-rocc-examples

	$ cd $ROCKET_ROCC_EXAMPLES_DIR
	$ git apply $THIS_REPO_DIR/rocket-rocc-examples.patch
	$ cd $ROCKET_ROCC_EXAMPLES_DIR/rocc-software
	$ git apply $THIS_REPO_DIR/xcustom.patch

## Build the FFT project

	$ cd $ROCKETCHIP_DIR/emulator
	$ make CONFIG=FFTACCConfig

## Build the FFT test program

	$ cd $ROCKET_ROCC_EXAMPLES_DIR
	$ autoconf
	$ mkdir build && cd build
	$ ../configure --with-riscvtools=$ROCKETCHIP_DIR/riscv-tools
	$ make

## Patch Proxy Kernel (pk) and rebuild Proxy Kernel

	$ cd $RISCV_PK_DIR
	$ git apply $ROCKET_ROCC_EXAMPLES_DIR/patches/riscv-pk.patch

	$ cd $ROCKETCHIP_DIR/riscv-tools
	$ ./build-spike-pk.sh

## Run test program

	$ cd $ROCKETCHIP_DIR/emulator
	$ ./emulator-freechips.rocketchip.system-FFTACCConfig pk $ROCKET_ROCC_EXAMPLES_DIR/build/pk/examples-pk-fftacc

## Expected result
	$ ./emulator-freechips.rocketchip.system-FFTACCConfig pk /home/liangzh/Desktop/rocket-rocc-examples/build/pk/examples-pk-fftacc
	This emulator compiled with JTAG Remote Bitbang client. To enable, use +jtag_rbb_enable=1.
	Listening on port 33627
	prireal = 0, priimag = 12 
	prireal = 0, priimag = 1 
	prireal = 0, priimag = 1 
	prireal = 0, priimag = 1 
	prireal = 7, priimag = 1 
	prireal = 0, priimag = -8 
	prireal = 0, priimag = 1 
	prireal = 0, priimag = 1 

	retreal = 7, retimag = 0 
	retreal = -7, retimag = 0 
	retreal = 7, retimag = 0 
	retreal = -7, retimag = 0 
	retreal = 7, retimag = 0 
	retreal = -7, retimag = 0 
	retreal = 7, retimag = 0 
	retreal = -7, retimag = 0 




