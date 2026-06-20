# Plisky Nuke Fusion

Plisky Nuke Fusion library to add wrappers around Plisky tools for use within nuke.



[Versonify Nuke Wrapper](version-usingnuke.md)



## PnF Release Notes.


## 0.3.6

- ✅ Feature - Updated Versonify support to be compatible with Austen release.
  * This adds a compatibility check to PNF, which should allow it to correctly support the underlying capabilities of the different tools as they version.  The compatibility check calls the tools and queries what their version number is, once it knows then it will know which features are supported.  The first one of these is the change of parameters and non zero return code parameter support for the Austen release of Versonify.  PNF can now detect whether the Versonify version can support these features.
  
        
- ✅ Feature - net10 support added to the package.
   * No change to the functionality, just added an additional binary support for .net 10.

