# CWP-Tools

These tools are designed to help work with CrowdStrikes CWP products and CR registries.

## Warning (Proof of Concept)

This is *not a secure method* of storing secets in an unencrypted bash shell. This is meant as a demostration purpose only on how one might load environmental variables into the shell for processing. 

Recommendation is to a a secure vault for the secrets, or some other approved encrypted manner where secrets are not stored at rest unencrypted.

## `falcon_activate`

Sourcing `. bin/falcon_activate` into your shell will create two new aliases that allow for loading and unloading of the environmental bash variables stored in the following files.

```
~/.falcon_secrets 
~/.falcon_common
```

Example the example files for how exported variables with the correct names are created in the example files.

![Screenshot](docs/Fetching%20Tags.png?raw=true "Screenshot") <!-- .element height="50%" width="50%" -->
