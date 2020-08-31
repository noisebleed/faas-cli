我是光年实验室高级招聘经理。
我在github上访问了你的开源项目，你的代码超赞。你最近有没有在看工作机会，我们在招软件开发工程师，拉钩和BOSS等招聘网站也发布了相关岗位，有公司和职位的详细信息。
我们公司在杭州，业务主要做流量增长，是很多大型互联网公司的流量顾问。公司弹性工作制，福利齐全，发展潜力大，良好的办公环境和学习氛围。
公司官网是http://www.gnlab.com,公司地址是杭州市西湖区古墩路紫金广场B座，若你感兴趣，欢迎与我联系，
电话是0571-88839161，手机号：18668131388，微信号：echo 'bGhsaGxoMTEyNAo='|base64 -D ,静待佳音。如有打扰，还请见谅，祝生活愉快工作顺利。

## faas-cli

[![Build Status](https://travis-ci.org/alexellis/faas-cli.svg?branch=master)](https://travis-ci.org/alexellis/faas-cli)

This experimental CLI can be used to and deploy functions to FaaS or to build Node.js or Python functions from a templates meaning you just write a handler file (handler.py/handler.js). Read on for examples and usage.

> Functions as a Service is a serverless framework for Docker: [Read more on docs.get-faas.com](http://docs.get-faas.com/)

### Installing the tool

The easiest way to install the faas-cli is by doing:

```
$ curl -sSL https://cli.openfaas.com | sudo sh
```

Note that the tool is also available on brew. The last section also documents how to build it from source.

### Running the tool

The tool can be used to create a Docker image to be deployed on FaaS through a template meaning you only have to write a single handler file. The templates currently supported are: node and python, however you can create a FaaS function out of any process.

#### YAML files for ease of use

You can define individual functions or a set of of them within a YAML file. This makes the CLI easier to use and means you can use this file to deploy to your FaaS instance.

Here is an example file using the `samples.yml` file included in the repository.

```
provider:
  name: faas
  gateway: http://localhost:8080

functions:
  url-ping:
    lang: python
    handler: ./sample/url_ping
    image: alexellis2/faas-urlping
```

This url_ping function is defined in the samples/url__ping folder makes use of Python. All we had to do was to write a `handler.py` file and then to list off any Python modules in `requirements.txt`.

* Build the files in the .yml file:

```
$ ./faas-cli -action build -f ./samples.yml
```

> `-f` specifies the file or URL to download your YAML file from. The long version of the `-f` flag is: `-yaml`.

You can also download over HTTP/s:

```
$ ./faas-cli -action build -f https://raw.githubusercontent.com/alexellis/faas-cli/master/samples.yml
```

Docker along with a Python template will be used to build an image named alexellis2/faas-urlping.

* Deploy your function

Now you can use the following command to deploy your function(s):

```
$ ./faas-cli -action deploy -f ./samples.yml
```

* Possible entries for functions are documented below:

```
functions:
  deployed_function_name:
    lang: node or python (optional)
    handler: ./path/to/handler (optional)
    image: docker-image-name
    environment:
      env1: value1
      env2: "value2"
```

Use environmental variables for setting tokens and configuration.

**Accessing the function with `curl`**

You can initiate a HTTP POST via `curl`:

* with the `-d` flag i.e. `-d "my data here"`
* or with `--data-binary @filename.txt` to send a whole file including newlines
* if you want to pass input from STDIN then use `--data-binary @-`

```
$ curl -d '{"hello": "world"}' http://localhost:8080/function/nodejs-echo
{ nodeVersion: 'v6.9.1', input: '{"hello": "world"}' }

$ curl --data-binary @README.md http://localhost:8080/function/nodejs-echo

$ uname -a | curl http://localhost:8080/function/nodejs-echo--data-binary @-
```

> For further instructions on the manual CLI flags (without using a YAML file) read [manual_cli.md](https://github.com/alexellis/faas-cli/blob/master/MANUAL_CLI.md)

## FaaS-CLI Developers / Contributors

**Updating the brew formula**

The `brew` formula for the faas-cli is part of the official [homebrew-core](https://github.com/Homebrew/homebrew-core/blob/master/Formula/faas-cli.rb) repo on Github. It needs to be updated for each subsequent release.

These are the guidelines for updating brew:

https://github.com/Homebrew/homebrew-core/blob/master/CONTRIBUTING.md

After `brew edit` run the build and test the results:

```
$ brew uninstall --force faas-cli ; \
  brew install --build-from-source faas-cli ; \
  brew test faas-cli ; \
  brew audit --strict faas-cli
```

**Updating the utility-script**

Please raise a PR for the get.sh file held in this repository. It's used when people install via `curl` and `cli.openfaas.com`. The updated file then has to be redeployed to the hosting server.

### Installation / pre-requirements

* Docker

Install Docker because it is used to build Docker images if you create new functions.

* FaaS - deployed and live

This CLI can build and deploy templated functions, so it's best if you have FaaS started up on your laptop. Head over to http://docs.get-faas.com/ and get up and running with a sample stack in 60 seconds.

* Golang

> Here's how to install Go in 60 seconds.

* Grab Go 1.7.x from https://golang.org/dl/

Then after installing run this command or place it in your `$HOME/.bash_profile`

```
export GOPATH=$HOME/go
```

* Now clone / build `faas-cli`:

```
$ mkdir -p $GOPATH/src/github.com/alexellis/
$ cd $GOPATH/src/github.com/alexellis/
$ git clone https://github.com/alexellis/faas-cli
$ cd faas-cli
$ go get -d -v
$ go build
```

### License and contributing

This project is part of the FaaS project licensed under the MIT License.

For more details see the [Contributing guide](https://github.com/alexellis/faas-cli/blob/master/CONTRIBUTING.md).
