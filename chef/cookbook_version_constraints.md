# Cookbook version constraints

When releasing a potentially breaking change to a [Chef][0] cookbook, you
might want to prevent that change rolling out to production servers but test
it in a staging environment.

One way to do this is to use cookbook version constraints on your
[environments][1] directly:

```shell
$ knife environment edit production
```

```json
{
  "cookbook_versions": {
    "my_cookbook": "= 0.1.2"
  }
}
```

This will prevent any version of `my_cookbook` except `0.1.2` being applied to
a production node. As Chef will download the latest version of a cookbook by
default, this means that other environments will get the new cookbook for
testing.

[0]: https://www.chef.io/
[1]: http://docs.chef.io/environments.html
