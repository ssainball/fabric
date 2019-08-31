# Commercial paper tutorial

**Audience:** Architects, application and smart contract developers,
administrators 

대상 : 설계자, 애플리케이션 및 스마트 계약 개발자, 관리자

This tutorial will show you how to install and use a commercial paper sample
application and smart contract. It is a task-oriented topic, so it emphasizes
procedures above concepts. When you’d like to understand the concepts in more
detail, you can read the
[Developing Applications](../developapps/developing_applications.html) topic.

이 학습서에서는 commercial paper sample 애플리케이션 및 스마트 계약을 설치하고 사용하는 방법을 보여줍니다. 작업 중심의 주제이므로 개념 이상의 절차를 강조합니다. 개념을 보다 자세히 이해하려면 Developing Applications 주제를 읽으십시오.

![commercialpaper.tutorial](./commercial_paper.diagram.1.png) *In this tutorial
two organizations, MagnetoCorp and DigiBank, trade commercial paper with each
other using PaperNet, a Hyperledger Fabric blockchain network.*

이 자습서에서는 MagnetoCorp와 DigiBank라는 두 조직이 Hyperledger Fabric 블록체인 네트워크 인 PaperNet을 사용하여 commercial paper 를 서로 교환합니다.

Once you've set up a basic network, you'll act as Isabella, an employee of
MagnetoCorp, who will issue a commercial paper on its behalf. You'll then switch
hats to take the role of Balaji, an employee of DigiBank, who will buy this
commercial paper, hold it for a period of time, and then redeem it with
MagnetoCorp for a small profit.

기본 네트워크를 설정하면 MagnetoCorp의 직원 인 Isabella의 역할을 수행하여 이를 대신 하여 commercial paper 를 발행합니다. 그런 다음 모자를 전환하여 DigiBank의 직원 인 Balaji 가 이 commercial paper 를 구입하고 일정 기간 동안 보유한 다음 MagnetoCorp 로 소량의 이익을 위해 교환하십시오.

You'll act as an developer, end user, and administrator, each in different
organizations, performing the following steps designed to help you understand
what it's like to collaborate as two different organizations working
independently, but according to mutually agreed rules in a Hyperledger Fabric
network.

서로 다른 조직에서 각각 개발자, 최종 사용자 및 관리자 역할을 수행하여 독립적으로 작업 하는 두 조직이 서로 협력하고 Hyperledger Fabric의 상호 합의 된 규칙에 따라 협업 하는 방법을 이해하도록 돕기 위해 다음 단계를 수행합니다.

