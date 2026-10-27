# DSI cellular-data fix qualified — 2026-10-26

Restores the omitted stock /system/etc/data/dsi_config.xml, keeping all stock
profile values. XML declaration is moved before the opening comment only.
CM13 live test initialized DSI and established LTE after correct APN selection.
CM12 full signed ROM passed LTE browser access and YouTube streaming after
reboot. No APN guessing, modem firmware replacement or RIL shim is required
by this fix. APN selection must match the actual carrier subscription.
Raw subscriber/location logs and private signing keys are not published.

# LG K7 MS330 vendor inputs for CM13

**2026-10-25: selected inputs are qualified in the enforcing `cm_m1-userdebug`
ROM.** Use `bringup/cm13-m1` with matching
[product](https://github.com/airi-kozume/cyanogenmod_device_lge_m1/tree/bringup/cm13-m1),
[common](https://github.com/airi-kozume/android_device_lge_m1-common/tree/resurrection/cm13-static-audit)
and [kernel](https://github.com/airi-kozume/cyanogenmod_kernel_lge_m1/tree/bringup/cm13-m1)
branches. The product README contains the current hardware matrix.

| Inputs | Selection / observed result |
| --- | --- |
| `proprietary/` | Original MS330 inputs; archive presence does not imply selection |
| `radio-stock/` | Retained radio/runtime providers alongside CM13 platform providers; SIM detection/IMEI/baseband work |
| `camera-cm13/` | Matched H631 sensor adapter/JPEG pair; both cameras save photos |
| `video-stock/` | Complete K7 Venus bundle; front/rear AVC/AAC recording and hardware playback work with matching codec declarations |
| `proprietary/lib/hw/gps.default.so` | Exact stock GPS HAL; selected as `gps.k7-stock.so` behind common-tree callback adapter; Maps location confirmed |
| `webview/` | Original signed WebView 107.0.5304.36; system-provider login confirmed |
| `microg/` | Pinned upstream Services, Companion and GsfProxy, bundled without re-signing |

Camera/sensor/rotation, GPS, wireless/audio and video need the matching device
and platform selections. Keep matched JPEG/firmware sets together. Symbol-name
closure alone does not prove ABI compatibility: GPS additionally needs the
common tree's XTRA callback adapter. Per-set provenance records pin inputs.

## Stage upstream APKs

Services 0.3.17.252432, Companion 0.3.17.40226 and GsfProxy 0.1.0 are selected in
current normal-product builds. Their self-check entries have been confirmed
during bring-up. Generated APKs are ignored by Git; stage their pinned originals:

```sh
python3 microg/stage.py --fetch
# Or reuse a local cache:
python3 microg/stage.py --cache /path/to/cache
```

Hashes, original signatures and native layouts are checked. See
[microG input notes](microg/README.md). Separate framework support is required.

## Remaining scope

Active-SIM cellular data and normal-use battery endurance await testing. GPS/Maps
and overnight idle now pass observed tests. Calls/call audio are outside scope:
no VoLTE support, while local networks require VoLTE. Other K7 variants are
unqualified. This repository is not a standalone build environment or ROM;
workspace audits and rollback packages are maintained separately.
