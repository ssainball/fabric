Install Samples, Binaries and Docker Images
===========================================

While we work on developing real installers for the Hyperledger Fabric
binaries, we provide a script that will download and install samples and
binaries to your system. We think that you'll find the sample applications
installed useful to learn more about the capabilities and operations of
Hyperledger Fabric.

``Hyperledger Fabric 바이너리 용 실제 설치 프로그램을 개발하는 동안 시스템에 샘플 및 바이너리를 다운로드하여 설치하는 스크립트를 제공합니다. Hyperledger Fabric의 기능 및 작동에 대해 자세히 알아볼 수 있도록 설치된 샘플 응용 프로그램이 유용하다고 생각합니다.``


.. note:: If you are running on **Windows** you will want to make use of the
	  Docker Quickstart Terminal for the upcoming terminal commands.
          Please visit the :doc:`prereqs` if you haven't previously installed
          it.
	  
	  ``Windows에서 실행중인 경우 터미널 명령에 Docker Quickstart Termianl 을 사용하려고합니다. 이전에 설치하지 않은 경우 :doc:`prereqs`를 방문하십시오.``

          If you are using Docker Toolbox on Windows 7 or macOS, you
          will need to use a location under ``C:\Users`` (Windows 7) or
          ``/Users`` (macOS) when installing and running the samples.
	  
	  ``Windows 7 또는 macOS에서 Docker Toolbox를 사용하는 경우 샘플을 설치하고 실행할 때 C:\Users(Windows 7) 또는 /Users(macOS) 아래에서 실행해야합니다.``

          If you are using Docker for Mac, you will need to use a location
          under ``/Users``, ``/Volumes``, ``/private``, or ``/tmp``.  To use a different
          location, please consult the Docker documentation for
          `file sharing <https://docs.docker.com/docker-for-mac/#file-sharing>`__.
	  
	  ``Mac용 도커를 사용하는 경우, 당신은 /Users, /Volumes, /private, 또는 /tmp 디렉토리에서 실행해야 합니다. 다른 위치를 사용하려면  Docker 설명서를 참조하십시오.``

          If you are using Docker for Windows, please consult the Docker
          documentation for `shared drives <https://docs.docker.com/docker-for-windows/#shared-drives>`__
          and use a location under one of the shared drives.
	  
	  ``Windows 용 Docker를 사용하는 경우 공유 드라이브 Docker 문서를 참조하십시오.``

Determine a location on your machine where you want to place the `fabric-samples`
repository and enter that directory in a terminal window. The
command that follows will perform the following steps:

``패브릭 샘플 저장소를 배치 할 시스템의 위치를 판별하고 터미널 창에 해당 디렉토리를 입력하십시오. 다음 다음 단계를 수행합니다.``

#. If needed, clone the `hyperledger/fabric-samples <https://github.com/hyperledger/fabric-samples>`_ repository

#. Checkout the appropriate version tag

#. Install the Hyperledger Fabric platform-specific binaries and config files
   for the version specified into the /bin and /config directories of fabric-samples

#. Download the Hyperledger Fabric docker images for the version specified



`
#. 필요한 경우 hyperledger / fabric-samples 저장소를 복제하십시오.

#. 적절한 버전 태그를 확인하십시오.

#. 패브릭 샘플의 / bin 및 / config 디렉토리에 지정된 버전의 Hyperledger Fabric 플랫폼 별 바이너리 및 구성 파일을 설치하십시오.

#. 지정된 버전의 Hyperledger Fabric 도커 이미지 다운로드
`


Once you are ready, and in the directory into which you will install the
Fabric Samples and binaries, go ahead and execute the command to pull down
the binaries and images.

``준비가 되면 Fabric Samples 및 바이너리를 설치할 디렉토리에 바이너리 및 이미지를 가져 오는 명령을 실행하십시오.``

.. note:: If you want the latest production release, omit all version identifiers.

``최신 프로덕션 릴리스를 원하면 모든 버전 식별자를 생략하십시오.``


.. code:: bash

  curl -sSL http://bit.ly/2ysbOFE | bash -s

.. note:: If you want a specific release, pass a version identifier for Fabric,
          Fabric-ca and thirdparty Docker images.
          The command below demonstrates how to download **Fabric v1.4.3**

``특정 릴리스를 원하는 경우 Fabric, Fabric-ca 및 타사 Docker 이미지의 버전 식별자를 전달하십시오. 아래 명령은 Fabric v1.4.3 을 다운로드하는 방법을 보여줍니다.``

