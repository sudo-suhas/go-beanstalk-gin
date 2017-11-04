## Create gin webapp

1. [Environment setup](#environment-setup)
1. [Initialise git repo](#initialise-git-repo)
1. [Setup git meta files](#setup-git-meta-files)
1. [Implement the webapp](#implement-the-webapp)
1. [`dep` init](#dep-init)
1. [Update `gin` version contraint](#update-gin-version-contraint)

After you have completed all the steps, your repo should look something like
this - [`create-gin-webapp`]

### Environment setup

- Install Go from here - https://golang.org/dl/
- Install `dep`, a Go dependency management tool using instructions described
  here - https://github.com/golang/dep#setup.

### Initialise git repo

Create the project folder inside your Go workspace and initialise the git repo:

```shell
$ mkdir -p $GOPATH/src/github.com/<USERNAME>/go-beanstalk-gin && cd $_

$ git init
Initialized empty Git repository in E:/go/src/github.com/sudo-suhas/go-beanstalk-gin/.git/

$ git commit --allow-empty -m "chore(git): init"
[master (root-commit) ff509aa] chore(git): init

```

You can read more about initialising a git repo here -
https://kevin.deldycke.com/2010/05/initialize-git-repositories/.

### Setup git meta files

Add git meta files `.gitignore` and `.gitattributes`. I use a modified version
of [github/gitignore:Go.gitignore@`master`] to also ignore the `vendor`
directory. If you are on windows, it's pretty important to add the
`.gitattributes` file:

```
* text eol=lf

```

And run this for good measure:

```shell
$ git config core.autocrlf false
```

This ensures that all your files in the repo use `LF` and not `CRLF`.
You can read more about this here -
https://help.github.com/articles/dealing-with-line-endings/. Unfortunately,
when the command line took for Elastic Beanstalk packages and deploys your
application, it does not handle the line endings and this can cause issues on
linux.

### Implement the webapp

For this demo, I'll use a basic starting template from `gin` examples:

```shell
λ curl https://raw.githubusercontent.com/gin-gonic/gin/master/examples/basic/main.go > application.go
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1213  100  1213    0     0   2027      0 --:--:-- --:--:-- --:--:--  2411
```

### `dep` init

Initialise `dep` to be able to vendor the dependencies:

```shell
$ dep init
  Locking in master (661970f) for transitive dep golang.org/x/sys
  Using ^1.2.0 as constraint for direct dep github.com/gin-gonic/gin
  Locking in v1.2 (d459835) for direct dep github.com/gin-gonic/gin
  Locking in master (22d885f) for transitive dep github.com/gin-contrib/sse
  Locking in master (1643683) for transitive dep github.com/golang/protobuf
  Locking in v2 (eb3733d) for transitive dep gopkg.in/yaml.v2
  Locking in master (d03304b) for transitive dep github.com/ugorji/go
  Locking in v8.18.2 (5f1438d) for transitive dep gopkg.in/go-playground/validator.v8
  Locking in v0.0.3 (0360b2a) for transitive dep github.com/mattn/go-isatty
```

This creates a `Gopkg.toml` and `Gopkg.lock` file and populates the vendor
directory:

```shell
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        Gopkg.lock
        Gopkg.toml

nothing added to commit but untracked files present (use "git add" to track)
```

After this, you can run the application:

```shell
$ go run application.go
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /ping                     --> main.main.func1 (3 handlers)
[GIN-debug] GET    /user/:name               --> main.main.func2 (3 handlers)
[GIN-debug] POST   /admin                    --> main.main.func3 (4 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```

### Update `gin` version contraint

On intialising `dep`, it automatically locks `gin` to `v1.2.0` which is the
latest release. However, I want to use some of the changes currently in master.
Specifically, it is possible to use `jsoniter` for JSON
marshalling/unmarshalling via build tags. To do this, we need to edit the
`Gopkg.toml` file:

```diff
@@ -23,4 +23,4 @@

 [[constraint]]
   name = "github.com/gin-gonic/gin"
-  version = "1.2.0"
+  branch = "master"
```

Now run `dep ensure` to update `vendor`:

```shell
# -v enables verbose mode
$ dep ensure -v
Root project is "github.com/sudo-suhas/go-beanstalk-gin"
 1 transitively valid internal packages
 1 external packages imported from 1 projects
(0)   ✓ select (root)
(1)     ? attempt github.com/gin-gonic/gin with 1 pkgs; at least 1 versions to try
(1)         try github.com/gin-gonic/gin@v1.2
(2)     ✗   github.com/gin-gonic/gin@v1.2 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v1.1.4
(2)     ✗   github.com/gin-gonic/gin@v1.1.4 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v1.1.3
(2)     ✗   github.com/gin-gonic/gin@v1.1.3 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v1.1.2
(2)     ✗   github.com/gin-gonic/gin@v1.1.2 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v1.1.1
(2)     ✗   github.com/gin-gonic/gin@v1.1.1 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v1.1
(2)     ✗   github.com/gin-gonic/gin@v1.1 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v1.0
(2)     ✗   github.com/gin-gonic/gin@v1.0 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v0.6
(2)     ✗   github.com/gin-gonic/gin@v0.6 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v0.5
(2)     ✗   github.com/gin-gonic/gin@v0.5 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v0.4
(2)     ✗   github.com/gin-gonic/gin@v0.4 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v0.3
(2)     ✗   github.com/gin-gonic/gin@v0.3 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v0.1
(2)     ✗   github.com/gin-gonic/gin@v0.1 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@v1.0-rc.2
(2)     ✗   github.com/gin-gonic/gin@v1.0-rc.2 not allowed by constraint master:
(2)         master from (root)
(1)         try github.com/gin-gonic/gin@master
(1)     ✓ select github.com/gin-gonic/gin@master w/4 pkgs
(2)     ? attempt github.com/gin-contrib/sse with 1 pkgs; at least 1 versions to try
(2)         try github.com/gin-contrib/sse@master
(2)     ✓ select github.com/gin-contrib/sse@master w/1 pkgs
(3)     ? attempt github.com/golang/protobuf with 1 pkgs; at least 1 versions to try
(3)         try github.com/golang/protobuf@master
(3)     ✓ select github.com/golang/protobuf@master w/1 pkgs
(4)     ? attempt gopkg.in/go-playground/validator.v8 with 1 pkgs; at least 1 versions to try
(4)         try gopkg.in/go-playground/validator.v8@v8.18.2
(4)     ✓ select gopkg.in/go-playground/validator.v8@v8.18.2 w/1 pkgs
(5)     ? attempt github.com/mattn/go-isatty with 1 pkgs; at least 1 versions to try
(5)         try github.com/mattn/go-isatty@v0.0.3
(5)     ✓ select github.com/mattn/go-isatty@v0.0.3 w/1 pkgs
(6)     ? attempt github.com/ugorji/go with 1 pkgs; at least 1 versions to try
(6)         try github.com/ugorji/go@master
(6)     ✓ select github.com/ugorji/go@master w/1 pkgs
(7)     ? attempt golang.org/x/sys with 1 pkgs; at least 1 versions to try
(7)         try golang.org/x/sys@master
(7)     ✓ select golang.org/x/sys@master w/1 pkgs
(8)     ? attempt gopkg.in/yaml.v2 with 1 pkgs; at least 1 versions to try
(8)         try gopkg.in/yaml.v2@v2
(8)     ✓ select gopkg.in/yaml.v2@v2 w/1 pkgs
(9)     ? attempt github.com/json-iterator/go with 1 pkgs; 15 versions to try
(9)         try github.com/json-iterator/go@1.0.3
(9)     ✓ select github.com/json-iterator/go@1.0.3 w/1 pkgs
  ✓ found solution with 12 packages from 9 projects

Solver wall times by segment:
              b-gmal: 19.2996609s
     b-list-versions:  9.3609352s
         b-list-pkgs:  8.3195198s
     b-source-exists:  1.7614697s
             satisfy:  489.3494ms
            new-atom:  460.6403ms
         select-atom:    2.9788ms
         select-root:    2.0005ms
  b-deduce-proj-root:          0s
               other:          0s
          b-pair-rev:          0s
      b-pair-version:          0s
           b-matches:          0s

  TOTAL: 39.6965546s

(1/9) Wrote gopkg.in/yaml.v2@v2
(2/9) Wrote gopkg.in/go-playground/validator.v8@v8.18.2
(3/9) Wrote github.com/mattn/go-isatty@v0.0.3
(4/9) Wrote github.com/gin-contrib/sse@master
(5/9) Wrote github.com/golang/protobuf@master
(6/9) Wrote github.com/ugorji/go@master
(7/9) Wrote github.com/gin-gonic/gin@master
(8/9) Wrote golang.org/x/sys@master
(9/9) Wrote github.com/json-iterator/go@1.0.3
```

[`create-gin-webapp`]: https://github.com/sudo-suhas/go-beanstalk-gin/tree/create-gin-webapp
[github/gitignore:Go.gitignore@`master`]: https://github.com/github/gitignore/blob/master/Go.gitignore
