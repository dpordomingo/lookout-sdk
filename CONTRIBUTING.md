# Contribution Guidelines

As all source{d} projects, this project follows the
[source{d} Contributing Guidelines](https://github.com/src-d/guide/blob/master/engineering/documents/CONTRIBUTING.md).

## Additional Contribution Guidelines

In addition to the [source{d} Contributing Guidelines](https://github.com/src-d/guide/blob/master/engineering/documents/CONTRIBUTING.md),
this project follows the following guidelines.

### Generated Code

Before submitting a pull request make sure all the generated code changes are also committed.

### Dependencies

Go dependencies are managed with [dep](https://golang.github.io/dep/). Use `make godep` to make sure the `vendor` directory is up to date, and commit any necessary changes.

### TOC

Please update the readme Table of Contents with:
```bash
make toc
```

Then remove the first link for the `Table of Contents` section itself.


### Release Process

 - Make sure the code is up to date using `make protogen`.
 - Update `VERSION` in `python/setup.py` with the same version that you will use for the tag (manual step required until [#2](https://github.com/src-d/lookout-sdk/issues/2) is implemented).
 - Create the release tag.
