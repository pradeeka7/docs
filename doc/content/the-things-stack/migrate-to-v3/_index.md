---
title: Migrate to The Things Stack Community Edition
weight: 100
aliases:
 - /the-things-stack-v3/migrate-to-v3
---

This guide was written to explain the overall migration process of migrating from The Things Network V2 to The Things Stack Community Edition in a several easy-to-follow steps.

Let us assume that your gateway and device are still connected to The Things Network V2. In this section, we will first consider migrating your end device, then your gateway. You will also need to add your applications, integrations and payload formatters to The Things Stack Community Edition.

> We highly recommend to test the migration on a single end device or a small batch of end devices, just to make sure the migration process is succesfull and that you are getting an expected result.

> Migrating a single end device can be easily done through The Things Stack Community Edition Console. Migrating end devices in bulk can be performed with the `ttn-lw-migrate` tool.

#### Video Migrate from The Things Network V2 to The Things Stack
<iframe width="560" height="315" src="https://www.youtube.com/embed/JUEJ_9LdnuI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Log in to The Things Stack Community Edition

To be able to continue following steps below, you first need to log in to The Things Stack Community Edition by <a href="https://console.cloud.thethings.network/" target="_blank">selecting a cluster that is closest to you geographically</a>. You can do so by logging in with your The Things Network credentials via The Things ID.

## Add an application in The Things Stack Community Edition

To migrate your end device, you first need to <a href="https://www.thethingsindustries.com/docs/integrations/adding-applications/" target="_blank">add an application</a> on the The Things Stack Community Edition. The `Application ID` does not neccessarily have to be the same as the one in The Things Network V2.

## Add your payload formatters and integrations

After adding an application in The Things Stack Community Edition, you also need to re-add the associated elements, like payload formatters (known as coders and decoders in The Things Network V2) and integrations. The concept remained the same as in The Things Network V2, with slight improvements. 

> The format of payload coders and decoders from the The Things Network V2 is still supported in The Things Stack Community Edition.

See more info on how to <a href="https://www.thethingsindustries.com/docs/integrations/payload-formatters/" target="_blank">write payload formatters</a> and <a href="https://www.thethingsindustries.com/docs/integrations/adding-integrations/" target="_blank">add integrations</a> in The Things Stack Community Edition. 

Next, you need to migrate your end device(s). Depending on if it is one or more devices, choose the right method for migration.

## Method 1: Migrate an end device using The Things Stack Community Edition Console

### Add an end device in The Things Stack Community Edition

First, <a href="https://www.thethingsindustries.com/docs/devices/adding-devices/" target="_blank">add a device</a> in The Things Stack Community Edition. You can do it manually for both ABP and OTAA devices. For OTAA devices, you can also do it by submitting the type of your device if it is available in the <a href="https://thethingsindustries.com/docs/integrations/payload-formatters/device-repo/" target="_blank">Device Repository</a>. 

When manually adding an OTAA device, make sure you follow these steps correctly:

- Select **Over the air actiovation (OTAA)**
- Choose **LoRaWAN version** `MAC V1.0.2` (this is probably your version, since this has been the most stable and most commonly used version for a long time)
- Create an **End device ID** (does not have to match the `Device ID` in The Things Network V2)
- Enter your end device's **AppEUI** and **DevEUI** (these have to be the same as the ones in The Things Network V2)
- Select your frequency plan
- Select **Regional Parameters version** `PHY V1.0.2 REV B` (this is most probably your version)
- **Do not** check the boxes **Supports class B/C**
- **Advanced settings** probably do not have to be set
- Enter your end device's **AppKey** (has to match the one in The Things Network V2)

> We highly recommend using OTAA! See <a href="https://www.thethingsindustries.com/docs/devices/abp-vs-otaa/" target="_blank">why OTAA is better than ABP</a>.

If you have a **really good reason** to use ABP, you can add an ABP device to The Things Stack Community Edition by following next steps:

- Select **Activation by personalization (ABP)**
- Choose **LoRaWAN version** `MAC V1.0.2` (this is probably your version, since this has been the most stable and most commonly used version for a long time)
- Create an **End device ID** (does not have to match the `Device ID` in The Things Network V2)
- The **DevEUI** field is optional
- Select your frequency plan
- Select **Regional Parameters version** `PHY V1.0.2 REV B` (this is most probably your version)
- **Do not** check the boxes **Supports class B/C**
- Enter your device's **DevAddr** and **NwkSKey** (these have to be the same as the ones in The Things Network V2)
- **Advanced settings** must be set on registration (beware that changing these settings might not work later)
    - Set **Frame counter width** to `32 bit` (this is probably your value)
    - Set **RX1 Delay** to `1` (set in seconds)
    - Set **RX1 Data Rate Offset** to `0`
    - Your device probably resets frame counters, so check the **Resets Frame Counters** box
    - Set **RX2 Data Rate Index** to `3` if your frequency plan is EU868 
    - Set **RX2 Frequency** to `869525000` if your frequency plan is EU868
    - Set **Factory Preset Frequencies** for EU868 devices to `868100000, 868300000, 868500000` for all devices, or to `867100000, 867300000, 867500000, 867700000, 867900000, 868100000, 868300000, 868500000` for 8-channel devices

