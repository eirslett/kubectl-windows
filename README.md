# Project archived

Kubernetes [distributes its own kubectl binaries](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl) for Windows now. You should use them instead.

# Running kubectl on Windows

### Why??

It's nice to be able to run kubectl remotely against a kubernetes cluster. Then, you don't have to ssh into the servers to run commands. (Also, you won't need to run kubectl from a Linux VM, which is what other people suggest.) You'll probably need commands like mkdir (provided through cygwin or a standard git installation with cygwin binaries included). But you might also be able to use the pre-built binary without these tools. (I'm not sure, really)

## Download a pre-built Windows binary

See [releases](https://github.com/eirslett/kubectl-windows/releases), download kubectl.exe, and put kubectl.exe somewhere in your PATH.

#### Configuring kubectl to use a remote kubernetes cluster

1. cd C:\users\yourusername (Or wherever your %HOME% directory is)
2. mkdir .kube
3. cd .kube
4. touch config
5. Edit the config file with your editor of choice - notepad for example.

It could for example look like this:

```yaml
apiVersion: v1
clusters:
- cluster:
    server: https://123.456.789.123:9999
    certificate-authority-data: yoursertificate
  name: your-k8s-cluster-name
contexts:
- context:
    cluster: your-k8s-cluster-name
    namespace: default
    user: admin
  name: default-context
current-context: default-context
kind: Config
preferences: {}
users:
- name: admin
  user:
    token: your-login-token
```

## Do It Yourself (aka. building from source)

You can also try to build it yourself:

1. [Set up a Go environment](https://golang.org/doc/install).
1. Set your GOPATH environment variable.  For these instructions we will use C:\gopath but %USERPROFILE%\go is more common.
1. Add %GOPATH\bin to your PATH environment variable (exes like godep will be placed here and kubectl by the end of the install process)
1. Make a directory to clone kubernetes: `mkdir C:\gopath\src\k8s.io` then `cd C:\gopath\src\k8s.io`
1. [View release tags](https://github.com/kubernetes/kubernetes/releases) and choose the release you wish to build (like v1.5.0)
1. Checkout kubernetes using the release tag you choose -- we're using v1.5.0 in this example: `git clone --depth 1 https://github.com/kubernetes/kubernetes v1.5.0` 
1. Install [Godep](https://github.com/tools/godep) (for dependency management) and [mercurial](https://www.mercurial-scm.org/downloads) (for fetching some dependencies)
1. Fetch GO dependencies: `cd C:\gopath\src\k82.io\kubernetes\` then `godep restore` (If some dependencies fail, it might work anyways.  Note this may take awhile - up to 10 min)
1. Build kubectl `cd C:\gopath\src\k8s.io\kubernetes\cmd\kubectl` then `go install .` (Note this may take awhile - up to 10 min)
1. The kubectl binary should now be at C:\gopath\bin\kubectl.exe
