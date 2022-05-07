# Impact GO Work Heroku Buildpack

A Heroku buildpack that configures a GO project built on the new 1.18 workspace.

## Detailed Description

This buildpack will setup Heroku GO build directives and the GO procfile based on environment variables.  This environmental variable solution will allow a monorepo relying on a GO workspace to build multiple different dynos.

This buildpack will not compile GO code or complete a release. It is intended as a pre-processor for the standard GO work buildpack.

### Environment Variables

2 environment variables are expected, both pointing to files within the GO project itself.

#### `HEROKU_WORK_DIRECTIVES_FILE`
This variable points to a file containing Heroku build directives that will be added to the `go.work` file.

For example:
> HEROKU_WORK_DIRECTIVES_FILE=project/heroku-directives.txt

The `project/heroku-directives.txt` file contains the following content
```
// +heroku goVersion go1.18
// +heroku install ./cmd/...
```

The `go.work` file also has the placeholder `HEROKU_WORK_DIRECTIVES` added.

The build will replace the placeholder variable `HEROKU_WORK_DIRECTIVES` with the content of the `project/heroku-directives.txt` file.

#### `HEROKU_WORK_PROCFILE`
This variable points to a file containing the Heroku procfile contents.

For example:
> HEROKU_WORK_PROCFILE=project/Project-Procfile

The `project/Project-Procfile` file contains the following content
```
web: server
```

The contents of this file are copied to the root of the project in a new file named `Procfile`.
