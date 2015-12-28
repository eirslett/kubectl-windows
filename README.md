# Running kubectl on Windows

### Why??

It's nice to be able to run kubectl remotely against a kubernetes cluster. Then, you don't have to ssh into the servers to run commands. (Also, you won't need to run kubectl from a Linux VM, which is what other people suggest.) You'll probably need commands like mkdir (provided through cygwin or a standard git installation with cygwin binaries included). But you might also be able to use the pre-built binary without these tools. (I'm not sure, really)

## Download a pre-built Windows binary

I've built a binary for you. It's commited into this repository. It's called kubectl-1.1.3.exe. You can [download it](https://github.com/eirslett/kubectl-windows/raw/master/kubectl-1.1.3.exe) and put it somewhere in your PATH. (I suggest that you rename it to kubectl.exe)

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

1. [Set up a Go environment](https://golang.org/doc/install). Let's assume your GOPATH is at C:\gopath.
2. mkdir C:\gopath\src\k8s.io
3. cd C:\gopath\src\k8s.io
4. git clone https://github.com/kubernetes/kubernetes
5. Install prerequisites: Amongst them: Godep (for dependency management), mercurial (for fetching some dependencies) ...
6. cd C:\gopath\src\k82.io\kubernetes\Godeps
7. godep restore (to fetch dependencies. If some dependencies fail, it might work anyways)
8. cd C:\gopath\src\k8s.io\kubernetes\cmd\kubectl
9. go install .
10. The kubectl binary should now be at C:\gopath\bin\kubectl.exe
