# 🐑 Mouflon — CLI tool to get OIDC tokens

<img align="right" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Mouflon_Corse.jpg/300px-Mouflon_Corse.jpg" />

Mouflon acts as an *OIDC client* to retrieve an **access token** from an OIDC provider.

Upon initial execution, it opens a browser and executes the typical OIDC redirects to get an *access token*
via *authorization_code* grant.

If successful, it caches the *access token response* (thus both the *access token* and the *refresh token*), and then
returns the *access token* (as long as it's valid), or uses the *refresh token* to refresh the *access token* 
and of course return the new *access token*. If also the *refresh token* is expired, it again opens the browser to execute
the OIDC authorization.

## Status

*mouflon* works, but is pretty basic and not very flexible.

- opening the browser works only in Linux and the fallback solution is implemented very naively
- supports only keycloak, only a single realm and a single client
- close to no error handling. So it will throw stack traces without any hints upon errors

## Installation

Before installing Mouflon, ensure that [*Deno*](https://deno.land/) is installed on your system. If it's not installed, you can find the installation instructions on the [Deno installation page](https://deno.land/#installation).

### Setting Up Mouflon

1. **Place `mouflon.ts` in a Suitable Directory**: Download or clone the `mouflon.ts` file into a directory of your choice. For example:
   ```bash
   git clone [repository-url] ~/path/to/mouflon-directory
   ```
   Make sure to replace `[repository-url]` with the actual URL of the repository and `~/path/to/mouflon-directory` with the path where you want to store the script.

2. **Make `mouflon.ts` Executable**: Change the permissions of the file to make it executable.
   ```bash
   chmod +x ~/path/to/mouflon-directory/mouflon.ts
   ```

3. **Create a Symbolic Link in Your `$PATH`**:
    - Decide on a directory within your `$PATH` where you want to place the symbolic link. Common choices include `~/bin` or `/usr/local/bin`.
    - Create a symbolic link to `mouflon.ts`. This allows you to run `mouflon.ts` from any location without specifying the full path. Replace `~/bin` with your chosen directory if different:
      ```bash
      ln -s ~/path/to/mouflon-directory/mouflon.ts ~/bin/mouflon.ts
      ```

4. **Verify the Installation**:
    - Ensure the symlink was created successfully by listing the contents of the directory:
      ```bash
      ls -l ~/bin
      ```
    - Test running the script to confirm everything is set up correctly:
      ```bash
      mouflon.ts --help
      ```

This guide provides a generic approach to installing Mouflon with a focus on creating a symbolic link within a directory that's part of the user's `$PATH`. It assumes the user has already downloaded `mouflon.ts` and has Deno installed on their system.
## Configuration 

### Keycloak

Create an OIDC client (Standard flow enabled), should be "confidential", allow `http://localhost:4800/` as redirect URL.

Download the "*Keycloak OIDC JSON*" file available under the "*Installation*" tab.

### Mouflon

Copy said JSON-file into `~/.config/mouflon/default.json` (if you set `$XDG_CONFIG_HOME` replace `~/.config` with that value).

Future versions could allow other configurations (selectable via CLI-arg) and other providers.

Currently, *mouflon* does **not** validate the JSON file.

## Usage

Simply execute `mouflon.ts` or `./mouflon.ts`

Get full AccessTokenResponse with `mouflon.ts --full-response`

## Examples

for bash

```shell script
curl -H "Authorization: Bearer $(mouflon.ts)" https://example.com/protected
```
    
or fish shell

```shell script
AT=(mouflon.ts) curl -H "Authorization: Bearer $AT" https://example.com/protected
```