### Prevent your end device from performing a join to The Things Network V2

When you have registered your device in The Things Stack Community Edition, you should prevent your device from joining The Things Network V2. 

For OTAA device, you can simply do it by deleting your device from The Things Network V2, however, this is **not recommended** - you might lose some data. The recommended practice is to change the `AppKey` in The Things Network V2. This way, your existing session would not be terminated yet, but new Join requests sent by your end device would not be accepted by The Things Network V2 cluster. 

> If the migration process does not go as expected, you can return the old value of your `AppKey` and reconnect it to The Things Network V2.

For ABP device, you will have to delete it from The Things Network V2, since these devices can establish only one session during their lifetime. Having the same session in two places would introduce serious conflicts.

### Force your OTAA end device to perform a join to The Things Stack Community Edition network

Your OTAA end device will now need to perform a new join to The Things Stack Community Edition. 

How to do this is largely depends on your device's firmware:

- If your end device does a new join occasionally (every week, month or so), you can only wait for it to happen
- If your end device lets you send a downlink message to trigger a new join - send the command from The Things Network V2 Console
- If your end device does a new join when it loses the connection - delete that device from The Things Network V2 Console
- If your end device does a new join when it is power cycled - power cycle it 

Since you have not migrated your gateway from The Things Network V2 yet, the new Join request sent by your end device will be received by The Things Network V2 cluster. This request will not be accepted if you changed your end device's `AppKey` (as recommended above) or if you have deleted it.

Interesting thing is that your end device's new Join request will be received by The Things Stack Community Edition cluster too! You can thank for this to <a href="https://www.thethingsindustries.com/docs/reference/peering/#packet-broker" target="_blank">Packet Broker</a> (you can verify this by observing the uplinks metadata in The Things Stack Community Edition Console). Now, The Things Stack Community Edition cluster will accept this Join request, so your end device will get a new `DevAddr`, channel settings and other MAC parameters. Based on a newly assigned `DevAddr`, Packet Broker will route the traffic to The Things Stack Community Edition network.

### What to do with ABP end device?

If you are using an ABP device, that means your end device is still associated with the The Things Network V2 `DevAddr`, so the Packet Broker will not be able to route its traffic to The Things Stack Community Edition cluster if you have not connected your gateway to The Things Stack Community Edition. In order to route this traffic correctly, follow the steps in **Migrate your gateway** section at the end of this page.

## Method 2: Migrate end devices using the migration tool

To complete the migration this way, you need to have  <a href="https://github.com/TheThingsNetwork/lorawan-stack-migrate" target="_blank">`ttn-lw-migrate` tool</a>. 

`ttn-lw-migrate` is used to export end devices and applications from The Things Network V2 cluster to a <a href="https://www.thethingsindustries.com/docs/getting-started/migrating/device-json/" target="_blank">JSON file</a>. This JSON file can later be <a href="https://www.thethingsindustries.com/docs/getting-started/migrating/import-devices/" target="_blank">imported</a> in The Things Stack Community Edition via Console or via CLI.

### Devices have to rejoin The Things Stack Community Edition

Before you start migrating your devices, it is good to know a few things about migrating active sessions. 

Starting from The Things Stack version `v3.12.0`, for certain deployments it is possible to migrate active sessions as well. Migrating an active session means that OTAA devices will not have to perform a new join on the The Things Stack network. 

{{< warning >}} Unfortunately, migrating active sessions is **not available for The Things Network V2 and The Things Stack Community Edition**. This means that, if you are using these deployments, your device will have to perform a new join on The Things Stack Community Edition network.

The reason for this is that The Things Stack Community Edition is using a different `DevAddr` block than the one The Things Network V2 is using.

Migrating active sessions is available only for The Things Industries V2 (V2 SaaS) customers migrating to The Things Stack Cloud and other paid offerings of The Things Stack. {{</ warning >}}

### Configure the environment

After installing the migration tool, you need to configure the environmental variables:

```bash
$ export TTNV2_APP_ID="ttn-v2-application-ID"
$ export TTNV2_APP_ACCESS_KEY="access-key-of-your-ttn-v2-application" 
$ export FREQUENCY_PLAN_ID="EU_863_870_TTN"
```

> See <a href="https://www.thethingsindustries.com/docs/reference/frequency-plans/" target="_blank">supported frequency plans</a> list, and adjust the `FREQUENCY_PLAN_ID` according to your setup.

