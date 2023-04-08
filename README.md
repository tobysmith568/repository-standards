# Repository Standards

This document details the standards to which Toby Smith ([@tobysmith568](https://github.com/tobysmith568)) aims to hold his repositories to.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

This is a living document so it will change over time; as it does, repositories should also be updated to remain up to date.

## Setup

- Repository names should be all lowercase.
	- Words should be separated by hyphens.
	- Full stops may be used when the repository name is a domain name.
- The default branch must be called "main".
- All branches should be lowercase.
- All branches should be prefixed with the initials of the branch owner, e.g. `ts/`.
- GitHub features like Wikis and Projects must be turned off if they're not being used.
- The repository's Details section must not show elements like Packages or Environments if they're not being used.
- The repository's Details section should have a description and relevant tags. The URL field should point to either the project itself for websites, or to the package page for software packages (npm/NuGet/etc).

## Files

### README.md

- Each repository must have an `README.md` file in the root directory.
- This file must explain what the project is for and how to run it.
- The first section of the file should include shields about the project.
	- All projects should use shields showing their code coverage.
	- Software packages should also include shields like their version, licence, and minified size when relevant.
- The final section of the file must be titled "Licence"; it must:
	- State which licence is used by the repository.
	- Link to the LICENCE.md file using a relative path.

### LICENCE.md

- Each repository must have a `LICENCE.md` file in the root directory.
- This file must contain the licence used by the repository.
  - Repositories should typically either use the ISC licence or simply declare a copyright.
	  - The year range in the copyright should be kept up to date when changes are made to the repository content.

### .gitignore

- Each repository must have an `.gitignore` file in the root directory.
- This file should be generated using gitignore.io and should be built up using the following as a base:   
[https://www.toptal.com/developers/gitignore?templates=linux,windows,macos](https://www.toptal.com/developers/gitignore?templates=linux,windows,macos).
  - IDEs should be added. Most project should include VS Code and repositories containing C# should also include Visual Studio and Rider.
  - Languages and frameworks should be added.
- Project-specific manual additions to the file must be added above the gitignore.io generated content.

### .gitattributes

- Each repository must have an `.gitattributes` file in the root directory.
- This file must at least specify `* text eol=lf`

### .vscode/settings.json

- Directories that are intended to be opened using VS Code must have a `.vscode/settings.json` file relative to them.
- For projects that use TypeScript this must contain `"typescript.tsdk": "node_modules/typescript/lib"`.
- For projects that use Prettier this must contain `"editor.defaultFormatter": "esbenp.prettier-vscode"`.

### .vscode/extensions.json

- Directories that are intended to be opened using VS Code must have a `.vscode/extensions.json` file relative to them.
- All extensions that interact with the project specifically must be added.

### .editorconfig

- Each repository must have an `.editorconfig` file in the root directory.
- This file should be based on the `.editorconfig` file in the assets directory of this repository, though language sections may be removed when appropriate.
- More specific `.editorconfig` files may be found in child directories when appropriate, e.g. to include C# language rules.
- Projects that are intended to be opened using VS Code must add the `EditorConfig.EditorConfig` VS Code extension in the `.vscode/extensions.json` file.

### package.json

- Node.JS projects must have a `package.json` file.
- This file should be based on the `package.json` file in the assets directory of this repository.
  - npm packages must set `private` to `false`.

### .renovaterc.json

- Each repository must have a `.renovaterc.json` file in the root directory.
- This file should be based on the `.renovaterc.json` file in the assets directory of this repository.
- If a project is considered to be stable, and has full unit and end-to-end test coverage, then this file may be updated to auto-merge using the "branch" strategy
  - Some dependencies may be configured to only auto-merge for minor, patch, pin, and digest increments.

### .cspell.json

- Node.JS projects should have a `.cspell.json` file in the root directory.
  - Such projects should also add the `streetsidesoftware.code-spell-checker` VS Code extension in the `.vscode/extensions.json` file.
  -  Such projects should also install `cspell` as a dev-dependency in their `package.json` file.
- This file should be based on the `.cspell.json` file in the assets directory of this repository.
  - The `dictionaries` key should be updated to include relevant dictionaries for the project. Potential examples include: "fullstack", "node", "npm", "html", "html-symbol-entities", "css", "typescript", "dotnet", "csharp".
  - The `ignorePaths` key should be updated to include generated files including build outputs, logs, and coverage reports.

### .eslintrc.json

- Projects that contain TypeScript and/or JavaScript must contain an `eslintrc.json` file in the same directory as the `package.json` file.
- This file should be generated by installing `eslint` as a dev-dependency and then running the ESLint CLI using `npm init @eslint/config`.
  - Projects that have an eslintrc file generated as a part of their framework's initial setup may keep that configuration.
- This file should be extended by installing `eslint-config-prettier` as a dev-dependency and then adding `prettier` to the `extends` section.
- Projects that have an ESLint configuration must also list the `dbaeumer.vscode-eslint` VS Code extension in the `.vscode/extensions.json` file.

### .prettierrc.json

- Projects should ideally not use a local Prettier configuration file.
- Node.JS projects should have `@tobysmith568/prettier-config` installed as a dev-dependency and should have `"prettier": "@tobysmith568/prettier-config"` listed in the `package.json` file.
	- If a project requires different rules to the shared configuration, then it may overwrite specific rules in a file called .prettierrc.json.
- Projects that use Prettier must have the `esbenp.prettier-vscode` VS Code extension in the `.vscode/extensions.json` file.

### .licenses.json

- Node.JS projects must have a `licenses.json` file in the root directory.
  - Such projects should also have the `license-cop` and `@license-cop/permissive` packages installed as dev-dependencies.
- This file should be called `.licenses.json`, though it may be called `.licenses.jsonc` if it contains comments.
- This file should be based on the `.licenses.json` file in the assets directory of this repository.
  - The config should extend `@license-cop/permissive`.
  - The config may include other licenses and/or packages where required.

### tsconfig.json

- Projects that use TypeScript must have a `tsconfig.json` file in the same directory as the `package.json` file.
- `strict` must be set to `true`.
- To allow for test files to be linted, `"include"` should contain globs for both the src and test files.
- To avoid test files being included in builds, there should be a second file called `tsconfig.build.json` that extends the former.
  - This file should have a `"exclude"` key that should contain globs for all test files and an `"include"` key that should contain globs for all src files only.
  - This file should be passed into build tools or the TypeScript CLI directly using the `-p` flag.

## GitHub Actions

### CI / CD

- Each repository should have GitHub Action CI/CD pipelines.
- Depending on the delivery method, the pipelines may either be different jobs in the same file named "ci-cd.yml", or in separate files named "ci.yml" and "cd.yml".
- The CI pipeline should run on all appropriate operating systems.
  - Websites only need to be run on the environment they'll be deployed to, e.g. dockerised websites only need to run on linux-latest.
  - Packages should be run anywhere that a consumer might run the package; this should normally be linux-latest, windows-latest, and macos-latest.
- The CI pipeline for packages should also be run using all current and LTS versions of relevant frameworks, e.g. Node.JS or .NET.
- In projects that use ESLint, the CI pipeline should run the ESLint CLI using `npx eslint . --max-warnings 0`.
- In projects that use Prettier, the CI pipeline should run the Prettier CLI using `npx prettier --check .`.
- In projects that use CSpell, the CI pipeline should run the CSpell CLI using `npx cspell "**/*.*"`.
- In projects that use License-Cop, the CI pipeline should run the License-Cop CLI using `npx license-cop`.
- The CI pipeline should report code coverage to CodeCov.

### codeql-analysis.yml

- Each repository must have a `codeql-analysis.yml` GitHub Action.
- This file should be generated by GitHub via the `/security/code-scanning` page for the repository.

## Node.JS and web projects

### Package manager

- Node.JS and Web projects should use npm.

### Frontend frameworks

- Any frontend frameworks may be used.
  - Current preferences are:
    - Next.JS for small projects.
    - Angular for large/data-intense projects.
    - Alpine.JS for static files.
  
### Styling

- React projects should use Emotion using the Styled Components syntax.
- Other projects should use Emotion or Sass where possible.
  - Projects using Sass must use the SCSS syntax.
- Vanilla CSS must otherwise be used where the previous two cannot be.

### Unit tests

- Projects should use Jest as their test runner, assertion library, and mocking library.
  - Test files must be called "x.spec.ts(x)" where x is the name of the file under test.
  - `describe()` blocks must be used.
    - When testing classes they must be used once for the class name and then once for each method under test.
    - When testing components they must be used once for the component name and then once for each piece of functionality under test.
    - For other files they must be used once for the file name and then once for each exported method under test.
  - Tests must use `it()` rather than `test()`.
  - Test names must begin with the word "should".
  - React projects must use React Testing Library.
    - Enzyme must not be used.
  - Projects that do not have a Jest configuration as a part of their boilerplate should be set up using ts-jest.

## C# Projects

### .csproj

- Projects should have nullable set to enable.
  - Test projects may have it set to disable.
- Projects should have implicit usings set to enable.
- Projects must explicitly define their lang version.
  - This should be kept as modern as possible.
- Global usings must be declared in .csproj files with the `<Using />` tag.
  - Global usings must not be declared in .cs files.

### Unit tests

- Unit test projects should be named "x.Tests" where "x" is the name of the project under test.
- Unit test projects should use the NUnit test framework.
- Unit test projects should use the FluentAssertions assertions library.
- Unit test projects should use the Moq mocking library.
- Test names must begin with "Should_" and should then be pascal case.