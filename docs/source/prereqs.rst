Prerequisites
=============

Before we begin, if you haven't already done so, you may wish to check that
you have all the prerequisites below installed on the platform(s)
on which you'll be developing blockchain applications and/or operating
Hyperledger Fabric.

``시작하기 전에 아직 그렇게하지 않은 경우 블록 체인 응용 프로그램을 개발하거나 Hyperledger Fabric을 운영 할 플랫폼에 아래의 모든 필수 구성 요소가 설치되어 있는지 확인할 수 있습니다.``

Install cURL
------------

Download the latest version of the `cURL
<https://curl.haxx.se/download.html>`__ tool if it is not already
installed or if you get errors running the curl commands from the
documentation.

``최신 버전의 cURL 도구가 설치되어 있지 않거나 문서에서 curl 명령을 실행하는 중에 오류가 발생하면 다운로드하십시오.``

.. note:: If you're on Windows please see the specific note on `Windows
   extras`_ below.
   
   ``Windows를 사용하는 경우 아래의 'Windows extras' 에 대한 특정 참고 사항을 참조하십시오.``

Docker and Docker Compose
-------------------------

You will need the following installed on the platform on which you will be
operating, or developing on (or for), Hyperledger Fabric:

``Hyperledger Fabric을 운영하거나 개발할 플랫폼에 다음이 설치되어 있어야합니다.``

  - MacOSX, \*nix, or Windows 10: `Docker <https://www.docker.com/get-docker>`__
    Docker version 17.06.2-ce or greater is required.
  - Older versions of Windows: `Docker
    Toolbox <https://docs.docker.com/toolbox/toolbox_install_windows/>`__ -
    again, Docker version Docker 17.06.2-ce or greater is required.

You can check the version of Docker you have installed with the following
command from a terminal prompt:

``터미널 프롬프트에서 다음 명령으로 설치 한 Docker 버전을 확인할 수 있습니다.``

.. code:: bash

  docker --version

.. note:: Installing Docker for Mac or Windows, or Docker Toolbox will also
          install Docker Compose. If you already had Docker installed, you
          should check that you have Docker Compose version 1.14.0 or greater
          installed. If not, we recommend that you install a more recent
          version of Docker.
	  
	  ``Mac 또는 Windows 용 Docker 또는 Docker Toolbox를 설치하면 Docker Compose도 설치됩니다. Docker가 이미 설치되어 있으면 Docker Compose 버전 1.14.0 이상이 설치되어 있는지 확인해야합니다. 그렇지 않은 경우 최신 버전의 Docker를 설치하는 것이 좋습니다.``

You can check the version of Docker Compose you have installed with the
following command from a terminal prompt:

``터미널 프롬프트에서 다음 명령으로 설치 한 Docker Compose의 버전을 확인할 수 있습니다.``

.. code:: bash

  docker-compose --version

.. _Golang:

Go Programming Language
-----------------------

Hyperledger Fabric uses the Go Programming Language for many of its
components.

``Hyperledger Fabric은 많은 구성 요소에 Go Programming Language를 사용합니다.``



  - `Go <https://golang.org/dl/>`__ version 1.11.x is required.

Given that we will be writing chaincode programs in Go, there are two
environment variables you will need to set properly; you can make these
settings permanent by placing them in the appropriate startup file, such
as your personal ``~/.bashrc`` file if you are using the ``bash`` shell
under Linux.

``Go에서 체인 코드 프로그램을 작성한다고 가정 할 때 올바르게 설정해야하는 두 가지 환경 변수가 있습니다. Linux에서 "bash" 쉘을 사용하는 경우 개인 "~/.bashrc" 파일과 같은 적절한 시작 파일에 배치하여이 설정을 영구적으로 만들 수 있습니다.``

First, you must set the environment variable ``GOPATH`` to point at the
Go workspace containing the downloaded Fabric code base, with something like:

