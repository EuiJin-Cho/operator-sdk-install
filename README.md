# operator-sdk-install
operator-sdk installation

install go-lang
```
wget https://go.dev/dl/go1.18.4.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf ./go1.18.4.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin

```

install make, gcc
```
sudo apt install gcc -y
sudo apt install make -y

```

install operator-sdk
```
git clone https://github.com/operator-framework/operator-sdk
cd operator-sdk
make build
make install

```

check install complete
```
operator-sdk version

```

(option)if operator-sdk command can not use
```
cp build/operator-sdk /usr/local/bin/

```

## example(Memcached)
make project directory
```
mkdir -p $HOME/projects/memcached-operator
cd $HOME/projects/memcached-operator

```

Create a new project 
```
operator-sdk init --domain example.com --repo github.com/example/memcached-operator

```

Create a new API, Controller
```
operator-sdk create api --group cache --version v1alpha1 --kind Memcached --resource --controller

```

Modify under file
/root/projects/memcached-operator/api/v1alpha1/memcached_types.go
/root/projects/memcached-operator/controllers/memcached_controller.go
/root/projects/memcached-operator/main.go
```
```


Operator build on /root/projects/memcached-operator and check
```
make install
kubectl get crd
```

run operator
```
make run ENABLE_WEBHOOKS=false
```

deploy CR
modify yaml file - create spec:size field (ex. size: 3)
```
kubectl apply -f /root/projects/memcached-operator/config/samples/cache_v1alpha1_memcached.yaml
```



### modified main.go
ref: https://github.com/kubernetes-sigs/kubebuilder/issues/766

original main.go
```
 89         if err = (&controllers.MemcachedReconciler{
 90                 Client: mgr.GetClient(),
 91                 Scheme: mgr.GetScheme(),
 92         }).SetupWithManager(mgr); err != nil {
 93                 setupLog.Error(err, "unable to create controller", "controller", "Memcached")
 94                 os.Exit(1)
 95         }
```

modified main.go
```
 89         if err = (&controllers.MemcachedReconciler{
 90                 Client: mgr.GetClient(),
 91                 Log:    ctrl.Log.WithName("controllers").WithName("memcached"),
 92                 Scheme: mgr.GetScheme(),
 93         }).SetupWithManager(mgr); err != nil {
 94                 setupLog.Error(err, "unable to create controller", "controller", "Memcached")
 95                 os.Exit(1)
 96         }
```
