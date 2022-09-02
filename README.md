## Description

This program makes developer easily to `sync and generate code in any language based on proto` from **_git remote repo_** or **_defined in local directory_** through `configuration yaml file`

## Installation

### macOS using Homebrew

```sh
brew tap louishuyng/sync-rpc-proto && brew install sync-proto
```

## Usage

First you need to define `protodep.yaml` file. [See instruction below](#configuration-file)

- Running with `protodep.yaml` defined in **_current directory_** that script execution

```bash
sync-proto
```

- Running with `yaml file` defined in **_other directory_** that script execution

```bash
sync-proto -f other_dirctory/custom.yaml
```

- Running with mode `local` (the default mode is `remote`)

```bash
sync-proto -m local
```

## Configuration File

```yaml
source: grpc-proto-repo
branch: main
outpb: app/grpc/pb
outdir: app/grpc/rpc_pb
token_key: GH_PROTO_REPO_TOKEN
dependencies:
  - auth/jwt
  - user/login_signup
command: python -m grpc_tools.protoc -I./$outpb --python_out=./$outdir --grpc_python_out=./$outdir $dependency.proto
```

`source`

The **_github remote url_** that including your proto files

`branch`

The branch you want to download proto and sync (for best practice `branch` can be use to separate environment)

`outpb`

The directory proto will be downloaded from `source`

`outdir`

The directory code will be generate based on proto after running script

`token_key`

The github token key set in (`.env` file or set as `environment variable`) get based on this [instruction](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

`dependencies`

The list proto you want to sync from `source`

- In remote git we have folder like below
  ```
  ├── auth
  │   ├── jwt.proto
  ├── user
  │   ├── login_signup.proto
  ```
- Then if we want to sync jwt proto and login_signup.proto we can define the array value like below in configuration file

  ```yaml
  dependencies:
    - auth/jwt
    - user/login_signup
  ```

`command`

The command to generate a code it be differently in each language.

In command script we can use `$outpb` `$outdir` and `$dependency` to reflect with the key already defined in configuration file.

Based on above configuration we will replace the value like below:

- `$outpb` => `app/grpc/pb`
- `$outpb` => `app/grpc/pb`
- `$dependency` => `auth/jwt` and `user/login_signup` in each iteration

#### Note: In local mode

- We can ignore keys `source`, `branch` and `token_key`

## Roadmap

- Remove `command` and add `language` key to detect context running script

## Support

For support, email huynguyennbk@gmail.com
