# BuildNinja YAML Configuration Examples

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Ready-to-use YAML configurations for BuildNinja Config File runner. Config as code for your CI/CD pipelines - a simpler Jenkins alternative for build automation.

## Quick Start

1. Copy any `.yaml` file to your repository
2. In BuildNinja, create a new Build Configuration
3. Add VCS Settings pointing to your repo
4. Add Execution Step → Select **Config File** runner
5. Enter the path to your YAML file (e.g., `hello-world.yaml`)
6. Run the build

## Real Project Examples

For ready-to-use configurations that build real open-source projects, see:

**[buildninja-quickstart](https://github.com/BuildNinja-CICD/buildninja-quickstart)** - 10 working configs including:
- ripgrep (Rust, 59K stars)
- Facebook Lexical (TypeScript, 22K stars)
- Open WebUI (Python/Vite, 75K stars)
- Reactive Resume (pnpm monorepo, 970K users)
- And more...

## Examples

| File | Description | Runners Used |
|------|-------------|--------------|
| [hello-world.yaml](hello-world.yaml) | Simplest test config | Command Line |
| [nodejs-build.yaml](nodejs-build.yaml) | Node.js build + test | Command Line |
| [dotnet-build.yaml](dotnet-build.yaml) | .NET build + test | MSBuild, VSTest |
| [multi-step.yaml](multi-step.yaml) | Multiple sequential steps | Command Line |
| [with-artifacts.yaml](with-artifacts.yaml) | Build with artifact collection | Command Line |

## YAML Schema

### Basic Structure

```yaml
name: config-name                # Build configuration name
description: What this does      # Brief description
version: 1.0                     # Config version

settings:
  vcs-steps:      # Version control (optional)
  build-steps:    # Build execution (required)
  artifacts:      # Artifact collection (optional)
```

### VCS Steps

```yaml
vcs-steps:
  - name: Checkout Repository
    key: vcs_git
    disabled: false
    args:
      url: https://github.com/user/repo.git
      branch: main
      authType: anonymous
```

#### With Token Authentication

```yaml
vcs-steps:
  - name: Checkout Private Repo
    key: vcs_git
    disabled: false
    args:
      url: https://github.com/user/private-repo.git
      branch: main
      authType: pat
      username: git-user
    argsProtected:
      token: your-personal-access-token
```

### Build Steps

```yaml
build-steps:
  - key: runner_cmd
    name: Step Name
    disabled: false
    workDir: ""
    args:
      commands: echo Hello
    argsProtected: {}
```

#### Multi-line Commands

```yaml
build-steps:
  - key: runner_cmd
    name: Multiple Commands
    disabled: false
    workDir: ""
    args:
      commands: |-
        echo "First command"
        echo "Second command"
        npm install
    argsProtected: {}
```

### Artifacts

```yaml
artifacts:
  - name: build-output
    condition: buildSucceeds
    paths:
      - dist
      - build
```

## Supported Runners

| Runner Key | Description | Use Case |
|------------|-------------|----------|
| `runner_cmd` | Command Line | Shell commands, scripts |
| `runner_msbuild` | MSBuild | .NET solutions, C# projects |
| `runner_vstest` | VSTest | .NET test assemblies |

## Artifact Conditions

| Condition | When Collected |
|-----------|----------------|
| `always` | Every build |
| `buildSucceeds` | Only on successful builds |
| `buildFails` | Only on failed builds |

## Runner-Specific Arguments

### MSBuild Runner (`runner_msbuild`)

```yaml
- key: runner_msbuild
  name: Build Solution
  args:
    solution: MyApp.sln
    configuration: Release
    parameters: "/m"
```

### VSTest Runner (`runner_vstest`)

```yaml
- key: runner_vstest
  name: Run Tests
  args:
    testAssembly: tests/MyApp.Tests.dll
    reportDir: reports
    reportName: test-results
    failCriterian: failedTests
    args: ""
```

## Docker Images

- **Server:** [grapehub/buildninja-server](https://hub.docker.com/r/grapehub/buildninja-server)
- **Agent:** [grapehub/buildninja-agent](https://hub.docker.com/r/grapehub/buildninja-agent)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

- Website: [buildninja.grapehub.io](https://buildninja.grapehub.io)
- Documentation: [buildninja.grapehub.io/docs](https://buildninja.grapehub.io/docs)
- Issues: [GitHub Issues](https://github.com/BuildNinja-CICD/buildninja-yaml-config-examples/issues)
- Contact: hello@grapehub.io
