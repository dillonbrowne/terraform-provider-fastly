# Fastly Terraform Provider

- Website: https://www.terraform.io
- Documentation: https://registry.terraform.io/providers/fastly/fastly/latest/docs
- Mailing list: http://groups.google.com/group/terraform-tool
- [![Gitter chat](https://badges.gitter.im/hashicorp-terraform/Lobby.png)](https://gitter.im/hashicorp-terraform/Lobby)

Requirements
------------

-	[Terraform](https://www.terraform.io/downloads.html) 0.12.x or higher 
-	[Go](https://golang.org/doc/install) 1.14 (to build the provider plugin)

> NOTE: the last version of the Fastly provider to support Terraform 0.11.x and below was [v0.26.0](https://github.com/fastly/terraform-provider-fastly/releases/tag/v0.26.0)

## Building The Provider

Clone repository to: `$GOPATH/src/github.com/fastly/terraform-provider-fastly`

```sh
$ mkdir -p $GOPATH/src/github.com/fastly; cd $GOPATH/src/github.com/fastly
$ git clone git@github.com:fastly/terraform-provider-fastly
```

Enter the provider directory and build the provider

```sh
$ cd $GOPATH/src/github.com/fastly/terraform-provider-fastly
$ make build
```

## Developing the Provider

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (version 1.14+ is *required*).

To compile the provider, run `make build`. This will build the provider and put the provider binary in a local `bin` directory.

```sh
$ make build
...
```

Alongside the newly built binary a file called `developer_overrides.tfrc` will be created.  The `make build` target will communicate 
back details for setting the `TF_CLI_CONFIG_FILE` environment variable that will enable Terraform to use your locally built provider binary.
* HashiCorp - [Development Overrides for Provider developers](https://www.terraform.io/docs/cli/config/config-file.html#development-overrides-for-provider-developers). 

## Testing

In order to test the provider, you can simply run `make test`.

```sh
$ make test
```

In order to run the full suite of Acceptance tests, run `make testacc`.

*Note:* Acceptance tests create real resources, and often cost money to run. You should expect that the full acceptance test suite will take hours to run.

```sh
$ make testacc
```

In order to run an individual acceptance test, the '-run' flag can be used together with a regular expression.
The following example uses a regular expression matching single test called 'TestAccFastlyServiceV1_basic'.

```sh
$ make testacc TESTARGS='-run=TestAccFastlyServiceV1_basic'
```

The following example uses a regular expression to execute a grouping of basic acceptance tests.

```sh
$ make testacc TESTARGS='-run=TestAccFastlyServiceV1_.*_basic'
```

In order to run the tests with extra debugging context, prefix the `make` command with `TF_LOG` (see the [terraform documentation](https://www.terraform.io/docs/internals/debugging.html) for details).

```sh
$ TF_LOG=trace make testacc
```

By default, the tests run with a parallelism of 4.
This can be reduced if some tests are failing due to network-related issues, or increased if possible, to reduce the running time of the tests.
Prefix the `make` command with `TEST_PARALLELISM`, as in the following example, to configure this.

```sh
$ TEST_PARALLELISM=8 make testacc
```

Depending on the Fastly account used, some features may not be enabled (e.g. Platform TLS).
This may result in some tests failing, potentially with `403 Unauthorised` errors, when the full test suite is being run.
Check the [Fastly API documentation](https://developer.fastly.com/reference/api/) to confirm if the failing tests use features in Limited Availability or only available to certain customers.
If this is the case, either use the `TESTARGS` regular expressions described above, or temporarily add `t.SkipNow()` to the top of any tests that should be excluded. 

## Building The Documentation

The documentation is built from components (go templates) stored in the `templates` folder.
Building the documentation copies the full markdown into the `docs` folder, ready for deployment to Hashicorp.

> NOTE: you'll need the [`tfplugindocs`](https://github.com/hashicorp/terraform-plugin-docs) tool for generating the Markdown to be deployed to Hashicorp. For more information on generating documentation, refer to https://www.terraform.io/docs/registry/providers/docs.html

* To validate the `/template` directory structure:
```
make validate-docs
```

* To build the `/docs` documentation Markdown files:
```
make generate-docs
```

* To view the documentation:
Paste `/docs` Markdown file content into https://registry.terraform.io/tools/doc-preview

## Contributing

Refer to [CONTRIBUTING.md](./CONTRIBUTING.md)
