
# Deploy helm charts using ksonnet

### Create new branch

For every new feature create a new git branch from the master.
For ex: creating a *postgres* component.
```
git checkout -b postgres
```

### Setup ks to work with Helm 

Follow [this](https://ksonnet.io/docs/examples/helm/) tutorial and mind the below notes for things to work.

Note: Run the commands from inside the `ks` folder else it will throw error.   
> ERROR finding app root from starting path: : unable to find ksonnet project

### Get all helm charts list

```
ks registry describe helm-stable
```
**Note:** Use ks version `0.13.0` when working with helm charts else you won't be able to install the packages. For other work use the latest version `0.13.1`.

### Show the manifest

After following the tutorial, once you generate the component using the `ks generate` command. Use the following to show the manifest.
```
ks show default -c postgres
```

**Note:** `postgres` is the name of component given while generating the prototype.
You can also list down all the components first.
```
ks component list
```

### Modify manifest values

Edit the `params.libsonnet` file and move to the component you want to edit.
Add the parameters by referring to helm chart [README.md](https://github.com/helm/charts/tree/master/stable/zeppelin#configuration) page and add values in the JSON format.

For ex:
> Before
```
zeppelin: {
      name: "zeppelin",
      values: {},
      version: "1.1.0",
    },
  },
```

> After  
```
zeppelin: {
      name: "zeppelin",
      values: {
        zeppelin: {
         resources: {
         limits: {
                memory: '2048Mi',
                cpu: '1000m',
                },
	        },
          },
        },
      version: "1.1.0",
    },
  },
```

Now again use the `show` command to check the modified manifest.
```
ks show default -c postgres
```

### Deploy the manifest to K8s cluster

Make sure ksonnet is pointing to correct env by listing the env
```
ks env list
```

If the default env requires changes then use

```
ks env set default --server=<serverIP>
```


Refer the ksonnet [documentation](https://ksonnet.io/docs/cli-reference/#ks-envks-env-set).
