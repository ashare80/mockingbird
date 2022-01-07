# Common Problems

Look up solutions to common issues.

## Overview

Need help? Join the [#mockingbird Slack channel](https://join.slack.com/t/birdopensource/shared_invite/zt-wogxij50-3ZM7F8ZxFXvPkE0j8xTtmw) or
[file an issue](https://github.com/birdrides/mockingbird/issues/new/choose).

> Note: The problems below are ordered by how far in the setup process they would occur.

### Mocks don’t exist or are out of date

Mocks are generated when the test target is built. Build for testing (⇧⌘U) or run any test case (⌘U) and check that generated mock files appear in `$(SRCROOT)/MockingbirdMocks`. If no files are generated then something is wrong with the installation. See <doc:Debugging-the-Generator> for more information.

### The generated file contains no mocks

By default, mocks are only generated for types used in your tests; see <doc:Thunk-Pruning> for more information. Try mocking a type with `mock(SomeType.self)` and then build for testing (⇧⌘U). If no mocks appear in the generated file then something is wrong with the installation. See <doc:Debugging-the-Generator> for more information.

### Compiler error in generated mock file

You may have found a (rare) generator bug. Try <doc:Debugging-the-Generator>, <doc:Excluding-Files>, or [filing an issue](https://github.com/birdrides/mockingbird/issues/new/choose).

### Supporting source files do not compile

Supporting source files should not be imported into Xcode. If you want to use Xcode to add or modify supporting source files, make sure they are not added as sources to any targets.

### Mocks are not generated for external types in third-party frameworks or libraries

Please see <doc:Mocking-External-Types> for details and best practices.

### Trying to mock a type shows a compiler error

There are a few common issues that might prevent generated types from being mocked. See <doc:Generator-Diagnostics> for a list of compile time errors generated by Mockingbird and how to resolve them.

### Cannot call stubbing or verification functions

Ensure that Mockingbird is imported at the top of the test file.

### Expression type is ambiguous without more context error

This usually happens when trying to stub or verify a mock that was explicitly coerced into its supertype. Make sure the variable storing the mock has the concrete mock type, e.g. `MyTypeMock` instead of `MyType`.

### Tests crash with an unable to load framework, image not found error

Link Mockingbird and ensure that it’s included in the test bundle by adding it to the “Copy Files” build phase.

### Tests crash with a fatal error containing a message about “thunk stubs”

Mockingbird only generates mocking code (known as thunks) for types referenced in tests. Some projects that indirectly reference types may incorrectly encounter thunk stubs in tests. See <doc:Thunk-Pruning> for more information and resolution steps.

### Unable to stub or verify methods with parameters

Ensure that all parameter types explicitly conform to `Equatable` or can be compared by reference. Note that `struct` types that _implicitly_ conform to `Equatable` have undefined behavior. Use a wildcard argument matcher such as `any()` or `any(where:)` to match non-equatable or implicitly equatable types. See <doc:Matching-Arguments> for all available matchers.