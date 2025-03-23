# Terraform Wrapper

## Overview

TerraformWrapper is a C# utility that provides a convenient way to manage and work with multiple versions of Terraform CLI at the same time. This tool enables you to:

- Easily switch between different Terraform versions
- Automatically download specific versions when needed
- Manage Terraform versions without manual installation
- Update the wrapper to the latest version
- Configure settings for your environment

## System Requirements

- .NET 9.0 or later
- Windows operating system

## Installation

1. Download the TerraformWrapper binary from the releases page
2. (Optional) Add the directory containing the wrapper to your PATH:
   ```
   TerraformWrapper.exe addtopath
   ```
   This will allow you to call the wrapper from any directory. You'll need to restart your console for the change to take effect.

## Command Reference

### General Command Format

The TerraformWrapper uses the following command format:

```
TerraformWrapper.exe [command]
```

or for executing Terraform commands:

```
TerraformWrapper.exe [version] [terraform arguments]
```

### Available Commands

#### `addtopath`

Adds the wrapper's directory to the user's PATH environment variable.

```
TerraformWrapper.exe addtopath
```

**Note**: You must restart your console for this change to take effect.

#### `createsettingsfile`

Creates a `settings.json` file in the wrapper's directory that exposes configurable settings.

```
TerraformWrapper.exe createsettingsfile
```

**Warning**: Modifying these settings without understanding their purpose may cause the application to malfunction.

#### `selfupdate`

Checks for and downloads the latest version of the TerraformWrapper itself.

```
TerraformWrapper.exe selfupdate
```

#### `selfversion`

Displays the current version of the TerraformWrapper.

```
TerraformWrapper.exe selfversion
```

#### `tfupdate`

Downloads the latest patch version of every major.minor Terraform version available.

```
TerraformWrapper.exe tfupdate
```

This is useful to ensure you have the latest patch versions of all Terraform major.minor versions.

#### Running Terraform with a Specific Version

```
TerraformWrapper.exe [major.minor] [terraform arguments]
```

For example:
```
TerraformWrapper.exe 1.9 init
```

This will run `terraform init` using Terraform version 1.9.x (the latest patch version of 1.9).

**Notes**:
- You must specify only the major and minor versions (e.g., `1.9`, not `1.9.2`).
- The wrapper will automatically download the specified version if it's not already available locally.

## Examples

### Initialize a Project with Terraform 1.8

```
TerraformWrapper.exe 1.8 init
```

### Apply Infrastructure Changes with Terraform 1.7

```
TerraformWrapper.exe 1.7 apply
```

### Create a Terraform Plan with Terraform 1.9 and Save It to File

```
TerraformWrapper.exe 1.9 plan -out=myplan.tfplan
```

### Show Detailed Help for Terraform Commands

```
TerraformWrapper.exe 1.8 --help
```

## Configuration

The TerraformWrapper uses a `settings.json` file for configuration. You can create this file using the `createsettingsfile` command.

### Configuration Options

The following settings can be configured:

| Setting | Description | Default Value |
|---------|-------------|---------------|
| `RootPattern` | Regex pattern for finding Terraform versions | `"/terraform/(\\d+\\.\\d+\\.\\d+)/"` |
| `TerraformFQDN` | FQDN for Terraform downloads | `"releases.hashicorp.com"` |
| `TerraformRootURL` | Root URL for Terraform downloads | `"https://releases.hashicorp.com/terraform"` |
| `FileDownloadPattern` | Regex pattern for finding Windows downloads | `"\\\"https://.*_windows_amd64\\.zip\\\""` |
| `FileExeName` | Terraform executable name | `"terraform.exe"` |
| `MinimumVersion` | Minimum Terraform version to use | `"1.0.0"` |
| `CertSubjectName` | Certificate subject name for validation | `"HashiCorp, Inc."` |

**Note**: Changing these settings without understanding their purpose may break the application.

## How It Works

TerraformWrapper keeps track of downloaded Terraform versions in its directory. When you request a specific version:

1. It checks if that version is already downloaded
2. If not, it downloads the latest patch version of the requested major.minor version
3. It then runs your Terraform command using the appropriate executable

Downloaded versions are stored as `terraform_[version].exe` in the wrapper's directory.

## Security

The wrapper validates the cryptographic signature of all downloaded Terraform versions to ensure authenticity. Only executables signed by HashiCorp, Inc. are used.
