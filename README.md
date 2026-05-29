# Romestead AMP Template

Unofficial AMP Generic module template for hosting a **Romestead Dedicated Server** with [CubeCoders AMP](https://cubecoders.com/AMP).

This template is intended for personal/local AMP use and early testing. It is **not an official CubeCoders AMP template** and should be validated before use in production or submitted upstream.

## Status

Experimental.

The template has been drafted from publicly available Romestead dedicated server information and AMP Generic module conventions. It may require adjustment after real-world testing, especially around startup detection, runtime dependencies, and console output parsing.

## What This Template Provides

* SteamCMD installation for the Romestead dedicated server
* Linux launch using `dotnet Server.dll`
* Windows launch using `Server.exe`
* Default UDP game port configuration
* Basic editable server settings through AMP
* STDIO console control
* Graceful shutdown using the `stop` command
* Basic server config generation

## Known Requirements

### AMP

This template is designed for the AMP **Generic** module.

### SteamCMD

Romestead Dedicated Server is installed through SteamCMD.

Steam app/tool ID used by this template:

```text
4763510
```

### Runtime

On Linux, the server is expected to run with:

```bash
dotnet Server.dll
```

This means the AMP host or container must have a compatible .NET runtime installed.

The template currently assumes a .NET 8-compatible runtime may be required. Verify this against the version distributed with the Romestead server files.

On Windows, the template attempts to launch:

```text
Server.exe
```

## Template Files

This template is made up of the following files:

```text
romestead.kvp
romesteadconfig.json
romesteadmetaconfig.json
romesteadports.json
romesteadupdates.json
```

## Installation

1. Stop AMP if required by your installation method.
2. Copy the template files into your AMP template directory.
3. Restart AMP.
4. Create a new Generic AMP instance using the Romestead template.
5. Run the instance update task.
6. Start the server.

The SteamCMD update process should download the Romestead dedicated server files into the app directory.

## Default Server Settings

The generated default `config.json` contains:

```json
{
  "AutoStartWorldName": "",
  "AutoCreateAndLoadWorld": true,
  "AutoCreateWorldSize": 1,
  "AutoCreateWorldSeed": null,
  "Password": "",
  "Port": 8050,
  "MaxPlayers": 8,
  "EnableCheats": false
}
```

## Default Port

|   Port | Protocol | Purpose                |
| -----: | :------: | ---------------------- |
| `8050` |    UDP   | Romestead game traffic |

Make sure this port is open in your firewall and forwarded if hosting from behind NAT.

## Console Commands

The server appears to support STDIO console commands.

Useful commands to test:

```text
list
say hello
save
stop
```

The AMP template uses:

```text
stop
```

as the graceful shutdown command.

## Current Limitations

This template should be considered a starting point.

The following items still need to be confirmed through testing:

* Exact Linux runtime requirement
* Whether Windows should launch `Server.exe` directly or through another runtime
* Correct startup-ready console message
* Correct `Console.AppReadyRegex`
* Player join/leave/chat regex patterns
* Valid values for `AutoCreateWorldSize`
* Maximum supported player count
* Whether additional UDP or TCP ports are required
* Whether the server supports a true remote admin protocol beyond STDIO

Until the startup-ready message is confirmed, the template may use immediate readiness detection.

## Testing Checklist

After installing the template, test the following:

* [ ] AMP can create a Romestead instance
* [ ] SteamCMD update completes successfully
* [ ] Server files are downloaded into the expected directory
* [ ] `config.json` is created on first update
* [ ] Server starts successfully on Linux
* [ ] Server starts successfully on Windows
* [ ] AMP console shows readable output
* [ ] `list` command works from the AMP console
* [ ] `say` command works from the AMP console
* [ ] `save` command works from the AMP console
* [ ] AMP can stop the server cleanly using `stop`
* [ ] Clients can connect on UDP port `8050`
* [ ] Password-protected servers work
* [ ] World auto-creation works
* [ ] Max player setting is respected

## Suggested Improvements

Once startup logs are captured, update the template with a proper ready regex.

For example, replace immediate readiness detection with something like:

```ini
App.ApplicationReadyMode=RegexMatch
Console.AppReadyRegex=YOUR_CONFIRMED_READY_MESSAGE_HERE
```

Do not guess this value. Capture the actual server startup output first.

## Upstream Submission Notes

CubeCoders maintainers require AMP templates to be tested and functional before submission.

Before opening a pull request to `CubeCoders/AMPTemplates`:

* Test the template on a clean AMP instance
* Confirm install, start, stop, and update behavior
* Confirm Linux and Windows behavior where possible
* Replace placeholder or uncertain values
* Remove local-only notes
* Ensure the template follows AMPTemplates repository conventions
* Do not submit an untested AI-generated draft as-is

This repository is intended to help bootstrap local testing before producing a clean, validated template.

## Disclaimer

This is an unofficial community template.

Romestead is the property of its respective developer/publisher. AMP is developed by CubeCoders. This project is not affiliated with or endorsed by either party.
