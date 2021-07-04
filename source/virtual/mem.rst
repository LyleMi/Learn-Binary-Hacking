内存虚拟化
========================================

地址类型
----------------------------------------
- Guest虚拟地址 (Guest Virtual Address, GVA)
- Guest物理地址 (Guest Physical Address, GPA)
- Host虚拟地址 (Host Virtual Address, HVA)
- Host物理地址 (Host Physical Address, HPA)

EPT(Extended Page Table)
----------------------------------------
VT-x 提供了EPT技术，直接在硬件上支持了GVA->GPA->HPA的两次地址转换，降低了内存虚拟化的难度，提高了性能。