.. code:: bash

  export GOPATH=$HOME/go

.. note:: You **must** set the GOPATH variable

  Even though, in Linux, Go's ``GOPATH`` variable can be a colon-separated list
  of directories, and will use a default value of ``$HOME/go`` if it is unset,
  the current Fabric build framework still requires you to set and export that
  variable, and it must contain **only** the single directory name for your Go
  workspace. (This restriction might be removed in a future release.)
  
  ``Linux에서 Go의 GOPATH 변수는 콜론으로 구분 된 디렉토리 목록이 될 수 있지만 설정되지 않은 경우 기본값 $ HOME / go를 사용하지만 현재 Fabric 빌드 프레임 워크에서는 해당 변수를 설정하고 내 보내야합니다. Go 작업 공간의 단일 디렉토리 이름 만 포함해야합니다. (이 제한은 다음 릴리스에서 제거 될 수 있습니다.)``

Second, you should (again, in the appropriate startup file) extend your
command search path to include the Go ``bin`` directory, such as the following
example for ``bash`` under Linux:

''둘째, (적절한 시작 파일에서) Linux에서 bash에 대한 다음 예제와 같이 Go bin 디렉토리를 포함하도록 명령 검색 경로를 확장해야합니다.''

.. code:: bash

  export PATH=$PATH:$GOPATH/bin

While this directory may not exist in a new Go workspace installation, it is
populated later by the Fabric build system with a small number of Go executables
used by other parts of the build system. So even if you currently have no such
directory yet, extend your shell search path as above.

``이 디렉토리는 새 Go 작업 공간 설치에 없을 수 있지만 나중에 빌드 시스템의 다른 부분에서 사용하는 소수의 Go 실행 파일로 Fabric 빌드 시스템에 의해 채워집니다. 따라서 현재 그러한 디렉토리가없는 경우에도 위와 같이 쉘 검색 경로를 확장하십시오.``

Node.js Runtime and NPM
-----------------------

If you will be developing applications for Hyperledger Fabric leveraging the
Hyperledger Fabric SDK for Node.js, version 8 is supported from 8.9.4 and higher.
Node.js version 10 is supported from 10.15.3 and higher.

``Node.js 용 Hyperledger Fabric SDK를 활용하여 Hyperledger Fabric 용 애플리케이션을 개발할 경우 버전 8은 8.9.4 이상에서 지원됩니다. Node.js 버전 10은 10.15.3 이상에서 지원됩니다.``

  - `Node.js <https://nodejs.org/en/download/>`__ download

.. note:: Installing Node.js will also install NPM, however it is recommended
          that you confirm the version of NPM installed. You can upgrade
          the ``npm`` tool with the following command:
	  
	  ``Node.js를 설치하면 NPM도 설치되지만 설치된 NPM 버전을 확인하는 것이 좋습니다. 다음 명령을 사용하여 npm 도구를 업그레이드 할 수 있습니다.``

.. code:: bash

  npm install npm@5.6.0 -g

Python
^^^^^^

.. note:: The following applies to Ubuntu 16.04 users only.

``다음은 Ubuntu 16.04 사용자에게만 해당됩니다.``

By default Ubuntu 16.04 comes with Python 3.5.1 installed as the ``python3`` binary.
The Fabric Node.js SDK requires an iteration of Python 2.7 in order for ``npm install``
operations to complete successfully.  Retrieve the 2.7 version with the following command:

``기본적으로 Ubuntu 16.04는 Python 3.5.1이 python3 바이너리로 설치되어 제공됩니다. npm 설치 작업을 성공적으로 완료하려면 Fabric Node.js SDK에 Python 2.7을 반복해야합니다. 다음 명령으로 2.7 버전을 검색하십시오.``

.. code:: bash

  sudo apt-get install python

Check your version(s):

.. code:: bash

  python --version

.. _windows-extras:

Windows extras
--------------

