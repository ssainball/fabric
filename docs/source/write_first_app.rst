Writing Your First Application
==============================

.. note:: If you're not yet familiar with the fundamental architecture of a
          Fabric network, you may want to visit the :doc:`key_concepts` section
          prior to continuing.
          
          ``패브릭 네트워크의 기본 아키텍처에 아직 익숙하지 않은 경우 계속하기 전에 Key Concepts 섹션을 방문하십시오.``

          It is also worth noting that this tutorial serves as an introduction
          to Fabric applications and uses simple smart contracts and
          applications. For a more in-depth look at Fabric applications and
          smart contracts, check out our
          :doc:`developapps/developing_applications` section or the
          :doc:`tutorial/commercial_paper`.
          
          ``본 자습서는 패브릭 어플리케이션의 소개 역할을 하며 간단한 스마트 계약과 어플리케이션을 사용한다는 점도 주목할 가치가 있습니다. 패브릭 애플리케이션 및 스마트 계약에 대한 자세한 내용은 애플리케이션 개발 섹션 또는 상용 문서 튜토리얼을 참조하십시오.``

In this tutorial we'll be looking at a handful of sample programs to see how
Fabric apps work. These applications and the smart contracts they use are
collectively known as ``FabCar``. They provide a great starting point to
understand a Hyperledger Fabric blockchain. You'll learn how to write an
application and smart contract to query and update a ledger, and how to use a
Certificate Authority to generate the X.509 certificates used by applications
which interact with a permissioned blockchain.

''이 튜토리얼에서는 패브릭 앱이 어떻게 작동하는지 보기 위해 몇 개의 샘플 프로그램을 살펴 보겠습니다. 이러한 애플리케이션과 이들이 사용하는 스마트 계약은 일반적으로 FabCar라고 합니다. 그들은 Hyperledger Fabric 블록체인을 이해하기 위한 훌륭한 출발점을 제공합니다. 원장을 쿼리하고 업데이트하기 위해 응용 프로그램 및 스마트 계약을 작성하는 방법과 인증 기관을 사용하여 권한이 부여 된 블록 체인과 상호 작용하는 응용 프로그램에서 사용하는 X.509 인증서를 생성하는 방법에 대해 알아 봅니다.''



We will use the application SDK --- described in detail in the
:doc:`/developapps/application` topic -- to invoke a smart contract which
queries and updates the ledger using the smart contract SDK --- described in
detail in section :doc:`/developapps/smartcontract`.

''우리는 애플리케이션 SDK를 사용하겠습니다(Application topic에 자세히 설명되어 있음). 스마트 계약 SDK를 사용하여 원장을 쿼리하고 업데이트하는 스마트 계약을 호출합니다(Smart Contract Processing 섹션에서 자세히 설명되어 있음). ''



We’ll go through three principle steps:

''다음 세 가지 기본 단계를 거칩니다.''



  **1. Setting up a development environment.** Our application needs a network
  to interact with, so we'll get a basic network our smart contracts and
  application will use.
  
  ''개발 환경 설정. 우리의 어플리케이션은 상호작용 할 네트워크가 필요하므로 스마트 계약과 응용 프로그램에서 사용할 기본 네트워크를 얻게 됩니다.''

  .. image:: images/AppConceptsOverview.png

  **2. Learning about a sample smart contract, FabCar.**
  We’ll inspect the smart contract to learn about the transactions within them,
  and how they are used by applications to query and update the ledger.
  
  ''스마트 계약 샘플(FabCar)에 대해 학습합니다. 우리는 JavaScript로 작성된 현명한 계약을 사용합니다. 스마트 계약을 검사하여 거래 내에서의 거래와 응용 프로그램에서 원장을 쿼리하고 업데이트하는 데 사용되는 방법에 대해 알아 봅니다.''

  **3. Develop a sample application which uses FabCar.** Our application will
  use the FabCar smart contract to query and update car assets on the ledger.
  We'll get into the code of the apps and the transactions they create,
  including querying a car, querying a range of cars, and creating a new car.
  
  ''FabCar를 사용하는 샘플 어플리케이션을 개발 하십시오. 우리의 애플리케이션은 FabCar 스마트 계약을 사용하여 원장의 자동차 자산을 조회하고 업데이트 합니다. 우리는 자동차 쿼리, 다양한 자동차 쿼리 및 새 자동차 만들기를 포함하여 앱의 코드와 앱이 생성하는 트랜잭션에 대해 알아 봅니다.''



After completing this tutorial you should have a basic understanding of how an
application is programmed in conjunction with a smart contract to interact with
the ledger hosted and replicated on the peers in a Fabric network.

''이 튜토리얼을 완료한 후, 패브릭 네트워크의 피어에서 호스팅 되고 복제된 원장과 상호 작용하기 위해 스마트 계약과 함께 응용 프로그램을 프로그래밍 하는 방법에 대한 기본 지식을 이해해야 합니다.''