.. code:: bash

  curl -sSL http://bit.ly/2ysbOFE | bash -s -- <fabric_version> <fabric-ca_version> <thirdparty_version>
  curl -sSL http://bit.ly/2ysbOFE | bash -s -- 1.4.3 1.4.3 0.4.15

.. note:: If you get an error running the above curl command, you may
          have too old a version of curl that does not handle
          redirects or an unsupported environment.
	  
	  ``위 curl 명령을 실행하는 중에 오류가 발생하면 경로 재 지정 또는 지원되지 않는 환경을 처리하지 못하는 curl 버전이 너무 오래된 것일 수 있습니다.``

	  Please visit the :doc:`prereqs` page for additional
	  information on where to find the latest version of curl and
	  get the right environment. Alternately, you can substitute
	  the un-shortened URL:
	  https://raw.githubusercontent.com/hyperledger/fabric/master/scripts/bootstrap.sh
	  
	  ``최신 버전의 curl을 찾고 올바른 환경을 얻는 위치에 대한 자세한 내용은 : doc :`prereqs` 페이지를 방문하십시오 . 또는 단축되지 않은 URL을 https://raw.githubusercontent.com/hyperledger/fabric/master/scripts/bootstrap.sh로 대체 할 수 있습니다.``

The command above downloads and executes a bash script
that will download and extract all of the platform-specific binaries you
will need to set up your network and place them into the cloned repo you
created above. It retrieves the following platform-specific binaries:

``위의 명령은 bash 스크립트를 다운로드하고 실행하여 네트워크를 설정하고 위에서 만든 복제 된 저장소에 배치해야하는 모든 플랫폼 별 바이너리를 다운로드하고 추출합니다. 다음과 같은 플랫폼 별 바이너리를 검색합니다.``

  * ``configtxgen``,
  * ``configtxlator``,
  * ``cryptogen``,
  * ``discover``,
  * ``idemixgen``
  * ``orderer``,
  * ``peer``, and
  * ``fabric-ca-client``

and places them in the ``bin`` sub-directory of the current working
directory.

``bin현재 작업 디렉토리 의 하위 디렉토리에 배치합니다.``

You may want to add that to your PATH environment variable so that these
can be picked up without fully qualifying the path to each binary. e.g.:

``각 바이너리에 대한 경로를 완전히 규정하지 않고 선택할 수 있도록 PATH 환경 변수에 추가 할 수 있습니다. 예 :``

.. code:: bash

  export PATH=<path to download location>/bin:$PATH

Finally, the script will download the Hyperledger Fabric docker images from
`Docker Hub <https://hub.docker.com/u/hyperledger/>`__ into
your local Docker registry and tag them as 'latest'.

``마지막으로 스크립트는 Docker Hub 의 Hyperledger Fabric 도커 이미지를 로컬 Docker 레지스트리로 다운로드하여 '최신'으로 태그합니다.``

The script lists out the Docker images installed upon conclusion.

``스크립트는 결론에 따라 설치된 Docker 이미지를 나열합니다.``

Look at the names for each image; these are the components that will ultimately
comprise our Hyperledger Fabric network.  You will also notice that you have
two instances of the same image ID - one tagged as "amd64-1.x.x" and
one tagged as "latest". Prior to 1.2.0, the image being downloaded was determined
by ``uname -m`` and showed as "x86_64-1.x.x".

``각 이미지의 이름을보십시오. 이것들은 궁극적으로 Hyperledger Fabric 네트워크를 구성하는 구성 요소입니다. 또한 동일한 이미지 ID의 두 인스턴스가 있습니다. 하나는 "amd64-1.xx"로 태그되고 다른 하나는 "최신"으로 태그되었습니다. 1.2.0 이전에는 다운로드중인 이미지가 uname -m"x86_64-1.xx"로 결정되었습니다.``

.. note:: On different architectures, the x86_64/amd64 would be replaced
          with the string identifying your architecture.
	  
	  ``다른 아키텍처에서는 x86_64 / amd64가 아키텍처를 식별하는 문자열로 대체됩니다.``

.. note:: If you have questions not addressed by this documentation, or run into
          issues with any of the tutorials, please visit the :doc:`questions`
          page for some tips on where to find additional help.
	  
	  ``이 문서에서 다루지 않은 질문이 있거나 튜토리얼에 문제가있는 경우 : doc :`questions` 페이지 를 방문하여 추가 도움을 얻을 수있는 위치에 대한 팁을 얻으십시오.``

.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/