* [Set up machine](#prerequisites) and [download samples](#download-samples)
* [Create a network](#create-network)
*  Understand the structure of a [smart contract](#smart-contract)
* Work as an organization, [MagnetoCorp](#working-as-magnetocorp), to
  [install](#install-contract) and [instantiate](#instantiate-contract) smart
  contract
* Understand the structure of a MagnetoCorp
  [application](#application-structure), including its
  [dependencies](#application-dependencies)
* Configure and use a [wallet and identities](#wallet)
* Run a MagnetoCorp application to [issue a commercial
  paper](#issue-application)
* Understand how a second organization, [Digibank](#working-as-digibank), uses
  the smart contract in their [applications](#digibank-applications)
* As Digibank, [run](#run-as-digibank) applications that
  [buy](#buy-application) and [redeem](#redeem-application) commercial paper



* Machine 설정 및 샘플 다운로드
* 네트워크 만들기
* 스마트 컨트랙트의 구조 이해
* 스마트 계약을 설치하고 인스턴스화하기 위해 MagnetoCorp 조직으로 작업
* 의존성을 포함하여 MagnetoCorp 애플리케이션의 구조 이해
* 지갑과 아이덴티티 구성 및 사용
* MagnetoCorp 응용 프로그램을 실행하여 상업용 용지를 발행하십시오
* 두 번째 조직인 Digibank가 애플리케이션에서 스마트 계약을 사용하는 방법 이해
* Digibank 로서 commercial paper 를 구매하고 사용하는 응용 프로그램을 실행하십시오.

This tutorial has been tested on MacOS and Ubuntu, and should work on other
Linux distributions. A Windows version is under development.

이 튜토리얼은 MacOS 및 Ubuntu에서 테스트되었으며 다른 Linux 배포판에서 작동합니다. Windows 버전은 개발 중입니다.


## Prerequisites

Before you start, you must install some prerequisite technology required by the
tutorial. We've kept these to a minimum so that you can get going quickly.

시작하기 전에 학습서에 필요한 일부 전제 조건 기술을 설치해야합니다. 설치 소요 시간을 최소화 하였습니다.

You **must** have the following technologies installed:

다음 기술들이 설치되어 있어야합니다:

  * [**Node**](https://nodejs.org/en/about/) version 8.9.0, or higher. Node is
    a JavaScript runtime that you can use to run applications and smart
    contracts. You are recommended to use the LTS (Long Term Support) version
    of node. Install node [here](https://nodejs.org/en/).
    
  * Node 버전 8.9.0 이상 Node는 애플리케이션 및 스마트 계약을 실행하는 데 사용할 수 있는 JavaScript 런타임입니다. LTS(Long Term Support) 버전의 노드를 사용하는 것이 좋습니다. 노드를 설치하십시오.


  * [**Docker**](https://www.docker.com/get-started) version 18.06, or higher.
    Docker help developers and administrators create standard environments for
    building and running applications and smart contracts. Hyperledger Fabric is
    provided as a set of Docker images, and the PaperNet smart contract will run
    in a docker container. Install Docker
    [here](https://www.docker.com/get-started).

  * Docker 버전 18.06 이상 Docker는 개발자와 관리자가 응용 프로그램 및 스마트 계약을 작성하고 실행하기위한 표준 환경을 만들 수 있도록 도와줍니다. Hyperledger Fabric은 Docker 이미지 세트로 제공되며 PaperNet 스마트 계약은 docker 컨테이너에서 실행 됩니다. Docker를 설치하십시오.

You **will** find it helpful to install the following technologies:

다음 기술을 설치하면 도움이됩니다.

  * A source code editor, such as
    [**Visual Studio Code**](https://code.visualstudio.com/) version 1.28, or
    higher. VS Code will help you develop and test your application and smart
    contract. Install VS Code [here](https://code.visualstudio.com/Download).

    Many excellent code editors are available including
    [Atom](https://atom.io/), [Sublime Text](http://www.sublimetext.com/) and
    [Brackets](http://www.sublimetext.com/).


  * Visual Studio Code 버전 1.28 이상과 같은 소스 코드 편집기 VS Code는 응용 프로 그램과 현명한 계약을 개발하고 테스트하는 데 도움이됩니다. VS 코드를 설치하십시오.

    Atom, Sublime Text 및 Brackets를 포함한 많은 코드 편집기를 사용할 수 있습니다.


You **may** find it helpful to install the following technologies as you become
more experienced with application and smart contract development. There's no
requirement to install these when you first run the tutorial:

응용 프로그램 및 스마트 계약 개발에 익숙해지면 다음 기술을 설치하는 것이 도움이 될 수 있습니다. 튜토리얼을 처음 실행할 때 설치하지 않아도됩니다.


  * [**Node Version Manager**](https://github.com/creationix/nvm). NVM helps you
    easily switch between different versions of node -- it can be really helpful
    if you're working on multiple projects at the same time. Install NVM
    [here](https://github.com/creationix/nvm#installation).

  * 노드 버전 관리자. NVM을 사용하면 서로 다른 버전의 노드간에 쉽게 전환 할 수 있습니다. 동시에 여러 프로젝트를 작업하는 경우 매우 유용합니다. NVM을 설치하십시오.


## Download samples

The commercial paper tutorial is one of the Hyperledger Fabric
[samples](https://github.com/hyperledger/fabric-samples) held in a public
[GitHub](https://www.github.com) repository called `fabric-samples`. As you're
going to run the tutorial on your machine, your first task is to download the
`fabric-samples` repository.

'Commercial paper tutorial'은 패브릭 샘플이라는 공용 GitHub 저장소에 보관 된 Hyperledger Fabric 샘플 중 하나입니다. 머신에서 튜토리얼을 실행할 때 가장 먼저 할 일은 패브릭 샘플 저장소를 다운로드하는 것입니다.

![commercialpaper.download](./commercial_paper.diagram.2.png) *Download the
`fabric-samples` GitHub repository to your local machine.*

패브릭 샘플 GitHub 저장소를 로컬 머신으로 다운로드하십시오.

`$GOPATH` is an important environment variable in Hyperledger Fabric; it
identifies the root directory for installation. It is important to get right no
matter which programming language you're using! Open a new terminal window and
check your `$GOPATH` is set using the `env` command:

$ GOPATH는 Hyperledger Fabric에서 중요한 환경 변수입니다. 설치를위한 루트 디렉토리를 식별합니다. 사용중인 프로그래밍 언어에 상관없이 올바르게 이해하는 것이 중요합니다! 새 터미널 창을 열고 env 명령을 사용하여 $ GOPATH가 설정되어 있는지 확인하십시오.


```
$ env
...
GOPATH=/Users/username/go
NVM_BIN=/Users/username/.nvm/versions/node/v8.11.2/bin
NVM_IOJS_ORG_MIRROR=https://iojs.org/dist
...
```

Use the following
[instructions](https://github.com/golang/go/wiki/SettingGOPATH) if your
`$GOPATH` is not set.

$ GOPATH가 설정되지 않은 경우 다음 지시 사항을 사용하십시오.

You can now create a directory relative to `$GOPATH `where `fabric-samples` will
be installed:

이제 패브릭 샘플이 설치 될 $ GOPATH와 관련된 디렉토리를 만들 수 있습니다.

```
$ mkdir -p $GOPATH/src/github.com/hyperledger/
$ cd $GOPATH/src/github.com/hyperledger/
```

Use the [`git clone`](https://git-scm.com/docs/git-clone) command to copy
[`fabric-samples`](https://github.com/hyperledger/fabric-samples) repository to
this location:

git clone 명령을 사용하여 패브릭 샘플 저장소를 이 위치로 복사하십시오.


```
$ git clone https://github.com/hyperledger/fabric-samples.git
```

Feel free to examine the directory structure of `fabric-samples`:

패브릭 샘플의 디렉토리 구조를 자유롭게 확인하십시오.

```
$ cd fabric-samples
$ ls

CODE_OF_CONDUCT.md    balance-transfer            fabric-ca
CONTRIBUTING.md       basic-network               first-network
Jenkinsfile           chaincode                   high-throughput
LICENSE               chaincode-docker-devmode    scripts
MAINTAINERS.md        commercial-paper            README.md
fabcar
```

Notice the `commercial-paper` directory -- that's where our sample is located!

샘플이 있는 commercial-paper 디렉토리를 주목하십시오!

You've now completed the first stage of the tutorial! As you proceed, you'll
open multiple command windows open for different users and components. For
example:

이제 튜토리얼의 첫 번째 단계를 완료했습니다! 계속 진행하면 다른 사용자 및 구성 요소에 대해 여러 개의 명령 창이 열립니다. 예를 들면 다음과 같습니다.

* to run applications on behalf of Isabella and Balaji who will trade commercial
  paper with each other
* to issue commands to on behalf of administrators from MagnetoCorp and
  DigiBank, including installing and instantiating smart contracts
* to show peer, orderer and CA log output

* Isabella와 Balaji를 대신하여 상업용 용지를 거래하는 응용 프로그램을 실행
* 스마트 계약 설치 및 인스턴스화를 포함하여 MagnetoCorp 및 DigiBank의 관리자를 대신하여 명령을 발행
* 피어, 주문자 및 CA 로그 출력을 표시

We'll make it clear when you should run a command from particular command
window; for example:

특정 명령 창에서 명령을 실행해야 할 때 명확하게 설명합니다. 예를 들면 다음과 같습니다.

```
(isabella)$ ls
```

indicates that you should run the `ls` command from Isabella's window.

Isabella의 창에서 ls 명령을 실행해야 함을 나타냅니다.


## Create network

The tutorial currently uses the basic network; it will be updated soon to a
configuration which better reflects the multi-organization structure of
PaperNet. For now, this network is sufficient to show you how to develop an
application and smart contract.

이 튜토리얼은 현재 기본 네트워크를 사용합니다. PaperNet의 다중 구성 구조를보다 잘 반영하는 구성으로 곧 업데이트 될 예정입니다. 현재이 네트워크는 애플리케이션 및 스마트 계약 개발 방법을 보여주기에 충분합니다.

![commercialpaper.network](./commercial_paper.diagram.3.png) *The Hyperledger
Fabric basic network comprises a peer and its ledger database, an orderer and a
certificate authority (CA). Each of these components runs as a docker
container.*

Hyperledger Fabric 기본 네트워크는 피어 및 원장 데이터베이스, 주문자 및 인증 기관 (CA)으로 구성됩니다. 이러한 각 구성 요소는 도커 컨테이너로 실행됩니다.

The peer, its [ledger](../ledger/ledger.html#world-state-database-options), the
orderer and the CA each run in the their own docker container. In production
environments, organizations typically use existing CAs that are shared with
other systems; they're not dedicated to the Fabric network.

피어, 원장, 주문자 및 CA는 각각 자체 도커 컨테이너에서 실행됩니다. 프로덕션 환경에서 조직은 일반적으로 다른 시스템과 공유되는 기존 CA를 사용합니다. 그들은 Fabric 네트워크 전용이 아닙니다.

You can manage the basic network using the commands and configuration included
in the `fabric-samples\basic-network` directory. Let's start the network on your
local machine with the `start.sh` shell script:

fabric-samples \ basic-network 디렉토리에 포함 된 명령 및 구성을 사용하여 기본 네트워크를 관리 할 수 있습니다. start.sh 셸 스크립트를 사용하여 로컬 컴퓨터에서 네트워크를 시작해 보겠습니다.

```
$ cd fabric-samples/basic-network
$ ./start.sh

docker-compose -f docker-compose.yml up -d ca.example.com orderer.example.com peer0.org1.example.com couchdb
Creating network "net_basic" with the default driver
Pulling ca.example.com (hyperledger/fabric-ca:)...
latest: Pulling from hyperledger/fabric-ca
3b37166ec614: Pull complete
504facff238f: Pull complete
(...)
Pulling orderer.example.com (hyperledger/fabric-orderer:)...
latest: Pulling from hyperledger/fabric-orderer
3b37166ec614: Already exists
504facff238f: Already exists
(...)
Pulling couchdb (hyperledger/fabric-couchdb:)...
latest: Pulling from hyperledger/fabric-couchdb
3b37166ec614: Already exists
504facff238f: Already exists
(...)
Pulling peer0.org1.example.com (hyperledger/fabric-peer:)...
latest: Pulling from hyperledger/fabric-peer
3b37166ec614: Already exists
504facff238f: Already exists
(...)
Creating orderer.example.com ... done
Creating couchdb             ... done
Creating ca.example.com         ... done
Creating peer0.org1.example.com ... done
(...)
2018-11-07 13:47:31.634 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-11-07 13:47:31.730 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
```

Notice how the `docker-compose -f docker-compose.yml up -d ca.example.com...`
command pulls the four Hyperledger Fabric container images from
[DockerHub](https://hub.docker.com/), and then starts them. These containers
have the most up-to-date version of the software for these Hyperledger Fabric
components. Feel free to explore the `basic-network` directory -- we'll use
much of its contents during this tutorial.

'docker-compose -f docker-compose.yml up -d ca.example.com ...'명령이 어떻게 DockerHub에서 4 개의 Hyperledger Fabric 컨테이너 이미지를 가져 와서 시작하는지 확인하십시오. 이 컨테이너에는 이러한 Hyperledger Fabric 구성 요소를위한 최신 버전의 소프트웨어가 있습니다. 기본 네트워크 디렉토리를 자유롭게 탐색하십시오.이 자습서에서는 많은 내용을 사용합니다.

You can list the docker containers that are running the basic-network components
using the `docker ps` command:

docker ps 명령을 사용하여 기본 네트워크 구성 요소를 실행중인 도커 컨테이너를 나열 할 수 있습니다.


```
$ docker ps

CONTAINER ID        IMAGE                        COMMAND                  CREATED              STATUS              PORTS                                            NAMES
ada3d078989b        hyperledger/fabric-peer      "peer node start"        About a minute ago   Up About a minute   0.0.0.0:7051->7051/tcp, 0.0.0.0:7053->7053/tcp   peer0.org1.example.com
1fa1fd107bfb        hyperledger/fabric-orderer   "orderer"                About a minute ago   Up About a minute   0.0.0.0:7050->7050/tcp                           orderer.example.com
53fe614274f7        hyperledger/fabric-couchdb   "tini -- /docker-ent…"   About a minute ago   Up About a minute   4369/tcp, 9100/tcp, 0.0.0.0:5984->5984/tcp       couchdb
469201085a20        hyperledger/fabric-ca        "sh -c 'fabric-ca-se…"   About a minute ago   Up About a minute   0.0.0.0:7054->7054/tcp                           ca.example.com
```

See if you can map these containers to the basic-network (you may need to
horizontally scroll to locate the information):

이러한 컨테이너를 기본 네트워크에 매핑 할 수 있는지 확인하십시오 (정보를 찾으려면 가로로 스크롤해야 할 수도 있음).

* A peer `peer0.org1.example.com` is running in container `ada3d078989b`
* An orderer `orderer.example.com` is running in container `1fa1fd107bfb`
* A CouchDB database `couchdb` is running in container `53fe614274f7`
* A CA `ca.example.com` is running in container `469201085a20`

* 피어 peer0.org1.example.com이 컨테이너 ada3d078989b에서 실행 중입니다.
* 주문자 orderer.example.com이 컨테이너 1fa1fd107bfb에서 실행 중입니다.
* CouchDB 데이터베이스 couchdb가 컨테이너 53fe614274f7에서 실행 중
* CA ca.example.com이 컨테이너 469201085a20에서 실행 중입니다.

These containers all form a [docker network](https://docs.docker.com/network/)
called `net_basic`. You can view the network with the `docker network` command:

이 컨테이너는 모두 net_basic이라는 도커 네트워크를 형성합니다. docker network 명령을 사용하여 네트워크를 볼 수 있습니다:


```
$ docker network inspect net_basic

    {
        "Name": "net_basic",
        "Id": "62e9d37d00a0eda6c6301a76022c695f8e01258edaba6f65e876166164466ee5",
        "Created": "2018-11-07T13:46:30.4992927Z",
        "Containers": {
            "1fa1fd107bfbe61522e4a26a57c2178d82b2918d5d423e7ee626c79b8a233624": {
                "Name": "orderer.example.com",
                "IPv4Address": "172.20.0.4/16",
            },
            "469201085a20b6a8f476d1ac993abce3103e59e3a23b9125032b77b02b715f2c": {
                "Name": "ca.example.com",
                "IPv4Address": "172.20.0.2/16",
            },
            "53fe614274f7a40392210f980b53b421e242484dd3deac52bbfe49cb636ce720": {
                "Name": "couchdb",
                "IPv4Address": "172.20.0.3/16",
            },
            "ada3d078989b568c6e060fa7bf62301b4bf55bed8ac1c938d514c81c42d8727a": {
                "Name": "peer0.org1.example.com",
                "IPv4Address": "172.20.0.5/16",
            }
        },
        "Labels": {}
    }
```

See how the four containers use different IP addresses, while being part of a
single docker network. (We've abbreviated the output for clarity.)

단일 도커 네트워크의 일부인 4 개의 컨테이너가 다른 IP 주소를 사용하는 방법을 확인하십시오. (명확성을 위해 출력을 축약했습니다.)

To recap: you've downloaded the Hyperledger Fabric samples repository from
GitHub and you've got the basic network running on your local machine. Let's now
start to play the role of MagnetoCorp, who wish to trade commercial paper.

요약하자면, GitHub에서 Hyperledger Fabric 샘플 저장소를 다운로드했으며 로컬 컴퓨터에서 기본 네트워크를 실행하고 있습니다. 이제 상용 용지 거래를 원하는 MagnetoCorp의 역할을 시작하겠습니다.



## Working as MagnetoCorp

To monitor the MagnetoCorp components of PaperNet, an administrator can view the
aggregated output from a set of docker containers using the `logspout`
[tool](https://github.com/gliderlabs/logspout#logspout). It collects the
different output streams into one place, making it easy to see what's happening
from a single window. This can be really helpful for administrators when
installing smart contracts or for developers when invoking smart contracts, for
example.

PaperNet의 MagnetoCorp 구성 요소를 모니터링하기 위해 관리자는 logspout 도구를 사용하여 일련의 도커 컨테이너에서 집계 된 출력을 볼 수 있습니다. 다른 출력 스트림을 한 곳으로 수집하여 단일 창에서 발생하는 상황을 쉽게 확인할 수 있습니다. 예를 들어 스마트 계약을 설치할 때 관리자 나 스마트 계약을 호출 할 때 개발자에게 도움이 될 수 있습니다.

Let's now monitor PaperNet as a MagnetoCorp administrator. Open a new window in
the `fabric-samples` directory, and locate and run the `monitordocker.sh`
script to start the `logspout` tool for the PaperNet docker containers
associated with the docker network `net_basic`:

이제 PaperNet을 MagnetoCorp 관리자로 모니터링하겠습니다. fabric-samples 디렉토리에서 새 창을 열고 monitordocker.sh 스크립트를 찾아 실행하여 docker 네트워크 net_basic과 연관된 PaperNet 도커 컨테이너에 대한 logspout 도구를 시작하십시오.

```
(magnetocorp admin)$ cd commercial-paper/organization/magnetocorp/configuration/cli/
(magnetocorp admin)$ ./monitordocker.sh net_basic
...
latest: Pulling from gliderlabs/logspout
4fe2ade4980c: Pull complete
decca452f519: Pull complete
(...)
Starting monitoring on all containers on the network net_basic
b7f3586e5d0233de5a454df369b8eadab0613886fc9877529587345fc01a3582
```

Note that you can pass a port number to the above command if the default port in `monitordocker.sh` is already in use.

monitordocker.sh의 기본 포트가 이미 사용중인 경우 포트 번호를 위 명령에 전달할 수 있습니다.

```
(magnetocorp admin)$ ./monitordocker.sh net_basic <port_number>
```

This window will now show output from the docker containers, so let's start
another terminal window which will allow the MagnetoCorp administrator to
interact with the network.

이 창에는 이제 도커 컨테이너의 출력이 표시되므로 MagnetoCorp 관리자가 네트워크와 상호 작용할 수있는 다른 터미널 창을 시작하겠습니다.

![commercialpaper.workmagneto](./commercial_paper.diagram.4.png) *A MagnetoCorp
administrator interacts with the network via a docker container.*

To interact with PaperNet, a MagnetoCorp administrator needs to use the
Hyperledger Fabric `peer` commands. Conveniently, these are available pre-built
in the `hyperledger/fabric-tools`
[docker image](https://hub.docker.com/r/hyperledger/fabric-tools/).

PaperNet과 상호 작용하려면 MagnetoCorp 관리자가 Hyperledger Fabric 피어 명령을 사용해야합니다. 편리하게, 이들은 'hyperledger/fabric-tools'  도커 이미지에 사전 구축되어 있습니다.

Let's start a MagnetoCorp-specific docker container for the administrator using
the `docker-compose` [command](https://docs.docker.com/compose/overview/):

docker-compose 명령을 사용하여 관리자를위한 MagnetoCorp 특정 도커 컨테이너를 시작해 보겠습니다.

```
(magnetocorp admin)$ cd commercial-paper/organization/magnetocorp/configuration/cli/
(magnetocorp admin)$ docker-compose -f docker-compose.yml up -d cliMagnetoCorp

Pulling cliMagnetoCorp (hyperledger/fabric-tools:)...
latest: Pulling from hyperledger/fabric-tools
3b37166ec614: Already exists
(...)
Digest: sha256:058cff3b378c1f3ebe35d56deb7bf33171bf19b327d91b452991509b8e9c7870
Status: Downloaded newer image for hyperledger/fabric-tools:latest
Creating cliMagnetoCorp ... done
```

Again, see how the `hyperledger/fabric-tools` docker image was retrieved from
Docker Hub and added to the network:

다시 한 번 'hyperledger/fabric-tools' docker 이미지가 Docker Hub에서 검색되어 네트워크에 추가 된 방법을 확인하십시오.

```
(magnetocorp admin)$ docker ps

CONTAINER ID        IMAGE                        COMMAND                  CREATED              STATUS              PORTS                                            NAMES
562a88b25149        hyperledger/fabric-tools     "/bin/bash"              About a minute ago   Up About a minute                                                    cliMagnetoCorp
b7f3586e5d02        gliderlabs/logspout          "/bin/logspout"          7 minutes ago        Up 7 minutes        127.0.0.1:8000->80/tcp                           logspout
ada3d078989b        hyperledger/fabric-peer      "peer node start"        29 minutes ago       Up 29 minutes       0.0.0.0:7051->7051/tcp, 0.0.0.0:7053->7053/tcp   peer0.org1.example.com
1fa1fd107bfb        hyperledger/fabric-orderer   "orderer"                29 minutes ago       Up 29 minutes       0.0.0.0:7050->7050/tcp                           orderer.example.com
53fe614274f7        hyperledger/fabric-couchdb   "tini -- /docker-ent…"   29 minutes ago       Up 29 minutes       4369/tcp, 9100/tcp, 0.0.0.0:5984->5984/tcp       couchdb
469201085a20        hyperledger/fabric-ca        "sh -c 'fabric-ca-se…"   29 minutes ago       Up 29 minutes       0.0.0.0:7054->7054/tcp                           ca.example.com
```

The MagnetoCorp administrator will use the command line in container
`562a88b25149` to interact with PaperNet. Notice also the `logspout` container
`b7f3586e5d02`; this is capturing the output of all other docker containers for
the `monitordocker.sh` command.

MagnetoCorp 관리자는 컨테이너 562a88b25149의 명령 행을 사용하여 PaperNet과 상호 작용합니다. 또한 로그 아웃 컨테이너 b7f3586e5d02를 확인하십시오. 이것은 monitordocker.sh 명령에 대한 다른 모든 도커 컨테이너의 출력을 캡처합니다.

Let's now use this command line to interact with PaperNet as the MagnetoCorp
administrator.

이제이 명령 행을 사용하여 MagnetoCorp 관리자로서 PaperNet과 상호 작용하십시오.



## Smart contract

`issue`, `buy` and `redeem` are the three functions at the heart of the PaperNet
smart contract. It is used by applications to submit transactions which
correspondingly issue, buy and redeem commercial paper on the ledger. Our next
task is to examine this smart contract.

발행, 구매 및 사용은 PaperNet 스마트 계약의 핵심 기능입니다. 응용 프로그램에서 원장에 상업용 용지를 발행, 구매 및 교환하는 거래를 제출하는 데 사용됩니다. 다음 과제는이 현명한 계약을 검토하는 것입니다.

Open a new terminal window to represent a MagnetoCorp developer and change to
the directory that contains MagnetoCorp's copy of the smart contract to view it
with your chosen editor (VS Code in this tutorial):

MagnetoCorp 개발자를 나타내는 새 터미널 창을 열고 MagnetoCorp의 스마트 계약 사본이 포함 된 디렉토리로 변경하여 선택한 편집기 (이 자습서의 VS 코드)로이를 확인하십시오.

```
(magnetocorp developer)$ cd commercial-paper/organization/magnetocorp/contract
(magnetocorp developer)$ code .
```

In the `lib` directory of the folder, you'll see `papercontract.js` file -- this
contains the commercial paper smart contract!

폴더의 lib 디렉토리에 papercontract.js 파일이 표시됩니다. 여기에는 commercial paper 스마트 계약이 포함되어 있습니다!

![commercialpaper.vscode1](./commercial_paper.diagram.10.png) *An example code
editor displaying the commercial paper smart contract in `papercontract.js`*

`papercontract.js` is a JavaScript program designed to run in the node.js
environment. Note the following key program lines:

papercontract.js는 node.js 환경에서 실행되도록 설계된 JavaScript 프로그램입니다. 다음의 주요 프로그램 라인에 유의하십시오.

* `const { Contract, Context } = require('fabric-contract-api');`

  This statement brings into scope two key Hyperledger Fabric classes that will
  be used extensively by the smart contract  -- `Contract` and `Context`. You
  can learn more about these classes in the
  [`fabric-shim` JSDOCS](https://fabric-shim.github.io/).

  이 문장은 스마트 계약에 의해 광범위하게 사용될 두 가지 주요 Hyperledger Fabric 클래스 (계약 및 컨텍스트)를 제공합니다. fabric-shim JSDOCS에서 이러한 클래스에 대해 자세히 알아볼 수 있습니다.

* `class CommercialPaperContract extends Contract {`

  This defines the smart contract class `CommercialPaperContract` based on the
  built-in Fabric `Contract` class.  The methods which implement the key
  transactions to `issue`, `buy` and `redeem` commercial paper are defined
  within this class.

  내장 된 패브릭 계약 클래스를 기반으로 스마트 계약 클래스 CommercialPaperContract를 정의합니다. commercial paper 발행, 구매 및 교환하기 위해 주요 거래를 구현하는 방법은 이 클래스에서 정의됩니다.


* `async issue(ctx, issuer, paperNumber, issueDateTime, maturityDateTime...) {`

  This method defines the commercial paper `issue` transaction for PaperNet. The
  parameters that are passed to this method will be used to create the new
  commercial paper.
  
  이 방법은 PaperNet의 commercial paper 발행 거래를 정의합니다. 이 방법으로 전달 된 매개 변수는 새 상업용 용지를 만드는 데 사용됩니다.

  Locate and examine the `buy` and `redeem` transactions within the smart
  contract.
  
  스마트 계약 내에서 구매 및 교환 거래를 찾아서 검토하십시오.


* `let paper = CommercialPaper.createInstance(issuer, paperNumber, issueDateTime...);`

  Within the `issue` transaction, this statement creates a new commercial paper
  in memory using the `CommercialPaper` class with the supplied transaction
  inputs. Examine the `buy` and `redeem` transactions to see how they similarly
  use this class.
  
  문제 거래 내에서이 명세서는 제공된 거래 입력과 함께 CommercialPaper 클래스를 사용하여 메모리에 새 상용 용지를 만듭니다. 구매 및 교환 거래를 검토하여 유사하게이 클래스를 사용하는 방법을 확인하십시오.


* `await ctx.paperList.addPaper(paper);`

  This statement adds the new commercial paper to the ledger using
  `ctx.paperList`, an instance of a `PaperList` class that was created when the
  smart contract context `CommercialPaperContext` was initialized. Again,
  examine the `buy` and `redeem` methods to see how they use this class.
  
  이 명령문은 스마트 계약 컨텍스트 CommercialPaperContext가 초기화 될 때 작성된 PaperList 클래스의 인스턴스 인 ctx.paperList 를 사용하여 새로운 commercial paper 를 원장에 추가합니다. 다시 구매 및 사용 방법을 검토하여이 클래스를 어떻게 사용하는지 확인하십시오.


* `return paper.toBuffer();`

  This statement returns a binary buffer as response from the `issue`
  transaction for processing by the caller of the smart contract.
  
  이 명령문은 스마트 계약의 호출자가 처리하기 위해 발행 트랜잭션의 응답으로 이진 버퍼를 리턴합니다.


Feel free to examine other files in the `contract` directory to understand how
the smart contract works, and read in detail how `papercontract.js` is
designed in the smart contract [topic](../developapps/smartcontract.html).

contract 디렉토리의 다른 파일을 검토하여 스마트 계약의 작동 방식을 이해하고 스마트 계약 주제에서 papercontract.js 가 설계되는 방식을 자세히 읽으십시오.



## Install contract

Before `papercontract` can be invoked by applications, it must be installed onto
the appropriate peer nodes in PaperNet.  MagnetoCorp and DigiBank administrators
are able to install `papercontract` onto peers over which they respectively have
authority.

![commercialpaper.install](./commercial_paper.diagram.6.png) *A MagnetoCorp
administrator installs a copy of the `papercontract` onto a MagnetoCorp peer.*

Smart contracts are the focus of application development, and are contained
within a Hyperledger Fabric artifact called [chaincode](../chaincode.html). One
or more smart contracts can be defined within a single chaincode, and installing
a chaincode will allow them to be consumed by the different organizations in
PaperNet.  It means that only administrators need to worry about chaincode;
everyone else can think in terms of smart contracts.

The MagnetoCorp administrator uses the `peer chaincode install` command to copy
the `papercontract` smart contract from their local machine's file system to the
file system within the target peer's docker container. Once the smart contract
is installed on the peer and instantiated on a channel,
`papercontract` can be invoked by applications, and interact with the ledger
database via the
[putState()](https://fabric-shim.github.io/release-1.3/fabric-shim.ChaincodeStub.html#putState__anchor)
and
[getState()](https://fabric-shim.github.io/release-1.3/fabric-shim.ChaincodeStub.html#getState__anchor)
Fabric APIs. Examine how these APIs are used by `StateList` class within
`ledger-api\statelist.js`.

Let's now install `papercontract` as the MagnetoCorp administrator. In the
MagnetoCorp administrator's command window, use the `docker exec` command to run
the `peer chaincode install` command in the `cliMagnetCorp` container:

```
(magnetocorp admin)$ docker exec cliMagnetoCorp peer chaincode install -n papercontract -v 0 -p /opt/gopath/src/github.com/contract -l node

2018-11-07 14:21:48.400 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-11-07 14:21:48.400 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-11-07 14:21:48.466 UTC [chaincodeCmd] install -> INFO 003 Installed remotely response:<status:200 payload:"OK" >
```

The `cliMagnetCorp` container has set
`CORE_PEER_ADDRESS=peer0.org1.example.com:7051` to target its commands to
`peer0.org1.example.com`, and the `INFO 003 Installed remotely...` indicates
`papercontract` has been successfully installed on this peer. Currently, the
MagnetoCorp administrator only has to install a copy of `papercontract` on a
single MagentoCorp peer.

Note how `peer chaincode install` command specified the smart contract path,
`-p`, relative to the `cliMagnetoCorp` container's file system:
`/opt/gopath/src/github.com/contract`. This path has been mapped to the local
file system path `.../organization/magnetocorp/contract` via the
`magnetocorp/configuration/cli/docker-compose.yml` file:

```yaml
volumes:
    - ...
    - ./../../../../organization/magnetocorp:/opt/gopath/src/github.com/
    - ...
```

See how the `volume` directive maps `organization/magnetocorp` to
`/opt/gopath/src/github.com/` providing this container access to your local file
system where MagnetoCorp's copy of the `papercontract` smart contract is held.

You can read more about `docker compose`
[here](https://docs.docker.com/compose/overview/) and `peer chaincode install`
command [here](../commands/peerchaincode.html).

## Instantiate contract

Now that `papercontract` chaincode containing the `CommercialPaper` smart
contract is installed on the required PaperNet peers, an administrator can make
it available to different network channels, so that it can be invoked by
applications connected to those channels. Because we're using the basic network
configuration for PaperNet, we're only going to make `papercontract` available
in a single network channel, `mychannel`.

![commercialpaper.instant](./commercial_paper.diagram.7.png) *A MagnetoCorp
administrator instantiates `papercontract` chaincode containing the smart
contract. A new docker chaincode container will be created to run
`papercontract`.*

The MagnetoCorp administrator uses the `peer chaincode instantiate` command to
instantiate `papercontract` on `mychannel`:

```
(magnetocorp admin)$ docker exec cliMagnetoCorp peer chaincode instantiate -n papercontract -v 0 -l node -c '{"Args":["org.papernet.commercialpaper:instantiate"]}' -C mychannel -P "AND ('Org1MSP.member')"

2018-11-07 14:22:11.162 UTC [chaincodeCmd] InitCmdFactory -> INFO 001 Retrieved channel (mychannel) orderer endpoint: orderer.example.com:7050
2018-11-07 14:22:11.163 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default escc
2018-11-07 14:22:11.163 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default vscc
```

One of the most important parameters on `instantiate` is `-P`. It specifies the
[endorsement policy](../endorsement-policies.html) for `papercontract`,
describing the set of organizations that must endorse (execute and sign) a
transaction before it can be determined as valid. All transactions, whether
valid or invalid, will be recorded on the [ledger blockchain](../ledger/ledger.html#blockchain),
but only valid transactions will update the [world
state](../ledger/ledger.html#world-state).

In passing, see how `instantiate` passes the orderer address
`orderer.example.com:7050`. This is because it additionally submits an
instantiate transaction to the orderer, which will include the transaction
in the next block and distribute it to all peers that have joined
`mychannel`, enabling any peer to execute the chaincode in their own
isolated chaincode container. Note that `instantiate` only needs to be issued
once for `papercontract` even though typically it is installed on many peers.

See how a `papercontract` container has been started with the `docker ps`
command:

```
(magnetocorp admin)$ docker ps

CONTAINER ID        IMAGE                                              COMMAND                  CREATED             STATUS              PORTS          NAMES
4fac1b91bfda        dev-peer0.org1.example.com-papercontract-0-d96...  "/bin/sh -c 'cd /usr…"   2 minutes ago       Up 2 minutes                       dev-peer0.org1.example.com-papercontract-0
```

Notice that the container is named
`dev-peer0.org1.example.com-papercontract-0-d96...` to indicate which peer
started it, and the fact that it's running `papercontract` version `0`.

Now that we've got a basic PaperNet up and running, and `papercontract`
installed and instantiated, let's turn our attention to the MagnetoCorp
application which issues a commercial paper.

## Application structure

The smart contract contained in `papercontract` is called by MagnetoCorp's
application `issue.js`. Isabella uses this application to submit a transaction
to the ledger which issues commercial paper `00001`. Let's quickly examine how
the `issue` application works.

![commercialpaper.application](./commercial_paper.diagram.8.png) *A gateway
allows an application to focus on transaction generation, submission and
response. It coordinates transaction proposal, ordering and notification
processing between the different network components.*

Because the `issue` application submits transactions on behalf of Isabella, it
starts by retrieving Isabella's X.509 certificate from her
[wallet](../developapps/wallet.html), which might be stored on the local file
system or a Hardware Security Module
[HSM](https://en.wikipedia.org/wiki/Hardware_security_module). The `issue`
application is then able to utilize the gateway to submit transactions on the
channel. The Hyperledger Fabric SDK provides a
[gateway](../developapps/gateway.html) abstraction so that applications can
focus on application logic while delegating network interaction to the
gateway. Gateways and wallets make it straightforward to write Hyperledger
Fabric applications.

So let's examine the `issue` application that Isabella is going to use. open a
separate terminal window for her, and in `fabric-samples` locate the MagnetoCorp
`/application` folder:

```
(magnetocorp user)$ cd commercial-paper/organization/magnetocorp/application/
(magnetocorp user)$ ls

addToWallet.js		issue.js		package.json
```

`addToWallet.js` is the program that Isabella is going to use to load her
identity into her wallet, and `issue.js` will use this identity to create
commercial paper `00001` on behalf of MagnetoCorp by invoking `papercontract`.

Change to the directory that contains MagnetoCorp's copy of the application
`issue.js`, and use your code editor to examine it:

```
(magnetocorp user)$ cd commercial-paper/organization/magnetocorp/application
(magnetocorp user)$ code issue.js
```

Examine this directory; it contains the issue application and all its
dependencies.

![commercialpaper.vscode2](./commercial_paper.diagram.11.png) *A code editor
displaying the contents of the commercial paper application directory.*

Note the following key program lines in `issue.js`:

* `const { FileSystemWallet, Gateway } = require('fabric-network');`

  This statement brings two key Hyperledger Fabric SDK classes into scope --
  `Wallet` and `Gateway`. Because Isabella's X.509 certificate is in the local
  file system, the application uses `FileSystemWallet`.


* `const wallet = new FileSystemWallet('../identity/user/isabella/wallet');`

  This statement identifies that the application will use `isabella` wallet when
  it connects to the blockchain network channel. The application will select a
  particular identity within `isabella` wallet. (The wallet must have been
  loaded with the Isabella's X.509 certificate -- that's what `addToWallet.js`
  does.)


* `await gateway.connect(connectionProfile, connectionOptions);`

  This line of code connects to the network using the gateway identified by
  `connectionProfile`, using the identity referred to in `ConnectionOptions`.

  See how `../gateway/networkConnection.yaml` and `User1@org1.example.com` are
  used for these values respectively.


* `const network = await gateway.getNetwork('mychannel');`

  This connects the application to the network channel `mychannel`, where the
  `papercontract` was previously instantiated.


*  `const contract = await network.getContract('papercontract', 'org.papernet.comm...');`

  This statement gives the application addressability to smart contract defined
  by the namespace `org.papernet.commercialpaper` within `papercontract`. Once
  an application has issued getContract, it can submit any transaction
  implemented within it.


* `const issueResponse = await contract.submitTransaction('issue', 'MagnetoCorp', '00001'...);`

  This line of code submits the a transaction to the network using the `issue`
  transaction defined within the smart contract. `MagnetoCorp`, `00001`... are
  the values to be used by the `issue` transaction to create a new commercial
  paper.

* `let paper = CommercialPaper.fromBuffer(issueResponse);`

  This statement processes the response from the `issue` transaction. The
  response needs to deserialized from a buffer into `paper`, a `CommercialPaper`
  object which can interpreted correctly by the application.


Feel free to examine other files in the `/application` directory to understand
how `issue.js` works, and read in detail how it is implemented in the
application [topic](../developapps/application.html).

## Application dependencies

The `issue.js` application is written in JavaScript and designed to run in the
node.js environment that acts as a client to the PaperNet network.
As is common practice, MagnetoCorp's application is built on many
external node packages -- to improve quality and speed of development. Consider
how `issue.js` includes the `js-yaml`
[package](https://www.npmjs.com/package/js-yaml) to process the YAML gateway
connection profile, or the `fabric-network`
[package](https://www.npmjs.com/package/fabric-network) to access the `Gateway`
and `Wallet` classes:

```JavaScript
const yaml = require('js-yaml');
const { FileSystemWallet, Gateway } = require('fabric-network');
```

These packages have to be downloaded from [npm](https://www.npmjs.com/) to the
local file system using the `npm install` command. By convention, packages must
be installed into an application-relative `/node_modules` directory for use at
runtime.

Examine the `package.json` file to see how `issue.js` identifies the packages to
download and their exact versions:

```json
  "dependencies": {
    "fabric-network": "^1.4.0-beta",
    "fabric-client": "^1.4.0-beta",
    "js-yaml": "^3.12.0"
  },
```

**npm** versioning is very powerful; you can read more about it
[here](https://docs.npmjs.com/getting-started/semantic-versioning).

Let's install these packages with the `npm install` command -- this may take up
to a minute to complete:

```
(magnetocorp user)$ npm install

(           ) extract:lodash: sill extract ansi-styles@3.2.1
(...)
added 738 packages in 46.701s
```

See how this command has updated the directory:

```
(magnetocorp user)$ ls

addToWallet.js		node_modules	      	package.json
issue.js	      	package-lock.json
```

Examine the `node_modules` directory to see the packages that have been
installed. There are lots, because `js-yaml` and `fabric-network` are themselves
built on other npm packages! Helpfully, the `package-lock.json`
[file](https://docs.npmjs.com/files/package-lock.json) identifies the exact
versions installed, which can prove invaluable if you want to exactly reproduce
environments; to test, diagnose problems or deliver proven applications for
example.

## Wallet

Isabella is almost ready to run `issue.js` to issue MagnetoCorp commercial paper
`00001`; there's just one remaining task to perform! As `issue.js` acts on
behalf of Isabella, and therefore MagnetoCorp, it will use identity from her
[wallet](../developapps/wallet.html) that reflects these facts. We now need to
perform this one-time activity of adding appropriate X.509 credentials to her
wallet.

In Isabella's terminal window, run the `addToWallet.js` program to add identity
information to her wallet:

```
(isabella)$ node addToWallet.js

done
```

Isabella can store multiple identities in her wallet, though in our example, she
only uses one -- `User1@org.example.com`. This identity is currently associated
with the basic network, rather than a more realistic PaperNet configuration --
we'll update this tutorial soon.

`addToWallet.js` is a simple file-copying program which you can examine at your
leisure. It moves an identity from the basic network sample to Isabella's
wallet. Let's focus on the result of this program -- the contents of
the wallet which will be used to submit transactions to `PaperNet`:

```
(isabella)$ ls ../identity/user/isabella/wallet/

User1@org1.example.com
```

See how the directory structure maps the `User1@org1.example.com` identity --
other identities used by Isabella would have their own folder. Within this
directory you'll find the identity information that `issue.js` will use on
behalf of `isabella`:


```
(isabella)$ ls ../identity/user/isabella/wallet/User1@org1.example.com

User1@org1.example.com      c75bd6911a...-priv      c75bd6911a...-pub
```

Notice:

* a private key `c75bd6911a...-priv` used to sign transactions on Isabella's
  behalf, but not distributed outside of her immediate control.


* a public key `c75bd6911a...-pub` which is cryptographically linked to
  Isabella's private key. This is wholly contained within Isabella's X.509
  certificate.


* a certificate `User1@org.example.com` which contains Isabella's public key
  and other X.509 attributes added by the Certificate Authority at certificate
  creation. This certificate is distributed to the network so that different
  actors at different times can cryptographically verify information created by
  Isabella's private key.

  Learn more about certificates
  [here](../identity/identity.html#digital-certificates). In practice, the
  certificate file also contains some Fabric-specific metadata such as
  Isabella's organization and role -- read more in the
  [wallet](../developapps/wallet.html) topic.

## Issue application

Isabella can now use `issue.js` to submit a transaction that will issue
MagnetoCorp commercial paper `00001`:

```
(isabella)$ node issue.js

Connect to Fabric gateway.
Use network channel: mychannel.
Use org.papernet.commercialpaper smart contract.
Submit commercial paper issue transaction.
Process issue transaction response.
MagnetoCorp commercial paper : 00001 successfully issued for value 5000000
Transaction complete.
Disconnect from Fabric gateway.
Issue program complete.
```

The `node` command initializes a node.js environment, and runs `issue.js`. We
can see from the program output that MagnetoCorp commercial paper 00001 was
issued with a face value of 5M USD.

As you've seen, to achieve this, the application invokes the `issue` transaction
defined in the `CommercialPaper` smart contract within `papercontract.js`. This
had been installed and instantiated in the network by the MagnetoCorp
administrator. It's the smart contract which interacts with the ledger via the
Fabric APIs, most notably `putState()` and `getState()`, to represent the new
commercial paper as a vector state within the world state. We'll see how this
vector state is subsequently manipulated by the `buy` and `redeem` transactions
also defined within the smart contract.

All the time, the underlying Fabric SDK handles the transaction endorsement,
ordering and notification process, making the application's logic
straightforward; the SDK uses a [gateway](../developapps/gateway.html) to
abstract away network details and
[connectionOptions](../developapps/connectoptions.html) to declare more advanced
processing strategies such as transaction retry.

Let's now follow the lifecycle of MagnetoCorp `00001` by switching our emphasis
to DigiBank, who will buy the commercial paper.

## Working as DigiBank

Now that commercial paper `00001`has been issued by MagnetoCorp, let's switch
context to interact with PaperNet as employees of DigiBank. First, we'll act as
administrator who will create a console configured to interact with PaperNet.
Then Balaji, an end user, will use Digibank's `buy` application to buy
commercial paper `00001`, moving it to the next stage in its lifecycle.

![commercialpaper.workdigi](./commercial_paper.diagram.5.png) *DigiBank
administrators and applications interact with the PaperNet network.*

As the tutorial currently uses the basic network for PaperNet, the network
configuration is quite simple. Administrators use a console similar to
MagnetoCorp, but configured for Digibank's file system. Likewise, Digibank end
users will use applications which invoke the same smart contract as MagnetoCorp
applications, though they contain Digibank-specific logic and configuration.
It's the smart contract which captures the shared business process, and the
ledger which holds the shared business data, no matter which applications call
them.

Let's open up a separate terminal to allow the DigiBank administrator to
interact with PaperNet. In `fabric-samples`:

```
(digibank admin)$ cd commercial-paper/organization/digibank/configuration/cli/
(digibank admin)$ docker-compose -f docker-compose.yml up -d cliDigiBank

(...)
Creating cliDigiBank ... done
```

This docker container is now available for Digibank administrators to interact
with the network:

```(digibank admin)$ docker ps
CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS              PORT         NAMES
858c2d2961d4        hyperledger/fabric-tools         "/bin/bash"              18 seconds ago      Up 18 seconds                    cliDigiBank
```

In this tutorial, you'll use the command line container named `cliDigiBank` to
interact with the network on behalf of DigiBank. We've not shown all the docker
containers, and in the real world DigiBank users would only see the network
components (peers, orderers, CAs) to which they have access.

Digibank's administrator doesn't have much to do in this tutorial right now
because the PaperNet network configuration is so simple. Let's turn our
attention to Balaji.

## Digibank applications

Balaji uses DigiBank's `buy` application to submit a transaction to the ledger
which transfers ownership of commercial paper `00001` from MagnetoCorp to
DigiBank. The `CommercialPaper` smart contract is the same as that used by
MagnetoCorp's application, however the transaction is different this time --
it's `buy` rather than `issue`. Let's examine how DigiBank's application works.

Open a separate terminal window for Balaji. In `fabric-samples`, change to the
DigiBank application directory that contains the application, `buy.js`, and open
it with your editor:

```
(balaji)$ cd commercial-paper/organization/digibank/application/
(balaji)$ code buy.js
```

As you can see, this directory contains both the `buy` and `redeem` applications
that will be used by Balaji.



![commercialpaper.vscode3](./commercial_paper.diagram.12.png) *DigiBank's
commercial paper directory containing the `buy.js` and `redeem.js`
applications.*

DigiBank's `buy.js` application is very similar in structure to MagnetoCorp's
`issue.js` with two important differences:


  * **Identity**: the user is a DigiBank user `Balaji` rather than MagnetoCorp's
    `Isabella`

    ```JavaScript
    const wallet = new FileSystemWallet('../identity/user/balaji/wallet');`
    ```

    See how the application uses the `balaji` wallet when it connects to the
    PaperNet network channel. `buy.js` selects a particular identity within
    `balaji` wallet.


  * **Transaction**: the invoked transaction is `buy` rather than `issue`

    ```JavaScript
    `const buyResponse = await contract.submitTransaction('buy', 'MagnetoCorp', '00001'...);`
    ```

    A `buy` transaction is submitted with the values `MagnetoCorp`, `00001`...,
    that are used by the `CommercialPaper` smart contract class to transfer
    ownership of commercial paper `00001` to DigiBank.

Feel free to examine other files in the `application` directory to understand
how the application works, and read in detail how `buy.js` is implemented in
the application [topic](../developapps/application.html).

## Run as DigiBank

The DigiBank applications which buy and redeem commercial paper have a very
similar structure to MagnetoCorp's issue application. Therefore, let’s install
their dependencies and set up Balaji's wallet so that he can use these
applications to buy and redeem commercial paper.

Like MagnetoCorp, Digibank must the install the required application packages
using the `npm install` command, and again, this make take a short time to
complete.

In the DigiBank administrator window, install the application dependencies:

```
(digibank admin)$ cd commercial-paper/organization/digibank/application/
(digibank admin)$ npm install

(            ) extract:lodash: sill extract ansi-styles@3.2.1
(...)
added 738 packages in 46.701s
```

In Balaji's terminal window, run the `addToWallet.js` program to add identity
information to his wallet:

```
(balaji)$ node addToWallet.js

done
```

The `addToWallet.js` program has added identity information for `balaji`, to his
wallet, which will be used by `buy.js` and `redeem.js` to submit transactions to
`PaperNet`.

Like Isabella, Balaji can store multiple identities in his wallet, though in our
example, he only uses one -- `Admin@org.example.com`. His corresponding wallet
structure `digibank/identity/user/balaji/wallet/Admin@org1.example.com`
contains is very similar Isabella's -- feel free to examine it.

## Buy application

Balaji can now use `buy.js` to submit a transaction that will transfer ownership
of MagnetoCorp commercial paper `00001` to DigiBank.

Run the `buy` application in Balaji's window:

```
(balaji)$ node buy.js

Connect to Fabric gateway.
Use network channel: mychannel.
Use org.papernet.commercialpaper smart contract.
Submit commercial paper buy transaction.
Process buy transaction response.
MagnetoCorp commercial paper : 00001 successfully purchased by DigiBank
Transaction complete.
Disconnect from Fabric gateway.
Buy program complete.
```

You can see the program output that MagnetoCorp commercial paper 00001 was
successfully purchased by Balaji on behalf of DigiBank. `buy.js` invoked the
`buy` transaction defined in the `CommercialPaper` smart contract which updated
commercial paper `00001` within the world state using the `putState()` and
`getState()` Fabric APIs. As you've seen, the application logic to buy and issue
commercial paper is very similar, as is the smart contract logic.

## Redeem application

The final transaction in the lifecycle of commercial paper `00001` is for
DigiBank to redeem it with MagnetoCorp. Balaji uses `redeem.js` to submit a
transaction to perform the redeem logic within the smart contract.

Run the `redeem` transaction in Balaji's window:

```
(balaji)$ node redeem.js

Connect to Fabric gateway.
Use network channel: mychannel.
Use org.papernet.commercialpaper smart contract.
Submit commercial paper redeem transaction.
Process redeem transaction response.
MagnetoCorp commercial paper : 00001 successfully redeemed with MagnetoCorp
Transaction complete.
Disconnect from Fabric gateway.
Redeem program complete.
```

Again, see how the commercial paper 00001 was successfully redeemed when
`redeem.js` invoked the `redeem` transaction defined in `CommercialPaper`.
Again, it updated commercial paper `00001` within the world state to reflect
that the ownership returned to MagnetoCorp, the issuer of the paper.

## Further reading

To understand how applications and smart contracts shown in this tutorial work
in more detail, you'll find it helpful to read
[Developing Applications](../developapps/developing_applications.html). This
topic will give you a fuller explanation of the commercial paper scenario, the
`PaperNet` business network, its actors, and how the applications and smart
contracts they use work in detail.

Also feel free to use this sample to start creating your own applications and
smart contracts!

<!--- Licensed under Creative Commons Attribution 4.0 International License
https://creativecommons.org/licenses/by/4.0/ -->