.. note:: These applications are also compatible with :doc:`discovery-overview`
          and :doc:`private-data/private-data`, though we won't explicitly show
          how to use our apps to leverage those features.
          
          ''이러한 응용 프로그램은 'Service Discovery' 및 'Private data' 와도 호환되지만 앱을 사용하여 이러한 기능을 활용하는 방법을 명시적으로 보여주지는 않습니다.''



Set up the blockchain network
-----------------------------

.. note:: This next section requires you to be in the ``first-network``
          subdirectory within your local clone of the ``fabric-samples`` repo.
          
          ''다음 섹션에서는 'fabric-samples' 리포지토리의 로컬 복사본 내 'first-network' 하위 디렉토리에 있어야 합니다.''



If you've already run through :doc:`build_network`, you will have downloaded
``fabric-samples`` and have a network up and running. Before you run this
tutorial, you must stop this network:

''이미 'Building Your First Network' 구축을 진행 한 경우 'fabric-samples' 을 다운로드하고 네트워크를 가동 할 수 있습니다. 이 학습서를 실행하기 전에이 네트워크를 중지해야합니다.''



.. code:: bash

  ./byfn.sh down

If you have run through this tutorial before, use the following commands to
kill any stale or active containers. Note, this will take down **all** of your
containers whether they're Fabric related or not.

이 학습서를 전에 실행 한 경우 다음 명령을 사용하여 오래된 컨테이너 또는 활성 컨테이너를 종료하십시오. 참고로 패브릭 관련 여부에 관계없이 모든 컨테이너가 중단됩니다.



.. code:: bash

  docker rm -f $(docker ps -aq)
  docker rmi -f $(docker images | grep fabcar | awk '{print $3}')

If you don't have a development environment and the accompanying artifacts for
the network and applications, visit the :doc:`prereqs` page and ensure you have
the necessary dependencies installed on your machine.

개발 환경과 네트워크 및 애플리케이션에 대한 아티팩트가 없는 경우 Prerequisites 페이지를 방문하여 시스템에 필요한 종속성(dependencies)이 설치되어 있는지 확인하십시오.



Next, if you haven't done so already, visit the :doc:`install` page and follow
the provided instructions. Return to this tutorial once you have cloned the
``fabric-samples`` repository, and downloaded the latest stable Fabric images
and available utilities.

그런 다음 샘플, 바이너리 및 도커 이미지 설치 페이지를 방문하여 제공된 지침을 따르십시오. 'fabric-samples' 저장소를 복제하고 최신 안정적인 패브릭 이미지와 사용 가능한 유틸리티를 다운로드한 후 이 튜토리얼로 돌아오십시오.



If you are using Mac OS and running Mojave, you will need to `install Xcode
<./tutorial/installxcode.html>`_.

Mac OS를 사용하고 Mojave를 실행하고 있다면 Xcode를 설치해야 합니다.



Launch the network
^^^^^^^^^^^^^^^^^^

.. note:: This next section requires you to be in the ``fabcar``
          subdirectory within your local clone of the ``fabric-samples`` repo.

          This tutorial demonstrates the JavaScript versions of the ``FabCar``
          smart contract and application, but the ``fabric-samples`` repo also
          contains Java and TypeScript versions of this sample. To try the
          Java or TypeScript versions, change the ``javascript`` argument
          for ``./startFabric.sh`` below to either ``java`` or ``typescript``
          and follow the instructions written to the terminal.

Launch your network using the ``startFabric.sh`` shell script. This command will
spin up a blockchain network comprising peers, orderers, certificate
authorities and more.  It will also install and instantiate a JavaScript version
of the ``FabCar`` smart contract which will be used by our application to access
the ledger. We'll learn more about these components as we go through the
tutorial.

startFabric.sh 쉘 스크립트를 사용하여 네트워크를 시작하십시오. 이 명령은 피어, 주문자, 인증 기관 등으로 구성된 블록 체인 네트워크를 가동시킵니다. 또한 응용 프로그램에서 원장에 액세스하는 데 사용할 FabCar 스마트 계약의 JavaScript 버전을 설치하고 인스턴스화합니다. 자습서를 진행하면서 이러한 구성 요소에 대해 자세히 알아 봅니다.



.. code:: bash

  ./startFabric.sh javascript

Alright, you’ve now got a sample network up and running, and the ``FabCar``
smart contract installed and instantiated. Let’s install our application
pre-requisites so that we can try it out, and see how everything works together.

이제 샘플 네트워크가 설정되어 있고 FabCar 스마트 계약이 설치되어 인스턴스화되었습니다. 응용 프로그램 사전 요구 사항을 설치하여 시험해보고 모든 것이 어떻게 작동하는지 살펴 보겠습니다.



Install the application
^^^^^^^^^^^^^^^^^^^^^^^

.. note:: The following instructions require you to be in the
          ``fabcar/javascript`` subdirectory within your local clone of the
          ``fabric-samples`` repo.

Run the following command to install the Fabric dependencies for the
applications. It will take about a minute to complete:

다음 명령을 실행하여 응용 프로그램에 대한 패브릭 종속성을 설치하십시오. 완료하는 데 약 1분 정도 소요:



.. code:: bash

  npm install