If you are developing on Windows 7, you will want to work within the
Docker Quickstart Terminal which uses `Git Bash
<https://git-scm.com/downloads>`__ and provides a better alternative
to the built-in Windows shell.

``Windows 7에서 개발하는 경우 Git Bash를 사용하고 내장 Windows 쉘에 대한 더 나은 대안을 제공하는 Docker 빠른 시작 터미널 내에서 작업하고 싶을 것입니다.``

However experience has shown this to be a poor development environment
with limited functionality. It is suitable to run Docker based
scenarios, such as :doc:`getting_started`, but you may have
difficulties with operations involving the ``make`` and ``docker``
commands.

``그러나 경험에 의하면이 기능은 기능이 제한적인 개발 환경이 열악합니다. : doc :`getting_started`와 같은 Docker 기반 시나리오를 실행하는 것이 적합하지만 make 및 docker 명령과 관련된 작업에는 문제가있을 수 있습니다.``

On Windows 10 you should use the native Docker distribution and you
may use the Windows PowerShell. However, for the ``binaries``
command to succeed you will still need to have the ``uname`` command
available. You can get it as part of Git but beware that only the
64bit version is supported.

``Windows 10에서는 기본 Docker 배포를 사용해야하며 Windows PowerShell을 사용할 수 있습니다. 그러나 바이너리 명령이 성공하려면 uname 명령을 계속 사용할 수 있어야합니다. Git의 일부로 얻을 수 있지만 64 비트 버전 만 지원됩니다.``

Before running any ``git clone`` commands, run the following commands:

``다음 명령으로 할 수 있습니다.``

::

    git config --global core.autocrlf false
    git config --global core.longpaths true

You can check the setting of these parameters with the following commands:

``다음 명령으로 이러한 매개 변수의 설정을 확인할 수 있습니다.``

::

    git config --get core.autocrlf
    git config --get core.longpaths

These need to be ``false`` and ``true`` respectively.

``이들은 각각 거짓과 참이어야합니다.``

The ``curl`` command that comes with Git and Docker Toolbox is old and
does not handle properly the redirect used in
:doc:`getting_started`. Make sure you install and use a newer version
from the `cURL downloads page <https://curl.haxx.se/download.html>`__

``Git 및 Docker Toolbox와 함께 제공되는 curl 명령은 오래되었으며 : doc :`getting_started`에 사용 된 리디렉션을 올바르게 처리하지 못합니다. cURL 다운로드 페이지에서 최신 버전을 설치하고 사용해야합니다``

For Node.js you also need the necessary Visual Studio C++ Build Tools
which are freely available and can be installed with the following
command:

``Node.js의 경우 무료로 제공되며 다음 명령으로 설치할 수있는 필수 Visual Studio C ++ 빌드 도구가 필요합니다.``

.. code:: bash

	  npm install --global windows-build-tools

See the `NPM windows-build-tools page
<https://www.npmjs.com/package/windows-build-tools>`__ for more
details.

``자세한 내용은 NPM windows-build-tools 페이지를 참조하십시오.``

Once this is done, you should also install the NPM GRPC module with the
following command:

``이 작업이 완료되면 다음 명령을 사용하여 NPM GRPC 모듈도 설치해야합니다.``

.. code:: bash

	  npm install --global grpc

Your environment should now be ready to go through the
:doc:`getting_started` samples and tutorials.

``이제 환경은 : doc :`getting_started` 샘플 및 자습서를 진행할 준비가 되었습니다.``

.. note:: If you have questions not addressed by this documentation, or run into
          issues with any of the tutorials, please visit the :doc:`questions`
          page for some tips on where to find additional help.
	  
	  ``이 문서에서 다루지 않은 질문이 있거나 튜토리얼에 문제가있는 경우 : doc :`questions` 페이지를 방문하여 추가 도움을 얻을 수있는 위치에 대한 팁을 얻으십시오.``

.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/
