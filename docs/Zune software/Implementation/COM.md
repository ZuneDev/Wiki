# COM
The Zune software relies on public COM APIs exposed by Windows and many private COM interfaces implemented in various DLLs such as `ZuneNativeLib.dll`.


## Interfaces

### IGlobalInterfaceTable
* **IID:** `00000146-0000-0000-C000-000000000046`
* **Category:** COM
* [**Docs (Official)**](https://learn.microsoft.com/en-us/windows/win32/api/objidl/nn-objidl-iglobalinterfacetable)

### IMultiLanguage
* **IID:** `275C23E1-3747-11D0-9FEA-00AA003F8646`
* **Category:** MLang
* [**Docs (Official)**](https://learn.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/platform-apis/aa741022(v=vs.85))

### IPortableDevicePropVariantCollection
* **IID:** `89B2E422-4F1B-4316-BCEF-A44AFEA83EB3`
* **Category:** Windows Portable Devices
* [**Docs (Official)**](https://learn.microsoft.com/en-us/windows/win32/wpd_sdk/iportabledevicepropvariantcollection)

### IPCUsageDataMeter
* **IID:** `600D6E55-DEA5-4E4C-9C3A-6BD642A45B9D`
* **Category:** Zune

### IOleObject
* **IID:** `00000112-0000-0000-C000-000000000046`
* **Category:** COM
* [**Docs (Official)**](https://learn.microsoft.com/en-us/windows/win32/api/oleidl/nn-oleidl-ioleobject)

### ???
* **IID:** `0B8B0046-9271-40CD-BAF9-25EF8AA8F923`
* Something to do with IQueryPropertyBag?


## Classes

### StdGlobalInterfaceTable
* **CLSID:** `00000323-0000-0000-C000-000000000046`
* **Implements:**
    * [IGlobalInterfaceTable](#iglobalinterfacetable)

### CMultiLanguage
* **CLSID:** `275C23E2-3747-11D0-9FEA-00AA003F8646`
* **Implements:**
    * [IMultiLanguage](#imultilanguage)

### CPortableDevicePropVariantCollection
* **CLSID:** `08A99E2F-6D6D-4B80-AF5A-BAF2BCBE4CB9`
* **Category:** Windows Portable Devices
* **Implements:**
    * [IPortableDevicePropVariantCollection](#iportabledevicepropvariantcollection)

### CPCUsageDataMeter
* **CLSID:** `5C140836-43DE-11D3-847D-00C04F79DBC0`
* **Category:** Zune


## *Needs triage*
- `AE0D9387-DCCF-4BC1-93E6-C690B060C202` IID_???
- `6C03E4FE-05DB-4DDA-9E3C-06233A6D5D65` IID_??? (audiodev.dll?)
- `EF6B490D-DCD8-437A-AFFC-DA8B60EE4A3C` IID_??? (something WPD, SPS)
- `0000000E-0000-0000-C000-000000000046` IID_IBindCtx
- `0000010C-0000-0000-C000-000000000046` IID_IPersist
- `00000100-0000-0000-C000-000000000046` IID_IEnumUnknown
- `0000010F-0000-0000-C000-000000000046` IID_IAdviseSink
- `7FD52380-4E07-101B-AE2D-08002B2EC713` IID_IPersistStreamInit
- `00000109-0000-0000-C000-000000000046` IID_IPersistStream
- `00000118-0000-0000-C000-000000000046` IID_IOleClientSite
- `3AF24292-0C96-11CE-A0CF-00AA00600AB8` IID_IViewObjectEx
- `00000127-0000-0000-C000-000000000046` IID_IViewObject2
- `0000010D-0000-0000-C000-000000000046` IID_IViewObject
- `FC4801A3-2BA9-11CF-A229-00AA003D7352` IID_IObjectWithSite
