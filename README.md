Example project using the lattice ecp5 evaluation board and litex (using lxbuildenv).

Steps to reproduce (without this repo):
- prepare the evaluation board as described in litex_boards/platforms/lattice_ecp5_evn.py
  - modify ecp5 evaluation board by removing R22 and R23 and installing a 0 ohm link on R34 and R35
  - program the ftdi chip to use uart as https://github.com/trabucayre/fixFT2232_ecp5evn
- build your soc
  - download lxbuildenv.py
  - create project files and provide name with: python3 lxbuildenv.py --init
  - edit name.py (with the name you supplied) to look like ecp5_lxbuildenv_example.py
  - build the soc using: python3 name.py --build --integrated-main-ram-size 0x3000
- build your demo software
  - navigate to the demo project: cd deps/litex/litex/soc/software/demo
  - build the demo with: python3 demo.py --build-path ../../../../../../build/name
- upload gateware and software to board
  - navigate back to main directory
  - upload you soc: python3 name.py --load
  - connect to board and upload program using litex_term: ./bin/litex_term /dev/ttyUSB2 --kernel=deps/litex/litex/soc/software/demo/demo.bin
  - issue a reboot command and watch for any CRC errors, when the demo terminal connects go rotate your donut

Some notes on the above steps:
- replace name with whatever you named your project
- if building the demo complains that main_ram is missing, verify you used the integrated-main-ram-size argument in the soc build step
- when connecting with litex_term the device might appear as a different device not at /dev/ttyUSB2