This process is installing the key application dependencies defined in
``package.json``. The most important of which is the ``fabric-network`` class;
it enables an application to use identities, wallets, and gateways to connect to
channels, submit transactions, and wait for notifications. This tutorial also
uses the ``fabric-ca-client`` class to enroll users with their respective
certificate authorities, generating a valid identity which is then used by
``fabric-network`` class methods.

이 프로세스는 inpackage.json에 정의 된 주요 애플리케이션 종속성을 설치합니다. 가장 중요한 것은 패브릭 네트워크 클래스입니다. 애플리케이션이 ID, 지갑 및 게이트웨이를 사용하여 채널에 연결하고 트랜잭션을 제출하며 알림을 기다릴 수 있습니다. 이 학습서에서는 fabric-ca-client 클래스를 사용하여 사용자를 각각의 인증 기관에 등록하여 유효한 ID를 생성 한 다음 fabric-network 클래스 메소드에서 사용합니다.

Once ``npm install`` completes, everything is in place to run the application.
For this tutorial, you'll primarily be using the application JavaScript files in
the ``fabcar/javascript`` directory. Let's take a look at what's inside:

npm 설치가 완료되면 모든 것이 응용 프로그램을 실행하기위한 것입니다. 이 학습서에서는 주로 'fabcar/javascript' 디렉토리에서 애플리케이션 JavaScript 파일을 사용합니다. 내부 내용을 살펴 보겠습니다.



.. code:: bash

  ls

You should see the following:

다음과 같은 화면을 볼 수 있습니다:



.. code:: bash

  enrollAdmin.js  node_modules       package.json  registerUser.js
  invoke.js       package-lock.json  query.js      wallet

There are files for other program languages, for example in the
``fabcar/typescript`` directory. You can read these once you've used the
JavaScript example -- the principles are the same.

예를 들어 fabcar/typescript 디렉토리에 다른 프로그램 언어에 대한 파일이 있습니다. JavaScript 예를 사용한 후에는 이러한 내용을 읽을 수 있으며, 원칙은 동일합니다.



If you are using Mac OS and running Mojave, you will need to `install Xcode
<./tutorial/installxcode.html>`_.

Mac OS를 사용하고 Mojave를 실행하고 있다면 Xcode를 설치해야 합니다.



Enrolling the admin user
------------------------

.. note:: The following two sections involve communication with the Certificate
          Authority. You may find it useful to stream the CA logs when running
          the upcoming programs by opening a new terminal shell and running
          ``docker logs -f ca.example.com``.

When we created the network, an admin user --- literally called ``admin`` ---
was created as the **registrar** for the certificate authority (CA). Our first
step is to generate the private key, public key, and X.509 certificate for
``admin`` using the ``enroll.js`` program. This process uses a **Certificate
Signing Request** (CSR) --- the private and public key are first generated
locally and the public key is then sent to the CA which returns an encoded
certificate for use by the application. These three credentials are then stored
in the wallet, allowing us to act as an administrator for the CA.

네트워크를 만들 때, admin 사용자가 CA(인증 기관)의 등록자로 생성되었습니다. 우리의 첫 번째 단계는 enroll.js 프로그램을 사용하여 관리자를 위한 개인키, 공개키, X.509 인증서를 생성하는 것입니다. 이 프로세스는 CSR(인증서 서명 요청)을 사용합니다. 즉, 개인키 및 공용키가 먼저 로컬에서 생성되고 그 후에 공용키가 CA로 전송되어 응용 프로그램에서 사용하기 위해 인코딩된 인증서를 반환합니다. 그러면 이 세 가지 자격 증명이 지갑에 저장되어 우리가 CA의 관리자 역할을 수행 할 수 있습니다.



We will subsequently register and enroll a new application user which will be
used by our application to interact with the blockchain.

이후 우리는 새로운 애플리케이션 사용자 등록 및 권한 부여를 할 것이며, 이 사용자는 우리의 애플리케이션에서 블록체인과의 상호작용을 위해 사용될 것입니다.



Let's enroll user ``admin``:

사용자 관리자를 등록하십시오.



.. code:: bash

  node enrollAdmin.js

This command has stored the CA administrator's credentials in the ``wallet``
directory.

이 명령은 CA 관리자의 자격증명을 wallet 디렉토리에 저장했습니다.



Register and enroll ``user1``
-----------------------------

Now that we have the administrator's credentials in a wallet, we can enroll a
new user --- ``user1`` --- which will be used to query and update the ledger:

이제 우리는 관리자 자격 증명을 지갑에 가지고 있으므로, 새로운 사용자인 user1을 등록할 수 있으며, user1은 원장을 쿼리하고 업데이트하는 데 사용될 것입니다.



.. code:: bash

  node registerUser.js

Similar to the admin enrollment, this program uses a CSR to enroll ``user1`` and
store its credentials alongside those of ``admin`` in the wallet. We now have
identities for two separate users --- ``admin`` and ``user1`` --- and these are
used by our application.

관리자 등록과 마찬가지로이 프로그램은 CSR을 사용하여 user1을 등록하고 관리자의 자격 증명과 함께 지갑에 자격 증명을 저장합니다. 이제 두 명의 개별 사용자 (admin 및 user1)에 대한 ID가 있으며 이들은 응용 프로그램에서 사용됩니다.



