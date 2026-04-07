# Install golang

## Download golang
```bash
$ wget https://go.dev/dl/go1.20.3.linux-amd64.tar.gz
```

## Install golang
```bash
$ rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.3.linux-amd64.tar.gz
$ echo "export PATH=$PATH:/usr/local/go/bin" | tee -a $HOME/.profile
$ export PATH=$PATH:/usr/local/go/bin
```

## Check installation
```bash
$ go version
```

# Compile Unbound-Exporter

Unbound-Exporter from https://github.com/ar51an/unbound-exporter

Check for new Content - Last Update 2023-01-27

```bash
# Download dependencies
$ go mod tidy
# Build application
$ go build
# Reduce size
$ strip unbound-exporter
```

