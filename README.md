# dotnet-cpm-test

## Steps to reproduce the issue

- From root of repo checkout, run the following commands:

```cmd
git clean -xdf
git checkout .
dotnet nuget locals all --clear

```

- Perform a restore and see it fail:

```cmd
dotnet restore .\dotnet-cpm-test2.sln
    dotnet-cpm-test2\test.csproj : error NU1100: Unable to resolve 'Spectre.Console (>= 0.49.1)' for 'net8.0'. PackageSourceMapping is enabled, the following source(s) were not considered: NuGet.
```

- Try to find out why `Spectre.Console`

```cmd
dotnet nuget why .\dotnet-cpm-test2.sln Spectre.Console
Project 'test' does not have a dependency on 'Spectre.Console'.
```

Note that the response says

```plaintext
Project 'test' does not have a dependency on 'Spectre.Console'.
```

But we know that `Spectre.Console.Cli` depends on `Spectre.Console`.

Expected output for `dotnet nuget why` command is that:

- It should show that `Spectre.Console.Cli` depends on `Spectre.Console`.
- Alternatively show an error that it doesn't have enough info to track dependencies with help on how to.