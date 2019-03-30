# IPFS Data Visualizations

[![](https://img.shields.io/badge/made%20by-Protocol%20Labs-blue.svg?style=flat-square)](http://ipn.io)
[![](https://img.shields.io/badge/project-IPFS-blue.svg?style=flat-square)](http://ipfs.io/)
[![](https://img.shields.io/badge/freenode-%23ipfs-blue.svg?style=flat-square)](http://webchat.freenode.net/?channels=%23ipfs)
[![Stories in Ready](https://badge.waffle.io/ipfs/dataviz.svg?label=ready&title=Ready)](http://waffle.io/ipfs/dataviz)

> ipfs data visualizations

## Table of Contents

- [Install](#install)
- [Usage](#usage)
  - [graphmd](#graphmd)
  - [D3 Tree](#d3-tree)
- [Contribute](#contribute)
  - [Want to hack on IPFS?](#want-to-hack-on-ipfs)
- [License](#license)

## Install

To install and run the D3 Tree locally, first [install IPFS](https://ipfs.io/docs/install/). Then:


```sh
ipfs daemon
# Open a new terminal window
git clone git@github.com:ipfs/dataviz.git
cd dataviz/webapps/tree-ltr
make
```

This should load up the tree viz in your browser.
Change the IPFS hash at the end of the URL to see any other IPFS tree.

## Usage

### graphmd

** Status: stable **

Graphs of Merkel DAGs from the command line.

![](https://ipfs.io/ipfs/QmbefthRKDReojALJi8nGPwvUVPqe1aXdoD9ysX44aUfvG/graph.png)

More information here: https://ipfs.io/ipfs/QmTkzDwWqPbnAh5YiV5VwcTLnGdwSNsNTn2aDxdXBFca7D/example#/ipfs/QmQwAP9vFjbCtKvD8RkJdCvPHqLQjZfW7Mqbbqx18zd8j7/graphmd/README.md


### D3 Tree

** Status: experimental **

We have adapted a [D3 left-to-right tree example by Mike Bostock](http://mbostock.github.io/d3/talk/20111018/tree.html) for use in visualizing IPFS.

![](https://cdn.rawgit.com/ipfs/dataviz/6021cea7e49224b1bab784ce04e6ef7019be625b/webapps/tree-ltr/doc/ipfs-core.png)

This can be served up via IPFS, and read data from IPFS.  For example,
the tree visualizing its own source code:

https://v04x.ipfs.io/ipfs/QmX5smVTZfF8p1VC8Y3VtjGqjvDVPWvyBk24JgvnMwHtjC/viz#QmX5smVTZfF8p1VC8Y3VtjGqjvDVPWvyBk24JgvnMwHtjC

The first hash is the visualization; the second hash is whatever is being visualized.

For example, to explore Brewster Kahle's blog (quite large, give it a minute to load), we would visit:

https://v04x.ipfs.io/ipfs/QmX5smVTZfF8p1VC8Y3VtjGqjvDVPWvyBk24JgvnMwHtjC/viz#QmavE42xtK1VovJFVTVkCR5Jdf761QWtxmvak9Zx718TVr

(To see the raw blog content, visit <https://ipfs.io/ipfs/QmavE42xtK1VovJFVTVkCR5Jdf761QWtxmvak9Zx718TVr>)

## Contribute

Feel free to join in. All welcome.

This repository falls under the IPFS [Code of Conduct](https://github.com/ipfs/community/blob/master/code-of-conduct.md).

### Want to hack on IPFS?

[![](https://cdn.rawgit.com/jbenet/contribute-ipfs-gif/master/img/contribute.gif)](https://github.com/ipfs/community/blob/master/contributing.md)

## License

MIT

