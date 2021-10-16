arguments in launch.json not work in code-server
===

## My Code

```
mkdir -p code-server-debug-args
mkdir -p code-server-debug-args/.vscode

cat <<'EOF'>.vscode/launch.json
{
    "configurations": [
        {
            "name": "Launch Package",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "${fileDirname}",
            "args": [
                "myargs"
            ]
        }
    ]
}
EOF

cat <<'EOF'>main.go
package main

import (
	"fmt"
	"os"
)

func main() {
	fmt.Print(os.Args)
}
EOF

cat <<'EOF'>go.mod # optional
module app

go 1.15
EOF
```

## Testing

### Case 01: go run

``` bash
➜  code-server-debug-args git:(master) ✗ go run main.go myargs
[/tmp/go-build412015233/b001/exe/main myargs]
```

### Case 02: debug in vscode(remote ssh)

``` bash
Starting: /root/go/bin/dlv-dap dap --check-go-version=false --listen=127.0.0.1:41885 --log-dest=3 from /opt/code-server-debug-args
DAP server listening at: 127.0.0.1:41885
[/tmp/__debug_bin458249886 myargs]
```


### Case 03: debug in code-server

``` bash
Starting: /root/go/bin/dlv-dap dap --check-go-version=false --listen=127.0.0.1:33611 --log-dest=3 from /opt/code-server-debug-args
DAP server listening at: 127.0.0.1:33611
[/tmp/__debug_bin3637673742]
```