外围设备
----------------------------------------
将 QEMU 作为 PC 系统仿真器使用可提供各种外围设备。需要的标准外围设备包括硬件 Video Graphics Array (VGA) 仿真器、PS/2 鼠标和键盘、电子集成驱动器（Integrated Drive Electronics）硬盘和 CD-ROM 接口，以及软盘仿真。另外，QEMU 包括对 NE2000 Peripheral Controller Interconnect (PCI) 网络适配器、串行端口、大量的声卡和 PCI Universal Host Controller Interface (UHCI) Universal Serial Bus (USB) 控制器（带虚拟 USB 集线器）的仿真。Processor symmetric multiprocessing (SMP) 支持也得到了对 255 个 CPU 的支持。

除了仿真标准 PC 或 ISA PC（不带 PCI 总线）外，QEMU 还可以仿真其他非 PC 硬件，如 ARM Versatile 基线板（使用 926E）和 Malta million instructions per second (MIPS) 板。对于各种其他平台，包括 Power Macintosh G3 (Blue &amp; White) 和 Sun-4u 平台，都能正常工作。
