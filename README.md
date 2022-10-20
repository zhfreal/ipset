# ipset

[![Go Report Card](https://goreportcard.com/badge/github.com/zhfreal/ipset?style=flat-square)](https://goreportcard.com/report/github.com/zhfreal/ipset)
[![GitHub tag](https://img.shields.io/github/v/tag/zhfreal/ipset.svg?sort=semver&style=flat-square)](https://github.com/zhfreal/ipset/releases)
[![PkgGoDev](https://pkg.go.dev/badge/github.com/zhfreal/ipset)](https://pkg.go.dev/github.com/zhfreal/ipset)
[![Go Version](https://img.shields.io/github/go-mod/go-version/zhfreal/ipset?style=flat-square)](https://go.dev/dl/)

netlink ipset package for Go.

## Usage

### Your code:
```Go
package main

import (
	"log"

	"github.com/zhfreal/ipset"
)

func main() {
	// must call Init first
	if err := ipset.Init(); err != nil {
		log.Printf("error in ipset Init: %s", err)
		return
	}

	// default is ipv4 without timeout
	ipset.Destroy("myset")
	ipset.Create("myset")
	ipset.Add("myset", "1.1.1.1")
	ipset.Add("myset", "2.2.2.0/24")

	// ipv6 and timeout example
	// ipset create myset6 hash:net family inet6 timeout 60
	ipset.Create("myset6", ipset.OptIPv6(), ipset.OptTimeout(60))
	ipset.Flush("myset6")

	ipset.Add("myset6", "2022::1", ipset.OptTimeout(10))
	ipset.Add("myset6", "2022::1/32")
}
```

### Result:
`ipset list myset`

```
Name: myset
Type: hash:net
Revision: 1
Header: family inet hashsize 1024 maxelem 65536
Size in memory: 552
References: 0
Number of entries: 2
Members:
1.1.1.1
2.2.2.0/24
```

`ipset list myset6`

```
Name: myset6
Type: hash:net
Revision: 1
Header: family inet6 hashsize 1024 maxelem 65536 timeout 60
Size in memory: 1432
References: 0
Number of entries: 2
Members:
2022::1 timeout 9
2022::/32 timeout 59
```

## Links

- [glider](https://github.com/zhfreal/glider): a forward proxy with ipset management features powered by this package.
