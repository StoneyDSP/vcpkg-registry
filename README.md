# vcpkg-registry

The vcpkg package registry for [StoneyDSP](https://github.com/StoneyDSP).

[![ports](https://github.com/StoneyDSP/vcpkg-registry/actions/workflows/ports.yml/badge.svg)](https://github.com/StoneyDSP/vcpkg-registry/actions/workflows/ports.yml)

Provides the following packages to [vcpkg](https://githyub.com/microsoft/vcpkg):

- [StoneyDSP](https://github.com/StoneyDSP/StoneyDSP) - The StoneyDSP C++ audio library
- [Rack-SDK](https://github.com/StoneyDSP/Rack-SDK) - Unofficial CMake targets for VCV-Rack 2 SDK
- [StoneyVCV](https://github.com/StoneyDSP/StoneyVCV) - StoneyDSP modules for VCV Rack 2
- <s>StoneyJUCE - The StoneyDSP C++ audio library for JUCE</s> - TBD...

To acquire any of the above packages, simply add us to your project's [`vcpkg-configuration.json`](https://learn.microsoft.com/en-us/vcpkg/reference/vcpkg-configuration-json) and specify which packages you wish to source from our registry (example):

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/vcpkg-tool/main/docs/vcpkg-configuration.schema.json",
  "default-registry": {
    "kind": "git",
    "baseline": "66b4b34d99ab272fcf21f2bd12b616e371c6bb31",
    "repository": "https://github.com/microsoft/vcpkg"
  },
  "registries": [
    {
      "kind": "git",
      "baseline": "478b4e0f7d8781bc1b1320b8afef4f0620e9f3f4",
      "reference": "development",
      "repository": "https://github.com/StoneyDSP/vcpkg-registry.git",
      "packages": [
		"stoneydsp",
		"rack-sdk"
      ]
    },
  ]

}
```

Then add your chosen packages to your project's [`vcpkg.json`](https://learn.microsoft.com/en-us/vcpkg/reference/vcpkg-json) (example):

```json
{
  "name": "myplugin",
  "version": "2.0.0",
  "license": "MIT",
  "supports": "(linux & x64)|(osx & x64)|(osx & arm64)|(windows & mingw & x64)",
  "dependencies": [
    {
      "name": "catch2",
      "version>=": "3.5.2"
    },
    {
      "name": "stoneydsp",
      "version>=": "0.1.562"
    },
    {
      "name": "rack-sdk",
      "version>=": "2.5.2",
      "default-features": true,
      "features": [
        "dep", "lib"
      ]
    }
  ]
}
```

*NOTE:* in the above example, the package `catch2` would be taken from the `"default-registry"`, which in this case is Microsoft's officially curated package registry, while both `rack-sdk` and `stoneydsp` would be fetched from our vcpkg-registry, since this was specified in the hypothetical project's `vcpkg-configuration.json` file, above.

This enables you to add our registry to your project, along with a *constraint* to ensure that your vcpkg only tries to pick up the packages which we support (see list above), and not interfere with any existing dependencies you already have.

Our [GitHub workflow](https://github.com/StoneyDSP/vcpkg-registry/actions) ensures that our portfiles are tested regularly.

Please [raise an issue](https://github.com/StoneyDSP/vcpkg-registry/issues/new/choose) if you do find anything not working as expected.
