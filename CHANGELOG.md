## v1.2
Bug Fixes:
- Properly retrieve valid secondary signatures for cert name verification (via updated pinvoke library)

Improvements:
- Updated to .NET 9

## v1.1
New Features:
- selfupdate: Update the wrapper to the latest available and compatible version.
- selfversion: Show the current version of the wrapper.

Improvements:
- update verb has been renamed to tfupdate.

## v1.0
New Features:
- addtopath: Add wrapper's directory to the user's path. Requires a console restart to take effect.
- createsettingsfile: Create a settings.json file that exposes the settings. Changes here are very likely to break the app.
- update: Download the latest hotfix/patch version of every major.minor version of the Terraform CLI available.
- major.minor arguments...: Run the Terraform CLI version specified with the arguments after the version.
