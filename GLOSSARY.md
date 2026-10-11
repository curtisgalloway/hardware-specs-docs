<!--
SPDX-FileCopyrightText: 2026 contributors
SPDX-License-Identifier: CC-BY-4.0
-->

# Hardware terms

- **GIC:** Arm Generic Interrupt Controller; routes interrupt requests to processors.
- **Distributor / redistributor:** GIC components that configure shared interrupts / processor-local interrupts.
- **SPI / PPI / INTID:** shared peripheral interrupt / private peripheral interrupt / architectural interrupt number. Here SPI is an interrupt class, not the serial peripheral bus.
- **CRU:** clock and reset unit; selects and gates clocks and controls reset signals.
- **Mux / pin control:** selection of the signal function carried by a physical pin.
- **GPIO bank:** a group of general-purpose input/output pins sharing a controller.
- **High-word write mask:** an update convention where upper write bits select which lower bits change; applicability is specific to the cited register/helper.
- **AArch64:** the 64-bit execution state of an Arm processor, with its instruction set and registers.
- **System register:** a processor register accessed by instructions rather than a memory-mapped address.
- **MMIO:** memory-mapped input/output; device registers reached through addresses.
- **Firmware handoff:** the processor and device state left when boot firmware transfers execution.

- **Interface identity / implementation revision:** the programming interface a block exposes / the particular hardware version implementing it. An interface name alone does not establish a hardware revision.
- **UART:** universal asynchronous receiver/transmitter; a serial interface for sending and receiving bytes.
- **IOC:** input/output controller; the register domains used by the cited pin-control implementation.
- **ITS:** interrupt translation service; the GIC component that translates message-based interrupts.
- **SGI:** software-generated interrupt; an interrupt requested by software for a processor.
- **PE:** processing element; an individual processor execution context served by a GIC CPU interface.
- **Baud:** serial symbol rate per second; the UART settings here use one bit per symbol.

Spec format terms are defined in the [driver-lab glossary](https://github.com/curtisgalloway/driver-lab/blob/main/GLOSSARY.md).
