# AtlasExample

## Create ``atlas.workspace`` file using ``atlas init``.

Modify this file as needed (there are multiple options described in the docs: https://nim-lang.org/docs/atlas.html). For example, in this case I choose to have my dependencies in a ``deps`` folder.


## Create a local project :

```sh
mkdir ProjectA/
cd ProjectA/
nimble init
```
Note that I find the ``src/`` folder confusing and recommend against using it, so I will modify my nimble file by removing the ``srcDir`` and "flattening" the repo first.

Then I will modify the nimble file with my dependencies. In this case, I added ``threading``, ``arraymancer``, ``timelog``.

Then I run ``atlas install ./ProjectA.nimble``.
This will :
* Clone your dependencies in ``deps/`` folder
* Create a local ``nim.cfg`` which update --path options

You can check that dependencies are found by importing them in the tests. Note that other projects will not be aware of ProjectA dependencies when setup that way.

## Create a "workspace" project: 

For ProjectA/ the nim.cfg files was locally inside the folder. Using atlas you can also create a "workspace" nimble file, that all sub-project should be able to find and depend on.


For example : 

```sh
atlas use norm
atlas use db_connector
```

This will create ``AtlasExample.nimble`` and ``nim.cfg``.

Now, because ``norm`` was added to the **workspace**, all project will be aware of it. This is the closest equivalent to ``nimble develop``.


## Locking your dependencies

To create the atlas.lock file:

```sh
cd deps/
atlas pin
```

Commit the lock file created. 

If you need to recreate the same workspace, just use ``atlas rep`` command:

```sh
cd deps/
atlas rep
```

To update every dependency and the lock file simply run :

```sh
atlas updateDeps
cd deps/
atlas pin
```
