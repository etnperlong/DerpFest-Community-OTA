# DerpFest Community OTA

OTA metadata for etnperlong's DerpFest Community builds. This is not an official
DerpFest release repository: ROM ZIPs are hosted externally, and only update
metadata and plain-text release notes live here.

Current device: **TB375FC** — Lenovo Xiaoxin Pad Pro 12.7 (2025).

## Client integration

Once this repository is published with a `main` branch, point the device's
Updater resource overlay at:

| Resource | URL |
| --- | --- |
| `updater_server_url` | `https://raw.githubusercontent.com/etnperlong/DerpFest-Community-OTA/main/{device}.json` |
| `device_info_url` | `https://raw.githubusercontent.com/etnperlong/DerpFest-Community-OTA/main/info/device-info.json` |
| `menu_changelog_url` | `https://raw.githubusercontent.com/etnperlong/DerpFest-Community-OTA/main/changelog_%1$s.txt` |

`{device}` and `%1$s` are upstream placeholders. Confirm that
`ro.updater.next_device`, or otherwise `ro.derpfest.device`, resolves to the
device codename.

## Publishing updates

1. Build and sign a full A/B OTA ZIP compatible with the installed release.
2. Upload the ZIP to external storage and verify its public HTTPS URL.
3. Update `<codename>.json`:
   - `filename`, byte `size`, and `url` from the final ZIP;
   - `datetime` from the ZIP's `post-timestamp`;
   - `romtype` as `Community` and `version` in upstream-prefixed form (e.g.
     `v17`);
   - `id` as the ZIP's lowercase SHA-256 — a stable update ID, not a checksum
     Updater verifies.
4. Prepend the release to `changelog_<codename>.txt`, newest first.
5. Publish the metadata. The feed keeps one latest full package per device;
   replace the previous entry rather than appending.

Only full OTA packages are listed; incremental OTA generation is out of scope.
The JSON `version` is a display string, not an installation compatibility gate.
Never replace an existing release ZIP with different bytes.

## Notes

- `support_group` is `null`; the Updater falls back to its default DerpFest
  Telegram link rather than hiding the button.
- `menu_downloads_url` and `report_issue_url` are client resources, not fields
  in the device JSON.

## Upstream contracts

- [OTA response model](https://github.com/DerpFest-AOSP/android_packages_apps_Updater/blob/17/app/src/main/java/org/lineageos/updater/data/source/network/NetworkUpdate.kt)
- [Device information model](https://github.com/DerpFest-AOSP/android_packages_apps_Updater/blob/17/app/src/main/java/org/lineageos/updater/data/source/network/NetworkDeviceInfo.kt)
- [Official OTA example](https://github.com/DerpFest-AOSP/Updater-Stuff/blob/master/raphael.json)
