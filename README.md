# NotFoundException

## 0. 기본 설정
### 1) Visual Studio Code 설치(https://code.visualstudio.com/)
### 2) .net core SDK 설치
### 3) Git 설치
    $ git config user.name "Github 사용자 이름"
    $ git config user.email <email 주소>
### 4) MinGW 설치(gcc) - optional
### 5) Docker 설치
* * * 

## 1. Visual Studio Code 설정
1. C# Extension 설치
2. C/C++ Extension 설치
3. CMake Tools 설치
4. Dracula Official Theme 설치
5. Live Server Extension 설치
6. Visual Studio IntelliCode Extension 설치
7. Bootstrap 4, Font awesome 4, Font Awesome 5 Free & Pro snippets Extension 설치
8. Docker Extension 설치
* * *

## 2. 주요 단축키
1. Preview README.md : Ctrl + Shift + V
2. EXPLORER 창 열기/닫기 : Ctrl + B
3. 새파일 만들기 : Ctrl + N
4. VSCode 새 창 띄우기 : Ctrl + Shift + N
5. 현재 파일창 닫기 : Ctrl + W
6. Extensions 창 띄우기 : Ctrl + Shift + X
7. 창 좌우 분리 : Ctrl + \
8. 창 좌우 선택 : Ctrl + 1 / 2
9. Explorer : Ctrl + Shift + E
10. VSCode Terminal : Ctrl + `
11. 모두 저장 : Ctrl + K, S
12. 파일 찾기 : Ctrl + S
13. Debug 창 이동 : Ctrl + Shift + D
* * *

## 3. dotnet CLI
### 1) .sln 파일 생성
    $ dotnet new sln -n NotFoundException
    $ dotnet sln add src\Web\Web.csproj

### 2) 프로젝트 생성
    $ dotnet new mvc -n Web -o "src\\Web"
    - 설명 : -n 프로젝트명 -o "프로젝트 생성파일 출력위치"

### 3) Nuget 추가
    $ dotnet add package <package-name>
    $ dotnet add "src\Web" package <package-name>

### 4) Nuget 제거
    $ dotnet remove package <package-name>
    $ dotnet remove "src\Web" package <package-name>

* * *

## 4. Webpack 설정
(참고 : https://medium.com/@rich.wifidelity/asp-net-mvc-core-2-2-no-spa-with-webpack-4-jquery-bootstrap-babel-7-1f6584665566)

1. npm 설치
2. src\Web\ClientApp 폴더 만들기
3. ClientApp> npm init -y
4. ClientApp> npm i -D @babel/core @babel/plugin-syntax-dynamic-import @babel/preset-env @fortawesome/fontawesome-free aspnet-webpack babel-loader babel-plugin-dynamic-import-webpack compression-webpack-plugin css-loader file-loader mini-css-extract-plugin node-sass rimraf sass-loader style-loader url-loader webpack webpack-cli html-webpack-plugin webpack-dev-middleware webpack-hot-middleware
5. ClientApp> npm i -S @babel/polyfill bootstrap jquery moment popper.js
* * *

## 5. Github 설정
### 1) 이미 Remote git에 bin 혹은 obj 폴더가 있는 경우
    $ git rm -r bin(obj)
    $ git commit -m "Remove bin(or obj) folder"
    $ git push origin master
* * *

## 6. Docker 설정

### 0) Docker CMD
	1. 전체 중지
	$ docker ps -aq | foreach {docker stop $_}

	2. 전체 삭제 
	$ docker ps -aq | foreach {docker rm $_}

### 1) .dockerignore
    .dockerignore
    .env
    .git
    .gitignore
    .vs
    .vscode
    */bin
    */obj
    **/.toolstarget

### 2) csproj 폴더 내 Dockerfile 생성
    # RUN ALL CONTAINERS FROM ROOT (folder with .sln file):
    # docker-compose build
    # docker-compose up
    #
    # RUN JUST THIS CONTAINER FROM ROOT (folder with .sln file):
    # docker build --pull -t web -f src/Web/Dockerfile .
    #
    # RUN COMMAND
    #  docker run --name eshopweb --rm -it -p 5106:5106 web
    FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
    WORKDIR /app

    COPY *.sln .
    COPY . .
    WORKDIR /app/src/Web
    RUN dotnet restore

    RUN dotnet publish -c Release -o out

    FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS runtime
    WORKDIR /app
    COPY --from=build /app/src/Web/out ./

    # Optional: Set this here if not setting it from docker-compose.yml
    # ENV ASPNETCORE_ENVIRONMENT Development

    ENTRYPOINT ["dotnet", "Web.dll"]

## 7. Nuget 설정
    "Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation"  Version="3.0.0" 
    "Microsoft.AspNetCore.SpaServices"                   Version="3.0.0" 
    "Microsoft.AspNetCore.SpaServices.Extensions"        Version="3.0.0"
    "Swashbuckle.AspNetCore.Swagger"                     Version="5.0.0-rc4"
    "Swashbuckle.AspNetCore.SwaggerGen"                  Version="5.0.0-rc4"
    "Swashbuckle.AspNetCore.SwaggerUI"                   Version="5.0.0-rc4"

## Issue Solve
### 1) .net core 3 Runtime Update 후 IIS에서 "관리코드 없음"이 없는 경우(Asp.net Core 2.X 프로젝트)
    1) IIS에서 웹 사이트 우클릭 - [웹사이트 관리] - [고급설정] 클릭
    2) (일반) - 응용프로그램 풀 - <Classic .NET AppPool> 로 설정 후 확인

### 2) NPM package.json에서 <vscode package.json String does not match the pattern> 오류/경고 메시지 
    1) package.json 내에 "name"을 소문자로만 표현(대문자 입력시 경고 메시지 발생)

### 3) Docker desktop 실행 안될 경우(하이퍼바이저가 실행되고 있지 않으므로 가상 컴퓨터를 시작하지 못했습니다.)
    $ dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
    $ bcdedit /set hypervisorlaunchtype auto
    - CMD Windows 관리자 권한으로 실행
    - 위 명령 실행 후 컴퓨터 재시작

### 4) cannot locate specified dockerfile dockerfile
    version: '3.4'

    services:
     notfoundexceptionmvc:
        image: ${DOCKER_REGISTRY-}notfoundexceptionmvc
        build:
            context: .
            dockerfile: src\Web\Dockerfile    -> src/Web/Dockerfile로 변경 


### 5) unable to configure HTTPS endpoint. No server certificate was specified, and the default developer certificate could not be found or is out of date
	- 참고 : https://docs.microsoft.com/ko-kr/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.0&tabs=netcore-cli#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos
	- C:\Users{USER} \AppData\Roaming\ASP.NET\Https 폴더를 삭제 합니다.
	- 솔루션을 정리 합니다. bin 및 obj 폴더를 삭제합니다.
	- 개발 도구를 다시 시작 합니다. 예를 들어 Visual Studio, Visual Studio Code 또는 Mac용 Visual Studio입니다.
