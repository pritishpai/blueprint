# Version changelog

## 0.4.1

* Fixed `MockInstallation` to emulate workspace-global setup ([#69](https://github.com/databrickslabs/blueprint/issues/69)). In this release, the `MockInstallation` class in the `installation` module has been updated to better replicate a workspace-global setup, enhancing testing and development accuracy. The `is_global` method now utilizes the `product` method instead of `_product`, and a new instance variable `_is_global` with a default value of `True` is introduced in the `__init__` method. Moreover, a new `product` method is included, which consistently returns the string "mock". These enhancements resolve issue [#69](https://github.com/databrickslabs/blueprint/issues/69), "Fixed `MockInstallation` to emulate workspace-global setup", ensuring the `MockInstallation` instance behaves as a global installation, facilitating precise and reliable testing and development for our software engineering team.
* Improved `MockPrompts` with `extend()` method ([#68](https://github.com/databrickslabs/blueprint/issues/68)). In this release, we've added an `extend()` method to the `MockPrompts` class in our library's TUI module. This new method allows developers to add new patterns and corresponding answers to the existing list of questions and answers in a `MockPrompts` object. The added patterns are compiled as regular expressions and the questions and answers list is sorted by the length of the regular expression patterns in descending order. This feature is particularly useful for writing tests where prompt answers need to be changed, as it enables better control and customization of prompt responses during testing. By extending the list of questions and answers, you can handle additional prompts without modifying the existing ones, resulting in more organized and maintainable test code. If a prompt hasn't been mocked, attempting to ask a question with it will raise a `ValueError` with an appropriate error message.
* Use Hatch v1.9.4 to as build machine requirement ([#70](https://github.com/databrickslabs/blueprint/issues/70)). The Hatch package version for the build machine requirement has been updated from 1.7.0 to 1.9.4 in this change. This update streamlines the Hatch setup and version management, removing the specific installation step and listing `hatch` directly in the required field. The pre-setup command now only includes "hatch env create". Additionally, the acceptance tool version has been updated to ensure consistent project building and testing with the specified Hatch version. This change is implemented in the acceptance workflow file and the version of the acceptance tool used by the sandbox. This update ensures that the project can utilize the latest features and bug fixes available in Hatch 1.9.4, improving the reliability and efficiency of the build process. This change is part of the resolution of issue [#70](https://github.com/databrickslabs/blueprint/issues/70).


## 0.4.0

* Added commands with interactive prompts ([#66](https://github.com/databrickslabs/blueprint/issues/66)). This commit introduces a new feature in the Databricks Labs project to support interactive prompts in the command-line interface (CLI) for enhanced user interactivity. The `Prompts` argument, imported from `databricks.labs.blueprint.tui`, is now integrated into the `@app.command` decorator, enabling the creation of commands with user interaction like confirmation prompts. An example of this is the `me` command, which confirms whether the user wants to proceed before displaying the current username. The commit also refactored the code to make it more efficient and maintainable, removing redundancy in creating client instances. The `AccountClient` and `WorkspaceClient` instances can now be provided automatically with the product name and version. These changes improve the CLI by making it more interactive, user-friendly, and adaptable to various use cases while also optimizing the codebase for better efficiency and maintainability.
* Added more code documentation ([#64](https://github.com/databrickslabs/blueprint/issues/64)). This release introduces new features and updates to various files in the open-source library. The `cli.py` file in the `src/databricks/labs/blueprint` directory has been updated with a new decorator, `command`, which registers a function as a command. The `entrypoint.py` file in the `databricks.labs.blueprint` module now includes a module-level docstring describing its purpose, as well as documentation for the various standard libraries it imports. The `Installation` class in the `installers.py` file has new methods for handling files, such as `load`, `load_or_default`, `upload`, `load_local`, and `files`. The `installers.py` file also includes a new `InstallationState` dataclass, which is used to track installations. The `limiter.py` file now includes code documentation for the `RateLimiter` class and the `rate_limited` decorator, which are used to limit the rate of requests. The `logger.py` file includes a new `NiceFormatter` class, which provides a nicer format for logging messages with colors and bold text if the console supports it. The `parallel.py` file has been updated with new methods for running tasks in parallel and returning results and errors. The `TUI.py` file has been documented, and includes imports for logging, regular expressions, and collections abstract base class. Lastly, the `upgrades.py` file has been updated with additional code documentation and new methods for loading and applying upgrade scripts. Overall, these changes improve the functionality, maintainability, and usability of the open-source library.
* Fixed init-project command ([#65](https://github.com/databrickslabs/blueprint/issues/65)). In this release, the `init-project` command has been improved with several bug fixes and new functionalities. A new import statement for the `sys` module has been added, and a `docs` directory is now included in the copied directories and files during initialization. The `init_project` function has been updated to open files using the default system encoding, ensuring proper reading and writing of file contents. The `relative_paths` function in the `entrypoint.py` file now returns absolute paths if the common path is the root directory, addressing issue [#41](https://github.com/databrickslabs/blueprint/issues/41). Additionally, several test functions have been added to `tests/unit/test_entrypoint.py`, enhancing the reliability and robustness of the `init-project` command by providing comprehensive tests for supporting functions. Overall, these changes significantly improve the functionality and reliability of the `init-project` command, ensuring a more consistent and accurate project initialization process.
* Using `ProductInfo` with integration tests ([#63](https://github.com/databrickslabs/blueprint/issues/63)). In this update, the `ProductInfo` class has been enhanced with a new class method `for_testing(klass)` to facilitate effective integration testing. This method generates a new `ProductInfo` object with a random `product_name`, enabling the creation of distinct installation directories for each test execution. Prior to this change, conflicts and issues could arise when multiple test executions shared the same integration test folder. With the introduction of this new method, developers can now ensure that their integration tests run with unique product names and separate installation directories, enhancing testing isolation and accuracy. This update is demonstrated in the provided code snippet and includes a new test case to confirm the generation of unique product names. Furthermore, a pre-existing test case has been modified to provide a more specific error message related to the `SingleSourceVersionError`. This enhancement aims to improve the integration testing capabilities of the codebase and is designed to be easily adopted by other software engineers utilizing this project.


## 0.3.1

* Fixed the order of marshal to handle Dataclass with as_dict before other types to avoid SerdeError ([#60](https://github.com/databrickslabs/blueprint/issues/60)). In this release, we have addressed an issue that caused a SerdeError during the installation.save operation with a Dataclass object. The error was due to the order of evaluation in the _marshal_dataclass method. The order has been updated to evaluate the `as_dict` method first if it exists in the Dataclass, which resolves the SerdeError. To ensure the correctness of the fix, we have added a new test_data_class function that tests the save and load functionality with a Dataclass object. The test defines a Policy Dataclass with an `as_dict` method that returns a dictionary representation of the object and checks if the file is written correctly and if the loaded object matches the original object. This change has been thoroughly unit tested to ensure that it works as expected.


## 0.3.0

* Added automated upgrade framework ([#50](https://github.com/databrickslabs/blueprint/issues/50)). This update introduces an automated upgrade framework for managing and applying upgrades to the product, with a new `upgrades.py` file that includes a `ProductInfo` class having methods for version handling, wheel building, and exception handling. The test code organization has been improved, and new test cases, functions, and a directory structure for fixtures and unit tests have been added for the upgrades functionality. The `test_wheels.py` file now checks the version of the Databricks SDK and handles cases where the version marker is missing or does not contain the `__version__` variable. Additionally, a new `Application State Migrations` section has been added to the README, explaining the process of seamless upgrades from version X to version Z through version Y, addressing the need for configuration or database state migrations as the application evolves. Users can apply these upgrades by following an idiomatic usage pattern involving several classes and functions. Furthermore, improvements have been made to the `_trim_leading_whitespace` function in the `commands.py` file of the `databricks.labs.blueprint` module, ensuring accurate and consistent removal of leading whitespace for each line in the command string, leading to better overall functionality and maintainability.
* Added brute-forcing `SerdeError` with `as_dict()` and `from_dict()` ([#58](https://github.com/databrickslabs/blueprint/issues/58)). This commit introduces a brute-forcing approach for handling `SerdeError` using `as_dict()` and `from_dict()` methods in an open-source library. The new `SomePolicy` class demonstrates the usage of these methods for manual serialization and deserialization of custom classes. The `as_dict()` method returns a dictionary representation of the class instance, and the `from_dict()` method, decorated with `@classmethod`, creates a new instance from the provided dictionary. Additionally, the GitHub Actions workflow for acceptance tests has been updated to include the `ready_for_review` event type, ensuring that tests run not only for opened and synchronized pull requests but also when marked as "ready for review." These changes provide developers with more control over the deserialization process and facilitate debugging in cases where default deserialization fails, but should be used judiciously to avoid brittle code.
* Fixed nightly integration tests run as service principals ([#52](https://github.com/databrickslabs/blueprint/issues/52)). In this release, we have enhanced the compatibility of our codebase with service principals, particularly in the context of nightly integration tests. The `Installation` class in the `databricks.labs.blueprint.installation` module has been refactored, deprecating the `current` method and introducing two new methods: `assume_global` and `assume_user_home`. These methods enable users to install and manage `blueprint` as either a global or user-specific installation. Additionally, the `existing` method has been updated to work with the new `Installation` methods. In the test suite, the `test_installation.py` file has been updated to correctly detect global and user-specific installations when running as a service principal. These changes improve the testability and functionality of our software, ensuring seamless operation with service principals during nightly integration tests.
* Made `test_existing_installations_are_detected` more resilient ([#51](https://github.com/databrickslabs/blueprint/issues/51)). In this release, we have added a new test function `test_existing_installations_are_detected` that checks if existing installations are correctly detected and retries the test for up to 15 seconds if they are not. This improves the reliability of the test by making it more resilient to potential intermittent failures. We have also added an import from `databricks.sdk.retries` named `retried` which is used to retry the test function in case of an `AssertionError`. Additionally, the test function `test_existing` has been renamed to `test_existing_installations_are_detected` and the `xfail` marker has been removed. We have also renamed the test function `test_dataclass` to `test_loading_dataclass_from_installation` for better clarity. This change will help ensure that the library is correctly detecting existing installations and improve the overall quality of the codebase.


## 0.2.5

* Automatically enable workspace filesystem if the feature is disabled ([#42](https://github.com/databrickslabs/blueprint/pull/42)).


## 0.2.4

* Added more integration tests for `Installation` ([#39](https://github.com/databrickslabs/blueprint/pull/39)).
* Fixed `yaml` optional import error ([#38](https://github.com/databrickslabs/blueprint/pull/38)).


## 0.2.3

* Added special handling for notebooks in `Installation.upload(...)` ([#36](https://github.com/databrickslabs/blueprint/pull/36)).


## 0.2.2

* Fixed issues with uploading wheels to DBFS and loading a non-existing install state ([#34](https://github.com/databrickslabs/blueprint/pull/34)).


## 0.2.1

* Aligned `Installation` framework with UCX project ([#32](https://github.com/databrickslabs/blueprint/pull/32)).


## 0.2.0

* Added common install state primitives with strong typing ([#27](https://github.com/databrickslabs/blueprint/pull/27)).
* Added documentation for Invoking Databricks Connect ([#28](https://github.com/databrickslabs/blueprint/pull/28)).
* Added more documentation for Databricks CLI command router ([#30](https://github.com/databrickslabs/blueprint/pull/30)).
* Enforced `pylint` standards ([#29](https://github.com/databrickslabs/blueprint/pull/29)).


## 0.1.0

* Changed python requirement from 3.10.6 to 3.10 ([#25](https://github.com/databrickslabs/blueprint/pull/25)).


## 0.0.6

* Make `find_project_root` more deterministic ([#23](https://github.com/databrickslabs/blueprint/pull/23)).


## 0.0.5

* Make it work with `ucx` ([#21](https://github.com/databrickslabs/blueprint/pull/21)).


## 0.0.4

* Fixed sigstore action ([#19](https://github.com/databrickslabs/blueprint/pull/19)).


## 0.0.3

* Sign artifacts with Sigstore ([#17](https://github.com/databrickslabs/blueprint/pull/17)).


## 0.0.2

* Added extensive library documentation ([#14](https://github.com/databrickslabs/blueprint/pull/14)).
* Setup release to PyPI via GitHub OIDC ([#15](https://github.com/databrickslabs/blueprint/pull/15)).


## Release 0.0.1

* Added `.codegen.json` and `CHANGELOG.md` templates for automated releases.
* Added `CODEOWNERS` for code governance.
* Added command framework for Databricks CLI launcher frontend ([#10](https://github.com/databrickslabs/blueprint/pull/10)).
* Added ProductInfo unreleased version fallback ([#9](https://github.com/databrickslabs/blueprint/pull/9)).
