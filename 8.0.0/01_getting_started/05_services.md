# Services

--------------------------------------------------------

- [Framework Services](#framework-services)
- [App Services](#app-services)
- [CLI Services](#cli-services)

--------------------------------------------------------

Services are an easy and clean way of registering dependencies for you application. Services be instantiated at runtime or when first called explicitly depending on how they are configured via the IoC Container.

Serve includes a number of services for your convenience and you'll find a complete list in the `app/configurations/application.php` file. You can add your own or use the existing ones.

Services are split up in 3 groups. `framework`, `app` and `cli`. Note that if you are not running serve via a `CLI`, the `cli` services will not be available.

All Serve core decencies are registered as services, however there are additional utility dependencies that use static methods that do not require a service. These are documented in the Learn More section of this documentation.

--------------------------------------------------------

### Framework Services

| Service      | Application Access     |
|:-------------|:-----------------------|
| Access       | `$serve->Access`       |
| Application  | `$serve->Application`  |
| Cache        | `$serve->Cache`        |
| Config       | `$serve->Config`       |
| Cookie       | `$serve->Cookie`       |
| Crypto       | `$serve->Crypto`       |
| Database     | `$serve->Database`     |
| Deployment   | `$serve->Deployment`   |
| ErrorHandler | `$serve->ErrorHandler` |
| Events       | `$serve->Events`       |
| Filesystem   | `$serve->Filesystem`   |
| Filters      | `$serve->Filters`      |
| Gatekeeper   | `$serve->Gatekeeper`   |
| Onion        | `$serve->Onion`        |
| Pixl         | `$serve->Pixl`         |
| Request      | `$serve->Request`      |
| Response     | `$serve->Response`     |
| Router       | `$serve->Router`       |
| Session      | `$serve->Session`      |
| Spam         | `$serve->Spam`         |
| UserAgent    | `$serve->UserAgent`    |
| Validator    | `$serve->Validator`    |
| View         | `$serve->View`         |

--------------------------------------------------------

### CLI Services

| Service      | Application Access     |
|:-------------|:-----------------------|
| Input        | `$serve->Input`        |
| Output       | `$serve->Output`       |
| Cli          | `$serve->Cli`          |
| Console      | `$serve->Console`      |

--------------------------------------------------------

### App Services
App services are for registering custom dependencies inside your own application. Please see [Registering Services](/8.0.0/01_getting_started/04_dependency_injection#registering-services) for more details on how to register your own dependencies.