Time to interact with the ledger...

원장과 교감할 시간...



Querying the ledger
-------------------

Each peer in a blockchain network hosts a copy of the ledger, and an application
program can query the ledger by invoking a smart contract which queries the most
recent value of the ledger and returns it to the application.

블록 체인 네트워크의 각 피어는 원장의 사본을 호스팅하며 응용 프로그램은 가장 최근의 원장 가치를 쿼리하고이를 응용 프로그램으로 반환하는 스마트 계약을 호출하여 원장을 쿼리 할 수 있습니다.



Here is a simplified representation of how a query works:

다음은 쿼리의 작동 방식을 단순화한 것입니다.



.. image:: tutorial/write_first_app.diagram.1.png

Applications read data from the `ledger <./ledger/ledger.html>`_ using a query.
The most common queries involve the current values of data in the ledger -- its
`world state <./ledger/ledger.html#world-state>`_. The world state is
represented as a set of key-value pairs, and applications can query data for a
single key or multiple keys. Moreover, the ledger world state can be configured
to use a database like CouchDB which supports complex queries when key-values
are modeled as JSON data. This can be very helpful when looking for all assets
that match certain keywords with particular values; all cars with a particular
owner, for example.

응용 프로그램은 쿼리를 사용하여 원장에서 데이터를 읽습니다. 가장 일반적인 쿼리에는 원장의 현재 데이터 값인 'world state'가 포함됩니다. world state 는 일련의 키-값 쌍으로 표시되며 응용 프로그램은 단일 키 또는 여러 키에 대한 데이터를 쿼리 할 수 있습니다. 또한 원장 world state 는 키-값이 JSON 데이터로 모델링 될 때 복잡한 쿼리를 지원하는 CouchDB와 같은 데이터베이스를 사용하도록 구성 할 수 있습니다. 이는 특정 키워드가 특정 값과 일치하는 모든 자산을 찾을 때 매우 유용합니다. 예를 들어 특정 소유자의 자동차 등을 검색 할 수 있습니다.



First, let's run our ``query.js`` program to return a listing of all the cars on
the ledger. This program uses our second identity -- ``user1`` -- to access the
ledger:

먼저, query.js 프로그램을 실행하여 원장에 있는 모든 차들의 목록을 반환하십시오. 이 프로그램은 두 번째 ID인 user1을 사용하여 원장에 액세스 합니다:



.. code:: bash

  node query.js

The output should look like this:

출력은 다음과 같습니다.



.. code:: json

  Wallet path: ...fabric-samples/fabcar/javascript/wallet
  Transaction has been evaluated, result is:
  [{"Key":"CAR0", "Record":{"colour":"blue","make":"Toyota","model":"Prius","owner":"Tomoko"}},
  {"Key":"CAR1", "Record":{"colour":"red","make":"Ford","model":"Mustang","owner":"Brad"}},
  {"Key":"CAR2", "Record":{"colour":"green","make":"Hyundai","model":"Tucson","owner":"Jin Soo"}},
  {"Key":"CAR3", "Record":{"colour":"yellow","make":"Volkswagen","model":"Passat","owner":"Max"}},
  {"Key":"CAR4", "Record":{"colour":"black","make":"Tesla","model":"S","owner":"Adriana"}},
  {"Key":"CAR5", "Record":{"colour":"purple","make":"Peugeot","model":"205","owner":"Michel"}},
  {"Key":"CAR6", "Record":{"colour":"white","make":"Chery","model":"S22L","owner":"Aarav"}},
  {"Key":"CAR7", "Record":{"colour":"violet","make":"Fiat","model":"Punto","owner":"Pari"}},
  {"Key":"CAR8", "Record":{"colour":"indigo","make":"Tata","model":"Nano","owner":"Valeria"}},
  {"Key":"CAR9", "Record":{"colour":"brown","make":"Holden","model":"Barina","owner":"Shotaro"}}]

Let's take a closer look at this program. Use an editor (e.g. atom or visual
studio) and open ``query.js``.

이 프로그램을 자세히 살펴 보겠습니다. 편집기 (예 : atom 또는 visual studio)를 사용하고 query.js를 엽니다.



The application starts by bringing in scope two key classes from the
``fabric-network`` module; ``FileSystemWallet`` and ``Gateway``. These classes
will be used to locate the ``user1`` identity in the wallet, and use it to
connect to the network:

애플리케이션은 패브릭 네트워크 모듈에서 두 가지 핵심 클래스를 가져 와서 시작합니다. FileSystemWallet 및 Gateway. 이 클래스는 전자 지갑에서 user1 ID를 찾고 네트워크에 연결하는 데 사용됩니다.



.. code:: bash

  const { FileSystemWallet, Gateway } = require('fabric-network');

The application connects to the network using a gateway:

응용 프로그램은 Gateway 를 사용하여 네트워크에 연결합니다.



.. code:: bash

  const gateway = new Gateway();
  await gateway.connect(ccp, { wallet, identity: 'user1' });

