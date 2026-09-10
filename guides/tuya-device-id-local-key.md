# Find a Tuya Device ID and Local Key for LocalTuya

LocalTuya normally needs three device values:

- the local IP address
- the Tuya Device ID
- the Tuya Local Key

The Device ID and Local Key can be read through the Tuya Developer Platform after linking your Smart Life account to a cloud project. This guide describes the process using the Tuya menus available at the time of writing.

> **Keep the Local Key secret.** It allows software on your local network to communicate with the device. Never publish Local Keys, API secrets, access tokens, signed request headers, or complete debug responses.

## 1. Check the Smart Life account

The device must already be paired and working in Smart Life.

Check the account region in Smart Life under **Me → Settings → Account and Security → Region**. The cloud project's data center must support this region.

## 2. Create a Tuya Developer account

1. Open the [Tuya Developer Platform](https://platform.tuya.com/).
2. Sign in or create a developer account.
3. Open **Cloud → Development**.
4. Select **Create Cloud Project**.
5. Enter a project name and description.
6. Select **Smart Home** as the development method.
7. Select the data center that matches the Smart Life account region.
8. Create the project.

When Tuya asks for API services, enable the services offered for a Smart Home project. These normally include:

- Industry Basic Service
- Smart Home Basic Service
- Device Status Notification

Menu names and available services can change. Use the Smart Home project wizard when it is offered.

## 3. Link the Smart Life account

1. Open the new cloud project.
2. Open the **Devices** tab.
3. Select **Link Tuya App Account**.
4. Select **Add App Account**.
5. Tuya displays a QR code.
6. Open Smart Life and scan the QR code.
7. Confirm the login and authorization in Smart Life.
8. Select **Automatic Link** when Tuya asks how devices should be linked.
9. Allow the permissions needed to read and manage the devices.

The devices from the Smart Life account should now appear under **Devices → All Devices**.

If no devices appear, first check the selected data center and Smart Life account region.

## 4. Find the Device ID

Open **Devices → All Devices** in the cloud project.

The device table and device details show the Device ID. Copy the ID belonging to the correct device. Device names can be similar, so also check the product name and online state.

Do not confuse these values:

- **Device ID:** Tuya's identifier used by LocalTuya.
- **UUID:** another Tuya identifier; it is not the Device ID requested by LocalTuya.
- **Cloud IP:** the public internet address shown by Tuya; it is not the local IP address required by LocalTuya.

Find the local IP address in your router or DHCP server and reserve it there.

## 5. Find the Local Key

Depending on the current Tuya interface, the Local Key might be visible in the device details. If it is not shown, use the device API test:

1. Open the device in **Devices → All Devices**.
2. Select **Debug Device**, or open Tuya's API Explorer for the cloud project.
3. Find **Query Device Details**.
4. Enter the Device ID.
5. Run this request through the Tuya interface:

```text
GET /v2.0/cloud/thing/{device_id}
```

6. Find these fields in the successful response:

```json
{
  "result": {
    "id": "DEVICE_ID",
    "local_key": "LOCAL_KEY"
  },
  "success": true
}
```

Copy only the Device ID and Local Key into your private Home Assistant configuration. Do not publish the response.

## 6. App-account link limit

A Smart Life app account can be linked to no more than two Tuya cloud projects. Before linking it to a third project, unlink it from an old project:

1. Open the old cloud project.
2. Open **Devices → Link Tuya App Account**.
3. Find the linked Smart Life account.
4. Select **Unlink**.
5. Return to the new project and link the account again.

Unlinking the cloud project does not remove the devices from Smart Life. It removes that project's access to the app account.

## 7. When cloud trial access expires

First use the renewal, extension, or subscription options offered by Tuya for the existing developer account and project. Existing local control can continue while the device keeps the same Local Key, but cloud-based key retrieval may no longer be available.

If you legitimately move to another Tuya developer account or a new permitted cloud project, unlink the Smart Life account from the old project before linking it again. Follow Tuya's current plan and account terms. Do not create repeated accounts only to bypass trial or subscription restrictions.

## 8. Local Key changes

The Local Key can change when you:

- pair the device again
- reset the device
- remove and add it again in Smart Life

If a previously working LocalTuya device becomes unavailable after one of these actions, retrieve the new Local Key and update the device configuration.

## Official Tuya documentation

- [Create a Smart Home cloud project and link Smart Life](https://developer.tuya.com/en/docs/iot/Platform_Configuration_smarthome?id=Kamcgamwoevrx)
- [Link a Tuya app account and understand the two-project limit](https://developer.tuya.com/en/docs/iot/link-devices?id=Ka471nu1sfmkl)
- [Query Device Details API](https://developer.tuya.com/en/docs/cloud/3829469013?id=Kcp2l2v9wma0m)
