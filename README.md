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
operator-sdk init --domain example.com --repo github.com/example/memcached-operato

```

Create a new API, Controller
```
operator-sdk create api --group cache --version v1alpha1 --kind Memcached --resource --controller

```
