---
layout: post
title: "NRF52 Global Tech Tour"
date: 2015-11-10 11:52:14 +0100
author: Daniel Schnell
comments: true
categories: Bluetooth, BLE, Nordic, NRF52, NRF51, CC26xx, TI
---
{% img top /images/Global-Tech-Tour-2015_large.png 640 480 %}

Since BLE4.x has made its way into every modern mobile phone, there is a never-ending demand for new BLE4.xchips with better features at lower power to provide the "thing" functionality in IoT.

<!-- more -->

# Retrospective

## NRF51
{% img left https://www.nordicsemi.com/var/ezwebin_site/storage/images/news/news-releases/product-related-news/nordic-semiconductor-refreshes-its-nrf51-series-bluetooth-smart-ant-multiprotocol-socs-with-more-ram-and-ultra-compact-packaging-options/1096965-23-eng-GB/Nordic-Semiconductor-refreshes-its-nRF51-Series-Bluetooth-Smart-ANT-multiprotocol-SoCs-with-more-RAM-and-ultra-compact-packaging-options_full_article.jpg 320 200 'NRF51' %}

Nordic Semiconductor introduced the **Cortex M0-based** NRF51 line of wireless chips **in 2012**, which features foremost **BLE4.x** and **ANT** but also lesser known wireless protocols in the 2,4 GHz ISM band like **ESB** and **Gazell**.

The NRF51 has been improved over the years mainly by adding more Flash and SRAM **(32KB SRAM, 256KB Flash)** and is in this respect better than some other competitors, but falls short especially in terms of DMA support for peripherals and the number thereof. It runs on **16MHz** only, which makes it appear a little weak from todays view.

##TI CC26xx
{% img right http://www.ti.com/graphics/folders/partimages/CC2640.jpg 320 200 'TI CC2640' %}
In the beginning of 2015 Texas Instruments set the performance bar high by releasing to the public the CC26xx wireless micro controller. A **Cortex-M3** running at **48MHz** with **(20+8+2) kB of SRAM**, **128kB of Flash memory**, built-in **Cortex-M0 coprocessor** for sensor handling, **plenty of peripherals with DMA** and **state-of-the-art BLE4.x radio performance**. It is a chip optimized for lowest power with enough Oomph to run demanding protocols next to the usual wireless and sensor functionality. It can be used standalone and will run for years on a single CR2032 coin cell, though falling a bit short in RAM and Flash size.

## NRF52
{%img left https://www.nordicsemi.com/var/ezwebin_site/storage/images/news/news-releases/product-related-news/nordic-nrf52-series-redefines-single-chip-bluetooth-smart-by-marrying-barrier-breaking-performance-and-power-efficiency-with-on-chip-nfc-for-touch-to-pair/1479159-2-eng-GB/Nordic-nRF52-Series-redefines-single-chip-Bluetooth-Smart-by-marrying-barrier-breaking-performance-and-power-efficiency-with-on-chip-NFC-for-Touch-to-Pair_full_article.jpg 400 250 'NRF52832'%}

Nordic Semiconductor announced later in 2015 the specs of a new BLE4.x product: the **NRF52**. The NRF52 is the successor of the NRF51.

The NRF51 however will not be discontinued:

*"The NRF51 will still be produced for many many years. However development of new software features will be done for the NRF52"*

I wonder how the introduction of the TI CC26xx has influenced the pace at which Nordic has made the NRF52 appear on the market?

# NRF52 Global Tech Tour
{% img center /images/nrf52_global_tech_tour_demo.jpg 720 560 'Demo-Time' %}

Nordic promotes the NRF52 by currently running the NRF52 Global Tech Tour through the U.S., Europe and Asia. I had the opportunity to attend to a Global Tech Tour event in Munich to get first hands technical information about the NRF52 and insights into where Nordic is currenly setting its development focus.

The Tech Tour event takes a whole day and is aimed foremost at software and hardware developers. Fortunately it is also carried out from experts that are involved into soft- and hardware development at Nordic. There are numerous opportunities inside the presentations for Q&A and Nordic employees try their best to answer questions in the breaks as well. 

The material presented is stuffed with a lot of information on a basic to medium level and gives a good idea of how the NRF52 is designed and why some design decisions have been made. Also it enables a quick jumpstart for developers: they show initialization code, make a couple of demos and quizzes.

If one is into BLE4.x development and/or plans to make products with BLE, visiting a Nordic Global Tech Tour event is time well invested. There is no fee at all, you get a NRF52 DK development board, food and beverages are also served. In Europe the last opportunity was on November 09. in Amsterdam, but more locations in Asia are on the [roadmap](https://www.nordicsemi.com/eng/Events/Global-Tech-Tour-2015/Asia-Pacific) until mid December.

For those that cannot attend the Tech Tour, I am allowed to provide the slides as zip file [here](/assets/nordic_gtt/gtt.zip) (with kind permissions from Nordic)

## NRF52832 Features
The NRF52832 is the first chip in the NRF52 line and sets a new bar for BLE4.x chips in almost every area:

- **512 kB Flash**: 2 x better than NRF51
- **64kB SRAM**: 2 x better than NRF51
- **64MHz Cortex M4F**: whereas the TI CC26xx can run at different MHz values, the NRF52 is only able to run at 64MHz
- **38 uA/MHz**: in comparison to 275 uA/MHz of the NRF51 and 61 uA/Mhz of the TI (all running from Flash). This means you can run it on 64MHz with lower power than the TI with 48MHz or almost half the NRF51 power with 16MHz
- **2,2uA idle current** with RTC and 64KB RAM retention
- **BLE4.2 + ANT:** While the NRF51 family was segmented into chip versions featuring either BLE4.x + ANT (51422) or BLE4.x alone (51822), the NRF52 supports generally BLE4.x and ANT


It doesn't take the BLE4.x radio performance crown of the TI, but almost equalizes it:

- **-96dB/+4dBm** Rx sensitivity/Max Tx Output (TI CC26xx: -97dB/+5dBm)

These are the features I find especially noteworthy:

- **Automatic Power Management:** Each chip module requests the appropriate power regulator it needs automatically. The different regulators and modes are also chosen automatically. As a consequence, you cannot supply external circuits via e.g. setting a GPIO anymore. All the user needs for this to happen is to stop timers when not in use or stop the CPU when waiting for interrupts.
- **PPI/GPIOTE:** The programmable peripheral interconnect is like a NxM matrix where you can program N events to M tasks. An event can be internally synthesized or externally triggered via e.g. the push of a button. A task is the reaction to an event and can also be an internal functionality or setting a GPIO output to some value. The nice thing is that connections done like this can be handled without the CPU core waking up, resulting in reduced power consumption. The GPIOTE sits on top of that and makes it possible to choose almost any SoC pin for any available peripheral function. This feature was already present on NRF51, but read on ...
- **SPI / I2C / DMA:** One of the biggest shortcomings of the NRF51 was the missing DMA support for i2c/spi master mode. This made it necessary to use these peripherals in interrupt mode which wakes up the cpu core every time. In the NRF52 Nordic not only added DMA for all peripherals independant of slave or master mode, but did this in a very nice way: you can use a timer (e.g. RTC0) to periodically trigger rx/tx transfers on a serial device and use the PPI to automatically connect rx/tx sequences together. An additonal feature Nordic calls "autolog" increments buffer pointers for multiple DMA transfers. Via all those mechanisms it's possible to e.g. poll an external sensor continuously for its data without the need to wake up the cpu core for a single transfer at all !

These new features are in my eyes a nice-to-have but not essential:

- ** Touch-to-Pair:** Nordic hypes the "Touch-to-Pair" functionality of the NRF52 via its On-Chip NFC-A tag for Out-Of-Band BLE4.x pairing and this might have in the future a lot of appeal for the end user experience. But as long as it is not fully supported from Android and not at all supported from iOS, it's useless, because it means one has to provide an alternative OOB mechanism anyway.

- **PDM:** The newly added PDM device (PDM is a standard protocol for digital microphones) does not have a lot of practical meaning as long as the NRF52 can only achieve a few tens of kbaud over BLE4.x to get the sampled PCM waves out. Maybe if bigger BLE4.x MTU sizes are implemented by future Nordic soft devices, it will make more sense. Or one uses a different protocol like ESB or Gazell.

There are of course more features of the NRF52 that I have not covered here. Look at the [documentation](http://infocenter.nordicsemi.com/index.jsp) at the Nordic Info Center if you want to see more.


## NRF52832 Pricing
Nordic employees state that the price of the NRF52 is about 15% higher than the NRF51, though it has not been said which exact chip of the NRF51 family is the baseline for price comparisons. A sales manager of a major German distributor who also attended the Nordic Global Tech Tour told me numbers of around 2,70€/1k. Any Cortex M4F based microcontroller with similar RAM and Flash real estate is in the same ball park - at least.

Moreover due to the integrated balun of the NRF52 the overall BOM cost and complexity of a hardware design is further reduced in comparison to e.g. the TI CC2640 which does cost around 3,60€/1k and needs an external balun if one wants to use best radio performance in differential mode. One can use the TI in single ended mode, but then the overall radio performance drops to lower values than what is possible with the NRF52 (see PDF [CC26xx RF Frontends and Antennas]( https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&ved=0CCIQFjABahUKEwjt47_a6oDJAhWn_nIKHceMCvg&url=http%3A%2F%2Fprocessors.wiki.ti.com%2Fimages%2F4%2F45%2FCC26xx_HW_training_RF_Frontends_and_Antennas.pdf&usg=AFQjCNFTzqKPcoREu0zjTGDeMv7Wmjf4tQ&bvm=bv.106923889,d.bGQ&cad=rja))


## What's next ?
### MTU size
One of BLE4.x big shortcomings is the small maximum transfer unit size (MTU). The usable payload size per packet on a NRF51/NRF52 is 20 bytes. One can send multiple packets in a row without receiving acknowledgements from the peer. For a cycle time of 10ms this expands to ~44,8 kbaud. The problem here is the big overhead of the BLE4.x protocol. Increasing the payload size could considerably improve throughput. The radio of the NRF52 is already prepared for that and can be tuned to 256 bytes dynamic payload length. Let's see what of that Nordic provides in a future version of a NRF52 soft device.

In comparison the TI CC26xx is already able to transmit around 293 kbaud with [the same technique](http://processors.wiki.ti.com/index.php/CC26XX_BLE_Throughput).

### Internet of things _around us_
Nordic shared their IoT vision in their presentations. They expect the current paradigm of the "Internet of my things" to shift to the "Internet of things around us". What they mean by this, is that typical BLE4.x applications use point-to-point connections from e.g. a smart phone to the BLE4.x "thing". In the future, more and more environments will use IPv6-enabled BLE4.x devices in an ad-hoc network, which will be controlled by a router or bridge device so that a smart phone does not necessarily have to be around at all. 6LoWPAN is an open standard that Nordic adapts for this purpose to their BLE4.x chips NRF51/NRF52 via the IoT SDK. Whereas for the NRF51 the SDK [is already available](https://www.nordicsemi.com/eng/Products/Bluetooth-Smart-Bluetooth-low-energy/nRF51-IoT-SDK), a first release for the NRF52 is planned at the end of the year.

Here is Nordics architecture of the NRF52 6LoWPAN stack:

{% img center /images/nrf52-ipv6.tiff 'NRF52 ipv6 stack' %}

IPv6 brings a lot of new possibilities and complications to the "thing". What seems clear is that a device needs a lot more CPU processing power and Flash space if it comes to security protocols like TLS/SSL. Something in the realms of > 100kB Flash space. Bigger radio packets and MTU sizes are also required.

Nordic lays out a rough architecture for a 6LoWPAN router device for prototyping purposes like this:

{% img center /images/headless_router_raspi_ipv6.tiff  'headless router 6LoWPAN' %}

All components for the above router device are freely available. Nordic provides a Linux kernel image for a V1 Raspberry Pi (not for the new V2 Raspberry Pi).

# Tools for Development
Whoever closely follows the SoC market development in recent years, sees how silicon manufacturers move away from being simple hardware vendors to being providers of complete hard- and software stacks. Having data sheets, reference layouts, reference manuals, application notes et al available when introducing a new SoC are all important prerequisites for successful market adoption. But the increasing functionality and complexity of modern chips make it necessary that developers spend considerable time and efforts to acquire the specific knowledge for bringing this functionality into new products. Reducing that precious developers time to drive rapid product development is equally important as the feature set itself from a SoC perspective.

That is the reason why SoC vendors provide SDK's and ready-to-go projects to showcase their SoC functionality. To reduce the complexity for the SoC vendors they have to limit themselves to certain toolchains they fully support. 

## Toolchain
In the embedded world there is the gold standard toolchain GCC and Binutils. GCC is free and Open Source, constantly evolving and probably the most used compiler in the embedded market. With GCC & friends it's possible to program a linux system and most micro controllers available. There are other compilers and toolchains on the market - most of them commercial, but none of these have the broad coverage for different cpu's and microcontrollers as GCC does. A SoC vendor should therefore think twice before _requiring_ other toolchains than GCC.

In the ARM world however one can assume that the most efficient compiler on a compiled byte-by-byte basis is the official compiler from ARM itself. So for Nordic as SoC manufacturer who integrates solely ARM cores into their chips it would be stupid to not support the official ARM compiler as well.

Nordic has chosen to support 3 compilers: **GCC, ARM Keil and IAR**, where ARM Keil is their "main" supported toolchain.

## NRF52 DK

{% img center /images/nRF52DK.jpg 'My NRF52 DK sample'%}

The NRF52DK is the official NRF52833 development board. All attendees of the Global Tech Tour get one for free. It's not the NRF52 DK preview that is still sold from distributors. The one we've got has already the final hardware design. Being Arduino compatible, a lot of available shields can be used for prototyping.

The NRF52 DK comes with a built-in Segger JLink debugger. Seggers are the gold standard for debuggers in the ARM world. They support more or less all ARM chips out there and they are fast. Including the Segger into the NRF52 DK makes it possible to program and debug the final PCB with the development kit itself (and most other ARM boards out there ...). If you do ARM development and don't already own a Segger, with the NRF52DK you can have one for a bargain.

Unfortunatley the NRF52DK we've got contains also a buggy NRF52832 Engineering REV.B chip (NRF52832 QFAA-BA) which among other minor problems draws too much currents in the mA range. 

{% img center /images/nrf52832-eng-revb.jpg 'NRF52832 QFAA-BA: Engineering Rev.B'%}

Basing a power measurement on such a chip will not come close to what you can expect from a production type. We have been told by Nordic that these bugs will be fixed when mass production starts.
Exact errata infos can be found on their [infocenter page](http://infocenter.nordicsemi.com/index.jsp).

# Conclusions

The NRF52 is a nice product. It has most what you would expect from a state-of-the-art BLE4.x chip and if Nordic gets the errata's straight for the mass production units, they have a keeper. I am personally impressed by the whole package: hard- and software-wise. You can use Open-Source tools or likewise proprietary, whatever your needs and policies or of your employer.

The only real competitor is the TI CC26xx. One can expect that TI and others will react. TI has the man power and financial fundings for getting a feature-wise comparable product to the NRF52 out in a not so distant future. What I am not so sure is, if TI gets the software side straight until then.

*[rant on]*
One of the most annoying issues with TI products is their closed software world: for the CC26xx one needs the proprietary Code Composer Studio (CCS). Though it's free of charge, one has to register at TI's website with a company name and click through a lot of agreements for downloading their BLE4.x stack or their SDK's. CCS is based on Eclipse but there is no way one could simply grab Eclipse and download TI's CC26xx software packages in the official Eclipse market place. CCS is a complicated product with a TI proprietary compiler which is as of today the only supported compiler that can translate BLE4.x projects for the CC26xx. Moreover one has to use the proprietary TI RTOS as OS kernel for a BLE4.x project. A developer coming from a GCC world who would like to use, e.g. FreeRTOS is out of luck and has to learn a complete new development environment for mastering the CC26xx. This directly translates into loss of time-to-market. Here it doesn't end: CCS currently doesn't support any Seggers (or the other way around: Segger doesn't support CCS), only their own debuggers which in case of e.g. the XDS-100 is as slow as it gets. There is however support planned in a future release of CCS in [March 2016](https://www.segger.com/ti-code-composer-studio.html). If you can wait that long ...
*[rant off]*

I applaud Nordic's move for opening up the access for documentation and most SDK's on their [infocenter page](http://infocenter.nordicsemi.com/index.jsp). It was not long time ago, where they also used a similar procedure as TI today. It's good to see them supporting Open Source technology and that they design no artificial vendor lock-in around their products. This openness can be a big asset in the future, because developers like it and remember it.

The NRF52 Global Tech Tour was an interesting visit. As a software developer I am looking forward getting my hands dirty with BLE4.x development on the NRF52 DK.