If you own a private The Things Industries V2 cluster, you can still use the migration tool, but with an extra variable configured:

```bash
$ export TTNV2_DISCOVERY_SERVER_ADDRESS="<instance-id>.thethings.industries:1900"
```

> If the Discovery Server of your private The Things Industries V2 cluster does not use TLS, you will need to use the `ttnv2.discovery-server-insecure` flag when running the `ttn-lw-migrate` tool.

### Export a single end device from The Things Network V2 

Before exporting an end device from The Things Network V2, test the export with:

```bash
$ ttn-lw-migrate devices --dry-run --verbose --source ttnv2 "v2-end-device-ID" > devices.json
```

If this goes without errors, use the following command in order to export the end device for real and clear the security keys:

```bash
$ ttn-lw-migrate device --source ttnv2 "v2-end-device-ID" > devices.json
```

### Export more than one end device from The Things Network V2

To export more than one device, create a file named `device_ids.txt` that will contain one `Device ID` per line:

```
dev1
dev2
dev3
```

Then, test the export with:

```bash
$ ttn-lw-migrate devices --dry-run --verbose --source ttnv2 < device_ids.txt > devices.json
```

If this goes without errors, use the following command in order to export end devices:

```bash
$ ttn-lw-migrate device --source ttnv2 < device_ids.txt > devices.json
```

### Export all end devices associated with your The Things Network V2 application

To export all devices that your The Things Network V2 application contains to a file named `all-devices.json`, use the following commands:

```bash 
$ ttn-lw-migrate application --verbose --dry-run --source ttnv2 "ttn-v2-application-ID" > all-devices.json # testing export - make sure it goes without errors
$ ttn-lw-migrate application --source ttnv2 "ttn-v2-application-ID" > all-devices.json
```

### Import end device(s) in the The Things Stack Community Edition application

When you have exported your end devices to a JSON file (let's use `devices.json`), you can import those devices in The Things Stack Community Edition application via Console or via CLI. 

If you want to import via Console, just head over to your The Things Stack Community Edition application and click the **Import end devices** button available under **End devices**. Choose **The Things Stack JSON** format and upload the `devices.json` under **File**. Hit the **Create end devices** button, et voilà! 

If you want to use the CLI to import end devices, just run the following command:

```bash
$ ttn-lw-cli end-devices create --application-id "v3-application-id" < devices.json
```

> Which ever method you are using for importing end devices, in case of any failure you will see a relevant error message.

### Adjust MAC settings for imported devices in The Things Stack Community Edition

If you have migrated an OTAA end device and your device performed a new join to the The Things Stack Community Edition, the MAC settings will be automatically configured correctly by the Network Server. 

Hovewer, if you have migrated an ABP device, you will have to configure the MAC settings manually. 

This is needed because some recommended settings used by The Things Network V2 and The Things Stack Community Edition Network Server are different. For example, `RxDelay` parameter used by the The Things Network V2 is 1 second by default, while the recommended value for The Things Stack Community Edition is 5 seconds. The Things Stack Community Edition Network Server will try to automatically change this for you, but it might take some time until the MAC command reaches the end device. 

See the official The Things Stack documentation to learn how to <a href="https://www.thethingsindustries.com/docs/devices/mac-settings/" target="_blank">configure MAC settings</a>.

## Migrate your gateway

<a href="https://www.thethingsindustries.com/docs/getting-started/migrating/gateway-migration/" target="_blank">Migrating your gateway</a> from The Things Network V2 to The Things Stack Community Edition cluster is a really easy process. 

First, you need to <a href="https://www.thethingsindustries.com/docs/gateways/adding-gateways/" target="_blank">add a gateway to The Things Stack Community Edition</a>.

Then, you need change the address of The Things Stack Community Edition cluster you are using - this will depend on which type of gateway you are using, <a href="https://www.thethingsindustries.com/docs/gateways/semtech-udp-packet-forwarder/" target="_blank">Semtech UDP Packet Forwarder</a> or <a href="https://www.thethingsindustries.com/docs/gateways/lora-basics-station/" target="_blank">LoRa Basics Station</a>. Let us assume you are using `eu1.cloud.thethings.network`.

- If you are using a Semtech UDP Packet Forwarder, change the `server_address` in your `global_conf.json` to `eu1.cloud.thethings.network`
- If you are using LoRa Basics Station with LNS subprotocol, change the LNS server address to `wss://eu1.cloud.thethings.network:8887` 
- If you are using LoRa Basics Station with CUPS subprotocol, change the CUPS server address to `https://eu1.cloud.thethings.network:443`

Once you update the cluster address, the data that reaches your gateway will be routed to your The Things Stack V3 cluster.