This code creates a new gateway and then uses it to connect the application to
the network. ``ccp`` describes the network that the gateway will access with the
identity ``user1`` from ``wallet``. See how the ``ccp`` has been loaded from
``../../basic-network/connection.json`` and parsed as a JSON file:

이 코드는 새 게이트웨이를 만든 다음 이를 사용하여 응용 프로그램을 네트워크에 연결합니다. ccp는 게이트웨이가 월렛에서 ID user1으로 gateway가 액세스 할 네트워크를 설명합니다. ccp가 ../../basic-network/connection.json에서 로드되어 JSON 파일로 구문 분석 된 방법을 참조하십시오.



.. code:: bash

  const ccpPath = path.resolve(__dirname, '..', '..', 'basic-network', 'connection.json');
  const ccpJSON = fs.readFileSync(ccpPath, 'utf8');
  const ccp = JSON.parse(ccpJSON);

If you'd like to understand more about the structure of a connection profile,
and how it defines the network, check out
`the connection profile topic <./developapps/connectionprofile.html>`_.

connection profile 의 구조 및 네트워크를 정의하는 방법에 대한 자세한 내용을 보려면 connection profile topic 을 확인하십시오.


A network can be divided into multiple channels, and the next important line of
code connects the application to a particular channel within the network,
``mychannel``:

네트워크는 여러 채널로 나눌 수 있으며 다음으로 중요한 코드는 애플리케이션을 네트워크 내의 특정 채널 인 mychannel에 연결합니다.



.. code:: bash
  const network = await gateway.getNetwork('mychannel');

  const network = await gateway.getNetwork('mychannel');

Within this channel, we can access the smart contract ``fabcar`` to interact
with the ledger:

이 채널 내에서 스마트 계약 fabcar에 액세스하여 원장과 상호 작용할 수 있습니다.



.. code:: bash

  const contract = network.getContract('fabcar');

Within ``fabcar`` there are many different **transactions**, and our application
initially uses the ``queryAllCars`` transaction to access the ledger world state
data:

fabcar에는 여러 가지 다른 트랜잭션이 있으며, 애플리케이션은 처음에 queryAllCars 트랜잭션을 사용하여 원장 world state 데이터에 액세스합니다.



.. code:: bash

  const result = await contract.evaluateTransaction('queryAllCars');

The ``evaluateTransaction`` method represents one of the simplest interaction
with a smart contract in blockchain network. It simply picks a peer defined in
the connection profile and sends the request to it, where it is evaluated. The
smart contract queries all the cars on the peer's copy of the ledger and returns
the result to the application. This interaction does not result in an update the
ledger.

evaluateTransaction method는 블록체인 네트워크에서 스마트 계약과 가장 간단한 상호 작용 중 하나를 나타냅니다. 연결 프로파일에 정의 된 피어를 선택하여 요청을 보내 평가합니다. 스마트 계약은 피어의 원장 사본에있는 모든 자동차를 쿼리하고 결과를 응용 프로그램에 반환합니다. 이 상호 작용으로 인해 원장이 업데이트되지 않습니다.



The FabCar smart contract
-------------------------

Let's take a look at the transactions within the ``FabCar`` smart contract.
Navigate to the ``chaincode/fabcar/javascript/lib`` subdirectory at the root of
``fabric-samples`` and open ``fabcar.js`` in your editor.

FabCar 스마트 계약 내 거래를 살펴 보겠습니다. 'fabric-samples' 에서 chaincode/fabcar/javascript/lib 서브 디렉토리로 이동하여 편집기에서 fabcar.js를 여십시오.



See how our smart contract is defined using the ``Contract`` class:

Contract 클래스를 사용하여 스마트 계약이 어떻게 정의되는지 확인하십시오.



