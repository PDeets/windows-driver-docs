---
title: Support for DirectMusic Properties
description: Support for DirectMusic Properties
ms.assetid: f44cf9a9-bdfc-4794-b064-4d1126fb83aa
keywords:
- hardware acceleration WDK audio
- miniport drivers WDK audio , kernel-mode hardware acceleration
- synthesizers WDK audio , kernel-mode hardware acceleration
- synthesizers WDK audio , property support
- IKsControl interface
- property-set GUIDs WDK audio
ms.author: windowsdriverdev
ms.date: 04/20/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# Support for DirectMusic Properties


## <span id="support_for_directmusic_properties"></span><span id="SUPPORT_FOR_DIRECTMUSIC_PROPERTIES"></span>


A DirectMusic-synthesis miniport driver specifies its hardware capabilities in the form of an array of property items. Each property item is a [**PCPROPERTY\_ITEM**](https://msdn.microsoft.com/library/windows/hardware/ff537722) structure that contains the following:

-   A property set GUID that defines a particular hardware feature that is defined by DirectMusic.

-   A pointer to a property-handler method that is implemented within the driver.

-   A set of flags that specify whether the handler can get the property, set the property, and indicate basic support for the property.

### <span id="property_set_guids"></span><span id="PROPERTY_SET_GUIDS"></span>Property Set GUIDs

The following property-set GUIDs are defined by DirectMusic:

-   GUID\_DMUS\_PROP\_DLS1

-   GUID\_DMUS\_PROP\_DLS2

-   GUID\_DMUS\_PROP\_Effects

-   GUID\_DMUS\_PROP\_GM\_Hardware

-   GUID\_DMUS\_PROP\_GS\_Capable

-   GUID\_DMUS\_PROP\_GS\_Hardware

-   GUID\_DMUS\_PROP\_INSTRUMENT2

-   GUID\_DMUS\_PROP\_LegacyCaps

-   GUID\_DMUS\_PROP\_MemorySize

-   GUID\_DMUS\_PROP\_SampleMemorySize

-   GUID\_DMUS\_PROP\_SamplePlaybackRate

-   GUID\_DMUS\_PROP\_SetSynthSink

-   GUID\_DMUS\_PROP\_SynthSink\_DSOUND

-   GUID\_DMUS\_PROP\_SynthSink\_WAVE

-   GUID\_DMUS\_PROP\_Volume

-   GUID\_DMUS\_PROP\_WavesReverb

-   GUID\_DMUS\_PROP\_WriteLatency

-   GUID\_DMUS\_PROP\_WritePeriod

-   GUID\_DMUS\_PROP\_XG\_Capable

-   GUID\_DMUS\_PROP\_XG\_Hardware

For definitions of the preceding property set GUIDs, see the description of the [**KSPROPERTY**](https://msdn.microsoft.com/library/windows/hardware/ff564262) structure in the DirectX 8.0 Programmer's Reference in the Microsoft Windows SDK. The property set for each of the preceding GUIDs consists of a single element that is identified by an index of zero.

### <span id="ikscontrol_interface"></span><span id="IKSCONTROL_INTERFACE"></span>IKsControl Interface

The [IKsControl](https://msdn.microsoft.com/library/windows/hardware/ff559766) interface is used to get, set, or query for basic support of properties, events, and methods. This interface is part of the WDM kernel streaming (KS) architecture, but is also used by DirectMusic to expose properties of DirectMusic ports. To retrieve this interface, call the **IDirectMusicPort::QueryInterface** method (described in the Windows SDK documentation) with the *riid* parameter set to **IID\_IKsControl**.

The [IKsControl](https://msdn.microsoft.com/library/windows/hardware/ff559766) interface has three methods: [**KsProperty**](https://msdn.microsoft.com/library/windows/hardware/ff559792), [**KsEvent**](https://msdn.microsoft.com/library/windows/hardware/ff559772), and [**KsMethod**](https://msdn.microsoft.com/library/windows/hardware/ff559785). At present, only **KsProperty** is supported by DirectMusic.

The [**IKsControl::KsProperty**](https://msdn.microsoft.com/library/windows/hardware/ff559795) method gets or sets the value of a property. The manner in which a property item request is routed to a particular DirectMusic port depends on how the port is implemented:

-   No properties are supported by ports that represent DirectMusic emulation on top of the Microsoft Win32 handle-based multimedia calls (the midiOut and midiIn APIs). Use the **GUID\_DMUS\_PROP\_LegacyCaps** property set GUID to query a port for whether it is implemented with Win32 multimedia calls.

-   Property item requests to a port that represents a pluggable software synthesizer are handled entirely in user mode. The topology of this type of port is a synthesizer (represented by an [IDirectMusicSynth](https://msdn.microsoft.com/library/windows/hardware/ff536519) interface) that is connected to a sink node (an [IDirectMusicSynthSink](https://msdn.microsoft.com/library/windows/hardware/ff536520) interface). The property request is given first to the synthesizer node, and then to the sink node if it is not recognized by the synthesizer.

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[audio\audio]:%20Support%20for%20DirectMusic%20Properties%20%20RELEASE:%20%287/18/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")


