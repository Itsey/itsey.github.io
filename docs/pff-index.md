# Plisky Fallout Fusion

Plisky Fallout Fusion (PFF) is the maintained package for integrating Plisky tools with the Fallout build engine, replacing the legacy `Plisky.Nuke.Fusion` (PNF) package when the original Nuke project stopped being actively maintained.

PFF is the compatibility layer for the Plisky toolchain in Fallout, and it continues to support Mollycoddle, Versonify, and the Discord hook integrations used by modern build automation.

[Versonify Fallout Wrapper](version-usingnuke.md)

[Mollycoddle Fallout Wrapper](molly-nuke.md)

## PnF / PFF Release Notes

## V 1.0.0 - Fallout Migration Release

- ✅ Breaking Change - Renamed the package from `Plisky.Nuke.Fusion` to `Plisky.Fallout.Fusion`.
- ✅ Breaking Change - Migrated from the Nuke build engine to Fallout, the community-maintained fork of Nuke.
- ✅ Maintenance - Continued support for Mollycoddle, Versonify and Discord webhooks.

## V 0.4.0 - Bronte & .NET 10 Compatibility Release

- ✅ Feature - Added Framework property to MollycoddleSettings for target framework resolution.
- ✅ Feature - Updated Versonify support to be compatible with Bronte release.
- ✅ Fix - Hardened Versonify command execution and error handling.
- ✅ Chore - Upgraded build and test projects to .NET 10.

## V 0.3.9 - Austen Compatibility Release

- ✅ Feature - Updated Versonify support to be compatible with Austen release.
- ✅ Feature - net10 support added to the package.

## V 0.3.6

- ✅ Feature - Updated Versonify support to be compatible with Austen release.
  * This adds a compatibility check to PNF/PFF, which allows the package to support the underlying capabilities of the different tools as they evolve. The compatibility check calls the tools and queries what their version number is, then determines which features are supported. The first example is the change of parameters and non-zero return code handling added in the Austen release of Versonify.

- ✅ Feature - net10 support added to the package.
  * No change to the functionality, just added an additional binary support for .NET 10.

## Legacy notes

- V0.3: Fix to support Nuke 9.04
- V0.2: Basic support for Versonify and MollyCoddle
- V0: Initial version

