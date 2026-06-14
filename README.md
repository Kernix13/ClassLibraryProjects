# Class Library project

A simple utility library that contains a single string-handling method. It also includes unit testing by adding a test project to the solution file using MSTest.

<!--
  GitHub slug: ClassLibraryProjects
  About text: ?
  Topics: csharp, mstest, ?
 -->

<span aria-hidden="true"><br></span>

## Prerequisites

- [.NET SDK 10.0](https://dotnet.microsoft.com/en-us/download)
- [Visual Studio Code](https://code.visualstudio.com/) with [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)

<span aria-hidden="true"><br></span>

## Installation & Usage

1. Clone this repository and switch into project folder

   ```sh
   git clone https://github.com/Kernix13/ClassLibraryProjects.git
   cd ClassLibraryProjects
   ```

2. Run the console application

   ```bash
   cd Showcase
   dotnet run
   # enter an upper or lowercase string
   ```

3. Build the console application

   ```bash
   cd Showcase
   dotnet build
   ```

4. Run the tests

   ```bash
   dotnet test
   ```

<span aria-hidden="true"><br></span>

## Project structure

```python
ClassLibraryProjects/
│   ├── ClassLibraryProjects.slnx   # Root solution file
│   └── README.md
│
│   ├── StringLibrary/
│   │   ├── StringLibrary.csproj
│   │   └── Class1.cs               # String method
│   │
│   ├── ShowCase/                   # Console App
│   │   ├── ShowCase.csproj
│   │   └── Program.cs
│   │
│   └── StringLibraryTest/          # Test project
│       ├── StringLibraryTest.csproj
│       └── Test1.cs
```

<span aria-hidden="true"><br></span>

<span aria-hidden="true"><br></span>

## Notes for this project

- A class library defines types and methods that are called by an application
  - I can not grasp what that means or its importance!
- NOTE: A solution serves as a container for one or more projects
- What is _WinGet configuration file_?

### xUnit branch

I created an alternate test project using xUnit. Switch to the branch `experiment/xunit-tests` to see the code.

### Commands for the string method library

```sh
# 1. Create and enter the root workspace folder
mkdir ClassLibraryProjects
cd ClassLibraryProjects

# 2. Create the solution file in the root
dotnet new sln

# 3. Create the Class Library project in its own subfolder
dotnet new classlib -o StringLibrary

# 4. Link the project to the solution
dotnet sln add StringLibrary/StringLibrary.csproj
```

### Commands for the console app

- Initially, the new console app project doesn't have access to the class library.
- To allow it to call methods in the class library, create a project reference to the class library project (step #3).

```sh
# 1. To create the console app (ShowCase)
dotnet new console -o ShowCase

# 2. Add to the root solution file
dotnet sln add ShowCase/ShowCase.csproj

# 3. Add the Project Reference
dotnet add ShowCase/ShowCase.csproj reference StringLibrary/StringLibrary.csproj

# 4. Run the console app
cd Showcase
dotnet run

# Build and run your console app from the root folder using the --project flag
dotnet run --project ShowCase
```

- The `reference` keyword is mandatory: Without the keyword `reference`, the dotnet add command will assume you are trying to add a NuGet package
- You can add the following to `ShowCase.csproj` for the project reference

```xml
<ItemGroup>
    <ProjectReference Include="..\StringLibrary\StringLibrary.csproj" />
</ItemGroup>
```

Using statement in Program.cs:

```cs
// See note below about namespace
using UtilityLibraries;
```

### Commands for the test project

- Unit tests provide automated software testing
- MSTest is one of three test frameworks you can choose from. The others are xUnit and nUnit.
- MSTest syntax used in this project but ignore because I will use xUnit:
  - `[TestClass]`
  - `[TestMethod]`
- When a unit test runs, each method marked with the `TestMethod` attribute in a class marked with the `TestClass` attribute executes automatically.
- A test method ends when the first failure is found or when all tests contained in the method succeed.
- The most common tests call members of the Assert class. Many assert methods include at least two parameters, one of which is the expected test result and the other of which is the actual test result.
- Some of the Assert class's most frequently called methods are shown in the following table:
  - `Assert.AreEqual`: Verifies that two values or objects are equal
  - `Assert.AreSame`: Verifies that two object variables refer to the same object
  - `Assert.IsFalse`: Verifies that a condition is `false`
  - `Assert.IsNotNull`: Verifies that an object isn't `null`
  - `Assert.Throws`: to indicate the type of exception it's expected to throw. The test fails if the specified exception isn't thrown

> NOTE: There are good notes in the [Test a .NET class library](https://learn.microsoft.com/en-us/dotnet/core/tutorials/test-class-library?pivots=vscode#add-and-run-unit-test-methods) tutorial about testing for an empty string (`String.Empty`) and a `null` string.

```sh
# 1. Create the test project
dotnet new mstest -o StringLibraryTest

# 2. Connect the Test Project to the Solution
dotnet sln add StringLibraryTest/StringLibraryTest.csproj

# 3. Add the Reference
dotnet add StringLibraryTest/StringLibraryTest.csproj reference StringLibrary/StringLibrary.csproj

# Run Your Tests (execute from the terminal root)
dotnet test
```

- NOTE: I think you should run `dotnet build` if you make any changes before running `dotnet test`
- Also make sure to validate that a test fails
- Look into what the "_Release version of the library_" is!

Test1.cs `using` statement and namespace:

```cs
using UtilityLibraries;

namespace StringLibraryTest;
```

Run the tests with the Release build configuration (why would you do this?)

```sh
dotnet test StringLibraryTest/StringLibraryTest.csproj --configuration Release
```

### Summary shell commands

```sh
# Console Application
dotnet new console

# Class Library
dotnet new classlib

# xUnit Test Project
dotnet new xunit

# MSTest Project
dotnet new mstest

# Testing summary
# To create the test project
dotnet new xunit -o StringLibraryTest # or:
dotnet new mstest -o StringLibraryTest
dotnet sln add StringLibraryTest/StringLibraryTest.csproj

# Add the Reference
dotnet add StringLibraryTest/StringLibraryTest.csproj reference StringLibrary/StringLibrary.csproj

# Run Your Tests - execute everything right from the terminal root
dotnet test
```

<span aria-hidden="true"><br></span>

## Project Name vs. Namespace (They Don't Have to Match)

- The Project/Folder Name (StringLibrary) controls the physical name of the file on your hard drive
- The Namespace (UtilityLibraries) is purely logical. It is just a label used inside the C# code to organize things and prevent naming conflicts

```cs
namespace UtilityLibraries;

// I would have used:
namespace ClassLibraryProject;
// I did not know that your namespace does not need to match the project name!
```

<!--
   1. See notes at bottom of m3-w4.md on C# Dev Kit Solutions explorer changes
   2. create a branch test/use-xunit and refactor to use xUnit framework
 -->