.. code:: bash

  class FabCar extends Contract {...

Within this class structure, you'll see that we have the following
transactions defined: ``initLedger``, ``queryCar``, ``queryAllCars``,
``createCar``, and ``changeCarOwner``. For example:

이 클래스 구조 내에서 initLedger, queryCar, queryAllCars, createCar 및 changeCarOwner 트랜잭션이 정의되어 있음을 알 수 있습니다. 예를 들면 다음과 같습니다.




.. code:: bash

  async queryCar(ctx, carNumber) {...}
  async queryAllCars(ctx) {...}

Let's take a closer look at the ``queryAllCars`` transaction to see how it
interacts with the ledger.

queryAllCars 트랜잭션을 자세히 살펴보고 장부와의 상호 작용 방식을 살펴 보겠습니다.



.. code:: bash

  async queryAllCars(ctx) {

    const startKey = 'CAR0';
    const endKey = 'CAR999';

    const iterator = await ctx.stub.getStateByRange(startKey, endKey);


This code defines the range of cars that ``queryAllCars`` will retrieve from the
ledger. Every car between ``CAR0`` and ``CAR999`` -- 1,000 cars in all, assuming
every key has been tagged properly -- will be returned by the query. The
remainder of the code iterates through the query results and packages them into
JSON for the application.

이 코드는 queryAllCars가 원장에서 검색 할 자동차 범위를 정의합니다. CAR0과 CAR999 사이의 모든 자동차(모든 키가 올바르게 태그되었다고 가정하면) 1,000 대의 자동차가 쿼리에 의해 반환됩니다. 나머지 코드는 쿼리 결과를 반복하여 애플리케이션의 JSON으로 패키지합니다.



Below is a representation of how an application would call different
transactions in a smart contract. Each transaction uses a broad set of APIs such
as ``getStateByRange`` to interact with the ledger. You can read more about
these APIs in `detail
<https://fabric-shim.github.io/master/index.html?redirect=true>`_.

아래는 스마트 계약에서 애플리케이션이 다른 트랜잭션을 호출하는 방법을 나타냅니다. 각 트랜잭션은 getStateByRange와 같은 광범위한 API를 사용하여 원장과 상호 작용합니다. 이러한 API에 대한 자세한 내용을 읽을 수 있습니다.



.. image:: images/RunningtheSample.png

We can see our ``queryAllCars`` transaction, and another called ``createCar``.
We will use this later in the tutorial to update the ledger, and add a new block
to the blockchain.

우리는 queryAllCars 거래(트랜잭션)와 createChar 로 불리는 또 다른 거래를 볼 수 있습니다. 튜토리얼 뒷부분에서 이것을 사용하여 원장을 업데이트하고 블록 체인에 새 블록을 추가합니다.



But first, go back to the ``query`` program and change the
``evaluateTransaction`` request to query ``CAR4``. The ``query`` program should
now look like this:

그러나 먼저 query 프로그램으로 돌아가서 evaluationTransaction 요청을 CAR4 쿼리로 변경하십시오. 쿼리 프로그램은 이제 다음과 같아야합니다.



.. code:: bash

  const result = await contract.evaluateTransaction('queryCar', 'CAR4');

Save the program and navigate back to your ``fabcar/javascript`` directory.
Now run the ``query`` program again:

프로그램을 저장하고 fabcar/javascript 디렉토리로 다시 이동하십시오. 이제 query 프로그램을 다시 실행하십시오.



.. code:: bash

  node query.js

You should see the following:

다음이 표시되어야합니다.



.. code:: json

  Wallet path: ...fabric-samples/fabcar/javascript/wallet
  Transaction has been evaluated, result is:
  {"colour":"black","make":"Tesla","model":"S","owner":"Adriana"}

If you go back and look at the result from when the transaction was
``queryAllCars``, you can see that ``CAR4`` was Adriana’s black Tesla model S,
which is the result that was returned here.

돌아가서 queryAllCars 트랜잭션 결과를 보면 CAR4가 Adriana의 검은색 테슬라 모델 S라는 것을 알 수 있습니다.이 결과는 여기로 반환됩니다.



We can use the ``queryCar`` transaction to query against any car, using its
key (e.g. ``CAR0``) and get whatever make, model, color, and owner correspond to
that car.

queryCar 트랜잭션을 사용하여 키(예 : CAR0)를 사용하여 모든 자동차에 대해 쿼리하고 해당 자동차에 해당하는 제조사, 모델, 색상 및 소유자를 얻을 수 있습니다.



Great. At this point you should be comfortable with the basic query transactions
in the smart contract and the handful of parameters in the query program.

이 시점에서 스마트 계약의 기본 쿼리 트랜잭션과 쿼리 프로그램의 몇 가지 매개 변수에 익숙해야합니다.



Time to update the ledger...

원장을 업데이트 할 시간…



Updating the ledger
-------------------

Now that we’ve done a few ledger queries and added a bit of code, we’re ready to
update the ledger. There are a lot of potential updates we could make, but
let's start by creating a **new** car.

원장 쿼리를 몇 번 수행하고 약간의 코드를 추가 했으므로 원장을 업데이트 할 준비가되었습니다. 우리가 할 수있는 많은 잠재적 인 업데이트가 있지만 새 차를 만드는 것으로 시작하겠습니다.



From an application perspective, updating the ledger is simple. An application
submits a transaction to the blockchain network, and when it has been
validated and committed, the application receives a notification that
the transaction has been successful. Under the covers this involves the process
of **consensus** whereby the different components of the blockchain network work
together to ensure that every proposed update to the ledger is valid and
performed in an agreed and consistent order.

응용 프로그램 관점에서 원장을 업데이트하는 것은 간단합니다. 응용 프로그램은 트랜잭션을 블록체인 네트워크에 제출하고, 유효성이 확인되고 커밋되면 응용 프로그램은 트랜잭션이 성공했다는 알림을받습니다. 여기에는 블록 체인 네트워크의 서로 다른 구성 요소가 함께 작동하여 원장에 대한 모든 제안 된 업데이트가 유효하고 합의되고 일관된 순서로 수행되도록하는 합의 과정이 포함됩니다.



.. image:: tutorial/write_first_app.diagram.2.png

Above, you can see the major components that make this process work. As well as
the multiple peers which each host a copy of the ledger, and optionally a copy
of the smart contract, the network also contains an ordering service. The
ordering service coordinates transactions for a network; it creates blocks
containing transactions in a well-defined sequence originating from all the
different applications connected to the network.

위의 과정을 수행하는 주요 구성 요소를 볼 수 있습니다. 각 원장의 사본과 선택적으로 스마트 계약의 사본을 호스팅하는 여러 피어뿐만 아니라 네트워크에는 주문 서비스도 포함됩니다. 주문 서비스는 네트워크의 거래를 조정합니다. 네트워크에 연결된 모든 다른 응용 프로그램에서 시작하여 잘 정의 된 순서로 트랜잭션을 포함하는 블록을 만듭니다.



Our first update to the ledger will create a new car. We have a separate program
called ``invoke.js`` that we will use to make updates to the ledger. Just as with
queries, use an editor to open the program and navigate to the code block where
we construct our transaction and submit it to the network:

원장에 대한 첫 업데이트는 새 차를 만들 것입니다. 원장을 업데이트하는 데 사용할 invoke.js라는 별도의 프로그램이 있습니다. 쿼리와 마찬가지로 편집기를 사용하여 프로그램을 열고 트랜잭션을 구성하고 네트워크에 제출하는 코드 블록으로 이동하십시오.



.. code:: bash

  await contract.submitTransaction('createCar', 'CAR12', 'Honda', 'Accord', 'Black', 'Tom');

See how the applications calls the smart contract transaction ``createCar`` to
create a black Honda Accord with an owner named Tom. We use ``CAR12`` as the
identifying key here, just to show that we don't need to use sequential keys.

애플리케이션이 스마트계약 트랜잭션 createCar를 호출하여 Tom이라는 소유자와 함께 검은 Honda Accord를 작성하는 방법을 참조하십시오. 여기서는 순차 키를 사용할 필요가 없음을 나타 내기 위해 CAR12를 식별키로 사용합니다.



Save it and run the program:

저장하고 프로그램을 실행하십시오.



.. code:: bash

  node invoke.js

If the invoke is successful, you will see output like this:

호출이 성공하면 다음과 같은 출력이 표시됩니다.



.. code:: bash

  Wallet path: ...fabric-samples/fabcar/javascript/wallet
  2018-12-11T14:11:40.935Z - info: [TransactionEventHandler]: _strategySuccess: strategy success for transaction "9076cd4279a71ecf99665aed0ed3590a25bba040fa6b4dd6d010f42bb26ff5d1"
  Transaction has been submitted

Notice how the ``invoke`` application interacted with the blockchain network
using the ``submitTransaction`` API, rather than ``evaluateTransaction``.

invoke 애플리케이션이 evaluateTransaction API 대신 submitTransaction API를 사용하여 블록 체인 네트워크와 어떻게 상호 작용했는지 확인하십시오.



.. code:: bash

  await contract.submitTransaction('createCar', 'CAR12', 'Honda', 'Accord', 'Black', 'Tom');

``submitTransaction`` is much more sophisticated than ``evaluateTransaction``.
Rather than interacting with a single peer, the SDK will send the
``submitTransaction`` proposal to every required organization's peer in the
blockchain network. Each of these peers will execute the requested smart
contract using this proposal, to generate a transaction response which it signs
and returns to the SDK. The SDK collects all the signed transaction responses
into a single transaction, which it then sends to the orderer. The orderer
collects and sequences transactions from every application into a block of
transactions. It then distributes these blocks to every peer in the network,
where every transaction is validated and committed. Finally, the SDK is
notified, allowing it to return control to the application.

submitTransaction은 evaluationTransaction보다 훨씬 정교합니다. SDK는 단일 피어와 상호 작용하지 않고 블록체인 네트워크에서 모든 필수 조직의 피어에게 submitTransaction 제안을 보냅니다. 이러한 각 피어는 이 제안을 사용하여 요청된 스마트 계약을 실행하여 서명하고 SDK로 반환하는 트랜잭션 응답을 생성합니다. SDK는 서명된 모든 트랜잭션 응답을 단일 트랜잭션으로 수집한 다음 주문자에게 보냅니다. 주문자는 모든 애플리케이션에서 트랜잭션을 수집하여 트랜잭션 블록으로 시퀀싱합니다. 그런 다음 이 블록을 네트워크의 모든 피어에 배포하여 모든 트랜잭션을 확인하고 커밋합니다. 마지막으로 SDK에 알림이 전달되어 응용 프로그램으로 제어권을 되돌릴 수 있습니다.



 note:: ``submitTransaction`` also includes a listener that checks to make
          sure the transaction has been validated and committed to the ledger.
          Applications should either utilize a commit listener, or
          leverage an API like ``submitTransaction`` that does this for you.
          Without doing this, your transaction may not have been successfully
          orderered, validated, and committed to the ledger.

``submitTransaction`` does all this for the application! The process by which
the application, smart contract, peers and ordering service work together to
keep the ledger consistent across the network is called consensus, and it is
explained in detail in this `section <./peers/peers.html>`_.

submitTransaction은 애플리케이션을 위해 이 모든 것을 수행합니다! 애플리케이션, 스마트 계약, 피어 및 ordering 서비스가 함께 작동하여 네트워크에서 원장을 일관되게 유지하는 프로세스를 합의라고하며 이 섹션에서 자세히 설명합니다.



To see that this transaction has been written to the ledger, go back to
``query.js`` and change the argument from ``CAR4`` to ``CAR12``.

이 트랜잭션이 원장에 작성되었는지 확인하려면 query.js로 돌아가서 인수를 CAR4에서 CAR12로 변경하십시오.



In other words, change this:

다시 말해, 이것을 바꾸십시오 :



.. code:: bash

  const result = await contract.evaluateTransaction('queryCar', 'CAR4');

To this:

이에 :



.. code:: bash

  const result = await contract.evaluateTransaction('queryCar', 'CAR12');



Save once again, then query:

다시 한 번 저장 한 후 다음을 쿼리하십시오.


.. code:: bash

  node query.js

Which should return this:

.. code:: bash

  Wallet path: ...fabric-samples/fabcar/javascript/wallet
  Transaction has been evaluated, result is:
  {"colour":"Black","make":"Honda","model":"Accord","owner":"Tom"}

Congratulations. You’ve created a car and verified that its recorded on the
ledger!

축하합니다 자동차를 만들고 장부에 기록 된 것을 확인했습니다!



So now that we’ve done that, let’s say that Tom is feeling generous and he
wants to give his Honda Accord to someone named Dave.

이제 Tom이  Dave라는 사람에게 Honda Accord를 주고 싶다고 가정 해 봅시다.



To do this, go back to ``invoke.js`` and change the smart contract transaction
from ``createCar`` to ``changeCarOwner`` with a corresponding change in input
arguments:

이렇게 하려면 invoke.js로 돌아가서 스마트 인수 트랜잭션을 createCar에서 changeCarOwner로 변경하고 입력 인수를 변경하십시오.



.. code:: bash

  await contract.submitTransaction('changeCarOwner', 'CAR12', 'Dave');

The first argument --- ``CAR12`` --- identifies the car that will be changing
owners. The second argument --- ``Dave`` --- defines the new owner of the car.

첫 번째 인수인 CAR12는 소유자를 변경할 자동차를 식별합니다. 두 번째 인수인 Dave는 자동차의 새로운 소유자를 정의합니다.



Save and execute the program again:

프로그램을 저장하고 다시 실행하십시오.



.. code:: bash

  node invoke.js

Now let’s query the ledger again and ensure that Dave is now associated with the
``CAR12`` key:

이제 원장을 다시 쿼리하고 Dave가 이제 CAR12 키와 연결되어 있는지 확인하십시오.



.. code:: bash

  node query.js

It should return this result:

다음 결과를 반환해야 합니다.



.. code:: bash

   Wallet path: ...fabric-samples/fabcar/javascript/wallet
   Transaction has been evaluated, result is:
   {"colour":"Black","make":"Honda","model":"Accord","owner":"Dave"}

The ownership of ``CAR12`` has been changed from Tom to Dave.

다음 결과를 반환해야 합니다.



.. note:: In a real world application the smart contract would likely have some
          access control logic. For example, only certain authorized users may
          create new cars, and only the car owner may transfer the car to
          somebody else.
          
          실제 애플리케이션에서는 스마트 계약이 접속 제어 논리를 가지고 있을 가능성이 있다. 예를 들어, 허가 받은 특정 사용자만 새 차를 만들 수 있으며, 자동차 소유자만 다른 사람에게 차를 옮길 수 있습니다.


          

Summary
-------

Now that we’ve done a few queries and a few updates, you should have a pretty
good sense of how applications interact with a blockchain network using a smart
contract to query or update the ledger. You’ve seen the basics of the roles
smart contracts, APIs, and the SDK play in queries and updates and you should
have a feel for how different kinds of applications could be used to perform
other business tasks and operations.

이제 몇 가지 쿼리 및 몇 가지 업데이트를 수행했으므로, 당신은 애플리케이션이 스마트 계약을 사용하여 블록체인 네트워크와 어떻게 상호 작용하는지 상당히 잘 알고 있어야 합니다. 스마트 계약, API 및 SDK가 쿼리 및 업데이트에서 수행하는 역할의 기본을 살펴보았으며, 다른 종류의 애플리케이션이 다른 비즈니스 작업 및 운영을 수행하는 데 어떻게 사용될 수 있는지에 대한 느낌이 있어야 합니다.



Additional resources
--------------------

As we said in the introduction, we have a whole section on
:doc:`developapps/developing_applications` that includes in-depth information on
smart contracts, process and data design, a tutorial using a more in-depth
Commercial Paper `tutorial <./tutorial/commercial_paper.html>`_ and a large
amount of other material relating to the development of applications.

서론에서 말했듯이, 우리는 스마트 계약, 프로세스 및 데이터 설계에 대한 심층적인 정보, (보다 심층적인 Commercial Paper) 튜토리얼 및 (애플리케이션 개발과 관련된 많은 양의 기타 자료를 포함하는) 애플리케이션 개발에 관한 전체 섹션이 있습니다.



.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/
