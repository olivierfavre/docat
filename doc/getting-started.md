## Getting started with DOCAT

### docatl, the docat CLI 🙀

The most convenient way to interact with docat is with it's official CLI tool, [docatl](https://github.com/docat-org/docatl).

You can download a standalone binary of the latest release for your platform [here](https://github.com/docat-org/docatl/releases/latest) or [use go](https://github.com/docat-org/docatl#using-go) or [docker](https://github.com/docat-org/docatl#using-docker) to install it.

After you've installed `docatl`, just point it to your documentation folder, which will be packaged and pushed to `docat`.
To do this, use the following command:

```sh
docatl push --host http://localhost:8000 ./docs/ awesome-project 1.0.0
```

Use `docatl --help` to discover all commands available to manage your `docat` documentation!

### Raw API endpoints

The following sections document the RAW API endpoints you can `curl`.

The API specification is exposed as an OpenAPI Documentation at http://localhost:8000/api/v1/openapi.json, 
via Swagger UI at http://localhost:8000/api/docs and 
as a pure documentation with redoc at http://localhost:8000/api/redoc.

#### Upload your documentation

You can upload any static HTML page by zipping it and uploading the zip file.

> Note: if an `index.html` file is present in the root of the zip file
  it will be served automatically.

For example to upload the file `docs.zip` as version `1.0.0` for `awesome-project` using `curl`:

```sh
curl -X POST -F "file=@docs.zip" http://localhost:8000/api/awesome-project/1.0.0
```

Any file type can be uploaded. To view an uploaded pdf, specify it's full path:

`http://localhost:8000/#/awesome-project/1.0.0/my_awesome.pdf`

You can also manually upload your documentation.
A very simple web form can be found under [upload](#/upload).

#### Tag documentation

After uploading you can tag a specific version. This can be useful when
the latest version should be available as `http://localhost:8000/docs/awesome-project/latest`

To tag the version `1.0.0` as `latest` for `awesome-project`:

```sh
curl -X PUT http://localhost:8000/api/awesome-project/1.0.0/tags/latest
```

#### Claim Project

Claiming a Project returns a `token` which can be used for actions
which require authentication (for example for deleting a version).
Each Project can be claimed **exactly once**, so best store the token safely.

```sh
curl -X GET http://localhost:8000/api/awesome-project/claim
```

#### Authentication

To make an authenticated call, specify a header with the key `Docat-Api-Key` and your token as the value:

```sh
curl -X DELETE --header "Docat-Api-Key: <token>" http://localhost:8000/api/awesome-project/1.0.0
```

#### Delete Version

To delete a Project version you need to be authenticated.

To remove the version `1.0.0` from `awesome-project`:

```sh
curl -X DELETE --header "Docat-Api-Key: <token>" http://localhost:8000/api/awesome-project/1.0.0
```

#### Upload Project Icon

To upload a icon, you don't need a token, except if you want to replace an existing icon.

To set `example-image.png` as the icon for `awesome-project`, which already has an icon:

```sh
curl -X POST -F "file=@example-image.png" --header "Docat-Api-Key: <token>" http://localhost:8000/api/awesome-project/icon
```
