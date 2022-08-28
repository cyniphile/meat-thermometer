I recently bought a [wireless thermometer](https://www.amazon.com/Wireless-Thermometer-Injector-Upgraded-Grilling/dp/B084L86VTX/ref=asc_df_B084L86VTX/?tag=&linkCode=df0&hvadid=416720912294&hvpos=&hvnetw=g&hvrand=8648917547180588802&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9067609&hvtargid=pla-886773437227&ref=&adgrpid=90729935261&th=1) for my grill. The device has a probe hub and a receiver that receives information from the hub wirelessly (up to 500ft). I was wondering if I could hack the device to send wireless signals to my phone. 

There didn't seem to be any schematics or manuals online, so I decided to crack open the device with my Wowstick (yup) to at least get a basic idea of how it worked wirelessly. Maybe the frequency was compatible with normal wi-fi tech in some way? 

Here is a picture:

![](2022-08-14-14-50-13.png)

Board is labeled "BB7-子机_V2.2 FS-06B". 子机 means either "handset" or "slave" (characters mean "son machine" haha). No searches of Google or Baidu pulled up anything useful.

Let's just look at some of these components to get a feel for things, starting with the, >squint< 





## HOLTEK HT1621B

Googling...ok this is the [LCD controller](https://www.holtek.com/productdetail/-/vg/ht1621. There isn't much going on the other side of this PCB (besides the lcd), so for now I'm not going to remove it (the lcd), because I'm not sure it's necessary and I'm afraid of breaking things. 

## ANT1

On this is clearly some kind of antenna, but it isn't marked very much. I did a [google image search](https://www.google.com/search?q=wireless+antenna+component+500+ft+coil&tbm=isch&ved=2ahUKEwjz3v7ehMf5AhUbrXIEHVCWAG8Q2-cCegQIABAA&oq=wireless+antenna+component+500+ft+coil&gs_lcp=CgNpbWcQAzoECCMQJ1CfAliXB2DvCGgAcAB4AIABZIgB-gKSAQM1LjGYAQCgAQGqAQtnd3Mtd2l6LWltZ8ABAQ&sclient=img&ei=HUn5YrPcA5vaytMP0KyC-AY&bih=1016&biw=1792) that came up withe some pictures close to what I was looking for. 

[This](https://www.adafruit.com/product/4269?gclid=Cj0KCQjwuuKXBhCRARIsAC-gM0he67gxTymu_0_HUhDWDzFzUhCZhhFHXREGbrKEUA4TQSbaQrEvWIkaAqkfEALw_wcB) claims it's "850~950MHz"

There as section of capacitors and inductors that is probably doing some signal filtering. 

![](2022-08-15-17-56-57.png)

(?? I wonder if I could get data from the DATA chanel? Probably would need some kind of oscilloscope.)

More mysterious are the oscillator and nearby IC. 

![](2022-08-15-17-46-11.png)

The IC is marked "9A1i3". Maybe the oscillator is a clock for the IC, but seems odd because the IC is small and has few pins, making it seem like it would just be a couple logic gates something. 

## Tubular Component Thingy

Using my phone it got the marking to be YXC112GH, which appears to be some kind of resonator made by YXC (probably as a clock, maybe for one of the ICs?). Also, I couldn't find the exact part. 

## Med size chip

No markings or other labels on the pcb near it

## 

-----------------------

Ok, and nothing else is marked too well/seems that interesting. I'm guessing the other larger chip is the some kind of SoC for the temperature sensing? Also slightly curious about the

Seems like the 800-900mhz range is not going to be detectable on a phone without special hardware, e.g.: 

https://www.amazon.com/NooElec-NESDR-Mini-Compatible-Packages/dp/B009U7WZCA/ref=psdc_3012292011_t2_B073JWDXMG

But should do a bit more research to make sure. 

Could also us https://en.wikipedia.org/wiki/LoRa, though seems like too long a range. This is because the antenna was suggested for https://learn.adafruit.com/adafruit-rfm69hcw-and-rfm96-rfm95-rfm98-lora-packet-padio-breakouts/assembly and https://learn.adafruit.com/radio-featherwing

----------------

Reached out to company customer support via both email and facebook. Got this response

```
You sent
Hi thank you. I wanted to get some information about the communication protocol for your grill thermometer.
I'm working on building an app to track grill temperatures

Viminne
Hi, We are discussing this possibility with our factory engineers
I remember someone asked the same problem few years ago, but at that time there was no way to connect with app, it seems that it was because the transmission protocol did not match, I am now reconfirming with the factory

You sent
Yeah you need an external antenna i think for 900mhz
But I have that
But I'm not sure about the protocol
Thank you so much for your help

Viminne
The wireless frequency is: 915MHz.
I checked my email with that buyer and his plan is to read the transmission with a software-defined radio and store the data in a time-series influxdb database to present to a Grafana web UI. I don't know if that works or not

You sent
Yeah that was my plan too!
I was just wondering if I could get more specifics on the transmission that your device is sending out
For example if there was an encryption or error correction scheme I needed to be aware of

Viminne WM Zhang
ok, I am sending email to asking our factory now, but not sure if they will send us more infos, because they are OEM, Although we have an exclusive sales agreement with him, not sure if they will be willing to provide their technical content

You sent
Ok well thanks for asking. I'm just an open source developer

Viminne WM Zhang
I got this reply: this thermometer uses the wireless 433 transmission protocol, which does not support connecting to a mobile phone
```

------------------

Alex suggested [searching FCC](https://apps.fcc.gov/oetcf/eas/reports/GranteeSearch.cfm?calledFromFrame=N). Good idea, but nothing came up for the owners of these trademarks: https://uspto.report/TM/88187155, https://trademarks.justia.com/881/87/taimasi-88187155.html. There was a note that some devices don't need to be registered individually, which is usually the case when an FCC ID is not readily visible on the device. I found no FCC ID either on the device shell, on the circuit boards, or in the manual, though it does have an FCC stamp of approval.
